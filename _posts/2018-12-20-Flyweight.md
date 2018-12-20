---
layout:  post
title:  Patrón Flyweight
---

### Lectura de ~3 minutos

Usa la memoria compartida para poder dar soporte a un gran número de objetos pequeños de forma eficiente.

### Aplicabilidad

La efectividad de este patrón depende de como y donde se usa, aplica esta patrón si se cumplen **TODAS** las condiciones siguientes:

- La aplicación usa un gran número de objetos.

- Los costes de almacenamientos son grandes por la cantidad de objetos.

- Casi todo el estado de cada objeto puede ser extrínseco (externo al objeto).

- Muchos grupos de objetos pueden ser reemplazados por relativamente pocos objetos compartidos una vez que el estado extrínseco se remueve.

- La aplicación no depende de la identidad de cada objeto.

### Estructura

![Flyweight Diagram]({{ site.baseurl}}/images/FlyweightPattern.png){:class="img-responsive"}

Se define una interfaz que con la que se manejarán los objetos compartidos. Para asegurar que son compartidos, se crean a través de un objeto [factoría]({{ site.baseurl}}{% post_url 2018-12-11-FactoryMethod %}). Una vez creados, el cliente usa los objetos como si fueran únicos.

### Consecuencias

- Reduce drásticamente el número de instancias.

- Parte de la información de los objetos compartidos pasa a ser responsabilidad del cliente.

Como de costumbre, [aquí](https://github.com/44r0n/FlyweightPattern) un pequeño ejemplo en net core.

Nos vemos en las próximas líneas.
