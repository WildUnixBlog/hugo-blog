+++
title = "Mi primera clase de Prolog"
date =  "2016-09-24"
author =  "M García"
description = "Prolog es un lenguaje para el paradigma de programación lógica usado en el ámbito didáctico con mero propósito educativo."
tags = ["prolog", "clase", "math", "programming"]
+++

## Introducción

El término "Prolog" (o "PROLOG"), proviene de la expresión en francés
"PROgrammation en LOGique", es un lenguaje de programación lógico e
interpretado, bastante conocido en el medio de investigación en
Inteligencia Artificial.

Prolog es un lenguaje de programación para ordenadores que se basa en
el lenguaje de la Lógica de Primer Orden y que se utiliza para
resolver problemas en los que entran en juego objetos y relaciones
entre ellos. Una de las ventajas de la programación lógica es que se
especifica qué se tiene que hacer (proagramación declarativa) y no cómo
se debe hacer (programación imperativa).

##Instalación

En Ubuntu 16.04 haremos la instalación ejecutando la orden:

	sudo apt-get install swi-prolog

o si deseamos algo más completo: 

	sudo apt-get install swi-prolog prolog-el

En nuestro Mac OS-X instalaremos SWI-Prolog a través de
[MacPorts](https://www.macports.org/install.php). Para Mac OS-X la
orden `port` de MacPorts es uno de los análogos al `apt-get` de
Ubuntu. La orden de consola sería:

	sudo port -v install swi-prolog

sin duda esto es lo mejor, aunque como mal menor siempre podemos
llevar a cabo la instalación a través del paquete mpkg provisto en la
[página oficial de descargas de SWI-Prolog](http://www.swi-prolog.org/download/stable),
donde también recomiendan y dan sugerencias para llevar a cabo la
instalación vía [MacPorts](https://www.macports.org/).

Si aún disfrutamos de Windows descargaremos el instalador según
nuestras preferencias de la
[página oficial de descargas de SWI-Prolog](http://www.swi-prolog.org/download/stable).

## Prolog y Emacs

Aún habiendo instalado el paquete `prolog-el` sigue siendo necesario,
hoy por hoy, incluir en el fichero `.emacs` lo siguiente:

	(autoload 'run-prolog "prolog" "Start a Prolog sub-process." t)
    (autoload 'prolog-mode "prolog" "Major mode for editing Prolog programs." t)
    (autoload 'mercury-mode "prolog" "Major mode for editing Mercury programs." t)
    (setq prolog-system 'swi)
    (setq auto-mode-alist (append '(("\\.pl$" . prolog-mode)
                                    ("\\.m$" . mercury-mode))
                                   auto-mode-alist))

si queremos que nuestro emacs de colorines y eso ... a nuestros
ficheros `.pl`. En cambio, si trabajamos en Mac OS-X la dificultad no
existe, pues el maravilloso
[Aquamacs](http://aquamacs.org/download.shtml) viene preparado
ya. Para ver cómo instalar Emacs en Windows, pueden visitar este
[post](http://ubuntudriver.blogspot.com.es/2011/08/instalar-emacs-en-windows.html).

El hola mundo

Ha llegado el momento de escribir el "hola mundo", ese programa que
nos da la seguridad de que hemos empezado con buen pie, que vamos por
buen camino y que sólo es cuestión de persistir. He aquí el código:

	mensaje :- nl,
               write('Ejemplo: "El hola mundo de Prolog" cargado. '),
               nl,
               nl.
	salude  :- write('¡hola ... mundo!').
            :- mensaje.

Tomaremos el texto anterior, editaremos un fichero que podría llamarse
`hola.pl`, por ejemplo, y abriremos una terminal (`Ctrl+t`) en la que
viajaremos hasta el lugar donde se halle el `hola.pl` (si no tendremos
que cargar el fichero dando el camino, que también se
puede). Seguidamente ejecutamos en la línea de la terminal la orden:

	swipl

y seguidamente, tras el diálogo de bienvenida ejecutamos

	?- ['hola.pl'].

con lo que tendremos el siguiente diálogo:

	Ejemplo: "El hola mundo de Prolog" cargado. 

	% hola.pl compiled 0.00 sec, 1,632 bytes
	true.

Ahora podemos pedir la validación del predicado salude:

	?- salude.
	¡hola ... mundo!
	true.

	?-

Hay que observar que cuando se pide una validación, dicha petición
acaba en "." y luego es cuando se pulsa `intro`. Puede llamar la
atención en nuestro particular código del `hola.pl` la línea `:-
mensaje.`. A grandes rasgos, su cometido es solicitar en tiempo de
interpretación la validación automática del predicado `mensaje` que
había sido argumentado aguas arriba de código. No le demos más
importancia por ahora, pero guardemos en la memoria el alcance de esta
técnica. Observar también que los ficheros que vamos a interpretar con
SWI-Prolog deben tener la extensión `.pl`.

##Un programa arquetípico

Habitualmente hemos recurrido en "Mi primera clase ..." a mostrar
ejecuciones aritméticas. Ahora dejamos esa vía para resaltar las muy
especiales características de este otro lenguaje. Nuestra propuesta
arquetípica es ahora un código, contenido en el fichero `razona.pl` que
muestra cómo Prolog "razona", pues Prolog incluye un "demostrador":

	p(a).                              
	p(X) :- q(X), r(X).                
	p(X) :- u(X).                      

	q(X) :- s(X).                      

	r(a).                              
	r(b).                              

	s(a).                              
	s(b).                              
	s(c).                              

	u(d).

Realmente los programas en Prolog son todos como éste, con mayor o
menor sofistificación. La estructura del esquema ordena ---y aquí el
orden importa--- una serie de reglas del tipo:

	cabeza_de_la_regla :- cuerpo_de_la_regla.

en las que puede faltar la `cabeza_de_la_regla` o el
`cuerpo_de_la_regla`, como hemos visto. El conjunto de las reglas dadas
constituye una base de datos que Prolog recorre de arriba hacia
abajo. En nuestro caso, la línea `p(a)` sirve para informar al intérprete
de que debe considerar que el predicado `p(X)` vale si `X=a`. La línea
`p(X) :- q(X), r(X).` sirve para informar al intérprete de que debe dar
por válido a `p(X)` para aquellos valores atribuibles a `X` con los cuales
resultase válido simultáneamente `q(X)` y `r(X)`. Pero veamos a nuestro
programa en acción con el siguiente diálogo:

	?- ['razona.pl'].
	% razona.pl compiled 0.00 sec, 2,776 bytes
	true.
	
	?- p(X).
	X = a.
	
	?- p(X).
	X = a.

	?- p(X).
	X = a;
	X = a;
	X = b;
	X = d.

Observe que en la primera línea sólo ha habido una respuesta porque
hemos pulsado `.` tras el primer `X = a`; ello indica al intérprete
que debe parar de buscar valores para `X` que satisfagan a `p`. Si
pulsamos en lugar de `.` el signo `;`, el intérprete busca otras
alternativas como valores de `X` que satisfagan a `p`. En nuestro caso
vuelve a encontrar que vale `a` por otra razón, encuentra como
candidatos a `b` y también a `d`. Entonces detiene la búsqueda al
constatar que no hay más candidatos. Nuestro lector puede tratar de
razonar el porqué de este comportamiento; será en segundo gran hito
que alcance ... y puede que el último pues encontrará entonces que
realmente lo que hace el intérprete es explorar un árbol unificando y
resolviendo.

## Algunas precisiones

Si queremos que el intérprete muestre su ayuda haremos:

	?- help(help).

y si queremos salir del intérprete haremos:

	?- halt.

Para cargar más de un programa en la interfax del intérprete podemos hacer lo siguiente

	?- ['hola.pl','razona.pl'].

y habremos cargado `hola.pl` y `razona.pl`. Habrá que tener cuidado de
que no haya conflictos en los contenidos de ambos ficheros. Podemos
ver lo que hemos cargado hasta el momento con:

	?- listing.

## Un reto

Les dejamos el siguiente código que puede formar parte del fichero "sumatorio.pl":

	sumatorio(1,1) :- !.
	sumatorio(N,S) :- N1 is N-1,
                      sumatorio(N1,S1),
                      S is N+S1.

Les invitamos a que  establezcan el siguiente diálogo:

	?- ['sumatorio.pl'].
	% sumatorio.pl compiled 0.00 sec, 1,200 bytes
	true.

	?- sumatorio(10,X).
	X = 55.

y que según lo observado traten de entender el programa. Podrían
editar seguidamente el fichero `sumatorio_dp.pl` con el mismo
contenido del anterior pero quitando el carácter `!` del código:

	sumatorio(1,1).
	sumatorio(N,S) :- N1 is N-1,
                      sumatorio(N1,S1),
                      S is N+S1.

Ahora pueden repetir lo hecho con `sumatorio.pl`. ¿Qué disfunción
observan? ¿A qué se debe?

## Para Aprender Más

Le sugerimos el estudio de los siguientes libros:

1. [Delahaye, J.P.; Formal Methods in Artificial Intelligence. John Wiley & Sons](https://is.gd/SgDmms)

2. [Dodd, T. PROLOG: A Logical Approach. Oxford University Press](https://eqageta.files.wordpress.com/2014/01/7khjil.pdf)

3. [Sterling, L.S; Shapiro, E. Y. The Art of Prolog. MIT Press](https://mitpress.mit.edu/books/art-prolog)

4. [William F. Clocksin, W.F.; Christopher S. Mellish, C.S. Programming in Prolog: Using the ISO Standard. Springer-Verlag](https://is.gd/s6sQPb)

y de los siguientes manuales o apuntes:

1. [prolog :- tutorial de J.R.Fisher](https://www.cpp.edu/~jrfisher/www/prolog_tutorial/contents.html)

2. [Llorens Largo, F., Castel de Haro, M. J.; Prácticas de Lógica. Prolog. Departamento de Ciencias de la Computación e Inteligencia Artificial. U. de Alicante](http://www.infor.uva.es/~teodoro/PrologAlicante.pdf)


Y ... esto es todo por hoy.
