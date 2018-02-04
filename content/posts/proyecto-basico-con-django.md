+++
title = "Proyecto básico con Django (crear un blog)"
date =  "2016-03-21"
author =  "Yábir G."
description = "En este post abordo la creación de un proyecto sencillo con Django a modo de guía para la gente que pueda estar iniciándose."
tags = ["django", "python"]
+++

## Introducción

[Django](https://www.djangoproject.com) es un framework web para Python. Existen otros como flash pyramid pero Django es uno de los más completos y que se preocupa de que únicamente nos tengamos que concentrar en lo básico. 

Con este post quiero cubrir un proyecto básico ya que creo que puede servir a las personas que estan iniciándose. Pretendo que sirva como guía y recomiendo la documentación de Django ya que es una de las más completas y en ella podemos encontrar facilmente cualquier cosa. No pretendo que nadie se asuste ya que aunque pueda parecer largo y al principio confuso no lo es tanto y es un proceso sencillo.

## Manos a la obra

### Creación del proyecto

Siempre que empiezo un proyecto lo primero que hago es crear un entorno virtual ya que me permite trabajar con distintas versiones de python y de Django. Si no sabes usarlo tenemos un post [aquí](https://wildunix.es/posts/guia-rapida-para-usar-virtualenv/). Para este proyecto voy a usar python3 y Django 1.9.4.

Partimos de que ya lo tenemos instalado y de que estamos trabajando dentro de nuestro entorno virtual. Lo primero que tenemos que hacer es crear nuestro proyecto de Django con la orden:

`django-admin.py startproject myproject`

Ahora la estructura de mi carpeta de virtualenv es:

    .
    ├── bin
    ├── include
    ├── lib
    └── myproject


"myproject" es el nombre de nuestro proyecto, lo podemos llamar como queramos. Para este tutorial usaré este nombre. Dentro de la carpeta que se ha generado, "myproject" tenemos un archívo llamado "manage.py" que es el que nos servirá a "grosso modo" para interactuar con las utilidades de Django y otra carpeta que se llama igual "myproject" en la que encontramos archivos de configuración para el proyecto.

Lo primero que vamos ha hacer es crear una aplicación que son por llamarlo ahora de alguna manera módulos donde tenemos la lógica de nuestro proyecto. Para ello usaremos el archivo "manage.py" con la orden:

`python manage.py startapp blog`

Esto nos creará otra carpeta a la misma altura del archivo manage.py que tendrá distintos archivo. Vamos a tratar cada uno a su tiempo. Otra vez el nombre blog es arbitrario y podemos elegir el que queramos.

### Empezando a trabajar

Empezaremos con el archivo "models.py" dentro de la carpeta generada con el nombre blog.

En este archivo se ubican los modelos para la base de datos. Django es muy potente y a partir de clases de python podemos generar tablas para bases de datos. 


    from django.db import models
    from django.contrib.auth.models import User
    from django.template.defaultfilters import slugify
    
    # Create your models here.
    
    class UserProfile(models.Model):
        nombre = models.CharField(max_length=300)
        usuario = models.OneToOneField(User)
    
        def __str__(self):
            return '%s' % self.nombre
    
    class Post(models.Model):
        titulo = models.CharField(max_length=200)
        slug = models.SlugField(max_length=100, unique=True)
        cuerpo = models.TextField()
        publicado = models.DateTimeField(auto_now_add=True)
        presentar = models.BooleanField(blank = True, null = False, default=True)
        autor = models.ForeignKey(UserProfile)
    
        def __str__(self):
            return self.titulo
       
        def save(self, *args, **kwargs):
            if not self.id:
                self.slug = slugify(self.titulo)
    
            super(Post, self).save(*args, **kwargs)


Vamos a analizar este archivo parte por parte. La primera línea importa la clase "models" de Django de la que van a heredar todas las clases que hagamos para nuestra base de datos. En la segunda línea importo el modelo que hace referencia a los usuarios y que ya viene definido por el propio Django.

Después creo una clase que se llama "UserProfile" que hereda de "models.Model". Dentro vemos como cada variable hace referencia a una columna de la base de datos e indicamos el tipo con los atributos de models. Una lista completa de los distintos campos que se pueden crear viene en esta [página](https://docs.djangoproject.com/en/1.9/ref/models/fields/#field-types). 

En este caso creo un campo de texto pequeño ("CharField") y le doy la propiedad de una longitud máxima de 300 caracteres. Después la variable usuario está asociada a un campo que establece una relación con la tabla de Usuarios del propio Django. En este caso esta relación es de uno a uno por lo que un perfil únicamente puede estar asociado a un usuario y un usuario únicamente a un perfil.

La función "__str__"  le indica a Django como ha de llamar a cada objeto de esta clase de cara a que una persona lo vea. En este caso indico que utilice la variable nombre de la clase.

La clase Post es semejante, destacar que "TextField()" se utiliza cuando la cantidad de texto que vamos a introducir excede los pocos caracteres. Apropiado para el cuerpo de un post. La variable "publicado" es un campo de tipo fecha con la característica de que la fecha se añade automáticamente al guardar cada objeto. Por último la relación "ForeignKey" es una relación de muchos a uno. En este varios posts estan asociados a un mismo perfil.

"Blank = True" y "Null = True" indican respectivamente que este campo no es obligatorio y que en la base de datos se rellene con Null si esta vacío.

Presentar va a ser una variable de tipo bool que vamos a usar para marcar los posts que queramos que se vean o que no. Le asignamos un valor por defecto mediante la etiqueta "default".

Para terminar con este archivo comentar comentar que la función save se va a ejecutar cuando se guarde el archivo y lo que va a hacer es que si es la primera vez que se hace, esto es que su id ("self.id") no existe el campo Slug se va a generar a partir del título. Este campo se va a rellenar con una cadena especial que no va a generar conflicto con una url.

A la misma altura del archivo "models.py" tenemos el archivo "views.py" donde se encuentra la lógica de cada proyecto, es decir, le diremos que tiene que hacer cuando se carga una url.

    from django.views.generic.list import ListView
    from django.views.generic.detail import DetailView
    from .models import Post
    
    class PostList(ListView):
        model = Post
        template_name = 'post_list.html'
        paginate_by = 6
        context_object_name = "post_list"
    
        def get_queryset(self, **kwargs):
            return Post.objects.filter(presentar=True).order_by("-publicado")
    
    
    
    class PostDetailView(DetailView):
    
        model = Post
        template_name = 'post_detail.html'
        context_object_name = 'post'
    
        def get_context_data(self, **kwargs):
            context = super(PostDetailView, self).get_context_data(**kwargs)
            return context

Con Django se han usado normalmente las vistas basadas en funciones pero llevan ya implementadas desde hace algunas versiones la vistas basadas en clases. Estas últimas son útiles ya que sustituyen a funciones que se repetían a lo largo de diferentes proyectos y con menos líneas nos permiten hacer más, además de que otorgan claridad al código. Estas merecerían una publicación para ellas solas pero aquí las vamos a tratar rápidamente.

En las dos primeras líneas importamos dos clases, [ListView](https://docs.djangoproject.com/en/1.9/ref/class-based-views/generic-display/#listview) que nos permite crear con facilidad listas de objetos y DetailView con la que podemos ver objectos.

La clase PostList hereda de Listview y variables que tienen que ser llamadas así (Una cosa que quiero destacar es que la única variable que nos haría falta definir el "model" ya que si no se definen las otras Django tiene estblecida una normalización de como se llaman las cosas). Por ejemplo "model" nos indica el modelo de la base de datos en el que se va a basar la lista. Este lo hemos importado en la tercera línea.

"template_name" nos indica la plantilla html sobre la que se va a mostrar. Dentro de poco veremos esto de las plantillas. 

"Paginate_by" nos permite crear páginas. En este caso le estamos indicando que cada página va a contener 6 elementos.

Por último "context_object_name" es como va a ser nombrada la variable en nuestra plantilla

Finalmente la función "get_queryset" nos devuelve la lista de objetos que vamos a usar. En esta función los que nos devuelve es una lista. Para construirla hacemos una busqueda en la que aplicamos un filtro, en este caso que el atributo presentar sea True y le decimos que la devuelva en orden inverso de publicación, esto es, la última primero. Podríamos prescindir de esta función pero lo mejor es que toda la lógica de nuestra aplicación este en este archivo.

Respecto a la clase PostDetailView es semejante a la anterior, unicamente que nos permite ver un objeto que le indicaremos a través de la url.


El archivo "urls.py" que se encuentra en la carpeta "myproject" es el archivo principal de urls. En él podemos declarar urls o incluir otros archivos de urls. Esto de incluir otros es bueno cuando tenemos modulos y queremos que las urls de una app compartan alguna parte.

    from django.contrib import admin
    from django.conf.urls import url
    import blog.views as views
    
    
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'^$', views.PostList.as_view(), name='home'),
        url(r'^posts/(?P<slug>[-\w]+)/$', views.PostDetailView.as_view(), name='post'),
    ]

en la primera línea importamos el administrador de Django que es una parte muy útil sobretodo cuando queremos probar cosas rápidamente ya que es muy potente y nos da control directo sobre nuestros modelos.

Después importamos la función que nos va a permitir crear nuestras urls y también importamos nuestro views.py.

urlpatterns es una lista donde están todas nuestras urls. La sintaxis es sencilla y a un string le asociamos una función. La segunda url es la que dirige cuando cuando entramos en la página princiapal, seria "/" y le asociamos nuestra vista basada en clases. Importante comentar que le agregamos `.as_view()` . Si en lugar de una vista basada en clases hubiesemos usado una fucnión esto no se usaría.

La tercera url es quizás la que pueda asustar a más gente pero realmente la idea es muy sencilla. En ella estamos diciendo que todas van a tener una parte común ("posts/") y después decimos que va a ver una serie de caracteres a la que vamos a identificar por "slug".

La variable name que hay en la urls es un nombre que le vamos a dar. Eso hace más sencillo referirnos a las urls. En lugar de escribir el camino a la url podemos escribir este nombre.

Ahora vamos a editar el archivo "admin.py" dentro de la app blog que nos va a permitir segistrar nuesta app en el administrador de django.

    from django.contrib import admin
    from .models import Post, UserProfile
    
    admin.site.register(Post)
    admin.site.register(UserProfile)

Simplemente importamos desde el archivo models.py de la propia app (.models) los dos modelos que habíamos creado y los registramos en nuestro administrador.

Nos quedan tres tareas. Primero configurar nuestro archivo "settings.py" en la carpeta "myproject". Este archivo concentra toda la configuración para nuestro proyecto y en él tenemos que revisar varias cosas.

* En "INSTALLED_APPS" tenemos que agregar `"blog",` al final de la lista. Esto le indica a Django que la carpeta blog es una app.
* En "TEMPLATES" en la lista "DIRS" vamos a agegar `os.path.join(BASE_DIR, "blog/templates")` Esto construye un camino absoluto a la carpeta templates dentro de la app blog. BASE_DIR es una variable que declara Django al principio del archivo "settings.py" y que corresponde al camino absoluto de nuestro proyecto. Esto nos va a servir a continuación cuando creemos las plantillas html ya que le estamos diciendo donde estan. 

Mencionar dos cosas, la variable DATABASES en el archivo settings.py configura la base de datos de nuestro proyecto. En este caso usamos sqlite que viene por defecto y no requiere de configuración alguna. Si quisiesemos usar mysql o postgresql tendríamos que configurar esto de acuerdo a como se indica en la [documentación oficial](https://docs.djangoproject.com/en/1.9/ref/databases/). Y también las variables STATIC y MEDIA que nos permitirán proporcionarle a Django archivo tipo css js imagenes en el caso de STATIC que crea el desarrollador o que usa y MEDIA para los recursos que suben los usuarios a la app. Tambén se puede consultar acerca de esto en la [documentación](https://docs.djangoproject.com/en/1.9/howto/static-files/).

Ahora vamos a crear la carpeta "templates" a la que antes hacíamos referencia dentro de la carpeta "blog". Dentro de esta nueva carpeta vamos a crear los archivos "post_list.html" y "post_detail.html" que habíamo nombrado antes en el archivo views.py. Además crearemos un tercer archivo llamado "base.html". 

En Django unas plantillas pueden heredar de otras y podemos usar partes llamadas bloques ("blocks"). Esto es muy útil en páginas como blogs o periódicos donde por ejemplo la cabecera es común a todas las páginas. En lugar de copiarla para cada vista heredamos de una plantilla base.

Los archivos vamos a escribirlos ahora así. 

base.html

    <!DOCTYPE html>
    <html>
      <head>
        <meta charset="utf-8">
        <title>{% block title %}{% endblock title %}</title>
      </head>
      <body>
    	<h1>Mi proyecto</h1>
        {% block main %}{% endblock main %}
      </body>
    </html>


post_list.html

    {% extends "base.html" %}
    {% block title %}Página principal{% endblock title %}
    {% block main %}
      {% for post in  post_list%}
          <a href="{% url 'post' post.slug %}">{{post.titulo}}</a>
      {% endfor %}
    
    {% endblock main %}


post_detail.html


    {% extends "base.html" %}
    {% block title %}{{post.titulo}}{% endblock title %}
    {% block main %}
    
    <h2>{{post.title}}</h2><br>
    <b>{{post.publicado}} por {{post.autor}}</b>
    <p>{{post.cuerpo}}</p>
    {% endblock main %}


Como se puede apreciar en base.html creamos una estructura html básica y un par de bloques que van a usar las otras dos plantillas. Para indicar que heredamos de otra usamos `{% extends "nombre_de_plantilla.html" %}` marcar que el lenguaje de plantillas de django designa a las variables con `{{ variable }}` y las funciones o los bucles por ejemplo usan `{%  %}`.

En post_detail.html estamos usando la variable post que la definimos antes en nuestra vista y para cada objeto llamamos a sus atributos con `.nombre_del_atributo` los atributos recordar que los definimos en nuestro archivo "models.py".

con `{% url %}` usamos lo que antes mencionábamos de los nombres para las urls. En este caso decimos que el enlace sea a posts y el argumento slug va a ser el slug propio de cada post.

### Poniendo la maquinaria a funcionar

Con esto ya estaría todo listo. Ahora vamos a probar nuestro proyecto. Nos ubicaremos con la consola a la altura de nuestro archivo "manage.py" y ejecutaremos:

* "python manage.py makemigrations" que hará las migraciones a la base de datos de nuestras aplicaciones tras haber hecho cambios.
* "python manage.py migrate" que aplicará los cambios a nuestra base de datos.
* "python manage.py createsuperuser" que creará un superusuario para nuestro proyecto.

Y por últmo "python manage.py runserver" que arrancará un servidor en el puerto 8000. Si todo está correcto debería ver algo así:


    Performing system checks...
    
    System check identified no issues (0 silenced).
    March 21, 2016 - 16:13:27
    Django version 1.9.4, using settings 'myproject.settings'
    Starting development server at http://127.0.0.1:8000/
    Quit the server with CONTROL-C.


Ahora si vamos a localhost:8000 deberíamos ver unicamente Mi proyecto ya que aparecía en la plantilla base.html. Si vemos esto significa que correctamente nuestra url ha enlazado con la vista que habíamos creado y ha cargado el template correcto. Además este se ha generado a partir del template base.html.

Vamos a crear un post para ver que todo funciona. Para ello vamos a usar el administrador de Django y para ello accedemos a localhost:8000/admin ya que venía definido en nuestras url y accederemos con los credenciales que introdujimos al crear el superusuario de nuestro proyecto.

En el panel que nos aparece vamos a crear primero un perfil, le daremos al "Add" que aparece al lado de User Profiles. Rellenamos los dos campos y hacemos click en Guardar.

Arriba aparece una barra en la que haremos click en Home para volver a la pantalla principal y haremos click en el Add junto a posts. Igual rellenamos los campos a excepción del slug (que se completará solo cuando guardemos) y guardamos.

Ahora si accedemos a localhost:8000 veremos que aparece un enlace y si hacemos click nos lleva al contenido del post.

La estructura final de mi proyecto es la siguiente:

    .
    ├── blog
    │   ├── admin.py
    │   ├── apps.py
    │   ├── __init__.py
    │   ├── migrations
    │   │   ├── 0001_initial.py
    │   │   ├── 0002_post_presentar.py
    │   │   ├── 0003_post_slug.py
    │   │   ├── __init__.py
    │   │   └── __pycache__
    │   ├── models.py
    │   ├── templates
    │   │   ├── base.html
    │   │   ├── post_detail.html
    │   │   └── post_list.html
    │   ├── tests.py
    │   └── views.py
    ├── db.sqlite3
    ├── manage.py
    └── myproject
        ├── __init__.py
        ├── settings.py
        ├── urls.py
        └── wsgi.py


Y esto es todo por hoy...
