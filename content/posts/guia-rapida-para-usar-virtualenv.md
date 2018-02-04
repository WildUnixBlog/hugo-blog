+++
title = "Guía rápida para usar virtualenv"
date =  "2016-03-20"
author =  "Yábir G."
description = "Virtualenv es una herramienta que nos permite mantener distintas configuraciones para distintos proyectos con python en un mismo ordenador evitando posibles problemas de incompatibilidades y usar herramientas antiguas."
tags = ["python", "virtualenv"]
+++

## ¿Qué es Virtualenv?

Como dice en su propia [documentacion](https://virtualenv.pypa.io/en/latest/) documentación es una herramienta para crear entornos aislados para python ¿Y qué tiene esto de bueno? Pues que un un mismo odenador podemos tener distintas versiones de librerias o incluso ¡distintas versiones del propio python! Esto es realmente útil cuando queremos hacer por ejemplo pruebas de un código en distintas versiones o replicar una instalación de otro ordenador. Además nos permite evitar instalar librerías a nivel global por lo que evitamos posibles problemas o incluso podemos comparar entre versiones.

## ¿Cómo lo instalo?

Lo podemos instalar usando apt-get con la orden:
`$ sudo apt-get install python-virtualenv`
O bien con pip (1.3 o mayor):
`$ sudo pip install virtualenv`

## ¿Cómo creamos un entorno?

Cuando vamos a crear un nuevo entorno virtual solo tenemos que tener en cuenta la versión de python que queremos usar. Por defecto usa aquella con la que se instaló virtualenv. Para crearlo usamos la orden:

`$ virtualenv -p python3 nombre_del_directorio`

De este modo indicamos que queremos crear un entorno virtual, con la opcion "p" (-p) indicamos que vamos a usar una versión concreta de python, en este caso le digo que quiero usar python3 (que es el nombre que en mi path carga python3, también podemos usar la ruta hasta el directorio de python como argumento. Ej: `/usr/bin/python3`) y el otro argumento que le pasamos, y que es obligatorio, es el nombre de la carpeta donde se va a crear el entorno. Puede ser una carpeta tanto nueva como existente.

### ¿Y... para usarlo?

Pues es bastante simple. Tenemos que acceder a la carpeta donde esta nuestro directorio

`$ cd nombre_del_directorio`

y una vez dentro activamos el entorno con la orden:

`$ source bin/activate`

Ahora podriamos instalar cualquier libreria con pip y solo estaría disponible en ese entorno. Hay que tener en cuenta que para nuestro código funcione tendremos que tener el entorno activado. Cuando esta activado antes de nuestro nombre de usuario en la terminal sale entre paréntesis el nombre del entorno que hemos creado.
Para cerrar el entorno ejecutamos la orden:

`$ deactivate`

Y esto es todo por hoy.
