---
layout: post
title: Github pages con Jekyll (o como he montado esto)
---

#### Lectura de ~5 minutos

[Github](http://github.com) además de ser una plataforma para desarollo que fomenta el open soruce, ahora también ofrecen la posibilidad de alojar páginas web estáticas de forma gratuita utilizando su plataforma. Las denominadas [Github pages](https://pages.github.com/).
## ¿Cómo monto uno?
 Primero debes darte de alta en [su plataforma](http://github.com), si eres desarrollador y no conoces esta plataforma, deberías. Luego, tal y como indican en su tutorial, debes crear un repositiorio con nombre `tunombreususario.github.io`![Alta de nuevo sitio](https://pages.github.com/images/user-repo@2x.png  "Alta de nuevo sitio")
 
#### Imagen  de [Github pages](https://pages.github.com/)

Una vez creado debes descargar el repositorio creado, que a partir de ahora contendrá el código de nuestro sitio. Para poder descargarlo debes tener instalado git y ejecutar el siguiente comando:
~~~
$ git clone https://github.com/username/username.github.io
~~~
Una vez descargado tendrás un repositorio vacío en el que la rama `master` del mismo será el que sea publicado. Para probarlo crea un archivo llamado `index.html` con el siguiente contenido:
~~~
<!DOCTYPE html>
<html>
<body>
<h1>Hello World</h1>
<p>I'm hosted with GitHub Pages.</p>
</body>
</html>
~~~
Y súbelo a tu reposotirio con las siguientes instrucciones:
~~~
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
~~~
¡Y ya está! para verlo funcionando accede con tu navegador a `http://tunombreusuario.github.io.` y verás el resultado.
## Muy bonito, ahora ¿cómo hago que sea un blog?
Para montar un blog con este sistema tienes dos opciones:

- "Soy un/una super desarrollador/desarrolladora y  quiero maquetar yo mismo/misma el sitio." En este caso no sigas leyendo y ponte manos a la obra.

- "Necesito de una herramienta para montar el blog." Como los mortales que no pertenecemos al primer grupo sigue leyendo.

Existe una herramienta para montaje de bloging llamado [Jekyll](https://jekyllrb.com/) y que, ¡oh vaya! se integra perfectamente en **Github**. 
Tal y como indican en [el repositorio de Jekyll now](https://github.com/barryclark/jekyll-now), puedes hacer un *fork* de su repositorio, renombrarlo como se ha hecho anteriormente a `tunombreusuario.github.com` y ya debes poder acceder.
![Fork de Jekyll now](https://raw.githubusercontent.com/44r0n/44r0n.github.io/master/images/step1.gif  "Fork de Jekyll now")
#### Imagen de [Jekyll now](https://github.com/barryclark/jekyll-now) 
Lo único que falta es modificar el archivo de configuración `_config.yml` añadiendo tu nombre de usuario, tu avatar y configurar las redes solciales que deseas que aparezcan en el blog.
![Configuración del blog](https://raw.githubusercontent.com/44r0n/44r0n.github.io/master/images/config.png  "Configuración del blog")
#### Imagen de [Jekyll now](https://github.com/barryclark/jekyll-now) 
Ahora solamente falta... ¡Tu primer post! Para ello edita el archivo `/_posts/2014-3-3-Hello-World.md`. **Jekyll** utiliza para los posts la sintaxis *Markdown* te dejo un [enlace](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)  a su sintaxis.
![Primer post](https://raw.githubusercontent.com/44r0n/44r0n.github.io/master/images/first-post.png  "Primer post")

#### Imagen de [Jekyll now](https://github.com/barryclark/jekyll-now) 
### Comentarios
**Jekyll** no incorpora un sistema de comentarios para tus potenciales lectores. En su lugar puedes utilizar [disqus](http://disqus.com) para delegar el sistema de comentarios del blog. Para habilitarlo debes darte de alta en su sistema y crear un nuevo sitio. Una vez creado debes añadir el *shortname* en el archivo de configuración `_config.yml` en la zona correspondiente.
### Desarrollo en local
Todos los archivos se pueden editar en **Github**, pero todos los cambios que se realicen se actualizarán al momento. Si queires tener un entorno en el que poder realizar un desarrollo para poder 'trastear' antes de subirlo puedes instalar en tu ordenador un entorno de desarrollo. Necesiatrás **Ruby** instalado en tu equipo. Cuando lo tengas ejecuta el siguiente comando:
~~~
$ gem install jekyll github-pages
~~~
Una vez instalado accede a la carpeta de tu proyecto clonado y ejecuta jekyll.
~~~
$ cd ruta-a-tu-proyecto
$ jekyll s
~~~
Una vez lanzado accede a [http://127.0.0.1:4000/](http://127.0.0.1:4000/) y tendrás el proyecto en ejecución.

Y con esto ya podrás crear tu propio blog alojado en Github pages.

Si este post te parece un tostón y quieres ver como se realiza en un video puedes hacerlo en [youtube](https://www.youtube.com/watch?v=lsvRyE5tPQQ&t=681s)  gracias a **Antonio Leiva**.

Nos vemos en las próximas líneas.