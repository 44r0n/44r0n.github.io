---
layout: post
title: Patrón composite
---

#### Lectura de ~2 minutos

El patrón composite compone una estructura en árbol para representar jerarquías, permitiendo a los clientes tratar a los objetos individuales y a las composiciones del mismo modo.

La clave de este patrón es una clase abstracta que representa tanto a las partes individuales como a las contenedoras.

### Aplicabilidad

-   Cuando se pretende representar jerarquías del mismo tipo.
-   Cuando el cliente no tiene porque conocer el tipo compuesto y el individual.

### Estructura

![UML Composite]({{ site.baseurl}}/images/UMLComposite.png){:class="img-responsive"}

El cliente siempre usará la interfaz de la clase abstracta **Component**. Si el objeto es una hoja (Leaf) ejecutará la operación, por el contrario si es un compuesto (Composite) se deberá recorrer todos sus componentes (**Component**) para ejecutar la operación.

### Consecuencias
-   Se define una jerarquía de objetos en la que el tipo primitivo (hoja) a su vez puede tener otro tipo de composición jerárquica.
-   Se simplifica el uso desde el punto de vista del cliente. Se trata igual a la jerarquía por completo que a un elemento.
-   Es mucho más sencillo añadir nuevos tipos de componentes.
-   Puede hacer el diseño excesivamente generalizado, de modo que complica añaidr un nuevo componente con restricciones.

Puedes encontrar un sencillísimo ejemplo en dotnetcore [aquí](https://github.com/44r0n/CompositePattern).

Nos vemos en las próximas líneas.
