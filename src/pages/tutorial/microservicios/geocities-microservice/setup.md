---
layout: default
title: Geocities Microservice Setup
permalink: /Tutorial/Microservicios/Geocities/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/eg.geocities.management/tree/development . En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/geocities-microservice` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:

```
microservices-tutorial
│
└─── geocities-microservice
│   │   README.md
│   │   .gitignore
```

3. (Opcional) Para inicializar este proyecto de TypeScript, utilizamos el [NX Framework](https://nx.dev/getting-started/intro). Este pone en practica un término llamado monorepositorio.
  - Para crear la estructura puede utilizar el siguiente comando `npx create-nx-workspace@latest`
  - La consola va a ir indicando las opciones a seleccionar para crear la solucion necesaria.


4. Para la ejecucion de este proyecto, recomendamos descargar el repositorio adentro de la estructura anteriormente descrita. 
  - Esto lo puede realizar mediante la utilización de `git`
  - [Link al repo](https://github.com/tfg2021-escinf-una/eg.geocities.management)


### Análisis de la estructura del repositorio.

```
microservices-tutorial
│
└─── geocities-microservice
│   │   apps
│   │   deployment
│   │   libs
│   │   tools
│   │   README.md
│   │   .gitignore
```

-  La estructura anteriormente descrita, propone una serie de orden a la hora de desarrollar el código 
    - ```apps``` es la carpeta donde internamente se encuentra todas las applicaciones que este repositorio contiene.
    - ```deployment``` contiene los archivos necesarios para efectuar el despliegue en el cluster de kubernetes.

### Análisis de la estructura dentro de apps.
```
apps
└─── geography
│   │   src
│   │   │   main.ts
│   │   │   app
│   │   │   │   controllers
│   │   │   │   routes
│   │   │   │   utils

```

- La carpeta app posee una serie de carpetas, cuyo objetivo es lograr segregar la funcionalidad para lograr obtener un API sencillo. 
- Para este ejemplo en especifico, se utilizaron tres carpetas claves
    - controllers (Aquí se encuentran las funciones que consumen otros servicios y nos retornan información serializada en JSON)
    - routes (Aquí se encuentra la configuración de la ruta, donde se especifica como se debe acceder y cual función del controller debe ejecutar)
    - utils (En dicho directorio se encuentra un wrapper sobre [Axios](https://axios-http.com/docs/intro) para simplificar los llamados)

### Ejemplo de una funcion del controlador

```
const {
  APIBaseUrl,
  APIHost,
  APIKey
} = environment

export const getCountries = async (req : Request, res : Response) => {
  const reqQueryParams = {
    currencyCode : req.query?.currencyCode,
  };

  const response = await requestHandler({
    type: 'GET',
    baseUrl: APIBaseUrl,
    endpoint: 'v1/geo/countries',
    params : reqQueryParams,
    headers: {
      'X-RapidAPI-Host': APIHost,
      'X-RapidAPI-Key': APIKey
    }
  });

  return res.status(response.statusCode)
    .json(response);
};
```

- Dicha funcion realiza una llamada a un servicio externo y nos retorna el resultado utilizando el formato JSON.

### Definicion de una ruta

```
export const useCountryRoutes = (app : Application) => {
  app.use((_req : Request, res : Response, next : NextFunction) => {
    res.header(
      'Access-Control-Allow-Headers',
      'x-access-token, Origin, Content-Type, Accept'
    );
    next();
  });

  app.get(`api/v1/countries/`, [
    //middleware in needed case.
  ], getCountries);
}
```

- Dicha función establece que cuando el usuario realiza una petición a `/api/v1/countries`, la función `getCountries` definida en el controlador será ejecutado.
- Finalmente, la funcion padre, `useCountryRoutes`, debe ser ejecutada para concluir el proceso. 

### Punto de entrada de la aplicacion. 

```
import * as express from 'express';
import { useCountryRoutes } from './app/routes';
import { environment } from './environment';
import { useSwagger } from './swagger';
import bodyParser = require('body-parser');

const { ProductionMode } = environment
const app = express();

app.use(bodyParser.json());

// liveness route
app.get('/', (_req, res) => {
  res.send('Web API running')
})

if(!ProductionMode)
  useSwagger(app);
useCountryRoutes(app);

// server configuration
const port = process.env.port || 3000;
const server = app.listen(port, () => {
  console.log(`ProductionMode: ${ProductionMode}`)
  console.log(`Listening at http://localhost:${port}/api`);
});
server.on('error', console.error);
```

- El código descrito con anterioridad, se encuentra en el archivo `main.ts`, el cual es donde se crea la aplicación mediante el uso de express. 

5. Ejecución del servidor
  - Esto se logra mediante la ejecución del siguiente comando:

```
  npx nx serve
```

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto `3000`.

[Anterior]({% link src/pages/tutorial/microservicios/geocities-microservice/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
