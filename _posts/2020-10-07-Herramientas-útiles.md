---
layout:  post
title: Herramientas útiles


---

### Lectura de ~3 minutos

Siempre intento estar atento a las herramientas que utilizo para trabajar o para mis proyectitos personales. Y es verdad que nuna se puede uno enterar de todo y siempre habrán cosas que se escaparán. Pero también voy descubriendo cosas.

Hoy vengo a molestaros un [linter para los mensajes de los commit de git](https://github.com/conventional-changelog/commitlint).

# ¿Que es commitlint?

Para el que no lo sepa, un linter es un analizador de código que se usa para detectar código incorrecto.

Puede parecer una tontería, pero cuando un repositorio crece y hay que buscar algo en la historia del repositorio, nos guiamos por los mensajes de commit y si no están bien escritos puede llegar a ser un verdadero quebradero de cabeza. Con el linter obligamos a que todo el equipo use el mismo estándar en los mensajes de commit haciendo (o intentandolo) que los mensajes de commit sean más legibles. Si usamos esta herramienta, con la buena prácitca de ir realizando commits con pequeñas modificiaciones, la historia puede quedar clara y será más fácil buscar en la historia del repositorio.

Esta herramienta no es mágica, si bien fuerza a que los mensajes de commit tengan cierta estructura, igualmente se pueden redactar mensajes de commit confusos dejando una historia de git difícil de seguir. Commitlint puede ser una ayuda (o incordio) a la hora de escribir los mensajes de commit.

[Commitlint](https://github.com/conventional-changelog/commitlint) está orientado a proyectos web realizados con `npm`.

# Instalación y configuración

En [página de proyecto de github](https://github.com/conventional-changelog/commitlint) se indica como realizar la instalación. Para utilizarlo primero necesitaremos `npm` instalado. Para  instalarlo debemos ir al directorio raíz del proyecto y ejecutar los comandos de instalación en una terminal:

```shell
# Instalación de commitlint cli y configuración
$ npm install --save-dev @commitlint/{config-conventional,cli}
# Instalación para Windows
$ npm install --save-dev @commitlint/config-conventional @commitlint/cli

# Configuración de commitlint para usar la configuración convencional
$ echo "modules.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js
```

Si en este momento realizamos un `commit` no se ejecutaría el linter, ya que solamente tenemos configurado el estilo de los commits. Para poder ejecutarlo en cada `commit` local podemos instalar otro paquete `npm` llamado [Husky](https://github.com/typicode/husky), como la raza de perros. Para ello ejecutamos el siguiente comando de `npm`:

```shell
$ npm install husky --save-dev
```

Después creamos un fichero llamado `.huskyrc` o añadimos las siguientes líneas en la raíz del código `json` del fichero `package.json`:

```json
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

Ahora en cada `commit` que relicemos se ejecutará el `linter` y analizará el texto de mensaje del mismo. En caso que no se ajuste a la configuración rechazará el commit.

Hay varios tipos de configuración que se pueden encontrar en [la propia página del proyecto](https://github.com/conventional-changelog/commitlint) así como también puedes crear la tuya propia.

# Conclusiones

Como ya he comentado al principio, es una herramienta que nos puede ayudar a mantener los mensajes de la historia de `git` con un estándar, cosa que nos puede ayudar a realizar búsquedas o simplemente a leer la historia del repositorio. Pero también hay que tener cuidado en tener confianza absoluta en este tipo de herramientas, ya que puede crear un obstáculo en el equipo a la hora de realizar los commits.

Nos vemos en las próximas líneas.
