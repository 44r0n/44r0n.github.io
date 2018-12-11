---
layout:  post
title:  Patrón Factory Method
---

### Lectura de ~2 minutos

Define una interfaz para crear un objeto, pero delega en sus subclases que clase instanciar.

### Aplicabilidad

- Una clase no puede anticipar que tipo de bojetos debe crear.

- Se quiere que las subclases especifiquen los objetos que se crean.

- Las clases delegan la responsabilidad de una de muchas subclases, y se quiere localizar en que sublcase se ha delegado.

### Estructura

![Factory Method Diagram]({{ site.baseurl}}/images/FactoryMethodPattern.png){:class="img-responsive"}

La case Creator sabe cuando se debe instanciar una sublcase de Product, pero no que subclase en concreto, por lo que delega esa decisión a ConcreteCreator.

### Consecuencias

- Otorga flexibilidad a la hora de crear nuevos objetos.

- Conceta jerarquías de clases.

Como de costumbre os dejo [aquí](https://github.com/44r0n/FactoryMethod) un pequeño ejemplo en net core.

Nos vemos en las próximas líneas.
