---
layout: default
title: React User Interface
permalink: /Tutorial/Microservicios/React/Setup
---
[Regresar]({% link src/pages/main.md %})
# Setup

En el siguiente link encontrará el repositorio creado para este ejemplo práctico: https://github.com/tfg2021-escinf-una/eg.tfg.react/tree/integration. En caso de tener alguna duda con el código, puede revisar directamente en el repositorio, o bien, clonarlo y modificarlo.

1. Abrir el repositorio `microservices-tutorial/react-ui` en el editor de texto

2. En este momento la lista de archivos en el proyecto es la siguiente:

```
microservices-tutorial
│
└─── react-ui
│   │   README.md
│   │   .gitignore
```

3. (Opcional) Para inicializar este proyecto de React junto con TypeScript, utilizamos el paquete [CRA](https://create-react-app.dev/docs/getting-started). 
  - Para crear la estructura puede utilizar el siguiente comando `npx create-react-app my-app --template typescript`
  - La consola va a ir indicando las opciones a seleccionar para crear la solucion necesaria.


4. Para la ejecucion de este proyecto, recomendamos descargar el repositorio adentro de la estructura anteriormente descrita. 
  - Esto lo puede realizar mediante la utilización de `git`
  - [Link al repo](https://github.com/tfg2021-escinf-una/eg.tfg.react/tree/integration)

5. Una vez clonado, debe proceder a la instalación de las dependencias. Esto se logra mediante el siguiente comando: 
```
    npm install
```

- Una vez terminada la ejecución de dicho comando, procedemos al análisis de dicha aplicación.

### Análisis de la estructura del repositorio.
```
microservices-tutorial
│
└─── react-ui
│   │   config
│   │   deployment
│   │   public
│   │   src
│   │   README.md
│   │   .gitignore
```

-  La estructura anteriormente descrita, propone una serie de orden a la hora de desarrollar el código 
    - ```src``` es la carpeta donde internamente se encuentra todo el código fuente para la ejecución de dicha aplicación.
    - ```config``` contiene un archivo llamado `config-overrides.js`. Este permite realizar cambios a los scripts de webpack sin necesidad de ejecutar un `npm eject`. 
    - ```deployment``` contiene los archivos necesarios para efectuar el despliegue en el cluster de kubernetes.
    - ```public``` contiene los archivos estáticos que no van a ser tomados como parte del build

### Análisis de la estructura dentro de src.
```
react-ui
└─── src
│   │   api
│   │   components
│   │   interfaces
│   │   pages
│   │   redux
│   │   router

```

- La carpeta src posee una serie de carpetas, cuyo objetivo es lograr segregar la funcionalidad de la página de una . 
- Para este ejemplo en especifico, se utilizaron tres carpetas claves
    - api (Aquí se encuentran las configuraciones realizadas para obtener funciones que permitiran llamar al API mediante React Hooks o Redux Thunks, se utilizó [RTK Query](https://redux-toolkit.js.org/rtk-query/overview))
    - components (Aqui se encuentran una serie de carpetas que contienen componentes necesarios para crear las vistas que dicha aplicación ofrece. Cabe destacar que se utilizó [Styled Components](https://styled-components.com/) para dar estilos a nuestro componentes)
    - pages (Archivos producto de la composición de componentes, es decir, una página puede contener 1:N componentes)
    - redux (Aquí se encuentra la lógica necesaria para crear un `State` que se maneja por medio de `reducers`. Para esta implementación utilizamos la nueva version llamada [Redux Toolkit](https://redux-toolkit.js.org/introduction/getting-started)), la cual ofrece una sencilla comprensión. (Cabe destacar que solo para el manejo de la sesión se utilizó `redux`)
    - router (Aquí se encuentra la configuración de la ruta, donde se especifica como se debe acceder y cual página va a ser renderizada)


### Ejemplo de un componente
```
import styled, { FlattenSimpleInterpolation } from 'styled-components'
import { IDeviceBreakpointsDefsProps } from '../../themes'

export const Container = styled.div<IDeviceBreakpointsDefsProps & {
  flex?: boolean,
  direction?: 'row' | 'column', 
  callback?: () => FlattenSimpleInterpolation
}>`
  ${({ theme, mobileS, mobileM, mobileL, laptop, laptopL, tablet, desktop, callback }) => 
    theme.breakpoints.create({
      mobileS: mobileS,
      mobileM: mobileM,
      mobileL: mobileL,
      laptop: laptop,
      laptopL: laptopL,
      tablet: tablet,
      desktop: desktop 
    })(callback)
  };

  ${({ flex, direction }) => flex && `
    display: flex;
    flex-direction: ${direction} !important;
  `};
`
```
- Dicho componente, es el producto de añadirle estilos a un simple `div` pueda tener. Detras de escenas, esto creará `@media queries` para manejar el diseño responsivo de la aplicación.

### Ejemplo de una página (Covid-19)
```
import { useNewsQuery, useVaccinesQuery } from '../../api'
import { Grid, GridItem, Typography, withAuthentication } from '../../components'
import { DataTable } from './composites/DataTable'
import { StyledCarousel, StyledCarouselContainer, StyledImg, StyledMainContainer, StyledVaccinesInfoContainer } from './CovidNews.styles'

const CovidNews = () => {
  const { data: newsData, isLoading: isLoadingNews } = useNewsQuery({})
  const { data: vaccinesData } = useVaccinesQuery({})
  return (
    <StyledMainContainer>
      { newsData && !isLoadingNews && 
        <StyledCarouselContainer xs={12}
                                 sm={11}
                                 md={10}
                                 lg={8}
                                 xl={8}>
          <StyledCarousel>
            {
              newsData.news.map((news, i) => (       
                <Grid key={i}>
                  <GridItem xs={12}
                            sm={12}
                            md={6}
                            lg={6}>
                    <StyledImg src={news.urlToImage} 
                              alt={news.title} />
                  </GridItem>
                  <GridItem xs={12}
                            sm={12}
                            md={6}
                            lg={6}>
                    <Typography as="h2"
                                weight='bolder'
                                size="xl">
                      {news.title}
                    </Typography>
                    <Typography as="h2"
                                size="md">
                      {news.content.split('[')[0]}
                    </Typography>
                  </GridItem>
                </Grid>)
              )
            }
          </StyledCarousel>
        </StyledCarouselContainer>
      } 
      {
        vaccinesData && 
          <StyledVaccinesInfoContainer xs={12}
                                       sm={11}
                                       md={10}
                                       lg={7}
                                       xl={7}>
            <Typography as="h2"
                        size='xl'
                        weight='bolder'
                        textAlign="center">
              Vaccines Research Info
            </Typography>
            <DataTable vaccines={vaccinesData} />
          </StyledVaccinesInfoContainer>
      } 
    </StyledMainContainer>
  )
}

export const AuthCovidNews = withAuthentication(CovidNews)
```
- Como mencionamos con anterioridad, las páginas, son la composición de componentes. Estas pueden crecer a nivel de líneas de código, sin embargo, como buena práctica se recomienda no exceder las 250 lineas. 
- En este ejemplo, esta página elaborará un carousel con las últimas noticias sobre el Covid-19, además, se construirá una tabla con información sobre la vacunación.

### Definicion de las rutas
 
```
import React from "react"
import { 
  AuthCovidNews,
  AuthHome,
  Login,
  Informative,
  Register,
  AuthCities,
  AuthProfile,
  AuthMail
} from "../pages"

/**
 * Here we are going to define all the routes of the application.
 * Also, if there are pages that need to be decorated with our authentication hoc,
 * this is the place to do it.
 */

export const routeConfig = [
  {
    path: '/',
    element : React.createElement(Informative),
    exact: true
  },
  {
    path: '/home',
    element : React.createElement(AuthHome),
    exact: true
  },
  ...
]
```
- Primeramente, se definió un arreglo donde se tiene las rutas y cual página debe ser renderizada en caso de acceder dicho recurso.

```
import { BrowserRouter, Route, Routes } from "react-router-dom";
import { withBaseWrapper } from "../components";
import { routeConfig } from "./routes";

const AppWrapper = withBaseWrapper(
  ({ children } : any) => (<>{ children }</>)
);

export const ApplicationRouter = () => {
  return (
    <BrowserRouter>
      <AppWrapper>
        <Routes>
          { routeConfig.map((route, index) => <Route key={index} {...route} /> ) }
        </Routes>
      </AppWrapper>
    </BrowserRouter>
  )
};
```
- Finalmente, se creó un componente que itera dicho arreglo y por elemento genera un componente `Route`, ofrecido por la libreria [React Router](https://reactrouter.com/en/main/start/tutorial)
- Teniendo listo esto, el sitio ya contiene navegabilidad.

5. Manejo de los Estados
    - Como se mencionó con anterioridad, `redux` se utilizó para el manejo del estado de sesion. 
    - Por otro lado en las páginas, dado a que el estado no tenía mucha complejidad se utilizó el hook `useReducer`, el cual es muy similar al manejo que `redux` ofrece.

5. Ejecución del servidor
  - Esto se logra mediante la ejecución del siguiente comando:

```
    npm run start
```

Una vez realizados todos estos pasos, el servidor está listo para recibir peticiones HTTP en el puerto `3000`.

[Anterior]({% link src/pages/tutorial/microservicios/react-ui/main.md %}){: .btn-previous}[Siguiente]({% link src/pages/tutorial/microservicios/main.md %}){: .btn-next}
