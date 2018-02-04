+++
title = "Instalar SageMath en Mac OS , Windows, Ubuntu y otros Linux"
date =  "2017-11-20"
author =  "Yábir G."
description = "Últimamente está siendo muy utilizado el sistema algebraico computacional que conocemos con el nombre de Sage. El nombre Sage, como ocurre en tantas ocasiones, es un acrónimo, esta vez de `System for Algebra and Geometry Experimentation`. En Inglés se usa frecuentemente  `sagemath` en lugar de Sage"
tags = ["linux", "mac", "sage", "python", "jupyter"]
+++

## Introducción

Últimamente está siendo muy utilizado el sistema algebraico computacional que conocemos con el nombre de [Sage](http://www.sagemath.org/). El nombre [Sage](http://es.wikipedia.org/wiki/Sage), como ocurre en tantas ocasiones, es un acrónimo, esta vez de "[System for Algebra and Geometry Experimentation](http://en.wikipedia.org/wiki/Sage_%28mathematics_sofxtware%29)". En Inglés se usa frecuentemente "[sagemath](http://www.sagemath.org/)" en lugar de "Sage" para distiguir de otros usos de la palabra en dicha lengua.

Si quisiera **trabajar con Sage en la nube** sin tenerlo instalado en
su máquina, es posible hacerlo. Basta conectarse a lugares como éste:

[https://cocalc.com/](https://cocalc.com/)

e iniciar sesión (login) por los medios indicados. Esto es conveniente
si quiere tener un primer contacto de tanteo con `sagemath` y probar
qué tal le va.

En Ubuntu es posible instalar sagemath mediante `apt`. Esta vía supone, como siempre, la facilidad de instalación y la ventaja de la actualización, pero también la ---a veces crucial--- carga de operar con una versión antigua.

En este blog nos esforzamos por ofrecer aportaciones originales y de calidad, en un intento de iluminar los huecos que frecuentemente quedan a oscuras en la vida diaria del usuario medio de Linux, que no es especialista. Así, hace tiempo que queríamos proporcionar a nuestros lectores un ejemplo de como instalar manualmente en nuestro Linux, quizá Ubuntu, la última versión.

Explicaremos en este post la instalación de Sage con `apt`, el camino recto, y la instalación alternativa que no es otra que la manual. Dada la utilidad de sagemath y la aún amplia comunidad de usuarios que disfrutan aún las delicias de Windows, haremos finalmente una alusión a cómo instalar/usar sagemath desde un ordenador bajo Windows.

Como nota al margen, indicar que `sagemath` aún continúa utilizando `Python 2.7` para su interfax de programación, sin que sepamos cuándo llegará ese anhelado momento en que podamos programar para `sagemath` en `Python 3._`. 

## Primer método: la instalación con apt-get (cómoda) 

Está disponible una  [PPA](https://launchpad.net/~aims/+archive/sagemath) para Ubuntu al efecto. Para instalar Sage por esta vía haremos:

    sudo apt-add-repository -y ppa:aims/sagemath
    sudo apt update
    sudo apt install sagemath-upstream-binary

Como hemos dicho, la ventaja de esta instalación es la facilidad y el inconveniente puede ser el disponer de, y estar usando, un material anticuado. No obstante, en los últimos tiempos esto no ocurre así pues sagemath está siendo cuidadosa y frecuentemente actualizado.

Hay que tener en cuenta que sagemath no está vía repositorio para máquinas de 32 bits, por lo que especialmente en ese caso, como en otros (tener una distro diferente, repositorio no disponible temporal o definitivamente, etc.) será preciso llevar a cabo la instalación manual que se explica en la siguiente sección (Segundo método: ...).

## Segundo método: la instalación manual (recomendada)

La instalación manual pondrá a nuestra disposición el último material, con el inconveniente de que habremos de actualizar también manualmente. Éste es un inconveniente no menor, sin duda. No obstante. la información que sigue ejemplifica una forma de proceder. Como ventaja, según este método tendremos recursos de escritura vía LaTeX conforme a la versión de sagemath.

La descarga de SAGE deberá ser hecha de la página oficial o de una imagen especular (mirror) suya. Iremos a un tal sitio a través de la página oficial de sagemath. En nuestro caso hemos ido a

[http://www.sagemath.org/download-linux.html](http://www.sagemath.org/download-linux.html)

y hemos seleccionado

[ftp://ftp.fu-berlin.de/unix/misc/sage/linux/index.html](ftp://ftp.fu-berlin.de/unix/misc/sage/linux/index.html)

Allí seleccionamos el carácter de la descarga, que en nuestro caso es "64bit"  y luego hemos pulsado sobre 

    sage-8.1-Ubuntu_16.04-x86_64.tar.bz2

que como vemos es un fichero adecuado para Ubuntu 16.04 de 64 bits y a buen seguro que lo será para algunas versiones posteriores. En cada caso elegiremos adecuadamente para nuestra máquina (32bit ó 64bit) y nuestra distribución, por lo que los nombres no serán exactamente los mismos.

Supongamos, para fijar ideas, que el fichero se ha descargado en "Descargas" y que tiene el nombre que dice más arriba. Entonces abrimos una (sesión de) terminal y hacemos lo siguiente:

1º) Lo copiamos en el directorio `/opt` por ser el idóneo para lo que sigue. Esto lo conseguimos con la orden (no olvidar sustituir `miUsuario` por su nombre de usuario en su equipo):

	sudo cp /home/miUsuario/Descargas/sage-8.1-Ubuntu_16.04-x86_64.tar.bz2 /opt

2º) Descomprimimos el fichero:
   
	cd /opt
    sudo tar -xvf  sage-8.1-Ubuntu_16.04-x86_64.tar.bz2

3º)  Con el fin de suprimir lo que ya no es necesario borramos el fichero comprimido, copiado previamente en `/opt`, mediante la orden:

	sudo rm sage-8.1-Ubuntu_16.04-x86_64.tar.bz2

Esto podemos hacerlo más tarde en caso de que queramos mantener el archivo hasta finalizar la instalación.

4º) Hacemos un nexo simbólico del ejecutable a un lugar incluido en el camino:

	sudo ln -s /opt/SageMath/sage /usr/local/bin/sage

Con esta acción, `sage` será ya una orden reconocible por el sistema.

5º) Encontraremos el camino al emblema de sagemath para nuestro eventual lanzador. Para ello podemos usar la orden `locate`:

    sudo updatedb && locate icon48x48.png | grep ^/opt

La línea anterior actualizará la base de datos de archivos y buscará por un archivo con `icon48x48.png` en su nombre en la carpeta `/opt`. Al ejecutar esa orden hemos obtenido la siguiente información:

	/opt/SageMath/local/lib/python2.7/site-packages/sagenb/data/sage/images/icon48x48.png

6º) Para trabajar cómodamente podemos generar un icono de lanzamiento de sagemath (si no se desea tener, podemos saltar este punto). Para ello escribiremos en la terminal:

	sudo nano /usr/share/applications/sage.desktop

En este archivo anotaremos la información para crear nuestro lanzador. Un ejemplo sería:

    [Desktop Entry]
    Name=Sagemath
    Comment=SageMath
    Exec=sage -notebook=jupyter
	Icon=/opt/SageMath/local/lib/python2.7/site-packages/sagenb/data/sage/images/icon48x48.png
    Terminal=false
    Type=Application

Guardamos el archivo con la combinación `ctrl+x` e `Y` y será creado. Sin necesidad de reiniciar debería aparecer buscándolo en el lanzador de aplicaciones, aunque la primera vez puede tardar un tiempo en aparecer.

Aprecie que `Exec=sage -notebook=jupyter` podría ser sustituido por `Exec=sage -notebook=sagenb` si deseásemos, o necesitásemos, usar el entorno de trabajo `sagenb` tradicional de `sagemath`.

7º)  Para lanzar la aplicación desde el icono, hacemos (recordar que en la mayoría de ordenadores `Super` es la tecla de `Windows`, en otros una con el anagrama de una casita; recordar que el más no se escribe, sólo quiere decir que se matenga pulsado `Super` y con otra mano se pulse la tecla `a`):

    Super + a

y escribimos  `sage` en la línea de búsqueda, con lo que aparecerá el icono que acabamos de crear; pulsamos sobre él ... y paciencia. Si es la primera vez, nos es solicitado un password que debemos establecer y no olvidar. Veremos que se abre el navegador predeterminado para poder usar nuestro sagemath en entorno gráfico.

7º) Si lo que queremos es iniciar nuestro sagemath desde la terminal, abriremos una de ellas (Ctrl + Alt + t) y ejecutamos en la orden:

    sage

Aparece el promt `sage:`; para salir `exit()`.

Si deseamos ejecutar `sagemath` en modo gráfico desde el terminal, siempre podremos ejecutar:

	sage -notebook=jupyter

o bien

	sage -notebook=sagenb

según las preferencias o las necesidades, y tendrá el mismo efecto que cuando pulsamos el icono generado en el paso anterior, inclusive la posible petición de password. Desde luego, lo más cómodo es tener un icono con el que lanzar el sistema de cálculo ... y mejor aún, que éste arranque en modo gráfico.

Se podría producir algún error en la instalación, algo así como:

    ImportError: libpari-gmp-2.8.so.0: cannot open shared object file: No such file or directory

Se solucionaría ubicándonos en el directorio `/opt/SageMath`  desde nuestra terminal y ejecutando la orden:

    sudo make distclean && make

## Restaurar las worksheets de instalaciones anteriores

`sagemath` guarda las worksheets de `sagenb` en la siguiente carpeta:

    /home/miUsuario/.sage/sage_notebook.sagenb/home/admin

en los sistemas `Linux` y en:

    /Users/miUsuario/.sage/sage_notebook.sagenb/home/admin

en `Mac OS X`, de forma que si hemos tenido una instalación y queremos restaurar las worksheets suyas en otra futura, debemos guardar el contenido de dicha carpeta y copiarlo sin más en la del mismo nombre de la nueva instalación.

## Convertir los ficheros `.sws` en ficheros `.ipynb`

En el caso de haber usado `sagemath` con anterioridad y necesitar pasar a formato `.ipynb` nuestros anteriores ficheros `.sws`, alojados en:

	/home/miUsuario/.sage/sage_notebook.sagenb/home/admin

si somos usuarios de `Linux` o en:

	/Users/miUsuario/.sage/sage_notebook.sagenb/home/admin

si somos usuarios de `Mac OS X`, podemos usar `sagenb-export`. Para instalarlo:

* Si somos usuarios de `Linux` o `Mac OS X` podemos ejecutar en la terminal:

		pip install git+https://github.com/vbraun/ExportSageNB.git

	valore, en su caso si usar `pip3` en lugar de `pip`. También podemos ejecutar:

		sage -pip install git+https://github.com/vbraun/ExportSageNB.git

* Si es usuario de un `Linux` basado en `Debian` también podría ejecutar la orden:

		sudo apt-get install python3-sagenb-export

aunque siempre recomendaremos encarecidamente instalar los complementos de `Python` con `pip` en lugar de con herramientas externas.

Una vez que tengamos a nuestra disposición `sagenb-export`, podremos convertir los `.sws` alojados en los directorios antes dichos a formato `ipynb`, legible por `jupyter`. Para ello ejecutaremos la siguiente orden en la línea de la consola:

	sagenb-export --list

El diálogo que produciría será algo así como éste:

	Equipo:~ user$ sagenb-export --list
    Unique ID       | Notebook Name
    -------------------------------------
	admin:0         | lecture_00
	admin:13        | lecture_01
	admin:14        | lecture_02
	admin:15        | lecture_03
	...

Entonces la orden desde consola:

	sagenb-export --ipynb=Output.ipynb admin:0

produciría el fichero `lecture_00.ipynb` en la carpeta desde la que se ha ejecutado y ya podría ser abierto con, por ejemplo, la orden:

	sage -notebook=jupyter lecture_00.ipynb

En la cita

[https://github.com/vbraun/ExportSageNB](https://github.com/vbraun/ExportSageNB)

aparecen comentadas algunas incidencias posibles.

## El caso de `Mac OS X`

Es el más sencillo. Descargue el paquete de instalación de, por ejemplo, el lugar de [Berlin](ftp://ftp.fu-berlin.de/unix/misc/sage/osx/index.html) eligiendo previamente su arquitectura, que casi seguro será a estas alturas `intel`. Si es el caso, descargue el paquete `sage-8.1-OSX_10.11.6-x86_64.app.dmg`, o equivalente en su momento, desde el sitio:
	[ftp://ftp.fu-berlin.de/unix/misc/sage/osx/intel/index.html](ftp://ftp.fu-berlin.de/unix/misc/sage/osx/intel/index.html)

y ahora proceda a instalar como lo hace habitualmente con cualquier `app` de `Mac OS X`, con dos precauciones:

* En el improbable caso de ir a usar `sagenb`, y sólo en éste o porque su equipo hubiese tenido una antigua instalación de `sagemath`, tenga o conserve creado el directorio `.sage` en el directorio `/Users/miUsuario`. Si fuera un viejo usuario de `sagemath`, ese directorio ya existirá y estará bien provisto de contenidos; no debe borrarlo al instalar, consérvelo para poder exportar sus `.sws` a formato `.ipynb` como se ha dicho antes.

* Habilitar en "Seguridad y privacidad" de "Preferencias del Sistema" la opción "cualquier sitio" del apartado "Permitir aplicaciones descargadas de:". Lo conseguirá poniendo el punto en el radio-botón apropiado al efecto previo clic en el candado para poder escribir. No olvide al acabar de lanzar `sagemath` por primera vez y verlo funcionar, volver a este lugar y revertir los cambios, llevando el punto al radio-botón de "Mac App Store y desarrolladores identificados".

## Usar sagemath bajo `Windows`

Si aún disfruta felizmente de `Windows`, puede usar los [contenidos de virtualización](ftp://ftp.fu-berlin.de/unix/misc/sage/win/index.html) que ofrece el sitio oficial (firmemente desaconsejado) o bien descargar este [instalador para `Windows`](https://github.com/sagemath/sage-windows/releases) y seguir las instrucciones.

Y ... esto es todo por hoy.
