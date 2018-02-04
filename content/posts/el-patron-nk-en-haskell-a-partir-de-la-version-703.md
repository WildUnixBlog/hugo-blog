+++
title = "El patron (n+k) en Haskell a partir de la versión 7.0.3"
date =  "2016-11-27"
author =  "M. García"
description = "Con las versiones de Haskell provistas en los Ubuntu anteriores a Ubuntu 11.10, los programadores de Haskell han venido usando un truco permitido, aunque no muy ortodoxo, que era incluir el patrón (n+k) en las definiciones de funciones, a la izquierda del igual definitorio."
tags = ["haskell", "clase", "math", "programming"]
+++

## Introducción

Con las versiones de Haskell provistas en los Ubuntu anteriores a Ubuntu 11.10, los programadores de Haskell han venido usando un truco permitido, aunque no muy ortodoxo, que era incluir el patrón (n+k) en las definiciones de funciones, a la izquierda del igual definitorio. Por ejemplo, la implementación del factorial era entendida por muchos así:

	factorial :: Integer -> Integer
	factorial 0 = 1
	factorial (n+1)  =  (n+1) * factorial n

en lugar de:

	fact :: Integer -> Integer
	fact 0 = 1
	fact n  =  n * fact (n-1)

El lector se preguntará intrigado por la diferencia entre `factorial` y `fact`. Es muy sutil, tanto que ni algunos desarrolladores la captan ... y todo se debe a un comportamiento *sui géneris* del intérprete/compilador. Resulta que al intentar calcular `factorial (-1)` el diálogo que se producía antes, por ejemplo bajo GHCi version 6.10.4, era:

	*Main> factorial (-1)
	*** Exception: factorial.hs:(2,0)-(3,36): Non-exhaustive patterns in function factorial


	*Main> fact (-1)
	*** Exception: stack overflow

##Explicación

Con `fact` ocurre lo siguiente:

	fact (-1)  = (-1) * fact (-2)
               = (-1) * (-2) * fact (-3)
               = etc ... por siempre

Con `factorial` ocurría algo distinto. Al intentar calcular `factorial (-1)` el intérprete busca si `(-1)` unifica con el argumento de la primera línea, y esto no ocurre porque `-1` es distinto de `0`; así pués intenta ver si `(-1)` unifica con `(n+1)` para algún valor de `n`, pero en las antiguas versiones `(n+k)`, con `k` natural fijo pero arbitrario, únicamente unificaba con valores enteros superiores o iguales a `k`. Por tanto, `(-1)` no unificaba con el patrón de ninguna de las líneas definitorias de `factorial` y, siendo `-1` una instancia del tipo `Integer`, la definición de factorial debería estar incompleta, de ahí el mensaje "Non-exhaustive patterns in function factorial".

Total, lo que hacía el programador era optimizar el esfuerzo al escribir código y conjurar el peligro de caer en un bucle infinito, promoviendo una definición incompleta cuya excepción siempre se podría capturar.

Los desarrolladores, creemos que con buen criterio, han impedido estas estratégias y han impedido definiciones como la de factorial en beneficio de definiciones como las de `fact`. De hecho, si con GHCi, version 7.0.3 intentamos interpretar un fichero que contuviera a `factorial`, se produciría un error como el siguiente:

	$ ghci factorial.hs
	GHCi, version 7.0.3: http://www.haskell.org/ghc/  :? for help
	Loading package ghc-prim ... linking ... done.
	Loading package integer-gmp ... linking ... done.
	Loading package base ... linking ... done.
	[1 of 1] Compiling Main             ( factorial.hs, interpreted )

	factorial.hs:3:12: Parse error in pattern: n + 1
	Failed, modules loaded: none.
	Prelude> 

donde se dice explícitamente que no admite el patrón `(n+1)`.

## Problema

Creemos que lo adecuado y aconsejable es programar sin el patrón `(n+k)` dando definiciones exhaustiva, con mensajes de error cuando sea conveniente; pero ... ¿qué hacemos con lo ya programado?

Para programas antiguos concebidos con esas estratégias, programas que no estamos dispuestos a modificar pues podría suponer un gran esfuerzo: ¿qué solución hay, si la hay?

## Solución

Pues sí la hay y la hemos encontrado. Mejor dicho, conocemos dos soluciones:

1 La primera sería incluir en la cabecera del fichero el siguiente texto:

	-- posible comentario
	{-# LANGUAGE NPlusKPatterns #-}
	module Main where

con lo que quedaría

	-- posible comentario
	{-# LANGUAGE NPlusKPatterns #-}
	module Main where


	factorial :: Integer -> Integer
	factorial 0 = 1
	factorial (n+1)  =  (n+1) * factorial n


que sería admitido por el nuevo ghci sin error. No es necesario poner aquí `Main` en `module Main where`, podríamos haber puesto otro nombre, pero dejo este para sugerir que podríamos estar ante el módulo `main` de un proyecto, que podría ser compilado también con `ghc`.

2 Al compilar el fichero `factorial.hs` que contiene, o llama, a la función factorial, haríamos lo siguiente:

	ghci -XNPlusKPatterns factorial.hs

Creemos que también funciona

	ghc  -XNPlusKPatterns .... factorial.hs

En realidad, cualquiera de las dos soluciones lo que hace es forzar al intérprete/compilador de Haskell a volver al pasado, volver a los esquemas que sí admitían el patrón definitorio `(n+k)`.

Y ... esto es todo por hoy.
