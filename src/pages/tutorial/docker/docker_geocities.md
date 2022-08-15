---
layout: default
title: Docker en Geocities Microservice
permalink: /Tutorial/Containers/Geocities
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en Geocities Microservice

1. Abrir el repositorio `microservices-tutorial/geocities-microservice` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM node:16.13.1 as build-env
LABEL maintainer=tfg2021.escinf.una@gmail.com

# Building the container

EXPOSE 3000
WORKDIR /app

COPY /package.json .
RUN npm install

COPY /dist/apps/geography .

ENTRYPOINT ["node", "./main.js"]
```
# TODO: Explicación

Una vez realizados estos pasos, se puede proceder con la ejecución de los comandos de Docker para crear la imagen y para correrla en el equipo:

```
docker build -t geocities-microservice .
docker run geocities-microservice
```

[Anterior]({% link src/pages/tutorial/docker/docker_notification.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/docker/docker_identity_management.md %}){: .btn-next}