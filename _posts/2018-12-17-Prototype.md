---
layout:  post
title:  Patrón Prototype
---

### Lectura de ~3 minutos

Especifica los tipos de objetos a crear usando una instancia de prototipo, y crea nuevos objetos copiando ese prototipo.

### Aplicabilidad

El sistema debe ser independiente de como se crean componen y representan los productos **y**:

- las clases a instanciar se especifican en tiempo de ejecución

  **o**

- se quiere evitar construir una jerarquía de clases factoría de forma paralela la jerarquía que se produce

  **o**

- las instancias de una clase tienen un estado concreto de pocas combinaciones posibles

### Esctructura

![Prototype Diagram]({{ site.baseurl}}/images/PrototypePattern.png){:class="img-responsive"}

Se define una clase abstracta que delega en las subclases la creación de nuevas instancias que son copia de la actual.

### Consecuencias

Tiene muhcas de las mismas consecuencias que [Abstract Factory]({{ site.baseurl}}{% post_url 2018-01-25-Abstract-Factory-Pattern %}) y [Builder]({{ site.baseurl}}{% post_url 2018-12-14-Builder %}), oculta las clases concretas al cliente, dejando que el cliente trabaje con clases de aplicación. Además contiene los siguientes beneficios:

- Permite añadir y quitar productos en tiempo de ejecución.

- Permite crear nuevos objetos variando valores o estructuras.

- Reduce el número de subclases.

- Permite configurar una aplicación con clases de forma dinámica.

[Aquí](https://github.com/44r0n/PrototypePattern) un pequeño ejemplo en Net core.

Nos vemos en las próximas líneas.
