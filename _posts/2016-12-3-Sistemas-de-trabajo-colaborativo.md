---
layout: post
title: Sistemas de trabajo colaborativo
---
Hoy en día, cualquier trabajo digital se realiza de forma colaborativa. Ya sea con compañeros sentados a tu lado. O puede que ese compañero esté a miles de kilómetros. Incluso puede que ese compañero seas tu mism@ en el futuro, o pasado. Ese trabajo digital no tiene porque crear software. Puede ser escribir un libro, un artículo de periodismo o un trabajo para el colegio. ¡También este post!

Si queremos tener un control del avance de ese trabajo debemos versionar el trabajo que estamos realizando. ¿No has puesto un nombre en el archivo como `trabajoantiguo.doc` o `trabajo1.doc` `trabajo2.doc`? Y si ese trabajo es entre varios compañeros, ¿como se combina el trabajo de todos en uno solo? Normalmente se comparten archivos entre los compañeros y hay un arduo trabajo por delante para ponerlo todo junto. ¿Y si dicho trabajo son varios ficheros? En lugar de nombrar el fichero se incluyen todos en una carpeta y se renombra esa carpeta. En su día pasé por todas estas situaciones y es un engorro trabajar así. Hoy en día existen herramientas que nos facilitan la vida en este sentido, los llamados sistemas de control de versiones. En este primer post voy a explicar los distintos tipos que existen para más adelante detallarlos en herramientas.

## ¿Qué debe cumplir un sistema de control de versiones?

Para que un sistema de control de versiones sea eficaz en su trabajo debe cumplir las siguientes características:

- Se debe poder saber que cambios se han realizado en cada fichero.
- Se debe poder saber quien ha realizado dichos cambios.
- Se debe poder saber cuando se han realizado dichos cambios.
- En caso que los cambios entren en conflicto se deben poder controlar y resolver.
- Comparar los ficheros entre versiones. No solamente con la anterior.

Dadas estas características hay dos tipos de sistemas de gestión de versiones.

### Sistemas centralizados.

En los sistemas de control de versiones centralizados existe un sólo lugar en el que se guardan los cambios. Una vez se realicen cambios y se confirman, todos los miembros del equipo pueden acceder a dichos cambios. Es muy sencillo aprender a usar un sistema de control de versiones centralizado. Pero el manejo de distintos desarrollos en el mismo proyecto se complica.

### Sistemas distribuidos.

En los sistemas de control de versiones distribuidos existen varios lugares en el que se guardan los cambios. En este sistema se confirman los cambios en cada lugar de trabajo para después poder confirmarlos entre ellos. Comúnmente se suele configurar uno de estos lugares como aglutinador de todos los cambios de los miembros del equipo. Al contrario que en el sistema de control de versiones centralizado es más complicado aprender a usar y el manejo de distintos desarrollos se hace más fácil.

Nos vemos en las próximas líneas.
