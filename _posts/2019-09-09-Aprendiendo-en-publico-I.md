---
layout:  post
title:  Aprendiendo en público - Introducción (I)
---

### Lectura de ~4 minutos

Este es el primer post de una serie.

¿Que es aprender en público? Es aprender cosas que no sabía e ir publicandolas a la vez que las voy aprendiendo. En este caso voy a aprender con vosotros una seria de tecnologías para hacer un sitio web pequeño que nos permita hacer registrarnos y autenticarnos. Así que sin más demora, empezamos.

## Tecnología base

El proyecto se puede dividir en 4 tipos de tecnología, transversal, base de datos, servicio y web. A continuación una pequeña explicación de qué tecnologías he elegido y porqué:

### Transversal

[**Docker**](https://www.docker.com).  ¿Porque [`Docker`](https://www.docker.com)? Por las razones que entiendo que da todo el mundo. Permite desarrollar teniendo la máquina de desarrollo limpia, sin instalar las dependencias de desarrollo o las herramientas que se necesitan en producción en tu máquina local y porque una vez tienes un contenedor de [`Docker`](https://www.docker.com) funcionando en el equpo de desarrollo sabes a ciencia cierta que funcionará igual en producción (o casi).

Primero necesitamos tener [`Docker`](https://www.docker.com) instalado en nuestro sistema. No voy a explicar como instalarlo o como se usa, [en su página web](https://www.docker.com) especifican las instrucciones a seguir dependiendo de tu sistema operativo y [aquí](https://docs.docker.com/get-started/) los primeros pasos para ir aprendiendo. 

Usaré `Docker Compose` para gestionarlo todo. ¿Porque no [`Kubernetes`](https://kubernetes.io)? Porque `Docker Compose` viene instalado con [`Docker`](https://www.docker.com) y es sencillo de utilizar para lo que quiero hacer: iniciar y parar pocos contenedores con las dependencias pertinentes.

### Base de datos

[**PostgreSQL**](https://www.postgresql.org) es una base de datos libre, con el suficiente tiempo de vida para estar consolidada, que se comporta decentemente a pequeña escala y crece a grande escala muy bien. 

Para facilitar el desarrollo la base de datos estará "Dockerizada", pero para producción se recomienda no hacerlo por temas de rendimiento y perdidas de datos al no gestionar bien las imágenes de [`Docker`](https://www.docker.com).

### Servicio

[**Node**](https://nodejs.org/en/). La parte del servicio estará hecho con [`Node`](https://nodejs.org/en/) y [`Typescript`](https://www.typescriptlang.org). ¿Porque? Porque me está gustando el camino que está tomando [`Node`](https://nodejs.org/en/), es compatible con todos los grandes sitemas operativos del mercado y me gusta el tipado fuerte y las opciones que trae [`Typescript`](https://www.typescriptlang.org), facilita el desarrollo si empieza a ser muy grande añadiendo pocas pegas si el proyecto es pequeño.

### Web

Para la web usaré [`Vue`](https://vuejs.org) y [`Bulma`](https://bulma.io). De entre todas las plataformas que existen actualmente para realizar un proyecto  'front', [`Vue`](https://vuejs.org) es la que está teniendo una buena aceptación y evolución. Como no soy maquetador/diseñador y como no tengo mucha idea sobre el tema, necesito de un framework para realizar el trabajo pesado de forma rápida. He elegido [`Bulma`](https://bulma.io) por la facilidad que ofrece en su uso y personalización.

### Discusión

¿Se podrían haber elegido otras tecnologías? Claro. ¿Habrían funcionado igual o mejor que las elegidas? Por supuesto. No voy a entrar al debate de que tecnología es mejor para tal o cual cosa. Para el caso que tengo entre manos he elegido las que he elegido y he dado las razones, y puede que no estés de acuerdo y es totalmente normal.

### Repositorio
Como siempre, [en este enlace](https://github.com/44r0n/Helenos) está el repositorio para ir comprobando lo que se va haciendo. Si ves alguna cosa que no te encaja y ves que se puede mejorar, el repositorio está abierto a contribuciones, `Forkealo` y propón una `Pull Request` sin ningún problema ;-)

Nos vemos en las próximas líneas.