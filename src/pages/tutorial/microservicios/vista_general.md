---
layout: default
title: Vista General
permalink: /Tutorial/Vista_General
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Vista General de la Aplicación a Construir


![Vista General de la Aplicación a Construir](/src/images/vista_general.png)

La aplicación por construir en el presente tutorial consta de 6 componentes: una interfaz de usuario, 4 microservicios y un Gateway API.
El objetivo de esta aplicación es explicar, mediante un ejemplo programado, la interacción de los microservicios y el UI mediante la comunicación HTTP.
A continuación se dará una breve explicación de cada uno de los componentes.

1. UI: Interfaz de usuario que consume los diferentes microservicios y muestra resultados al usuario final. Construido usando React.js
2. Covid-Info Microservice: Desarrollado en Go. Ofrece dos endpoints de tipo GET que permiten obtener información relacionada al Covid-19.
3. Notification Microservice: Desarrollado en Python. Permite la funcionalidad de envio de notificaciones por medio de correo electrónico.
4. Gateway Management: N/A
5. Identity Management: N/A
6. Geocities Microservice: N/A

A continuación, se presenta el primer paso del tutorial: la creación de los repositorios.

[Anterior]({% link src/pages/tutorial/main.md %}){: .btn-previous} [Siguiente]({% link src/pages/tutorial/crear_repo.md %}){: .btn-next}


