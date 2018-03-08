+++
title = "Adecuar emacs para usar Pdflatex y Evince por defecto"
date =  "2018-03-08"
author =  "M García"
description = "Por alguna razón que desconocemos, nuestro dúo preferido para editar ficheros LaTeX ---emacs y auxtex--- no viene preconfigurado en Ubuntu para usar por defecto el formato `pdflatex` y el visor `Evince`"
tags = ["latex", "emacs", "evince"]
+++

## Introducción

Por alguna razón que desconocemos, nuestro dúo preferido para editar ficheros LaTeX ---emacs y auxtex--- no viene preconfigurado en Ubuntu para usar por defecto el formato `pdflatex` y el visor `Evince`. Por una parte creemos desfasado compilar con LaTeX para crear un `.dvi` y por otra, la mayoría de visores no actualizan automáticamente un `.pdf` si éste ha sido modificado; no es el caso de Evince. Por tanto, queremos que al compilar los `.tex` desde emacs+auctex actúe `pdflatex` y que bajo demanda se abra el `.pdf` generado mediante Evince ... y por suerte emacs es muy configurable.

Suponemos ya instalado en nuestro Ubuntu `emacs`, TeX Live y `auctex` (cfr. [Instalar TeX Live en Ubuntu, Mac OS y Windows](https://wildunix.es/posts/instalar-tex-live-en-ubuntu-mac-os-y-windows/).)

Este post fue elaborado en colaboración con D. Alberto Rodríguez.

## Procedimiento

Seguiremos los siguientes pasos:

* Abrimos cualquier fichero .tex que tengamos a mano.

* Pulsamos en "LaTeX" del menú contextual de emacs.

* Posicionamos el cursor en "Customize AUCTeX" y pulsamos en "Extend this Menu".

* Volvemos a pulsar en "LaTeX" y poner el cursor sobre "Customize AUCTeX" ... ahora aparecerá un menú más amplio, como era de esperar.

* Nos posicionaremos sobre "Tex Command" y luego sobre "Tex Pdf Mode..." del submenú; haremos clic y veremos que se abre una nueva página. Se trata de operar sobre ella.

* Veremos el apartado "Tex Pdf Mode" y debe estar en "off (nill)". Esto debe cambiar y para ello pulsamos sobre la tecla "Toggle" y ahora aparecerá a "on (non-nill)". Es necesario guardar lo hecho, por lo que pulsaremos sobre el botón "Save for future sessions". Con esto AUCTeX compilará ahora los .tex con pdflatex y generará ficheros .pdf. Pulsamos sobre el botón "Exit".

* Seguidamente pulsamos de nuevo sobre "Latex" y pulsamos  sobre "Customize AUCTeX > Tex Command > Tex View > Tex View...". Vamos a "Tex Source Correlate Method" y:                       

	* Pulsamos en el botón "Value Menu" y seleccionamos "synctex". Pulsamos en el botón "State" y marcamos "Save for Future Sessions".

	* Pulsamos en "Value Menu" de la sección "Tex Source Correlate Start Server" y marcamos "Always".

	* Pulsamos en el botón "State" y marcamos "Save for Future Sessions".

	* Pulsamos en "Toggle" de la sección "Tex Source Correlate Mode". Pulsamos en el botón "State" y marcamos "Save for Future Sessions".

Pulsamos sobre el botón "Exit".

* Para seleccionar a Evince o Okular  como visor predeterminado de pdf:

	* Pulsamos de nuevo sobre "Latex" y pulsamos sobre "Customize AUCTeX > Tex Command > Tex View > Tex View Program List..." y pulsamos sobre el botón "INS" y rellenamos el formulario como sigue:

	    * en "Name:" escribimos:  Evince.

		* si no vemos junto a "Value Menu" la palabra "Command:", pulsamos en dicho botón y seleccionamos "Command".

		* En "Command:" escribimos: `evince --page-index=%(outpage) %o`

		* Pulsamos en "State" y seleccionamos "Save for Future Sessions".

		* Pulsamos sobre el botón "Exit".

	* Pulsamos de nuevo sobre "Latex" y pulsamos  sobre "Customize AUCTeX > Tex Command > Tex View > Tex View Program Selection.." y pulsamos sobre el botón "INS" y rellenamos el formulario como sigue:

	    * Pulsamos sobre el primer botón de "Value Menu" y seleccionamos "Single predicate"

		* Pulsamos en el  botón "Value Menu" contiguo y seleccionamos "output-pdf".

		* En "Viewer" pulsamos su botón correspondiente de "Value Menu" y seleccionamos "Evince".

		* Pulsamos en "State" y seleccionamos "Save for Future Sessions".

		* Pulsamos sobre el botón "Exit".

Ahora cerramos emacs y si volvemos a abrir con él algún `.tex`, veremos que funciona como deseábamos.

Como complemento y en relación con ello, les recomendamos leer este post nuestro.

## Para los que prefiren Okular

Si alguno de los lectores prefiere Okular en lugar de Evince, como visor de pdf,  puede seguir las indicaciones  que encontrará [aquí](http://mathieu.3maisons.org/wordpress/how-to-configure-emacs-and-auctex-to-work-with-a-pdf-viewer).

Y ... esto es todo por hoy.
