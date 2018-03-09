+++
title = "Crear un QRC con LaTeX e incluirlo en un fichero LaTeX"
date =  "2018-03-08"
author =  "M García"
description = "Los usuarios de LaTeX se muestran reacios a usar otras herramientas sipueden servirse de su preciado compilador para editar y que secomporta como una verdadera imprenta."
tags = ["latex", "qr"]
+++

## Introducción

Los usuarios de LaTeX se muestran reacios a usar otras herramientas si
pueden servirse de su preciado compilador para editar y que se
comporta como una verdadera imprenta. En este post venimos a ayudarlos
y a darles una razón más para seguir con "LaTeX para todo".

No hay que glosar la gran utilidad que tiene hoy codificar las
direcciones de internet en códigos QR de forma que se pueda acceder a
ellas desde dispositivos móviles. Estamos en la era de la inmediatez
... para lo bueno y para lo malo. Y también LaTeX sirve para esto.

En lo que sigue daremos la receta precisa para generar un código QR
con LaTeX y luego diremos como incluirlo en nuestro pdf generado con
LaTeX.

## Generación del código QR

Con el editor preferido haremos un fichero, digamos `miQRC.tex`, con
el siguiente contenido:

	\documentclass[12pt]{article}

	\usepackage{pst-barcode, auto-pst-pdf}

	\begin{document}

	\begin{pspicture}
		\psbarcode{http://wildunix.es/}{}{qrcode}
	\end{pspicture}

	\end{document}

y por supuesto que en lugar de la dirección

	http://wildunix.es/ 

pondremos la nuestra o lo que queramos. Seguidamente abrimos la
terminal (`Ctrl + Alt + t`) y con la orden `cd` nos desplazamos hasta
el lugar en donde tenemos el fichero `miQRC.tex`. Hecho esto
ejecutamos desde la terminal la orden:

	pdflatex -shell-escape miQRC

Como pueden ver, se han generado varios ficheros tras la ejecución de
la anterior orden; pues bien, nos interesa por ahora el que se llama
`miQRC-pics.pdf`. Éste será el fichero a incrustar en cualquier
fichero LaTeX.

## Inclusión del código QR en un fichero LaTeX

Supongamos que el fichero `miQRC-pics.pdf` está alojado en el directorio

	/home/miUsuario/Documentos/taller/

por supuesto que `miUsuario` será para cada cual un nombre como:
`luisaquero`, `albertml`, etc. ... cada cual tendrá su usuario
preferido. Ahora llega el momento de edita nuestro fichero de texto
que deseamos contenga el QRC. Tiene que incluir al menos lo siguiente
en su estructura:

    \documentclass[12pt]{article}
    \usepackage{graphicx}
    \usepackage[absolute]{textpos}
    \usepackage[utf8]{inputenc}
    
    \begin{document}
    
    \begin{textblock*}{297mm}(-4mm,135mm)
    \includegraphics[scale=1]{/home/miUsuario/Documentos/taller/miQRC-pics.pdf}
    \end{textblock*}
    
	\end{document}

y logicamente pondremos el texto que nos interese entre el
`\begin{document}` y el `\end{document}`. Hecho esto podemos compilar
en la forma habitual con `pdflatex`, es decir, con el formato que
genera un pdf como resultado. Por supuesto que cada cual puede jugar
con los números que aparecen arriba: `297, -4, 135` y así podrá situar
el cuadradito del código QR con el tamaño y en el lugar que prefiera
del texto.

Y ... esto es todo por hoy.
