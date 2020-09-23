---
layout:  post
title: Primeras partes de un proyecto (IV)
---

### Lectura de ~11 minutos

[En la anterior entrega vimos como tener listo un servidor de desarrollo para el proyecto]({{ site.baseurl}}{% post_url 2019-11-18-Aprendiendo-en-publico-III %}). En este post, que también va a ser extenso, veremos como tener un entorno de desarrollo para la web comunicandose con el servidor mediante contenedores.

Ahora que tenemos la parte de backend respirando, toca darle vida a la web y poder empezar a ver algo, no solo letritas en la pantalla. Lo que hay a continuación es un típico tutorial de [`Vue`](https://vuejs.org) aplicando [`Typescript`](https://www.typescriptlang.org). Este proyecto se encontrará en un directorio adyacente al servicio llamado `web`. 

Antes de que empieces a leer, este un pequeño tutorial de como montar una web para utilizar el servicio que hemos construido. Hay cosas que se han realizado aquí que no son óptimas para lanzarlas a producción ni son como se deberían maquetar.

Al igual que en el caso anterior, es un proyecto con `npm`, así que también se puede iniciar con `npm init` dentro del directorio, que en este caso se llama `web`. Este proyecto también estará en un contenedor [`Docker`](https://www.docker.com) por lo que podemos crear el fichero `package.json` directamente:

```json
{
  "name": "helenos-web",
  "version": "1.0.0",
  "description": "Web for helenos service",
  "main": "index.js",
  "scripts": {
    "build": "webpack",
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server --hot --host 0.0.0.0 --progress --mode development --watch-poll",
    "prod": "webpack --mode production"
  },
  "author": "Aarón Sánchez",
  "license": "GPL-3.0",
  "dependencies": {
    "@fortawesome/fontawesome-svg-core": "^1.2.19",
    "@fortawesome/free-brands-svg-icons": "^5.9.0",
    "@fortawesome/free-regular-svg-icons": "^5.9.0",
    "@fortawesome/free-solid-svg-icons": "^5.9.0",
    "@fortawesome/vue-fontawesome": "^0.1.6",
    "axios": "^0.19.0",
    "bulma": "^0.7.5",
    "lodash": "^4.17.14",
    "vue": "^2.6.10",
    "vue-class-component": "^7.1.0",
    "vue-property-decorator": "^8.2.1",
    "vue-router": "^3.0.7"
  },
  "devDependencies": {
    "@types/axios": "^0.14.0",
    "@types/lodash": "^4.14.135",
    "css-loader": "^3.0.0",
    "ts-loader": "^6.0.4",
    "typescript": "^3.5.2",
    "vue-loader": "^15.7.0",
    "vue-style-loader": "^4.1.2",
    "vue-template-compiler": "^2.6.10",
    "webpack": "^4.35.2",
    "webpack-cli": "^3.3.5",
    "webpack-dev-server": "^3.7.2"
  }
}
```

Las dependencias del proyecto son las siguientes:

* **axios**: `npm install axios --save`. Instalada como dependencia de poryecto. Esta librería nos permite hacer llamadas `http` desde la web.

* **bulma**: `npm install bulma --save`. Instalada como dependencia de proyecto. Esta librería no son más que los estilos de [Bulma](https://bulma.io/).

* **lodash**: `npm install lodash --save`. Instalada como dependencia de proyecto. Usamos esta librería para no sobrecargar las llamadas al servidor cuando escribimos el nombre de usuario y lo queremos validar contra nuestra base de datos conforme vamos escribiendo.

* **vue**: `npm install vue --save`. Instalada como dependencia de proyecto. Es el framework front en el que será creada la web.

* **vue-router**: `npm install vue-router --save`. Instalada como dependencia de proyecto. Esta librería nos permite enrutar páginas en nuestro proyecto.

* **@types/axios**: `npm install @types/axios --save-dev`. Instalada como dependencia de desarrollo. Son los tipos de [`Typescript`](https://www.typescriptlang.org) de `axios`.

* **@types/lodash**: `npm install @types/lodash --save-dev`. Instalada como dependencia de desarrollo. Son los tipos de [`Typescript`](https://www.typescriptlang.org) de `lodash`.

* **webpack**: `npm install webpack --save-dev`. Instalada como dependencia de desarrollo. Es un *bundler* con el que unificaremos todo el *javascript* transpilado en un solo fichero.

* **webpack-cli**: `npm install webpack-cli --save-dev`. Instalada como dependencia de desarrollo. Es el cliente de terminal de [`Webpack`](https://webpack.js.org) que usamos para lanzarlo.

* **webpack-dev-server**: `npm install webpack-dev-server --save-dev`. Instalada como dependencia de desarrollo. Es un pequeño servidor web de desarrollo que lo usaremos para que nos sirva el contenido estático de la web.

* **css-loader**: `npm install css-loader --save-dev`. Instalada como dependencia de desarrollo. Es un *plugin* de [`Webpack`](https://webpack.js.org) que usaremos para unificar los estilos.

* **ts-loader**: `npm install ts-loader --save-dev`. Instalada como dependencia de desarrollo. Es otro *plugin* de [`Webpack`](https://webpack.js.org) que usaremos para unificar el código `javascript` transpilado por [`Typescript`](https://www.typescriptlang.org).

* **typescript**: `npm install typescript --save-dev`. Instalada como dependencia de desarrollo. Al igual que en el caso del servidor web, usado para transpilar [`Typescript`](https://www.typescriptlang.org) a `javascript`.

* **vue-loader**: `npm install vue-loader --save-dev`. Instalada como dependencia de desarrollo. Es un *plugin* de [`Webpack`](https://webpack.js.org) que usaremos para autoirzar el uso de componentes en ficheros `.vue`.

* **vue-style-loader**: `npm install vue-style-loader --save-dev`. Instalada como dependencia de desarrollo. Otro *plugin* de [`Webpack`](https://webpack.js.org) similar a `css-loader` pero para ficheros `.vue`.

* **vue-template-compiler**: `npm install vue-template-compiler --save-dev`. Instalada como dependencia de desarrollo. Es el compilador de plantillas de [`Vue`](https://vuejs.org).

* **vue-class-component**: `npm install vue-property-decorator --save-dev`. Instalada como dependencia de desarrollo. Es la clase [`Vue`](https://vuejs.org) para [`Typescript`](https://www.typescriptlang.org).

* **vue-property-decorator**: `npm install vue-property-decorator --save-dev`. Instalada como dependencia de desarrollo. Son decoradores de [`Typescript`](https://www.typescriptlang.org) necesarios para la clase de [`Vue`](https://vuejs.org). 

* **@fortawesome/fontawesome-svg-core**: `npm install @fortawesome/fontawesome-svg-core --save-dev`. Instalada como dependencia de desarrollo. Es la librería de iconos núcleo de [Fontawesome](https://fontawesome.com/).

* **@fortawesome/free-brands-svg-icons**: `npm install @fortawesome/free-brands-svg-icons --save-dev`. Instalada como dependencia de desarrollo. Es la librería de iconos de marcas de [Fontawesome](https://fontawesome.com/).

* **@fortawesome/free-regular-svg-icons**: `npm install @fortawesome/free-regular-svg-icons --save-dev`. Instalada como dependencia de desarrollo. Es la librería de iconos regulares de [Fontawesome](https://fontawesome.com/).

* **@fortawesome/free-solid-svg-icons**: `npm install @fortawesome/free-solid-svg-icons --save-dev`. Instalada como dependencia de desarrollo. Es la librería de iconos con estilo sólido de [Fontawesome](https://fontawesome.com/).

* **@fortawesome/vue-fontawesome**: `npm install @fortawesome/vue-fontawesome --save-dev`. Instalada como dependencia de desarrollo. Es la librería de [Fontawesome](https://fontawesome.com/) para [`Vue`](https://vuejs.org).

### Configuración de la web

Primero configuraremos [`Typescript`](https://www.typescriptlang.org), que como en el apartado del servicio web lo podemos hacer mediante el comando `npm run tsc -- --init` o crear el fichero `tsconfing.json` con el siguiente contenido(recomendado):

```json
{
    "compilerOptions": {
      "target": "esnext",
      "module": "commonjs",
      "lib": ["es2015", "es2016", "dom", "es2017", "es6", "es5"],      
      "outDir": "./dist",
      "strict": true,
      "noImplicitAny": true,
      "strictNullChecks": true,      
      "strictPropertyInitialization": false,
      "noImplicitThis": true,      
      "noImplicitReturns": true,      
      "moduleResolution": "node",
      "baseUrl": ".",
      "paths": {
        "*": ["node_modules/*", "src/types"]
      },                           
      "esModuleInterop": true,
      "experimentalDecorators": true,       
      "emitDecoratorMetadata": true,    
    },
    "include": [
      "src/**/*"
    ]
  }
```

En este fichero dejamos [`Typescript`](https://www.typescriptlang.org) configurado para su uso con [`Vue`](https://vuejs.org). Y para que [`Typescript`](https://www.typescriptlang.org) reconozca los ficheros [`Vue`](https://vuejs.org) debemos crear el fichero `vue-shims.d.ts` en el directorio `web/src` con el siguiente contenido:

```typescript
declare module "*.vue" {
    import Vue from "vue";
    export default Vue;
}
```

Llegados a este punto ya podemos empezar a ponernos manos a la obra. Primero necesitamos un fichero `html`

 en el que tendremos el contenido de la web. Este fichero lo colocaremos a la altura de `package.json` y lo llamaremos `index.html`:

```html
<!doctype html>
<html>
<head></head>

<body>
    <div id="app">           
        <app></app>
    </div>
</body>
<script src="./dist/build.js"></script>

</html>
```

En él hacemos referencia al fichero `./dist/build.js`, que es el fichero de salida que generará [`Webpack`](https://webpack.js.org). El `div` con el `id="app"` es el `div` que contendrá todo el contenido de nuestra web. La etiqueta `app` es la que creamos mediante [`Vue`](https://vuejs.org) con el fichero `web/src/index.tx`:

```typescript
import Vue from "vue";
import VueRouter from 'vue-router';
import router from './router';
import 'bulma/css/bulma.css';
import App from './components/App.vue';

Vue.use(VueRouter);
Vue.component('app', App);

const app = new Vue({
  router  
}).$mount('#app');
// Now the app has started!
```

Ademas de montar la app de [`Vue`](https://vuejs.org) este fichero importa los estilos `css` de [`Bulma`](https://bulma.io), monta el enrutador mediante el fichero `web/src/router.ts`:

```typescript
import VueRouter from 'vue-router';
import PublicComponent from './components/PublicComponent.vue';
import LoginComponent from "./components/LoginComponent.vue";
import RegisterComponent from "./components/RegisterComponent.vue";
import ProtectedComponent from "./components/ProtectedComponent.vue";
import LogoutComponent from "./components/LogoutComponent.vue";

const routes = [
    { path: '/', name: 'PublicComponent', component: PublicComponent },
    { path: '/Login', name: 'LoginComponent', component: LoginComponent },
    { path: '/Register', name: 'RegisterComponent', component: RegisterComponent },
    { path: '/Protected', name: 'ProtectedComponent', component: ProtectedComponent },
    { path: '/Logout', name: 'LogoutComponent', component: LogoutComponent }
]


const router = new VueRouter({
    routes
});

export default router;
```

Las rutas no son más que un *array* de objetos con la ruta, el nombre y una referencia al componente de [`Vue`](https://vuejs.org). De vuelta al fichero `index.ts`, también hace referencia al componente `web/src/components/App.vue`, que es el componente que tiene el contenido de toda la aplicación:

```html
<template>
    <div>
        <menu-component></menu-component>
        <br/>
        <br />
        <br />
        <router-view></router-view>
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import VueRouter from 'vue-router';
import MenuComponent from './MenuComponent.vue';

Vue.component('menu-component', MenuComponent);

@Component
export default class App extends Vue {

}
</script>
```

El componente `web/src/components/MenuComponent.vue` contiene la barra de menú que siempre se encuentra en la parte superior:

```html
<template>
<nav class="navbar is-light" role="navigation" aria-label="main navigation">
    <div class="navbar-brand">
        <a @click="toogle" role="button" class="navbar-burger burger" :class="activeClass" aria-label="menu" aria-expanded="false" data-target="navbarBasicExample">
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
            <span aria-hidden="true"></span>
        </a>
    </div>

    <div class="navbar-menu" :class="activeClass">
        <div class="navbar-start">
            <router-link class="navbar-item" to="/">Public page</router-link>
            <router-link class="navbar-item" to="/Protected" v-show="registered"> <fa-icon :icon="['fas','user-shield']" /> &nbsp;Protedted zone</router-link>
        </div>

        <div class="navbar-end">
            <div class="navbar-item">
                <div class="buttons">
                    <router-link class="button is-primary" to="/Register">Register</router-link>
                    <router-link class="button is-light" to="/Login" v-show="!registered"> <fa-icon :icon="['fas','sign-in-alt']" /> &nbsp; Login</router-link>
                    <router-link class="button is-light" to="/Logout" v-show="registered"> <fa-icon :icon="['fas','sign-out-alt']" /> &nbsp;Logout</router-link>
                </div>
            </div>
        </div>
    </div>
</nav>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faSignInAlt, faSignOutAlt, faUserShield } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faSignInAlt, faSignOutAlt, faUserShield);

Vue.component('fa-icon', FontAwesomeIcon);

@Component
export default class MenuComponent extends Vue {
    registered = localStorage.getItem('siteUser') !== null;
    activeClass = '';

    @Watch('$route')
    onRouteChanged() {
        this.registered = localStorage.getItem('siteUser') !== null;
    }

    toogle() {
        this.activeClass = this.activeClass === '' ? 'is-active' : '';
    }
}
</script>
```

Aquí podemos ver varios elementos específicos de [`Vue`](https://vuejs.org), pero de momento nos debemos quedar con que las etiquetas `router-link` son los que nos permiten navegar entre las rutas configuradas anteriormente. Con el atributo `v-show` el elemento se muestra o no. La ruta por defecto es `/`, que se mapea al componente `web/src/components/PublicComponent.vue`:

```html
<template>
    <div class="container">   
        <font-awesome-icon :icon="['fab','galactic-republic']" size="10x" />
        <h1>This is the public page</h1>    
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { library } from '@fortawesome/fontawesome-svg-core';
import { faGalacticRepublic } from '@fortawesome/free-brands-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

library.add(faGalacticRepublic);

Vue.component('font-awesome-icon', FontAwesomeIcon);

@Component
export default class PublicComponent extends Vue {
    
}
</script>

```

Esta es una página que solo muestra un icono grande y un texto, nada interensante que comentar aquí. Pero el componente `web/src/components/RegisterComponent.vue` tiene más miga:

```html
<template>
    <div class="columns is-mobile">        
        <div class ="column is-three-fifths is-offset-one-fifth box">
            <h1 class="title">Register</h1>   
            <status-input
                labelText='User name:'
                :leftIcon='userIcon'
                :status='inputUserNameStatus'   
                type='text'             
                v-model='user.name'
             />             
            <status-message :status="emptyStatus">The user name cannot be empty.</status-message>
            <status-message :status="fourCharactersUserStatus">The user name must have at least 4 characters.</status-message>    
            <status-message :status="uniqueUserStatus">The user name bust be unique.</status-message>                
            <status-input 
                type='password'
                labelText='Password:'
                :leftIcon='passwordIcon'
                :status='inputPasswordStatus'
                v-model='user.password'
            />
            <status-input 
                type='password'
                labelText='Repeat password:'
                :leftIcon='passwordIcon'
                :status='inputPasswordStatus'
                v-model='user.repeatPassword'
            />
            <status-message :status='emptyPasswordStatus'>The password cannot be empty.</status-message>
            <status-message :status='eightCharactersPasswordStatus'>The password must contain at least 8 characters.</status-message>
            <status-message :status='equalPasswordStatus'>The two passwods must match</status-message>
            <div class="field is-grouped is-grouped-right">
                <p class="control">
                    <button class="button is-link" :class="loadingClass" :disabled=disableRegisterButton @click="register">Register</button>
                </p>
                <p class="control">
                    <button class="button is-dark is-outlined" @click="clear">Clear form</button>
                </p>
            </div>  
            <p class="content is-large">{{ newUserId }}</p>                
        </div>
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator';
import _ from "lodash";
import axios from 'axios';
import { library, IconDefinition } from '@fortawesome/fontawesome-svg-core';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faUser, faCheck, faUnlockAlt, faSpinner, faExclamation, faInfo } from '@fortawesome/free-solid-svg-icons';
import status from './statusComponents/status';
import statusMessage from './statusComponents/statusMessage.vue';
import statusInput from './statusComponents/statusInput.vue';

library.add(faUser, faUnlockAlt);

Vue.component('fa-icon', FontAwesomeIcon);
Vue.component('status-message', statusMessage);
Vue.component('status-input', statusInput);

@Component
export default class RegisterComponent extends Vue {
    user = { name: "", password: "", repeatPassword: ""};
    loadingClass = '';                 
    inputUserNameStatus: status = 'info';
    emptyStatus: status = 'info';    
    fourCharactersUserStatus: status = 'info';
    uniqueUserStatus: status = 'info';
    debouncedGetAvailableUser: (() => Promise<void>) & _.Cancelable;
    userIcon = faUser;

    passwordIcon = faUnlockAlt;
    inputPasswordStatus: status = 'info';
    emptyPasswordStatus: status = 'info';
    eightCharactersPasswordStatus: status = 'info';
    equalPasswordStatus: status = 'info';
    debouncedCheckPassword: (() => Promise<void>) & _.Cancelable;

    clearingUserName = false;    
    clearingPassword = false;
    clearingRepeatPassword = false;

    newUserId = '';

    @Watch('user.name')
    onUserNameChanged(val: string, oldValue: string) {  
        if(this.clearingUserName === false) {                  
            this.emptyStatus = 'load';        
            this.fourCharactersUserStatus = 'load';
            this.inputUserNameStatus = 'load';
            this.uniqueUserStatus = 'load';
            this.debouncedGetAvailableUser();
        } else {
            this.clearingUserName = false;
        }
    }    

    @Watch('user.password')
    onPasswordChanged(val: string, oldValue: string) {
        if(this.clearingPassword === false) {
            this.setLoadPassword();
        } else {
            this.clearingPassword = true;
        }
    }

    @Watch('user.repeatPassword')
    onRepeatPasswordChange(val: string, oldValue: string) {
        if(this.clearingRepeatPassword === false) {
            this.setLoadPassword();
        } else {
            this.clearingRepeatPassword = true;
        }
    }

    private setLoadPassword() {
        this.inputPasswordStatus = 'load';
        this.emptyPasswordStatus = 'load';
        this.eightCharactersPasswordStatus = 'load';
        this.equalPasswordStatus = 'load';
        this.debouncedCheckPassword();
    }

    created() {
        this.debouncedGetAvailableUser = _.debounce(this.checkUser, 500);
        this.debouncedCheckPassword = _.debounce(this.checkPassword, 500);
    }

    async checkUser() {        
        if (this.user.name.length === 0) {  
            this.emptyStatus = 'error';                  
            this.fourCharactersUserStatus = 'error';
            this.inputUserNameStatus = 'error';
            this.uniqueUserStatus = 'error';
        } else if (this.user.name.length < 4) {            
            this.emptyStatus = 'ok';                   
            this.fourCharactersUserStatus = 'error';
            this.inputUserNameStatus = 'error';
            this.uniqueUserStatus = 'error';            
        } else if (!await this.checkAvailableUser(this.user.name)) {
            this.emptyStatus = 'ok';
            this.fourCharactersUserStatus = 'ok';
            this.inputUserNameStatus = 'error';
            this.uniqueUserStatus = 'error';            
        }
        else {         
            this.emptyStatus = 'ok';
            this.fourCharactersUserStatus = 'ok';
            this.inputUserNameStatus = 'ok';
            this.uniqueUserStatus = 'ok';            
        }                       
    }

    async checkPassword() {
        if(this.user.password.length === 0) {
            this.emptyPasswordStatus = 'error';
            this.eightCharactersPasswordStatus = 'error';
            this.equalPasswordStatus = 'error';
            this.inputPasswordStatus = 'error';
        } else if (this.user.password.length < 8 ) {
            this.emptyPasswordStatus = 'ok';
            this.eightCharactersPasswordStatus = 'error';
            this.equalPasswordStatus = 'error';
            this.inputPasswordStatus = 'error';
        } else if (this.user.password !== this.user.repeatPassword) {
            this.emptyPasswordStatus = 'ok';
            this.eightCharactersPasswordStatus = 'ok';
            this.equalPasswordStatus = 'error';
            this.inputPasswordStatus = 'error';
        } else {
            this.emptyPasswordStatus = 'ok';
            this.eightCharactersPasswordStatus = 'ok';
            this.equalPasswordStatus = 'ok';
            this.inputPasswordStatus = 'ok';
        }
    }

    private async checkAvailableUser(userName: string): Promise<boolean> {
        try {
            const axiosResponse = await axios.post('http://localhost:3000/api/v1/user/userNameAvailable', {
                UserName: userName
            });
            return axiosResponse.data.available;
        }
        catch(error) {
            console.error(error.response.data.message);
            return false;
        }
    }

    clear() {
        this.user.name = "";
        this.user.password = "";
        this.user.repeatPassword = "";
        this.emptyStatus = 'info';                  
        this.fourCharactersUserStatus = 'info';
        this.inputUserNameStatus = 'info';
        this.uniqueUserStatus = 'info';   
        this.emptyPasswordStatus = 'info';
        this.eightCharactersPasswordStatus = 'info';
        this.equalPasswordStatus = 'info';
        this.inputPasswordStatus = 'info';    
        this.clearingUserName = true;
        this.clearingPassword = true;
        this.clearingRepeatPassword = true;
    }

    get disableRegisterButton(): boolean {        
        return (this.inputUserNameStatus !== 'ok' || this.inputPasswordStatus !== 'ok');
    }

    async register() {        
        this.loadingClass = 'is-loading';           

        try {
            const axiosResponse = await axios.post('http://localhost:3000/api/v1/user/register', {
                UserName: this.user.name,
                Password: this.user.password
            });
            this.newUserId = `Your new iser id is:  ${axiosResponse.data.user}`;
            console.log(`Your new user id is ${axiosResponse.data.user}`);
            this.clear();                                 
        }
        catch(error) {
            console.error(error.response.data.message);                                    
        }

        this.loadingClass = ''; 
    }
}
</script>
```

Lo sé, es un componente monstruoso y se debería refactorizar, pero para hacer este pequeño tutorial me sirve. Siguiendo el fichero desde la parte superior, vemos que el primer elemento que nos llama la atención es `status-input`. Este elemento es otro componente [`Vue`](https://vuejs.org) creado por nosotros. Este componente utiliza varias cosas:
- **Watch**. Podemos observar cambios, ya no solo sobre un componente en sí, si no sobre un objeto definido en nuestro modelo, en este caso podemos observar cambios en el nombre de usuario y la contraseña y reaccionar a dichos cambios, todo esto se puede ver con el decorador `Watch`.
- **Componentes personalizados**. Aquí podemos ver como se utilizan los componentes personalizados. Vemos que este elemento tiene atributos como `labelText` o `:leftIcon`. El primero está sin puntos porque se asigna a un valor concreto, el segundo lleva dos puntos delante porque se asigna a una variable del componente `RegisterComponent`, de modo que si se modifica la variable asignada [`Vue`](https://vuejs.org) reasignara dicha variable a `status-input` y repintará el componente `status-input` acorde. Entre todos los atributos hay uno en especial, `v-model`, que funciona igual que los demás y además si se cambia en el componente hijo también cambia el valor en el componente padre de modo que en el padre podemos cambiar estado acorde a esos cambios. En la etiqueta `v-model` no tiene porque ir solo una variable de tipo primitivo como se ha hecho en todo este proyecto, también lo podemos asignar a cualquier objeto y el componente hijo se encargará de decidir como pintarlo y asignarlo. Ahora que sabemos como se usa el componente `status-input` vamos a ver como se ha definido en el fichero `web/src/components/statusComponents/statusInput.vue`:

```html
<template>    
    <input-text 
        :labelText='labelText'
        :inputClass='inputClassStatus'                   
        :leftIcon='leftIcon'                 
        :rightIcon='rightIcon'
        :rightIconSpin='rightIconSpin'
        :type='type'             
        v-model='inputValue'                 
    />    
</template>

<script lang='ts'>
import { Vue, Component, Prop } from 'vue-property-decorator';
import status from './status';
import { library, IconDefinition } from '@fortawesome/fontawesome-svg-core';
import inputText from '../utils/inputText.vue';
import { faInfo, faExclamation, faCheck, faSpinner } from '@fortawesome/free-solid-svg-icons';

library.add(faCheck, faSpinner, faExclamation, faInfo);

Vue.component('input-text', inputText);

@Component
export default class StatusInput extends Vue {    
    @Prop() labelText: string;
    @Prop() type: string;
    @Prop() status: status;
    @Prop() leftIcon?: IconDefinition;
    @Prop() value: string;
    

    private lastStatusClass: string;

    get rightIcon(): IconDefinition | undefined {
        switch (this.status) {
            case 'info':
                return undefined;
            case 'error':
                return faExclamation;
            case 'ok':
                return faCheck;
            case 'load':
                return faSpinner;
        }
    }

    get rightIconSpin() { return  this.status === 'load'; }

    get inputClassStatus():string {
        switch (this.status) {
            case 'info':
                this.lastStatusClass = 'is-info';
                return 'is-info';
            case 'error':
                this.lastStatusClass = 'is-danger';
                return 'is-danger';
            case 'ok':
                this.lastStatusClass = 'is-success';
                return 'is-success';
            case 'load':
                return this.lastStatusClass;
        }
    }

    get inputValue(): string {
        return this.value;
    }

    set inputValue(newValue: string) {
        this.$emit('input', newValue);
    }
}
</script>

```

En la definición de la clase del componente podemos ver los atributos definidos en la etiqueta con `@Prop()`. Este componente en concreto se basa en el `status` que se le quiere dar para que éste automatice sus claes e iconos, de modo que desde el componente padre solo se le cambia el `status` para que funcione. Como el 'modelo' de este componente no se va a procesar en dicho componente, si no en el componente padre, necesitamos emitir el evento pertinente para que el padre lo pueda capturar y procesar como debe. Para ello hemos asignado al `v-model` del componente `input-text` la propiedad computada `inputValue`, que no es más que el `value` que nos asigna el componente padre. Cuando el valor de `v-model` cambia solamente pasamos el evento hacia el padre emitiendo el evento con la línea 
```typescript
set inputValue(newValue: string) {
    this.$emit('input', newValue);
}
```
A su vez, este componente se basa en el componente `web/src/components/utils/inputText.vue`:

```html
<template>
    <div class="field">
        <label class="label">{{ labelText }}</label>                
        <div class="control" :class="iconClass" >
            <input class="input" :class="inputClass" :type="type" ref="input" :value="value" @input="$emit('input', $event.target.value)" />            
            <span v-if="leftIcon !== undefined" class="icon is-small is-left" >
                <fa-icon :icon="[leftIcon.prefix, leftIcon.iconName]" />
            </span>
            <span v-if="rightIcon !== undefined" class="icon is-small is-right">
                <fa-icon :icon="[rightIcon.prefix, rightIcon.iconName]" :spin="rightIconSpin"/>
            </span>
        </div>        
    </div>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator';
import { IconDefinition } from '@fortawesome/fontawesome-svg-core';

@Component
export default class InputText extends Vue {    
    @Prop() labelText: string;
    @Prop() inputClass?: string;
    @Prop() value: string;
    @Prop() leftIcon?: IconDefinition;
    @Prop() rightIcon?: IconDefinition;
    @Prop() rightIconSpin?: boolean;
    @Prop() type: string;

    get iconClass(): string {
        const leftIconClass = this.leftIcon !== undefined ? 'has-icons-left' : '';
        const rightIconClass = this.rightIcon !== undefined ? 'has-icons-right' : '';
        return leftIconClass + ' ' + rightIconClass;
    }    
}
</script>

```

En este caso, el `v-model` que se nos asigna viene en la prop `value` y al igual que en el componente padre, cuando dicho valor cambia emitimos el evento al componente padre, pero esta vez lo hemos hecho inline con la etiqueta: 
```html
<input class="input" :class="inputClass" :type="type" ref="input" :value="value" @input="$emit('input', $event.target.value)" />
```

El resto de componentes utilizan las mismas técnicas que hemos visto ya hasta aquí. 

Una vez que lo tenemos todo montado debemos incorporarlo a nuestras imágenes de `Docker`, así que primero tenemos que crear el fichero `web/Dockerfile` con el siguiente contenido:
```
FROM node:lts as build-stage
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 4000
CMD [ "npm", "run", "dev" ]
```

También sería conveniente crear el fichero `web/.dockerignore` con el siguiente contenido:
```
./node_modules
```

Por último debemos modificar el fichero `docker-compose.yml` para añadir la web:
```yaml
version: '3'
services:
  web:
    build: ./web
    ports:
      - "4000:4000"
    volumes:
      - ./web:/usr/src/app
  webservice:
    build: ./service
    ports:
      - "3000:3000"
    environment:
      DEPLOY_ENV: ${DEPLOY_ENV}
    volumes:
      - ./service:/usr/src/app
  database:
    restart: always
    image: postgres
    ports:
      - "5432:5432"
    environment:
      POSTGRES_PASSWORD: docker
      POSTGRES_USER: docker
      POSTGRES_DB: helenosdb
```

La web la obtendremos en el puerto 4000 para no entorpecer nada de lo que tengamos lanzado. Si crees que tienes algo en los puertos que se indican en el fichero, siempre puedes modificarlos para que puedan ser accesibles desde tu máquina.
Y con la magia del comando `docker-compose up` se levantan los tres contenedores sirviendo la web, el servicio y la base de datos, todo con un solo comando. Si no has cambiado ningún puerto, en la url `http://localhost:4000` estará la web lista para usar.

Nos vemos en las próximas líneas.