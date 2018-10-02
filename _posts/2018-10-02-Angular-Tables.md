---
layout: post
title: Tabla generica en Angular
---

### Lectura de ~3 minutos

En la entrada [vistas genéricas]({{ site.baseurl}}{% post_url 2018-02-06-Vistas-Genericas %}) vimos un caso en el que era conveniente crear una tabla genérica para mostrar información. El ejemplo está hecho con [**.Net Core**](https://dotnet.github.io). En este caso vamos a realizar el mismo caso por con [**Angular**](https://angular.io).

[Aquí](https://github.com/44r0n/angular-generic-table) podeis encontrar el proyecto en cuestión, con un simple ejemplo. Espero que me haya quedado claro. De todos modos lo explico un poco.

El proyecto es un proyecto [**Angular**](https://angular.io) vacío con el componente *generic-table* que se encuentra en el directorio `src/app/generic-table`. En el fichero `generic-table.component.ts` están definidas las entradas y salidas del componente:

#### Entradas

- **columns: string[]**: especifica el nombre de las columnas que se van a mostrar en las tablas. 

- **elements: any[]**: especifica los elementos que se van a mostrar en la tabla.

- **attributeNames: string[]**: especifica el nombre de los atributos de los objetos que se han pasado en **elements**.

- **edit: boolean**: indica si se debe mostrar el botón de editar en una columna adicional a la derecha. Este botón lanza un evento de edición.

- **see: boolean**: indica si se debe mostrar el botón de ver en una columna adicional a la derecha. Este botón lanza un evento de visualización.

- **datePipes: string[]**: *pipe* para mostrar los campos de fecha.

- **statusFields: string[]**: indica los campos que son enumerados.

- **statusTypes: any[]**: el valor de los campos enumerados.

#### Salidas

- **elementSelected**: evento que se dispara cuando se pulsa el botón para editar. El evento devuelve el elemento seleccionada para la edición.

- **watchSelected**: evento que se dispara cuando se pulsa el botón ver. El evento devuelve el elemento seleccionada para el visionado.

Para ver el ejemplo en funcionamiento solamente con hacer `git clone`, `npm i` y `npm start` nos muestra una tabla genérica de heroes en el que se puede ver la información del heroe seleccionado.

Como he comentado al principio [aquí](https://github.com/44r0n/angular-generic-table) está el proyecto para que puedas trastear.

Nos vemos en las próximas líneas.
