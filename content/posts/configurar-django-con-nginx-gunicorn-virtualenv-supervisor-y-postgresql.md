+++
title = "Configurar Django con Nginx, Gunicorn, virtualenv, supervisor y PostgreSQL"
date =  "2016-07-20"
author =  "Yábir G."
description = "Configurar Django en el servidor de producción puede ser una tarea complicada y más si se quiere utilizar algunas tecnologías que han aparecido recientemente. Por esto comparto esta traducción que puede ser útil a mucha gente."
tags = ["python", "django", "nginx", "gunicorn", "postgres"]
+++


Esta es una traducción del post que puede encontrar en el enlace: 

[http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/](http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/)

contando con el permiso autor para compartirlo en español.

## Introducción

Django es un *framework* de desarrollo web eficiente, versátil y dinámico que está en continua evolución. Cuando Django empezó a ganar popularidad la configuración que se recomendaba era con Apache y mod_wsgi. El "arte" de hacer funcionar a Django ha cambiado y la configuración recomendada se ha hecho más eficiente y elástica a la par que más compleja e incluye herramientas como: Nginx, Gunicorn, virtualenv, supervisord y/o PostgreSQL.

En este post se explicará como combinar todas estas herramientas en un servidor corriendo Linux.

## Requisitos

Se asume que cuenta con un servidor en el que tiene privilegios de usuario. Yo (el autor original) uso un servidor corriendo Debian 7, así que todo lo aquí dicho debería funcionar en servidores con Ubuntu o otras distribuciones basadas en Debian.

Si está usando una distribución basada en RPM (como CentOS), necesitará reemplazar las ordenes apt-get por sus equivalentes yum y si usa FreeBSD puede instalar los componentes por ports.

Si no dispone de un servidor con el que "jugar", le recomendaría un servidor VPS económico en Digital Ocean. Si se registra con [este enlace][1] me ayudará a reducir mi siguiente factura :)

También se presupone que ha configurado su DNS para que apunte a la IP del servidor. Para esta guía el dominio será `example.com`

## Actualizar el sistema

Empezaremos asegurándonos que el sistema esté actualizado:

    sudo apt-get update
    sudo apt-get upgrade

## PostgreSQL

Para instalarlo ejecute la orden 

    sudo apt-get install postgresql postgresql-contrib

Y a continuación cree un usuario y una nueva base de datos para el proyecto.

    sudo su - postgres
    postgres@django:~$ createuser --interactive -P
    Enter name of role to add: hello_django
    Enter password for new role: 
    Enter it again: 
    Shall the new role be a superuser? (y/n) n
    Shall the new role be allowed to create databases? (y/n) n
    Shall the new role be allowed to create more new roles? (y/n) n
    postgres@django:~$
    
    postgres@django:~$ createdb --owner hello_django hello
    postgres@django:~$ logout

## Usuario de la aplicación

Incluso aunque Django tiene una buena lista con los [problemas de seguridad][2], las aplicaciones se pueden ver comprometidas. Si la aplicación tiene acceso limitado a los recursos de su servidor, los daños potenciales también pueden limitarse. La aplicación debería funcionar con un usuario con privilegios restringidos.

Cree un usuario para la aplicación llamado `hello` y asignelo a un grupo llamado `webapps`

    $ sudo groupadd --system webapps 
    $ sudo useradd --system --gid webapps --shell /bin/bash --home /webapps/hello_django hello

## Instalar virtualenv y crear un entorno para su aplicación

[Virtualenv][3] es una herramienta que permite crear entornos Python separados de nuestro sistema. Esto permite trabajar con aplicaciones con distintos requisitos al mismo tiempo. Por ejemplo, una basado en Django 1.5 y otro en Django 1.6. Virtualenv es facil de instalar en Debian:

    sudo apt-get install python-virtualenv

## Crear y activar un entorno para su aplicación

Me gusta que todas mis aplicaciones estén en el directorio `/webapps/`. Si lo prefiere `/var/www/` , `/srv/` o cualquier otra ubicación de su agrado. Cree un directorio para albergar su aplicación `/webapps/hello_django/` y cambie el propietario del directorio al usuario de su aplicación,  `hello`

    $ sudo mkdir -p /webapps/hello_django/
    $ sudo chown hello /webapps/hello_django/

Ahora cree un entorno de virtualenv siendo el usuario de su aplicación en el directorio de esta:

    $sudo su - hello hello@django:~$ cd /webapps/hello_django/
    hello@django:~$ virtualenv .
    
    New python executable in hello_django/bin/python Installing
    distribute..............done. Installing pip.....................done.
    
    hello@django:~$ source bin/activate (hello_django)hello@django:~$

Ahora el entorno está activado y se puede proceder a instalar Django en él.

    (hello_django)hello@django:~$ pip install django
    
    Downloading/unpacking django
    (...)
    Installing collected packages: django
    (...)
    Successfully installed django
    Cleaning up...

El entorno con Django debería estar ya preparado. Ahora proceda a instalar Django: 

    (hello_django)hello@django:~$ django-admin.py startproject hello

Puede comprobar que todo funcione ejecutando el servidor de desarrollo: 

    (hello_django)hello@django:~$ cd hello
    (hello_django)hello@django:~$ python manage.py runserver example.com:8000
    Validating models...
    
    0 errors found
    June 09, 2013 - 06:12:00
    Django version 1.5.1, using settings 'hello.settings'
    Development server is running at http://example.com:8000/
    Quit the server with CONTROL-C.

Ahora debería poder acceder al servidor atraves de la url http://example.com:8000

##Dar permisos de escritura a otros usuarios en el directorio de la aplicación

La aplicación va a funcionar como el usuario `hello`, el cual tiene permisos en todo el directorio. Si quiere que el usuario habitual pueda cambiar archivos de la apliación, puede cambiar el propietario del grupo del directorio a `users` y otorgar permisos de escritura al grupo.

    $ sudo chown -R hello:users /webapps/hello_django
    $ sudo chmod -R g+w /webapps/hello_django

Puede comprobar los grupos de los que usted es miembro usando la orden `groups` o `id`

    $ id
    uid=1000(michal) gid=1000(michal) groups=1000(michal),27(sudo),100(users)

Si no es miembro de users, se puede agregar a usted mismo con:

    $ sudo usermod -a -G users `whoami`

## Configurar PostgreSQL para trabajar con Django

Para poder usar PostgreSQL con Django será necesarios instalar `psycopg2`  en su entorno virtual. Este paso requiere la compilación de una extensión nativa escrita en C. La compilación fallará si no puede encontrar los archivos `header` y las librerías requeridas para el enlazar los programas de C con `libpq` (librería de comunicación con Postgres) y construir los modulos de Pyton (`python-dev` package). Tenemos que instalar estos dos paquetes primero y entonces podremos instalar `psycopg2` usando `pip`:

    $ sudo aptitude install libpq-dev python-dev
    (hello_django)hello@django:~$ pip install psycopg2

Y ahora podremos configurar nuestra base de datos en el archivo `settings.py` de Django:


    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql_psycopg2',
            'NAME': 'hello',
            'USER': 'hello_django',
            'PASSWORD': '1Ak5RTQt7mtw0OREsfPhJYzXIak41gnrm5NWYEosCeIduJck10awIzoys1wvbL8',
            'HOST': 'localhost',
            'PORT': '',                      # Set to empty string for default.
        }
    }

Y por último podremos construir la base de datos desde Django:

    (hello_django)hello@django:~$ python manage.py syncdb


## Gunicorn

En producción no usaremos el servidor de desarrollo de Django sino una aplicación dedicada a servir nuestro contenido, [gunicorn](http://gunicorn.org/).

Lo primero será instalar `gunicorn` en el entorno virtual de nuestra aplicación:

    (hello_django)hello@django:~$ pip install gunicorn
    Downloading/unpacking gunicorn
      Downloading gunicorn-0.17.4.tar.gz (372Kb): 372Kb downloaded
      Running setup.py egg_info for package gunicorn
    
    Installing collected packages: gunicorn
      Running setup.py install for gunicorn
    
        Installing gunicorn_paster script to /webapps/hello_django/bin
        Installing gunicorn script to /webapps/hello_django/bin
        Installing gunicorn_django script to /webapps/hello_django/bin
    Successfully installed gunicorn
    Cleaning up...

Puede comprobar que `gunicorn` funciona correctamente con la orden:

    (hello_django)hello@django:~$ gunicorn hello.wsgi:application --bind example.com:8001
    
   Ahora debería poder acceder al servidor a través de la url http://example.com:8001/ . Se ha cambiado intencionadamente 8000 por 8001 para forzar al navegador a establecer una nueva conexión.
    
   Gunicorn está ahora instalado y listo para servir su aplicación. Procedamos a establecer una configuración básica para hacerlo más útil. Estableceremos una serie de parámetros en un archivo `bash` que gurdaremos en `bin/gunicorn_start`.

	#!/bin/bash

	NAME="hello_app"                                  # Name of the application
	DJANGODIR=/webapps/hello_django/hello             # Django project directory
	SOCKFILE=/webapps/hello_django/run/gunicorn.sock  # we will communicte using this unix socket
	USER=hello                                        # the user to run as
	GROUP=webapps                                     # the group to run as
	NUM_WORKERS=3                                     # how many worker processes should Gunicorn spawn
	DJANGO_SETTINGS_MODULE=hello.settings             # which settings file should Django use
	DJANGO_WSGI_MODULE=hello.wsgi                     # WSGI module name

	echo "Starting $NAME as `whoami`"

	# Activate the virtual environment
	cd $DJANGODIR
	source ../bin/activate
	export DJANGO_SETTINGS_MODULE=$DJANGO_SETTINGS_MODULE
	export PYTHONPATH=$DJANGODIR:$PYTHONPATH

	# Create the run directory if it doesn't exist
	RUNDIR=$(dirname $SOCKFILE)
	test -d $RUNDIR || mkdir -p $RUNDIR

	# Start your Django Unicorn
	# Programs meant to be run under supervisor should not daemonize themselves (do not use --daemon)
	exec ../bin/gunicorn ${DJANGO_WSGI_MODULE}:application \
	--name $NAME \
	--workers $NUM_WORKERS \
	--user=$USER --group=$GROUP \
	--bind=unix:$SOCKFILE \
	--log-level=debug \
	--log-file=-
    
   Y una vez guardado deberá darle permisos de ejecución:
    
        $ sudo chmod u+x bin/gunicorn_start
    
   Puede comprobar que todo funciona ejecutando el script como `hello`:
    
    $ sudo su - hello
    hello@django:~$ bin/gunicorn_start
    Starting hello_app as hello
    2013-06-09 14:21:45 [10724] [INFO] Starting gunicorn 18.0
    2013-06-09 14:21:45 [10724] [DEBUG] Arbiter booted
    2013-06-09 14:21:45 [10724] [INFO] Listening at: unix:/webapps/hello_django/run/gunicorn.sock (10724)
    2013-06-09 14:21:45 [10724] [INFO] Using worker: sync
    2013-06-09 14:21:45 [10735] [INFO] Booting worker with pid: 10735
    2013-06-09 14:21:45 [10736] [INFO] Booting worker with pid: 10736
    2013-06-09 14:21:45 [10737] [INFO] Booting worker with pid: 10737
    
    ^C (CONTROL-C to kill Gunicorn)
    
    2013-06-09 14:21:48 [10736] [INFO] Worker exiting (pid: 10736)
    2013-06-09 14:21:48 [10735] [INFO] Worker exiting (pid: 10735)
    2013-06-09 14:21:48 [10724] [INFO] Handling signal: int
    2013-06-09 14:21:48 [10737] [INFO] Worker exiting (pid: 10737)
    2013-06-09 14:21:48 [10724] [INFO] Shutting down: Master
    $ exit

Necesitará modificar los parámetros y caminos en el archivo `gunicorn_start` para que coincidan con los de su aplicación.

Como regla establezca `--workers`  (`NUM_WORKERS`) de acuerdo con la fórmula siguiente: `2 * CPUs + 1`. La idea es que en algún momento la mitad de sus trabajadores estará haciendo tareas de I/O. Para una máquina de un núcleo establezca, de acuerdo con la regla, 3 trabajadores.

El argumento `--name` (`NAME`) será con el que se identifique al proceso en ordenes como `ps` ó `top`. Por defecto es `gunicorn`, lo cual puede provocar que sea difícil de identificar si hay varias aplicaciones funcionando.

Para que `--name` funcione será necesario que instale un módulo de Python llamado `setproctitle`. Para construir esta extensión nativa `pip` necesita tener acceso a los archivos `header` en C para Python. Puede añadirlos con el paquete `python-dev` y entonces instalar `setproctitle`.

    $ sudo aptitude install python-dev
    (hello_django)hello@django:~$ pip install setproctitle

Ahora cuando liste sus procesos, podrá ver que proceso de gunicorn corresponde a cada aplicación:

    $ ps aux
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    (...)
    hello    11588  0.7  0.2  58400 11568 ?        S    14:52   0:00 gunicorn: master [hello_app]
    hello    11602  0.5  0.3  66584 16040 ?        S    14:52   0:00 gunicorn: worker [hello_app]
    hello    11603  0.5  0.3  66592 16044 ?        S    14:52   0:00 gunicorn: worker [hello_app]
    hello    11604  0.5  0.3  66604 16052 ?        S    14:52   0:00 gunicorn: worker [hello_app]

## Iniciar y controlar Gunicorn con Supervisor

Su script `gunicorn_start` debería estar ahora listo y funcionando. Necesitamos estar seguros de que puede iniciarse automáticamente con el sistema y que puede reiniciarse si por alguna razón se apaga inesperadamente. Estas tareas pueden ser fácilmente administradas por un servicio llamado `supervisord`. Su instalación es muy sencilla: 

    $ sudo aptitude install supervisor

Cuando `Supervisor` este instalado, usted puede asignarle programas que iniciar y vigilar creando archivos en la carpeta `/etc/supervisor/conf.d`. Para nuestra aplicación `hello` crearemos un archivo llamado  `/etc/supervisor/conf.d/hello.conf` con el siguiente contenido:

{{< gist postrational 5747293 "hello.conf">}}

Puede asignarle muchas [más opciones](http://supervisord.org/configuration.html#program-x-section-settings), pero esta configuración básica debería ser suficiente.

Ahora cree un archivo para almacenar los "logs" de la aplicación:

    hello@django:~$ mkdir -p /webapps/hello_django/logs/
    hello@django:~$ touch /webapps/hello_django/logs/gunicorn_supervisor.log 

Después de guardar el archivo de configuración para su programa puede pedirle a `supervisor` que vuelva a leer los archivos de configuración y que actualize (lo cual hará que se inicie la nueva aplicación).

    $ sudo supervisorctl reread
    hello: available
    $ sudo supervisorctl update
    hello: added process group

También puede comprobar el estado de su aplicación, iniciarla, detenerla o reiniciarla usando supervisor.

    $ sudo supervisorctl status hello                       
    hello                            RUNNING    pid 18020, uptime 0:00:50
    $ sudo supervisorctl stop hello  
    hello: stopped
    $ sudo supervisorctl start hello                        
    hello: started
    $ sudo supervisorctl restart hello 
    hello: stopped
    hello: started

Su aplicación debería ahora iniciarse automáticamente después de un reinicio del sistema o reiniciarse tras un error inesperado.

## Nginx

Ahora es momento de configurar Nginx como servidor para nuestra aplicación y los archivos estáticos (`static files`) . 

    $ sudo apt-get install nginx
    $ sudo service nginx start

Puede navegar a la url de su servidor (http://example.com) con su navegador y Nginx debería saludarle con las palabras `“Welcome to nginx!”`

### Crear una configuración para django en el servidor virtual de Nginx.

Cada servidor virtual de Nginx podría describirse como un archivo en 
el directorio `/etc/nginx/sites-available`. Pude seleccionar qué sitios quiere hacer disponibles creando enlaces simbólicos a éstos en la carpeta `/etc/nginx/sites-enabled`

Cree un nuevo archivo de configuración de Nginx para su aplicación de Django funcionando en `example.com` en `/etc/nginx/sites-available/hello`. El archivo debería contener algo parecido a las siguientes líneas. Una explicación más detallada está disponible de parte de [los desarrolladores de Gunicorn.](https://github.com/benoitc/gunicorn/blob/master/examples/nginx.conf)

{{< gist postrational 5747293 >}}

Cree un enlace simbólico en la carpeta `sites-enabled`:

    $ sudo ln -s /etc/nginx/sites-available/hello /etc/nginx/sites-enabled/hello

Reinicie Nginx:

    $ sudo service nginx restart 

Si navega a su sitio web, debería poder observar su página de bienvenida de Django servida por `Nginx` y `Gunicorn`. 

En este momento podría ocurrir que observase la página por defecto de `“Welcome to nginx!”` en lugar de la de bienvenida de Django. Esto podría deberse al archivo de configuración por defecto de Nginx. Si no planea usarlo, elimine el enlace simbólico a este archivo del directorio `/etc/nginx/sites-enabled`.

Si encuentra algún problema con la configuración no dude en escribirme (tanto al autor original como a mí).

## Estructura final del directorio

    /webapps/hello_django/
    ├── bin                          <= Directory created by virtualenv
    │   ├── activate                 <= Environment activation script
    │   ├── django-admin.py
    │   ├── gunicorn
    │   ├── gunicorn_django
    │   ├── gunicorn_start           <= Script to start application with Gunicorn
    │   └── python
    ├── hello                        <= Django project directory, add this to PYTHONPATH
    │   ├── manage.py
    │   ├── project_application_1
    │   ├── project_application_2
    │   └── hello                    <= Project settings directory
    │       ├── __init__.py
    │       ├── settings.py          <= hello.settings - settings module Gunicorn will use
    │       ├── urls.py
    │       └── wsgi.py              <= hello.wsgi - WSGI module Gunicorn will use
    ├── include
    │   └── python2.7 -> /usr/include/python2.7
    ├── lib
    │   └── python2.7
    ├── lib64 -> /webapps/hello_django/lib
    ├── logs                         <= Application logs directory
    │   ├── gunicorn_supervisor.log
    │   ├── nginx-access.log
    │   └── nginx-error.log
    ├── media                        <= User uploaded files folder
    ├── run
    │   └── gunicorn.sock 
    └── static                       <= Collect and serve static files from here

## Desinstalar la aplicación de Django

Si en algún momento quiere desinstalar la aplicación siga los siguientes pasos.

Eliminar los servidores virtuales de la carpeta `sites-enabled`:

    $ sudo rm /etc/nginx/sites-enabled/hello_django

Reiniciar Nginx:

    $ sudo service nginx restart 

Si no piensa volver a usar esta aplicación la puede eliminar de la carpeta `sites-available`:

    $ sudo rm /etc/nginx/sites-available/hello_django

Detener la aplicación con Supervisor:

    $ sudo supervisorctl stop hello
Eliminar la aplicación del script de configuración de supervisor:

    $ sudo rm /etc/supervisor/conf.d/hello.conf

Si no va a volver a usar la aplicación puede eliminarla de `/webapps`

    $ sudo rm -r /webapps/hello_django

## Servir varias aplicaciones

Si precisase alguna ayuda configurando el servidor Nginx para mantener varias aplicaciones de Django, no dude en visitar el [siguiente post del autor](http://michal.karzynski.pl/blog/2013/10/29/serving-multiple-django-applications-with-nginx-gunicorn-supervisor/).


  [1]: https://www.digitalocean.com/?refcode=053914aba44d
  [2]: http://michal.karzynski.pl/blog/2013/06/09/django-nginx-gunicorn-virtualenv-supervisor/
  [3]: http://virtualenv.org/
