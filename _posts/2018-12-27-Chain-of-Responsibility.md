---
layout:  post
title:  Patrón Chain of Responsibility
---

### Lectura de ~3 minutos

Evita el acoplamiento entre quine envía la petición y quien la procesa, dando a más de un objeto la oportunidad de procesar la petición.

### Aplicabilidad

- Más de un objeto puede procesar la petición, pero no se sabe cual a priori.

- Se quiere lanzar la petición a un objeto de enetre varios sin especificar el receptor de forma explícita.

- El conjunto de objetos que debe procesar la petición se debería especificar de forma dinámica.

### Esctructura

![Chain of Responsibility Diagram]({{ site.baseurl}}/images/ChainofResponsibility.png){:class="img-responsive"}

Se define una clase abstracta que define la estructura de sucesión de peticiones y define una interfaz única para procesar todas las peticiones.

### Consecuencias

- Reduce el acoplamiento.

- Añade flexibilidad a la hora de asignar responsabilidades a objetos

- No se garantiza que la petición se procese.

Como siempre, [aquí](https://github.com/44r0n/ChainofResponsigility) un ejemplito en Net core.

Nos vemos en las próximas líneas.
