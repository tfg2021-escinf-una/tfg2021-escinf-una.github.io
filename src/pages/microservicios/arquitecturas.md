---
layout: default
title: Microservicios
permalink: /Microservicios/Arquitecturas
---

[Regresar]({% link src/pages/microservicios/microservicios.md %})

# Arquitecturas utilizando microservicios


## Monolítica

Los sistemas de información siguen patrones de diseño conforme a la necesidad que el cliente expone.

Bajo un esquema de microservicios, se puede tener una arquitectura basada en un único microservicio, donde consiste en tendría una capa de presentación, que realiza llamadas a una capa de aplicación para finalmente realizar una comunicación con las bases de datos. De una manera gráfica se podría visualizar de la siguiente manera:

![Arquitectura basada en un único microservicio REST](/src/images/monolito.png)

Arquitectura basada en un único microservicio REST.

Fuente: Restful API architecture based on laravel framework.

Es de suma importancia mencionar que al abordar una arquitectura de microservicios se tendrían múltiples capas de aplicación, sin embargo, de acuerdo con el ejemplo anterior se tiene un único microservicio el cual será el encargado de recibir las peticiones realizadas desde las múltiples capas de presentación con las que cuenta el aplicativo.

Ahora bien, siguiendo el ejemplo anteriormente expuesto, se podría tener muchas fallas en caso de extensión, tolerancia de fallos, rendimiento, ya que, se centraliza toda la lógica de negocio en un único recurso.

## Backend for Frontend (BFF)

Actualmente, han ido surgiendo prácticas donde se empieza a descentralizar la lógica de negocio, mediante la creación de múltiples microservicios, donde cada uno de ellos conserva una porción de lógica la cual ejecutará cierta necesidad del negocio. Por lo tanto, un aplicativo de una magnitud mediano/grande puede contener entre treinta y cuarenta microservicios.

Al tener múltiples microservicios alojando en internet, ya sea mediante kubernetes (k8s) o en un servidor, se van a tener múltiples direcciones de acceso, lo cual puede llegar a tener un grado de complejidad, dado que, para las aplicaciones que consumen dichos servicios es complejo el manejo de tantas direcciones.

Es por dicha razón, que diversos proveedores de servicio en la nube crearon productos conocidos como “API Management o API Gateways” donde básicamente se permite tener un único dominio que mapea al microservicio requerido, según el recurso expuesto en la URL. Esta descentralización de servicios, y unificación de ellos mediante los API Management o Gateways es lo que se conoce como Backend for Frontend, ya que permite que el desarrollador vaya creando los diferentes APIs dependiendo de las necesidades del cliente. (Microsoft, 2021).

El uso de este patrón, Backend for Frontend (BFF), involucrando el uso de un API Management, permitirá una serie de ventajas a nivel de escalabilidad y desarrollo de la infraestructura. Entre ellas, (Microsoft Azure, 2019), destacan:

- Libertad a la hora de seleccionar la tecnología de desarrollo
- Deployment totalmente independientes
- Tolerancia a fallos
- Simplicidad

Ahora bien, para ejemplificar de una manera gráfica, se muestra un diagrama de una arquitectura basada en microservicios utilizando un API Management o API Gateway:

![Arquitectura basada en múltiples microservicios](/src/images/microservicios_esquema.png)

Arquitectura basada en múltiples microservicios.

Fuente: https://banzaicloud.com/blog/backyards-api-gateway/#_


 
