+++
title = "Mi Primera Clase de Python"
date =  "2016-10-18"
author =  "M. García"
description = "Python es un lenguaje que hace hincapié en la sencillez del lenguaje y la facilidad para el programador.  Es un lenguaje sencillo y fácil para aquellos que se inician en la programación."
tags = ["python", "clase", "math", "programming"]
+++

## Introducción

[Python](http://es.wikipedia.org/wiki/Python) fue creado a finales de
los ochenta por
[Guido van Rossum](http://es.wikipedia.org/wiki/Guido_van_Rossum) en
el Centro para las Matemáticas y la Informática (CWI, Centrum Wiskunde
& Informatica), en los Países Bajos, como un sucesor del lenguaje de
programación ABC, capaz de manejar excepciones e interactuar con el
sistema operativo Amoeba. Recibe influencias de
[ABC](http://es.wikipedia.org/wiki/ABC_(lenguaje_de_programaci%C3%B3n)),
ALGOL 68,
[C](http://es.wikipedia.org/wiki/C_(lenguaje_de_programaci%C3%B3n)),
[Haskell](http://es.wikipedia.org/wiki/Haskell),
[Icon](http://es.wikipedia.org/wiki/Icon),
[Lisp](http://es.wikipedia.org/wiki/Lisp),
[Modula-3](http://es.wikipedia.org/wiki/Modula-3),
[Perl](http://es.wikipedia.org/wiki/Perl),
[Smalltalk](http://es.wikipedia.org/wiki/Smalltalk) y
[Java](http://es.wikipedia.org/wiki/Java_(lenguaje_de_programaci%C3%B3n)).

Python es un lenguaje de programación interpretado cuya filosofía hace
hincapié en una sintaxis muy limpia y que favorezca un código legible.

Se trata de un lenguaje de programación multiparadigma, ya que soporta
orientación a objetos, programación imperativa y, en menor medida,
programación funcional. Es un lenguaje interpretado, usa tipado
dinámico, es fuertemente tipado y multiplataforma.

Es administrado por la
[Python Software Foundation](http://es.wikipedia.org/wiki/Python_Software_Foundation). Posee
una licencia de código abierto, denominada
[Python Software Foundation License](http://es.wikipedia.org/wiki/Python_Software_Foundation_License),
que es compatible con la
[Licencia pública general de GNU](http://es.wikipedia.org/wiki/GNU_General_Public_License)
a partir de la versión 2.1.1, e incompatible en ciertas versiones
anteriores.

## Instalación y Complementación Básica

La información más amplia sobre instalación, no sólo para Ubuntu, la
tenemos en
[esta dirección](http://staff.not.iac.es/~rcardenes/hg/diveintopython3-es/installing-python.html). Debemos
saber que, dada la extrema importancia de Python en programación, la
mayoría de las instalaciones de Ubuntu incluyen una versión, que suele
ser antigua. Para saber cuál es la versión instalada por defecto
ejecutaremos en la terminal:

	python -V

En la instalación básica de `Ubuntu 16.04` la respuesta a esta orden es

	Python 2.7.12

Si queremos o lo necesitamos podemos hacer convivir esta versión con
otras. Para la instalación de las nuevas podemos ejecutar, por
ejemplo:

	sudo apt-get install python3.3-minimal

con lo que instalaríamos lo básico de `Python 3.3.0`. Creemos que no
es conveniente cambiar la versión de Python por defecto en el sistema,
ya que hay incompatibilidades entre ellas y cuando el sistema recurra
a Python debe encontrar su versión por defecto, si queremos evitar
problemas.

Como hemos dicho, la instalación por defecto es bastante básica. Puede
interesar instalar herramientas para el setup de nuestro Python; con
ellas podremos instalar, por ejemplo, `pip` que es a Python respecto a
librerías algo así ---para entendernos--- como a `apt-get` es a Ubuntu
para paquetes y metapaquetes.  Para instalar estas herramientas
haremos:

	sudo apt-get install python-setuptools

y si lo queremos para Python 3 podemos ejecutar

	sudo apt-get install python3-setuptools

Hecho esto tenemos a nuestra disposición `easy_install`. Si nos fijamos tenemos una para  Python 2.7.12 y otra para Python 3:

	easy_install    easy_install3

Ahora podemos instalar `pip`:

	sudo easy_install pip

o bien

	sudo easy_install3 pip

o los dos y así quedarían instalados

	pip      pip-2.7  pip-3.2

(`pip` y `pip-2.7` deben ser lo mismo)

Para mostrar un ejemplo (ver más
[aquí](http://ww43.guide.python-distribute.org/installation.html)) de
cómo instalar algo (en el ejemplo, el paquete `Markdown`)

	sudo pip install Markdown

Para saber qué tenemos instalado con `pip`, y en cuál versión está,
ejecutamos:

	pip freeze

Para desinstalar puede usarse la opción uninstall. Por ejemplo, si
quisieramos desinstalar `Markdown`, previamente instalado,
ejecutaríamos:

	pip uninstall Markdown

Si deseamos actualizar una librería instalada con `pip` ejecutaremos:

	pip install PackageNameHere --upgrade

o bien:

	pip install PackageNameHere -U

En fin, el mejor manual de `pip` que hemos encontrado es
[éste](https://pip.pypa.io/en/latest/). Para ver generalidades, y algo
menos específico, puede consultarse también
[este otro](https://pip.pypa.io/en/latest/).

Por emacs no hemos de preocuparnos pues la instalación básica viene
preparada para abrir y entender `ficheros .py` (los ficheros de Python
acaban en `.py`). Si no hemos intalado emacs podemos hacerlo con la
orden

	sudo apt-get install emacs24

## Uso Básico

Para ejemplificar lo que expliquemos en esta sección nos basaremos en
los siguientes dos ficheros que hemos editado nosotros: [fibo.py](http://wildunix.es/static/files/fibo.py) y
[tools.py](http://wildunix.es/static/files/tools.py).

En primer lugar, podemos hacer invocaciones de librerías instaladas
como ésta:

	python -c "import markdown; print markdown.markdown('**Excellent**')"

Si hemos codificado un fichero, digamos nuestro `tools.py`, abrimos
una terminal (`Ctrl + Alt + t`) y nos desplazamos dentro de ella al
lugar en el que hemos guardado `tools.py` ---digamos
`mi_usuario@mi_usuario-nombre_equipo:~/Documents/taller_python`,
podemos tener dentro del terminal el siguiente diálogo:

	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$ python3
	Python 3.2.3 (default, Oct 19 2012, 20:10:41) 
	[GCC 4.6.3] on linux2
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import tools
	>>> a = [0,0,-1,5,4,4,7]
	>>> tools.sortSr(a)
	[-1, 0, 4, 5, 7]
	>>> 

Si al importar un módulo, e incorporar su nombre al espacio de
módulos, pensamos que vamos a usar frecuentemente una de sus
funciones, podemos asignarle un nombre local (a modo de abreviatura):

	>>> import fibo
	>>> fib = fibo.fib
	>>> fib(500)
	1 1 2 3 5 8 13 21 34 55 89 144 233 377

Pero si de todo `tools.py` sólo quisiéramos el método `sortSr` y no
quisiéramos cargar el resto, entonces podríamos hacer:

	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$ python3
	Python 3.2.3 (default, Oct 19 2012, 20:10:41) 
	[GCC 4.6.3] on linux2
	Type "help", "copyright", "credits" or "license" for more information.
	>>> from tools import sortSr
	>>> a = [0,0,-1,5,4,4,7]
	>>> sortSr(a)
	[-1, 0, 4, 5, 7]
	>>> quit()
	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$

La orden `quit()` nos saca del intérprete de Python hasta la línea de
órdenes del sistema operativo. Se puede usar equivalentemente la orden
`exit()` o `Ctrl+D`.

Como hemos visto antes, hay una variante de la declaración import que
importa los nombres de un módulo directamente al espacio de nombres
del módulo que hace la importación. Por ejemplo:

	>>> from fibo import fib, fib2
	>>> fib(500)
	1 1 2 3 5 8 13 21 34 55 89 144 233 377

Esto no introduce en el espacio de nombres local el nombre del módulo
desde el cual se está importando. Hay incluso una variante para
importar todos los nombres que un módulo define sin importar el nombre
del módulo al espacio de nombres de módulos:

	>>> from fibo import *
	>>> fib(500)
	1 1 2 3 5 8 13 21 34 55 89 144 233 377

Esto importa todos los nombres excepto aquellos que comienzan con un
subrayado (_)

## Renombrando un espacio de nombres (namespace)

Al importar un módulo, el nombre del espacio de nombres puede ser cambiado:

	>>> import math as mathematics
	>>> print(mathematics.cos(mathematics.pi))
	-1.0

Después de este `import` existe un espacio de nombres `mathematics`
pero no un espacio de nombres `math`. Es posible importar justo unos
pocos métodos de un módulo:

	>>> from math import pi,pow as power, sin as sinus
	>>> power(2,3)
	8.0
	>>> sinus(pi)
	1.2246467991473532e-16

## Ejecutar funciones desde la línea de órdenes

Si mostramos el contenido de fibo.py, por ejemplo con la orden:

	cat fibo.py

veremos que al final tiene el siguiente código

	if __name__ == "__main__":
		import sys
		fib(int(sys.argv[1]))

Esto nos permite el siguiente uso:

	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$ python3 fibo.py 50
	1 1 2 3 5 8 13 21 34
	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$

Como se comprueba, sin necesidad de entrar en el interprete y cargar
fibo.py hemos podido usar una función señalada dentro de él como
método "main" y lo hemos hecho con 50 como argumento previamente
convertido en entero.

## Algo Más sobre el Uso de Módulos

Por razones de eficiencia, cada módulo se importa una vez por sesión
del intérprete. Por lo tanto, si fuesen modificados módulos, se
tendría que reiniciar el intérprete o, si es sólo uno de ellos y se
quiere probar interactivamente, en Python 2 se puede usar  `reload()`y
se haría con la sintaxis:

	reload(module_name)

por ejemplo:

	>>> reload(fibo)

Si estamos usando Python 3 hemos de saber que `reload()` no puede ser
usado en la consola por defecto y tendríamos que ensayar algo
diferente. El resumen es este:


* En versiones de Python superiores a la 3.4:
		
		import importlib
		importlib.reload(module_name)

* En versiones de Python inferiores o 3.4:

		import imp
		imp.reload(module_name)

* En versiones 2.x de Python:

		reload(module)
		
Y para tener una solución genérica que funcione en las diferentes
versiones de Python valdría este retazo de código:

	try:
		reload  # Python 2.7
	except NameError:
		try:
			from importlib import reload  # Python 3.4+
		except ImportError:
			from imp import reload  # Python 3.0 - 3.3

## Archivos "compilados" en Python

Cabe la posibilidad de generar a partir de módulos otros "compilados a
byte" que en general se cargarán más rápido. Esto puede usarse también
para distribuir el código de una biblioteca de Python en una forma que
es moderadamente difícil de hacerle ingeniería inversa. Como ejemplo

	miUsuario@miUsuario-miEquipo:~$ python -m compileall fibo.py

Esto genera el fichero `fibo.pyc` que se comporta de la siguiente forma:

	miUsuario@miUsuario-miEquipo:~$ python fibo.pyc 50
	1 1 2 3 5 8 13 21 34

Si estamos usando Python 3, la orden:

	miUsuario@miUsuario-miEquipo:~$ python3 -m compileall fibo.py
	
genera igualmente un fichero `.pyc` pero en este caso podría tener un
nombre como `fibo.cpython-35.pyc` dentro de una carpeta creada con el
nombre `__pycache__`.

El fichero `.pyc`  es independiente de la plataforma. La orden

	miUsuario@miUsuar-miEquipo:~$ python -O -m compileall /path/fibo.py

Esto genera el fichero `fibo.pyo` que se comporta de la siguiente forma:

	miUsuario@miUsuario-miEquipo:~$ python fibo.pyo 50
	1 1 2 3 5 8 13 21 34

El módulo compileall puede crear archivos `.pyc` (o archivos `.pyo`
cuando se usa la opción `-O`) para todos los módulos en un
directorio. En resumen:

* `-O` hace que se genere un código compilado optimizado a bytecode que se almacena en un fichero `.pyo` en lugar de `.pyc`

* Si usamos `-OO` generaremos un código compilado a bityecode más optimizado, sin `assert` ni `docstrings`.

* `-m compileall` genera todos los archivos fuente Python de un directorio que se le indique.

##Pidiendo ayuda

Si queremos conocer los detalles de la ayuda sobre una función o
método, podemos invocar `help()`. Por ejemplo, si queremos saber del
método `str` escribimos:

	>>> help(str)

y pulsamos `intro`. Aparecerá la primera página de la ayuda; las
siguientes aparecen pulsando la barra espaciadora. Si queremos avanzar
línea a línea debemos pulsar intro. Para salir de la ayuda, basta con
pulsar sobre la tecla `q`.

##Instalar IPython

Si queremos instalar IPython, podemos remitirnos a este
[nuestro otro post](https://wildunix.es/posts/instalar-ipython-mediante-pip-en-ubuntu-y-mac-os-x/).

## ¡Hola ... mundo!

Para terminar esta clase recomendamos el siguiente
[manual de Python](http://docs.python.org.ar/tutorial/pdfs/TutorialPython3.pdf)
y, como no podía ser de otra forma, saludaremos al mundo desde Python:

	miUsuario@miUsuario-miEquipo:~/Documents/tallerPython$ python3
	Python 3.2.3 (default, Oct 19 2012, 20:10:41) 
	[GCC 4.6.3] on linux2
	Type "help", "copyright", "credits" or "license" for more information.
	>>> s = 'Hola mundo.'
	>>> str(s)
	'Hola mundo.'
	>>> 

Y ... esto es todo por hoy.


  [1]: http://wildunix.es/static/files/fibo.py
  [2]: http://wildunix.es/static/files/tools.py
