---
layout:  post
title:  Patrón Interpreter
---

### Lectura de ~3 minutos

Dado un lenguaje, define una representación para su gramática para interpretar sentencias de dicho lenguaje.

### Aplicabilidad

Se debe usar cuando ha un lenguaje que interpretar y se puede representar sus sentencias en un árbol de sintáxis abstracto. El patrón interpreter funciona mejor en los siguientes casos:

- La gramática es simple

- La eficiencia no es un punto crítico.

### Esctructura

![Interpreter Diagram]({{ site.baseurl}}/images/InterpreterPattern.png){:class="img-responsive"}

Se define una interfaz que indica que operaciones se pueden realizar en el interprete dado un contexto. Las clases que implementan dicha interfaz se diferencian en expresiones terminales (que no necesitan seguir evaluando el contexto) y las no terminales (que necesitan seguir evaluando el contexto).

### Consecuencias

- Es sencillo cambiar y extender la gramática.

- Es sencillo implementar la gramática.

- Las gramáticas comlejas son difíciles de mantener.

- Permite añadir nuevas formas de interpretar expresiones.

Como de costumbre [aquí](https://github.com/44r0n/InterpreterPattern) un pequeño ejemplo en Net core.

Nos vemos en las próximas líneas. 
