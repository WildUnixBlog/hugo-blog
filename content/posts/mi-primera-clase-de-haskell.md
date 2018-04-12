+++
title = "Mi Primera Clase de Haskell"
date =  "2016-09-24"
author =  "M. García"
description = "Haskell es el lenguaje de programación arquetípico de la programación funcional con carácter perezoso."
tags = ["haskell", "clase", "math", "programming"]
+++

## Introducción

Hay un paradigma de programación que se denomina "programación
declarativa".  Surge en contraposición a la "programación imperativa",
que en sus comienzos estaba representada por lenguajes muy próximos a
las peculiaridades de la propia máquina: operaciones artiméticas
simples, instrucciones de acceso a la memoria, etc. Ese tipo de
programas resultan a menudo crípticos, llegando a impedir su
comprensión incluso a programadores entrenados, aunque en la
actualidad los lenguajes imperativos han atenuado mucho este efecto.

La idea de la programación declarativa es escribir programas
especificando o "declarando" un conjunto de condiciones,
proposiciones, afirmaciones, restricciones, ecuaciones o
transformaciones que describen sin ambigüedad el problema y contienen
su solución. Dicha solución es obtenida mediante mecanismos internos
de control para la inferencia de información, sin que el programador
haya especificado exactamente cómo encontrarla. A la computadora se le
indica qué es lo que se desea obtener, no cómo hacerlo. No existen
asignaciones destructivas y las variables son utilizadas con
"transparencia referencial".  En otras palabras, el resultado de
evaluar una expresión compuesta depende únicamente del resultado de
evaluar las subexpresiones que la componen y de nada más; no depende
de la historia del programa en ejecución ni del orden de evaluación de
las subexpresiones que la componen. Esta propiedad no se da en
lenguajes imperativos, donde abundan los efectos cmolaterales por
asignaciones destructivas. Veamos a Python, que no tiene la
transparencia referencial, en un ejercicio de violación de la
misma. Para ello consideremos el retazo de código siguiente que
podríamos dar como contenido al fichero `transparencia.py`:

	counter = 0

	def incremental(n):
		global counter
		i = 0
		while i <= n:
			counter += 1
			i += 1
    print counter

	incremental(5)
	incremental(5)

para entender el efecto, podemos interpretarlo y ver que ofrece el siguiente diálogo:

	iMac-de-miUsuario:~ my_login$ python transparencia.py
	6
	12
	iMac-de-miUsuario:~ my_login$

Es decir, dos invocaciones consecutivas de la misma función ---pero en
ambientes diferentes--- han dado respuestas diferentes. Y es que las
funciones en los lenguajes no funcionales, como es el caso de Python,
no tienen por qué comportarse como funciones en el sentido matemático;
son más bien algo así como "subrutinas".  Lo que un lenguaje funcional
reclama, y consigue por serlo, es la recuperación de concepto
"función" con el sentido dado por la matemática ... y la computación.

En estas páginas hemos hablado ya de un subparadigma del paradigma de
programación declarativa cuando presentamos el post de
[Prolog](https://wildunix.es/posts/mi-primera-clase-de-prolog/):
la
[programación lógica](http://es.wikipedia.org/wiki/Programaci%C3%B3n_l%C3%B3gica). Tal
paradigma está fundamentado por la
[Lógica de Primer Orden](http://es.wikipedia.org/wiki/L%C3%B3gica_de_primer_orden).

Sin embargo, los comienzos del siglo XX vieron como era alumbrada otra
lógica, la llamada
[Lógica Combinatoria](http://es.wikipedia.org/wiki/L%C3%B3gica_combinatoria). Basándose
en ella, o en una explicación suya denominada
[Lambda Calculus](https://es.wikipedia.org/wiki/C%C3%A1lculo_lambda)
(desarrollado en la década de los 30 del siglo XX para investigar la
noción de función, la aplicación de funciones y la recursión), surgió
el segundo gran subparadigma de la programación declarativa: la
[programación funcional](http://es.wikipedia.org/wiki/Programaci%C3%B3n_funcional). Un
lenguaje funcional es, a la postre, el resultado de una reelaboración
del lambda calculus puro.

La programación funcional se basa en la utilización de funciones
aritméticas que no manejan datos mutables o de estado. Hace uso de la
aplicación de funciones en contraposición con el el paradigma de
programación imperativa, que hace énfasis en los cambios de estado.
La diferencia entre una función matemática y la noción de función
utilizada en la programación imperativa es que las funciones
imperativas pueden tener efectos secundarios, al cambiar el valor de
cálculos realizados previamente, carecen de transparencia referencial:
la misma expresión sintáctica puede resultar en valores diferentes en
diferentes momentos dependiendo del estado del programa en
ejecución. Con código funcional, en contraste, el valor generado por
una función depende exclusivamente de los argumentos alimentados a la
función. Al eliminar los efectos secundarios se pude entender y
predecir el comportamiento de un programa mucho más fácilmente, y esta
es una de las principales motivaciones para utilizar la programación
funcional.

El tercer gran subparadigma de la programación declarativa es la
programación algebraica, representada por lenguajes como `Maude` y
`SQL`.

En la actualidad, el lenguaje que encarna por antonomasia los cánones
de la programación funcional es
[Haskell](http://es.wikipedia.org/wiki/Haskell), un lenguaje de
programación: estandarizado, multipropósito, puramente funcional,
perezoso, con semánticas no estrictas y con fuerte tipificación
estática. Su nombre se debe al lógico estadounidense Haskell B. Curry
por la utilidad que tuvieron al respecto sus observaciones y las de su
escuela.

Haskell nació cuando en 1980 se constituyó un comité cuyo objetivo era
crear un lenguaje funcional que reuniese las características de los
múltiples lenguajes funcionales de la época, el más notable de los
cuales era Miranda, y resolviera la confusión creada por la
proliferación de los mismos.

Las características más interesantes de
[Haskell](https://wiki.haskell.org/Haskell) incluyen el soporte para
tipos de datos y funciones recursivas, listas, tuplas, guardas y calce
de patrones. La combinación de las mismas puede llevar en el caso de
algunas funciones a una implementación casi trivial, y sin embargo ser
su correspondiente versión en lenguajes imperativos extremadamente
tediosas de programar.

## Instalación

Al instalar Haskell en nuestro ordenador, conviene instalar la
plataforma completa. En Ubuntu, cuando ésta está disponible, la
instalación es trivial:

	sudo apt-get install haskell-platform

y si somos felices usuarios de Mac OS X con
[macport](https://www.macports.org/install.php) instalado (depende de
[Xcode](https://itunes.apple.com/es/app/xcode/id497799835?mt=12) y
posterior instalación de `Command Line Tools`), la instalación de la
plataforma Haskell la efectuaremos con la orden de consola:

	sudo port install haskell-platform

Al instalar la plataforma tendremos instalado `Cabal`. `Cabal` es el
sistema de paquetes estándar para el software de Haskell. Ayuda a
configurar, compilar e instalar el software de Haskell y distribuirlo
fácilmente a otros usuarios y desarrolladores. Ayuda con la
instalación de paquetes existentes y también ayuda a los
desarrolladores con sus propios paquetes. Se puede utilizar para
trabajar con paquetes locales o para instalar paquetes desde archivos
de paquetes en línea, incluyendo las dependencias que instala de forma
automática. Por defecto está configurado para utilizar Hackage que es
archivo del paquete central de Haskell y que contiene miles de
bibliotecas y aplicaciones en el formato de paquete de Cabal.

Un ejemplo de utilización de Cabal para instalar paquetes de un
archivo remoto de paquetes contenido en Hackage y que se llama xmonad
sería utilizando la orden en línea llamada cabal:

	cabal install xmonad

lo cual instalará el paquete `xmonad` además de todas sus dependencias.

## En cuanto al editor

Siempre decimos que nuestros lectores pueden usar su editor preferido,
sin embargo en este caso hay algunas restricciones. Nunca nunca
usaremos editores que al tabular introduzcan caracteres no
imprimibles. La razón es que Haskell es un lenguaje que ha suprimido
los terminadores de línea, lo que ha conducido a usar indentación en
el código. Pues bien, la indentación no es real si el editor incluye
caracteres no imprimibles. Hemos visto a compañeros y profesores
volverse locos tratando de entender por qué no funciona un código que
a todas luces era perfecto; y no funcionaba porque la indentación
estaba rota por culpa de "fantasmas no visibles" en el
renglón. Recomendación: Sublime Text o Emacs ... o en el otro orden
mejor.

### Haskell y Emacs

Los archivos de los proyectos de Haskell son ficheros .hs. Para que
Emacs pueda manejar fácilmente dichos ficheros, instalaremos en
nuestro Ubuntu el paquete haskell-mode

	sudo apt-get install haskell-mode

### Haskell y Sublime Text

Instalaremos Sublime Text mediante:

	sudo add-apt-repository ppa:webupd8team/sublime-text-3 
	sudo apt-get update 
	sudo apt-get install sublime-text-installer

Sublime Text está totalmente preparado para usar Haskell, salvo por un
detalle: los caracteres no imprimibles introducidos por el
tabulador. Para poder usar dicha tecla con toda comodidad y que no se
incluyan estos indeseables caracteres debemos ir a

	Preferences > Settings - User 

y agregar dentro de las llaves

	"translate_tabs_to_spaces": true, 

## El "hola mundo"

Haskell tiene un intérprete y un compilador. Saludemos al mundo
primero desde el intérprete. Para ello invocaremos al intérprete desde
la línea de comandos con la orden:

	ghci

y el resultado será un diálogo del tipo:

	Last login: Sat Sep 27 17:59:09 on console
	iMac-de-miUsuario:~ my_login$ ghci
	GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
	Loading package ghc-prim ... linking ... done.
	Loading package integer-gmp ... linking ... done.
	Loading package base ... linking ... done.
	Prelude>  

Hay que saber que `Prelude` es el ambiente general mínimo de librerías
y funciones cargadas al abrir el intérprete, ambiente que se puede
completar a deseo del usuario.  Ahora escribiremos:

	Prelude> putStrLn "Hola ... mundo!"

y al pulsar intro obtenemos el diálogo

	Hola ... mundo!

	Prelude>

Para salir del intérprete escribiremos

	Prelude> :q

y pulsando intro saldremos a la línea de órdenes de la consola.

Si lo que queremos hacer es un ejecutable que nos permita difundir el
saludo al mundo entre nuestros amigos, que puede que no tengan Haskell
instalado, utilizaremos el compilador. Para ello usaremos nuestro
editor preferido y con él abriremos un fichero que llamaremos hola.hs,
por ejemplo. El contenido del fichero será exclusivamente la línea:

	main = putStrLn "Hola ... mundo!"

Cerramos el fichero y desde la línea de órdenes de la consola
ejecutamos la orden:

	ghc -o hola hola.hs

por supuesto que hola es el nombre que le deseamos dar al ejecutable;
además del mismo, se han creado otros dos: el `hola.hi` y el
`hola.o`. Estos dos ficheros son
[auxiliares y necesarios](http://www.haskell.org/ghc/docs/2.10/users_guide/user_174.html)
para la compilación. Para ejecutar nuestro ejecutable, escribiremos en
la línea de órdenes (suponemos que todo se ha llevado a cabo donde
está ubicado el fichero `hola.hs`):

	./hola

y al pulsar intro tendremos el cortés mensaje:

	Hola ... mundo!

## Ejemplos

Para ejemplificar el uso de Haskell podemos recurrir a funciones
conocidas ya en los lenguaje imperativos y que produzcan fuerte
contraste. Con nuestro editor preferido abriremos un fichero
`example.hs` y le pondremos el siguiente contenido, que irá creciendo:

	qsort [] = []
	qsort (x:xs) = qsort ([y | y <- xs, y < x]) ++ [x] ++ qsort ([y | y <- xs, y >= x])

o si se prefiere, el código más elaborado y eficaz:

	qsort :: (Ord a) => [a] -> [a]
	qsort [] = []
	qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++ qsort (filter (>= x) xs)

Ahora, si estamos ubicados con la consola en el directorio que
contiene al fichero `example.hs`, tenemos dos opciones: abrir el
intérprete y cargar el fichero `example.hs` o abrir el intérprete para
él. La primera opción se materializa ejecutando:

	ghci

pulsando intro y cuando aparezca el diálogo:

	Last login: Sat Sep 27 17:59:09 on console
	iMac-de-miUsuario:~ my_login$ ghci
	GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
	Loading package ghc-prim ... linking ... done.
	Loading package integer-gmp ... linking ... done.
	Loading package base ... linking ... done.
	Prelude> 

entonces escribiremos:

	Prelude> :l example.hs

y al pulsar intro obtendremos el diálogo:

	[1 of 1] Compiling Main             ( example.hs, interpreted )
	Ok, modules loaded: Main.
	*Main> 

El retazo `Ok, modules loaded: Main.` indica que el fichero
`example.hs` ha sido interpretado correctamente y el retazo de código
`*Main> ` indica que el ambiente `Prelude` ha sido completado con las
funciones, tipos de datos, clases, etc. recién interpretadas en
`example.hs`.

Ahora seremos capaces de ordenar una lista con nuestra función:

	*Main> qsort ([2,-1,5,3])
	[-1,2,3,5]
	*Main>

¿No parece magnífico? Sin embargo no todo lo bello es apropiado. Lo
mejor para ordenar es utilizar la función primitiva `sort`. Pero aquí
surge el primer problema:

	*Main> sort([2,3,1])

	<interactive>:5:1:
		Not in scope: `sort'
		Perhaps you meant one of these:
	`sqrt' (imported from Prelude), `qsort' (line 1)
	*Main>

Este mensaje significa, ni más ni menos, que la función sort no está
en Prelude, ni por supuesto en example.hs. Para incluirla debemos
cargar un módulo instalado por defecto en el árbol de Haskell que la
contenga:

	*Main> :m + Data.List
	Prelude Data.List >


y ahora podemos usar la función `sort` como sugeríamos antes. Pero
obsérvese que hemos perdido nuestro contenido del fichero
`example.hs`; ahora deberíamos recargarlo.

A veces es útil tener información sobre funciones. Podemos hacer, por
ejemplo:

	*Main Data.List> :t qsort
	qsort :: Ord a => [a] -> [a]
	*Main Data.List> :i qsort
	qsort :: Ord a => [a] -> [a] -- Defined at example.hs:2:1
	*Main Data.List> :i sort
	sort :: Ord a => [a] -> [a] -- Defined in `Data.List'
	*Main Data.List> 

Ahora sabemos que la línea

	qsort :: Ord a => [a] -> [a]

define el tipo de la función `qsort`; debemos leerla así: si `a` es un
objeto incluido en la clase `Ord` de objetos, `qsort` asigna a una
lista de objetos `a` otra lista de objetos `a`.

Para importar varios módulos se puede proceder así:

    iMac-de-miUsuario:~ my_login$ ghci 
    GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
    Loading package ghc-prim ... linking ... done.
    Loading package integer-gmp ... linking ... done.
    Loading package base ... linking ... done.
    Prelude> :m + Data.List Data.Char
    Prelude Data.List Data.Char>
	Prelude Data.Char List> :m - Data.Char
	Prelude Data.List>

o también así:

    iMac-de-miUsuario:~ my_login$ ghci 
    GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
    Loading package ghc-prim ... linking ... done.
    Loading package integer-gmp ... linking ... done.
    Loading package base ... linking ... done.
    Prelude> import Data.Char
    Prelude Data.Char> import Data.List as List
	Prelude Data.Char List> :m - Data.List
	Prelude Data.Char>

Para saber más de las órdenes básicas del intérprete podemos hacer:

	Prelude> :h

Si queremos introducir nuestras funciones en el intérprete sin
depender de un fichero del tipo example.hs, podemos. Habríamos de
proceder así:

	*Prelude> let fac n = if n == 0 then 1 else n * fac (n-1)
	*Prelude> fac 25
	15511210043330985984000000
	*Prelude>

aunque lo mejor sería incluir en example.hs el código:

	fac :: Integer -> Integer
	fac 0 = 1
	fac n = n * fac (n-1) 

y recargar `example.hs`:

	*Main Data.List> :l examples.hs 
	[1 of 1] Compiling Main             ( examples.hs, interpreted )
	Ok, modules loaded: Main.
	*Main Data.List> fac 25
	15511210043330985984000000
	*Main Data.List> 

Si queremos una impresión de la estadística de tiempo/memoria tras
cada evaluación, podemos ejecutar:

	Prelude> :set +s

desharemos lo hecho con la orden:

	Prelude> :unset +s

Recordemos que para salir del intérprete ejecutaremos:

	Prelude> :q

Para comentar una línea la hacemos preceder de la secuencia de
caractéres "--" y para comentar un párrafo de código lo ponemos entre
"{-" y "-}":

	-- esta línea está comentada

	{- este párrafo está comentado
		por ser sólo una explicación -}

## Un ejemplo más elaborado

De [D. José E. Labra G.](http://www.x.edu.uy/inet/IntHaskell98.pdf)
hemos tomado el contenido ligeramente modificado de los ficheros
[Pila.hs](https://dl.dropboxusercontent.com/u/46506049/Pila.hs) y
[Menu.hs](https://dl.dropboxusercontent.com/u/46506049/Menu.hs). El
segundo es un ejemplo de menú que importa y opera con los objetos del
primero.

Si queremos hacer un ejecutable con Menu.hs podemos operar de dos
maneras. Suponemos que en la consola nos hemos colocado donde están
los dos ficheros en cuestión. La primera forma sería:

	ghc --make Menu.hs

y ahora podríamos ejecutar el ejecutable resultado con 

	./Menu

La segunda forma sería ejecutar consecutivamente las siguientes dos
órdenes en la consola:

	ghc -c -O Pila.hs Menu.hs
	ghc -o menu -O Pila.o Menu.o

y ahora podríamos ejecutar el ejecutable menu con 

	./Menu

La ventaja de esta última forma de proceder es que con la orden:

	ghc -c -O Pila.hs Menu.hs

creamos los ficheros `Pila.o` y `Menu.o` que nos valdrían para
pasarlos a otra parte del equipo que deba elaborar el fichero
ejecutable `Menu`, pero que sin embargo no deba, o no pueda, conocer
los ficheros `Menu.hs` y/o `Pila.hs`. Una vez hayan sido pasados los
mencionados ficheros `Pila.o` y `Menu.o`, los receptores compondrían
el fichero ejecutable menu mediante la orden de consola:

	ghc -o menu -O Pila.o Menu.o

## Ejecutar sin compilar y sin interpretar

Supongamos que tenemos un fichero llamado, por ejemplo,
`fmapping_io.hs` y con el siguiente contenido:

	import Data.Char 
	import Data.List 

	main = do line <- fmap (intersperse '-' . reverse . map toUpper) getLine 
	          putStrLn line

Podríamos ejecutarlo como sigue:

	iMac-de-miUsuario:~ my_login$ runhaskell fmapping_io.hs
	esto es una prueba

	A-B-E-U-R-P- -A-N-U- -S-E- -O-T-S-E

## La principal arma de Haskell

Como explican los autores en su libro
[Razonando con Haskell](http://www.lcc.uma.es/~pepeg/pfHaskell/index.html),
en muchos lenguajes imperativos (p. e. `C++`) con el mecanismo de paso
por valor (*call by value*) siempre se evalúan los argumentos antes de
la llamada. Una alternativa a este mecanismo es el paso por necesidad
(*call by need*), en el cual se evalúa solamente los argumentos
necesarios; el problema de tal mecanismo para los lenguajes
imperativos es que su implementación es compleja.

En el contexto de los lenguajes funcionales se habla de dos modos de
evaluación: impaciente (eager) y perezosa (lazy); en el primer caso el
evaluador hace todo lo que puede mientras que en el segundo hace
solamente lo preciso. El mecanismo impaciente corresponde al paso por
valor mientras que el perezoso al paso por necesidad.

Le proponemos que tomen de nuevo su fichero `example.hs` y que
incluyan en él el siguiente código:

	desde :: Integer -> [Integer]
	desde n = [n..]

	suma :: Integer -> [Integer] -> Integer
	suma 0 _ = 0
	suma n (x:xs) = x + suma (n-1) xs

La función desde, dado un valor entero n, entrega la lista de todos
los números enteros superiores o iguales a n, ordenados en el orden de
los números enteros según se avanza hacia el final de la lista. Así,
`[0..]` es la lista en orden creciente de todos los números enteros
comenzando en `0`: la lista de los números naturales en su propio
orden.

La función `suma` tiene dos argumento: un número entero (que pensamos
será siempre un número natural), como primer argumento, y una lista de
números enteros, como segundo argumento. Su labor es sumar las `n`
primeras entradas de la lista argumento, y si ese `n` fuese `0`,
ofrecer `0` como resultado; esto último es la condición de
detención. Si se piensa bien, todo esto se puede entender como un
bucle `while` de los lenguajes imperativos.

Pero qué pasa si nuestra malicia natural nos lleva, por afán de
diversión, a hacer una jugarreta al intérprete de Haskell, pasándole
como segundo argumento una lista infinita. ¿Qué espera el lector?
¿será éste el final de Haskell, de nuestra máquina y del tiempo
relativista? Nada más lejos de la realidad y valga para ello el
siguiente ejemplo:

	ghci example.hs

y recibimos el diálogo siguiente, señal de que ¡todo ha ido bien por
ahora!

	GHCi, version 7.6.3: http://www.haskell.org/ghc/  :? for help
	Loading package ghc-prim ... linking ... done.
	Loading package integer-gmp ... linking ... done.
	Loading package base ... linking ... done.
	[1 of 1] Compiling Main             ( example.hs, interpreted )
	Ok, modules loaded: Main.

Como el coyote, probemos que la trampa está hecha y funciona ... y
atentos a ejecutar `Ctrl + C` inmediatamente después de `desde 0` o
... esto no acabará realmente.

	*Main> desde 0
	[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,
	30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,
	57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,
	8^CInterrupted.

Seguidamente hagamos este inocente cálculo:

	*Main> 1 + 2 + 3 + 4 + 5
	15

Finalmente pedirle al correcaminos que caiga en la trampa del precipicio infinito:

	*Main> suma 5 (desde 1)
	15
	(0.00 secs, 1030416 bytes)
	*Main>
	
Nos costará, si no conocemos de antes a Haskell, sobreponernos a la impresión: hemos pedidoque sume los primeros 5 números de una ¡lista infinita! y ¡lo ha hecho en 0.00 segundos! ¿Dónde está el truco? Ni más ni menos en que Haskell evalúa perezosamente y al recibir la orden:

	*Main> suma 5 (desde 1)

no ha confeccionado primero `desde 1`, tomado luego sus primeras 5 entrada y sumado finalmente para ofrecer el resultado `15`. En su lugar lo que hizo es calcular la primera entradade `desde 1`, separarla y mandar que le sea sumada la segunda entrada de `desde 1`, la cualpasa a calcular sabiendo que queda una selección menos que hacer, etc. El proceso continúa hasta que sabemos que no queda selección de entrada alguna que hacer ya en `desde 1` y entonces mandamos sumar `0` y damos la orden de parar de sumar, para seguidamente efectuar definitivamente la suma hilvanada durante todo este tiempo.

La habilidad de Haskell: ¿para qué calcular `desde 1` y luego sumar sus 5 primeras entradas, si para nuestro propósito sólo son imprescindibles esas 5 entradas? ¡cuando sea necesario calcular/extraer más entradas de `desde 1`, ya lo haremos! ¡No nos precipitemos haciendo más trabajo del necesario! ¡Hagamos el sucintamente imprescindible no sea que nos cansemos ... eh!

Con el código anterior hay un pequeño problema, que realmente sólo es aparente:

	*Main> suma 5 [1,2,3,4]
	*** Exception: example.hs:(23,1)-(24,33): Non-exhaustive patterns in function suma

	*Main>

Como vemos se genera una excepción, que podría ser capturada
convenientemente. Un código alternativo para la función suma podría
ser:

	suma :: Integer -> [Integer] -> Integer
	suma 0 _         = 0
	suma _ []        = error "lista demasiado corta"
	suma n (x:xs) = x + suma (n-1) xs

que ahora proporciona el siguiente diálogo:

	*Main> suma 5 [1,2,3,4]
	*** Exception: lista demasiado corta

	*Main> suma 5 [1,2,3,4,5]
	15
	*Main> suma 5 [1,2,3,4,5,6]
	15
	*Main> suma 5 (desde 1)
	15
	*Main>

De nada serviría la evaluación perezosa con un código como éste:

	desde :: Integer -> [Integer]
	desde n = [n..]

	longitud :: [a] -> Integer
	longitud [] = 0
	longitud (x:xs) = 1 + longitud xs

	sumaB :: Integer -> [Integer] -> Integer
	sumaB n lst@(x:xs)
	 | n == 0    = 0
	 | n > len   = error "lista demasiado corta."
     | otherwise = x + sumaB (n-1) xs
                 where len = longitud lst

cuando `lst` es infinita:

	*Main> suma 5 (desde 1)

sencillamente no funciona, pues la condición de la ejecución exige
hacer el cálculo de una longitud, el cual no acaba.

Otro gran recurso en la programación funcional con Haskell es la
utilización en las funciones de "argumento con patrón". En la línea
anterior:

	suma n (x:xs) = x + suma (n-1) xs

el segundo argumento de la función `suma`, a saber `(x:xs)`, es
aportado con un patrón, lo que permite un uso suyo eficiente dentro de
la definición del caso: `x + suma (n-1) xs`

Así es Haskell. 

## Nuestro reto habitual

Esta vez consiste en pensar qué significa el siguiente código Haskell,
que podríamos poner en nuestro fichero Criba.hs, cargar en el
intérprete y ejecutar:

	module Criba (module Criba) where

	criba :: [Integer] -> [Integer]
	criba (p:xs) = [x | x <- xs, p `noDivideA` x]
                          where m `noDivideA` n = mod n m /= 0

	primos :: [Integer]
	primos = map head (iterate criba [2..])

	primosEq :: [Integer]
	primosEq = map head  lprimos 
               where lprimos = [2..] : map criba lprimos 

sin ánimo de producir vértigo.

No nos emocionemos demasiado, pues aunque es muy elegante no es
eficiente ni la criba real; es solamente un código de gran valor
pedagógico.

## Enlaces de interés para saber más por sí mismo

Hemos dado la primera clase. Ahora:

- The Glorious Glasgow Haskell Compilation System [User's Guide](https://downloads.haskell.org/~ghc/7.8.2/docs/html/users_guide/), Version 7.8.2
- Una [introducción a Haskell](http://www.haskell.org/haskellwiki/Es/Introduccion) telegráfica y en español.
- Podemos [echar 10 minutos más](http://www.haskell.org/haskellwiki/Learn_Haskell_in_10_minutes) bien empleados y aprenderlo casi todo.
- Información técnica [sobre los módulos](http://hackage.haskell.org/package/base-4.7.0.1/docs/). Una página [similar](http://zvon.org/other/haskell/Outputglobal/index.html), más antigua pero de mucho interés.
- [Wikihaskell](http://osl2.uca.es/wikihaskell/index.php/P%C3%A1gina_principal) de la UCA.
- Una guía de usuario para [Cabal](http://www.haskell.org/cabal/users-guide/) y la guía de Cabal de [Wikihaskell](http://osl2.uca.es/wikihaskell/index.php/Biblioteca_de_empaquetamiento_Cabal).
- [Learn You a Haskell for Great Good](http://learnyouahaskell.com/) de Miran Lipovaca.
- [Aprende Haskell](http://aprendehaskell.es/), por el bien de todos.
- El [mundo real](http://book.realworldhaskell.org/read/) de Haskell.
- [Razonando con Haskell](http://www.lcc.uma.es/~pepeg/pfHaskell/index.html). Un curso sobre programación funcional.
- Una [introducción amable](http://www.haskell.org/tutorial/) a Haskell.

Y ... esto es todo por hoy.
