+++
title = "Almacenar contraseñas de forma segura"
date =  "2017-08-14"
author =  "Yábir G."
description = "Es recomendable usar distintas contraseñas para cada servicio que utilicemos, además de cumplir ciertos requisitos como tener una longitud considerable o incluir caracteres de distinto tipo; pero a medida que acumulamos contraseñas se vuelve cada vez más difícil recordarlas y es frecuente que opt"
tags = ["contraseñas", "almacenar", "seguro", "seguridad"]
+++


## Introducción

Cada vez son mas frecuentes los ataques a sitios webs con la pretesión
de apoderarse de grandes cantidades de contraseñas, para aprovechar el
error que comete mucha gente de usar la misma contraseña en distintos
sitios de internet.

Lo recomendable es usar distintas contraseñas para cada servicio que
utilicemos, además de cumplir ciertos requisitos como tener una
longitud considerable o incluir caracteres de distinto tipo; pero a
medida que acumulamos contraseñas se vuelve cada vez más difícil
recordarlas y es frecuente que optar por repetir contraseñas.

Para evitar esto la mejor solución es utilizar un "baul de
contraseñas".

## ¿Qué es un baul de contraseñas?

Es una base de datos en la que se guardan los nombres de usuario y
contraseñas para cada servicio de manera que esten protegidas usango
algún sistema de cifrado. Para cifrar las contraseñas se usa una
**única** contraseña, que será la que tendremos que recordar y que es
con la que se cifran el resto de contraseñas.

Esto aporta una gran ventaja porque ahora únicamente tenemos que
recordad una contraseña, lo que hace que ésta pueda ser más larga y
complicada de robar.

## ¿Qué baul puedo usar?

Una alternativa gratuita es [LastPass](https://www.lastpass.com/)
(incluye servicios premium pero se puede usar sin problema de manera
gratuita). Existen más, como [Enpass](https://www.enpass.io/) que cobra
solo por su aplicación movil o [KeePass](http://keepass.info/)

La ventaja que tienen las dos primeras aplicaciones es que nuestras
contraseñas estan sincronizadas en todos nuestros dispositivos sin
tener que preocuparnos de configurar nada. Así podremos acceder a
ellas con extensiones de navegador o aplicaciones de escritorio desde
cualquier plataforma, pero por lo general no son de código abierto y
sabemos poco de como están almacenadas nuestras contraseñas mas allá
de una forma segura.

## KeePass

KeePass está disponible para todas las plataformas, pero deja al
usuario la tarea de mantener una copia de la base de datos
sincronizada con el resto de dispositivos.

En sistemas basados en `arch` existen varias versiones, tanto una
oficial como varias mantenidas por la comunidad. Recomendamos
`KeePassXC` que por defecto trae un plugin para poder sincronizarlo
con el navegador, aunque a las otras versiones disponibles se les puede
instalar sin mayor problema. Todas se encuentran disponibles en los
repositorios oficiales o en los de la comunidad.

Para instalarlo desde la línea de órdenes podemos usar: 

	pacman -Sy keepassxc

Al instalarlo tendremos que crear una nueva base de datos y se nos
preguntará por una clave. Como se ha dicho antes es conveniente que
sea larga y facil de recordar, aunque no sea muy complicada, algo así
como `estaSemanaTrabajo5HorasMenos` que nos aporta una seguridad
bastante buena como se puede comprobar
[aquí](http://www.passwordmeter.com/)

Una vez creada nuestra base de datos podemos almacenar las
contraseñas en ella o importarla desde otros servicios como se indica
en [este enlace](http://keepass.info/help/base/importexport.html).

Para poder rellenar los formularios de manera automática sin tener que
copiar la contraseñá desde KeePass existen varias extensiones de navegador:

* En chrome
  uso
  [chromelPass](https://chrome.google.com/webstore/detail/chromeipass/ompiailgknfdndiefoaoiligalphfdae)

* En firefox está
  disponible
  [PasslFox](https://addons.mozilla.org/en-US/firefox/addon/passifox/)

## Algunas recomendaciones

Si es la primera vez que usa un baul de contraseñas es recomendable
que no almacene todas las contraseñas la primera vez y deje un tiempo,
hasta que tenga un poco de experiencia con su funcionamiento para
poder ver que es capaz de recordar la contraseña maestra; ya que si la
pierde no tendrá acceso a las contraseñas almacenadas.

Es también recomendable usar lo que se conoce como `autentificación en
dos pasos` en aquellos sitios que ofrezcan esta manera de
acceder. Consiste en introducir un código temporal tras haber
introducido correctamente la contraseña y al que sólo el propietario
tiene acceso. De esta manera evitamos que si alguien se apodera de la
contraseña tenga acceso a nuestros datos. Hay que tener en cuenta que
el envio de este codigo por SMS no es la mejor solución, ya que se ha
demostrado que este método es vulnerable. No obstante esto es mejor
que nada.
