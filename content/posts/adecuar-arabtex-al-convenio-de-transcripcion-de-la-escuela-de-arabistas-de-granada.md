+++
title = "Adecuar Arabtex al convenio de transcripción de la Escuela de Arabistas de Granada"
date =  "2016-08-28"
author =  "M García"
description = "Adecuación de ArabTex al convenio de transcripción de las escuelas de estudios árabes de Madrid y Granada. Revista Al-Andalus"
tags = ["latex", "arabTex", "arab", "arabe", "postgres"]
+++

## Introducción

Por alguna razón que se nos escapa ahora mismo, los estudiantes de
letras no usan LaTeX, cuando las dificultades de LaTeX están en la
edición científica. La edición científica tiene la dificultad y la
potencia; la edición humanística, en cambio, no tiene la dificultad y
sí la potencia.

Nuestro deseo es difundir el software libre también entre los
humanistas y los artistas, así que hoy traemos aquí un modo de
escribir árabe con LaTeX, a través de Arabtex. Sin embargo, el uso de
Arabtex para los hispanos tiene una dificultad, cual es que al ordenar
transcribir a Arabtex, no puedes elegir más que entre unos cuantos
métodos, ninguno de los cuales es de los oficiales entre los arabistas
españoles o las escuelas de arabismo españolas o las revistas de
arabismo español.

Aunque el autor de Arabtex ---el eminente profesor Klaus Lagally, al
que tanto tenemos que agradecer--- conoce este problema, el hecho es
que todavía no ha podido incluir en Arabtex el convenio de
transcripción de la Escuela de Granada. Mientras llega ese esperado
momento, nosotros explicamos aquí cómo se haría.

En lo que sigue suponemos que ha realizado una instalación completa de
la distribución de LaTeX que se cononoce con el nombre de TeX Live. Si
ha instalado MiKTeX u otra el procedimiento es el mismo y sólo
cambiará la ubicación de los ficheros nombrados.

Por otra parte nosotros preferimos usar como editor `emacs`; si usted
prefiere otro, por favor cambie en lo que sigue `emacs` por el nombre
de su editor (p.e. `nano`). 

## La transformación del fichero `atrans.sty`

Lo primero será localizar en nuestra distribución el lugar en que se
situa el fichero `atrans.sty`. Tanto en Ubuntu como en Mac OS X
podemos emplear la orden `locate`. En el caso de Mac OS X pondremos al
día la base de datos de `locate` mediante:

	sudo /usr/libexec/locate.updatedb

y seguidamente, para hacer la búsqueda, ejecutaremos la orden:

	locate atrans.sty

La respuesta será entonces algo como esto:
	
	/usr/local/texlive/2015/texmf-dist/tex/latex/arabtex/atrans.sty

de lo que deducimos que

	/usr/local/texlive/2015/texmf-dist/tex/latex/arabtex/

es el camino al fichero que vamos a transformar. En el caso de Ubuntu
procederemos de forma análoga. Primero actualizamos la base de datos
de `locate`:

	sudo updatedb

y seguidamente hacemos la búsqueda ejecutando la orden:

	locate atrans.sty

La respuesta será entonces algo como esto:

	/usr/local/texlive/2015/texmf-dist/tex/latex/arabtex/atrans.sty

de lo que deducimos que

	/usr/local/texlive/2015/texmf-dist/tex/latex/arabtex/

es el camino al fichero que vamos a transformar. Como se ve, coinciden
en el caso de esos sistemas y ello no es una casualidad.

Antes de manipularlo, conviene hacer una copia del fichero
`atrans.sty` en el lugar en el que lo hemos encontrado. En Linux y Mac
OS X se puede usar a tal efecto la orden `cp` del sistema.

La manipulación se lleva a cabo como sigue (no olvide cambiar en lo
que le decimos nuestro camino por el suyo y nuestro editor preferido
por el suyo). Hemos de editar el fichero:
`atrans.sty`:

	sudo emacs /usr/local/texlive/2015/texmf-dist/tex/latex/arabtex/atrans.sty

y seguidamente proceder como le indicamos.

### Primero

En la línea 413, entre `uighure\tr@uighe` y `}` colocamos la
etiqueta del convenio notacional, que es la siguiente:

	spanish\tr@grsch

### Segundo

En la línea 400, por ejemplo después de

	\a@message {Uyghuric~English~transliteration.}}}

colocamos el siguiente párrafo que contiene el convenio notacional de
la Escuela de Granada:

	%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
	{\catcode `\^ 7 \catcode `\ =9 \catcode `\^^M=9 \catcode `\^^I=9
	\catcode `\~=10
	\gdef \tr@grsch {% define School of Granada transliteration
	%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
	\tr@zdmg \let \@ub \underbar
	\gdef \tr@R {g} \gdef \tr@G {\^{y}}
	\gdef \tr@X {j}
	\a@message {School~of~Granada~transliteration.}}}

### Tercero

En la línea 22, entre los créditos del documento y al final de los
mismos, una referencia a quien diseñó estas modificaciones así como
indicación de la fecha en que incluyó el añadido:

	% School of Granada Transcription appended by Francisco M. Garc\'{\i}a
	% Olmedo (folmedo@ugr.es), Granada, Spain (04/03/1995)
	%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

## Cómo usar el convenio de transcripción de la Escuela de Granada (spanish)

No hay más que seguir las instrucciones de escritura del manual de
Arabtex e incluir donde proceda:

	\settrans{spanish}

sustituyendo al renglón para la transcripción que indique el manual, si
lo que quiere es escribir de acuerdo con el convenio de transcripción
de la Escuela de Arabistas de Granada en lugar de otro.

[Aquí](http://www.ugr.es/local/folmedo/zip/arabe.zip) encontrará
algunos ejemplos de ficheros hechos para compilar usando Arabtex. En
los ficheros `.tex` encontrará el código y en el `.pdf` qué aparece al
compilar.

Y ... esto es todo por hoy.
