---
layout: default
title: Docker en Notification Microservice
permalink: /Tutorial/Containers/Notification
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en Notification Microservice

1. Abrir el repositorio `microservices-tutorial/notification-microservice` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM python:3.10-alpine

COPY /requirements.txt .

RUN pip install -r requirements.txt

COPY /app .

ENV FLASK_APP="app"

EXPOSE 5000

CMD [ "python3", "-m" , "flask", "run" ]
```

- Se usa la versión Alpine de Python como base de la imagen.
- Se copia el archivo de dependencias ```requirements.txt```.
- Se ejecuta la instalación local de las dependencias del archivo ```requirements.txt```.
- Se copian únicamete los archivos necesarios.
- Se define la variable de entorno utilizada por Flask: FLASK_APP="app"
- Se expone el puerto 5000.
- Se ejecuta el comando ```python3 -m flask run``` que dará inicio al servidor construido en el contendor.

Una vez realizados estos pasos, se puede proceder con la ejecución de los comandos de Docker para crear la imagen y para correrla en el equipo:

```
docker build -t notification-microservice .
docker run notification-microservice
```

[Anterior]({% link src/pages/tutorial/docker/docker_covid_info.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/docker/docker_geocities.md %}){: .btn-next}