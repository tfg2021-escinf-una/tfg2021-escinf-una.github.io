---
layout: default
title: Docker en Gateway Management Microservice
permalink: /Tutorial/Containers/GatewayManagement
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en Gateway Management Microservice

1. Abrir el repositorio `microservices-tutorial/gateway-management-microservice` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build-env
ARG Configuration=Release

COPY . .

#Compiling the application with the .NET SDK.

RUN dotnet restore ./src/*.sln
RUN dotnet build ./src/*.sln --no-restore --nologo -v normal
RUN dotnet publish ./src/EG.Gateway.Microservice.csproj -c ${Configuration} -o /publish/app --no-build --nologo -v normal

#Build application with runtime .NET image

FROM mcr.microsoft.com/dotnet/aspnet:6.0

ENV ASPNETCORE_URLS="http://*:8081"
EXPOSE 8081/tcp

RUN mkdir -p /var/app \
    && groupadd -g 2000 container_group \
    && useradd -u 1000 container_user \
    && chown -R container_user:container_group /var/app

COPY --from=build-env /publish/app /var/app
WORKDIR /var/app
USER container_user

ENTRYPOINT ["dotnet", "EG.Gateway.Microservice.dll"]
```

# TODO: Explicación

Una vez realizados estos pasos, se puede proceder con la ejecución de los comandos de Docker para crear la imagen y para correrla en el equipo:

```
docker build -t gateway-management-microservice .
docker run gateway-management-microservice
```

[Anterior]({% link src/pages/tutorial/docker/docker_identity_management.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/docker/docker_react_ui.md %}){: .btn-next}