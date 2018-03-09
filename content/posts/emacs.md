+++
title = "Un Resumen de Atajos Fundamentales de emacs"
date =  "2018-03-09"
author =  "M García"
description = "Cada usuario de `emacs` tiene sus atajos predilectos. A continución enumeramos los nuestros, los que atesoramos, los que más hemos usado con el paso del tiempo."
tags = ["latex", "emacs"]
+++
# 
## Introducción

Cada usuario de `emacs` tiene sus atajos predilectos. A continución
enumeramos los nuestros, los que atesoramos, los que más hemos usado
con el paso del tiempo.

La productividad está en proporción directa con el número de atajos
usados; eres tanto más productivo cuanto menos llevas las manos
al ratón. Es así como hemos comprendido que `emacs` es el
mejor aliado en el escritorio, puede que también el único.

## El Manual en Español

[Manual de emacs en español](http://www.nongnu.org/emacs-man-es/)

Otros enlaces de interés:

1. [lapipaplena](http://www.lapipaplena.org/emacs-cuatro-cositas-para-no-perecer-en-el-intento/)
1. [crysol](http://crysol.org/es/node/279)
1. [antonio mario](http://antonio-mario.com/un-poco-de-emacs-iii-comandos-basicos-2/)
1. [Héctor Mora](http://www.fcaglp.unlp.edu.ar/~observacional/manuales/emacs_man.pdf)
1. [Manual completo de AUC TeX](ftp://ftp.dante.de/tex-archive/info/spanish/guia-atx/guia-atx.pdf)

## Significado de algunas teclas en la terminología de emacs

	Alt = M (meta)
	Crtl = C (control)
	SPC (barra espaciadora)
	RET (intro)
	RETRO (retroceso)
	TAB (tabuladora)

## Teclas de Socorro

	C-g   (cancela algo: una orden, una combinación de teclas, etc.)
	C-x u (deshace lo último)

## Abrir un Fichero

	C-x C-f (abrir un archivo)

## Salir

	C-x C-c (Salir de Emacs)
	C-x C-s (Guardar sin salir)
	C-x C-w (Guardar como)
	C-x s   (Guardar todos los ficheros abiertos. Preguntará.)

## Teclas para el Movimiento

	C-a     (ir al comienzo de una línea)
	C-e     (ir al final de una línea)
	C-f     (un caracter hacia adelante)
	C-b     (un caracter hacia atrás)
	M-f     (una palabra hacia adelante)
	M-b     (una palabra hacia atrás)
	C-n     (ir a la siguiente línea)
	C-p     (ir a la línea anterior)
	C-o     (Inserta linea en blanco a continuación del cursor)
	M-g g   (ir a una línea)
	M-a     (ir al inicio del párrafo)
	M-e     (ir al final del párrafo)
	C-v     (ir a la página siguiente)
	M-8 C-v (sube la línea de cursor hacia el borde superior de la pantalla 8 líneas)
	M-v     (ir a la página anterior)
	M-8 M-v (baja la línea de cursor hacia el borde inferior de la pantalla 8 líneas)
	M-<     (ir al principio del texto en el buffer)
	M->     (ir al final del texto en el buffer)
	C-l     (redibuja la pantalla. La primera vez que se pulsa, coloca la línea del cursor en el centro, la segunda arriba y la tercera abajo)

## Copiar, cortar y pegar

	C-SPC (inicio del marcado de texto)
	C-h   (marcar todo el buffer, “Seleccionar todo”)
	M-w   (copiar)
	C-w   (cortar)
	C-y   (pegar)

## Buscar alguna palabra

	C-s     (busca hacia adelante)
	C-r     (busca hacia atras)
	C-s C-s (repite la busqueda)

## Borrar

	M-d     (palabra despues del cursor)
	M-3 M-d (borra 3 palabras)
	C-k     (del cursor a fin de linea)
	M-3 C-k (borra 3 lineas)
	M-k     (todo el párrafo)
	M-3 M-k (borra 3 párrafos)
	C-x C-o	(elimina todas las lineas en blanco menos una)
	Esc-SPC	(elimina todos los blancos menos uno)
	M-\	    (elimina todos los blancos)

Nota.- Como puede verse, muchas órdenes admiten un dígito: Si borrar
una linea es `C-k`, borrar 5 líneas será `M-5 C-k`, lo cual significa
que se entra el dígito con la tecla `Alt` pulsada.

## Regiones

	C-SCP (coloca una marca donde está el cursor como comienzo de un bloque)

flechas: por medio de las flechas o de las teclas de avance y
retroceso de páginas se obtiene el final deseado del bloque.

	M-w     (copia un bloque, cuando hay uno marcado)
	C-y     (pega el último bloque marcado o el último bloque cortado o la ultima línea borrada o el último grupo de líneas borradas (después de varias veces `C-k`)
	C-w     (corta el bloque marcado)
	C-x C-x (cambio entre la posición de la marca y del cursor)
	C-x C-u (cambia a mayúsculas la región marcada)
	C-x C-l (cambia a minúsculas la región marcada)

## Rectángulos

Los rectángulos se marcan de la misma forma que las regiones aunque
visualmente aparece resaltada toda la región. Es decir, los
rectángulos se marcan con `C-SCP` y las flecha.

	C-x r k   (corta y copia el rectángulo)
	C-x r y   (pega un rectángulo donde está el cursor)
	C-x r o   (abre, en blanco, un espacio rectangular del tamaño del marcado. Hace los desplazamientos necesarios)
	C-x r c   (borra (deja en blanco) el espacio rectangular marcado pero no lo copia)
	C-x r d   (suprime el espacio rectangular marcado pero no lo copia)
	C-x r r 5 (copia el rect´angulo marcado, sin cortarlo, en el registro 5)
	C-x r i 5 (pega el rectángulo almacenado en el registro 5, en el sitio donde está el cursor)

## Ventanas

	C-x 2 (división horizontal de la ventana en dos)
	C-x 3 (división vertical de la ventana en dos)
	C-x 1 (deja sólo la ventana activa abierta)
	C-x o (cambiar de ventana)
	C-x 0 (eliminar ventana actual)
	C-x { (la acorta en dirección horizontal)
	C-x ^ (la alarga en dirección vertical)
	C-x } (alarga la ventana activa en dirección horizontal)

## Buffers

	C-x b   (cambiar de buffer)
	C-x k   (cierra la buffer actual; pide confirmación)
	C-x C-b (lista buffers; cada archivo se abre en un buffer)

## Marcas por si queremos volver a una línea más adelante o de aquí a varios días:

	C-x r m (solicita nombre para la línea marcada)
	C-x r b (ir a la línea marcada con el nombre que entremos, en este caso la “b”)
	C-x r l (lista todas las marcas)

## Imprimir

	C-u M-x ps-print-buffer-with-faces (imprimir a ps. Luego con ps2pdf pasar a pdf)
	M-x print-buffer                   (imprimir archivo con numeración y cabeceras)
	M-x lpr-buffer                     (imprimir sin numeración ni cabeceras)
	M-x print-region                   (imprimir trozo seleccionado con numeración y cabeceras)
	M-x lpr-region                     (imprimir trozo seleccionado sin numeración ni cabeceras)

## Varios

	M-x tetris                           (otros juegos: dunnet, snake, doctor, zone…)
	M-x help-with-tutorial-spec-language (abre buffer con los idiomas disponibles para el manual)
	F10                                  (abre la ayuda)
	C-x d                                (abre un directorio que se especifique)
	C-x RET f                   (entrar codificación de caracteres: latin-1-unix [latin1], utf-8-unix [utf8]..)

## Introducir Órdenes de la Shell durante la Sesión de emacs

	C-z     (suspende la sesion emacs y entra en la shell. Volver a emacs con fg o con %emacs)
	M-!     (muestra en el mini buffer un mensaje para entrar un comando y lo abre un una ventana)
	C-u M-! (inserta la salida del comando en la posición del cursor)

## Algunas posibilidades

	M-x recover file            (levantar respaldo del archivo)
	M-x describe-key RET        (y presionando una combinación de teclas devuelve el comando asignado)
	M-x apropos RET print       (muestra información de “print”)
	M-x list-faces-display      (ver lista y muestra de los estilos disponibles)
	M-x list-packages           (abre en un nuevo buffer la lista de paquetes disponibles para instalar)
	M-x set-foreground-color    (pedirá color en inglés para las fuentes)
	M-x set-background-color    (pedirá color para el fondo)
	M-x w3m-browser-url         (pedirá url para conectar. Precisa w3m-el)
	M-x global-linum-mode       (mostras/esconder números de linea)
	M-x global-visual-line-mode (cortar/no cortar palabras al final de la pantalla)
	M-x global-hl-line-mode     (resaltar la linea donde está el cursor)
	M-x goto-line               (ir a una linea que se especifique)
	M-x insert file             (para insertar en el cursor un archivo)
		
## Calendario
		
	M-x calendar (abre el calendario)
	C-u M-x      (preguntará el mes y año que desea centrado en el calendario de tres meses; el calendario ocupa su propio buffer, cuyo modo principal es el modo Calendar)

## Atajos de AUC TeX (`auctex`)

Suponiendo que tenemos instalado el paquete complementario que
llamamos Auctex y que facilita el uso con `emacs` de ficheros `.tex`
(podemos hacerlo mediante `M-x list-packages` seleccionando `auctex`,
si bien con Aquamacs está integrado por defecto):

	C-c ; (marca como comentario un párrafo seleccionado o lo desmarca)
	C-c % (marca como comentario el párrafo en el que está el cursor)
	C-c ' (marca como comentario el párrafo en el que está el cursor)
	
	C-c C-e     (pide seleccionar un entorno LaTeX para escribir su esquema)
	C-c RET     (pide la macro que queremos insertar)
	C-c C-m     (pide la macro que queremos insertar)
	C-c C-f C-b (inserta la macro LaTeX \textbf{})
	C-c C-f C-c (inserta la macro LaTeX \textsc{}, pequeñas versales)
	C-c C-f C-d (borra fuente)
	C-c C-f C-e (inserta \emph{})
	C-c C-f C-f (inserta \textsf{})
	C-c C-f RET (inserta \textmd{})
	C-c C-f TAB (inserta \textit{})
	C-c C-f C-i (inserta \textit{})
	C-c C-f C-n (inserta \textnormal{})
	C-c C-f C-r (inserta \textrm{})
	C-c C-f C-s (inserta \textsl{})
	C-c C-f C-t (inserta \texttt{})
	C-c C-f C-u (inserta \textup{})
	C-c C-f u   (genera un cuadro informativo)
	
	C-c C-j     (inserte \item )
	M-RET       (inserta \item )

	M-C-a       (el cursor va a la línea de apertura del entorno actual)
    M-C-e       (el cursor va a la línea de cierre del entorno actual)
