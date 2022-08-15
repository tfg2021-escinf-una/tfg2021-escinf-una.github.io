---
layout: default
title: Tutorial
permalink: /Tutorial/Crear_Repos
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Creación de Repositorios

Para mantener el versionamiento de cada uno de los microservicios se recomienda crear un repositorio para cada uno de ellos.

Se recomienda crear una carpeta llamada `microservices-tutorial` para almacenar todos los repositorios.

A continuación se muestran los pasos para la creación de cada repositorio.

## Interfaz de Usuario

1. Ingresar a la página principal de GitHub.
2. Click en el botón New.
3. Colocar el nombre del repositorio (para efectos del tutorial, el nombre será `react-ui`) .
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional] (este archivo se utiliza para explicar conceptos de configuracion y de funcionalidad del código).
6. Seleccionar `Node` como plantilla para el archivo .gitignore (esto evita que los node_modules y las dependencias se incluyan en el repositorio, haciéndolo más ligero).
7. Seleccionar una licencia (para efectos de este tutorial se utilizará MIT).
8. Click en crear repositorio.
9. Clone el repositorio `react-ui` dentro de la carpeta `microservices-tutorial`.

## Notification Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `notification-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Python` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `notification-microservice` dentro de la carpeta `microservices-tutorial`.

## Covid Info Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `covid-info-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Go` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `covid-info-microservice` dentro de la carpeta `microservices-tutorial`.

## Covid Info Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `covid-info-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Go` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `covid-info-microservice` dentro de la carpeta `microservices-tutorial`.

## Geocities Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `geocities-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Node` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `geocities-microservice` dentro de la carpeta `microservices-tutorial`.

## Identity Management Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `identity-management-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Visual Studio` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `identity-management-microservice` dentro de la carpeta `microservices-tutorial`.

## Gateway Management Microservice

1. Ingresar a la página principal de Github
2. Click en el botón New.
3. Colocar el nombre del repositorio: `gateway-management-microservice`.
4. Seleccionar la privacidad del repositorio:
    - público :book:
    - privado :lock:
5. Marcar la opción de agrgar el README file [Opcional].
6. Seleccionar `Visual Studio` como plantilla para el archivo .gitignore.
7. Seleccionar una licencia: MIT.
8. Click en crear repositorio.
9. Clone el repositorio `gateway-management-microservice` dentro de la carpeta `microservices-tutorial`.

Al finalizar la creación de los repositorios, el contenido de la carpeta `microservices-tutorial` deberia ser similar al siguiente:
```
microservices-tutorial
│
└─── react-ui
│   │   README.md
│   │   .gitignore
│
└─── notification-microservice
│   │   README.md
│   │   .gitignore
│
└─── covid-info-microservice
│   │   README.md
│   │   .gitignore
│
└─── geocities-microservice
│   │   README.md
│   │   .gitignore
│
└─── identity-management-microservice
│   │   README.md
│   │   .gitignore
│
└─── gateway-management-microservice
│   │   README.md
│   │   .gitignore
```

A continuación, se iniciará con la implementación de la funcionalidad de cada uno de los componentes de la aplicación.

[Anterior]({% link src/pages/tutorial/microservicios/vista_general.md %}){: .btn-previous} [Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}



