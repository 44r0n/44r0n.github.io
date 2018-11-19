---
layout:  post

title:  Primera versión de skeleton-styled


---

### Lectura de ~n minutos

Ha costado un poco, pero desde la semana pasada ya está disponible la primera versión de [skeleton-styled](https://github.com/44r0n/skeleton-styled). Tiene ya unas pocas descargas, 1k dice el badget. Lo que empezó siendo un side project para aprender como llevar un proyecto de puro front, siendo yo un completo ignorante de ese sector, se está convirtiendo en un proyecto que me está gustando más de lo que yo pensaba, y que me está enseñando muchísimo más de lo que sospechaba.

## Cosas aprendidas

Cabe señalar aquí, que el proyectó empezó siendo un *fork* de [**skeleton-flexbox**](https://github.com/andreobriennz/skeleton-flexbox), no he hecho yo personalmente todo el trabajo. Dicho lo cual, aquí va un pequeño listado de lo que me ha enseñado este proyectito:

- He aprendido superficialmente como funciona [**Gulpjs**](https://gulpjs.com) (ni siquiera lo había escuchado mencionar antes de empezar con esto), usado para preprocesar una cantidad mediana de ficheros [**scss**](https://sass-lang.com) y una ínfima cantidad de ficheros **js**. Aprendí a indicar en qué orden quiero que se procesen los ficheros, incluso indicar si se añaden ficheros nuevos que siga funcionando sin tener que modificar nada de la configuración. También aprendí a definir varias tareas, dependiendo de si me interesaba tener [**gulp**](https://gulpjs.com) a la escucha de modificaciones en el proyecto para reprocesar los ficheros, o simplemente hacer una pasada para la integración continua.

-  El proyecto original utiliza [**scss**](https://sass-lang.com), así que me puse a ojear cómo funciona. Como dice su web, es **css** vitaminado. Y depaso aprendí a tocar algo de **css**, antes de esto sabía algo, ahora sé un poquito más. Aprendí a hace animaciones interesantes mediante **css**, algo que sabía que se podía, pero no tenía ni idea de cómo. Os dejo un enlace de una web que encontré que recopila algunas animaciones interesantes: http://animista.net que está en beta, pero es una pasada.

- Vengo del mundo del back, así que de *javascript* me defiendo un poquito. No he querido meterle ningún *framework*, nada de [**jQuery**](https://jquery.com), [**Angular**](https://angular.io), [**React**](https://www.reactjs.org), [**Vue**](https://vuejs.org) y un largo etcétera puede ir aquí. La razón es simple: no quiero que decisiones de terceros comprometan el tamaño del proyecto, a pesar que el rendimiento pueda ser mejor. Quiero tocar el barro, equivocarme y aprender, que es de lo que se trata el proyecto. Estoy de acuerdo con la gente que piensa, y algunos me han dicho, que con estos *framework*, el trabajo sería más ligero y algunas cosas podrían funcionar mejor. De hecho mientras iba haciendo componentes iba pensando como sería en [**Angular**](https://angular.io) por ejemplo. Eso lo dejo para cuando esto esté completado o para quien quiera hacerlo, al fin y al cabo este es un proyecto bajo licencia [**MIT**](https://es.wikipedia.org/wiki/Licencia_MIT). 
  
- Aprendí a hacer integración continua con [**Github**](https://github.com) y [**Travis**](https://travis-ci.org), esto da para otra entrada. Básicamente aprendí a a sincronizar el repositorio de [**Github**](https://github.com) con el servicio de [**Travis**](https://travis-ci.org), para que éste último lo ejecutara lo mencionado en [**Gulpjs**](https://gulpjs.com) y publicara los ficheros correspondientes en [npm](https://www.npmjs.com) y en [Github releases](https://github.com/44r0n/skeleton-styled/releases) cada vez que yo creaba una *release*. Tengo pendiente realizar las pruebas automáticas, el problema es que no se me ocurre como hacerlas. Soy muy novato en eso de hacer pruebas automáticas en front.

Es muy probable que algunas cosas las haya hecho mal, y que estén mal. [Las *issue* de Github](https://github.com/44r0n/skeleton-styled/issues) siempre están abiertas para que se puedan proponer mejoras e incluso iniciar una colaboración. Así que si ves algo que te chirría te agradecería mucho una indicación para poder corregirlo.

## Estado actual

~~Ahora mismo~~ Cuando tengo tiempo, trabajo en la siguiente versión (1.1.0), no tiene fecha prevista de lanzamiento (estará lista cuando lo esté) y vendrá con alguna corrección de errores, que siempre los hay, y algunos componentes nuevos. Si estás leyendo esto y se te ocurre algún componente que pueda encajar lo dudes en [abrir una *issue*](https://github.com/44r0n/skeleton-styled/issues), no solo se puede colaborar añadiendo código, hay otras formas.



## Futuro

Quien sabe donde puede llegar el proyecto. Seguiré trabajando en él hasta que vea que no puedo o que no me sirve para seguir aprendiendo o... Lo que me ha sorprendido es ver como las descargas subían sin apenas hacerle publicidad. Si a alguien le sirve es que algo hace.



Nos vemos en las próximas líneas.


