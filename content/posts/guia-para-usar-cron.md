+++
title = "Guía para usar Cron"
date =  "2016-09-01"
author =  "Yábir G."
description = "Cron es una herramienta de los sistemas unix muy útil para establecer tareas rutinarias que queremos ejecutar a una hora determinada."
tags = ["linux", "cron", "tools", "herramientas", "programar"]
+++

## Introducción

`cron` es una herramienta de los sistema unix para ejecutar ordenes o scripts automáticamente en una determinada hora o fecha. Un uso frecuente podría ser el envió de emails en determinados servidores con información o back-ups de datos.

Como se marca en [este enlace de stackoverflow](http://stackoverflow.com/questions/21789148/difference-between-cron-and-crontab) existe una diferencia de términos que algunos usuarios confunden. `cron` es el servicio que nos permite realizar las tareas periódicamente, `crond` es el `demonio` que lee los archivos de configuración, que se denominan `contrab` y que indican las acciones que se van a llevar a cabo y cuando.

## Ordenes principales

Existen dos ordenes que nos van a servir para manejar `cron`:

* Para listar las tarea usaremos `crontab -l`
* Para editar las tareas usaremos `crontab -e`

Es importante señalar que si lo hacemos de esta manera las ordenes se ejecutarán como si lo hiciésemos nosotros. Si queremos que se ejecuten como usuario `root` deberemos agregar `sudo` antes de la orden.

Para incluir una tarea la agregaremos al fichero que se nos abre con la orden `crontab -e`.

## Formato de las tareas

La estructura de una tarea al cuando usamos la orden `crontab -e` es parecida a la siguiente: 

    * * * * * /directorio/del/script/script.sh

Encontramos 5 estrellas y una orden o script que ejecutar. Cada estrella representa lo siguiente:

    minuto hora dia mes dia_semana


* minuto: Controla en que minuto se ejecuta la orden indicada y acepta valores entre 0 y 59
*  hora: Controla la hora en la que se ejecuta la orden indicada. Usa el formato de 24h y acepta valores entre 0 y 23 siendo 0 medianoche.
*  dia: Es el día del mes en el que se ejecuta la orden indicada, acepta valores entre 1 y 31
*  mes: Es el mes en el que la orden indicada se ejecuta. Acepta valores entre 1 y 12.
*  dia_semana: El día de la semana en el que se ejecuta la orden indicada. Acepta valores de 0 a 6 siendo 0 el domingo y 6 el sábado. El 7 también es domingo.

Si no desea establecer un valor para un campo simplemente escriba `*` que se interpreta como `cada`.

Un ejemplo sería: 

    0 8 * * 1 echo "Esta orden se ejecuta cada lunes a las 8 de la mañana"

También podemos pasar listas usando comas como separadores. Ej:

    0 8 * * 1,2,3,4,5 echo "Esta orden se ejecuta de lunes a viernes"

O como alternativa podemos escribir 1-5 con idéntico resultado.

    0 8 * * 1-5 echo "Esta orden se ejecuta de lunes a viernes"

O por ejemplo si queremos que sea cada cinco minutos podemos escribir:

    */5 8 * * 1-5 echo "Esta orden se ejecuta de lunes a viernes cada 5 minutos"

Además existen palabras reservadas que simplifican expresiones como: `@yearly`, `@monthly`, `@weekly`, `@daily`, `@hourly` o `@reboot`. Esta última hace que se ejecute la orden al inicio. Un ejemplo sería:

    @hourly echo "Cada hora aparece este mensaje"

## ¿Hace falta reiniciar?

Esta es una pregunta que me hice la primera vez que use `cron` y como se indica [en esta respuesta](http://stackoverflow.com/questions/10193788/restarting-cron-after-changing-crontab-file) no es necesario. No obstante podemos reiniciar el servicio ejecutando en la terminal:

    sudo service cron reload

Y ... esto es todo por hoy.
