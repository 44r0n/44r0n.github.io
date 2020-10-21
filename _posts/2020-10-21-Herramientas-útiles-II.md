---
layout:  post
title: Herramientas útiles (II)


---

### Lectura de ~3 minutos

En el post de hace unos días [Herramientas útiles]({{ site.baseurl}}{% post_url 2020-10-07-Herramientas-útiles %}), os comenté que había encontrado una herramienta para controlar los mensajes de commit. Tras unos días de uso me di cuenta que es muy útil, pero empieza a complicarse cuando llevas varios días sin usarlo y no recuerdas la esctructura de los commits (cosa bastante fácil de ocurrir).

Así que en este caso entra en juego otra herramienta que se llama [Commitizen](https://github.com/commitizen/cz-cli). Esta herramienta nos permite realizar el **commit** siguiendo un wizzard en la consola y es totalmente compatible con [Commitlint](https://github.com/conventional-changelog/commitlint).

# Instalación y configuración

En la página [Commitizen](https://github.com/commitizen/cz-cli) recomiendan instalarlo  de forma global al sistema, pero yo prefiero instalarlo de forma local al proyecto, manías mías. El comando de instalación es el siguiente:

```shell
# Instalación global
$ npm install commitizen -g
# Instalación local
$ npm install commitizen --save-dev
```

Una vez instalado pasamos a configurar el repositorio con el siguiente comando:

```shell
# Instalación global
$ commitizen init cz-conventional-changelog --save-dev --save-exact
# Instalación local
$ npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

Como se indica en la documentación, este comando hace tres cosas por nosotros:

1. Instala el adaptador cz-convenvtional-changelog de npm

2. Lo guarda en el apartado *dependencies* o *dev-dependencies*.

3. Añade el siguiente fragmento de al fichero `packages.json`:

```json
"config": {
    "commitizen": {
      "path": "cz-conventional-changelog"
    }
  }
```

Una vez instalado lo podemos ejecutar :

```shell
# Instalación global
$ cz
# Instalación local
$ npx cz
```

Esto arracnará el wizzard y al rellenarlo creará el **commit** por nosotros.

Ahora podemos añadir [Commitizen](https://github.com/commitizen/cz-cli) a los scripts de **npm**:

```json
"scripts": {
    "commit": "cz"
}
```

Y lo podemos ejecutar siempre que lo necesitemos con:

```shell
$ npm run commit
```

Si bien esto nos abstrae de si la instalación es local o global nos saca del flujo de **git**, así que en la propia documentación nos proponen añadir un *hook* en **git** para que ejecute [Commitizen](https://github.com/commitizen/cz-cli), para ello debemos editar el fichero ``.git/hooks/prepare-commit-msg`:

```sh
#!/bin/bash
exec < /dev/tty && node_modules/.bin/cz --hook || true
```

Si has seguido el tutorial anterior, estamos usando [Husky](https://github.com/typicode/husky), así que en lugar de manipular el *hook* de **git** modificamos el fichero `package.json` en el apartado de *husky* para que quede así:

```json
"husky": {
    "hooks": {
      "prepare-commit-msg": "exec < /dev/tty && git cz --hook || true",
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
```

Así que ahora el ejecutar `git commit` se lanzará el **wizzard** y no salimos del flujo de **git**.

Espero que os sea de ayuda. Nos vemos en las próximas líneas.












