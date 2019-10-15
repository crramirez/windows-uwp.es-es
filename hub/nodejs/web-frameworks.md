---
title: Introducción a los marcos Web de node. js en Windows
description: Una guía para ayudarle a empezar a trabajar con Marcos Web de node. js en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, Learning NodeJS, nodo en Windows, nodo en WSL, nodo en Linux en Windows, nodo de instalación en Windows, NodeJS con vs Code, desarrollar con nodo en Windows, desarrollar con NodeJS en Windows, instalar nodo en WSL, NodeJS en Windows Subsistema para Linux
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: a3c1cd980884dc50107c05207665d0c1ef88938e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314959"
---
# <a name="get-started-with-nodejs-web-frameworks-on-windows"></a>Introducción a los marcos Web de node. js en Windows

Una guía paso a paso que le ayudará a empezar a usar marcos Web de node. js en Windows, incluidos los siguientes:. js, Nuxt. js y Gatsby.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de node. js con WSL 2](./setup-on-wsl2.md), entre los que se incluyen:

- Instale Windows 10 Insider Preview compilación 18932 o posterior.
- Habilite la característica WSL 2 en Windows.
- Instale una distribución de Linux (Ubuntu 18,04 para nuestros ejemplos). Puede comprobarlo con: `wsl lsb_release -a`
- Asegúrese de que la distribución de Ubuntu 18,04 se está ejecutando en el modo WSL 2. (WSL puede ejecutar distribuciones en el modo V1 o V2). Para comprobarlo, abra PowerShell y escriba: `wsl -l -v`
- Con PowerShell, establezca Ubuntu 18,04 como su distribución predeterminada, con: `wsl -s ubuntu 18.04`

## <a name="get-started-with-nextjs"></a>Introducción a Next. js

Next. js es un marco para crear aplicaciones JavaScript representadas por el servidor basadas en React. js, node. js, WebPack y Babel. js. Es básicamente un proyecto reutilizable para reAct, diseñado con atención a los procedimientos recomendados, que le permite crear aplicaciones web "universales" de manera sencilla y coherente, con prácticamente cualquier configuración. Estas aplicaciones web "universales" representadas por servidor también se denominan a veces "isomórficos", lo que significa que el código se comparte entre el cliente y el servidor.

Para crear un siguiente proyecto. js, que incluye la instalación de Next, reAct y reAct-DOM:

1. Abra el terminal de WSL (es decir, Ubuntu 18,04).

2. Cree una nueva carpeta de proyecto: `mkdir NextProjects` y escriba ese directorio: `cd NextProjects`.

3. Instale Next. js y cree un proyecto (reemplazando "My-Next-app" por lo que le gustaría llamar a su aplicación): `npm create next-app my-next-app`.

4. Una vez instalado el paquete, cambie los directorios a la nueva carpeta de la aplicación, `cd my-next-app` y, a continuación, use `code .` para abrir el proyecto. js siguiente en VS Code. Esto le permitirá ver el siguiente marco. js que se ha creado para la aplicación. Tenga en cuenta que VS Code ha abierto la aplicación en un entorno de WSL remoto (como se indica en la pestaña verde en la parte inferior izquierda de la ventana de VS Code). Esto significa que mientras usa VS Code para editar en el sistema operativo Windows, todavía está ejecutando la aplicación en el sistema operativo Linux.

    ![WSL: extensión remota](../images/wsl-remote-extension.png)

5. Hay 3 comandos que debe conocer una vez instalado el siguiente. js:

    - `npm run dev` para ejecutar una instancia de desarrollo con recarga en caliente, seguimiento de archivos y tarea que se vuelve a ejecutar.
    - `npm run build` para compilar el proyecto.
    - `npm start` para iniciar la aplicación en modo de producción.

    Abra el terminal WSL integrado en VS Code (**Ver terminal de >** ). Asegúrese de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/NextProjects/my-next-app$`). Después, intente ejecutar una instancia de desarrollo de la nueva aplicación. js siguiente con: `npm run dev`

6. El servidor de desarrollo local se iniciará y, una vez que las páginas del proyecto terminen de compilar, el terminal mostrará "compilado correctamente-listo en [http://localhost:3000](http://localhost:3000)". Seleccione este vínculo localhost para abrir la nueva aplicación. js siguiente en un explorador Web.

    ![La siguiente aplicación. js que se ejecuta en localhost: 3000](../images/next-app.png)

7. Abra el archivo `pages/index.js` en el editor de VS Code. Busque el título de la página `<h1 className='title'>Welcome to Next.js!</h1>` y cámbielo a `<h1 className='title'>This is my new Next.js app!</h1>`. Con el explorador Web todavía abierto en localhost: 3000, guarde el cambio y observe que la característica de recarga en caliente compila y actualiza automáticamente el cambio en el explorador.

8. Veamos el modo en que Next. js controla los errores. Quite la etiqueta de cierre `</h1>` para que el código de título tenga el siguiente aspecto: `<h1 className='title'>This is my new Next.js app!`. Guarde este cambio y observe que se mostrará el error "no se pudo compilar" en el explorador y en el terminal, lo que le permitirá saber que se espera una etiqueta de cierre para `<h1>`. Reemplace la etiqueta de cierre `</h1>`, guárdela y la página se volverá a cargar.

Puede usar el depurador de VS Code con la siguiente aplicación. js seleccionando la tecla F5 o yendo a **ver > depurar** (Ctrl + Mayús + D) y **Ver > consola de depuración** (Ctrl + Mayús + Y) en la barra de menús. Si selecciona el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guarde los detalles de configuración de la depuración. Para obtener más información, vea [depuración de vs Code](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code ventana de depuración y el icono de configuración Launch. JSON](../images/vscode-debug-launch-configuration.png)

Para obtener más información acerca de Next. js, consulte los [documentos de. js siguientes](https://nextjs.org/docs).

## <a name="get-started-with-nuxtjs"></a>Introducción a Nuxt. js

Nuxt. js es un marco para crear aplicaciones JavaScript representadas por el servidor basadas en Vue. js, node. js, WebPack y Babel. js. Fue inspirado en el siguiente. js. Es básicamente un proyecto reutilizable para Vue. Al igual que el siguiente. js, se ha diseñado con atención a los procedimientos recomendados y permite crear aplicaciones web "universales" de manera sencilla y coherente, con prácticamente cualquier configuración. Estas aplicaciones web "universales" representadas por servidor también se denominan a veces "isomórficos", lo que significa que el código se comparte entre el cliente y el servidor.

Para crear un proyecto de Nuxt. js, que incluirá la respuesta a una serie de preguntas sobre el tipo de marco de trabajo de servidor integrado, el marco de la interfaz de usuario, el marco de pruebas, el modo, los módulos y el pelusa que quiere instalar:

1. Abra el terminal de WSL (es decir, Ubuntu 18,04).

2. Cree una nueva carpeta de proyecto: `mkdir NuxtProjects` y escriba ese directorio: `cd NuxtProjects`.

3. Instale Nuxt. js y cree un proyecto (reemplazando "My-Nuxt-app" por lo que le gustaría llamar a su aplicación): `npm create nuxt-app my-nuxt-app`

4. El instalador de Nuxt. js ahora le preguntará las siguientes preguntas:
    - Nombre del proyecto: My-nuxtjs-App
    - Descripción del proyecto: Descripción de la aplicación Nuxt. js.
    - Nombre del autor: Utilizo mi alias de GitHub.
    - Elija el administrador de paquetes: Hilado o **NPM** : usamos NPM para nuestros ejemplos.
    - Elija marco de interfaz de usuario: Ninguno, Vue Design Vue, bootstrap Vue, etc. Vamos a elegir **Vuetify** para este ejemplo, pero la comunidad Vue creó un buen resumen comparando [estos marcos de interfaz de usuario](https://vue-community.org/guide/ecosystem/ui-libraries.html#summary-tldr) para ayudarle a elegir el mejor ajuste para su proyecto.
    - Elija marcos de servidor personalizados: None, AdonisJs, Express, Fastify, etc. Vamos a elegir **ninguno** en este ejemplo, pero puede encontrar una [comparación del marco de trabajo de 2019-2020 Server](https://dev.to/santypk4/introducing-the-best-10-node-js-frameworks-for-2019-and-2020-mcm) en el sitio de dev.to.
    - Elija módulos de Nuxt. js (use la barra espaciadora para seleccionar módulos o simplemente escriba si no desea): Axios (para simplificar las solicitudes HTTP) o [compatibilidad con PWA](https://pwa.nuxtjs.org/) (para agregar un servicio de trabajo, un archivo manifest. JSON, etc.). No vamos a agregar un módulo para este ejemplo.
    - Elija Herramientas de detección de errores: Archivos preconfigurados de **ESLint**, adornarla y pelusa. Vamos a elegir **ESLint** (una herramienta para analizar el código y avisarle de posibles errores).
    - Elegir un marco de pruebas: **None**, jest, Ava. Vamos a elegir **ninguna** , ya que no trataremos las pruebas en esta guía de inicio rápido.
    - Elegir el modo de representación: **Universal (SSR)** o aplicación de una sola página (Spa). Vamos a elegir **universal (SSR)** en nuestro ejemplo, pero los [documentos de Nuxt. js](https://nuxtjs.org/guide#server-rendered-universal-ssr-) señalan algunas de las diferencias: SSR que requiere un servidor node. js que ejecute al servidor: representar la aplicación y Spa para el hospedaje estático.
    - Elija Herramientas de desarrollo: **jsconfig. JSON** (recomendado para vs code para que funcione la finalización del código de IntelliSense)

5. Una vez creado el proyecto, `cd my-nuxtjs-app` para escribir el directorio del proyecto de Nuxt. js y, a continuación, escriba `code .` para abrir el proyecto en el entorno de VS Code WSL-Remote.

    ![WSL: extensión remota](../images/wsl-remote-extension.png)

6. Hay 3 comandos que debe conocer una vez instalado Nuxt. js:

    - `npm run dev` para ejecutar una instancia de desarrollo con recarga en caliente, seguimiento de archivos y tarea que se vuelve a ejecutar.
    - `npm run build` para compilar el proyecto.
    - `npm start` para iniciar la aplicación en modo de producción.

    Abra el terminal WSL integrado en VS Code (**Ver terminal de >** ). Asegúrese de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/NuxtProjects/my-nuxt-app$`). Después, intente ejecutar una instancia de desarrollo de la nueva aplicación de Nuxt. js con: `npm run dev`

6. El servidor de desarrollo local se iniciará (mostrando algunos tipos de barras de progreso de las compilaciones de cliente y servidor). Una vez que el proyecto termine de compilar, el terminal mostrará "compilado correctamente" junto con cuánto tiempo tardó en compilarse. Apunte el explorador Web a [http://localhost:3000](http://localhost:3000) para abrir la nueva aplicación de Nuxt. js.

    ![La aplicación Nuxt. js que se ejecuta en localhost: 3000](../images/nuxt-app.png)

7. Abra el archivo `pages/index.vue` en el editor de VS Code. Busque el título de la página `<v-card-title class="headline">Welcome to the Vuetify + Nuxt.js template</v-card-title>` y cámbielo a `<v-card-title class="headline">This is my new Nuxt.js app!</v-card-title>`. Con el explorador Web todavía abierto en localhost: 3000, guarde el cambio y observe que la característica de recarga en caliente compila y actualiza automáticamente el cambio en el explorador.

8. Veamos cómo Nuxt. js controla los errores. Quite la etiqueta de cierre `</v-card-title>` para que el código de título tenga el siguiente aspecto: `<v-card-title class="headline">This is my new Nuxt.js app!`. Guarde este cambio y observe que se mostrará un error de compilación en el explorador y en el terminal, lo que le permitirá saber que falta una etiqueta de cierre para `<v-card-title>`, junto con los números de línea en los que se puede encontrar el error en el código. Reemplace la etiqueta de cierre `</v-card-title>`, guárdela y la página se volverá a cargar.

Puede usar el depurador de VS Code con la aplicación Nuxt. js seleccionando la tecla F5 o yendo a **ver > depurar** (Ctrl + Mayús + D) y **Ver > consola de depuración** (Ctrl + Mayús + Y) en la barra de menús. Si selecciona el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guarde los detalles de configuración de la depuración. Para obtener más información, vea [depuración de vs Code](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code ventana de depuración y el icono de configuración Launch. JSON](../images/vscode-debug-launch-configuration.png)

Para más información sobre Nuxt. js, consulte la [Guía de Nuxt. js](https://nuxtjs.org/guide).

## <a name="get-started-with-gatsbyjs"></a>Introducción a Gatsby. js

Gatsby. js es un marco de trabajo generador de sitios estático basado en React. js, en lugar de ser representado por servidor como Next. js y Nuxt. js. Un generador de sitios estáticos genera HTML estático en tiempo de compilación. No requiere un servidor. Next. js y Nuxt. js generan HTML en tiempo de ejecución (cada vez que entra una nueva solicitud). Requieren un servidor para ejecutarse. Gatsby también dicta cómo controlar los datos de la aplicación (con GraphQL), mientras que Next. js y Nuxt. js dejan esa decisión.

Para crear un proyecto de Gatsby. js:

1. Abra el terminal de WSL (es decir, Ubuntu 18,04).
2. Cree una nueva carpeta de proyecto: `mkdir GatsbyProjects` y escriba ese directorio: `cd GatsbyProjects`
3. Use NPM para instalar la CLI de Gatsby: `npm install -g gatsby-cli`. Una vez instalado, Compruebe la versión con `gatsby --version`.
4. Cree el proyecto de Gatsby. js: `gatsby new my-gatsby-app`
5. Una vez instalado el paquete, cambie los directorios a la nueva carpeta de la aplicación, `cd my-gatsby-app` y, a continuación, use `code .` para abrir el proyecto Gatsby en VS Code. Esto le permitirá ver el marco de trabajo de Gatsby. js que se ha creado para la aplicación mediante el explorador de archivos de VS Code. Tenga en cuenta que VS Code ha abierto la aplicación en un entorno de WSL remoto (como se indica en la pestaña verde en la parte inferior izquierda de la ventana de VS Code). Esto significa que mientras usa VS Code para editar en el sistema operativo Windows, todavía está ejecutando la aplicación en el sistema operativo Linux.

    ![WSL: extensión remota](../images/wsl-remote-extension.png)

6. Hay 3 comandos que debe conocer una vez instalado Gatsby:

    - `gatsby develop` para ejecutar una instancia de desarrollo con recarga en caliente.
    - `gatsby build` para crear una compilación de producción.
    - `gatsby serve` para iniciar la aplicación en modo de producción.

    Abra el terminal WSL integrado en VS Code (**Ver terminal de >** ). Asegúrese de que la ruta de acceso del terminal apunta al directorio del proyecto (es decir, `~/GatsbyProjects/my-gatsby-app$`). Después, intente ejecutar una instancia de desarrollo de la aplicación nueva con: `gatsby develop`

7. Una vez finalizada la compilación del nuevo proyecto de Gatsby, el terminal mostrará "ahora puede ver Gatsby-Starter-default en el explorador. [http://localhost:8000/](http://localhost:8000/). " Seleccione este vínculo localhost para ver el nuevo proyecto compilado en un explorador Web.

> [!NOTE]
> Observará que la salida de terminal también le indica que puede "ver GraphiQL, un IDE en el explorador, para explorar los datos y el esquema del sitio: [http://localhost:8000/___graphql](http://localhost:8000/___graphql)". GraphQL consolida las API en un IDE de autodocumentación (GraphiQL) integrado en Gatsby. Además de explorar los datos y el esquema del sitio, puede realizar operaciones de GraphQL como consultas, mutaciones y suscripciones. Para obtener más información, vea [Introducción a GraphiQL](https://www.gatsbyjs.org/docs/running-queries-with-graphiql/).

8. Abra el archivo `src/pages/index.js` en el editor de VS Code. Busque el título de la página `<h1 >Hi people</h1>` y cámbielo a `<h1 >Hi (Your Name)!</h1>`. Con el explorador Web todavía abierto en localhost: 8000, guarde el cambio y observe que la característica de recarga en caliente compila y actualiza automáticamente el cambio en el explorador.

    ![La aplicación Gatsby. js que se ejecuta en localhost: 3000](../images/gatsby-app.png)

9. Veamos el modo en que Next. js controla los errores. Quite la etiqueta de cierre `</h1>` para que el código de título tenga el siguiente aspecto: `<h1>Hi (Your Name)!`. Guarde este cambio y observe que se mostrará el error "no se pudo compilar" en el explorador y en el terminal, lo que le permitirá saber que se espera una etiqueta de cierre para `<h1>`. Reemplace la etiqueta de cierre `</h1>`, guárdela y la página se volverá a cargar.

Puede usar el depurador de VS Code con la siguiente aplicación. js seleccionando la tecla F5 o yendo a **ver > depurar** (Ctrl + Mayús + D) y **Ver > consola de depuración** (Ctrl + Mayús + Y) en la barra de menús. Si selecciona el icono de engranaje en la ventana Depurar, se creará un archivo de configuración de inicio (`launch.json`) para que guarde los detalles de configuración de la depuración. Para obtener más información, vea [depuración de vs Code](https://code.visualstudio.com/docs/editor/debugging#_launch-configurations).

![VS Code ventana de depuración y el icono de configuración Launch. JSON](../images/vscode-debug-launch-configuration.png)

Para obtener más información sobre Gatsby, consulte los [documentos de Gatsby. js](https://www.gatsbyjs.org/docs/).
