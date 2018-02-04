+++
title = "Instalar TeX Live en Ubuntu, Mac OS y Windows"
date =  "2017-08-11"
author =  "M. García"
description = "Instrucciones para la realización de una instalación de TexLive en Ubuntu, Mac Os y Windows"
tags = ["enigma", "haskell", "máquina"]
+++


## Introducción

Es incontrovertible la gran potencia de edición que ofrece LaTeX,
desarrolle usted su actividad en las letras o en las ciencias; los
primeros tiene la potencia sin la dificultad, con lo que todo son
ganacias.

Para concer más detalles sobre el sistema y sus autores, Donald
E. Knuth y Leslie Lamport, visite la entrada de
[LaTeX en la Wikipedia](https://es.wikipedia.org/wiki/LaTeX).

Como es bien sabido, el software que se instala en Ubuntu/Debian con
apt-get es a menudo antiguo. Es el caso de la distribución de Tex Live
de LaTeX al instalar Ubuntu 14.04 LTS, con apt-get se instalaba TeX Live
2013/Debian pudiendo bien estar viva ya la versión TeX Live 2014 u
otra posterior.

En este post explicaremos como instalar la última distribución de TeX
Live en nuestro Ubuntu o cualquier otro Linux. Para ello evitaremos
obviamente el uso de `apt-get` o herramientas equivalentes según el
sistema.

Realmente la decisión de no utilizar `apt-get` en Ubuntu para instalar
TeX Live conlleva, como diremos, no instalar con `apt-get`: `auctex`,
`catdvi`, `sagemath`, etc.

## Instalar TeX Live en Ubuntu sin usar `apt-get`

Instalación de TeX Live

`texlive` es nuestra distribución preferida de LaTeX. Resulta penoso
ver como con nuestra instalación de Ubuntu, si instalamos texlive con
`apt-get`, casi siempre instalamos una versión antigua de nuestro LaTeX.

Para evitarlo podemos hacer una instalación manual de texlive por
medio de las siguientes instrucciones.  Lo primero es instalar
`perl-tk`:

	sudo apt-get install perl-tk

seguidamente, según figura en
[Installing TeX Live over the Internet](https://www.tug.org/texlive/acquire-netinstall.html),
podemos
[bajar el instalador](https://www.tug.org/texlive/acquire-netinstall.html)
para Unix: un pequeño programita que nos facilita la instalación.  Si
se prefiere algo universal podemos
[bajar este otro instalador](http://mirror.ctan.org/systems/texlive/tlnet/install-tl.zip),
pero no es necesario ni recomendable en modo alguno para nuestro
Ubuntu. Suponiendo que tenemos ya `install-tl-unx.tar.gz`, abrimos la
consola y vamos al lugar donde se ha ubicado, supongamos para fijar
ideas que es `/Descargas` tras lo que ejecutaremos

	tar -xf install-tl-unx.tar.gz

colgando de `/Descargas`, tras la descopresión, se habrá generado un directorio con
un nombre algo así como éste:

	install-tl-20150125

entrando en ese directorio (`mi_usuario` es como habitualmente el nombre
de nuestra cuenta en nuestro equipo)

	cd /home/mi_usuario/Descargas/install-tl-20150125

(cada cual sustituirá `mi_usuario` e `install-tl-20150125`œ por lo que
proceda en su situación) y seguidamente ejecutamos lo siguiente:

	sudo ./install-tl -gui perltk

Se abrirá una ventana titulada `Install-tl` con la que se puede
adecuar a voluntad las condiciones de la instalación.  Recomendamos
firmemente hacer sólo un cambio, y no dejar de hacerlo: pulsar en el
botón "Cambiar" del último apartado titulado "Crear enlaces simbólicos
en los directorios de sistema", pulsar en el botón adjunto al epígrafe
"crear enlaces simbólicos en los directorios estándar" (se pondrá
rojo) de la pequeña subventanita que se abrirá y finalmente pulsar en
el botón de ésta "Aceptar". Con ello mandamos crear los
importantísimos enlaces simbólicos (esto se hace ya automáticamente):

	ficheros ejecutables en: /usr/local/bin
	páginas de manual en: /usr/local/share/man
	información en: /usr/local/share/info

Antes figuraba junto al epígrafe "Crear enlaces simbólicos en los
directorios de sistema" de la ventana principal la palabra "No" y
ahora figura la palabra "Sí". Pulsamos tras cerciorarnos de ello el
botón con la inscripción "Instalar Tex Live".  Ahora comienza un
proceso de instalación que puede durar algún tiempo. Cuando acabe
actualizaremos lo instalado desde la consola con las órdenes:

	sudo tlmgr update --self
	sudo tlmgr update --all

y periódicamente podemos, y debemos, repetir estas dos órdenes para
mantener nuestro sistema actualizado. Podemos consultar el uso de
`tlmgr` en [este manual](https://www.tug.org/texlive/doc/tlmgr.html).

## Instalar Texlive en Mac OS

La distribución TeX Live para Mac OS viene servida desde el sitio
oficial de [MacTeX](https://tug.org/mactex/), donde hay instrucciones
detalladas aunque el proceso no tiene complicación alguna "dejándose
llevar" y leyendo las instrucciones; el instalador es
[MacTeX.pkg](https://tug.org/mactex/mactex-download.html).

Nosotros instalaríamos previamente `Xcode` desde el Mac App Store y
seguidamente `Command Line Tools`. Para hacer esto último, podemos
[ejecutar en la terminal](http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/):

	xcode-select --install

y dejarse llevar.

## Instalar Texlive en Windows

### En Windows 7

Para la instalación en Windows 7, siga las siguientes instrucciones:

* Descargue `install-tl.zip` de
  [http://tug.org/texlive/acquire-netinstall.html](http://tug.org/texlive/acquire-netinstall.html)
  y descomprímalo en cualquier lugar de su preferencia. Al acabar
  podrá borrar lo que resulta de esta descompresión.

* Al descomprimir `install-tl` ir a la carpeta con nombre similar a
  `install-tl-20162017`.

* Desactive temporalmente el antivirus hasta que concluya la instalación
  y procure que no salte el salvapantallas o se active la hibernación
  del equipo. Todo ello podría impedir o dificultar la instalación.

* Sobre `install-tl-windows.bat`, pulse botón derecho del ratón.

* Haga clic sobre "Ejecutar como administrador".

* Responda "Sí" a la pregunta: ¿Desea permitir ... en el equipo?

* Se abrirá la ventana emergente 1/5; ponga tic sobre "Change default
  repository" y pulse "Siguiente".

* En la ventana 1-1/5 desplegar posibilidades de "Mirror" y elegir
  "Spain [http://osl.ugr.es/CTAN](http://osl.ugr.es/CTAN)" o cualquier
  otro repositorio si éste no estuviese disponible; pulse ahora
  "Siguiente".

* En la ventana que emergerá 2/5 y recomienda "Ruta de instalación", NO
  cambie y pulse en "Siguiente".

* No toque nada en la siguiente ventana que se abre 3/5 y pulse
  "Siguiente".

* En la ventana siguiente 4/5 pulse sobre "Instalar".

* La ventana 5/5 es la de instalación; espere a que concluya.


### En Windows 10

* Descargue `install-tl.zip` de
  [http://tug.org/texlive/acquire-netinstall.html](http://tug.org/texlive/acquire-netinstall.html)
  y descomprímalo en cualquier lugar de su preferencia. Al acabar
  podrá borrar lo que resulta de esta descompresión.

* Vaya a la carpeta generada al descomprimir `install-tl`; será algo
así como `install-tl-20162017`.

* Desactive temporalmente el antivirus hasta que concluya la instalación
  y procure que no salte el salvapantallas o se active la hibernación
  del equipo. Todo ello podría impedir o dificultar la instalación.

* Sobre `install-tl-windows.bat` (puede que en su equipo no esté
  visible la extensión `.bat`), pulse botón derecho del ratón.

* Haga clic sobre "Ejecutar como administrador".

* Responda "Sí" en la ventana "¿Quieres permitir que esta aplicación
  haga cambios en el dispositivo?".

* Se abrirá la ventana emergente 1/5; ponga tic sobre "Change default
  repository" y pulse "Siguiente".

* En la ventana 1-1/5 desplegar posibilidades de "Mirror" y elegir
  "Spain [http://osl.ugr.es/CTAN](http://osl.ugr.es/CTAN)" o cualquier
  otro repositorio si éste no estuviese disponible; pulse ahora
  "Siguiente".

* En la ventana que emerge 2/5 y recomienda "Ruta de instalación", NO
  cambie y pulse en "Siguiente".

* No toque nada en la siguiente ventana que se abre, la 3/5, y pulse
"Siguiente".

* En la ventana siguiente 4/5 pulse sobre "Instalar"

* La ventana 5/5 es la de instalación, espere a que concluya.

## Instalación de `GNU Emacs` 

Han pasado décadas y cada día nos parece más actual, práctico y
eficiente, para quien necesita escribir realmente a LaTeX, el sabor
`LaTeX + emacs`. Un conocido muy versado en LaTeX elogió una vez al
anciano editor diciendo: "emacs es el editor del pasado venido del
futuro"; creemos que acertó.

### Instalación de `emacs` en Ubuntu

Está claro que no es conveniente instalar auctex mediante apt-get,
pues se llevaría a cabo una instalación paralela de LaTeX que en modo
alguno conviene tener en el árbol del sistema interactuando con la
instalación "manual" antes realizada.

No hay problema en instalar `emacs` disfrutando de la comodidad de
`apt-get`; eso sí, instalaremos la última versión estable ofrecida (al
momento de escribir el post era la Emacs 24.3.1). Para ello:

	sudo apt-get install emacs24

La adecuación de `emacs` es similar a la de Aquamacs, explicada más
abajo.

### Instalación de `emacs` en Windows

Encontrará las instrucciones de descarga en:

[http://www.gnu.org/software/emacs/download.html](http://www.gnu.org/software/emacs/download.html)

y podrá ser descargado del uno de los nexos indicados en dicha página.
Hemos usado:

[http://ftp.gnu.org/gnu/emacs/windows/](http://ftp.gnu.org/gnu/emacs/windows/)

Hemos bajado y descomprimido el fichero
`emacs-25.1-2-x86_64-w64-mingw32.zip`. Seguidamente haga lo siguiente:

* Se genera el directorio `bin`

* Ponga el cursor sobre `runemacs.exe` (puede que en su sistema sólo
  vea `runemacs` porque lo tenga modificado para no mostrar las
  extensiones de los ficheros) y pulse botón derecho y luego sobre
  "Abrir". Así probará que funciona.

* Mueva el directorio `bin` que resulta de descomprimir a la
  carpeta: `C:\Archivos de Programa\emacs\bin`, por ejemplo.

* Ponga el cursor sobre `runemacs` o `runemacs.exe` y pulse botón
  derecho, seguidamente pulse sobre `Crear acceso directo`.

* Surgirá `runemacs - Acceso directo`; como acceso directo que es,
  arrástrelo a un lugar cómodo para poder usarlo.

### Instalación de `auctex`

Una vez que tengamos instalado TeX Live y `emacs`, pasaremos a
instalar `auctex`. Hay muchas formas de hacerlo, pero la que
preferimos nosotros por su facilidad, su efectividad y la
universalidad es la siguiente:

* Abra `emacs`

* Pulse la secuencia `M-x list-packages` (por supuesto `M-x` significa
`Alt-x`)

Si no queremos, o no podemos, usar el ratón:

* lleve el cursor hasta `auctex`

* `Ctrl-x o` (lleva a la segunda subventana que acaba de abrirse),
  `Ctrl-i` (lleva al botón de "Install"), `Intro`, seguidamente `y`
  para responder y finalmente `Intro`.

Si prefiere usar el ratón:

*  haga clic sobre `auctex`

*  haga clic sobre el botón de "Install" en la subventana que se ha abierto

*  haga clic sobre el botón de "Yes"

En cualquiera de los casos, sabremos que ha acabado el proceso cuando
veamos en la subventana inferior el mensaje "Installed".

### Instalación de `emacs` en Mac OS

Para Mac OS existe una excelente versión de `emacs` llamada
[`Aquamacs`](http://aquamacs.org/). La
[descarga de Aquamacs](http://aquamacs.org/download.shtml) proporciona
un fichero `.dmg` que se instala en la forma usual y archiconocida de
instalar estos paquetes en nuestro Mac OS. La configuración puede ser
hecha a gusto del usuario. Sugerimos realizar las siguientes
operaciones:

	Options > Option, Command, Meta keys > ... Meta  & Spa

con esto hacemos que reconozca nuestro teclado español. La selección
del molde de letra preferida hay que hacerla para cada tipo de
fichero, pudiendo tener un molde de letra para cada uno de ellos. Por
ejemplo, para ficheros `.tex` nos ha gustado el juego `Monaco` (una
alternativa interesante es
[SimSun](http://www.myfontfree.com/download-simsun-zip75734.htm)), así
que abrimos uno de esto ficheros que tengamos a mano y hacemos:

	Options > Appearance > Fonts for Latex-Mode...

seleccionamos en la caja que se abre los siguiente parámetros:

	All Fonts + Monaco + Normal + 16

Si hubiésemos cargado un fichero `.txt`, aparecería "Fonts for
Text-Mode..." en lugar de "Fonts for Latex-Mode...". Así procedemos
con todos los tipos de fichero que queramos, eligiendo a nuestro gusto
la fuente de letra y su tamaño.

Seguidamente seleccionamos la codificación que nos guste; recomendamos
fuertemente elegir `UTF-8`. Para ello:

	Options > Language > Set Language Environment > UTF-8

Otra opción interesante es:

	Options > View > Size Indication

Finalmente, para guardar todas las opciones elegidas, hacemos:

	Options > Save Options

No olvidemos desmontar el disco virtual de instalación de Aquamacs que
surgió al principio de todo, cuando hicimos doble clic sobre
"Aquamacs-Emacs-3.3.dmg". Esto se consigue haciendo clic sobre el
icono del disco virtual "Aquamacs Emacs" que aparece sobre el tapiz de
nuestra mac mientras tenemos pulsada la tecla "Ctrl", seleccionando
luego "Expulsar Aquamacs Emacs".

### Alternativas a `emacs`

Muchos "usuarios" prefieren instalar como editor para LaTeX la
utilidad Texmaker. Otros usuarios prefieren
[Sublime Text 3](https://www.sublimetext.com/3) o
[Atom](https://atom.io/). Puede usar eso como alternativa a `emacs` o
cualquier otra utilidad que le sugieran por ahí entre mil
alternativas.

En cualquier caso elija algo que no le haga prisionero del ratón, pues
en tal caso su mano pasará más tiempo en el ratón que en el teclado y
ello dificultará la edición. Recomendamos firmemente usar un editor
con una gran colección de atajos de teclado, tipo `emacs`, de forma
que pudiera escribir sin la ayuda del ratón.

## Sharelatex

Conviene usar LaTeX en una instalación nativa en su ordenador, pero si
no la tuviera a mano considere abrir una cuenta y usar
[Sharelatex](https://www.sharelatex.com?r=1412eb69&rm=d&rs=b) por sus
innegables ventajas.

## Muy importante

Si es usuario de `sagemath` en Linux, puede no ser conveniente
instalar esta utilidad desde repositorio, pues ello significará una
instalación paralela de LaTeX que quedará inconvenientemente
superpuesta a la anteriormente hecha. La alternativa es la instalación
manual, que es muy limpia y está explicada con sumo detalle en
[éste nuestro post](https://wildunix.es/posts/instalar-sage-en-mac-os-x-ubuntu-y-otros-linux-uso-bajo-windows/).

Por la misma razón deberemos evitar instalar catdvi.

## Nexos de Interés

[Donald E. Knuth (高德纳)](http://www-cs-faculty.stanford.edu/~knuth/)

[Comprehensive TeX Archive Network (CTAN)](https://www.ctan.org/)

[LaTeX Templates](http://www.latextemplates.com/)

[The LaTeX Font Catalogue](http://www.tug.dk/FontCatalogue/)

[LaTeX en Wikibooks](https://en.wikibooks.org/wiki/LaTeX)

[TeX en stackexchange](http://tex.stackexchange.com/)

[Modelos de Portadas](http://tex.stackexchange.com/questions/85904/showcase-of-beautiful-title-page-done-in-tex)

[Tikz](http://www.texample.net/tikz/)

[Brevísima Introducción a Emacs de D. Hector M. Mora Escobar](http://www.fcaglp.unlp.edu.ar/~observacional/manuales/emacs_man.pdf)

[http://wwwae.ciemat.es/~oglez/webcms/oginfo/combinaciones_emacs.html](http://wwwae.ciemat.es/~oglez/webcms/oginfo/combinaciones_emacs.html)

[http://www.maths.manchester.ac.uk/~gb/emacs/installauctex.pdf](http://www.maths.manchester.ac.uk/~gb/emacs/installauctex.pdf)

[http://www.gnu.org/software/auctex/](http://www.gnu.org/software/auctex/)

[https://www.gnu.org/software/auctex/manual/auctex.index.html](https://www.gnu.org/software/auctex/manual/auctex.index.html)

[http://www.emacswiki.org/emacs/AUCTeX](http://www.emacswiki.org/emacs/AUCTeX)

[http://www.blackhats.es/wordpress/?p=209](http://www.blackhats.es/wordpress/?p=209)

[https://support.google.com/chrome/answer/157179?hl=es](https://support.google.com/chrome/answer/157179?hl=es)

Y ... esto es todo por hoy.

## Posdata 

La instalación de TeX Live aquí explicada soluciona el problema que
presenta el editor Atom con los paquetes añadidos:

* latex
* language-latex

al intentar compilar un fichero `.tex`, arrojando el mensaje de error:

	TypeError: Cannot read property 'outputFilePath' of undefined
	at /home/miUsuario/.atom/packages/latex/lib/latex.coffee:63:24
	at /home/miUsuario/.atom/packages/latex/lib/builders/latexmk.coffee:17:9
	at ChildProcess.exithandler (child_process.js:752:5)
	at ChildProcess.emit (events.js:110:17)
	at maybeClose (child_process.js:1013:16)
	at Socket. (child_process.js:1181:11)
	at Socket.emit (events.js:107:17)
	at Pipe.close (net.js:461:12)

y que ha sido explicado en el "issue outputFilePath #37" de GitHub
sobre el editor Atom.
