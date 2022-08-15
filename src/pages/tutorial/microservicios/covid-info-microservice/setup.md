---
layout: default
title: Covid-Info Microservice Setup
permalink: /Tutorial/Microservicios/CovidInfo/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/covid-info/ . En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/covid-info-microservice` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:

```
microservices-tutorial
│
└─── covid-info-microservice
│   │   README.md
│   │   .gitignore
│   │   LICENSE
```

3. Para inicializar el proyecto de Go es necesario crear un módulo para manejar las dependencias
``
go mod init covid-info
``
En este archivo se pueden ver todas las dependencias que se instalan para el proyecto. Un ejemplo del archivo go.mod es el siguiente: [go.mod](https://github.com/tfg2021-escinf-una/covid-info/blob/main/go.mod)

4. Cree el archivo main.go y copie el contenido del siguiente [link: main.go](https://github.com/tfg2021-escinf-una/covid-info/blob/6c558b2fd2442126d63177fdb82df2fd9f357551/main.go).

### Análisis del código copiado

```
import (
	docs "covid-info/docs"
	"encoding/json"
	"io/ioutil"
	"net/http"
	"os"
    "fmt"

	"github.com/gin-gonic/gin"
	"github.com/swaggo/files"
	"github.com/swaggo/gin-swagger"
)
```

- Se realiza el import de las dependencias para ejecutar el microservicio. Las más importantes se explican a continuación: 
    - ```os``` permite la utilización de features del Sistema Operativo.
    - ```net/http``` se utiliza para hacer llamados HTTP.
    - ```io/ioutil``` permite el manejo de operaciones de Entrada y Salida.
    - ```fmt``` permite el manejo de operaciones de Entrada y Salida.
    - ```github.com/gin-gonic/gin``` Framework para la creación de APIs.
    - ```github.com/swaggo/gin-swagger``` Facilita la creación de documentación para los endpoints del API.

```
var API_HOST string
var API_KEY string
var API_URL string
```

- Son variables de ambiente que se utilizarán para almacenar la información del API que será consumido por Go y que trae la información relacionada a las noticias y vacunas relacionadas al Covid-19.

```
func main() {
    router := gin.Default()
	router.Use(CORS)
    docs.SwaggerInfo.BasePath = "/api/v1"
    v1 := router.Group("/api/v1")
    API_HOST = os.Getenv("API_HOST")
    API_KEY = os.Getenv("API_KEY")

    router.GET("/vaccines", getVaccines)
    router.GET("/worldData", getWorldData)
	router.GET("/news", getCovidNews)
	router.GET("/liveness", liveness)

    router.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler))
    router.Run("0.0.0.0:8080")
}
```

- Se crea el API router utilizando Gin.
- Se usa el decorador CORS para el router creado en el punto anterior.
- Se definen instrucciones para la creación de la documentación de los endpoints
- Se obtienen los valores del HOST y KEY del API.
- Se definen los URL y los métodos relacionados a los endpoints.
- Se ejecuta el API en el puerto 8080 del equipo.

```
func getVaccines(c *gin.Context) {
	url := fmt.Sprintf("%s/vaccines/get-all-vaccines", API_URL)
	req, _ := http.NewRequest("GET", url, nil)
	req.Header.Add("X-RapidAPI-Host", API_HOST)
	req.Header.Add("X-RapidAPI-Key", API_KEY)
	res, err := http.DefaultClient.Do(req)
    errorValidation(err, c)
	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)
    var data any
    json.Unmarshal(body, &data)
    c.IndentedJSON(http.StatusOK, data)
}
func getWorldData(c *gin.Context) {
    ...
}
func getCovidNews(c *gin.Context) {
    ...
}
```
- Se completa el URL del request con el dominio almacenado en la variable API_URL
- Se crea un nuevo request de tipo GET para traer la información.
- Se completan los headers requeridos por el API usando las variables de entorno.
- Se completa el request HTTP, se obtiene la respuesta del servidor.
- Con la respuesta, se hace el formato JSON del endpoint.
- Se responde la solicitud con el Status OK (200) y la información obtenida del endpoint consultado.

5. Instalar las dependencias del go.mod
``
go get .
``
6. Declarar las variables de entorno API_HOST y API_KEY con la información obtenida de RapidAPI.

7. Ejecutar el servidor
``
go run .
``

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto 8080.

[Anterior]({% link src/pages/tutorial/microservicios/covid-info-microservice/rapid_api_info.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
