---
layout: default
title: Acceso al API de GeoDB Cities en RapidAPI
permalink: /Tutorial/Microservicios/Geocities/RapidAPI
---
[Regresar]({% link src/pages/main.md %})
# Acceso al API de GeoDB Cities en RapidAPI

Para obtener la información relacionada a ciudades y países la forma más sencilla es consultar un API externo que, por medio de HTTP, obtenga todos los datos de fuentes confiables.

Existen diferentes alternativas de APIs para obtener esta información. Sin embargo, al ser únicamente con fines didácticos, se utilizará el API gratuito de RapidAPI. A continuación se muestran los pasos a seguir para obtener el URL y el KEY del API relacionado a Covid-19.

## Utilizando RapidAPI

1. Ingresar a la página oficial del API: [GeoDB Cities](https://rapidapi.com/wirefreethought/api/geodb-cities/).

2. Revisar la documentación de los endpoints que ofrece el API. Este servicio web tiene una gran cantidad de endpoints a disposición, por lo que se le invita a crear distintos métodos y obtener más datos utilizando este servicio. No obstante, para fines del tutorial, se utilizarán únicamente 3 endpoints:
    - Countries
    - Country Details
    - Country Regions
3. Iniciar sesión en RapidAPI (puede utilizar su cuenta de GitHub para esto).
4. Presionar el botón de Inscribirse al API de GeoDB Cities
5. Una vez que esté inscrito en el API, obtendrá los valores del Host (X-RapidAPI-Host) y el Key (X-RapidAPI-Key) a utilizar. Almacene esta información, ya que se utilizará en el paso siguiente.


[Anterior]({% link src/pages/tutorial/microservicios/geocities-microservice/main.md %}){: .btn-previous} [Siguiente]({% link src/pages/tutorial/microservicios/geocities-microservice/setup.md %}){: .btn-next}