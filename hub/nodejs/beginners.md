---
title: Introducción al uso de NodeJS en Windows para principiantes
description: Una guía para ayudar a los principiantes a empezar con el desarrollo de Node.js en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, Node.js, Windows 10, Microsoft, aprendizaje de NodeJS, Node en Windows, Node en Windows para principiantes, desarrollo con Node en Windows, desarrollador con NodeJS en Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657087"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Introducción al uso de Node.js en Windows para principiantes

Si no está familiarizado con Node.js, esta guía le ayudará a conocer algunos aspectos básicos.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de Node.js en Windows nativo](./setup-on-windows.md), que incluyen:

- Instalar un administrador de versiones de Node.js.
- Instalar Visual Studio Code.

La instalación de Node.js directamente en Windows es la manera más sencilla de empezar a realizar operaciones básicas de Node.js con la mínima configuración.

Cuando esté listo para usar Node.js para desarrollar aplicaciones para producción, lo que suele implicar la implementación en un servidor Linux, se recomienda que [configure el entorno de desarrollo de Node.js con WSL2](./setup-on-wsl2.md). Aunque es posible implementar aplicaciones web en servidores Windows, es mucho más común [usar servidores Linux para hospedar las aplicaciones de Node.js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Tipos de aplicaciones de Node.js

Node.js es un entorno de tiempo de ejecución de JavaScript que se usa principalmente para crear aplicaciones web. Dicho de otro modo, se trata de una implementación del lado servidor de JavaScript que se usa para escribir el back-end de una aplicación. (No obstante, muchos marcos de Node.js también pueden controlar el front-end). Estos son algunos ejemplos de lo que puede crear con Node.js.

- **Aplicaciones de una sola página (SPA)** : aplicaciones web que funcionan dentro de un explorador y no tienen que volver a cargar una página cada vez que se usa para obtener datos nuevos. Algunos ejemplos de SPA incluyen aplicaciones de redes sociales, correo electrónico o mapas, dibujo o texto en línea, etc.
- **Aplicaciones en tiempo real (RTA)** : aplicaciones web que permiten a los usuarios recibir información en cuanto la publica un autor, en lugar de requerir que el usuario (o el software) compruebe un origen periódicamente para comprobar si hay actualizaciones. Algunas RTA de ejemplo incluyen aplicaciones de mensajería instantánea o salones de chat, juegos multijugador en línea que se pueden reproducir en el explorador, documentos de colaboración en línea, almacenamiento de la comunidad, aplicaciones de videoconferencia, etc.
- **Aplicaciones de streaming de datos**: aplicaciones (o servicios) que envían datos o contenido a medida que llegan (o se crean) mientras mantienen la conexión abierta para seguir descargando datos, contenido o componentes según sea necesario. Algunos ejemplos incluyen aplicaciones de streaming de audio y vídeo.
- **API de REST**: interfaces que proporcionan datos para la interacción con la aplicación web de otra persona. Por ejemplo, un servicio de la API de Calendario podría proporcionar fechas y horas para una ubicación de concierto que podrían usarse en un sitio web de eventos locales de otra persona.
- **Aplicaciones representadas en el lado servidor (SSR)** : estas aplicaciones web se pueden ejecutar en el cliente (explorador o front-end) y en el servidor (back-end), lo que permite que las páginas dinámicas muestren (generen HTML para) el contenido conocido y capten rápidamente el contenido desconocido a medida que esté disponible. A menudo se denominan aplicaciones "isomórficas" o "universales". Las SSR utilizan métodos de SPA, ya que no se tienen que volver a cargar cada vez que se utilizan. Sin embargo, las SSR ofrecen algunas ventajas que pueden parecerte o no importantes, como hacer que el contenido de tu sitio aparezca en los resultados de la búsqueda de Google y proporcionar una imagen de vista previa cuando los vínculos a la aplicación se compartan en redes sociales, como Twitter o Facebook. Un posible inconveniente es que requieren un servidor Node.js que se ejecute constantemente. En términos de ejemplos, una aplicación de red social que promociona eventos que los usuarios quieren que aparezcan en los resultados de la búsqueda y en las redes sociales puede utilizar SSR, mientras que para una aplicación de correo electrónico será suficiente una SPA. También puedes ejecutar aplicaciones representadas por el servidor que no sean SPA, como, por ejemplo, un blog de WordPress. Como puedes ver, las cosas pueden complicarse. Solo tienes que decidir lo que es importante.
- **Herramientas de la línea de comandos**: le permiten automatizar tareas repetitivas y, a continuación, distribuir la herramienta en todo el ecosistema de Node.js. Un ejemplo de una herramienta de línea de comandos es cURL, que es una dirección URL de cliente y se usa para descargar contenido de una dirección URL de Internet. cURL se usa a menudo para instalar componentes como Node.js o, en nuestro caso, un administrador de versiones de Node.js.
- **Programación de hardware**: aunque no es tan popular como las aplicaciones web, Node.js está ganando popularidad para los usos de IoT, como la recopilación de datos de sensores, balizas, transmisores, motores o cualquier elemento que genere grandes cantidades de datos. Node.js puede habilitar la recopilación de datos, el análisis de dichos datos, la comunicación entre un dispositivo y un servidor, y la realización de acciones basadas en el análisis. NPM contiene más de 80 paquetes para controladores de Arduino, Raspberry PI, Intel IoT Edison, varios sensores y dispositivos Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Intente usar Node.js en VS Code

1. Abre la línea de comandos (símbolo del sistema, PowerShell o lo que prefieras), crea el nuevo directorio `mkdir HelloNode` y, a continuación, accede al directorio: `cd HelloNode`.

2. Crea un archivo de JavaScript denominado "app.js" con una variable denominada "msg" dentro de: `echo var msg > app.js`.

3. Abre el directorio y el archivo app.js en VS Code: `code .`.

4. Agrega una variable de cadena simple ("Hola mundo") y, a continuación, envía el contenido de la cadena a la consola. Para ello, escribe lo siguiente en el archivo "app.js":

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Ejecutar el archivo "app.js" con Node.js. Abre el terminal en VS Code. Para ello, selecciona **Ver** > **Terminal** (o selecciona Ctrl+` mediante el carácter de tilde grave). Si necesitas cambiar el terminal predeterminado, selecciona el menú desplegable y elige **Seleccionar el shell predeterminado**.

6. En el terminal, escribe: `node app.js`. Deberías ver la salida: "Hola mundo".

> [!NOTE]
> Ten en cuenta que, al escribir `console` en el archivo "app.js", VS Code muestra las opciones admitidas relacionadas con el objeto [`console`](https://developer.mozilla.org/docs/Web/API/Console) que puedes elegir al usar IntelliSense. Intenta experimentar con IntelliSense utilizando otros [objetos de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Prueba el nuevo [terminal de Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si tienes previsto usar varias líneas de comandos (Ubuntu, PowerShell, símbolo del sistema de Windows, etc.) o si quieres [personalizar el terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluido el texto, los colores de fondo, los enlaces de teclado, los paneles de varias ventanas, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configuración de un marco de aplicación web básico mediante Express

Express es un marco de trabajo de Node.js minimalista, flexible y simplificado que facilita el desarrollo de una aplicación web capaz de administrar varios tipos de solicitudes, como GET, PUT, POST y DELETE. Express incluye un generador de aplicaciones que creará automáticamente una arquitectura de archivos para la aplicación.

Para crear un proyecto con Express.js:

1. Abre la línea de comandos (símbolo del sistema, PowerShell o la prefieras).
2. Crea la nueva carpeta de proyecto `mkdir ExpressProjects` y accede al directorio `cd ExpressProjects`.
3. Usa Express para crear una plantilla de proyecto HelloWorld: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Aquí usamos el comando `npx` para ejecutar el paquete de Node Express.js sin instalarlo realmente (o bien instalándolo temporalmente en función de cómo quieras imaginarlo). Si intentas usar el comando `express` o comprobar la versión de Express instalada mediante `express --version`, recibirás una respuesta que te indicará que no se encuentra Express. Si quieres instalar Express de forma global para usarlo una y otra vez, utiliza `npm install -g express-generator`. Puedes ver una lista de los paquetes que ha instalado npm mediante `npm list`. Se mostrarán por profundidad (el número de directorios anidados en profundidad). Los paquetes que has instalado estarán en la profundidad 0. Las dependencias de ese paquete estarán en la profundidad 1, las más dependencias más alejadas en la profundidad 2, y así sucesivamente. Para más información, consulta el artículo sobre la [diferencia entre npx y npm](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) en Stackoverflow.

4. Para examinar los archivos y las carpetas que Express ha incluido, abre el proyecto en VS Code con `code .`.

   Los archivos que genera Express crearán una aplicación web que utiliza una arquitectura que puede parecer un poco abrumadora al principio. En la ventana del **Explorador** de VS Code (presiona Ctrl + Mayús + E para verla), observarás que se han generado los archivos y las carpetas siguientes:

   - `bin`. Contiene el archivo ejecutable que inicia la aplicación. Activa un servidor (en el puerto 3000 si no se proporciona ninguna alternativa) y configura el control de errores básico. 
   - `public`. Contiene todos los archivos a los que se accede públicamente, que incluyen archivos JavaScript, hojas de estilo CSS, archivos de fuente, imágenes y cualquier otro recurso que necesiten los usuarios cuando se conecten a tu sitio web.
   - `routes`. Contiene todos los controladores de ruta de la aplicación. En esta carpeta se generan automáticamente dos archivos, `index.js` y `users.js`, que sirven como ejemplos de cómo separar la configuración de la ruta de la aplicación.
   - `views`. Contiene los archivos que utiliza el motor de plantillas. Express está configurado para buscar aquí una vista coincidente cuando se llama al método de representación. El motor de plantillas predeterminado es Jade, pero está en desuso en favor de Pug, por lo que hemos usado la marca `--view` para cambiar el motor de vista (plantilla). Puede ver las opciones de marca `--view`, entre otras, mediante `express --help`.
   - `app.js`. Punto de inicio de la aplicación. Lo carga todo y comienza a atender las solicitudes de los usuarios. Básicamente, es el pegamento que une todas las partes.
   - `package.json`. Contiene la descripción del proyecto, el administrador de scripts y el manifiesto de la aplicación. Su finalidad principal es realizar un seguimiento de las dependencias de la aplicación y las versiones respectivas.

5. Ahora debes instalar las dependencias que Express usa para compilar y ejecutar la aplicación HelloWorld Express (los paquetes usados para tareas como la ejecución del servidor, tal como se define en el archivo `package.json`). En VS Code, abre el terminal mediante las opciones **Ver** > **Terminal** (o presiona Ctrl + `, con el carácter de tilde grave); asegúrate de que todavía estás en el directorio del proyecto "HelloWorld". Instala las dependencias de paquetes de Express con:

```bash
npm install
```

6. Ahora tienes el marco configurado para una aplicación web de varias páginas que tiene acceso a una gran variedad de API y métodos de utilidad HTTP y middleware, lo que facilita la creación de una API sólida. Inicia la aplicación Express en un servidor virtual. Para ello, escribe:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> La parte `DEBUG=myapp:*` del comando anterior significa que estás indicando a Node.js que desea activar el registro con fines de depuración. No olvides reemplazar "myapp" por el nombre de la aplicación. Puedes encontrar el nombre de la aplicación en el archivo `package.json` bajo la propiedad "name". Al usar `npx cross-env` se establece la variable de entorno `DEBUG` en cualquier terminal, pero también se puede establecer con la forma específica del terminal. El comando `npm start` indica a npm que ejecute los scripts en el archivo `package.json`.

7. Ahora, para ver la aplicación en ejecución, puedes abrir un explorador web e ir a **localhost:3000**.

   ![Captura de pantalla de la aplicación Express en ejecución en un explorador](../images/express-app.png)

8. Ahora que la aplicación HelloWorld Express se ejecuta localmente en el explorador, intenta realizar un cambio; para ello, abre la carpeta "views" en el directorio del proyecto y selecciona el archivo "index.pug". Una vez abierto, cambia `h1= title` a `h1= "Hello World!"` y selecciona **Guardar** (Ctrl + S). Para ver el cambio, actualiza la URL de **localhost:3000** en el explorador web.

9. Para detener la ejecución de la aplicación Express, escribe lo siguiente en el terminal: **Ctrl+C**

## <a name="try-using-a-nodejs-module"></a>Prueba con un módulo de Node.js

Node.js tiene herramientas que te ayudan a desarrollar aplicaciones web del lado servidor, algunas integradas y muchas más disponibles a través de npm. Estos módulos pueden ayudar con muchas tareas:

|Herramienta               |Se utiliza para                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulación de imágenes, que incluye la edición, el cambio de tamaño, la compresión, etc., directamente en el código de JavaScript |
|PDFKit             |Generación de PDF                                                                                            |
|validador.js       |Validación de cadenas                                                                                         |
|imagemin, UglifyJS2|Minificación                                                                                              |
|spritesmith        |Generación de hojas de Sprite                                                                                   |
|winston            |Registro                                                                                                  |
|commander.js       |Creación de aplicaciones de línea de comandos                                                                       |

Vamos a usar el módulo de sistema operativo integrado para obtener información sobre el sistema operativo del equipo:

1) En la línea de comandos, abre la CLI de Node.js. Verás el mensaje `>`, que te indica que estás usando Node.js después de escribir: `node`.

2) Para identificar el sistema operativo que estás usando actualmente (que debe devolver una respuesta que te indique que estás en Windows), escribe: `os.platform()`.

3) Para comprobar la arquitectura de la CPU, escribe: `os.arch()`.

4) Para ver las CPU disponibles en el sistema, escribe: `os.cpus()`.

5) Para salir de la CLI de Node.js escribe `.exit` o selecciona Ctrl + C dos veces.

   > [!TIP]
   > Puedes usar el módulo de sistema operativo de Node.js para realizar acciones como comprobar la plataforma y devolver una variable específica de la plataforma: Win32/.bat para el desarrollo de Windows, darwin/.sh para Mac/UNIX, Linux, SunOS, etc. (por ejemplo, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Pasos siguientes

En esta guía, has aprendido algunas cosas básicas sobre lo que puedes hacer con Node.js, has probado la línea de comandos de Node.js en VS Code, has creado una aplicación web simple con Express.js y la has ejecutado localmente en el explorador web. Después has intentado usar algunos de los módulos de Node.js integrados. Para obtener más información sobre cómo instalar y usar algunos marcos web de Node.js populares, consulta la guía siguiente, en la que se trata el Next.js (un marco web representado por el servidor basado en React), Nuxt.js (un marco web representado por el servidor basado en Vue) y Gatsby (un marco web representado de forma estática basado en React). También puedes ir directamente al aprendizaje sobre el uso de bases de datos de MongoDB o PostgreSQL o contenedores de Docker.

- [Introducción a los marcos web de Node.js en Windows](./web-frameworks.md)
- [Introducción a la conexión de aplicaciones de Node.js a una base de datos](./databases.md)
- [Introducción al uso de contenedores de Docker con Node.js](./containers.md)
