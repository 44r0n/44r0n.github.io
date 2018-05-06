---
layout: post
title: Otro tutorial de Angular y Electron
---

#### Lectura de un buen retete

Este tutorial está basado en el que puedes encontrar [aquí](https://angular.io/tutorial). Vamos, que básicamente lo he leído, hecho y traducido. Lo que vas a leer a continuación es lo mismo que puedes encontrar allí. Este tutorial lo hago para que me sirva de fácil referencia y, si ya puestos le puede servir a alguien más, pues eso que nos llevamos todos.

# Primeros pasos

## Instalación

Para instalar angular debes tener primer instalado `npm` que normalmente viene instalado si instalas **Node**. Luego ejecuta el siguiente comando:

~~~terminal
$ npm install -g @angular/cli
~~~

## Crear una aplicación

Las aplicaciones también se crean a través de la consola. En la terminal ejecuta el siguiente comando:

~~~terminal
$ ng new angular-tour-of-heroes
~~~

Este comando crea un directorio nuevo con los ficheros iniciales y con un proyecto `git`local con el primer `commit` ya hecho con los ficheros del proyecto.

## Lanzar la aplicación

~~~terminal
$ cd angular-tour-of-heroes
$ ng serve --open
~~~

Entramos al directorio de la aplicación y la lanzamos. El comando `ng serve` construye la aplicación, inicia un servidor de desarrollo y se queda comprobando los ficheros para reconstruir el proyecto en caso de modificación. El parámetro `--open` abre el navegador por defecto a `http://localhost:4200` que es la dirección del proyecto construido.

Lo que has iniciado es el núcleo de la aplicación. Este núcleo es un componente. Estos componentes son bloques usados para construir aplicaciones **Angular**. Muestran información, recogen la información introducida por el usuario y realizan acciones dependiendo de la información introducida.

Abre el editor de código de tu preferencia y abre el directorio `src/app`, allí está el núcleo `AppComponente` compuesto por tres ficheros:

1. `app.component.ts` es la clase del componente escrita en **TypeScript**.
2. `app.component.html` es la plantilla del componente escrita en **HTML**.
3. `app.component.css` es el estilo privado del componente escrito en **CSS**.

Abre el fichero `app.component.ts` y cambia el valor de la variable title:

~~~ts
title = 'Tour of Heroes';
~~~

Abre la plantilla de componente `app.component.html`, borra el contenido generado y reemplazalo por:

~~~html
<h1>{% raw %}{{title}}{% endraw %}</h1>
~~~

**Angular** marca con dobles llaves la variables que se van a utilizar.

El fichero `styes.css` contiene el estilo para la aplicación. Añade el siguiente estilo al fichero:

~~~css
/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[text], button {
  color: #888;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
~~~

## Crear un componente

Para generar un componente nuevo se utiliza el comando `ng generate comnent`, en este caso cre un componente llamado heroes:

~~~terminal
$ ng generate component heroes
~~~

Se creará el directorio `src/app/heroes` y los tres ficheros. El fichero `heroes.component` debe ser como el que sigue:

~~~typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
~~~

Siempre se debe importar el símbolo `Component` mediante `@Component`. Se generan 3 propiedades de metadatos:

- selector: Es el selector **CSS** del componente.
- teplanteUrl: localización de la plantilla del componente.
- styleUrls: localización de los estilos **CSS** privados del componente.

El selector **CSS** del componente coincide con el nombre del identificador **HTML** que identifica el componente con un componente padre.

El método `ngOnInit` es un *hook* del ciclo de vida de **Angular** que es llamado después de crear un componente. Es el lugar idóneo para añadir lógica de inicialización. Siempre debe exportar la case así se puede importar en cualquier otro lugar para ser usada.

### Añadir una propiedad

Añade una propiedad a la clase héroe:

~~~typescript
hero = 'Windstorm';
~~~

### Mostrar la propiedad

Para mostrarlo abre la plantilla `heroes.component.html` y reemplaza el contenido con la nueva propiedad:

~~~html
{% raw %}{{hero}}{% endraw %}
~~~

### Muestra el componente

Para que el nuevo componente se muestre, debe ser añadido a la plantilla `AppComponent`. Añade la siguiente línea al fichero `src/app/app.component.html`:

~~~html
<app-heroes></app-heroes>
~~~

Si el comando `ng serve` está en funcionamiento, el navegador se actualizará automáticamente.

### Crear la clase Heroe

Un héroe no es solo un nombre. Crea la clase Hero en el directorio `src/app` y crea las propiedades `id` y `name`:

~~~typescript
export class Hero {
  id: number;
  name: string;
}
~~~

Vuelve al componente `HeroesComponent` y refactoriza la variable hero:

~~~typescript
hero: Hero = {
  id:1,
  name:'Windstorm'
};
~~~

La página no se mostrará correctamente, ahora la variable hero es un objeto.

### Mostrar el objeto hero

Abre la plantilla y muestra el `id` y `name`:

~~~html
<h2>{% raw %}{{hero.name}}{% endraw %}</h2>
<div><span>id: </span>{% raw %}{{hero.id}}{% endraw %}</div>
<div><span>name: </span>{% raw %}{{hero.name}}{% endraw %}</div>
~~~

Al guardar el fichero el navegador se actualizará y el héroe se mostrará correctamente.

### Formateo

Si quieres que el nombre del héroe aparezca en mayúsculas modifica la etiqueta de cabecera:

~~~html
<h2>{% raw %}{{hero.name | uppercase}}{% endraw %}</h2>
~~~

La palabra `uppercase` es una interpolación de la variable, que se utiliza a la derecha del operador `|`. Este tipo de operadores se utilizan para formatear texto, divisas, fechas, etc. **Angular** ofrece algunos por defecto pero siempre puedes crear más.

### Editar el héroe

Para editar el héroe utiliza un imput, este input deberá mostrar el nombre del héroe y modificarlo, de modo que la información fluye en dos sentidos, del objeto a la pantalla y de vuelta al objeto. `ng-Model` habilita esta característica. Abre el fichero `heroes.component.html` y añade el siguiente contenido:

~~~html
<div>
  <label>name: <input [(ngModel)]="hero.name" placeholder="name"/></label>
</div>
~~~

Ahora la página no funciona. Para ver el error abre las herramientas de desarrollador del navegador y mira en la consola de errores, debe aparecer el siguiente error:

~~~terminal
Template parse errors:
Can't bind to 'ngModel' since it isn't a known property of 'input'.
~~~

`ngModel` no está disponible por defecto. **Angular** necesita saber como encargan las piezas de la aplicación y que ficheros y librerías se necesitan. A esta información se le llama *metadatos*. Alguno ya lo hemos visto en `@Component`. Otros son los `@NgModule`. Los más importantes se indican en el nivel de `AppModule`. **Angular** crea una clase `AppModule` en `src/app/app.module.ts`. Ábrelo y el siguiente contenido:

~~~typescript
import {FormsModule} from '@angular/forms';
~~~

Y luego añádelo al array de metadatos imports:

~~~typescript
imports: [
  BrowserModule,
  FormsModule
],
~~~

Ahora, cuando el navegador refresque, podrás editar el nombre del héroe y los cambios se verán inmediatamente cambiados en pantalla.

Todo componente debe estar declarado en el módulo `NgModule`. A pesar de que esta vez ha funcionado sin que se haya añadido. Ha funcionado porque se ha generado a través de la línea de comandos. Comprueba que el componente está importado en el fichero `src/app/app.modules.ts`:

~~~typescript
import {HeroesComponent} from './heroes/heroes.component';
~~~

Y declarado en el array `@NgModule.declarations`:

~~~typescript
declarations: [
  AppComponent,
  HeroesComponent
],
~~~

`AppModule` declara tanto el `AppComponent` como el `HeroesComponent`.

## Mostrar un listado

Ahora que puedes editar y ver un héroe, es hora de crear un listado de héroes. Primero necesitas una colección de héroes para mostrarlos, en este caso y por el momento crearás un *mock* con un set de héroes definido. Para ello crea un fichero en el directorio `src/app` llamado `mock-heroes.ts` y define un *array* de 10 héroes. El fichero debe quedar como algo así:

~~~typescript
import {Hero} from './hero';
export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
~~~

Ahora que ya dispones de información para mostrar abre el fichero `heroes.component.ts` del directorio `src/app/heroes` e importa el listado que acabas de crear:

~~~typescript
import {HEROES} from '../mock-heroes';
~~~

Y añade la propiedad heroes a la clase:

~~~typescript
heroes = HEROES;
~~~

Abre la plantilla `HeroesComponent` y modificalo siguiendo los siguientes pasos:

1. Añade una etiqueta `<h2>` al inicio.
2. Justo a continuación añade una lista desordenada `<ul>`.
3. Inserta un elemento `<li>` a la lista que muestre las propiedades del héroe.
4. Añade las clases **css** para el estilo.

Debería quedar algo así:

~~~html
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <span class="badge">{% raw %}{{hero.id}}</span>{{hero.name}}{% endraw %}
  </li>
</ul>
~~~

Ahora modifica el elemento `<li>`:

~~~html
<li *ngFor="let hero of heroes">
~~~

`*ngFor` es la directiva de **Angular** para bucles, repite la etiqueta en la que está alojada para cada elemento en la lista. En este caso `<li>` es la etiqueta donde está alojada, `heroes` la lista de la clase `HeroesComponent` y `hero` contiene el héroe actual para cada iteración de la lista. **IMPORTANTE** el asterisco `*`.

Al refrescar el navegador debe aparecer el listado.

### Héroes con estilo

Este listado debería ser más atractivo. En el inicio del tutorial has definido en el fichero `styles.css` los estilos básicos para toda la aplicación, pero no incluye estilos para la lista de héroes. Puedes añadir los estilos en este mismo fichero, pero crecería a cada componente que añades. Puede que prefieras definir estilos privados para cada componente, así el código *HTML* y *CSS* están en el mismo directorio.

Esta forma hace que sea más sencillo reusar el componente y mostrarlo de forma intencionada diferente al estilo global. Los estilos se definen indistintamente en el array `@Component.styes` o en las hojas de estilo definidos en el array `Component.styleUrls`. Cuando se crea el componente, en este caso `HeroesComponent`, se crea un fichero `heroes.component.css` vacío y se añade al array `@Component.styleUrls`:

~~~typescript
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
~~~

Abre el fichero `heroes.component.css` y añade el siguiente contenido:

~~~css
/* HeroesComponent's private CSS styles */
.selected {
  background-color: #CFD8DC !important;
  color: white;
}
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes li.selected:hover {
  background-color: #BBD8DC !important;
  color: white;
}
.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}
.heroes .text {
  position: relative;
  top: -3px;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #607D8B;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}
~~~

### Mostrar los detalles

Al hacer click en un héroe de la lista, se debería mostrar los detalles. Empieza por añadir el evento `onclick` al elemento `<li>`:

~~~html
<li *ngFor="let hero of heroes" (clic)="onSelect(hero)">
~~~

Los paréntesis alrededor de `click` indican a **Angular** qye debe escuchar el evento `click` de los elementos `<li>`, y cuando el usuario haga click ejecute el método `onSelect` del componente `HeroesComponent`, que definirás a continuación, y le envía como parámetro el mismo heroe que se ha definido en el bucle `*ngFor`.

Abre el fichero `heroes.component.ts` y renombra `hero` a `selectedHero`, pero no lo asignes. No hay heroe seleccionado cuando se lanza la aplicación. Añade el siguiente método, que asigna el héroe clicado de la plantilla al componente `selectedHero`:

~~~typescript
selectedHero: Hero;
onSelect (hero: Hero): void {
  this.selectedHero = hero;
}
~~~

La plantilla todavía apunta a la propiedad hero, renombralo a selectedHero:

~~~html
{% raw %}
<h2>{{selectedHero.name | uppercase}} Details </h2>
<div><span>id: </span> {{selectedHero.id}} </div>
<div><label>name: <input [(ngModel)] = "selectedHero.name" placeholder="name"/></label></div>
{% endraw %}
~~~

## Componentes maestro/detalle

Hasta ahora el componente `HeroesComponent` muestra tanto la lista de héroes como el detalle del héroe seleccionado. Mantener todas las características en un componente en una aplicación que crece no es mantenible. Querrás separar componentes grandes en pequeños subcomponentes, cada uno realizando un trabajo concreto.

En este caso, primer paso es trasladar el componente de detalle a otro componente y reusarlo, así el componente `HeroesComponent` mostrará el detalle del héroe seleccionado.

Usa la consola de **Angular** para generar un nuevo componente llamado `hero-detail`:

~~~terminal
$ ng generate component hero-detail
~~~

A continuación corta el *HTML* que contiene el detalle en la plantilla `HeroesComponent` y pegalo en el nuevo fichero de plantilla `HeroDetailComponent`. El *HTML* pegado hace referencia a `selectedHero`, pero el nuevo componente puede mostrar cualquier héroe, así que reemplaza todos los `selectedHero` por `hero` en la plantilla. La plantilla debería parecerse a:

~~~html
{% raw %}
<div *ngIf="hero">
  <h2>{{hero.name | uppercase}} Details </h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label> name: <input [(ngModel)] = "hero.name" placeholder="name" /></label>
  </div>
</div>
{% endraw %}
~~~

Ahora debes añadir la propiedad `@Input` al héroe. La plantilla `HeroDetailComponent` se vincula a la propiedad del componente `hero` que es del tipo `Hero`. Abre la clase `HeroDetailComponent` e importa `Hero`:

~~~typescript
import {Hero} from '../hero';
~~~

La propiedad `hero` debe ser una propiedad `Input`, anotada con el decorador `@Input()`, porque `HeroesComponent` lo usa de forma externa del siguiente modo:

~~~html
<app-hero-detail [hero] = "selectedHero"></app-hero-detail>
~~~

Añade `@angular/core` para incluir el símbolo `Input` en el fichero `hero-detail-component.ts`:

~~~typescript
import {Component,OnInit,Input} from '@angular/core';
~~~

Ahora añade una propiedad `hero` precedido del decorador `@Input()`:

~~~typescript
@Input() hero: Hero;
~~~

Este es el único cambio que deberías hacer en la clase `HeroDetailComponent`. No hay más propiedades ni lógica de presentación. Este componente sólo recibe un objeto de tipo `hero` y lo muestra.

### Mostrar el detalle del subcomponente

`HeroesComponent` aún es una vista maestro detalle. Se usa para mostrar los detalles del héroe, antes de que lo cortaras de la plantilla. Ahora lo delega en `HeroDetailComponent`. Ambos componentes tienen una relación padre/hijo. El padre `HeroesComponent` controlará el hijo `HeroDetailComponent` enviando un héroe para mostrarlo cada vez que se haga *click* en un héroe de la lista. No hay que cambiar la clase `HeroesComponent` per sí su plantilla.

El selector `HeroDetailComponent` es `app-hero-detail`. Añade `<app-hero-detail>` cerca del final de la plantilla `HeroesComponent`, donde el detalle del héroe estaba.

~~~html
<app-hero-detail [hero] = "selectedHero"></app-hero-detail>
~~~

`[hero] = "selectedHero"` es la forma de vincular de **Angular**. Es un vínculo de una sola dirección, desde `HeroesComponent` a la propiedad `hero`. Cuando se haga *click* en un hero de la lista, `selectedHero` cambia. Cuando `selectedHero` cambie, la propiedad vinculada actualiza `hero` y `HeroDetailComponent` muestra el nuevo héroe. La plantilla `heroes.component.html` debería quedar parecido a:

~~~html
{% raw %}
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes"
      [class.selected] = "hero === selectedHero"
      (click)="onSelect(hero)">
      <span class="badge">{{hero.id}}</span>
      {{hero.name}}
  </li>
</ul>
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
{% endraw %}
~~~

Cuando el navegador actualice, la aplicación seguirá igual que antes.

Lo que ha cambiado es `HeroesComponent`. Se ha refactorizado en dos componente, que trae consigo los siguientes beneficios:

1. Se ha simplificado `HeroesComponent`, reduciendo sus responsabilidades.
2. `HeroDetailComponent` puede evolucionar sin tener que modificar `HeroesComponent`.
3. Y viceversa, `HeroesComponent` puede evolucionar sin modificar `HeroDetailComponent`.
4. Puedes reusar `HeroDetailComponent` en un componente futuro.

## Servicios

Actualmente `HeroesComponent` está obteniendo y mostrando datos de ejemplo.

### ¿Por qué servicios?

Los componentes no deberían obtener o guardar información directamente y tampoco mostrar información de ejemplo. Se deben centrar en mostrar información y delegar el acceso de la información a un servicio.

Para crear un servicio, en lugar de usar `new`, usarás la inyección de dependencias para inyectarlo en el constructor `HeroesComponent`. Los servicios se deben usar para compartir información entre clases que no se conocen. Crea `MessageService` e inyectalo en dos lugares:

1. En `HeroService` que usa el servicio para enviar un mensaje.
2. En `MessagesComponent` que muestra el mensaje.

### Crear el servicio

Para crear `HeroService`, generalo desde la terminal:

~~~terminal
$ ng generate service hero
~~~

Este comando genera el esqueleto `HeroService` en `src/app/hero.service.ts` que debería ser:

~~~typescript
import {Injectable} from '@angular/core';

@Injectable()
export class HeroService {
  constructor() {}
}
~~~

Observa que el servicio importa `Injectable` de **Angular** y anota la clase con el decorador `@Injectable`. Este decorador le dice a **Angular** que el servicio puede tener dependencias inyectadas. En este momento no las tiene, pero las tendrá. Tanto si las tiene como si no, es buena práctica mantener el decorador.

### Obtener la información

`HeroService` puede obtener la información desde cualquier lugar, un servicio web, una base de datos local o un mock. Quitando el acceso a la información desde los componentes puedes cambiar la implementación sin cambiar ningún componente. Para este caso utilizaremos un mock. Importa `Hero` y `HEROES` en `HeroService`.

~~~typescript
import {Hero} from './hero';
import {HEROES} from './mock-heroes';
~~~

Añade el método `getHeroes`:

~~~typescript
getHeroes(): Hero[] {
  return HEROES
}
~~~

Debes proveer el servicio `HeroService` al sistema de inyección de dependencias antes de que **Angular** pueda inyectarlo a `HeroesComponent`. Hay varias formas de hacerlo: en `HeroesComponent`, en `AppComponent` y en `AppModule`. Cada uno tiene ventajas y desventajas. Aquí usaras `AppModule`. Está elección es popular y se puede generar desde el terminal:

~~~terminal
$ ng generate service hero --module=app
~~~

Como en el paso anterior no se ha indicado, lo debes introducir de forma manual. Abre la clase `AppModule`, importa `HeroService` y añadelo al array `@NgModule.providers`:

~~~TypeScript
providers: [
  HeroService,
  /* ... */
],
~~~

`Providers` indica a **Angular** que cree una instancia única y compartida de `HeroService` y que lo inyecte a cada clase que lo pida.

Ahora abre la clase `HeroesComponent`, borra la importación `HEROES` y sustituyela por `HeroService`:

~~~typescript
import {HeroService} from '../hero.service';
~~~

Reemplaza la definición `heroes`:

~~~typescript
heroes: Hero[];
~~~

Para inyectar el servicio añade un parámetro privado al constructor:

~~~typescript
constructor (private heroService: HeroService) {}
~~~

El parámetro se define como una propiedad privada y se identifica como una inyección de `HeroService`. Cuando **Angular** cree un `HeroesComponent`, el sistema de inyección de dependencias establecerá el parámetro `heroService` a la instancia única de `HeroService`.

Ahora crea una función para recibir heroes del servicio:

~~~typescript
getHeroes(): void{
  this.heroes = this.heroService.getHeroes();
}
~~~

Esta función puede ser llamada desde el constructor, pero no es una buena práctica. Utiliza el constructor para la inicialización simple, como por ejemplo, vincular los parámetros del constructor a propiedades. El constructor no debería hacer nada. En lugar de esto llama a `getHeroes()` desde ek ciclo de vida `ngOnInit` y deja que **Angular** llame a `ngOnInit` en el momento apropiado después de construir la instancia `HeroesComponent`:

~~~typescript
ngOnInit() {
  this.getHeroes();
}
~~~

Cuando el navegador actualice, la aplicación debería funcionar como hasta ahora.

### Observable data

El método `HeroService.getHeroes()` obtiene los heroes de forma asíncrona. `HeroesComponent` usa `getHeroes` como si se obtuviera de forma síncrona, `this.heroes = this.heroService.getHeroes();`. Esto no funcionará en una aplicación real. Ahora mismo funciona porque se obtienen heroes mock. La aplicación se modificará para obtener heroes de un servidor remoto, cuya operación es asíncrona. `HeroService` debe esperar a que el servidor responde, `getHeroes()` no puede devolver la información de forma inmediata, y el navegador no se debe bloquear en las esperas del servicio. `HeroService.getHeroes()` debe ser de alguna forma asíncrono. Se puede hacer mediante *callback*, que devuelva una promesa, o un *Observable*. En este tutorial, `HeroService.getHeroes` devolverá un *Observable*.

*Observable* es una clase de `RxJS`. Más adelante verás que los métodos `HttpClient` devuelven *Observables* de `RxJS`. Ahora simularás esa respuesta con la función `of()`. Abre `HeroService` e importa `Observable` y `of` de `RxJS`:

~~~typescript
import {Observable} from 'rxjs/Observable';
import {of} from 'rxjs/observable/of';
~~~

Reemplaza el método `getHeroes`:

~~~typescript
getHeroes(): Observable<Hero[]>{
  return of(HEROES);
}
~~~

`of(HEROES)` devuelve `Observable[Hero]>` que tiene un solo valor, el array de mock de heroes. Ahora que `HeroService.getHeroes` devuelve `Observable<Hero[]>` modifica `HeroesComponent`, reemplaza `getHeroes`:

~~~typescript
getHeroes(): void {
  this.heroService.getHeroes().subscribe(heroes => this.heroes = heroes);
}
~~~

La diferencia está en `Observable.subscribe()`. En la versión anterior se asigna el array de héroes a la propiedad de forma síncrona como si el servidor devolviera héroes de forma instantánea o el navegador bloqueara la interfaz mientras espera la respuesta. En la nueva versión se espera a que `Observable` emita el array de heroes, que podría suceder en cualquier momento. Luego `subscribe` pasa el array al `callback`, que asigna la propiedad heroes del componente. Esta aproximación funcionará cuando `HeroService` haga las peticiones al servidor.

### Mensajes

En la refactorización hemos dejado los mensajes olvidados. Volverás a habilitarlos utilizando los servicios. Primero has de crear el componente, abre el terminal y ejecuta el siguiente comando:

~~~terminal
$ ng generate componente messages
~~~

Se generarán los ficheros de componente en el directorio `src/app/messages` y se declarará `MessagesComponente` en `AppModule`. Modifica `AppComponent` para que muestre `MessagesComponent`:

~~~html
{% raw %}
<h1>{{title}}</h1>
<app-heroes></app-heroes>
<app-messages></app-messages>
{% endraw %}
~~~

Si actualizas el navegador verás el párrafo por defecto al final de la página. Ejecuta la siguiente instrucción en la terminal para crear `MessageService`:

~~~terminal
$ ng generate service message --module=app
~~~

La opción `--module=app` indica que debe proveer este servicio en `AppModule`. Abre `MessageService` y reemplaza el contenido por el siguiente:

~~~typescript
import {Injectable} from '@angular/core';

@Injectable()
export class MessageService{
  messages: string [] = [];
  add(message: string) {
    this.messages.push(message);
  }
  clear() {
    this.messages = [];
  }
}
~~~

El servicio expone su caché de mensajes y dos métodos: uno para añadir, `add()` y otro para limpiar `clear()` la caché de mensajes. Vuelve a abrir `HeroService` e importa el servicio `MessageService`:

~~~typescript
import {MessageService} from './message.service';
~~~

Modifica el constructor con el parámetro privado `messageService`. **Angular** inyectará un objeto `MessageService` único a la propiedad:

~~~typescript
constructor (private messageService: MessageService) {}
~~~

Modifica el método `getHeroes` para enviar un mensajes cuando se obtienen los héroes:

~~~typescript
getHeroes(): Observable<Hero[]> {
  //TODO: send the message -after- fetching the heroes
  this.messageService.add('HeroService: fetched heroes');
  return of(HEROES);
}
~~~

`MessagesComponent` debería mostrar todos los mensajes enviados por `HeroService` cuando obtiene los heroes. Abre `MessagesComponent` e importa `MessageService`:

~~~typescript
import {MessageService} from '../message.service';
~~~

Modifica el constructor y añade un parámetro público `messageService`. **Angular** inyectará cuando cree un `HeroService`:

~~~typescript
constructor(public messageService: MessageService) {}
~~~

`messageService` debe ser público porque se va a usar en la plantilla. Abre `messages.component.html` y modificalo con lo siguiente:

~~~html
<div *ngIf="messageService.messages.length">
  <h2>Messages</h2>
  {% raw %}
  <button class="clear"
          (click)="messageService.clear()">Clear</button>
  <div *ngFor='let message of messageService.messages'>{{message}}</div>
  {% endraw %}
</div>
~~~

La plantilla se vincula directamente a `messageService`:

- `*ngIf` solo muestra los mensajes si hay mensajes que mostrar.
- `*ngFor` hace un bucle con todos los mensajes y los muestra en elementos `div` separados.
- La vinculación de eventos de **Angular**, vincula el `click` del botón a `MessageService.clear();`

Los mensajes tendrán mejor apariencia con el siguiente *css*:

~~~css
/* MessagesComponent's private CSS styles */
h2 {
  color: red;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[text], button {
  color: crimson;
  font-family: Cambria, Georgia;
}

button.clear {
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #aaa;
  cursor: auto;
}
button.clear {
  color: #888;
  margin-bottom: 12px;
}
~~~

Al actualizar el navegador, la página muestra los heroes. Abajo podrás ver los mensajes de `HeroService`. Haz click en el botón 'Clear' para hacer desaparecer los mensajes.

## Enrutado

A la aplicación le falta un poco de navegación, a continuación la modificarás para que puedas navegar del siguiente modo:
![NAVDiagram]({{ site.baseurl}}/images/nav-diagram.png){:class="img-responsive"}

Una buena práctica en **Angular** es cargar y configurar el enrutador en módulo separado. Por convenio, el nombre de la clase es `AppRoutingModule` que se encuentra en `app-routing.module.ts` en el directorio `src/app`. Escribe el siguiente comando en un terminal:

~~~terminal
$ ng generate module app-routing --flat --module=app
~~~

`--flat` indica que se debe poner en el directorio `src/app` en lugar de us propio directorio. `--module=app` lo registra en el array de imports de `AppModule`. El fichero generado debe ser algo así:

~~~typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';

@NgModule({
  imports: [
    CommonModule
  ],
  declarations: []
})
export class AppRoutingModule { }
~~~

Normalmente no se declara componentes en un componente de enrutado, así que puedes eliminar `@NgModule.declarations` y las referencias `CommonModule` también. Configurarás el enrutador con `Routes` en `RoyterModule` así que se debe importar desde a librería `@angular/router`. Añade un array `@NgModule.exports` en `RouterModule`, hace que las directivas `RouterModule` estén disponibles en los componentes `AppModule` que los necesiten. `AppRoutingModule` debe ser como el siguiente:

~~~TypeScript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';

@NgModule({
  exports: [ RouterModule ]
})
export class AppRoutingModule {}
~~~

### Añadiendo rutas

`Routest` indica al enrutador que vista debe mostrar cuando el usuario hace click en un enlace o escribe una *URL* en la barra de direcciones. Un `Route` de **Angular** tiene las siguientes dos propiedades:

1. `path`: un texto que coincide con la *URL* del navegador.
2. `component`: el componente que el enrutador debe crear cuando se navega a esta ruta.

Cuando intentas navegar a `HeroesComponent` la *URL* es algo parecido como `localhost:4200/heroes`. Importa `HeroesComponent` y referencialo en `Route`. Luego define un array de rutas con un único `route` al componente:

~~~typescript
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
~~~

Una vez has acabado, el enrutador emparejará esa *URL* con `path: 'heroes'` y mostrará `HeroesComponent`.

Antes de usarlo debes inicializarlo y comprobar si la localización del navegador cambia. Añade `RouterModule` al array `@NgModule.imports` y configuralo con `routes` en un solo paso llamando a `RouterModule.forRoot()`:

~~~typescript
imports: [ RouterModule.forRoot(routes) ],
~~~

El método se llama `forRoot()` porque configuras el enrutador a nivel de raíz.

Abre la plantilla `AppComponent` y reemplaza `<app-heroes>` por `<router-outlet>`:

~~~HTML
{% raw %}
<h1>{{title}}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
{% endraw %}
~~~

Debes borrar `<app-heroes>` porque no muestras `HeroesComponent` cuando el usuario navega. `<router-outlet>` indica al enrutador que vista debe mostrar. Ejecuta el comando `neg serve` y navega al sitio. El navegador debe mostrar el título de la aplicación pero no el listado de héroes. La *URL* del navegador debe acabar en `/`, la ruta a `HeroesComponent` es `/heroes`.

### Rutas de enlace

Los usuarios no deben introducir *URL* en la barra de navegación. Deberían poder hacer click en los enlaces para navegar. Añade un elemento `<nav>` con un `<a>` en su interior para que se pueda navegar a `HeroesComponent`. El fichero `AppComponent` debe ser como el siguiente:

~~~html
<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
~~~

Un atributo de tipo `router Link` se establece a `"/heroes"`, es el texto que se empareja a la ruta a `HeroesComponent`. `router link` es el selector para la directiva `router link` que torna los clicks de usuario a navegaciones enrutadas. Cuando el navegador actualice, mostrará el titulo de la aplicación y el enlace, pero no la lista. Haz click en el enlace, la barra de navegación debe actualizarse a `/heroes` y debe aparecer el listado de héroes.

### Vista de tablero

El enrutado cobra sentido cuando hay varias vistas. Pero ahora mismo solo hay una. Añade `DashboardComponent` usando la terminal:

~~~terminal
$ ng generate component dashboard
~~~

Se generan los ficheros `DashboardComponent` y se declara en `AppModule`. Reemplaza el contenido de los tres ficheros al siguiente. `src/app/dashboard/dashboard.component.html`:

~~~html
{% raw %}
<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <a *ngFor="let hero of heroes" class="col-1-4">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>
{% endraw %}
~~~

`src/app/dashboard/dashboard.compnent.ts`

~~~typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
~~~

`src/app/dashboard/dashboard.component.css`
~~~css
/* DashboardComponent's private CSS styles */
[class*='col-'] {
  float: left;
  padding-right: 20px;
  padding-bottom: 20px;
}
[class*='col-']:last-of-type {
  padding-right: 0;
}
a {
  text-decoration: none;
}
*, *:after, *:before {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
h3 {
  text-align: center; margin-bottom: 0;
}
h4 {
  position: relative;
}
.grid {
  margin: 0;
}
.col-1-4 {
  width: 25%;
}
.module {
  padding: 20px;
  text-align: center;
  color: #eee;
  max-height: 120px;
  min-width: 120px;
  background-color: #607D8B;
  border-radius: 2px;
}
.module:hover {
  background-color: #EEE;
  cursor: pointer;
  color: #607d8b;
}
.grid-pad {
  padding: 10px 0;
}
.grid-pad > [class*='col-']:last-of-type {
  padding-right: 20px;
}
@media (max-width: 600px) {
  .module {
    font-size: 10px;
    max-height: 75px; }
}
@media (max-width: 1024px) {
  .grid {
    margin: 0;
  }
  .module {
    min-width: 60px;
  }
}
~~~

La plantilla presenta un tablero de nombres de héroe que son enlaces.

- `*ngFor` crea tantos enlaces como componentes hay en el array `heroes`.
- Los enlaces tienen estilo de bloques coloreados por el fichero `dashboard.component.css`.
- Los enlaces no dirigen a ningún sitio, lo harán en breve.

La clase es parecida a `HeroesComponent`.

- Define un array de `heroes`.
- El constructor espera que **Angular** inyecte `HeroService` en una propiedad privada `heroService`.
- El ciclo de vida `ngOnInit()` llama a `getHeroes`.

Este `getHeroes` redice el número de héroes mostrados a 4:

~~~typescript
getHeroes(): void {
  this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes.slice(1, 5));
}
~~~

Para navegar a este tablero, el enrutador necesita la ruta apropiada. Importa `DashboardComponent` en `AppRoutingModule`:

~~~typescript
import { DashboardComponent }   from './dashboard/dashboard.component';
~~~

Añade la ruta al array `AppRoutingModule.routes` que enlace con `DashboardComponent`:

~~~typescript
{ path: 'dashboard', component: DashboardComponent },
~~~

### Ruta por defecto

Cuando la aplicación se inicia, la dirección a la que apunta el navegador es la raíz del sitio. Esto no coincide con una ruta existente así que el enrutador no navega a ningún sitio. El espacio debajo de `<router-outlet>` está en blanco. Para hacer que la aplicación navegue al panel de forma automática, añade lo siguiente al array `AppRoutingModule.Routes`:

~~~typescript
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
~~~

Esta ruta redirecciona una *URL* que coincide con la ruta vacía cuya dirección es `/dashboard`. Cuando el navegador actualice, el enrutador cargará `DashboardComponent` y debe mostrar en la barra de navegación la *URL* `/dashboard`.

### Vuelta atrás

El usuario debería poder navegar en ambos sentidos entre `DashboardComponent` y `HeroesComponent` haciendo click en botones de navegación. Añade un enlace en `AppComponent`, encima del enlace Heroes:

~~~html
<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
~~~

Cuando el navegador actualice podrás navegar libremente entre las dos vistas haciendo click en los enlaces.

Ahora mismo `HeroDetailComponent` muestra el detalle de un héroe seleccionado y solamente es visible al final de `HeroesComponent`. El usuario debería poder acceder a este detalle de tres maneras:

1. Haciendo click en el héroe del tablero.
2. Haciendo click al héroe de la lista.
3. Introduciendo la *URL* en la barra de navegación.

El listado de héroes no necesita el detalle del mismo, por lo que abre el fichero `heroes/heroes.component.html` y elimina `<app-hero-detail>`. Con este cambio si se hace click en un elemento de héroe no hace nada. Se solucionará en breve.

### Rutas detalle

*URL* tipo `~/detail/11` son un buen tipo de *URL* para navegar al detalle de héroe. Abre `AppRoutingModule` e importa `HeroDetailComponent`:

~~~typescript
import { HeroDetailComponent }  from './hero-detail/hero-detail.component';
~~~

Luego añade una ruta parametrizada al array `AppRoutingModule.routes`:

~~~typescript
{ path: 'detail/:id', component: HeroDetailComponent },
~~~

Los dos puntos en una ruta indica que `:id` es un parámetro específico de héroes. A estas alturas, las rutas de aplicación deberían ser:

~~~typescript
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes', component: HeroesComponent }
];
~~~

Ahora que se puede acceder al detalle del héroe por enrutado puedes habilitar esta navegación desde el panel de héroes. Abre `src/app/dashboard/dashboard.compo.html` y añade el `routerLink` al enlace:

~~~html
{% raw %}
<a *ngFor="let hero of heroes" class="col-1-4"
    routerLink="/detail/{{hero.id}}">
{% endraw %}
~~~

Modifica también en el listado de héroes en `src/app/heroes/heroes.component.html`, ahora es un elemento `<li>` en el que al hacer click llama al método `onSelect()`:

~~~html
{% raw %}
<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
{% endraw %}
~~~

Se debe cambiar, el elemento `<li>` no debe realizar esa llamada, solamente debe contener el enlace, sustituye el código anterior por:

~~~html
{% raw %}
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>
{% endraw %}
~~~

Al realizar estos cambios el estilo privado deja de funcionar, para solucionarlo modifica el fichero `heroes.component.css`:

~~~css
/* HeroDetailComponent's private CSS styles */
label {
  display: inline-block;
  width: 3em;
  margin: .5em 0;
  color: #607D8B;
  font-weight: bold;
}
input {
  height: 2em;
  font-size: 1em;
  padding-left: .4em;
}
button {
  margin-top: 20px;
  font-family: Arial;
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer; cursor: hand;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}
~~~

`HeroesComponent` funciona perfectamente, pero el método `onSelect()` y la propiedad `selectedHero` ya no van a seguir en uso, elimina ese código. Siempre es bueno tener el código limpio, lo agradecerás más tarde. La clase debería quedar como lo siguiente:

~~~typescript
export class HeroesComponent implements OnInit {
  heroes: Hero[];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}
~~~

### HeroDetailComponent enrutable

Antes, el padre `HeroesComponent` establecía el valor de la propiedad `HeroDetailComponent.hero` para mostrarlo. Ya no lo hace. Ahora el enrutador crea `HeroDetailComponent` en respuesta a una *URL* tipo `~/detail/11`. `HeroDetailComponent` necesita una nueva forma de obtener `hero-to-display`.

- Debe obtener la ruta que lo creó.
- Debe extraer el `id` de la ruta.
- Debe obtener el héroe con ese `id` del servicio via `HeroService`.

Añade los siguientes `imports` a `hero-detail.component.ts`:

~~~typescript
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService }  from '../hero.service';
~~~

Inyecta los servicios `ActivatedRoute`, `HeroService` y `Location`  a esta instancia.

~~~typescript
constructor(
  private route: ActivatedRoute,
  private heroService: HeroService,
  private location: Location
) {}
~~~

Este componente está interesado en los parámetros de la ruta. El parámetro "id" es el `id` del héroe a mostrar. `HeroService` obtiene la información del héroe de un servidor remoto y este componente lo usará para mostrarlo. `Location` es un servicio de **Angular** que interactúa con el navegador. Lo verás más adelante.

En el ciclo de vida `ngOnInit()` llama a `getHero()` y defínelo tal que así:

~~~typescript
ngOnInit(): void {
  this.getHero();
}

getHero(): void {
  const id = +this.route.snapshot.paramMap.get('id');
  this.heroService.getHero(id)
    .subscribe(hero => this.hero = hero);
}
~~~

`route.snapshot` es una imagen estática de la información de la ruta. `paramMap` es un diccionario de ritas de parámetros y valores extraídos de la *URL*. La clave `"id"` devuelve el `id` del héroe que se debe obtener. Los parámetros de las rutas son siempre texto. El operador (+) de JavaScript convierte el texto a numérico, que es lo que el `id` del héroe debería ser. Si actualizas en el navegador el compilador debe dar un error. `HeroService` no tiene un método `getHero()`. Añádelo:

~~~typescript
getHero(id: number): Observable<Hero> {
  // TODO: send the message _after_ fetching the hero
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(HEROES.find(hero => hero.id === id));
}
~~~

Igual que `getHeroes()`, `getHero()` es un método asíncrono. Devuelve el mock de héroes como un `Observable` usando la función `of()` de RxJS. Debes de ser capaz de reimplementar `getHero()` como una llamada real de `Http` sin tener que cambiar la llamada de `HeroDetailComponent`.

Cuando actualice el navegador la aplicación ya debe volver a funcionar. Si haces click en un héroe del tablero navegarás a la información detalle del héroe. Si escribes la *URL* en el navegador, por ejemplo `localhost:4200/detail/11`, el enrutador navegará a la información detalle del héroe con id 11.

### Encontrando en camino de vuelta

Haciendo click en el botón atrás del navegador puedes volver al listado desde el detalle o al tablero, dependiendo desde dónde hayas navegado. Estaría bien que un botón en la vista `HeroDetail` pudiera hacer eso. Añade un botón de vuelta atrás en la plantilla del componente y vinculalo al método `goBack()`.

~~~html
<button (click)="goBack()">go back</button>
~~~

El método `goBack()` navega atrás un paso en el historial del navegador usando el servicio `Location` que has inyectado anteriormente:

~~~typescript
goBack(): void {
  this.location.back();
}
~~~

Actualiza el navegador y empieza ha navegar por la aplicación. Como usuario debes de ser capaz de navegar por toda la aplicación sin tener que escribir *URL* ni hacer click en el botón atrás del navegador.

## Http

Ahora mismo, lo que le falta a la aplicación es persistencia. Lo conseguirás mediante los siguientes pasos:

- `HeroService` obtiene la información con peticiones *HTTP*.
- Los usuarios deben poder añadir, editar y eliminar héroes y guardar estos cambios vía *HTTP*.
- Los usuarios deben poder buscar héroes por su nombre.

### Habilitando servicios HTTP

`HttpClient` es un mecanismo de **Angular** para comunicarse con un servidor remoto a través de *HTTP*. Para que `HttpClient` esté disponible en toda la aplicación sigue los siguientes pasos:

- Abre el fichero raíz `AppModule`.
- Importa `HtppClientModule` desde `@angular/common/http`.
- Añádelo al array `@NgModule.imports`.

### Simulando la información en el servidor

En este tutorial simularemos un servidor que contiene la información usando el módulo `In-memory Web API`. Después de instalar el módulo, la aplicación hará peticiones y recibirá respuestas de `HttpClient` sin saber que `In-memory Web API` está interceptando esas peticiones. Esta funcionalidad viene muy bien para el tutorial. No tienes que configurar un servidor. También es conveniente realizar este tipo de desarrollo en las etapas tempranas, cuando el api del servidor no está totalmente definido o no está implementado. Instala el paquete `In-memory Web API` desde `npm`. La versión está bloqueada a `v0.5` para mantener la compatibilidad actual con `@angular/cli`.

~~~terminal
$ npm install angular-in-memory-web-api@0.5 --save
~~~

Importa `HttpClientInMemoryWebApiModule` y `InMemoryDataService`:

~~~typescript
import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService }  from './in-memory-data.service';
~~~

Añade `HttpClientInMemoryWebApiModule` al array `@NgModule.imports`:

~~~typescript
HttpClientModule,

// The HttpClientInMemoryWebApiModule module intercepts HTTP requests
// and returns simulated server responses.
// Remove it when a real server is ready to receive requests.
HttpClientInMemoryWebApiModule.forRoot(
  InMemoryDataService, { dataEncapsulation: false }
)
~~~

El método de configuración `forRoot()` obtiene una clase `InMemoryDataService` que prima la base de datos en memoria. Crea una clase en `src/app/in-memory-data.service.ts` que tenga el siguiente contenido:

~~~typescript
import { InMemoryDbService } from 'angular-in-memory-web-api';

export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
      { id: 11, name: 'Mr. Nice' },
      { id: 12, name: 'Narco' },
      { id: 13, name: 'Bombasto' },
      { id: 14, name: 'Celeritas' },
      { id: 15, name: 'Magneta' },
      { id: 16, name: 'RubberMan' },
      { id: 17, name: 'Dynama' },
      { id: 18, name: 'Dr IQ' },
      { id: 19, name: 'Magma' },
      { id: 20, name: 'Tornado' }
    ];
    return {heroes};
  }
}
~~~

Este fichero reemplaza `mock-heroes.ts`, que puedes eliminar ahora. Cuando el tengas un servidor api listo, podrás desacoplar `In-memory Web API`, y las peticiones de la aplicación irán al servidor.

### HTTP Heroes

Importa los símbolos *HTTP* que necesitas en `hero.services.ts`:

~~~typescript
import { HttpClient, HttpHeaders } from '@angular/common/http';
~~~

Inyecta `HtppClient` al constructor en una propiedad privada llamada `http`:

~~~typescript
constructor(
  private http: HttpClient,
  private messageService: MessageService) { }
~~~

Mantén inyectado el servicio `MessageService`:

~~~typescript
/** Log a HeroService message with the MessageService */
private log(message: string) {
  this.messageService.add('HeroService: ' + message);
}
~~~

Define la variable `heroesUrl` con la dirección de héroes del servidor:

~~~typescript
private heroesUrl = 'api/heroes';  // URL to web api
~~~

Actualmente `HeroService.getHeroes()` usa la función `of()` de RxJS para obtener el array de héroes:

~~~typescript
getHeroes(): Observable<Hero[]> {
  return of(HEROES);
}
~~~

Convierte ese método para usar `HttpClient`:

~~~typescript
/** GET heroes from the server */
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
}
~~~

Actualiza el navegador. La información de los héroes debe cargarse correctamente desde el servidor mock. Has cambiado `of` por `http.get` y la aplicación sigue funcionando porque ambos devuelve un `Observable<Hero[]>`.

Todos los métodos `HttpClient` devuelven un `Observable` RxJs de algo. *HTTP* es un protocolo de petición/respuesta. Haces una petición y devuelve una única respuesta. En general, `Observable` puede devolver varios valores en el tiempo. Un `Observable` de `HttpClient` siempre emitirá un único valor y luego se completa, nunca se emite otra vez. Este `HttpClient.get` en particular, devuelve un `Observabble<Hero[]>`, literalmente un array de héroes observable. En la práctica es un único array de héroes.

### Cuando las cosas no van bien

Cuando se realizan llamadas a un servidor remoto, las conexiones pueden fallar, por lo que `HeroService.getHeroes()` debe estar preparado para esos errores y actuar en consecuencia. Para capturar los errores y los debes canalizar desde `http.get()` a través de un operador `catchError()`de RxJS. Importa el símbolo `catchError` desde `rxjs/operators` y otros que necesitarás más tarde:

~~~typescript
import { catchError, map, tap } from 'rxjs/operators';
~~~

Ahora extiende el resultado con el método `pipe()`y dale un operador `catchError()`:

~~~typescript
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      catchError(this.handleError('getHeroes', []))
    );
}
~~~

El operador `catchError()`intercepta un `Observable` que ha fallado. Pasa el error a un manejador que puede hacer lo que desee con ese error. La función `handleError()` lo reporta y devuelve un resultado inocuo para que la aplicación siga funcionando.

El siguiente `errorHandler()` será compartido por varios métodos `HeroService` por lo que está generalizado. En lugar de manejar el error directamente, devuelve una función de manejador de error a `catchError` que ha sido configurado tanto para el nombre de la operación que ha fallado como para devolver un valor seguro.

~~~typescript
/**
 * Handle Http operation that failed.
 * Let the app continue.
 * @param operation - name of the operation that failed
 * @param result - optional value to return as the observable result
 */
private handleError<T> (operation = 'operation', result?: T) {
  return (error: any): Observable<T> => {

    // TODO: send the error to remote logging infrastructure
    console.error(error); // log to console instead

    // TODO: better job of transforming error for user consumption
    this.log(`${operation} failed: ${error.message}`);

    // Let the app keep running by returning an empty result.
    return of(result as T);
  };
}
~~~

Después de reportar el error a la consola, el manejador construye un mensaje amigable y devuelve un valor seguro a la aplicación para que siga funcionando. Como cada método del servicio devuelve distintos tipos de `Observable`, `errorHandler()` toma un tipo de parámetro para que pueda retornar un tipo seguro a la aplicación.

Los métodos `HeroService` entrarán en el flujo de los valores observables y enviarán un mensaje, a través de `log()`, al área de mensajes al final de la página. Se realizará a través del operador `tap`, que mira los valores observables, hace algo con esos valores y los devuelve. El operador `tap` no toca los valores en sí mismos. A continuación está la versión final del método `getHeroes` con `tap` que loguea la operación:

~~~typescript
/** GET heroes from the server */
getHeroes (): Observable<Hero[]> {
  return this.http.get<Hero[]>(this.heroesUrl)
    .pipe(
      tap(heroes => this.log(`fetched heroes`)),
      catchError(this.handleError('getHeroes', []))
    );
}
~~~

La gran mayoría de las *APIs* dan soporte a peticiones tipo *get by id* con la forma `api/hero/:id`. Añade un método `HeroService.getHero()` para realizar esa petición:

~~~typescript
/** GET hero by id. Will 404 if id not found */
getHero(id: number): Observable<Hero> {
  const url = `${this.heroesUrl}/${id}`;
  return this.http.get<Hero>(url).pipe(
    tap(_ => this.log(`fetched hero id=${id}`)),
    catchError(this.handleError<Hero>(`getHero id=${id}`))
  );
}
~~~

Hay tres diferencias significantes con `getHeroes()`:

- Construye *URL* de petición con el id de héroe deseado.
- El servidor debería responder con un único héroe en lugar de un array de héroes.
- `getHero` devuelve un `Observable<Hero>`en lugar de un array.

Ahora cuando editas el nombre de un héroe en la vista detalle, el nombre se actualiza en la cabecera, pero cuando vuelves atrás los cambios se pierden. Si quieres que los cambios persistan debes escribirlos en el servidor. Añade en la vista `hero-detail.component.html` un botón para guardar los cambios al final de la plantilla que esté vinculado al nuevo método `save()`:

~~~typescript
<button (click)="save()">save</button>
~~~

Añade el método `save()` al componente `hero-detail.component.ts` que persista los cambios usando el servicio de héroes, el método `updateHero()` y que luego vuelva a la vista anterior:

~~~typescript
save(): void {
   this.heroService.updateHero(this.hero)
     .subscribe(() => this.goBack());
 }
~~~

La estructura de `updateHero()` es simular a `getHeroes()`, pero usa `http.put()` para persistir los cambios en el servidor:

~~~typescript
/** PUT: update the hero on the server */
updateHero (hero: Hero): Observable<any> {
  return this.http.put(this.heroesUrl, hero, httpOptions).pipe(
    tap(_ => this.log(`updated hero id=${hero.id}`)),
    catchError(this.handleError<any>('updateHero'))
  );
}
~~~

El método `HttpClient.put()` recibe tres parámetros:

- La *URL*
- La información a actualizar.
- Las opciones.

La *URL* no cambia, la API sabe que héroe actualizar a través del `id` y espera una cabecera *HTTP* especial en las peticiones de guardado. Esa cabecera está en `httpOptions` definido en `HeroService`.

~~~typescript
const httpOptions = {
  headers: new HttpHeaders({ 'Content-Type': 'application/json' })
};
~~~

Actualiza el navegador, cambia el nombre de un héroe, haz click en el botón para volver atrás. El héroe que has modificado debe persistir con los cambios que hayas realizado.

Para añadir un héroe solo necesitamos el nombre. Usa un elemento `input` con un botón para añadirlo. Inserta el siguiente código en la plantilla `HeroesComponent` después de la cabecera:

~~~html
<div>
  <label>Hero name:
    <input #heroName />
  </label>
  <!-- (click) passes input value to add() and then clears the input -->
  <button (click)="add(heroName.value); heroName.value=''">
    add
  </button>
</div>
~~~

En respuesta al evento click, llama al manejador click del componente y limpia el campo `input` para dejarlo listo para otro nombre:

~~~typescript
add(name: string): void {
  name = name.trim();
  if (!name) { return; }
  this.heroService.addHero({ name } as Hero)
    .subscribe(hero => {
      this.heroes.push(hero);
    });
}
~~~

Cuando llega un nombre que no está en blanco, el manejador crea un `Hero`, como un objeto pero sin el `id`, y lo pasa al servicio con el método `addHero()`. Cuando `addHero` lo guarda, la función `subscribe` recibe el nuevo héroe y lo añade al listado de `heroes` a mostrar. Añade el método `addHero()` a la clase `HeroService`:

~~~typescript
/** POST: add a new hero to the server */
addHero (hero: Hero): Observable<Hero> {
  return this.http.post<Hero>(this.heroesUrl, hero, httpOptions).pipe(
    tap((hero: Hero) => this.log(`added hero w/ id=${hero.id}`)),
    catchError(this.handleError<Hero>('addHero'))
  );
}
~~~

`HeroService.addHero()` difiere de `updateHero` en dos formas:

- Llama a `HttpClient.post()` en lugar de `put`.
- Espera que el servidor genere el id del nuevo héroe, que es devuelto en `Observable<Hero>`.

Actualiza el navegador y añade algunos héroes.

Cada héroe en la lista debe tener un botón de borrado. Añade el siguiente elemento a la plantilla `HeroesComponent`, después del nombre del héroes repetido en el elemento `<li>`:

~~~html
<button class="delete" title="delete hero"
(click)="delete(hero)">x</button>
~~~

El código *HTML* del listado de héroes debería parecerse al siguiente:

~~~html
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
    <button class="delete" title="delete hero"
    (click)="delete(hero)">x</button>
  </li>
</ul>
~~~

La posición del botón de borrado se puede definir mediante *css*, añade lo siguiente a `heroes.component.css`:

~~~css
/* HeroesComponent's private CSS styles */
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  position: relative;
  cursor: pointer;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}

.heroes li:hover {
  color: #607D8B;
  background-color: #DDD;
  left: .1em;
}

.heroes a {
  color: #888;
  text-decoration: none;
  position: relative;
  display: block;
  width: 250px;
}

.heroes a:hover {
  color:#607D8B;
}

.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color: #607D8B;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  min-width: 16px;
  text-align: right;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}

button {
  background-color: #eee;
  border: none;
  padding: 5px 10px;
  border-radius: 4px;
  cursor: pointer;
  cursor: hand;
  font-family: Arial;
}

button:hover {
  background-color: #cfd8dc;
}

button.delete {
  position: relative;
  left: 194px;
  top: -32px;
  background-color: gray !important;
  color: white;
}
~~~

Y el manejador `delete()` del componente:

~~~typescript
delete(hero: Hero): void {
  this.heroes = this.heroes.filter(h => h !== hero);
  this.heroService.deleteHero(hero).subscribe();
}
~~~

Aunque el componente delegue el borrado del héroe en `HeroService`, sigue actualizando la lista de héroes. El método `delete()` elimina el héroe del listado anticipando que `HeroService` funcionará correctamente.

No hay que hacer nada con el `Observable` devuelto por `heroService.delete()`, pero **se debe realizar de todos modos**. En caso contrario no se realizaría la llamada al servidor.

Añade la función `deleteHero()` a `HeroService`:

~~~typescript
/** DELETE: delete the hero from the server */
deleteHero (hero: Hero | number): Observable<Hero> {
  const id = typeof hero === 'number' ? hero : hero.id;
  const url = `${this.heroesUrl}/${id}`;

  return this.http.delete<Hero>(url, httpOptions).pipe(
    tap(_ => this.log(`deleted hero id=${id}`)),
    catchError(this.handleError<Hero>('deleteHero'))
  );
}
~~~

En este método:

- Se llama a `HttpClient.delete`.
- La *URL* es el recurso de héroe más el `id` del héroe a eliminar.
- No necesitas enviar información como hiciste con `put` y `post`.
- Envías también `httpOptions`

Actualiza el navegador y ya tienes la funcionalidad de borrado.

### Búsqueda por nombre

En esta sección añadirás la funcionalidad de búsqueda al tablero de héroes. Cuando el usuario escriba teto en una caja de texto realizarás peticiones *HTTP* para obtener héroes filtrados por ese nombre. El objetivo es realizar las llamadas que sean necesarias.

Empieza añadiendo el método `searchHeroes` a `HeroService`:

~~~typescript
/* GET heroes whose name contains search term */
searchHeroes(term: string): Observable<Hero[]> {
  if (!term.trim()) {
    // if not search term, return empty hero array.
    return of([]);
  }
  return this.http.get<Hero[]>(`api/heroes/?name=${term}`).pipe(
    tap(_ => this.log(`found heroes matching "${term}"`)),
    catchError(this.handleError<Hero[]>('searchHeroes', []))
  );
}
~~~

El método devuelve inmediatamente un array vacío si no hay término que buscar. El resto es muy similar a `getHeroes()`. La única diferencia es la *URL*, que incluye una *query string* en el término de búsqueda.

Abre la plantilla `DashBoardComponent` y añade el elemento de búsqueda, `<app-hero-search>` al final.

~~~html
{% raw %}
<h3>Top Heroes</h3>
<div class="grid grid-pad">
  <a *ngFor="let hero of heroes" class="col-1-4"
      routerLink="/detail/{{hero.id}}">
    <div class="module hero">
      <h4>{{hero.name}}</h4>
    </div>
  </a>
</div>
{% endraw %}

<app-hero-search></app-hero-search>
~~~

Esta plantilla se parece mucho a la plantilla `HeroesComponent`.

Desafortunadamente, añadiendo este elemento, se rompe la aplicación. **Angular** no puede encontrar el componente que corresponde con `<app-hero-search>`. `HeroSearchComponent` no existe aún. Arréglalo introduciendo el siguiente comando:

~~~terminal
$ ng generate component hero-search
~~~

Se crean los tres `HeroSeachComponent` y se añade a las declaraciones `AppModule`. Reemplaza el contenido generado de la plantilla `HeroSearchComponent` y una caja de texto y una lista con los resultados:

~~~html
{% raw %}
<div id="search-component">
  <h4>Hero Search</h4>

  <input #searchBox id="search-box" (keyup)="search(searchBox.value)" />

  <ul class="search-result">
    <li *ngFor="let hero of heroes$ | async" >
      <a routerLink="/detail/{{hero.id}}">
        {{hero.name}}
      </a>
    </li>
  </ul>
</div>
{% endraw %}
~~~

Añade el siguiente estilo *css* a `hero-search.component.css`:

~~~css
/* HeroSearch private styles */
.search-result li {
  border-bottom: 1px solid gray;
  border-left: 1px solid gray;
  border-right: 1px solid gray;
  width:195px;
  height: 16px;
  padding: 5px;
  background-color: white;
  cursor: pointer;
  list-style-type: none;
}

.search-result li:hover {
  background-color: #607D8B;
}

.search-result li a {
  color: #888;
  display: block;
  text-decoration: none;
}

.search-result li a:hover {
  color: white;
}
.search-result li a:active {
  color: white;
}
#search-box {
  width: 200px;
  height: 20px;
}


ul.search-result {
  margin-top: 0;
  padding-left: 0;
}
~~~

Cuando el usuario escribe en la caja de texto, el evento *keyup* llama al método `search()` con el nuevo valor de la caja de texto.

Como esperamos `*ngFor` repite los objetos de héroe. Si te fijas en el `*ngFor`, itera sobre una lista llamada `heroes$`, no `heroe`:

~~~html
<li *ngFor="let hero of heroes$ | async" >
~~~

`$` indica que `heroe$` es un `Observable`, no un array. `*nFor` no puede hacer nada con un `Observable`. Pero también está el carácter `|` seguido de `async`, que define `AsyncPipe`. Este `AsyncPipe` suscribe `Observable` automáticamente por lo que lo tienes en la clase del componente.

Reemplaza el código generado de `HeroSearchComponent` por esto:

~~~typescript
import { Component, OnInit } from '@angular/core';

import { Observable } from 'rxjs/Observable';
import { Subject }    from 'rxjs/Subject';
import { of }         from 'rxjs/observable/of';

import {
   debounceTime, distinctUntilChanged, switchMap
 } from 'rxjs/operators';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-search',
  templateUrl: './hero-search.component.html',
  styleUrls: [ './hero-search.component.css' ]
})
export class HeroSearchComponent implements OnInit {
  heroes$: Observable<Hero[]>;
  private searchTerms = new Subject<string>();

  constructor(private heroService: HeroService) {}

  // Push a search term into the observable stream.
  search(term: string): void {
    this.searchTerms.next(term);
  }

  ngOnInit(): void {
    this.heroes$ = this.searchTerms.pipe(
      // wait 300ms after each keystroke before considering the term
      debounceTime(300),

      // ignore new term if same as previous term
      distinctUntilChanged(),

      // switch to new search observable each time the term changes
      switchMap((term: string) => this.heroService.searchHeroes(term)),
    );
  }
}
~~~

Fíjate que la declaración de `heroe$` es un `Observable`.

~~~typescript
heroes$: Observable<Hero[]>;
~~~

Lo podrás encontrar en `ngOnInit()`.

La propiedad `searchTerms` está declarada como `Subject` de RxJS.

~~~typescript
private searchTerms = new Subject<string>();

// Push a search term into the observable stream.
search(term: string): void {
  this.searchTerms.next(term);
}
~~~

`Subject` es una fuente de valores *observable* y un `Observable` en sí mismo. Te puedes suscribir a `Subject` tal y como haces con `Observable`. También puedes añadir valores a ese `Observable` llamado a su método `next(value)` como el hace el método `search()`. El método `search()` se le llama a través del evento vinculado `keystore`:

~~~html
<input #searchBox id="search-box" (keyup)="search(searchBox.value)" />
~~~

Siempre que se escriba en la caja de texto, se llama al método `search()` con el valor de la caja de texto con un "search term". Los `searchTerms` se transforman en `Observable` emitiendo un flujo de términos de búsqueda.

Pasando un nuevo término de búsqueda a `searchHeroes()` cada vez que el usuario pulsa una tecla puede crear un exceso de llamadas *HTTP*, consumiendo mucho ancho de banda.

En lugar de ello, el método `ngOnInit()` canaliza los *observables* `searchTerms` a través de una secuencia de operadores RxJS que reduce el número de llamadas a `searchHeroes()`, y finalmente devolviendo un *observable* como resultados de búsqueda:

~~~typescript
this.heroes$ = this.searchTerms.pipe(
  // wait 300ms after each keystroke before considering the term
  debounceTime(300),

  // ignore new term if same as previous term
  distinctUntilChanged(),

  // switch to new search observable each time the term changes
  switchMap((term: string) => this.heroService.searchHeroes(term)),
);
~~~

- `debounceTime(300)` espera hasta que los eventos de flujo de nuevo texto paran 300 milisegundos antes de pasar el último texto introducido. Nunca hagas peticiones más frecuentemente de 300 milisegundos.
- `distinctUntilChanged` se asegura que una petición solo se envía si el texto ha cambiado.
- `switchMap()` llama al servicio de búsqueda por cada término de búsqueda, devolviendo solo el último resultado.

Recuerda que el componente de `class` no se suscribe al observable `heroe$`. Esto es trabajo de `AsyncPipe` de la plantilla.

Vuelve a ejecutar la aplicación. En el tablero de héroes, introduce texto en la caja de texto para realizar la búsqueda. Si lo introducido coincide con nombres de héroe existentes debería aparecer algo como esto:

![TOHHEROSEARCH]({{ site.baseurl}}/images/toh-hero-search.png){:class="img-responsive"}

# Electron

Bueno, toda esto está muy bien, pero si lo que quieres es aprovechar todo este conocimiento y aplicarlo a una aplicación de escritorio multiplataforma. Puedes. Si no conoces **Electron** te dejo [aquí](https://electronjs.org) un enlace a su web. Básicamente te permite aplicar *HTML* y *javascript* entre otras a realizar aplicaciones de escritorio. En este cas lo tenemos facilito.

Abre el fichero `src/index.html` y cambia la dirección base generada por **Angular**. Esta dirección base está apuntando a `/`, esto causa problemas a **Electron**:

~~~html
<base href="./">
~~~

Si no tienes **Electron** instalado es el momento:

~~~terminal
$ npm install electron --save-dev
~~~

Ahora cambia el siguiente contenido en `package.json`:

~~~json
{
  "name": "angular-electron",
  "version": "0.0.0",
  "license": "MIT",
  "main": "main.js", // <-- aquí
  "scripts": {
    "ng": "ng",
    "start": "ng serve",
    "build": "ng build",
    "test": "ng test",
    "lint": "ng lint",
    "e2e": "ng e2e",
    "electron": "electron .", // <-- lanzar electron 
    "electron-build": "ng build --prod && electron ." // <-- construir la aplicación y luego lanza electron 
  },
  // ...omitido
}
~~~

Lo que queda es lanzarlo con el siguiente comando:

~~~terminal
$ npm run electron-build
~~~

Al poco tiempo se debe lanzar la misma aplicación que antes pero en una ventana de escritorio.

Esta última parte lo he encontrado en [la web de firebase](https://angularfirebase.com/lessons/desktop-apps-with-electron-and-angular/).

# Para acabar

Como de costumbre, dejo en **Github** los proyectos. [Aquí el de **Angular**](https://github.com/44r0n/hero-tour-tutorial), [aquí con **Electron**](https://github.com/44r0n/Electron-Tour-Of-Heroes)

Me ha salido un post kilométrico. Lo siento.

Si tienes dudas o comentarios no dudes en comentar.

Nos vemos en las próximas líneas.