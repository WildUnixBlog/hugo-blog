+++
title = "Acceder a un servidor mediante ssh y un certificado"
date =  "2016-03-20"
author =  "M. Garcia"
description = "Certificados (publicos/privados) para acceso ssh. Ubicación local y remota. Importancia y métodos."
tags = ["ssh", "certificado"]
+++

## Introducción

Cuando no es abierta una cuenta en un servidor, el administrador tiene
un problema, cual es hacernos llegar la contraseña. Si la envía en
abierto, estará comprometida; si la envía cifrada, ¿hasta cuándo puede
extenderse la labor de compartir contraseñas?

La solución a esta dificultad se basa en facilitar el acceso del
usuario mediante el uso de certificados. El administrador: abrirá la
cuenta, habilitará el servidor para poder acceder a las cuentas
mediante certificados, notificará al usuario que su cuenta está
abierta y quedará a la espera del certificado público de acceso.

Por su parte, el usuario procederá a generar una pareja de
certificados: el público y el privado, le hará llegar al administrador
el público en abierto y éste procederá a habilitar el acceso mediante
él y el privado, que sólo conocerá el usuario y tendrá guardado
celosamente incluso bajo un password dado en el momento de la
generación.

De esta manera incluso podremos tener una cuenta compartida por varios
usuarios, siempre que éstos consientan, de lo que será garante el
administrador.

## Instalación de ssh

Para usar `ssh` en un equipo remoto, al que llamaremos en adelante
"servidor", ésta utilidad debe estar instalada por su
administrador. En un equipo bajo Ubuntu se haría mediante:

	sudo apt-get install openssh-server

o forma similar como superusuario (en cuyo caso no escribiremos
`sudo`) en equipos bajo Debian. A partir de este momento tendremos
unas vastas posibilidades de acceso al servidor y de control sobre él.

## Acceso al servidor mediante certificado

En esta sección explicaremos:

1.  [la habilitación del servidor para el acceso mediante certificado](#Habilitación-del-servidor)
2.  [la generación de la pareja de certificados: el público y el privado](#Generación-de-la-Pareja-de-Certificados)
3.  [la habilitación del acceso a la cuenta mediante el certificado privado](#Habilitación-del-acceso-mediante-certificado)

### Habilitación del servidor

¿Qué ocurre si ignoramos las instrucciones de esta sección titulada
"Habilitación del servidor" y no hacemos ninguno de los cambios que se
dirán? El inicio de sesión vía `ssh` sólo funcionará para los usuarios
que tengan un campo de contraseña relleno en `/etc/shadow` o un
`authorized_key` para `ssh`. Se observará que el valor por defecto
para `PubkeyAuthentication` es `yes` y para `PermitEmptyPasswords` es
`no`, por lo que si incluso ambos son eliminados el comportamiento
será el mismo. Los usuarios que tengan un `authorized_key` para `ssh`
podrán entrar mediante la pareja de certificados sólo desde los
equipos con certificado privado cuyo correspondiente público haya sido
incluido de forma oportuna en el `authorized_key` del servidor; desde
el resto de equipos podrían entrar con la contraseña bajo la que esté
la cuenta del servidor, si existiera. Quien no tuviese un
`authorized_key` y sí contraseña, podría seguir usándola en la forma
habitual.

Los cambios que se describen a continuación deben ser efectuados
cuando se quiera impedir el acceso por contraseña, quedando como única
opción permitida el acceso por una pareja de claves pública y
privada. En tal caso no será necesaria la verificación de caducidad de
la contraseña `PAM` (Pluggable Authentication Modules), por lo que se
habría de deshabilitar este servicio como después se dirá.

La labor, de ser llevada a cabo, debe ser hecha por el administrador
del servidor. Suponiendo instalado `ssh`, existirá el fichero
`sshd_config` en el directorio `/etc/ssh/`, el cual debe contar con
los siguientes campos a los valores que se indica (si alguno no
existiese, habría de ser creado):

	RSAAuthentication yes
    PubkeyAuthentication yes
	ChallengeResponseAuthentication no
	PasswordAuthentication no
	UsePAM no

Es muy importante observar la buena costumbre de hacer una copia de
respaldo del fichero de configuración que vayamos a modificar antes de
hacer esa modificación.

	sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.20151214

En el caso de editarlo y cometer algún error, quizás lo dejemos
corrupto y podríamos tener un problema muy serio.

Hecho ese importante inciso, en el caso de nuestro equipo, un servidor
bajo `Ubuntu 14.04`, editamos el mencionado fichero con nuestro editor
preferido al efecto, `nano`, según la siguiente orden:

	sudo nano /etc/ssh/sshd_config

y cambiamos la línea:

	#PasswordAuthentication yes

por la línea:

	PasswordAuthentication no

lo cual supone descomentar tal línea y conmutar el `yes` al
`no`. Finalmente debimos cambiar la línea:

	UsePAM yes

por la línea

	UsePAM no

lo que supuso conmutar el `yes` al `no`.

Una vez hechos los cambios, los dejamos registrados y salimos (`^X`,
`y` y seguidamente `Intro`). Hecho esto es preciso relanzar el
servicio para que surta efecto. Ello será llevado a cabo con la orden:

	sudo service ssh reload
 
y habremos acabado en lo que a esta gestión respecta.

### Generación de la Pareja de Certificados

La generación de los certificados, suponiendo estar en un sistema
`Unix` como los basados en `Debian` o el caso de `Mac OS X`, se lleva
a cabo con una utilidad llamada `ssh-keygen` que encontraremos a
disposición por el simple hecho de tener instalado `ssh`. Obviamente
es una operación llevada a cabo por una CA o del lado del usuario en
el equipo desde el que desea acceder al servidor. En el caso de ser
del lado del usuario, en su equipo deberá estar creado el directorio
`/home/login_equip/.ssh` y si no estuviese creado, habríamos de
crearlo.

Ejecutaremos la siguiente orden:


	ssh-keygen -t rsa -b 4096 -C "su_email@dominio.ext"

donde la dirección electrónica se utiliza como una mera etiqueta y
podría ser eventualmente la que tuviése para comunicarse con el
administrador del servidor en el propio servidor. El diálogo será como
sigue:

	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/login_equip/.ssh/id_rsa):

Es muy recomendado por los especialistas pulsar ahora `Intro` y no
cambiar los ajustes por defecto. No obstante, si queremos distinguir
los certificados por servidores, le cambiaríamos el nombre a uno
sugerente poco explícito y, de ser explícito, en modo alguno
evadiríamos poner una clave de uso al certificado que estamos
generando (es recomendado poner la clave en cualquier caso):

	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again:

por supuesto que los renglones anteriores se presentan: uno, se
establece la contraseña (en `Unix` se escribe sin ver efecto), aparece
la segunda la línea y se repite la contraseña que debe
coincidir. Finalmente el sistema genera una huella dactilar (en inglés:
*fingerprint*):

	The key fingerprint is:
	SHA256:m+xH/L0ZLRqTFvHhpKMGoDdpNt2Apd89XXeYaUTdI1Y su_email@dominio.ext
	The key's randomart image is:
	+---[RSA 4096]----+
	|        .    .+E.|
	|       +     + =o|
	|      + .   o O =|
	|     . = + . O oo|
	|    . B S.o * +  |
	|     + + +o. = . |
	|        +.o.=.o .|
	|       . ....+.+ |
	|        ..  . o. |
	+----[SHA256]-----+

Al acabar la operación se han generado dos ficheros en `home/login_equip/.ssh`:

	id_rsa
	id_rsa.pub

El primero `id_rsa` es la clave privada, a la que no debe tener acceso
nadie salvo el usuario y que estará bajo custodia y a resguardo por la
contraseña. El fichero segundo `id_rsa.pub` será el que enviaremos al
administrador del servidor en abierto, aunque
discretamente. Eventualmente, si conservásemos aún acceso al servidor
mediante contraseña y éste nos fuera conocido, lo copiaríamos en
nuestra carpeta `.ssh` del servidor (la suponemos creada) mediante
---previa adaptación conveniente--- la orden:

	scp /home/login_equip/.ssh/id_rsa.pub login_server@nombre_servidor.dominio.ext:/home/login_server/.ssh

Insistimos en que esto presupone el conocimiento de la contraseña de
acceso a la cuenta en servidor y tenerlo, lo que queda fuera de
nuestro protocolo de seguridad.

Usualmente se elimina del equipo el certificado público una vez ha
sido utilizado y surtido efecto.

### Habilitación del acceso mediante certificado

En cualquier caso, nosotros o cuando el administrador reciba el
certificado público de nombre `id_rsa.pub` debemos o debe incluir su
contenido en el fichero `authorized_keys`. No cabe duda de que esta
operación debe ser hecha en el servidor. La orden puede ser la
siguiente:

	cat /home/login_server/.ssh/id_rsa.pub >> /home/login_server/.ssh/authorized_keys

Si queremos permitir el acceso mediante certificado para la misma
cuenta a varios usuarios, ejecutaríamos esta orden para el fichero
`id_rsa.pub` provisto por cada uno de ellos y sobre el 
`authorized_keys` de la cuenta.

El último paso es probar el acceso desde el equipo; para ello la
siguiente orden:

	ssh -i /home/login_equip/.ssh/id_rsa login_server@nombre_servidor.dominio.ext -o VisualHostKey=yes
	
y entonces el mensaje sería

	Host key fingerprint is SHA256:HVRPMg9A/9TXDQQsxbtWBPBcLxNvLUeq8xq7ugjXVYA
	+---[ECDSA 256]---+
	|         .=OX+* .|
	|         .E+oX.O+|
	|          ..+oX @|
	|         . ..=.*.|
	|        S . +o.  |
	|         . .oo   |
	|      . . ... .  |
	|       o .   +   |
	|        . oo+.   |
	+----[SHA256]-----+

	Last login: Sun Nov 29 17:13:47 2015 from 4.127.78.188.dynamic.ctelefono.com
	login_server@login_server 

### Compartir una Cuenta

Si una misma cuenta hubiera de ser compartida por varios usuarios, el
administrador puede recibir el certificado público `id_rsa.pub`
aportado por cada uno de los usuario y reproducir el proceso de la
primera vez:

	cat /home/login_server/.ssh/id_rsa.pub >> /home/login_server/.ssh/authorized_keys

La particularidad ahora es que esta labor podría hacerla desde la cuenta el primer
usuario dado de alta. 

## Para escribir este post han sido útiles los siguientes enlaces:

[Strappazzon, Nicola. Acceso por SSH mediante clave Pública y Privada en Ubuntu.](https://www.swapbytes.com/?s=ssh+mediante+clave)

[**GitHub**Help. Generating SSH keys.](https://help.github.com/articles/generating-ssh-keys/)

[LibrosWeb. 4.4. Preparando el servidor.](http://librosweb.es/libro/pro_git/capitulo_4/preparando_el_servidor.html)

[[*}super**user**. What is randomart produced by ssh-keygen?](http://superuser.com/questions/22535/what-is-randomart-produced-by-ssh-keygen)

[Geeky**Theory** Linux. COPIAR ARCHIVOS A TRAVÉS DE SSH CON SCP](https://geekytheory.com/copiar-archivos-a-traves-de-ssh-con-scp/)

[Which users are allowed to log in via SSH by default?](http://unix.stackexchange.com/questions/36804/which-users-are-allowed-to-log-in-via-ssh-by-default)

[Expired password and SSH key based login with UsePAM yes](http://unix.stackexchange.com/questions/160268/expired-password-and-ssh-key-based-login-with-usepam-yes)

Y ... esto es todo por hoy.
