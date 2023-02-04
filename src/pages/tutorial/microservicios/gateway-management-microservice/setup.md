---
layout: default
title: Gateway Microservice Setup
permalink: /Tutorial/Microservicios/Gateway/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/eg.gateway.management . En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/gateway-management-microservice` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:

```
microservices-tutorial
│
└─── gateway-management-microservice
│   │   README.md
│   │   .gitignore
│   │   LICENSE
```

3. Para inicializar el proyecto de C# recomendamos utilizar Visual Studio e utilizar el template que la aplicacion ofrece para la construccion del archivo.

    Por lo general, los proyectos basados en C#, generan un archivo con la extension `.csproj`, en esta es donde se instalan las dependencias necesarias para su adecuado 
    funcionamiento.   
    Un ejemplo de un archivo `.csproj` es el siguiente: [.csproj](https://github.com/tfg2021-escinf-una/eg.identity.management/blob/development/src/EG.IdentityManagement.Microservice.csproj)

4. Una vez que Visual Studio a creado la estructura de carpetas, vamos a proceder a efectuar un breve cambio en dicha estructura, cabe resaltar, que esta es meramente organizacional y opcional. 

```
microservices-tutorial
│
└─── gateway-management-microservice
│   └─── src
│   │   │   ProjectName.csproj
│   │   │   Routes
│   │   │   ocelot.json
│   │   │   Program.cs
│   │   deployment
│   │   README.md
│   │   .gitignore
│   │   LICENSE
```

### Análisis de la estructura propuesta.

-  La estructura anteriormente descrita, propone una serie de orden a la hora de desarrollar el código 
    - ```src``` es la carpeta donde internamente se encuentra todo el código fuente que es el que genera que la aplicación funcione.
    - ```deployment``` contiene los archivos necesarios para efectuar el despliegue en el cluster de kubernetes.

### Análisis de la estructura dentro de src.
```
src
└─── gateway-management-microservice
│   │   ProjectName.csproj
│   │   Routes
│   │   ocelot.json
│   │   Program.cs

```
- La carpeta src posee una serie de carpetas, cuyo objetivo es lograr segregar la funcionalidad para lograr obtener un API sencillo. 
- Para este ejemplo en especifico, se utilizó una libreria llamada [Ocelot](https://ocelot.readthedocs.io/en/latest/introduction/gettingstarted.html) para diseñar un gateway. 
    - Un gateway es un elemento que recibe todos las peticiones de una aplicacion y este la mapea al API correspondiente.

5. Implementación de piezas fundamentales en un API utilizando Ocelot.     
    - Basicamente consiste en definir las rutas a las que el gateway debe redireccionar, estas estan escritas en un archivo .json, y posteriormente en el Program.cs se debe inyectar una dependencia.
    - En nuestro caso definimos una carpeta llamada `Routes` que contienen archivos JSON con las rutas, a continuacion mostramos un breve ejemplo:

```json
{
  "Routes": [
    {
      "DownstreamPathTemplate": "/api/v1/countries",
      "DownstreamScheme": "https",
      "DownstreamHostAndPorts": [
        {
          "Host": "eg-geocities-management-dev-tfg2021-escinf-una.cloud.okteto.net",
          "Port": 443
        }
      ],
      "UpstreamPathTemplate": "/geocities/countries",
      "UpstreamHttpMethod": [ "GET" ]
    }
  ],
  "GlobalConfiguration": {
    "BaseUrl": "*"
  }
}
```

6. Dentro del Program.cs, configurar Ocelot.
### Configuracion de Ocelot. 
```
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using Ocelot.DependencyInjection;
using Ocelot.Middleware;
using System.IO;
using EG.Gateway.Microservice.Extensions;
using Microsoft.AspNetCore.Builder;
using Microsoft.Extensions.DependencyInjection;
using System;

namespace EG.Gateway.Microservice
{
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateHostBuilder(string[] args) =>
            new WebHostBuilder()
                .UseKestrel()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .ConfigureAppConfiguration((hostingContext, config) =>
                { 
                    config.SetBasePath(hostingContext.HostingEnvironment.ContentRootPath)
                        .AddJsonFile("appsettings.json", true, true)
                        .AddOcelot($"./Routes/{hostingContext.HostingEnvironment.EnvironmentName}/", hostingContext.HostingEnvironment)
                        .AddEnvironmentVariables();
                        
                })
                .ConfigureServices(services =>
                {
                    services.AddJwtAuthentication();
                    services.AddOcelot();
                    services.AddCors();
                })
                .ConfigureLogging((hostingContext, logging) =>
                {
                    logging.AddConsole();
                })
                .Configure(app  =>
                {
                    app.UseCors(x => x
                     .AllowAnyMethod()
                     .AllowAnyHeader()
                     .SetIsOriginAllowed(origin => true)
                     .AllowCredentials());
                    app.UseOcelot().Wait();
                });
    }
}
```
7. Ejecutar el servidor

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto `por definir`.

[Anterior]({% link src/pages/tutorial/microservicios/gateway-management-microservice/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
