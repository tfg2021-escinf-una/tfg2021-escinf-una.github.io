---
layout: default
title: Docker en Identity Management Microservice
permalink: /Tutorial/Containers/IdentityManagement
---
[Regresar al Inicio]({% link src/pages/main.md %})

# Docker en Identity Management Microservice

1. Abrir el repositorio `microservices-tutorial/identity-management-microservice` en el editor de texto.
2. Crear un archivo llamado `Dockerfile`. (No se le debe poner extensión)
3. Copiar el siguiente código dentro del `Dockerfile`:

```
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build-env
ARG Configuration=Release

COPY . .

#Compiling the application with the .NET SDK.

RUN dotnet restore ./src/*.sln
RUN dotnet build ./src/*.sln --no-restore --nologo -v normal
RUN dotnet test ./src/*.sln -c ${Configuration} --no-build --nologo
RUN dotnet publish ./src/EG.IdentityManagement.Microservice.csproj -c ${Configuration} -o /publish/app --no-build --nologo -v normal

#Build application with runtime .NET image

FROM mcr.microsoft.com/dotnet/aspnet:5.0-buster-slim-amd64

ENV ASPNETCORE_URLS="http://*:8081"
EXPOSE 8081/tcp

RUN mkdir -p /var/app \
    && groupadd -g 2000 container_group \
    && useradd -u 1000 container_user \
    && chown -R container_user:container_group /var/app

COPY --from=build-env /publish/app /var/app
WORKDIR /var/app
USER container_user

ENTRYPOINT ["dotnet", "EG.IdentityManagement.Microservice.dll"]
```

# TODO: Explicación

Una vez realizados estos pasos, se puede proceder con la ejecución de los comandos de Docker para crear la imagen y para correrla en el equipo:

```
docker build -t identity-management-microservice .
docker run identity-management-microservice
```

[Anterior]({% link src/pages/tutorial/docker/docker_geocities.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/docker/docker_gateway_management.md %}){: .btn-next}