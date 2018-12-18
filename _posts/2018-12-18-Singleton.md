---
layout:  post
title:  Patrón Singleton
---



### Lectura de ~2 minutos

Asegura que solo hay una instancia de una clase y provee un acceso a esa instancia.

### Aplicabilidad

- Debe haber exactamente una instancia de una clase y debe ser accesible.

- La única instancia debe ser extensible creando subclases y los clientes deberías poder usar dichas subclases sin modificar su código.

### Esctructura

![Singleton Diagram]({{ site.baseurl}}/images/SingletonPattern.png){:class="img-responsive"}

Se crea una clase con un objeto estático privado que será devuelto cada vez que se invoque el método de instanciarlo. La primera vez que se invoque este método se instanciará. El constructor de esta clase debe ser privado.

### Consecuencias

- El acceso a la única instancia está controlado.

- Reduce el espacio de nombres, no son necesarias variables globales ni su control.

- Permite refinamiento a través de subclases.

- Permite un número de instancias variable. Al tener control sobre las instancias que se crean, se puede controlar el número que se crea.

Como de costumbre [aquí](https://github.com/44r0n/SingletonPattern) un pequeño ejemplo en Net core.

Nos vemos en las próximas líneas.