---
layout: default
title: Identity Management Microservice Setup
permalink: /Tutorial/Microservicios/Idp/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/eg.identity.management . En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/identity-management-microservice` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:

```
microservices-tutorial
│
└─── identity-management-microservice
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
└─── identity-management-microservice
│   └─── src
│   │   │   ProjectName.csproj
│   │   │   Repositories
│   │   │   Services
│   │   │   Controllers
│   │   │   Program.cs
│   │   deployment
│   │   test
│   │   README.md
│   │   .gitignore
│   │   LICENSE
```

### Análisis de la estructura propuesta.

-  La estructura anteriormente descrita, propone una serie de orden a la hora de desarrollar el código 
    - ```src``` es la carpeta donde internamente se encuentra todo el código fuente que es el que genera que la aplicación funcione.
    - ```test``` es la carpeta que contiene los proyectos de pruebas de unidad e integracion. 
    - ```deployment``` contiene los archivos necesarios para efectuar el despliegue en el cluster de kubernetes.

### Análisis de la estructura dentro de src.
```
src
└─── identity-management-microservice
│   │   ProjectName.csproj
│   │   Repositories
│   │   Services
│   │   Controllers
│   │   Program.cs

```
- La carpeta src posee una serie de carpetas, cuyo objetivo es lograr segregar la funcionalidad para lograr obtener un API sencillo. 
- El patrón utilizado en este tutorial es el [Repository-Pattern](https://exceptionnotfound.net/the-repository-service-pattern-with-dependency-injection-and-asp-net-core/#:~:text=The%20Repository%2DService%20pattern%20breaks,against%20a%20single%20Model%20class.) 
    - Básicamente se obtiene al utilizar la inyeccion de dependencias a un contenedor para mediante el [Constructor-Injector](https://freecontent.manning.com/understanding-constructor-injection/) inyectar la dependencia. 
    - Una vez comprendido la inyección de dependencias vamos a crear los repositorios, estos serán los encargados de realizar la conexión a la base de datos y posteriormente retornar la información requerida.
    - Posteriormente se crean los servicios, estos tendrán una referencia al repositorio, por ende, este simplemente es como un tipo de encapsula donde uno manipula los datos obtenidos del repositorio.
    - Finalmente se crean los controladores. Estos tendrán referencias a los servicios, los cuales le retornan los datos debidamente ordenados para retornarlos en JSON.

5. Implementación de piezas fundamentales en un API no minimalista.     

### Ejemplo de un controlador
```
using AutoMapper;
using EG.IdentityManagement.Microservice.Entities.ViewModels;
using EG.IdentityManagement.Microservice.Identity;
using EG.IdentityManagement.Microservice.Services.Interfaces;
using Microsoft.AspNetCore.Mvc;
using System.Threading.Tasks;

namespace EG.IdentityManagement.Microservice.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class UserController : ControllerBase
    {
        private readonly IIdentityService _identityService;
        private readonly IMapper _mapper;

        public UserController(IIdentityService identityService,
                              IMapper mapper)
        {
            _identityService = identityService;
            _mapper = mapper;
        }

        [HttpPost]
        [Route("login")]
        public async Task<IActionResult> Login([FromBody] CredentialsViewModel credentials)
            => await _identityService.Login(credentials.EmailAddress, credentials.Password);

        [HttpPost]
        [Route("register")]
        public async Task<IActionResult> Register([FromBody] IdentityViewModel user)
            => await _identityService.Register(_mapper.Map<User>(user));

        [HttpPost]
        [Route("refresh")]
        public async Task<IActionResult> Refresh([FromBody] TokensViewModel token)
            => await _identityService.Refresh(token.jwtToken, token.refreshToken);
    }
}  
```

- Dicho controlador, tiene dos referencias inyectadas, un mapper y un servicio.
- Se definen tres diferentes rutas:
    - login
    - register
    - refresh
- Cada una de estas rutas, retorna el resultado de efectuar el llamado a un método en especifico.



### Ejemplo de un servicio
```
using EG.IdentityManagement.Microservice.Customizations.Identity;
using EG.IdentityManagement.Microservice.Entities;
using EG.IdentityManagement.Microservice.Entities.Const;
using EG.IdentityManagement.Microservice.Identity;
using EG.IdentityManagement.Microservice.Services.Interfaces;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Mvc;
using System;
using System.Collections.Generic;
using System.Dynamic;
using System.Linq;
using System.Net;
using System.Threading.Tasks;

namespace EG.IdentityManagement.Microservice.Services.Implementations
{
    public class IdentityService : IIdentityService
    {
        private readonly SignInManager<User> _signInManager;
        private readonly CustomUserManager<User> _userManager;

        public IdentityService(SignInManager<User> signInManager, CustomUserManager<User> userManager)
        {
            _signInManager = signInManager ?? throw new ArgumentNullException("SignInManager property should not come as null");
            _userManager = userManager ?? throw new ArgumentNullException("UserManager property should not come as null");
        }

        public async Task<IActionResult> Register(User user)
        {
            dynamic data = new ExpandoObject();
            var errors = new List<List<IdentityError>>();
            var registerResult = await _userManager.CreateAsync(user, user.Password);

            if (registerResult.Succeeded)
            {
                data.IdAssigned = user.Id;
                var roleResult = await _userManager.AddToRoleAsync(user, Constants.ApplicationRoles.StandardUser.ToString());

                if (roleResult.Succeeded)
                    data.RoleAssigned = Constants.ApplicationRoles.StandardUser.ToString();
                else
                    errors.Add(roleResult.Errors.ToList());
            }
            else
            {
                errors.Add(registerResult.Errors.ToList());
            }

            if (errors.Count != 0)
            {
                return new BadRequestObjectResult(new GenericResponse
                {
                    Errors = errors,
                    Data = data,
                    StatusCode = HttpStatusCode.BadRequest
                });
            }

            return new CreatedResult("/", new GenericResponse
            {
                Errors = errors,
                Data = data,
                StatusCode = HttpStatusCode.Created
            });
        }
    }
}
```
- Dicho servicio, tiene dos referencias inyectadas, un SignInManager y un CustomUserManager. Estas dos son referencias que .NET utiliza y uno como desarrollador puede sobreescribir.
- Se definen tres diferentes métodos: [Ver mas](https://github.com/tfg2021-escinf-una/eg.identity.management/blob/development/src/Services/Implementations/IdentityService.cs)
    - Login
    - Register
    - Refresh
- Cada una de estos métodos, retorna un Object Result que será serializado a JSON.
- Por otro lado, estos llaman a las dependencias `CustomUserManager` o `SignInManager` dependiendo del propósito del método


### Ejemplo de un repositorio
```
using EG.IdentityManagement.Microservice.Settings;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;
using MongoDB.Driver;
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace EG.IdentityManagement.Microservice.Repositories
{
    [ExcludeFromCodeCoverage]
    public class MongoRepository<T> : IMongoRepository<T>
    {
        private readonly MongoDbSettings _mongoDbSettings;
        private readonly IMongoClient _mongoClient;
        private readonly ILogger<MongoRepository<T>> _logger;
        private readonly string _collectionName;

        public MongoRepository(IOptions<MongoDbSettings> mongoDbSettings,
                               ILogger<MongoRepository<T>> logger,
                               IMongoClient mongoClient)
        {
            _mongoDbSettings = mongoDbSettings.Value ?? throw new ArgumentNullException("Value should not come as null");
            _mongoClient = mongoClient ?? throw new ArgumentNullException("Value should not come as null");
            _logger = logger;
            _collectionName = $"{typeof(T).Name}s";
        }

        public async Task<bool> InsertOneAsync(T obj)
        {
            try
            {
                var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
                var collection = database.GetCollection<T>(_collectionName);
                await collection.InsertOneAsync(obj);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, $"An unhandled error has occurred while inserting a document in the collection: {_collectionName}");
                return false;
            }

            return true;
        }

        public async Task<DeleteResult> DeleteOneAsync(FilterDefinition<T> filter, CancellationToken cancellationToken = default)
        {
            var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
            var collection = database.GetCollection<T>(_collectionName);
            return await collection.DeleteOneAsync(filter, cancellationToken);
        }

        public T Find(FilterDefinition<T> filter, FindOptions options = null)
        {
            var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
            var collection = database.GetCollection<T>(_collectionName);
            return collection.Find(filter, options)
                    .FirstOrDefault();
        }

        public async Task<T> FindOneAndUpdateAsync(FilterDefinition<T> filter, UpdateDefinition<T> update, CancellationToken cancellationToken = default)
        {
            var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
            var collection = database.GetCollection<T>(_collectionName);
            return await collection.FindOneAndUpdateAsync(
                filter,
                update,
                options: new FindOneAndUpdateOptions<T>
                {
                    // Do this to get the record AFTER the updates are applied
                    ReturnDocument = ReturnDocument.After
                });
        }

        public async Task<TNewResult> FindOneLookupAsync<TResult, TForeignDocument, TNewResult>(
            string foreignCollection,
            FieldDefinition<T> localField,
            FieldDefinition<TForeignDocument> foreignField,
            FieldDefinition<TNewResult> @as,
            FilterDefinition<T> filter)
        {
            var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
            var collection = database.GetCollection<T>(_collectionName);
            return await collection
                       .Aggregate()
                       .Match(filter)
                       .Lookup(foreignCollection,
                              localField,
                              foreignField,
                              @as)
                      .FirstOrDefaultAsync();
        }

        public async Task<IEnumerable<T>> Find(Expression<Func<T, bool>> filter, FindOptions options = null)
        {
            var database = _mongoClient.GetDatabase(_mongoDbSettings.DatabaseName);
            var collection = database.GetCollection<T>(_collectionName);
            return await collection.Find(filter, options)
                    .ToListAsync();
        }
    }
}
```

- Dicho repositorio tiene una serie de referencias que permiten realizar una conexion a la base de datos de Mongo.

5. Una vez calzadas todas estas piezas, es posible compilar la solucion sin errores.
6. Para definir las variables de ambiente es necesario utilizar el archivo denominado `appsettings.json`
    - Aqui deben ir todas la variables relacionadas a las conexiones de la base de datos.

```json 
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*"
}
```

7. Ejecutar el servidor

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto `por definir`.

[Anterior]({% link src/pages/tutorial/microservicios/identity-management-microservice/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
