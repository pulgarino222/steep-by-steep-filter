# steep by steep for filter node.js with typescript

    npm init
    

 - description: we crate a appi restfull that allow conect with a database in mysql relational and magment all information
 - keywords: restfull nodejs typescript javascript decorators seqlize
 - author: juan felipe pulgarin hernandez pulgarino222
### libraries used
 -    **express**: Framework web para Node.js.
-   **sequelize**: ORM para Node.js.
-   **sequelize-typescript**: Decoradores de TypeScript para Sequelize. Nos permite:
    -   Definir modelos con clases de TypeScript.
    -   Definir relaciones entre modelos con decoradores.
    -   Definir validaciones con decoradores.
-   **mysql2**: Driver de MySQL para Node.js.
-   **nodemon**: Herramienta que ayuda a desarrollar aplicaciones basadas en Node.js reiniciando automáticamente la aplicación cuando se detectan cambios en el código fuente.
-   **ts-node**: Ejecuta TypeScript directamente en Node.js sin necesidad de compilarlo previamente.
-   **typescript**: Lenguaje de programación que añade tipado estático a JavaScript.
-   **tsyringe**: Contenedor de inyección de dependencias para TypeScript.
-   **@types/express**: Definiciones de tipos para Express. Recordemos que @types significa que es un paquete de definiciones de tipos TypeScript.
-   **@types/sequelize**: Definiciones de tipos para Sequelize.
-   **@types/node**: Definiciones de tipos para Node.js.
-   **dotenv**: configura las variables de entorno en un archivo seguro .env

### install dev dependencies

    npm i typescript ts-node @types/node nodemon @types/sequelize @types/node @types/jsonwebtoken -D

### install  dependencies

    npm i tsyringe trying sequelize-typescript sequelize jsonwebtoken express @types/express dotenv mysql2


### start TypeScript file
```bash
npx tsc --init
```
### Edit file `tsconfig.json` for config TypeScript
```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

### Config Nodemon for help to develop
Crear un archivo `nodemon.json` en la raíz del proyecto con el siguiente contenido:

    touch nodemon.json

```json
{
  "watch": ["src"],
  "ext": "ts",
  "exec": "ts-node src/index.ts"
}
```
### Add un script in the `package.json` for init the server with Nodemon:
```json
"scripts": {
  "start": "nodemon"
}
```

### Create the file for config environment variables `.env`
```dotenv
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=password
DB_NAME=mydatabase
PORT=3000
```
### create a folder src and add a file index.ts
npm start and after if this code show a error it's fine 

    let  letter:string
    letter=2
and if this code show hola it's fine

    let  letter:string
    letter='hola'
    console.log(letter)

### conection with data base
creamos una carpeta config src/config/db.js

    import { Sequelize } from  'sequelize-typescript';
    import  env  from  'dotenv'
    env.config()
    const  sequelize:  Sequelize  =  new  Sequelize({
    dialect:  'mysql',
    host:  process.env.DB_HOST,
    username:  process.env.DB_USER,
    password:process.env.DB_PASSWORD,
    database:  process.env.DB_NAME
    // models: [User, Product], // Añade todos tus modelos aquí
    });
    export  default  sequelize;


### up server trought  index.ts
despues de crear la conexion a la base de datos probamos que realmente si se este conectando con un try catch 

    import  express  from  'express'
    import  env  from  'dotenv'
    import  sequelize  from  "./config/db";
    const  server=express()
    // server.use(express.json())//descomentar cuando empiece a usar req
    env.config()
    const  PORT=  process.env.PORT  ||  3001
    const  startserver=async()=>{
    try {
    await  sequelize.authenticate()
    await  sequelize.sync()
    console.log("Database connected!")
    server.listen(PORT,()=>{
    console.log(`server executted in http://localhost:${PORT}`)
    })
    } catch (error:any) {
    console.log(`somethings wrong from index.ts`,error)
    }
    }
    startserver()
    
    
