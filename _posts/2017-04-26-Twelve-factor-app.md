---
layout: post
title: Twelve factor app
---

#### Lectura de ~7 minutos

Recientemente me he encontrado con [esta web](https://12factor.net/es/) en la que se explican 12 factores o condiciones que se debe cumplir cualquier software que quiera ser considerado *como servicio*.

### 1. Código base.
Parte del principio que el software debe tener un único código base, gestionado mediante algún sistema de control de versiones. Teniendo una relación uno a uno entre las diferentes versiones de código base que pudiera tener el repositorio y la aplicación de tal forma que si:
-   Hay varios códigos base se considera un sistema distribuido, considerando cada código base una aplicación distinta.
-   El código compartido entre varias aplicaciones son librerías y se deben controlar mediante un gestor de dependencias.

Este punto debe ser de obligado cumplimiento, no solamente para aplicaciones de tipo *SaaS*, si no para cualquier tipo de aplicación.

### 2. Dependencias.
No hay dependencias explicitas de paquetes del sistema. Todas las dependencias están completamente y explícitamente indicadas mediante un manifiesto de *declaración de dependencias*. Las dependencias implícitas no deben afectar al sistema.
No se debe depender de ninguna herramienta del sistema `curl` por ejemplo. Si se debe utilizar alguna herramienta debe estar en la instalación de la aplicación.

Necesario para no tener quebraderos de cabeza.

### 3. Configuración.
La configuración del sistema no debe estar en código. Y tampoco debería estar en un fichero de configuración porque puede publicarse por error en el repositorio. Para ello la solución que se ofrece es guardar la configuración en variables de entorno.

Está bien planteado por temas de seguridad. Pero no estoy del todo de acuerdo. Si tienes un despliegue automático estilo `docker`, ¿como se declaran las variables de entorno para la configuración? ¿En el `dockerfile`? ¿No es acaso mejor tener el fichero de configuración `.yml` o `.config`?

### 4. Backing services.
Un *backing service* es un recurso que la aplicación puede consumir a través de la red. Por ejemplo una base de datos, correo, APIS's... No se debe diferenciar entre aplicaciones locales y aplicaciones de terceros. De forma que si cambiamos un recurso por otro no afecta al código base.

Como es el caso de punto de código base, lo considero de obligado cumplimiento para cualquier tipo de aplicación.

### 5. Construir, distribuir, ejecutar.
-   La *etapa de construcción* convierte el código en un ejecutable.
-   La *etapa de distribución* une la construcción anterior con la configuración para tener la aplicaicón lista para su ejecución en el entorno de ejecución.
-   La *fase de ejecución* ejecuta la aplicación en el entorno.

Se deben mantener siempre separadas estas fases, de forma que no se puede modificar el código de la aplicación si está en ejecución. Además cada distribución debe estar identificada de forma única.

Otro punto que debe ser de obligado cumplimiento por el bien de la salud mental. Nunca se deben hacer cambios directamente sobre producción.

### 6. Procesos.
La aplicación se ejecuta en el entorno de ejecución, sin estado. Si se necesita de persistencia se debe almacenar como un *baking service*.

En mi opinión es una buena forma de aislar dependencias.

### 7. Asignación de puertos.
Las aplicaciones *twelve factor* son autocontenidas, es decir, no depende de ningún servidor web para crear un servicio. Este problema se suele resolver mediante una declaración de dependencia a una librería web. Una vez la aplicación esta en ejecución, puede ser usada como un *baking service* de otra aplicación, simplemente se llama a la *URL* para obtener el servicio.

Un apartado a tener muy en cuenta, seguir esta filosofía nos aproxima al microservicio y nos aleja de las aplicaciones estilo silo difíciles de mantener.

### 8. Concurrencia.
Se debe tener en cuenta los procesos pesados, siendo responsabilidad del desarrollador distribuir la ejecución de la aplicación. En el caso de una aplicación web, procesar las llamadas *HTTP* por un lado y las tareas por otro. Cada proceso independiente se debe gestionar de forma única. Al dividir en procesos se puede escalar de forma que se debe permitir que cada proceso se pueda ejecutar en máquinas distintas.

Este punto no es de especial interés si la aplicación va a ser pequeña y de poca carga. Pero para las aplicaciones de gran carga es un punto esencial para que funcione debidamente.

### 9. Desechabilidad.
La aplicación se debe poder iniciar y finalizar en los momentos necesarios. Teniendo un tiempo de arranque y apagado mínimo podemos propagar cambios de la aplicación y configuraciones de forma rápida. Los procesos deben estar preparados para los casos de finalizaciones inesperadas.

Es un punto en el me da la sensción que no se le da la antención que merece. ¿Cuantas veces nos quedamos esperando a que la aplicación arranque? Cuando queremos algo lo queremos ya. No debería aplicarse solo a las aplicaciones web.

### 10.  Igualdad entre desarrollo y producción.
Hay tres puntos en los que varía un entorno de desarrollo y un entorno de producción.
-   **Diferencias de tiempo.** Los cambios que realiza un desarrollador puede tardar días o meses en ser visibles en producción.
-   **Diferencias de personal.** Los desarrolladores escriben el código y los ingenieros de operaciones se encargan de ponerlo en producción.
-   **Diferencias de herramientas.** En desarrollo se utilizan unas herramientas y en producción otras.

Se deben minimizar estas diferencias de forma que debe ser posible:
-   **Reducir diferencias de tiempo.** Debe ser posible desplegar el código que escribe un desarrollador en minutos.
-   **Reducir diferencias de personal.** Las mismas personas que escriben el código deben ser responsables de desplegarlo y observar como se comporta en producción.
-   **Reducir diferencias de herramientas.** Desarrollo y producción deben ser los más idénticos posible.

Resumiendo, se debe implementar una filosofía de desarrollo de integración continua, despliegue continuo, en el que los desarrolladores deben poder probar las mismas herramientas que habrán en producción y son ellos mismos los que ven el código en producción.

Otro punto que debería ser obligatorio, no solamente para aplicaciones web.

### 11. Historiales.
Toda aplicación *twelve factor* debe tener habilitado el historial, y dicho historial es un flujo. La aplicación no se debe preocupar del tratamiento de ficheros o accesos a bases de datos para dicho propósito.

Un punto en el que se debe tener cuidado. Si se tiene una aplicación con gran carga el historial puede colapsar el disco.

### 12. Administración de procesos.
Las tareas de gestión o administración que se quieran realizar sobre la aplicación solamente se debe realizar una vez. El código de administración o gestión se debe enviar con el código base para evitar problemas. Y además las dependencias que pudiesen haber también deben ser tratados de con un aislamiento de dependencias.

Éste último punto también debería ser de obligatorio cumplimiento para todo tipo de aplicaciones, no solo las aplicaciones web.

Nos vemos en las próximas líneas.
