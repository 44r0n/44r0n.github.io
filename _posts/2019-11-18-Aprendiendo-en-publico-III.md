---
layout:  post
title: Primeras partes de un proyecto (III)
---

### Lectura de ~8 minutos

La semana pasada vimos como [montaría una base de datos de desarrollo para el proyecto]({{ site.baseurl}}{% post_url 2019-10-20-Aprendiendo-en-publico-II %}). En este post, que adelanto va a ser un poco largo, explicaré como montar un entorno de desarrollo para la parte del servidor.

## Creando la base del servicio

El servicio estará hecho en [`Node`](https://nodejs.org/en/) y [`Typescript`](https://www.typescriptlang.org). Para iniciarlo podemos ejecutar un comando `nmp init` en el directorio donde se va a encontrar el proyecto del servicio y seguir los pasos que aparecen en el terminal, que creará un fichero `package.json`, y nos obliga a tener `npm` instalado en el equipo de desarrollo. Esto le quita la gracia de usar [`Docker`](https://www.docker.com). Así que podemos crear directamente el fichero y ya se encargará [`Docker`](https://www.docker.com) del resto:

```json
{
  "name": "helenos-service",
  "version": "1.0.0",
  "description": "Helenos service for registration, authentication and authorization of users.",
  "scripts": {
    "tsc": "tsc",
    "dev": "ts-node-dev --respawn --transpileOnly ./server.ts",
    "prod": "tsc && node ./build/server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "notyet"
  },
  "keywords": [
    "Users",
    "Registration",
    "Authentication",
    "Authorization"
  ],
  "author": "Aarón Sánchez",
  "license": "GPL-3.0",
  "devDependencies": {
    "@types/express": "^4.16.1",
    "ts-node-dev": "^1.0.0-pre.32",
    "typescript": "^3.4.4"
  },
  "dependencies": {
    "express": "^4.16.4"
  }
}
```

Empezaré por las dependencias del proyecto:

- **Express.** `npm install express --save`. Instalada como dependencia de proyecto. Es la librería que nos permite enrutar acciones a partir de URL.

- **Typescript**. `npm install typescript --save-dev`. Instalada como librería de desarrollo. Nos permite transpilar el código [`Typescript`](https://www.typescriptlang.org) a `Javascript`. 

- **ts-node-dev**. `npm install ts-node-dev --save-dev`. Instalada como librería de desarrollo. Nos permite lanzar el proyecto en desarrollo y se queda antento para refrescar la transpilación si un fichero se modifica.

- **@types/express**. `npm install @types/express --save-dev`. Instalada como librería de desarrollo. Son los tipos de la librería `express` antes instalada.

Antes de poder ejecutar nada, se debe configurar `Typescript`. Para ello o bien podemos ejecutar el comando `npm run tsc -- --init` o crear un fichero llamado `tsconfig.json` con el siguiente contenido:

```json
{
  "compilerOptions": {

    "target": "es5",
    "module": "commonjs",
    "outDir": "./build",
    "strict": true,
    "esModuleInterop": true
  }
}
```

Y un pequeño fichero `server.ts`  para probar el servicio con el siguiente contenido:

```typescript
import express from "express";

const app: express.Application = express();

app.get('/', (req, res) => {
    res.send('Hello World!');
    console.log("GET: '/'")
});

app.listen(3000, () => {
    console.log("Listening on port 3000");
});
```

Para comprobar que funciona el tinglado que hemos montado podemos crear un fichero `Dockerfile` en el directorio en el que se encuentra el proyecto del servicio:

```
FROM node:lts

WORKDIR /usr/src/app

COPY package*.json ./
RUN npm install
COPY . .

EXPOSE 8080
CMD [ "npm", "run", "dev" ]
```

Por si acaso hemos ejecutado algún comando `npm`, deberíamos crear un fichero `.dockerignore` con el siguiente contenido:

```
node_modules
build
```

Antes de poder lanzar el servicio debemos crear una imagen de `Docker` con el comando `docker build -t helenos/webservice .`. Una vez creada podemos ejecutarla con el comando `docker run -p 3000:3000 helenos/webservice`, este comando toma el control de la terminal y muestra en pantalla la salida de `Docker`. Al acceder a http://localhost:3000 deberiamos ver un "Hello World!", justo lo que estabamos buscando. Para pararlo debemos pulsar `Ctrl + C`. Para borrar la imagen que hemos creado debemos ejecutar el comando `docker rmi helenos/webservice`.

Una vez tenemos disponible el servicio, lo podemos incluir en nuestra base de servicios. Para ello debemos modificar nuestro `docker-compose.yml` para que tenga el siguiente aspecto:

```yaml
version: '3'
services:
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

Al ejecutar el comando `docker-compose up` se crearán las imagenes que corresponden, incluyendo la imagen con el servicio que acabamos de crear.

Al compartir el directorio `service` y lanzarlo en modo desarrollo, cada vez que modificamos un fichero en nuestro equipo, se modifica a su vez en el contenedor y se refresca. Esto se consigue indicando `volumes` en el fichero `docker-compose.yml`.

## Conexión con la base de datos

Para la comunicación con la base de datos voy a utilizar un *ORM*, [`Typeorm`](https://typeorm.io). Sé que hay un gran debate sobre si se deben usar o no estas capas de abstracción. Algún día os comentaré mi opinión al respecto.

Ahora que tenemos un servicio levantado, vamos a conectarlo con la base de datos. Esto nos va a crear algunas depenndencias nuevas en el proyecto:

- **typeorm**. `npm install typeorm --save`. Instalada como dependencia del proyecto. Es un *ORM* en [`Typescript`](https://www.typescriptlang.org) que nos va a facilitar las operaciones con la base de datos.

- **reflect-metadata**. `npm install reflect-metadata --save`. Instalada como dependencia del proyecto. La librería [`Typeorm`](https://typeorm.io) necesita esta dependencia.

- **pg**. `npm install pg --save`. Instalada como dependencia del proyecto. Es el driver que necesitamos para comunicarnos con la base de datos.

- **@types/node**. `npm install @types/node --save-dev`. Instalada como dependencia de desarrollo.

Una vez instaladas las dependencias hay que configurar [`Typescript`](https://www.typescriptlang.org) para que use correctamente el *ORM*, para ello modificaremos el fichero `tsconfig.json` para añadir las siguientes líneas:

```json
"emitDecoratorMetadata": true,
"experimentalDecorators": true
```

El fichero quedaría como algo así:

```json
{
  "compilerOptions": {

    "target": "es5",
    "module": "commonjs",
    "outDir": "./build",
    "strict": true,
    "esModuleInterop": true,
    "emitDecoratorMetadata": true,
        "experimentalDecorators": true
  }
}
```

Ahora que tenemos el entorno de desarrollo preparado vamos a configurar la conexión del servicio con la base de datos. Según la [documentación de Typeorm](https://typeorm.io/#/using-ormconfig), podemos crear un fichero de configuración `ormconfig.json` en el directorio raíz del proyecto del servicio con el siguiente contenido:

```json
{
    "type": "postgres",
    "host": "database",
    "port": 5432,
    "username": "docker",
    "password": "docker",
    "database": "helenosdb",
    "synchronize": true,
    "logging": false,
    "entities": [
       "src/entity/*.ts"
    ],
    "migrations": [
       "src/migration/**/*.ts"
    ],
    "subscribers": [
       "src/subscriber/**/*.ts"
    ],
    "cli": {
       "entitiesDir": "src/entity",
       "migrationsDir": "src/migration",
       "subscribersDir": "src/subscriber"
    }
}
```

Por ahora, y para esta demostración dejaremos este tipo de configuración, pero no es segura para desplegarla en producción, porque tenemos las contraseñas de acceso a la base de datos visibles en la base de código de la aplicación. Para el caso de producción podemos indicar dichas contraseñas en las variables de entorno, tal y como indica la [documentación de Typeorm](https://typeorm.io/#/using-ormconfig).

Ahora debemos realizar los cambios pertinentes para terner un servicio web que registre y autentique usuarios. Antes de ponernos a modificar ficheros a lo loco hay que instalar las dependencias pertinentes:

* **cors**: `npm install cors --save`. Instalada como dependencia de proyecto, añadido por si necesitamos acceder a recursos en otro servidor.

* **@types/cors**: `npm install @types/cors --save-dev`. Instalada como dependencia de desarrollo. 

* **body-parser**: `npm install body-parser --save`. Instalada como dependencia de proyecto, utilizado para parsear la información que vamos a recibir en formato *json*.

* **bcryptjs**: `npm install brcyptjs --save`. Usada para encriptar las contraseñas de los usuarios.

* **@types/bcrypt**: `npm install @types/bcryptjs --save-dev`. Los tipos de [`Typescript`](https://www.typescriptlang.org) de *bcrypt*.

* **moment**: `npm install moment --save`. Librería usada para manejar fechas en la sesión del usuario.

* **jwt-simple**: `npm install jwt-simple --save`. Librería usada para codificar y decodificar tokens de sesión.

Una vez instaladas las dependencias que necesitamos, primero modificamos el fichero `server.ts` para que únicamente lance el servicio:

```typescript
import app from "./app";

const PORT = process.env.PORT || 3000;

app.listen(PORT, () => {
    console.log(`Express server listening on port ${PORT}`);
});
```

La aplicación que lanza dicho servicio se encuentra en el fichero `app.ts`. Esta aplicación se encarga de autenticar cada petición y delegarla a un conjunto de rutas configurada (se especifica más adelante), en caso de no encontrar una ruta en la que delegar la petición, lanza un `404`. También resuelve explícitamente las rutas para registro e inicio de sesión:

```typescript
import express, { Request, Response, NextFunction } from "express";
import cors from "cors";
import * as bodyParser from "body-parser";
import { getConnection } from "typeorm";
import routes from "./routes";
import { UserController } from "./controllers/UserController";
import User from "./entity/User";

const baseUrl: string = process.env.BASE_URL || "/api/v1";

class App {
    public App: express.Application;

    constructor() {
        this.App = express();
        this.config();
    }

    private config() {
        this.App.use(cors());
        this.App.use(bodyParser.json());
        this.App.use(bodyParser.urlencoded({ extended: false }));
        this.App.use(async (req, res, next) => {
            console.log(`${new Date().toLocaleDateString()} - ${req.method}: ${req.url}`);
            await this.authorizeRequest(req, res, next); // This will call next
        });

        // Uses the configured routes
        this.App.use(baseUrl, routes); 

        // If no route is matched, it must be a 404
        this.App.use((req, res, next) => {
            res.status(404).json({ "error": "Endpoint not found" });
            next();
        });        
    }

    private async authorizeRequest(req: Request, res: Response, next: NextFunction) {
        const connection = await getConnection();
        const userController = new UserController(connection.getRepository(User));
        if (req.path.includes(baseUrl + "/user/login")) { return next(); }
        if (req.path.includes(baseUrl + "/user/register")) { return next(); }
        return await userController.Authenticate(req, res, next);
    }
}

export default new App().App;
```

Para resolver las rutas crearemos un directorio llamado `/routes` con los ficheros `index.ts`, `UserRoute.ts` y `ProtectedRoute.ts`. El fichero `UserRoute.ts` delega las acciones asociadas a las rutas `/register` y `/login` a las acciones correspondientes del controlador `UserController.ts` (definido más adelante), que a su vez usa `/entity/Token.ts` y `/entity/User.ts`, que no son más que las entidades que se van a usar en los controladores. El especial es `User.ts` ya que se usa para mapear usuarios a la base de datos. A continuación el contenido de los ficheros:

`index.ts` del directorio `/routes`:

```typescript
import { Router } from "express";
import UserRoute from "./UserRoute";
import ProtectedRoute from "./ProtectedRoute";

const router = Router();

router.use("/user", UserRoute);

router.use("/protected", ProtectedRoute);

export default router;
```

Este fichero tiene dependencias de `UserRoute.ts`:

```typescript
import { Request, Response, Router } from "express";
import { createConnection } from "typeorm";
import { UserController } from "../controllers/UserController";
import User from "../entity/User";

const router = Router();

createConnection().then(connection => {
    const userController = new UserController(connection.getRepository(User));

    router.route("/register")
        .post((req: Request, res: Response) => {
            userController.Register(req,res);
        });

    router.route("/login")
        .post((req: Request, res: Response) => {
            userController.Login(req,res);
        });

}).catch(err => console.error(`Could not create connection for users: ${err}`));

export default router;
```

`ProtectedRoute.ts`:

```typescript
import { Router, Response } from "express";
import IRequest from "../utils/Request";

const router = Router();

router.route("/")
    .all((req: IRequest, res: Response) => {
        res.status(200).json({ message: `You reached the protected zone! ${req.user!.UserName}` });
    });

export default router;
```

`User.ts`:

```typescript
import { Entity, Column, PrimaryColumn, Generated, BeforeInsert } from "typeorm";
import * as bcrypt from "bcrypt";

@Entity()
export default class User {
    @PrimaryColumn()
    @Generated("uuid")
    public Id: string;

    @Column({
        unique: true
    })
    public UserName: string;

    @Column()
    public Password: string;

    @BeforeInsert()
    public EncriptPassword(): void {
        this.Password = bcrypt.hashSync(this.Password, 10);
    }

    public async ComparePassword(candidatePassword: string): Promise<boolean> {
        return bcrypt.compare(candidatePassword, this.Password, (err) => {
            if(err) { return false; }
            return true;
        });
    }
}
```

Esta clase usa una notación especial para indicarle a *typeorm* como se debe mapear la entidad en la base de datos. Para más información [en su web](https://typeorm.io).

Varios de los ficheros que se han especificado anteriormente usan `UserController.ts` que se encuentra en el directorio `/controllers`:

```typescript
import { Repository } from "typeorm";
import User from "../entity/User";
import { Request, Response, NextFunction } from "express";
import moment from "moment";
import jwt from "jwt-simple";
import Token from "../entity/Token";
import IRequest from "../utils/Request";

export class UserController {

    private readonly repository: Repository<User>;

    public constructor(userRepository: Repository<User>) {
        this.repository = userRepository;
    }

    public async Authenticate(req: IRequest, res: Response, next: NextFunction) {
        const token = req.header("Authorization");
        if (token === undefined) {
            res.status(401).json({ message: "No token provided."});
            return;
        }

        try {
            const tokenSplit = token.split(" ");

            if (tokenSplit.length !== 2) {
                res.status(401).json({ message: "Bad token format." });
                return;
            }

            if (tokenSplit[0] !== "Bearer") {
                res.status(401).json({ message: "Bad token type."});
                return;
            }


            const tokenobj: Token = jwt.decode(tokenSplit[1], process.env.JWT_SECRET!);                    
            const foundUser = await this.repository.findOne({ Id: tokenobj.User });            

            if (foundUser === null || foundUser === undefined) {
                res.json(404).json({ message: 'User in the token not found.'});
                return;
            }

            req.user = foundUser;            
        } catch(err) {
            res.status(500).json({ message: `Internal server error: ${err}`});
            return;
        }
        next();
    }

    public async Register(req: Request, res: Response) {
        try {
            const errors: string[] = this.checkUserRequest(req.body);
            if (errors.length !== 0) {
                res.status(400).json({ message: errors.toString()});
                return;
            }

            if (await this.repository.findOne({ UserName: req.body.UserName }) !== undefined){
                res.status(400).json({ message: 'User name already in use'});
                return;
            }


            const user = this.repository.create(req.body as User);
            await this.repository.save(user);

            res.status(201).json({user: user.Id});
        } catch (error) {
            console.error(`Error registering user: ${error}`);
            res.status(500).json(error);
        }
    }

    public async Login(req: Request, res: Response) {
        try {
            const errors: string[] = this.checkUserRequest(req.body);
            if (errors.length !== 0) {
                res.status(400).json({ message: errors.toString()});
                return;
            }

            const foundUser = await this.repository.findOne({ UserName: req.body.UserName });
            if(foundUser === null || foundUser === undefined) {
                return false;
            }

            const success = await foundUser.ComparePassword(req.body.Password);
            if (success === false) {
                return res.status(401).json({ "message": "Invalid credentials" });
            }

            return res.status(200).json(this.genToken(foundUser));
        } catch (error) {
            console.error(`Error loging in: ${error}`);
            return res.status(500);
        }
    }

    private checkUserRequest = (body: any): string[] => {
        const errors: string[] = [];
        if (body.UserName === undefined) {
            errors.push('UserName cannot be empty');
        }

        if(body.Password === undefined) {
            errors.push('Password cannot be emty');
        }

        return errors;
    }

    private genToken = (user: User): Token => {
        const validDays: number =  Number(process.env.EXPIRE_TOKEN_TIME) || 7;
        const expires = moment().utc().add({ days: validDays }).unix();
        const token = jwt.encode({
            exp: expires,
            User: user.Id,
        }, process.env.JWT_SECRET!);

        return {
            Token: "Bearer " + token,
            Expires: moment.unix(expires).format(),
            User: user.Id
        };
    }
}
```

Esta clase es la más extensa de todas. Así que vamos a verla poco a poco. Es una clase típica de controlador en la que se inyecta el repositorio de usuarios que se va a usar. Los métodos que se usan como delegados de las llamadas de `server.ts` deben recibir los parámetros `request` y `response`. 

El método `Athorize` es especial porque no es un método final, es un método de **middleware**. 

El método `Register` registra un nuevo usuario en la base de datos y devuelve su id generado. Si el nombre de usuario ya está en uso devuelve un error.

El método `Login` devuelve un token de sesión para un usuario dado si el usuario y contraseña son correctos. Dicho token es el que se deberá usar para autenticarse en futuras llamadas.

Tanto `UserController.ts` como `ProtectedRoute.ts` tienen una dependencia a `IRequest.ts`:

```typescript
import { Request } from "express";
import User from "../entity/User";

interface IRequest extends Request {
    user?: User;
}

export default IRequest;
```

Esto es porque necesitamos saber en los métodos protegidos, cual es el usuario autenticado que ha realizado la llamada.

La configuración del servidor se encuentra en el fichero `.env` en el directorio raíz del proyecto del servidor:

```
BASE_URL='/api/v1'
EXPIRE_TOKEN_TYPE=7 #In days
JWT_SECRET='dfswe43*c-sdf3c'
```

Ahora deberíamos cambiar el fichero `Dockerfile` del servicio web para que ejecute un pequeño script de lanzamiento de servidor. Así podemos controlar en que entorno nos encontramos mediante la variable de entorno `DEPLOY_ENV` que se ha definido en `docker-compose.yml`: 

```
FROM node:lts

# Create app directory
WORKDIR /usr/src/app

# Copy the project and install dependencies
COPY . .

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

RUN npm run build

EXPOSE 8080
CMD [ "./init.sh" ]
```

El script `init.sh` debe estar en el mismo directorio que el fichero `Dockerfile`:

```sh
Environment="${DEPLOY_ENV:-prod}"

if [ $Environment = "prod" ]
then
    echo "Launching in production mode."
    npm run prod
else
    echo "Launching in development mode."
    npm run dev
fi
```

Llegados a este punto, si ejecutamos `docker-compose up` desde el directorio raíz se debería lanzar el servidor y la base de datos conectados y funcionando. Según la configuración indicada en el fichero `docker-compose.yml`, el puerto expuesto del servicio es el 3000, por lo que si accedemos a las rutas configuradas deberíamos poder registrarnos, loguearnos y acceder a rutas protegidas.

Nos vemos en las próximas líneas.