---
layout: post
title: Patrón decorator
---

#### Lectura de ~n minutos

El patrón decorator añade o retira responsabilidades a un objeto de forma dinámica. Es una alternativa a la herencia.

### Aplicabilidad

-   Añadir nuevas responsabilidades de forma dinámica y transparente, sin afectar a otros objetos.
-   Retirar dichas responsabilidades.   
-   Cuando la extensión por herencia es impracticable. Si hay muchas combinaciones de responsabilidades de forma que crearía una gran cantidad de subclases.

### Escrtuctura

![UMLDecorator]({{ site.baseurl}}/images/DecoratorPattern.png){:class="img-responsive"}

Se define una interfaz  de un componente a la que un componente concreto debe implementar. Al componente concreto se le añade responsabilidades a través de una clase que también implementa la interfaz de componente. Dicha clase mantiene una referencia del componente al que se le añade responsabilidades, de forma que se pueden añadir viarias.

### Consecuencias

-   Las responsabilidades se asignan de fomra dinámica no por herencia. Incluso se puede asignar dos veces el mismo tipo de responsabilidad.
-   La funcionalidad del sistema se puede definir en pequeños segmentos, en lugar de tener una clase compleja.
-   El objeto de tipo Decorator y el objeto de tipo Componente no son iguales, de modo que no se debe operar con el operador de igualdad con decorators.
-   Puede crear sistemas con exceso de clases pequeñas, una para cada tipo de responsabilidad.

Puedes encontrar, como siempre, un pequeño ejemplo en .net core [aquí](https://github.com/44r0n/DecoratorPattern).

Nos vemos en las próximas líneas.
