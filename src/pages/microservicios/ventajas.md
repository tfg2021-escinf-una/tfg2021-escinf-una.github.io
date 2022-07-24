---
layout: default
title: Microservicios
permalink: /Microservicios/Ventajas
---

[Regresar]({% link src/pages/microservicios/microservicios.md %})

# Ventajas de la implementación de microservicios
Al un microservicio ser una unidad que contiene una porción de lógica la cual vive en un ambiente totalmente propio, permite una serie de ventajas. Según Ali (2020), expone algunos puntos en los cuales se expresa porqué es bueno implementar microservicios, entre ellas destacan:
- Una mejor integración continua. Los pipelines se vuelven pequeños y fáciles de comprender. Además, al encapsular porciones pequeñas de lógica, estos tienen un buen tiempo de ejecución.
- Una mejor entrega continua, los deployments son rápidos debido a que los proyectos no tienen una gran cantidad de archivos que colocar en un contenedor o servidor.
- Reduce costos con base a la infraestructura. 
- Tolerancia a fallos
- La apertura de nuevas líneas de negocio se vuelve un trabajo más sencillo, ya que se crearán nuevos microservicios, en vez de tener que manipular una única solución, en otras palabras, ofrecen una alta escalabilidad.
- A nivel de desarrollo, estos pueden ser creados en cualquier lenguaje de programación, independientemente si hay microservicios que se encuentren en un específico lenguaje de programación.

Es por dicha razón que, muchos arquitectos de sistemas consideran que dicho enfoque es sumamente óptimo y beneficio, además, la computación en la nube ha venido a dar mucho soporte a esta arquitectura, ya que los diversos proveedores de servicios (Amazon, IBM, Microsoft), se han dedicado a implementar soluciones que buscan facilitar la puesta a producción de los microservicios, lo cual ha generado que muchas empresas migren sus infraestructuras a la nube.

Sin embargo, pese a las ventajas que ofrecen los microservicios, siempre es importante tener en cuenta el alcance que tiene el proyecto, dado que, la implementación de esta arquitectura se ve directamente afectada por el alcance. Esto debido a que, si el sistema a implementar es muy grande, es decir, donde existan n cantidad de módulos, dicha arquitectura se adapta de una manera óptima. No obstante, si el proyecto tiene un alcance corto donde no hay una gran cantidad de módulos a implementar, el enfoque monolítico puede ser la mejor elección en vista de que la cantidad de archivos de código serán pocos.
