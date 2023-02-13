---
layout: default
title: Notification Microservice
permalink: /Tutorial/Microservicios/Notification/
---
[Regresar]({% link src/pages/main.md %})

![Notification Microservice](/src/images/enfasis_python.png)

Este servicio tiene como objetivo principal el manejo de la identidad del usuario. Para hacer esto posible se decidió realizar una base de datos no relacional que aloja los usuarios registrados al sistema y un servicio en .NET que habilita “endpoints” que permiten realizar el proceso de autenticación. 
Una vez realizada la autenticación se generará un JSON Web Token, el cual será utilizado para autorizar si un usuario puede acceder a cierto recurso. En otras palabras, este servicio autentica y devuelve un token, dicho token debe ser enviado a los otros microservicios donde se verificará si el token es auténtico. Además, se revisará si los claims que dicho token provee, autorizan a acceder al recurso solicitado. 

[Anterior]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-previous} [Siguiente]({% link src/pages/tutorial/microservicios/notification-microservice/setup.md %}){: .btn-next}
