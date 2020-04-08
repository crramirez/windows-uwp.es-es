---
title: Introducción a los marcos web de Node.js en Windows
description: Una guía para ayudarle a empezar a usar los marcos web de Node.js en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, windows 10, microsoft, aprendizaje de nodejs, node en windows, node en wsl, node en linux en windows, instalar node en windows, nodejs con vs code, desarrollar con node en windows, desarrollar con nodejs en windows, instalar node en WSL, NodeJS en el Subsistema de Windows para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a8ce1d08136a74504e1b3bad26feadd61b72068f
ms.sourcegitcommit: 60d2d15dd0d365f82e4e90e4bc34b40cf5b4a247
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72517790"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Introducción a los marcos web de Node.js en Windows

Una guía paso a paso que te ayudará a empezar a usar marcos web de node.js en Windows, incluidos Next.js, Nuxt.js y Gatsby.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya has completado los pasos para [configurar el entorno de desarrollo de Node.js con WSL 2](./setup-on-wsl2.md), que incluyen:

- Instalar la compilación 18932 o posterior de Windows 10 Insider Preview.
- Habilitar la característica WSL 2 en Windows.
- Instalar una distribución de Linux (Ubuntu 18.04 para nuestros ejemplos). Puedes comprobarlo con: `wsl lsb_release -a`.
- Asegurarte de que la distribución de Ubuntu 18.04 se está ejecutando en modo WSL 2. (WSL puede ejecutar distribuciones en los modos V1 o V2). Para comprobarlo, abre PowerShell y escribe: `wsl -l -v`.
- Con PowerShell, establece Ubuntu 18.04 como la distribución predeterminada, con: `wsl -s ubuntu 18.04`.

## <a name="get-started-with-nextjs"></a>Introducción a Next.js

Next.js es un marco para crear aplicaciones de JavaScript representadas por el servidor basadas en React.js, Node.js, Webpack y Babel.js. Es básicamente un proyecto reutilizable para React, diseñado con atención a los procedimientos recomendados, que te permite crear aplicaciones web "universales" de manera sencilla y coherente con prácticamente cualquier configuración. Estas aplicaciones web "universales" representadas por el servidor también se denominan a veces "isomórficas", lo que significa que el código se comparte entre el cliente y el servidor.

Para crear un siguiente proyecto de Next.js, que incluye la instalación de next, react y react-dom:

1. Abre el terminal de WSL (es decir, Ubuntu 18.04).

2. Crea la nueva carpeta de proyecto `mkdir NextProjects` y accede al directorio `cd NextProjects`.

3. Instala Next.js y crea un proyecto (reemplazando "my-next-app" por el nombre que le quiera poner a la aplicación): `npm create next-app my-next-app`.

4. Una vez instalado el paquete, cambia los directorios a la nueva carpeta de la aplicación, `cd my-next-app`, y luego usa `code .` para abrir el siguiente proyecto de Next.js en VS Code. Esto te permitirá ver el siguiente marco de Next.js que se ha creado para la aplicación. Ten en cuenta que VS Code ha abierto la aplicación en un entorno de WSL remoto (como se indica en la pestaña verde en la parte inferior izquierda de la ventana de VS Code). Esto significa que aunque estás usando VS Code para editar en el sistema operativo Windows, la aplicación se seguirá ejecutando en el sistema operativo Linux.

    ![Extensión WSL-Remote](../images/wsl-remote-extension.png)

5. Hay tres comandos que debes conocer una vez instales Next.js:

    - `npm run dev` para ejecutar una instancia de desarrollo con recarga activa, supervisión de archivos y reejecución de tareas.
    - `npm run build` para compilar el proyecto.
    - `npm start` para iniciar la aplicación en modo de producción.

    Abre el terminal de WSL integrado en VS Code (**Ver > Terminal**). Asegúrate de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/NextProjects/my-next-app$`). Después, intenta ejecutar una instancia de desarrollo de la nueva aplicación Next.js con: `npm run dev`

6. El servidor de desarrollo local se iniciará y, una vez que las páginas del proyecto terminen de compilar, el terminal mostrará un mensaje para indicar que se ha compilado correctamente y está listo en [http://localhost:3000](http://localhost:3000). Selecciona este vínculo localhost para abrir la nueva aplicación Next.js en un explorador web.

    ![La aplicación Next.js ejecutándose en localhost:3000.](../images/next-app.png)

7. Abre el archivo `pages/index.js` en el editor de VS Code. Busca el título de la página `<h1 className='title'>Welcome to Next.js!</h1>` y cámbialo a `<h1 className='title'>This is my new Next.js app!</h1>`. Con el explorador web todavía abierto en localhost:3000, guarda el cambio y observa que la característica de recarga activa compila y actualiza automáticamente el cambio en el explorador.

8. Veamos el modo en que Next.js administra los errores. Quita la etiqueta de cierre de `</h1>` para que el código de título tenga el siguiente aspecto: `<h1 className='title'>This is my new Next.js app!`. Guarda este cambio y observa que se mostrará un error para indicar que no se pudo compilar en el explorador y en el terminal, lo que te permitirá saber que se espera una etiqueta de cierre para `<h1>`. Reemplaza la etiqueta de cierre `</h1>` y la página se volverá a cargar.

Puedes usar el depurador de VS Code con la siguiente aplicación Next.js; para ello, selecciona la tecla F5 o ve a **Ver > Depurar** (Ctrl+Mayús+D) y **Ver > Consola de depuración** (Ctrl+Mayús+Y) en la barra de menús. Si seleccionas el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guardes los detalles de configuración de la depuración. Para obtener más información, consulta [Depuración en VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Ventana de depuración de VS Code e icono de configuración de launch.json](../images/vscode-debug-launch-configuration.png)

Para obtener más información acerca de Next.js, consulta la [documentación de Next.js](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Introducción a Nuxt.js

Nuxt.js es un marco para crear aplicaciones de JavaScript representadas por el servidor basadas en Vue.js, Node.js, Webpack y Babel.js. Está inspirado en Next.js. Es básicamente un proyecto reutilizable para Vue. Al igual que Next.js, está diseñado con atención a los procedimientos recomendados, que te permite crear aplicaciones web "universales" de manera sencilla y coherente con prácticamente cualquier configuración. Estas aplicaciones web "universales" representadas por el servidor también se denominan a veces "isomórficas", lo que significa que el código se comparte entre el cliente y el servidor.

Para crear un proyecto de Nuxt.js, que incluirá las respuestas a una serie de preguntas sobre el tipo de marco de servidor integrado, el marco de la interfaz de usuario, el marco de pruebas, el modo, los módulos y el linter que quieres instalar:

1. Abre el terminal de WSL (es decir, Ubuntu 18.04).

2. Crea la nueva carpeta de proyecto `mkdir NuxtProjects` y accede al directorio `cd NuxtProjects`.

3. Instala Nuxt.js y crea un proyecto (reemplazando "my-nuxt-app" por el nombre que le quieras poner a la aplicación): `npm create nuxt-app my-nuxt-app`.

4. El instalador de Nuxt. js ahora te hará las siguientes preguntas:
    - Nombre del proyecto: my-nuxtjs-app
    - Descripción del proyecto: descripción de la aplicación Nuxt.js.
    - Nombre del autor: utilizo mi alias de GitHub.
    - Elige el administrador de paquetes: Yarn o **Npm** (usamos NPM para nuestros ejemplos).
    - Elige el marco de la interfaz de usuario: Ninguno, Ant Design Vue, Bootstrap Vue, etc. Vamos a elegir **Vuetify** para este ejemplo, pero la comunidad Vue creó un buen [resumen de comparativa de estos marcos de interfaz de usuario](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) para ayudarte a elegir el que mejor se adapte a tu proyecto.
    - Elige marcos de servidor personalizados: Ninguno, AdonisJs, Express, Fastify, etc. Vamos a elegir **Ninguno** para este ejemplo, pero puedes encontrar una [comparación de marcos de servidor 2019-2020](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) en el sitio Dev.to.
    - Elige módulos de Nuxt.js (usa la barra espaciadora para seleccionar módulos o simplemente dale a Enter si no quieres ninguno): Axios (para simplificar las solicitudes HTTP) o [PWA support](https://pwa.nuxtjs.org/) (para agregar un servicio de trabajo, un archivo manifest.json, etc.). No vamos a agregar ningún módulo para este ejemplo.
    - Elige herramientas de detección de errores: **ESLint**, Prettier, archivos almacenados provisionalmente de Lint. Vamos a elegir **ESLint** (una herramienta para analizar el código y avisar de posibles errores).
    - Elige un marco de pruebas: **Ninguno**, Jest, AVA. Vamos a elegir **Ninguno** ya que no trataremos las pruebas en esta guía de inicio rápido.
    - Elige el modo de representación: **universal (SSR)** o aplicación de página única (SPA). Vamos a elegir **Universal (SSR)** en nuestro ejemplo, pero en la [documentación de Nuxt.js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) se señalan algunas de las diferencias: SSR requiere un servidor Node.js en ejecución para representar en el servidor la aplicación y SPA para el hospedaje estático.
    - Elige las herramientas de desarrollo: **jsconfig.json** (recomendado para VS Code para que funcione la finalización del código de IntelliSense).

5. Una vez creado el proyecto, `cd my-nuxtjs-app` para indicar el directorio del proyecto de Nuxt.js y, a continuación, escribe `code .` para abrir el proyecto en el entorno de VS Code WSL-Remote.

    ![Extensión WSL-Remote](../images/wsl-remote-extension.png)

6. Hay tres comandos que debes conocer una vez instales Next.js:

    - `npm run dev` para ejecutar una instancia de desarrollo con recarga activa, supervisión de archivos y reejecución de tareas.
    - `npm run build` para compilar el proyecto.
    - `npm start` para iniciar la aplicación en modo de producción.

    Abre el terminal de WSL integrado en VS Code (**Ver > Terminal**). Asegúrate de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/NuxtProjects/my-nuxt-app$`). Después, intenta ejecutar una instancia de desarrollo de la nueva aplicación Nuxt.js con: `npm run dev`

6. El servidor de desarrollo local se iniciará (mostrando algunos tipos útiles de barras de progreso de las compilaciones de cliente y servidor). Una vez que el proyecto termine de compilarse, el terminal mostrará un mensaje que indica que se compilado correctamente junto con el tiempo que tardó en compilarse. Apunta con el explorador web a [http://localhost:3000](http://localhost:3000) para abrir la nueva aplicación de Nuxt.js.

    ![La aplicación Nuxt.js ejecutándose en localhost:3000.](../images/nuxt-app.png)

7. Abre el archivo `pages/index.vue` en el editor de VS Code. Busca el título de la página `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` y cámbialo a `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Con el explorador web todavía abierto en localhost:3000, guarda el cambio y observa que la característica de recarga activa compila y actualiza automáticamente el cambio en el explorador.

8. Veamos el modo en que Nuxt.js administra los errores. Quita la etiqueta de cierre de `</v-card-title>` para que el código de título tenga el siguiente aspecto: `<v-card-title class="headline">This is my new Nuxt.js app!`. Guarda este cambio y observa que se mostrará un error de compilación en el explorador y en el terminal, lo que te permitirá saber que falta una etiqueta de cierre para `<v-card-title>`, junto con los números de línea en los que se puede encontrar el error en el código. Reemplaza la etiqueta de cierre `</v-card-title>` y la página se volverá a cargar.

Puedes usar el depurador de VS Code con la siguiente aplicación Next.js; para ello, selecciona la tecla F5 o ve a **Ver > Depurar** (Ctrl+Mayús+D) y **Ver > Consola de depuración** (Ctrl+Mayús+Y) en la barra de menús. Si seleccionas el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guardes los detalles de configuración de la depuración. Para obtener más información, consulta [Depuración en VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Ventana de depuración de VS Code e icono de configuración de launch.json](../images/vscode-debug-launch-configuration.png)

Para obtener más información acerca de Nuxt.js, consulta la [documentación de Next.js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Introducción a Gatsby.js

Gatsby.js es un marco generador de sitios estático basado en React.js, en lugar de representarse en servidor como Next.js y Nuxt.js. Un generador de sitios estáticos genera HTML estático en tiempo de compilación. No requiere un servidor. Next.js y Nuxt.js generan HTML en tiempo de ejecución (cada vez que entra una nueva solicitud). Requieren un servidor para ejecutarse. Gatsby también dicta cómo controlar los datos de la aplicación (con GraphQL), mientras que Next.js y Nuxt.js dejan esa decisión en tus manos.

Para crear un proyecto de Gatsby.js:

1. Abre el terminal de WSL (es decir, Ubuntu 18.04).
2. Crea la nueva carpeta de proyecto `mkdir GatsbyProjects` y accede al directorio `cd GatsbyProjects`.
3. Usa NPM para instalar la CLI de Gatsby: `npm install -g gatsby-cli`. Una vez instalada, comprueba la versión con `gatsby --version`.
4. Crea el proyecto de Gatsby. js: `gatsby new my-gatsby-app`
5. Una vez instalado el paquete, cambia los directorios a la nueva carpeta de la aplicación, `cd my-gatsby-app`, y luego usa `code .` para abrir el siguiente proyecto de Next.js en VS Code. Esto le permitirá ver el siguiente marco de Gatsby.js que se ha creado para la aplicación con el Explorador de archivos de VS Code. Ten en cuenta que VS Code ha abierto la aplicación en un entorno de WSL remoto (como se indica en la pestaña verde en la parte inferior izquierda de la ventana de VS Code). Esto significa que aunque estás usando VS Code para editar en el sistema operativo Windows, la aplicación se seguirá ejecutando en el sistema operativo Linux.

    ![Extensión WSL-Remote](../images/wsl-remote-extension.png)

6. Hay 3 comandos que debes conocer una vez instales Gatsby:

    - `gatsby develop` para ejecutar una instancia de desarrollo con recarga activa.
    - `gatsby build` para crear una compilación de producción.
    - `gatsby serve` para iniciar la aplicación en modo de producción.

    Abre el terminal de WSL integrado en VS Code (**Ver > Terminal**). Asegúrate de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/GatsbyProjects/my-gatsby-app$`). Después, intenta ejecutar una instancia de desarrollo de la nueva aplicación con: `gatsby develop`

7. Una vez finalizada la compilación del nuevo proyecto de Gatsby, el terminal mostrará un mensaje indicando que ya puedes ver gatsby-starter-default en el explorador. [http://localhost:8000/](http://localhost:8000/). Selecciona este vínculo localhost para ver el nuevo proyecto compilado en un explorador web.

> [!NOTE]
> Observarás que la salida de terminal también indica que puedes ver GraphiQL, un IDE en el explorador, para explorar los datos y el esquema del sitio: [http://localhost:8000/___graphql](http://localhost:8000/___graphql). GraphQL consolida las API en un IDE de autodocumentación (GraphiQL) integrado en Gatsby. Además de explorar los datos y el esquema del sitio, puedes realizar operaciones de GraphQL como consultas, mutaciones y suscripciones. Para obtener más información, consulta [Introducción a GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Abre el archivo `src/pages/index.js` en el editor de VS Code. Busca el título de la página `<h1 >Hi people</h1>` y cámbialo a `<h1 >Hi (Your Name)!</h1>`. Con el explorador web todavía abierto en localhost:8000, guarda el cambio y observa que la característica de recarga activa compila y actualiza automáticamente el cambio en el explorador.

    ![La aplicación Gatsby.js ejecutándose en localhost:3000.](../images/gatsby-app.png)

9. Veamos el modo en que Next.js administra los errores. Quita la etiqueta de cierre de `</h1>` para que el código de título tenga el siguiente aspecto: `<h1>Hi (Your Name)!`. Guarda este cambio y observa que se mostrará un error para indicar que no se pudo compilar en el explorador y en el terminal, lo que te permitirá saber que se espera una etiqueta de cierre para `<h1>`. Reemplaza la etiqueta de cierre `</h1>` y la página se volverá a cargar.

Puedes usar el depurador de VS Code con la siguiente aplicación Next.js; para ello, selecciona la tecla F5 o ve a **Ver > Depurar** (Ctrl+Mayús+D) y **Ver > Consola de depuración** (Ctrl+Mayús+Y) en la barra de menús. Si seleccionas el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guardes los detalles de configuración de la depuración. Para obtener más información, consulta [Depuración en VS Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging).

![Ventana de depuración de VS Code e icono de configuración de launch.json](../images/vscode-debug-launch-configuration.png)

Para obtener más información sobre Gatsby, consulta la [documentación de Gatsby.js](https://www.gatsbyjs.org/docs/).
