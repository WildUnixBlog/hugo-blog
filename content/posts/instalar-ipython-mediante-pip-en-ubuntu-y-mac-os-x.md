+++
title = "Instalar IPython mediante pip en Ubuntu y Mac OS X"
date =  "2016-03-31"
author =  "M. Garcia"
description = "La instalación de las librerías de IPython mediante pip requiere otras en el sistema. Resolvemos esa situación."
tags = ["IPython", "python"]
+++

## Introducción

Una vez que hemos decido no hacer frente a los gastos del uso legal de
Mathematica y hemos desechado la ideas usar Maxima por las razones ya
bien conocidas, una buena apuesta es llamar a la puerta de Python y
servirse de las herramientas que se han derivado de él en la última
década; nos referimos a sagemath e IPython.

A la instalación de sagemath le hemos dedicado un instructivo post en
Ubuntu Driver. A IPython le dedicaremos alguno pronto, pero hay una
dificultad en su instalación que urge solventar.

No conocemos mejor directriz de instalación que la dada en la página
oficial donde en el apartado "I already have Python", dentro de la
casuística de instalación sugiere el uso de pip (esta herramienta ha
sido explicada en nuestro post titulado "Mi Primera Clase de
Python"). Por supuesto que casi la totalidad del común de los usuario
estará en esta situación, a saber, la de "yo ya tengo Python".

En este post hemos usado Python 3, tanto para Ubuntu como para Mac OS
X. La razón es que presenta algunas facilidades para la instalación y,
a fin de cuentas, representa el futuro.

## IPython en Ubuntu mediante pip

Al ejecutar la orden de instalación recomendada en la página oficial,
concretamente la orden

	sudo pip install "ipython[notebook]"

fracasa la instalación de la librería pyzmq con el siguiente mensaje:

     error: command 'x86_64-linux-gnu-gcc' failed with exit status 1
     ----------------------------------------
     Command "/usr/bin/python3 -c "import setuptools, tokenize;__file__='/tmp/pip-build-23a2_por/pyzmq/setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" fetch_libzmq install --record /tmp/pip-wmhla3sg-record/install-record.txt --single-version-externally-managed --compile --zmq=bundled" failed with error code 1 in /tmp/pip-build-23a2_por/pyzmq


Una vez analizado se ve que la razón de esta dificultad es que faltan
librerías de desarrollo de Python de las provistas en los paquetes
alojados en los repositorios de Ubuntu.

Es tan simple como instalar, si no estuviesen ya instalados, los
paquetes que se sugieren a continuación:

	sudo apt install python-dev
	sudo apt install python3-dev

y posteriormente instalar `g++`:

	sudo apt install g++

Es posible que nuestro sistema no cuente aún con la herramienta
`pip`. Para disponer de ella haremos lo siguiente:

	sudo easy_install3 pip

Y con esto podremos ya llevar a cabo la instalación sugerida según la
orden:

	sudo pip install "ipython[notebook]"

Por supuesto que nada de esto sería preciso si tan solo hubiéramos
optado por la instalación básica de IPython:

	sudo pip install ipython

Seguidamente, si queremos instalar la librería de Python llamada
`matplotlib` necesitaremos el paquete `libfreetype6-dev`:

	sudo apt install libfreetype6-dev

En algunas distribuciones, por ejemplo Ubuntu Server 16.04, no
disponemos del paquete `pkg-config` en la instalación básica, por lo
que habría que instalarlo en este momento

	sudo apt install pkg-config python3-tk

con lo que ya podremos hacer la instalación de `matplotlib` con `pip`:

	sudo pip install matplotlib

Si nos faltase la librería `matplotlib`, al intentar instalarla con `pip`
tendríamos al final un mensaje de error que acaba en:

	TypeError: unorderable types: str() < int()

	----------------------------------------
	Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-6fus7qwz/matplotlib

Convendría, a nuestro entender, instalar `sympy` por lo que ejecutaremos:

	sudo pip install sympy

Otra librería conveniente es `scipy` . Para ellas necesitaremos los
paquetes instalados ejecutando:

	sudo apt install libblas-dev liblapack-dev
	sudo apt install python-dev gfortran

y ya con esto instalamos `scipy`:

	sudo pip install scipy

Si queremos instalar `pygraphviz` necesitamos la librería `libgraphviz-dev`:

	sudo apt install libgraphviz-dev graphviz python3-dev

tras lo cual ejecutaremos:

	sudo pip install pygraphviz
	sudo pip install graphviz

También debemos instalar `networkx` y `nxpd`:

	sudo pip install networkx
	sudo pip install nxpd

También podemos instalar `simpy`, `gmpy` y `pytest`:

	sudo pip install simpy
	sudo pip install gmpy
	sudo pip install pytest

## IPython en Mac OS X mediante pip

En esta parte final del post seguimos instalando para `Python 3` bajo la
hipótesis de que tenemos en nuestro ordenador: Mac OS X "El Capitan" y
que ya contamos con `brew` instalado en el sistema. Para Yosemite
debería funcionar todo, pero no está probado.

Es posible que en su sistema alguna de las siguientes órdenes deban
ser dadas como superusuario, es decir, anteponiendo sudo, téngalo en
cuenta.

Lo más probable es que no tengamos `Pyhton 3` en el sistema, por lo que
habríamos de procecer a instalarlo. Para ello ejecutaremos desde la
terminal:

	brew install python3

Si lo tuviésemos ya, ignoraríamos esta orden. Lo siguiente sería la
instalación propiamente dicha mediante `pip`

	sudo pip3 install ipython pyzmq jinja2 tornado
	sudo pip3 install "ipython[notebook]"

Hecho esto, podemos probar el resultado ejecutando en terminal:

	ipython notebook

Ello debería lanzarnos en el navegador el entorno Jupyter. Como hemos
explicado antes, es muy conveniente tener las librerías usuales en el
cálculo científico-técnico:

	pip3 install sympy
	pip3 install numpy
	pip3 install matplotlib
	pip3 install scipy

La instalación de `pygraphviz` requiere la librería `graphviz`. Para ello:

	brew install graphviz
	sudo pip3 install --install-option="--include-path=/opt/local/include" --install-option="--library-path=/opt/local/lib" pygraphviz


	sudo pip3 install networkx
	sudo pip3 install nxpd

También podemos instalar `simpy`, `gmpy` y `pytest`:

	sudo pip3 install simpy
	sudo pip3 install gmpy	
	sudo pip3 install pytest

## Resumen sin Comentarios para el caso de Ubuntu

Hemos hecho la siguiente instalación (en algunos de los pasos
deberemos tener paciencia, mucha paciencia):

	sudo apt install python-dev

	sudo apt install python3-dev

	sudo apt install g++

	sudo easy_install3 pip
	
	sudo pip install "ipython[notebook]"

	sudo apt install libfreetype6-dev

	sudo apt install pkg-config 

	sudo pip install matplotlib

	sudo pip install sympy

	sudo apt install libblas-dev liblapack-dev

	sudo apt install python-dev gfortran

	sudo pip install scipy

	sudo apt install libgraphviz-dev graphviz python3-dev

	sudo pip install pygraphviz

	sudo pip install graphviz

	sudo pip install networkx

	sudo pip install nxpd

	sudo pip install simpy
	sudo pip install gmpy
	
	sudo pip install pytest
	
## Resumen sin Comentarios para el caso de Mac OS X "El Capitan"
	
	brew install python3
	pip3 install ipython pyzmq jinja2 tornado
	sudo pip3 install "ipython[notebook]"

	ipython notebook

	pip3 install sympy
	pip3 install numpy
	pip3 install matplotlib
	pip3 install scipy

	brew install graphviz
	sudo pip3 install --install-option="--include-path=/opt/local/include" --install-option="--library-path=/opt/local/lib" pygraphviz

	sudo pip3 install networkx
	sudo pip3 install nxpd

	sudo pip3 install simpy
	sudo pip3 install gmpy
	sudo pip3 install pytest

Y ... esto es todo por hoy.
