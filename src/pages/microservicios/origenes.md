---
layout: default
title: Microservicios
permalink: /Microservicios/Origenes
---
[Regresar]({% link src/pages/microservicios/microservicios.md %})

# Orígenes
A lo largo de la historia de la informática, se han desarrollado diversos patrones de diseño bajo un fundamento teórico, los cuales fueron expuestos con el fin de solventar las necesidades que existían en la época.

En la actualidad, los patrones de diseño han ido evolucionando, dado que, las necesidades de los clientes incrementan constantemente, además de la cantidad de usuarios que consumen dichos aplicativos.

Como resultado de la evolución, el desarrollo de aplicaciones basado en microservicios se ha fortalecido logrando tener un alto índice de popularidad gracias a los beneficios que contrae. Inicialmente, dicha temática fue desarrollada por el ingeniero Martin Fowler, con el objetivo de obtener un patrón de diseño que fuese apto para la mejora de rendimiento y de productividad en productos empresariales grandes. Él expone que, “los microservicios deben ser pequeños proyectos donde se alojan mínimas porciones de lógica, con el fin de evitar la acumulación de algoritmia en un único proyecto” (parr.1). 

No obstante, existe una gran cantidad de autores que concuerdan con dicha definición. Entre ellos se destaca Amazon Web Services (2021) donde define los microservicios como “un enfoque arquitectónico y organizativo para el desarrollo de software donde el software está compuesto por pequeños servicios independientes que se comunican a través de API bien definidas” (parr.1).

Indagando un poco más fondo, AWS (2021) menciona que un microservicio es un Application Programming Interface (API). Según Red Hat (2021), un API se conoce como una capa que permite la comunicación entre un producto y otro, donde se obtiene una gran flexibilidad, un diseño simple, y una fácil administración a la hora de implementar nuevas funcionalidades.

Por ende, un API provee beneficios en la implementación de una arquitectura, ya que descentraliza la lógica de las capas de presentación y la capa de aplicación.

Cabe resaltar que, su núcleo está programado bajo una serie de protocolos, actualmente, uno de los protocolos más utilizados es conocido como Representational State Transfer (REST).

Xianjun, C. (2017), expone que un REST API, es un proyecto que se realiza mediante una implementación ligera del protocolo conocido como Remote Procedure Call (RPC), utilizando como base el protocolo HTTP. Es por dicha razón que los desarrolladores encuentran viable la construcción de estos REST API, puesto que, la comunicación entre un aplicativo y el API será mediante consultas HTTP, las cuales son sencillas de implementar (p.2).

Al utilizar HTTP, es posible enviar peticiones utilizando Uniform Resource Identifiers (URI), los cuales son identificadores únicos que permiten el acceso a procedimiento que debe ejecutarse para obtener una respuesta. Cada uno de estos identificadores esto permite el uso de los siguientes verbos, GET, POST, PUT, DELETE, PATCH. Cada uno de estos verbos, debe ser utilizado dependiendo de la operación que se desee realizar sobre la base de datos.
