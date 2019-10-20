---
layout:  post
title: Aprendiendo el púbcico (II)
---

### Lectura de ~3 minutos

En el artículo anterior veíamos que [tecnologías usaría para un proyecto que registrara y autenticara usuarios]({{ site.baseurl}}{% post_url 2019-09-09-Aprendiendo-en-publico-I %}). En este post explicaré como he montado el primer punto, la base de datos.

Como he dicho, la base de datos para desarrollo la tendré "Dockerizada". Tendremos una base de datos [`PostgreSQL`](https://www.postgresql.org) dentro de [`Docker`](https://www.docker.com).

¿Cómo montamos esto?  La base de datos en sí será lo más "sencillo", puesto que no necesitaremos mucha configuración o intervención una vez montada. Así que "solamente" hace falta especifcar todos los detalles de la base de datos en un fichero que estará en el directorio raíz llamado `docker-compose.yml`:

```yaml
version: '3'
services:
  database:
    restart: always
    image: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_PASSWORD: docker
      POSTGRES_USER: docker
      POSTGRES_DB: initialdb
```

Para lanzar esto ejecutamos el comando `docker-compose up` o si no queremos que nos deje la consola ocupada con su variante `-d`.

Con esto ya tendremos una base de datos levantada y lista para el uso. Si hemos ejecutado el comando `docker-compose up` sin el flag `-d` tendremos el terminal ocupado mostrando la salida de [`Docker`](https://www.docker.com). Si queremos pararlo, con pulsar `Ctrl + C` pararemos la ejecución. En caso contrario podemos parar todos los contenedores lanzados con el comando `docker-compose stop`. Si también queremos eliminar los contenedores generados usaremos el comando `docker-compose down --volumes`. Si quieremos eliminar los contenedores creados por [`Docker`](https://www.docker.com) lo podemos hacer con el comando `docker rm <nombre-contenedor>` y si queremos eliminar las imagenes creadas lo podemos hacer con el comando `docker rmi <nombre-imagen>`.

Para comprobar que todo ha funcionado correctamente puedes intentar conectar con un cliente de [`PostgreSQL`](https://www.postgresql.org). Como se ha indicado en el fichoer `docker-compose.yml` el host es `localhost`, el puerto 5432, usuario docker y contraseña docker. Si consigues conectar es que todo ha ido correctamente.

En este punto en concreto no le sacaríamos partido a `Docker Compose` porque realmente sólo estamos creando una imagen y un contenedor. A partir del siguiente punto ya se le sacará más punta.

Cualquier duda o comentario no dudes en hacermelo llegar.

Nos vemos en las próximas líneas.