---
layout: post
title: Patrón Abstract Factory
---

#### Lectura de ~n minutos

Provee una interfaz para crear familias dependientes sin especificar clases concretas.

### Aplicabilidad

-   Cuando un sistema debe ser independiente de cómo sus productos son credos, compuestos y representados.
-   Cuando un sistema debe ser configurado por una o varias familias de productos.
-   Cuando una familia de productos están diseñados para ser usados juntos, y se quiere restringir ese uso conjunto.
-   Cuando se provee una librería de productos, y se quiere revelar sus interfaces, pero no su emplementación.

### Estructura

![UMLAbstractFactory]({{ site.baseurl}}/images/AbstractFactoryPattern.png){:class="img-responsive"}

Se define una serie de productos, cada producto tiene una interfaz abstracta que será usada por el cliente. A su vez de define una una factoría abstracta y sus implementaciones. Cada implementación de la factoría será la encargada de instanciar los productos.

### Consecuencias

-   Aisla las clases concretas. El cliente manipula las instacias a través de las interfaces abstractas. El patrón factory encapsula la responsabilidad y proceso de creación de los objetos, de modo que las clases de producto no aparecen en el código cliente.
-   Es fácil cambiar familias de productos. Al ser sólo la factoría la encargada de crear los objetos, al cambiar la factoría cambiamos la familia.
-   Promueve el uso de productos consistentes. Al crear los productos en familias, si se deben usar conjuntamente nos los ofrece en conjunto.
-   Se se debe implementar un nuevo tipo de familia se debe crear desde la abstracción, haciendo este proceso difícil.

[Aquí](https://github.com/44r0n/AbstractFactoryPattern.git) un pequeño ejemplito en .net core.

Nos vemos en las próximas líneas.
