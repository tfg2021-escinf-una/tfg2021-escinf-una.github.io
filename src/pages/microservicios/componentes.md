---
layout: default
title: Microservicios
permalink: /Microservicios/Componentes
---

[Regresar]({% link src/pages/microservicios/microservicios.md %})

# Componentes de un microservicio
Un microservicio consta de pocos componentes, no obstante, estos se ven directamente afectados por la lógica de negocio. Esto se debe porque estos pueden realizar un llamado a cualquier servicio que se requiera y en algunas ocasiones estas implementaciones poseen su grado de complejidad.

Ahora bien, un ejemplo de los componentes básicos los cuales conforman un microservicio, son los siguientes:
- Controller (Controladores)
- Model (Modelos)
- Data Access Layer (Capa de Acceso a Datos)

Cada uno de ellos tiene una función específica, en el caso de los controllers, Microsoft Docs afirma que estos son los encargados de ejecutar una porción de lógica accedida por una petición (2020). Cada una de estas porciones de lógica, son conocidas como endpoint.

Por otro lado, IBM (2019) afirma que, la capa de acceso a datos es la encargada de comunicarse con el sistema gestor de bases de datos. Es por dicha razón que, a nivel de código independientemente del lenguaje es posible encontrarse hileras de conexión hacía servidores de bases de datos.

Finalmente los modelos, se definen como una estructura de datos la cual refleja el esqueleto de como el objeto es guardado en la base de datos, Microsoft Docs (2021). Cabe destacar que, este será serializado en el formato conocido como Java Script Object Notation (JSON), el cual permite una fácil manipulación del contenido serializado. 
Desde una vista gráfica, un microservicio puede ser visto de la siguiente manera:

![Componentes de un microservicio](/src/images/componentes_microservicio.png)

Componentes de un microservicio.

Fuente:

https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-5.0&tabs=visual-studio

