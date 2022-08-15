---
layout: default
title: Docker en Covid-Info Microservice
permalink: /Tutorial/Containers/CovidInfo
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en Covid-Info Microservice

1. Abrir el repositorio `microservices-tutorial/covid-info-microservice` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM golang:1.18-alpine

WORKDIR /app

COPY go.mod ./
COPY go.sum ./
COPY main.go ./
ADD docs ./docs

RUN go mod download

RUN go build -o /covid-info

EXPOSE 8080

CMD [ "/covid-info" ]
```

- Se usa la versión Alpine de Golang como base de la imagen.
- Se copian únicamete los archivos necesarios.
- Se ejecuta la instalación local de las dependencias del archivo ```go.mod```.
- Se construye la aplicación con el nombre ```covid-info```.
- Se expone el puerto 8080.
- Se ejecuta el comando ```/covid-info``` que dará inicio al servidor construido en el contenedor.

Una vez realizados estos pasos, se puede proceder con la ejecución de los comandos de Docker para crear la imagen y para correrla en el equipo:

```
docker build -t covid-info-microservice .
docker run covid-info-microservice
```

[Anterior]({% link src/pages/tutorial/docker/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/docker/docker_notification.md %}){: .btn-next}