---
layout: default
title: Docker en React UI
permalink: /Tutorial/Containers/ReactUI
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en React UI

1. Abrir el repositorio `microservices-tutorial/react-ui` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM nginx
LABEL maintainer=tfg2021.escinf.una@gmail.com

EXPOSE 4200

COPY nginx.conf /etc/nginx/conf.d/default.conf
COPY /dist/apps/tfg-frontend /usr/share/nginx/html
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
docker build -t react-ui .
docker run react-ui
```

[Anterior]({% link src/pages/tutorial/docker/docker_gateway_management.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/ci_cd/main.md %}){: .btn-next}