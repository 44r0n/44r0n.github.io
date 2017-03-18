---
layout: post
title: Docker y MySQL. Race Problem
---

#### Lectura de ~3 minutos

Hoy he estado trabajando en my proyecto [Sessionmanager](https://github.com/44r0n/Sessionmanager). El mismo que dije [el post de objetivos](2017-01-05-objectives.md), ha cambiado un poco desde que me lo planteé hace unos meses. Y seguramente seguirá cambiando.

Ahora mismo sigue siendo un proyecto para gestionar usuarios, pero he optado por no ligarlo a *Sinatra* y *Ruby* de momento, se podrá hacer más adelante y de forma desaclopada. Como base de datos será de momento únicamente *MySQL*.

Al empezar me pregunté si debía instalar un servidor *MySQL* en mi equipo de desarrollo, pero ¡eh! ¡Espera! Me vino a la cabeza *[Docker](https://www.docker.com/)*, así que me puse a investigar si había una imagen para *MySQL*. Extraño que no la hubiera, y acerté, ahí estaba. Me puse manos a la obra, y tras unos tutoriales autodidactas ya tenía un servidor montado y en marcha. Funcionaba a las mil maravillas. Incluso he publicado mi pequeña [primera release](https://github.com/44r0n/Sessionmanager/releases/tag/0.1) y yo muy emocionado.

Los problemas vinieron a continuación cuando quise integrar [travis](https://travis-ci.org/), y no por parte del servicio de integración. Simplemente no encontraba como hacer esperar al script de pruebas que el servicio de *MySQL* estuviera listo y a la escucha. La primera idea que me vino a la cabeza es, sencillo con solo incluir un `sleep` de x tiempo te vale. ¡Pero no! ¿X? ¿Cuanto x? Así que otra búsqueda por la red. En los primeros enlaces me encontré con el siguiente script:

~~~
while ! mysqladmin ping -h"$DB_HOST" --silent; do
    sleep 1
done
~~~

Al probarlo se me queda el sistema en bucle infinito y no lograba a ejecutar las pruebas. Pero como bien sabemos, hay más de un modo de solucionar el mimso problema, y en el mismo hilo en el que encontré la solución anterior también encontré la siguiente:

~~~
until nc -z -v -w30 $CFG_MYSQL_HOST 3306
do
  echo "Waiting for database connection..."
  # wait for 5 seconds before check again
  sleep 5
done
~~~

¡Y por fin! En mi local funciona. Solo quedaba hacer el `push` de turno y ver la respuesta [travis](https://travis-ci.org/). Al parecer nuestro querido sistema de integración estaba de acuerdo conmigo y realizó las pruebas dandome un OK. Así que tarea realizada.

Lo que me llamó la atención es la cantidad de enlaces que encontré que ofrecián la solución de `sleep`, algunos con 5 segundos, otros con 10. Obviamente no es una solución válida.

Nos vemos en las próximas líneas.
