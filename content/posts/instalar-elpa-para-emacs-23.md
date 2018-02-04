+++
title = "Instalar ELPA para Emacs 23"
date =  "2016-03-19"
author =  "Yábir G."
description = "Los usuarios de `Emacs` saben de su editor preferido varias cosas: la independencia que les ha proporcionado del ratón por vernir de un tiempo en el que se vivía sin ratón, la sensación de control pleno que les confiere, la potencia del mismo y también la dificultad de su configuración al ir..."
tags = ["emacs"]
+++

## Introducción

Los usuarios de `Emacs` saben de su editor preferido varias cosas: la
independencia que les ha proporcionado del ratón por vernir de un
tiempo en el que se vivía sin ratón, la sensación de control pleno que
les confiere, la potencia del mismo y también la dificultad de su
configuración al ir un poquitín más allá de lo elemental.

En particular es complicada la gestión de instalar complementos para
`Emacs`. ¿Cuántas veces hemos recurrido a instalarlos mediante el
gestor de paquetes del sistema operativo, lo cual es menos que ideal?
El deseo de los usuarios de `Emacs` ha sido por largo tiempo disponer
de un sistema gestor de paquetes como: `apt`, `yum`, `homebrew`,
`pip`, etc. `Emacs 24` lo ha hecho finalmente; se llama
`package.el`. `ELPA` es el acrónimo de "Emacs Lisp Package Archive",
escrito originalmente por **Tom Tromey**. Está incluida en `GnuEmacs`
a partir de la versión 24. La biblioteca del gestor de paquetes para
el `ELPA` es `package.el`.

Pero ¿qué hay de `Emacs 23` y las versiones anteriores? Este post está
dedicado a los usuarios, puede que administradores, que están usando
`Emacs 23`, quizá porque están apurando una versión antigua de su
sistema y no pueden cambiar a `Emacs 24` o quizá porque encuetran en
la versión 23 algún rasgo que les interesa, etc. Explicaremos la
inexplicada y sencilla labor de poner `package.el` a disposición de su
`Emacs 23`.

## Descargar `package.el`

La descarga de la versión más reciente que conozco de `package.el`
puede hacerse 
[de aquí](http://git.savannah.gnu.org/gitweb/?p=emacs.git;a=blob_plain;hb=ba08b24186711eaeb3748f3d1f23e2c2d9ed0d09;f=lisp/emacs-lisp/package.el). Para
ello se puede usar el botón derecho del ratón o hacer clic con dos
dedos sobre el enlace y luego usar 'Guardar enlace como...'. El
resultado, si hacemos esto, será el fichero
`lisp-emacs-lisp-package.el` que quedará albergado digamos, por
ejemplo y para fijar ideas, en `Descargas` y representará la materia
prima de nuestra labor.

## Instalación de `package.el`

La instalación tiene dos partes sencillas:

1. crear el directorio `.emacs.d`
2. copia del fichero renombrado en el directorio `.emacs.d`
3. completación del fichero de configuración de `Emacs`

El directorio `.emacs.d` debería estar creado tras arrancar `Emacs` la
primera vez; si no existe por lo que sea, lo podemos crear manualmente:

	mdkir /home/suUsuario/.emacs.d

Si la descarga ha sido llevada a cabo según lo indicado, la copia del
fichero puede hacerse con la orden:

	mv /home/miUsuario/Descargas/lisp-emacs-lisp-package.el /home/miUsuario/.emacs.d/package.el

La operación estará concluida haciendo que el fichero `.emacs`, que
debe figurar en el directorio raíz del usuario `miUsuario`, comience
por el siguiente párrafo:

	(require 'package)
	;; Any add to list for package-archives (to add marmalade or melpa) goes here, e.g.:
	(add-to-list 'package-archives 
		'("marmalade" .
		  "https://marmalade-repo.org/packages/"))
	(package-initialize)

Por supuesto que si el fichero `.emacs` no existe allí, debemos crearlo
con cualquier editor, por ejemplo `nano` o el propio `Emacs`. En caso
de que al arrancar `Emacs 23` aparezca un error del tipo:

	Cannot load file, package...

entonces en lugar del código anterior debemos de incluir en `.emacs`
éste otro:

	(when
		(load
	      (expand-file-name "~/.emacs.d/package.el")))

## Uso de `package.el`

Para usar nuestra flamante instalación de `ElPA` no tenemos más que
arrancar `Emacs` y ejecutar la orden:

	M-x package-list-packages

donde `M-x` representa, como es bien sabido, la operación de pulsar la
tecla `Alt` y sin soltarla pulsar entonces la tecla `x`. Los usuarios
saben que podemos comenzar a escribir `pack` y usar la tecla de
tabulador para completar.

En
[este post](http://ubuntudriver.blogspot.com.es/2015/01/instalar-tex-live-en-ubuntu-sin-usar.html)
se muestra un ejemplo de como instalar una extensión mediante
`ELPA`. En el ejemplo mencionado se trata concretamente de instalar
`auctex`. Para más detalles sobre la instalación con `ELPA` se puede
consultar [éste otro post](http://ergoemacs.org/emacs/emacs_package_system.html).

### Para escribir este post han sido útiles los siguientes enlaces:

[ELPA](http://www.emacswiki.org/emacs/ELPA)

[Emacs: How to Install Packages Using ELPA, MELPA](http://ergoemacs.org/emacs/emacs_package_system.html)

[Install Emacs Lisp Package Archive (Emacs 21 and Emacs 22)](http://tromey.com/elpa/install.html)

[Instalar texlive sin usar apt-get](http://ubuntudriver.blogspot.com.es/2015/01/instalar-tex-live-en-ubuntu-sin-usar.html)

[Package.el](http://wikemacs.org/wiki/Package.el)

[(think)](http://batsov.com/articles/2012/02/19/package-management-in-emacs-the-good-the-bad-and-the-ugly/)

Y ... esto es todo por hoy.
