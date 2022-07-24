---
layout: default
title: Integración Contínua y Entrega Contínua
permalink: /CI_CD
---

[Regresar]({% link src/pages/main.md %})

# Integración Contínua y Entrega Contínua

Red Hat (2021), menciona que el proceso de integración y entrega continua permite la distribución de los aplicativos mediante el uso de la automatización en diversas etapas del desarrollo de aplicaciones. Esto quiere decir que, procesos que son sumamente repetitivos van a ser ejecutados de una manera automática, mientras que en décadas anteriores muchos de estos se realizaban de una manera manual.

Ahora bien, es importante definir el término automatización, según Red Hat (2021), este “consiste en el uso de sistemas de software para crear instrucciones y procesos repetibles a fin de reemplazar o reducir la interacción humana con los sistemas de TI.” (parr.1). Por ende, se puede deducir que el marco de integración y entrega continua se realizará por medio de plataformas, que permitirán la programación de las tareas a ejecutar para posteriormente realizar la publicación del sistema en sus respectivos servidores.

En la actualidad, se cuentan con múltiples plataformas para llevar acabo las automatizaciones necesarias, entre ellas destacan Bamboo, Jenkins, Azure DevOps Pipelines, entre otros. Cabe resaltar que cada uno de estos sistemas puede acoplarse de una mejor manera dependiendo del tipo de negocio que se maneja, sin embargo, la presente investigación se enfocará en Jenkins, esto debido a que el sistema es totalmente abierto.

Por otra parte, es importante el uso de un controlador de versiones. En los últimos años GIT es uno de los más utilizados. Según la documentación oficial de dicho software, menciona que compañías como Google, Facebook, Microsoft, Twitter, utilizan dicho sistema. Algunas de las mencionadas anteriormente tienen su propia infraestructura cloud, y, por ende, documentación que explica como iniciar una configuración para obtener un aplicativo bajo el marco de integración y entrega continua utiliza GIT como controlador de versiones.

Como se mencionó anteriormente, la integración y entrega continua consiste en automatizar procesos que son repetitivos. Para ejemplificar estos procesos, a continuación, se listará una serie de tareas que actualmente son automatizadas.

1.	Elaboración del código por parte del ingeniero utilizando herramientas como Visual Studio, Visual Studio Code, entre otros.

2.	Realizar un commit al repositorio mediante GIT

3.	En este punto inicia el proceso de automatización. La plataforma Jenkins, detectará que hubo una actualización en el código, por ende, iniciará a ejecutar los procesos que el usuario haya determinado, generalmente se encarga de ejecutar lo relacionado con testing, con el fin de verificar que no exista ninguna falla en un ambiente de producción.
Al final del proceso de testing, compilación y demás tareas ejecutadas, se debe crear una imagen de Docker. Es importante resaltar que, si dicho proyecto es una librería, Jenkins realizará un paquete NuGet o PyPi para alojarlo en un repositorio de artefactos en vez de una imagen de Docker.

4.	En caso de haber construido una imagen de Docker, la misma será enviada a Octopus, que posteriormente se encargará de la publicación de los archivos en los servidores. Cabe destacar que el proceso de publicación puede variar. Puesto que, se ve directamente afectado a la capacidad de hardware que se tenga en el momento, es decir, entre kubernetes y un virtual machine, las configuraciones son sumamente distinto.

A continuación, se mostrará de una manera gráfica el flujo mencionado con anterioridad:

![Arquitectura utilizando CI/CD sobre Máquinas virtuales o Kubernetes](/src/images/ci_cd_esquema.png)

Figura 2. Arquitectura utilizando CI/CD sobre Máquinas virtuales o Kubernetes

Fuente: Elaboración propia

Una vez analizada la figura anterior, se puede concluir la importancia que contraen las plataformas que permiten la ejecución de tareas. En un final, dicho proceso ayudará a optimizar el ciclo de vida del software. Por otra parte, ayudará a los desarrolladores a correr sus ramas en ambientes de prueba para posteriormente enviarlas a producción. Es por dicha razón que, el conocimiento en esta área es de suma importancia. Además, los desarrollos modernos utilizan esta metodología.
