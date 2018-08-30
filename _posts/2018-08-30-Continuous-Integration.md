---
layout: post
title: Integración continua y despliegue continuo, caso práctico.


---

#### Lectura de ~5 minutos

# ¿Integración qué?

Integración continua, palabro del mundo del software que ha estado pegando fuerte estos últimos tiempos. Básicamente lo que viene a decir es que cada miembro del equipo debe integrar el trabajo que realiza de forma frecuente, si es cada día mejor. Cada integración debe ser verificada por una construcción automática con sus tests para detectar problemas y errores lo más rápidamente posible. De modo que si se detecta un error en la época temprana antes se resuelve. Al tener un sistema que construye y prueba el proyecto por nosotros podemos desarrollar con más confianza y más rapidez.
Si quieres saber más sobre integración continua el artículo original de **Martin Fowler** sobre el tema lo tienes [aquí](https://www.martinfowler.com/articles/continuousIntegration.html). También hay literatura sobre el tema, [en el blog de **Javier Garzas**](http://www.javiergarzas.com/2014/08/implantar-integracion-continua.html) hay fabulosas entradas escritas por [**Ana M. del Carmen García Oterino**](http://www.javiergarzas.com/author/anamaria).

# ¿Y cómo lo pongo en práctica?

Hay varias formas:

- Con [Jenkins](https://jenkins.io).
- Con [Github](https://github.com) y [Travis](https://travis-ci.org).
- Con [Gitlab](https://gitlab.com).

Y seguro que hay muchísimos más. En este post vamos a ver el caso de [Gitlab](https://gitlab.com).

## ¿Qué necesitamos?

- Un proyecto de software al que queramos aplicar integración continua.
- Una cuenta en [Gitlab](https://gitlab.com) o bien una [instalación de Gitlab](https://about.gitlab.com/installation/).
- Conocimientos de [Git](https://git-scm.com). Puedes encontrar una introducción a como usar Git en la consola [aquí]({{ site.baseurl}}{% post_url 2017-05-26-Git %}).

Este caso es utilizando [Gitlab](https://gitlab.com). El primer paso a dar, es crear una cuenta si no la tienes. Una vez tengas creada la cuenta en [Gitlab](https://gitlab.com), crea un proyecto nuevo:

![newProjectGitlab]({{ site.baseurl}}/images/new-project-gitlab.png){:class="img-responsive"}

[Gitlab](https://gitlab.com) permite integración continua añadiendo un fichero llamado `.gitlab-ci.yml`, que se debe colocar en el directorio raíz del proyecto, este fichero indica los pasos que se deben dar para ejecutar las pruebas y el despliegue del proyecto. Al añadir este fichero indicamos  a [Gitlab](https://gitlab.com) que se va a proceder a realizar integración continua en el proyecto actual.

En cada `commit` realizado en el repositorio, [Gitlab](https://gitlab.com) busca el fichero `.gitlab-ci.yml` en la raíz. Si lo encuentra lanza unos *Runners* acorde al contenido del fichero. Si usas la versión web de [Gitlab](https://gitlab.com) la disponibilidad de estos *Runners* puede variar. 

[Aquí](https://docs.gitlab.com/ee/ci/quick_start/README.html) puedes encontrar la documentación  oficial del fichero `.gitlab-ci.yml`. En este caso, en la primera línea del fichero se indica la imagen de docker que se va a usar. En la etiqueta `stages` se indican las fases que se van a seguir para la integración indicando en qué orden se ejecutan. En la etiqueta `cache` se puede especificar uno o varios directorios que se comparte entre las distintas fases indicadas en `stages`, a pesar que en la [documentación oficial](https://docs.gitlab.com/ee/ci/) se da a entender que siempre funciona, no es así. Funciona según disponibilidad. Aún así es bueno indicarlo para que, cuando funcione, la construcción se agilice. Luego se deben indicar las fases para la construcción, dentro de cada uno se debe indicar los comandos necesarios que lo definen en `script`. En cada fase se puede indicar a que `stage` pertenece. Os dejo aquí un ejemplo para un proyecto de Angular.

```yaml
image: node:latest

cache:
  paths: 
    - node_modules/

stages:
  - build
  - test

build:
  stage: build
  script:
    - npm install
    - ./node_modules/@angular/cli/bin/ng build

test:
  stage: test
  script:
    - npm install
    - ./node_modules/@angular/cli/bin/ng lint
    - ./node_modules/@angular/cli/bin/ng test --browsers PhantomJS --watch=false
```

# Bonus Trak - Despliegue continuo

También se puede desplegar el proyecto, en este caso necesitamos un servidor web. Hemos elegido [Heroku](http://heroku.com). Crea un usuario y una vez registrado puedes crear un pipeline desde el botón new:

![newPipelineHeroku]({{ site.baseurl}}/images/new-pipeline-heroku.png){:class="img-responsive"}

Una vez creado debemos crear una clave para que [Gitlab](https://gitlab.com) pueda acceder a [Heroku](http://heroku.com). Esta clave se crea en las opciones de usuario que actualmente se encuentra en la parte superior derecha.

![apikeyHeroku]({{ site.baseurl}}/images/apikeyHeroku.png){:class="img-responsive"}

Y debemos copiar esa clave en el apartado de opciones de CI/CD de [Gitlab](https://gitlab.com).

![HerokuKeyInGitlab]({{ site.baseurl}}/images/HerokuKeyInGitlab.png){:class="img-responsive"}

Recuerda el nombre que has puesto en a la variable de la clave, ya que se usará en el fichero `gitlab-ci.yml`. Modificalo para añadir la fase de despliegue:

```yaml
image: node:latest

cache:
  paths: 
    - node_modules/

stages:
  - build
  - test
  - integration
  - staging
  - deploy

build:
  stage: build
  script:
    - npm install
    - ./node_modules/@angular/cli/bin/ng build

test:
  stage: test
  script:
    - npm install
    - ./node_modules/@angular/cli/bin/ng lint
    - ./node_modules/@angular/cli/bin/ng test --browsers PhantomJS --watch=false

deploy_staging:
  stage: deploy  
  script:
    - git remote add heroku https://heroku:$HEROKU_API_KEY@git.heroku.com/home-account-staging.git
    - git push heroku master
    - echo "Deployed to staging server"
  environment:
    name: staging
    url: https://nombre-de-tu-proyecto-staging.herokuapp.com/
  only:
    - tags

deploy_production:
  stage: deploy
  script:
    - git remote add heroku https://heroku:$HEROKU_API_KEY@git.heroku.com/home-account-prod.git
    - git push heroku master
    - echo "Deployed to production server"
  environment:
    name: production
    url: https://nombre-de-tu-proyecto.herokuapp.com/
  when: manual
  only:
    - tags
```

Así, cada vez que se haga un commit se realizarán las pruebas. Y cuando se cree la etiqueta se realizarán las pruebas y se desplegará de forma automática en tu sitio de [Heroku](http://heroku.com).

Si tienes más curiosidad de cómo se puede usar este fichero en integración y despliegue continuos en [Gitlab](https://gitlab.com), puedes verlo en [su página de documentacion](https://docs.gitlab.com/ee/ci/README.html).

Nos vemos en las próximas líneas.


