---
title: Introducción al uso de NodeJS en Windows para principiantes
description: Una guía para ayudar a los principiantes a empezar a trabajar con el desarrollo de node. js en Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: NodeJS, node. js, Windows 10, Microsoft, Learning NodeJS, nodo en Windows, nodo en Windows para principiantes, desarrollo con node en Windows, desarrollador con NodeJS en Windows
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 433eb5701696f590f10d8b3276481098b9ec073d
ms.sourcegitcommit: 8f9cea69f33b06166fec22677eaa43466352c14d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75657087"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Introducción al uso de node. js en Windows para principiantes

Si no está familiarizado con node. js, esta guía le ayudará a empezar a trabajar con algunos aspectos básicos.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de node. js en ventanas nativas](./setup-on-windows.md), entre las que se incluyen:

- Instale un administrador de versiones de node. js.
- Instale Visual Studio Code.

La instalación de node. js directamente en Windows es la manera más sencilla de empezar a realizar operaciones básicas de node. js con una cantidad mínima de configuración.

Una vez que esté listo para usar node. js para desarrollar aplicaciones para producción, lo que suele implicar la implementación en un servidor Linux, se recomienda [configurar el entorno de desarrollo de node. js con WSL2](./setup-on-wsl2.md). Aunque es posible implementar Web Apps en servidores de Windows, es mucho más habitual [usar servidores Linux para hospedar las aplicaciones de node. js](https://azure.microsoft.com/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Tipos de aplicaciones de Node.js

Node. js es un entorno de tiempo de ejecución de JavaScript que se usa principalmente para crear aplicaciones Web. De otro modo, se trata de una implementación del lado servidor de JavaScript que se usa para escribir el back-end de una aplicación. (Aunque muchos marcos de node. js también pueden controlar el front-end). Estos son algunos ejemplos de lo que puede crear con node. js.

- **Aplicaciones de una sola página (Spa)** : son aplicaciones web que funcionan dentro de un explorador y no necesitan recargar una página cada vez que se usa para obtener datos nuevos. Algunos ejemplos de spa incluyen aplicaciones de redes sociales, aplicaciones de correo electrónico o mapas, texto en línea o herramientas de dibujo, etc.
- **Aplicaciones en tiempo real (ATR)** : son aplicaciones web que permiten a los usuarios recibir información tan pronto como la publique un autor, en lugar de requerir que el usuario (o software) Compruebe un origen periódicamente para buscar actualizaciones. Algunos ATR de ejemplo incluyen aplicaciones de mensajería instantánea o salones de chat, juegos multijugador en línea que se pueden reproducir en el explorador, documentos de colaboración en línea, almacenamiento de la comunidad, aplicaciones de videoconferencia, etc.
- **Aplicaciones de streaming de datos**: son aplicaciones (o servicios) que envían datos o contenido a medida que llegan (o se crean) mientras se mantiene abierta la conexión para continuar con la descarga de datos, contenido o componentes según sea necesario. Algunos ejemplos incluyen aplicaciones de streaming de audio y vídeo.
- **API de REST**: son interfaces que proporcionan datos para que la aplicación Web de otra persona interactúe con ellos. Por ejemplo, un servicio de API de calendario podría proporcionar fechas y horas para una ubicación de concierto que podría ser usada por el sitio web de eventos locales de otro usuario.
- **Aplicaciones representadas en el servidor (SSRs)** : estas aplicaciones web se pueden ejecutar en el cliente (en el explorador o en el front-end) y en el servidor (el back-end), lo que permite que las páginas que son dinámicas se muestren (generar HTML para) el contenido que se sepa y que capte rápidamente el contenido que no se conoce como disponible. A menudo se conocen como aplicaciones "isomórficos" o "universales". SSRs usa métodos de SPA en que no es necesario volver a cargarlos cada vez que se usa. Sin embargo, SSRs ofrece algunas ventajas que pueden o no ser importantes para usted, como hacer que el contenido de su sitio aparezca en los resultados de Google Search y proporcionar una imagen de vista previa cuando los vínculos a la aplicación se comparten en un medio social como Twitter o Facebook. El posible inconveniente es que requieren un servidor node. js que se ejecute constantemente. En términos de ejemplos, una aplicación de redes sociales que admita eventos que los usuarios quiera que aparezcan en los resultados de la búsqueda y en los medios sociales puede beneficiarse de SSR, mientras que una aplicación de correo electrónico puede ser buena como SPA. También puede ejecutar aplicaciones no de SPA representadas por el servidor, que son algo parecido a un blog de WordPress. Como puede ver, las cosas pueden resultar complicadas, solo tiene que decidir lo que es importante.
- **Herramientas de línea de comandos**: le permiten automatizar las tareas repetitivas y, a continuación, distribuir la herramienta en el ecosistema de node. js. Un ejemplo de una herramienta de línea de comandos es Rizo, que es la dirección URL del cliente y se usa para descargar contenido de una dirección URL de Internet. Rizo se usa a menudo para instalar elementos como node. js o, en nuestro caso, un administrador de versiones de node. js.
- **Programación de hardware**: aunque no es tan popular como Web Apps, node. js aumenta la popularidad de los usos de IoT, como la recopilación de datos de sensores, balizas, transmisores, motores o cualquier cosa que genere grandes cantidades de datos. Node. js puede habilitar la recopilación de datos, el análisis de los datos, la comunicación entre un dispositivo y un servidor, y la realización de acciones basadas en el análisis. NPM contiene más de 80 paquetes para controladores Arduino, Raspberry PI, Intel IoT Edison, varios sensores y dispositivos Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Pruebe a usar node. js en VS Code

1. Abra la línea de comandos (símbolo del sistema, PowerShell o cualquier cosa que prefiera) y cree un nuevo directorio: `mkdir HelloNode`y, a continuación, escriba el directorio: `cd HelloNode`

2. Cree un archivo de JavaScript denominado "app. js" con una variable denominada "msg" dentro de: `echo var msg > app.js`

3. Abra el directorio y el archivo app. js en VS Code:: `code .`

4. Agregue una variable de cadena simple ("Hola mundo") y, a continuación, envíe el contenido de la cadena a la consola escribiendo esto en el archivo "app. js":

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Para ejecutar el archivo "app. js" con node. js. Abra el terminal justo dentro de VS Code; para ello, seleccione **ver** > **terminal** (o presione Ctrl + ' con el carácter de acento grave). Si necesita cambiar el terminal predeterminado, seleccione el menú desplegable y elija **seleccionar shell predeterminado**.

6. En el terminal, escriba: `node app.js`. Debería ver la salida: "Hola mundo".

> [!NOTE]
> Tenga en cuenta que, al escribir `console` en el archivo ' App. js ', VS Code muestra las opciones admitidas relacionadas con el objeto [`console`](https://developer.mozilla.org/docs/Web/API/Console) para que elija usar IntelliSense. Pruebe a experimentar con IntelliSense mediante otros [objetos de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Pruebe el nuevo [terminal de Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si tiene previsto usar varias líneas de comandos (Ubuntu, PowerShell, símbolo del sistema de Windows, etc.) o si desea [personalizar el terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluidos el texto, los colores de fondo, los enlaces de teclado, los paneles de ventana múltiples, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configuración de un marco de trabajo de aplicación web básica con Express

Express es una plataforma de Node.js mínima, flexible y simplificada que facilita el desarrollo de una aplicación web que pueda controlar varios tipos de solicitudes, como GET, PUT, POST y DELETE. Express incluye un generador de aplicaciones que se creará automáticamente una arquitectura de archivos para la aplicación.

Para crear un proyecto con Express. js:

1. Abra la línea de comandos (símbolo del sistema, PowerShell o cualquier cosa que prefiera).
2. Cree una nueva carpeta de proyecto: `mkdir ExpressProjects` y escriba ese directorio: `cd ExpressProjects`
3. Use Express para crear una plantilla de proyecto HelloWorld: `npx express-generator HelloWorld --view=pug`

>[!NOTE]
> Vamos a usar el comando de `npx` aquí para ejecutar el paquete de nodo Express. js sin instalarlo realmente (o bien instalarlo temporalmente en función de cómo desee imaginarlo). Si intenta usar el comando `express` o comprobar la versión de Express instalada mediante: `express --version`, recibirá una respuesta de que no se puede encontrar Express. Si desea instalar Express de forma global para usar una y otra vez, use: `npm install -g express-generator`. Puede ver una lista de los paquetes que ha instalado NPM mediante `npm list`. Se ordenarán por profundidad (número de profundización de los directorios anidados). Los paquetes que instaló estarán en la profundidad 0. Las dependencias de dichos paquetes estarán en la profundidad 1, las dependencias adicionales en la profundidad 2, etc. Para obtener más información, consulte [diferencia entre NPX y NPM](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) en stackoverflow.

4. Examine los archivos y las carpetas que se incluyen al abrir el proyecto en VS Code, con: `code .`

   Los archivos que genera Express crearán una aplicación web que use una arquitectura que, al principio, puede parecer algo abrumadora. Verá en la ventana del **Explorador** de vs Code (Ctrl + Mayús + E para ver) que se han generado los siguientes archivos y carpetas:

   - `bin`. contiene el archivo ejecutable que inicia la aplicación. Activa un servidor (en el puerto 3000, si no se indica ninguna alternativa) y configura un control de errores básico. 
   - `public`. contiene todos los archivos de acceso público, incluidos archivos de JavaScript, hojas de estilos de CSS, archivos de fuentes, imágenes y cualquier otro recurso que los usuarios necesitan cuando se conectan al sitio web.
   - `routes`. contiene todos los controladores de ruta de la aplicación. Dos archivos, `index.js` y `users.js`, se generan automáticamente en esta carpeta para servir como ejemplos de cómo separar la configuración de ruta de la aplicación.
   - `views`. contiene los archivos que usa el motor de plantillas. Express está configurado para buscar aquí una vista coincidente cuando se llama al método de representación. El motor de plantillas predeterminado es Jade, pero está en desuso a favor de Pug, por lo que usamos la marca `--view` para cambiar el motor de vistas (plantillas). Para ver las opciones de la marca `--view` y otras, use `express --help`.
   - `app.js`. el punto inicial de la aplicación. Carga todo y empieza a atender las solicitudes de usuario. En esencia, se trata del elemento que aglutina todas las piezas.
   - `package.json`. contiene la descripción del proyecto, el administrador de scripts y el manifiesto de la aplicación. Su principal objetivo es hacer el seguimiento de las dependencias de la aplicación y sus respectivas versiones.

5. Ahora debe instalar las dependencias que Express usa para compilar y ejecutar la aplicación HelloWorld Express (los paquetes usados para tareas como la ejecución del servidor, tal como se define en el archivo `package.json`). Dentro de VS Code, abra el terminal; para ello, seleccione **ver** > **terminal** (o presione Ctrl + ' con el carácter de acento grave), asegúrese de que todavía está en el directorio del proyecto "HelloWorld". Instale las dependencias de paquetes Express con:

```bash
npm install
```

6. En este momento, tiene el marco de trabajo configurado para una aplicación web de varias páginas con acceso a una gran variedad de API y de métodos de utilidad HTTP y middleware, lo que facilita la creación de una API sólida. Inicie la aplicación Express en un servidor virtual; para ello, escriba:

```bash
npx cross-env DEBUG=HelloWorld:* npm start
```

> [!TIP]
> La parte `DEBUG=myapp:*` del comando anterior significa que está indicando a node. js que desea activar el registro con fines de depuración. No olvide reemplazar "MyApp" por el nombre de la aplicación. Puede encontrar el nombre de la aplicación en el archivo de `package.json` en la propiedad "Name". Con `npx cross-env` se establece la variable de entorno de `DEBUG` en cualquier terminal, pero también se puede establecer con la forma específica del terminal. El comando `npm start` indica a NPM que ejecute los scripts en el archivo de `package.json`.

7. Ahora puede ver la aplicación en ejecución; para ello, abra un explorador Web y vaya a: **localhost: 3000**

   ![Captura de la aplicación Express en ejecución en un explorador](../images/express-app.png)

8. Ahora que la aplicación HelloWorld Express se ejecuta localmente en el explorador, intente realizar un cambio; para ello, abra la carpeta "views" en el directorio del proyecto y seleccione el archivo "index. Pug". Una vez abierto, cambie `h1= title` a `h1= "Hello World!"` y seleccione **Guardar** (Ctrl + S). Para ver el cambio, actualice la dirección URL de **localhost: 3000** en el explorador Web.

9. Para detener la ejecución de la aplicación Express, en el terminal, escriba: **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Prueba con un módulo de Node.js

Node.js tiene herramientas para ayudar con el desarrollo de aplicaciones web de servidor, algunas integradas y muchas más disponibles a través de npm. Estos módulos pueden ayudar con muchas tareas:

|Herramienta               |Usado para                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|gm, sharp          |Manipulación de imágenes, incluida la edición, el cambio de tamaño, la compresión, etc., directamente en el código de JavaScript |
|PDFKit             |Generación de archivos PDF                                                                                            |
|validator.js       |Validación de cadenas                                                                                         |
|imagemin, UglifyJS2|Minificación                                                                                              |
|spritesmith        |Generación de hojas de sprite                                                                                   |
|winston            |Registro                                                                                                  |
|commander.js       |Creación de aplicaciones de línea de comandos                                                                       |

Usaremos el módulo del sistema operativo integrado para obtener información sobre el sistema operativo del equipo:

1) En la línea de comandos, abra la CLI de node. js. Verá el mensaje de `>` que le permite saber que está usando node. js después de escribir: `node`

2) Para identificar el sistema operativo que está usando actualmente (que debe devolver una respuesta que le permita saber que está en Windows), escriba: `os.platform()`

3) Para comprobar la arquitectura de la CPU, escriba: `os.arch()`

4) Para ver las CPU disponibles en el sistema, escriba: `os.cpus()`

5) Para dejar la CLI de Node.js, escriba `.exit` o seleccione Ctrl-C dos veces.

   > [!TIP]
   > Puede usar el módulo de sistema operativo node. js para realizar acciones como comprobar la plataforma y devolver una variable específica de la plataforma: Win32/. bat para el desarrollo de Windows, Darwin/. sh para Mac/UNIX, Linux, SunOS, etc. (por ejemplo, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido algunas cosas básicas sobre lo que puede hacer con node. js, ha intentado usar la línea de comandos de node. js en VS Code, ha creado una aplicación web sencilla con Express. js y la ha ejecutado localmente en el explorador Web y después ha intentado usar algunos de los módulos node. js integrados. Para obtener más información sobre cómo instalar y usar algunos marcos Web de node. js populares, continúe con la siguiente guía, en la que se trata el siguiente. js (un marco web presentado por el servidor basado en React), Nuxt. js (un marco web presentado por el servidor basado en Vue) y Gatsby (a marco web representado de forma estática basada en React). También puede ir directamente a aprendizaje sobre cómo trabajar con bases de datos de MongoDB o PostgreSQL o con contenedores de Docker.

- [Introducción a los marcos Web de node. js en Windows](./web-frameworks.md)
- [Introducción a la conexión de aplicaciones de node. js a una base de datos](./databases.md)
- [Introducción al uso de contenedores de Docker con node. js](./containers.md)
