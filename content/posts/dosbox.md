+++
title = "Instalación y uso de DOSBox"
date =  "2019-07-04"
author =  "M. Garcia"
description = "MS-DOS (siglas de MicroSoft Disk Operating System, Sistema operativo de disco de Microsoft) es un sistema operativo para computadores basados en x86. Fue el miembro mas popular de la familia de sistemas operativos DOS de Microsoft, y el principal sistema para computadoras personales compatible con IBM PC en la década de1980 y mediados de 1990, hasta que fue sustituida gradualmente por sistemas operativos que ofrecían una interfaz gráfica de usuario, en particular por varias generaciones de Microsoft Windows."
tags = ["dosbox"]
+++

## Introducción

DOSBox es un emulador de MS-DOS.

MS-DOS (siglas de MicroSoft Disk Operating System, Sistema operativo de disco de Microsoft) es un sistema operativo para computadores basados en x86. Fue el miembro mas popular de la familia de sistemas operativos DOS de Microsoft, y el principal sistema para computadoras personales compatible con IBM PC en la década de1980 y mediados de 1990, hasta que fue sustituida gradualmente por sistemas operativos que ofrecían una interfaz gráfica de usuario, en particular por varias generaciones de Microsoft Windows.

La solución es usar un emulador de MS-DOS y en la actualidad hay dos famosos: dosemu y DOSBox. El primero es excelente pero sólo está actualmente para Linux; el segundo es también muy bueno y sí está para todas las plataformas. ¿Cuál elegir? pues los DOS ya que nuestro sistema de cabecera es Ubuntu.

Este post está dedicado a DOSBox y luego ofreceremos otro para dosemu. En cualquiera de los casos, aquí explicaremos el uso elemental y dejaremos que el usuario se informe en la red del avanzado.

## Descarga

La descarga para Mac OS X y Windows puede ser a través de la [página oficial](http://www.dosbox.com/download.php?main=1) o de [ésta otra](http://sourceforge.net/projects/dosbox/files/dosbox/). En el caso de Ubuntu, la instalación se hace con `apt` como diremos más adelante.


## Instalación y adecuación

### En Ubuntu

Abrimos nuestro terminal (`Ctrl+Alt+t`) y ejecutamos en ella la orden:

	sudo apt install dosbox

y cuando acabe el proceso de instalación ejecutamos DOSBox. Veremos que se abre la pantalla negra típica de esta utilidad, seguidamente la cerramos. En nuestro navegador interno en la estructura de archivos pulsamos `Ctrl+h`, con lo cual hacemos visibles los ficheros ocultos. En el directorio raíz del usuario que ejecutó DOSBox habrá uno de esos directorios ocultos llamado .dosbox. Si entramos en él veremos un fichero llamado algo así como `dosbox-0.74.conf` (el "0.74" es por la versión instalada, otros podrían tener otro número, aunque lo que importa es el `.conf`.

Usaremos nuestro editor preferido para abrir el fichero `dosbox-0.74.conf` y lo modificaremos de acuerdo con lo siguiente. En el apartado `[dos]` cambiaremos la línea

	keyboardlayout=auto

por la línea

	keyboardlayout=sp

con ello tendremos en español el teclado que entiende DOSBox. En el apartado `[autoexec]` incluiremos bajo las líneas de comentario (comienzan por #) la siguiente:

	mount c /home/mi_usuario/Documentos

Esto hace que cuando arranquemos DOSBox, comprenda que hay una unidad `C:` que corresponde con el directorio `/home/mi_usuario/Documentos`. Por supuesto que en lugar de `c` se podría haber elegido otra letra, igual que se podría haber elegido otro destino diferente a `Documentos`. Todo según nuestro interés.

Cuando arranquemos de nuevos DOSBox, podemos ejecutar la orden

	c:

en la línea de órdenes del programa, es decir tras el mensaje `Z:\>`, con lo que entraremos desde DOSBox en el directorio de trabajo deseado, en nuestro caso `/home/mi_usuario/Documentos`.

### En Mac OS X

Una vez bajado e instalado el paquete correspondiente, ejecutamos la aplicación DOSBox. Aparece la pantalla negra típica y dentro el prompt `Z:\>`.

A diferencia del caso de Ubuntu, no está creado el fichero `dosbox.conf`, por lo que tendremos que crearlo. Esto se hace ejecutando la orden:

	config -writeconf dosbox.conf

desde la línea de órdenes de DOSBox (en la línea que empieza por `Z:\>`). Pero, ¡cuidado! ahora tenemos el teclado inglés, por lo que el signo "-" hay que sacarlo pulsando sin mayúsculas en la tecla `?` es decir, pulsando en `'`. Esta operación crea el fichero `dosbox.conf` en Aplicaciones. Ahora debemos modificar el fichero `dosbox.conf` procediendo igual que en el caso de Ubuntu con el fichero `dosbox-0.74.conf`. La única diferencia es que en lugar de `mount c /home/mi_usuario/Documentos` escribiremos:

	mount c /Users/mi_usuario/Documents

Como decíamos antes, para entrar desde DOSBox en el directorio de trabajo, ejecutaríamos ahora desde la línea de órdenes `Z:\>` la orden:

	c:

## En Windows 7

Una vez bajado e instalado, hemos de saber que DOSBox está por defecto en el lenguaje ambiente del ordenador, español en nuestro caso.

Si queremos generar el fichero `dosbox.conf`, arrancaremos DOSBox y ejecutaremos en la línea del prompt `Z:\>` la orden

	config -writeconf dosbox.conf

Con ello se creará el fichero dosbox.conf en el directorio:

	Archivos de programa (x86)\DOSBox-0.74

si es que es allí donde hemos elegido instalar DOSBox (recomendamos ese lugar que es el que ofrece Windows 7 por defecto). Ahora podríamos modificar `dosbox.conf` con la añadidura del directorio `C:`, pues hemos dicho que no es necesario cambiar el idioma. La modificación sería añadir:

	mount c C:\Users\mi_usuario\carpeta_preferida

como última línea del fichero, en la sección `[autoexec]`. Ahora iremos a la carpeta de instalación de DOSBox (en nuestro caso es `c:\Archivos de programa (x86)\DOSBox-0.74`), situaremos el puntero sobre el archivo `DOSBox.exe`, pulsaremos el botón de la derecha y haremos clic en "Crear acceso directo". Aparecerá entonces el fichero "DOSBox.exe - Acceso directo"; situaremos el puntero sobre él, haremos clic con el botón de la derecha y pulsaremos sobre "Propiedades". Se abre entonces una ventana; en la casilla "Destino" de la misma añadiremos lo siguiente al final de lo que haya escrito:

	-userconf -conf dosbox.conf

con lo que quedará en dicha casilla "Destino" el texto:

	"C:\Program Files (x86)\DOSBox-0.74\DOSBox.exe" -userconf -conf dosbox.conf

Ahora el acceso directo modificado lo arrastraremos al escritorio o lo anclaremos en la barra de tareas o bien lo anclaremos en el menú de inicio; otra idea interesante es llevarlo a una carpeta donde coleccionaremos accesos directos a DOSBox, pero cada uno sobre un `dosbox.conf` creado con distinto nombre y contenido según el juego con el que deseemos jugar o la utilidad que queramos arrancar al pulsar ese acceso directo.

Si preferimos no tener estos accesos directos y sencillamente montar en cada momento el directorio que nos interese como unidad C:, entonces ejecutaremos cada vez que arranquemos DOSBox las órdenes:

	Z:\> mount c C:\Users\mi_usuario\carpeta_preferida
	Z:\> c:

y así entrar en el directorio de trabajo que hemos abreviado por c.

## Truco

Para cualquier plataforma, en la sección `[autoexec]` de `config.conf`, podríamos escribir en lugar de lo equivalente a `mount c /home/mi_usuario/Documentos`, lo equivalente a las siguientes tres líneas:

	mount c /home/mi_usuario/Documentos
	c:\
	dir

## Nota final

Han sido probadas otras utilidades que fracasan en DOSBox y en algunas plataformas cuando es requerido cierto nivel de prestaciones gráficas, al contrario que en dosemu en el que no fracasan y se ejecutan más rápidamente. Por ello, en un próximo post hablaremos de `dosemu` como alternativa.

Y ... esto es todo por hoy.
