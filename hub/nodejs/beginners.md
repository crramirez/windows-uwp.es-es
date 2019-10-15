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
ms.openlocfilehash: 24a2ea5288a627ed884d549ca3b6f9c6930ce34e
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72315139"
---
# <a name="get-started-using-nodejs-on-windows-for-beginners"></a>Introducción al uso de node. js en Windows para principiantes

Si no está familiarizado con node. js, esta guía le ayudará a empezar a trabajar con algunos aspectos básicos.

## <a name="prerequisites"></a>Requisitos previos

En esta guía se da por supuesto que ya ha completado los pasos para [configurar el entorno de desarrollo de node. js con WSL 2](./setup-on-wsl2.md), entre los que se incluyen:

- Instale Windows 10 Insider Preview compilación 18932 o posterior.
- Habilite la característica WSL 2 en Windows.
- Instale una distribución de Linux (Ubuntu 18,04 para nuestros ejemplos). Puede comprobarlo con: `wsl lsb_release -a`
- Asegúrese de que la distribución de Ubuntu 18,04 se está ejecutando en el modo WSL 2. (WSL puede ejecutar distribuciones en el modo V1 o V2). Para comprobarlo, abra PowerShell y escriba: `wsl -l -v`
- Establezca Ubuntu 18,04 como su distribución predeterminada con PowerShell, con: `wsl -s ubuntu 18.04`

> [!NOTE]
> Aunque hay algunos pasos de configuración adicionales para usar el subsistema de Windows para Linux, con WSL 2, junto con VS Code y la extensión WSL remota, se proporcionará el flujo de trabajo de desarrollo de node. js más suave, así como la alineación con la mayoría de herramientas, procedimientos los artículos, tutoriales y [entornos de implementación](https://docs.microsoft.com/en-us/azure/javascript/tutorial-vscode-azure-app-service-node-01) . Sin embargo, si se le compromete a usar node. js en Windows, consulte nuestra guía para [configurar el entorno de desarrollo de node. js directamente en Windows](./setup-on-windows.md). En caso de que se encuentre en una situación (poco frecuente) de la necesidad de hospedar una aplicación node. js en un servidor Windows, el escenario más común parece ser el [uso de un proxy inverso](https://medium.com/intrinsic/why-should-i-use-a-reverse-proxy-if-node-js-is-production-ready-5a079408b2ca). Hay dos maneras de hacerlo: 1) [uso de iisnode](https://harveywilliams.net/blog/installing-iisnode) o [directamente](https://dev.to/petereysermans/hosting-a-node-js-application-on-windows-with-iis-as-reverse-proxy-397b). No se mantienen estos recursos y se recomienda [el uso de servidores Linux para hospedar las aplicaciones de node. js](https://azure.microsoft.com/en-us/develop/nodejs/).

## <a name="types-of-nodejs-applications"></a>Tipos de aplicaciones de node. js

Node. js es un entorno de tiempo de ejecución de JavaScript que se usa principalmente para crear aplicaciones Web. De otro modo, se trata de una implementación del lado servidor de JavaScript que se usa para escribir el back-end de una aplicación. (Aunque muchos marcos de node. js también pueden controlar el front-end). Estos son algunos ejemplos de lo que puede crear con node. js.

- **Aplicaciones de una sola página (Spa)** : Se trata de aplicaciones web que funcionan dentro de un explorador y no tienen que volver a cargar una página cada vez que se usa para obtener datos nuevos. Algunos ejemplos de spa incluyen aplicaciones de redes sociales, aplicaciones de correo electrónico o mapas, texto en línea o herramientas de dibujo, etc.
- **Aplicaciones en tiempo real (ATR)** : Se trata de aplicaciones web que permiten a los usuarios recibir información tan pronto como la publique un autor, en lugar de requerir que el usuario (o software) Compruebe un origen periódicamente para comprobar si hay actualizaciones. Algunos ATR de ejemplo incluyen aplicaciones de mensajería instantánea o salones de chat, juegos multijugador en línea que se pueden reproducir en el explorador, documentos de colaboración en línea, almacenamiento de la comunidad, aplicaciones de videoconferencia, etc.
- **Aplicaciones de streaming de datos**: Se trata de aplicaciones (o servicios) que envían datos o contenido a medida que llegan (o se crean) mientras mantiene la conexión abierta para seguir descargando datos, contenido o componentes según sea necesario. Algunos ejemplos incluyen aplicaciones de streaming de audio y vídeo.
- **API de REST**: Se trata de interfaces que proporcionan datos para que la aplicación Web de otra persona interactúe con. Por ejemplo, un servicio de API de calendario podría proporcionar fechas y horas para una ubicación de concierto que podría ser usada por el sitio web de eventos locales de otro usuario.
- **Aplicaciones representadas en el servidor (SSRs)** : Estas aplicaciones web se pueden ejecutar en el cliente (en el explorador o en el front-end) y en el servidor (el back-end), lo que permite que se muestren las páginas que son dinámicas (generar HTML para) el contenido que se conoce y que capta rápidamente el contenido que no se conoce como disponible. A menudo se conocen como aplicaciones "isomórficos" o "universales". SSRs usa métodos de SPA en que no es necesario volver a cargarlos cada vez que se usa. Sin embargo, SRAs ofrece algunas ventajas que pueden o no ser importantes para usted, como hacer que el contenido de su sitio aparezca en los resultados de Google Search y proporcionar una imagen de vista previa cuando los vínculos a la aplicación se comparten en un medio social como Twitter o Facebook. El posible inconveniente es que requieren un servidor node. js que se ejecute constantemente. En términos de ejemplos, una aplicación de redes sociales que admita eventos que los usuarios quiera que aparezcan en los resultados de la búsqueda y en los medios sociales puede beneficiarse de SSR, mientras que una aplicación de correo electrónico puede ser buena como SPA. También puede ejecutar aplicaciones no de SPA representadas por el servidor, que son algo parecido a un blog de WordPress. Como puede ver, las cosas pueden resultar complicadas, solo tiene que decidir lo que es importante.
- **Herramientas de línea de comandos**: Esto le permite automatizar tareas repetitivas y, a continuación, distribuir la herramienta en todo el ecosistema node. js. Un ejemplo de una herramienta de línea de comandos es Rizo, que es la dirección URL del cliente y se usa para descargar contenido de una dirección URL de Internet. Rizo se usa a menudo para instalar elementos como node. js o, en nuestro caso, un administrador de versiones de node. js.
- **Programación de hardware**: Aunque no es tan popular como las aplicaciones Web, node. js aumenta la popularidad de los usos de IoT, como la recopilación de datos de sensores, balizas, transmisores, motores o cualquier elemento que genere grandes cantidades de datos. Node. js puede habilitar la recopilación de datos, el análisis de los datos, la comunicación entre un dispositivo y un servidor, y la realización de acciones basadas en el análisis. NPM contiene más de 80 paquetes para controladores Arduino, Raspberry PI, Intel IoT Edison, varios sensores y dispositivos Bluetooth.

## <a name="try-using-nodejs-in-vs-code"></a>Pruebe a usar node. js en VS Code

1. Abra el terminal Ubuntu y cree un nuevo directorio: `mkdir HelloNode` y, a continuación, escriba el directorio: `cd HelloNode`

2. Cree un archivo JavaScript vacío denominado "app. js": `touch app.js`

3. Abra el directorio y el archivo vacío en VS Code:: `code .`

4. Cree una variable de cadena simple en App. js y envíe el contenido de la cadena a la consola; para ello, escríbala en el archivo ' App. js ':

    ```js
    var msg = 'Hello World';
    console.log(msg);
    ```

5. Para ejecutar el archivo "app. js" con node. js. Abra el terminal Ubuntu justo dentro de VS Code seleccionando **Ver** **terminal**  >  (o presione Ctrl + ' con el carácter de acento grave). Si necesita cambiar el terminal predeterminado, seleccione el menú desplegable y elija **seleccionar shell predeterminado**. A continuación, seleccione **WSL** como el shell de Terminal Server predeterminado para usar con vs Code.

6. En el terminal, escriba: `node app.js`. Debería ver la salida: "Hola mundo".

> [!NOTE]
> Tenga en cuenta que, dado que ya ha instalado la extensión WSL remota, el directorio se abrirá en un entorno remoto que se ejecuta en el sistema Ubuntu Linux tal y como se indica en la pestaña verde en la parte inferior izquierda de la ventana de VS Code. Además, tenga en cuenta que al escribir `console` en el archivo ' App. js ', VS Code muestra las opciones admitidas relacionadas con el objeto [`console`](https://developer.mozilla.org/docs/Web/API/Console) para que elija usar IntelliSense. Pruebe a experimentar con IntelliSense mediante otros [objetos de JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

> [!TIP]
> Considere la posibilidad de probar el nuevo [terminal de Windows](https://github.com/microsoft/terminal/blob/master/doc/user-docs/index.md) si tiene previsto usar varias líneas de comandos (Ubuntu, PowerShell, símbolo del sistema de Windows, etc.) o si desea [personalizar el terminal](https://github.com/microsoft/terminal/blob/master/doc/user-docs/UsingJsonSettings.md), incluidos el texto, los colores de fondo, los enlaces de teclado, etc.

## <a name="set-up-a-basic-web-app-framework-by-using-express"></a>Configuración de un marco de aplicación web básico mediante Express

Express es un marco de trabajo de node. js mínimo, flexible y simplificado que facilita el desarrollo de una aplicación web que puede administrar varios tipos de solicitudes, como GET, PUT, POST y DELETE. Express incluye un generador de aplicaciones que creará automáticamente una arquitectura de archivo para la aplicación.

Para crear un proyecto con Express. js:

1. Abra el terminal WSL (Ubuntu 18,04).
2. Cree una nueva carpeta de proyecto: `mkdir ExpressProjects` y escriba ese directorio: `cd ExpressProjects`.
3. Use Express para crear una plantilla de proyecto HelloWorld: `npx express-generator HelloWorld --view=pug`.

>[!NOTE]
> Vamos a usar el comando `npx` aquí para ejecutar el paquete de nodo Express. js sin instalarlo realmente (o bien instalarlo temporalmente en función de cómo desee imaginarlo). Si intenta usar el comando `express` o comprobar la versión de Express instalada mediante: `express --version`, recibirá una respuesta de que no se puede encontrar Express. Si desea instalar Express de forma global para usar una y otra vez, use: `npm install -g express-generator`. Puede ver una lista de los paquetes instalados por NPM mediante `npm list`. Se mostrarán por profundidad (el número de directorios anidados en profundidad). Los paquetes que instaló estarán en profundidad 0. Las dependencias de ese paquete estarán en profundidad 1, más dependencias en profundidad 2, etc. Para obtener más información, consulte [diferencia entre NPX y NPM](https://stackoverflow.com/questions/50605219/difference-between-npx-and-npm) en stackoverflow.

4. Examine los archivos y las carpetas que se incluyen al abrir el proyecto en VS Code con: `code .`

   Los archivos que genera Express crearán una aplicación web que usa una arquitectura que puede parecer un poco abrumador al principio. Verá en la ventana del **Explorador** de vs Code (Ctrl + Mayús + E para ver) que se han generado los siguientes archivos y carpetas:

   - `bin` Contiene el archivo ejecutable que inicia la aplicación. Se activa un servidor (en el puerto 3000 si no se proporciona ninguna alternativa) y se configura el control de errores básico. 
   - `public` Contiene todos los archivos a los que se accede públicamente, incluidos archivos JavaScript, hojas de estilo CSS, archivos de fuente, imágenes y cualquier otro recurso que necesiten los usuarios cuando se conecten a su sitio Web.
   - `routes` Contiene todos los controladores de ruta de la aplicación. Dos archivos, `index.js` y `users.js`, se generan automáticamente en esta carpeta para servir como ejemplos de cómo separar la configuración de la ruta de la aplicación.
   - `views` Contiene los archivos utilizados por el motor de plantillas. Express está configurado para buscar una vista coincidente cuando se llama al método Render. El motor de plantillas predeterminado es Jade, pero Jade está en desuso en favor de Pug, por lo que hemos usado la marca `--view` para cambiar el motor de vista (plantilla). Puede ver las opciones de marca `--view`, y otras, mediante el uso de `express --help`.
   - `app.js` Punto de inicio de la aplicación. Carga todo y comienza a dar servicio a las solicitudes de los usuarios. Es básicamente el pegamento que contiene todas las partes.
   - `package.json` Contiene la descripción del proyecto, el administrador de scripts y el manifiesto de la aplicación. Su finalidad principal es realizar un seguimiento de las dependencias de la aplicación y de sus versiones respectivas.

5. Ahora debe instalar las dependencias que usa Express para compilar y ejecutar la aplicación HelloWorld Express (los paquetes usados para tareas como la ejecución del servidor, tal como se define en el archivo `package.json`). En VS Code, abra el terminal de WSL seleccionando **Ver** **terminal**  >  (o seleccione Ctrl + ' con el carácter de acento grave), asegúrese de que todavía está en el directorio del proyecto ' HelloWorld '. Instale las dependencias de paquetes Express con:

```bash
npm install
```

6. En este momento, tiene el marco de trabajo configurado para una aplicación Web de varias páginas que tiene acceso a una gran variedad de API y métodos de utilidad HTTP y middleware, lo que facilita la creación de una API sólida. Inicie la aplicación Express en un servidor virtual; para ello, escriba:

```bash
DEBUG=HelloWorld:* npm start
```

> [!TIP]
> La parte `DEBUG=myapp:*` del comando anterior significa que está indicando a node. js que desea activar el registro con fines de depuración. No olvide reemplazar "MyApp" por el nombre de la aplicación. Puede encontrar el nombre de la aplicación en el archivo package. JSON en la propiedad "Name". El comando `npm start` está indicando a NPM que ejecute los scripts en el archivo package. JSON.

7. Ahora puede ver la aplicación en ejecución; para ello, abra un explorador Web y vaya a: **localhost: 3000**

   ![Captura de pantalla de la aplicación Express en ejecución en un explorador](../images/express-app.png)

8. Ahora que la aplicación HelloWorld Express se ejecuta localmente en el explorador, intente realizar un cambio; para ello, abra la carpeta "views" en el directorio del proyecto y seleccione el archivo "index. Pug". Una vez abierto, cambie `h1= title` a `h1= "Hello World!"` y seleccione **Guardar** (Ctrl + S). Para ver el cambio, actualice la dirección URL de **localhost: 3000** en el explorador Web.

9. Para detener la ejecución de la aplicación Express, en el terminal, escriba: **Ctrl + C**

## <a name="try-using-a-nodejs-module"></a>Pruebe a usar un módulo de node. js

Node. js tiene herramientas que le ayudan a desarrollar aplicaciones web del lado servidor, algunas integradas y muchas más disponibles a través de NPM. Estos módulos pueden ayudar con muchas tareas:

|Herramienta               |Se usa para                                                                                                  |
|:----------------- |:---------------------------------------------------------------------------------------------------------|
|GM, Sharp          |Manipulación de imágenes, incluida la edición, el cambio de tamaño, la compresión, etc., directamente en el código de JavaScript |
|PDFKit             |Generación de PDF                                                                                            |
|validador. js       |Validación de cadenas                                                                                         |
|imagemin, UglifyJS2|Minificación                                                                                              |
|spritesmith        |Generación de hojas de Sprite                                                                                   |
|Winston            |Registro                                                                                                  |
|comandante. js       |Crear aplicaciones de línea de comandos                                                                       |

Vamos a usar el módulo de SO integrado para obtener información sobre el sistema operativo del equipo:

1) En el terminal WSL (Ubuntu 18,04), abra la CLI de node. js. Verá el mensaje `>` que le permite saber que está usando node. js después de escribir: `node`

2) Para identificar el sistema operativo que está usando actualmente (que debe devolver una respuesta que le permita saber que está en Windows), escriba: `os.platform()`

3) Para comprobar la arquitectura de la CPU, escriba: `os.arch()`

4) Para ver las CPU disponibles en el sistema, escriba: `os.cpus()`

5) Deje la CLI de node. js escribiendo `.exit` o seleccionando Ctrl + C dos veces.

   > [!TIP]
   > Puede usar el módulo de sistema operativo node. js para realizar acciones como comprobar la plataforma y devolver una variable específica de la plataforma: Win32/. bat para el desarrollo de Windows, Darwin/. sh para Mac/UNIX, Linux, SunOS, etc. (por ejemplo, `var isWin = process.platform === "win32";`).

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido algunas cosas básicas sobre lo que puede hacer con node. js, ha intentado usar la línea de comandos de node. js en VS Code, ha creado una aplicación web sencilla con Express. js y la ha ejecutado localmente en el explorador Web y después ha intentado usar algunos de los módulos node. js integrados. Para obtener más información sobre cómo instalar y usar algunos marcos Web de node. js populares, continúe con la siguiente guía, en la que se trata el siguiente. js (un marco web presentado por el servidor basado en React), Nuxt. js (un marco web presentado por el servidor basado en Vue) y Gatsby (a marco web representado de forma estática basada en React). También puede ir directamente a aprendizaje sobre cómo trabajar con bases de datos de MongoDB o PostgreSQL o con contenedores de Docker.

- [Introducción a los marcos Web de node. js en Windows](./web-frameworks.md)
- [Introducción a la conexión de aplicaciones de node. js a una base de datos](./databases.md)
- [Introducción al uso de contenedores de Docker con node. js](./containers.md)
