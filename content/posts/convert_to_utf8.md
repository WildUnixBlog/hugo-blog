+++
title = "Cómo Cambiar la Codificación de un Fichero en Linux y Mac OS "
description = "A veces recibimos ficheros `.wxm` hechos desde Windows que no podemos abrir con nuestro wxMaxima de Linux ni con el de Mac OS X"
date =  "2018-02-08"
author =  "M. Garcia"
tags = ["linux", "mac", "sage", "python", "jupyter"]
+++

## Introducción

A veces recibimos ficheros `.wxm` hechos desde Windows que no podemos
abrir con nuestro wxMaxima de Linux ni con el de Mac OS X. En el
proceso de intentar abrirlos, wxMaxima ofrece dos mensajes (sacados de
wxMaxima en Ubuntu 11.04):

	wxMaxima encontró un error durante la carga
	/home/mi_usuario/Documentos/fichero.wxm

	Failed to convert file
	"/home/mi_usuario/Documentos/fichero.wxm"
	to Unicode.

y es el segundo el que nos brinda la pista definitiva de lo que
ocurre. En efecto, parece sugerir que el fichero no está codificado en
`utf-8` como causa de que wxMaxima no pueda abrir el documento. ¿Cómo
salvar esta dificultad? Parece que lo razonable es cambiar la
codificación de `fichero.wxm` produciendo el `fichero_utf8.wxm`, que sí
podrá abrir wxMaxima, antes que tener un ordenador con Windows para
abrir estos documentos problemáticos.

Lo que sigue sirve para cambiar la codificación de toda clase de
ficheros; en particular puede ser útil para ficheros `.tex`, entre
otros. Para fijar ideas, haremos los ejemplos con el fichero `fichero.wxm`

[hacer click para descargar fichero wxm](/files/fichero.wxm)

que una vez descargado lo colocamos en:

	/home/mi_usuario/Documentos

y si estamos usando Mac OS X, lo situaremos en:

	/Users/mi_usuario/Documentos


## Cómo Saber la Codificación de un Fichero

Para poder cambiar la codificación, lo primero es saber en qué está
codificado el fichero problema, que  en nuestro ejemplo es
`fichero.wxm`. 

Siempre desde la consola/terminal de órdenes, que tendremos abierta, 
nos posicionamos en el lugar donde está el fichero problema; en nuestro
ejemplo conseguiremos esto con la orden de consola:

	cd /home/mi_usuario/Documentos

o bien

	cd /Users/mi_usuario/Documentos

si usamos Mac OS X. Ejecutaremos entonces la orden:

	file -i fichero.wxm

y obtendremos la siguiente información:

	fichero.wxm: text/x-c; charset=iso-8859-1

que nos dice que está codificado en `latin-1`, también conocido como
`iso-8859-1`.

## Cómo Cambiar la Codificación de un Fichero

La orden que cambia la codificación en Linux, y en Mac OS X, es `iconv`.

Una vez que hemos averiguado la codificación del fichero que queremos
tratar, en nuestro ejemplo es `fichero.wxm`, averiguamos con qué nombre
conoce `iconv` a esa codificación. Para ello ejecutaremos la orden de
consola:

	iconv -l

Se genera con ello una larguísima lista que contiene todos los
conjuntos de caracteres conocidos.  Advierte el mensaje que esto no
quiere decir necesariamente que todas las combinaciones de estos
nombres se puedan usar como parámetros de `from` y de `to` en la línea
de órdenes. Hay que tener en cuenta que un determinado conjunto de
caracteres puede aparecer con varios nombres.

Averiguamos, examinando esa lista, que la orden `iconv` entiende
`iso-8859-1` como `ISO-8859-1` y que `utf-8` es para ella `UTF-8`. Tal
nomenclatura es la que habremos de usar. 

En general, si queremos cambiar el fichero `fileCod01` de la
codificación `cod01` al fichero `fileCod02` codificado en la
codificación `cod02`, ejecutaremos la orden: 

	iconv -f cod1 -t cod2 -o fileCod02 fileCod01

La `f` es abreviatura de `from` y la `t` lo es de `to`. En nuestro
ejemplo tendríamos que ejecutar la orden:

	iconv -f ISO_8859-1 -t UTF-8 -o fichero_utf8.wxm fichero.wxm

El fichero `fichero_utf8.wxm` es idéntico a `fichero.wxm`, pero
codificado en `UTF-8` en lugar de `iso-8859-1`. Ahora `fichero_utf8.wxm` sí
puede ser abierto con nuestro wxMaxima de Linux y Mac OS X.

Podemos someter a prueba el fichero `fichero_utf8.wxm` y constatar que
efectivamente está cofificado en `UTF-8`. Para ello basta con usar de
nuevo la orden `file`, obteniendo el siguiente diálogo:

	file -i fichero_utf8.wxm 
	fichero_utf8.wxm: text/x-c; charset=utf-8

y ... esto es todo por hoy. 
