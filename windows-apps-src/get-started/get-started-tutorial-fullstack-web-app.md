---
author: libbymc
title: "Tutorial de introducción: aplicación web de una sola página con back-end de API de REST"
description: "Usa las tecnologías web populares para crear una aplicación web hospedada para la Tienda Windows"
keywords: "hosted web app, aplicación web hospedada, HWA, HWA, REST API, API de REST, single-page app, aplicación de una sola página, SPA, SPA"
ms.author: libbymc
ms.date: 05/10/2017
ms.topic: article
ms.prod: Microsoft Edge, Azure, Visual Studio Code
ms.technology: web
ms.openlocfilehash: 9dd2232f17e31a92f8cbf6fbffeebc0c21a798c5
ms.sourcegitcommit: 6b772d2a224f8a9c557dc517c6ec0592545e9a43
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/02/2017
---
# <a name="get-started-tutorial---single-page-web-app-with-rest-api-backend"></a>Tutorial de introducción: aplicación web de una sola página con back-end de API de REST

**Crear una aplicación web hospedada para la Tienda Windows con tecnologías web populares de apilamiento completo**

![Juego de memoria sencillo como aplicación web de una sola página](images/fullstack.png)

En este tutorial de dos partes se ofrece paseo rápido por el desarrollo web de apilamiento completo moderno mientras creas un juego de memoria sencillo que funciona en el explorador y como aplicación web hospedada para la Tienda Windows. En la parte I vas a crear un servicio sencillo de API de REST para el back-end del juego. Al hospedar la lógica del juego en la nube como servicio de API, se conserva el estado del juego y el usuario puede seguir jugando en la misma instancia de juego en diferentes dispositivos. En la parte II crearás la interfaz de usuario de front-end como aplicación web de una sola página con diseño dinámico.

Usaremos algunas de las tecnologías web más populares, incluido el tiempo de ejecución [Node.js](https://nodejs.org/en/) y [Express](http://expressjs.com/) para el desarrollo del lado servidor, el marco de trabajo de la interfaz de usuario [Bootstrap](http://getbootstrap.com/), el motor de plantillas [Pug](https://www.npmjs.com/package/pug) y [Swagger](http://swagger.io/tools/) para la creación de las API de RESTful. También podrás ganar experiencia en [Azure Portal](https://ms.portal.azure.com/) para el hospedaje y trabajo en la nube con el editor [Visual Studio Code](https://code.visualstudio.com/).

## <a name="prerequisites"></a>Requisitos previos

Si todavía no tienes estos recursos en la máquina, sigue estos vínculos de descarga:

 - [Node.js](https://nodejs.org/en/download/): asegúrate de seleccionar la opción para agregar Node a PATH.

 - [Generador Express](http://expressjs.com/en/starter/generator.html): después de instalar Node, instala Express mediante la ejecución de `npm install express-generator -g`

 - [Visual Studio Code](https://code.visualstudio.com/)

Si quieres completar los pasos finales del hospedaje del servicio de API y la aplicación de juego de memoria en Microsoft Azure, tendrás que [crear una cuenta gratuita de Azure](https://azure.microsoft.com/en-us/free/) si todavía no lo has hecho.

Si decides dejar (o posponer) la parte de Azure, simplemente omite las secciones finales de las partes I y II, que abarcan el hospedaje y el empaquetado de la aplicación en Azure para la Tienda Windows. El servicio de API y la aplicación web que crees se podrán ejecutar localmente (desde `http://localhost:8000` y `http://localhost:3000`, respectivamente) en tu máquina.

## <a name="part-i-build-a-rest-api-backend"></a>Parte I: crear un back-end de API de REST

Primero, crearemos una API del juego de memoria sencillo para iniciar nuestra aplicación web de juego de memoria. Usaremos [Swagger](http://swagger.io/) para definir nuestra API y generar código scaffolding y una interfaz de usuario web para la prueba manual.

Si quieres omitir esta parte e ir directamente a [Parte II: crear una aplicación web de una sola página](#part-ii-build-a-single-page-web-application), aquí tienes el [código terminado para la parte I](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend). Sigue las instrucciones de *LÉAME* para que el código funcione de forma local o consulta *5. Hospedar el servicio de API en Azure y habilitar CORS* para ejecutarlo desde Azure.

### <a name="game-overview"></a>Introducción al juego

*Memoria* (también llamado [*Memorama*](https://en.wikipedia.org/wiki/Concentration_(game)) y [*Pelmanism*](https://en.wikipedia.org/wiki/Pelmanism_(system))), es un juego sencillo que consta de una baraja de pares de cartas. Las cartas se colocan boca abajo en la mesa y el jugador mira los valores de las cartas, dos cada vez, buscando coincidencias. Después de cada turno, las tarjetas se vuelven a colocar boca abajo, a menos que se encuentre un par coincidente, en cuyo caso se retiran las dos cartas del juego. El objetivo del juego es encontrar todos los pares de cartas en la menor cantidad de turnos.

Para fines informativos, la estructura del juego que usaremos será muy sencilla: es un solo juego, un solo jugador. Sin embargo, la lógica del juego está del lado servidor (en lugar del lado cliente) para conservar el estado del juego, por lo que podrías seguir jugando el mismo juego en diferentes dispositivos.

La estructura de los datos para un juego de memoria consiste simplemente en una matriz de objetos JavaScript; cada uno de ellos representa una sola carta y los índices de la matriz actúan como identificadores de la carta. En el servidor, cada objeto carta tiene un valor y una marca **cleared**. Por ejemplo, un tablero de 2 coincidencias (4 cartas) se podría generar aleatoriamente y serializar de la siguiente manera.

```json
[
    { "cleared":"false",
        "value":"0",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"1",
    },
    { "cleared":"false",
        "value":"0",
    }
]
```

Cuando la matriz del tablero se pasa al cliente, se quitan las claves de valor de la matriz para evitar trampas (por ejemplo, inspeccionar el cuerpo de respuestas HTTP con las herramientas de F12 del explorador). Así vería el mismo juego un cliente que llamara al punto de conexión **GET /game** de REST:

```json
[{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"},{ "cleared":"false"}]
```

Hablando de puntos de conexión, el servicio del juego de memoria constará de tres API de REST.

#### <a name="post-new"></a>POST /new
Inicializa un tablero de juego nuevo del tamaño especificado (número de coincidencias).

| Parámetro | Descripción |
|-----------|-------------|
| int *size* |Número de pares coincidentes que se van a mezclar en el "tablero" del juego. Por ejemplo: `http://memorygameapisample/new?size=2`|

| Respuesta | Descripción |
|----------|-------------|
| 200 correctos | El nuevo juego de memoria del tamaño solicitado está listo.|
| 400 SOLICITUD INCORRECTA| El tamaño solicitado está fuera del intervalo aceptable.|


#### <a name="get-game"></a>GET /game
Recupera el estado actual del tablero del juego de memoria.

*Sin parámetros*

| Respuesta | Descripción |
|----------|-------------|
| 200 correctos | Devuelve la matriz JSON de objetos carta. Cada carta tiene una propiedad **cleared** que indica si ya se ha encontrado su pareja. Las cartas emparejadas también informan de su propiedad **value**. Por ejemplo: `[{"cleared":"false"},{"cleared":"false"},{"cleared":"true","value":1},{"cleared":"true","value":1}]`|

#### <a name="put-guess"></a>PUT /guess
Especifica una carta que se va a mostrar y busca una coincidencia con la carta mostrada anteriormente.

| Parámetro | Descripción |
|-----------|-------------|
| int *card* | El id. de la carta (índice de la matriz del tablero del juego) de la carta que se va a mostrar. Cada "guess" que se completa consta de dos cartas especificadas (es decir, dos llamadas a **/guess** con valores de *card* válidos y exclusivos). Por ejemplo: `http://memorygameapisample/guess?card=0`|

| Respuesta | Descripción |
|----------|-------------|
| 200 correctos | Devuelve JSON con los elementos **id** y **value** de la carta especificada. Por ejemplo: `[{"id":0,"value":1}]`|
| 400 SOLICITUD INCORRECTA |  Error de la carta especificada. Consulta el cuerpo de la respuesta HTTP para obtener más detalles.|

### <a name="1-spec-out-the-api-and-generate-code-stubs"></a>1. Especificar la API y generar códigos auxiliares

Vamos a usar [Swagger](http://swagger.io/) para transformar el diseño de la API del juego de memoria en código de servidor Node.js que funcione. Así es cómo puedes definir nuestras [API del juego de memoria como metadatos Swagger](https://github.com/Microsoft/Windows-tutorials-web/blob/master/Single-Page-App-with-REST-API/backend/api.json). Lo usaremos para generar código auxiliar de servidor.

1. Crea una nueva carpeta (en el *GitHub* local, por ejemplo) y descarga el archivo [**api.json**](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/api.json?token=ACEfklXAHTeLkHYaI5plV20QCGuqC31cks5ZFhVIwA%3D%3D) que contiene nuestras definiciones de API del juego de memoria. Asegúrate de que el nombre de la carpeta no contiene ningún espacio.

2. Abre tu shell favorito ([o usa el terminal integrado de Visual Studio Code](https://code.visualstudio.com/docs/editor/integrated-terminal)) en esa carpeta y ejecuta el siguiente comando del administrador de paquetes de nodos (NPM) para instalar la herramienta de código de scaffolding [Yeoman](http://yeoman.io/) (yo) y el generador Swagger para el entorno de nodo global (**-g**):

    ```
    npm install -g yo
    npm install -g generator-swaggerize
    ```

3. Ahora ya podemos generar el código scaffolding del servidor con Swagger:

    ```
    yo swaggerize
    ```

4. El comando **swaggerize** te hará varias preguntas.
    - Ruta de acceso (o dirección URL) al documento swagger: **api.json**
    - Marco de trabajo: **express**
    - Cómo te gustaría llamar a este proyecto (YourFolderNameHere): **[enter]**

    Responde a todo lo demás como quieras; la información es principalmente para proporcionar al archivo *package.json* los datos de contacto para que puedas distribuir el código como paquete NPM.

5. Por último, instala todas las dependencias (enumeradas en *package.json*) para el nuevo proyecto y la compatibilidad con [Swagger UI](http://swagger.io/swagger-ui/).

    ```
    npm install
    npm install swaggerize-ui
    ```

    Ahora inicia VS Code, selecciona **Archivo** > **Abrir carpeta...** y ve al directorio MemoryGameAPI. Este es el servidor de la API de Node.js que acabas de crear. Usa el popular marco de trabajo de aplicación web [ExpressJS](http://expressjs.com/en/4x/api.html) para estructurar y ejecutar el proyecto.

### <a name="2-customize-the-server-code-and-setup-debugging"></a>2. Personalizar el código de servidor y configurar la depuración

El archivo *server.js* de la raíz del proyecto actúa como la función "principal" de tu servidor. Ábrelo en VS Code y copia lo siguiente en él. Las líneas modificadas del código generado contienen comentarios con más explicaciones.

```javascript
'use strict';

var port = process.env.PORT || 8000; // Better flexibility than hardcoding the port

var Http = require('http');
var Express = require('express');
var BodyParser = require('body-parser');
var Swaggerize = require('swaggerize-express');
var SwaggerUi = require('swaggerize-ui'); // Provides UI for testing our API
var Path = require('path');

var App = Express();
var Server = Http.createServer(App);

App.use(function(req, res, next) {  // Enable cross origin resource sharing (for app frontend)
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,OPTIONS');
    res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization, Content-Length, X-Requested-With');

    // Prevents CORS preflight request (for PUT game_guess) from redirecting
    if ('OPTIONS' == req.method) {
      res.send(200);
    }
    else {
      next(); // Passes control to next (Swagger) handler
    }
});

App.use(BodyParser.json());
App.use(BodyParser.urlencoded({
    extended: true
}));

App.use(Swaggerize({
    api: Path.resolve('./config/swagger.json'),
    handlers: Path.resolve('./handlers'),
    docspath: '/swagger'   //  Hooks up the testing UI
}));

App.use('/', SwaggerUi({    // Serves the testing UI from our base URL
  docs: '/swagger'          //
}));

Server.listen(port, function () {  // Starts server with our modfied port settings
 });
```

Con esto, ha llegado el momento de ejecutar el servidor. Vamos a configurar Visual Studio Code para la depuración de Node mientras estamos en él. Selecciona el icono del panel **Depurar** (Ctrl + Mayús + D) y, a continuación, el icono de engranaje (abre launch.json) y modifica las "configurations" de la siguiente manera:

```json
"configurations": [
    {
        "type": "node",
        "request": "launch",
        "name": "Launch Program",
        "program": "${workspaceRoot}/server.js"
    }
]
```

Ahora presiona F5 y abre el explorador en [http://localhost:8000](http://localhost:8000). La página debe abrirse en la interfaz de usuario Swagger para nuestra API del juego de memoria y desde allí puedes expandir los detalles y los campos de entrada para cada uno de los métodos. Incluso puedes intentar llamar a las API, aunque sus respuestas contendrán solamente datos ficticios (proporcionados por el módulo [Swagmock](https://www.npmjs.com/package/swagmock)). Es el momento de agregar la lógica del juego para que estas API sean reales.

### <a name="3-set-up-your-route-handlers"></a>3. Configurar los controladores de rutas

El archivo de Swagger (config\swagger.json) da instrucciones a nuestro servidor sobre cómo controlar las distintas solicitudes HTTP de cliente asignando cada ruta de acceso de dirección URL que se define a un archivo de controlador (en \handlers) y cada método definido para esa ruta de acceso (por ejemplo, **GET**, **POST**) a un elemento `operationId` (función) dentro de ese archivo de controlador.

En este nivel de nuestro programa agregaremos alguna comprobación de la entrada antes de pasar las distintas solicitudes de cliente a nuestro modelo de datos. Descarga (o copia y pega):

 - El código de [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/game.js?token=ACEfkvhw6BUnkeSsZgnzVe086T5WLwjfks5ZFhW5wA%3D%3D) en tu archivo **handlers\game.js**
 - El código de [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/guess.js?token=ACEfkswel02rHVr0e61bVsNdpv_i1Rtuks5ZFhXPwA%3D%3D) en tu archivo **handlers\guess.js**
 - El código de [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/handlers/new.js?token=ACEfkgk2QXJeRc0aaLzY5ulFsAR4Juidks5ZFhXawA%3D%3D) en tu archivo **handlers\new.js**

 Puedes leer los comentarios contenidos en estos archivos para más información sobre los cambios, pero en esencia comprueban si hay errores de entrada básicos (por ejemplo, el cliente solicita un nuevo juego con menos de una coincidencia) y envían mensajes descriptivos del error si es necesario. Los controladores también enrutan las solicitudes de cliente válidas a través de los archivos de datos correspondientes (en \data) para realizar un mayor procesamiento. A continuación, vamos a trabajar en ellos.

### <a name="4-set-up-your-data-model"></a>4. Configurar el modelo de datos

Es el momento de intercambiar el servicio de datos ficticios del marcador de posición por un modelo de datos reales de nuestro tablero del juego de memoria.

En este nivel de nuestro programa se representan las cartas de memoria y se proporciona la mayor parte de la lógica del juego, incluida la acción de "mezclar" la baraja para un juego nuevo, la identificación de parejas de cartas coincidentes y el seguimiento del estado del juego. Copia y pega:

 - El código de [game.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/game.js?token=ACEfksAceJNQmhF82aHjQTx78jILYKfCks5ZFhX4wA%3D%3D) en tu archivo **data\game.js**
 - El código de [guess.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/guess.js?token=ACEfkvY69Zr1AZQ4iXgfCgDxeinT21bBks5ZFhYBwA%3D%3D) en tu archivo **data\guess.js**
 - El código de [new.js](https://raw.githubusercontent.com/Microsoft/Windows-tutorials-web/master/Single-Page-App-with-REST-API/backend/data/new.js?token=ACEfkiqeDN0HjZ4-gIKRh3wfVZPSlEmgks5ZFhYPwA%3D%3D) en tu archivo **data\new.js**

Por cuestiones de simplicidad, almacenamos nuestro tablero del juego en una variable global (`global.board`) en nuestro servidor Node. Pero en realidad deberías usar almacenamiento en la nube (como Google [Cloud Datastore](https://cloud.google.com/datastore/) o Azure [DocumentDB](https://azure.microsoft.com/en-us/services/documentdb/)) para convertirlo en un servicio de API de juego de memoria viable que admita simultáneamente varios juegos y jugadores.

Asegúrate de haber guardado todos los cambios en VS Code, vuelve a iniciar el servidor (F5 en VS Code o `npm start` desde el shell y, a continuación, ves hasta [http://localhost:8000](http://localhost:8000)) para probar la API del juego.

Cada vez que pulses el botón **Pruébalo** en una de las operaciones **/game**, **/guess** o **/new**, comprueba el **Cuerpo de la respuesta** resultante y el **Código de respuesta** más abajo, para comprobar que todo funciona como se espera.

Intenta: 

1. Crear un nuevo juego `size=2`.

    ![Iniciar un nuevo juego de memoria desde la interfaz de usuario Swagger](images\swagger_new.png)

2. Adivinar un par de valores.

    ![Adivinar una carta desde la interfaz de usuario Swagger](images\swagger_guess.png)

3. Comprobar el tablero del juego a medida que avanza el juego.

    ![Comprobar el estado del juego en la interfaz de usuario Swagger](images\swagger_game.png)

Si todo parece correcto, tu servicio de API está listo para hospedarse en Azure. Si te encuentras con problemas, intenta comentar las siguientes líneas en \data\game.js.

```javascript
for (var i=0; i < board.length; i++){
    var card = {};
    card.cleared = board[i].cleared;

    // Only reveal cleared card values
    //if ("true" == card.cleared) {        // To debug the board, comment out this line
        card.value = board[i].value;
    //}                                    // And this line
    currentBoardState.push(card);
}
```

Con este cambio, el método **GET /game** devolverá todos los valores de carta (incluidos los que todavía no se han borrado). Es una modificación de depuración útil que se debe mantener al crear el front-end para el juego de memoria.

### <a name="5-optional-host-your-api-service-on-azure-and-enable-cors"></a>5. (Opcional) Hospedar el servicio de API en Azure y habilitar CORS

Los documentos de Azure te guiarán en los procedimientos siguientes:

 - [Registro de una nueva *Aplicación de API* en el Azure Portal](https://docs.microsoft.com/en-us/azure/app-service-api/app-service-api-nodejs-api-app#a-idcreateapiappa-create-a-new-api-app)
 - [Configuración de la aplicación de API para la implementación de Git](https://docs.microsoft.com/en-us/azure/app-service-api/app-service-api-nodejs-api-app#set-up-your-new-api-app-for-git-deployment) e
 - [Implementación de código de aplicación de API en Azure](https://docs.microsoft.com/en-us/azure/app-service-api/app-service-api-nodejs-api-app#deploy-your-api-code-to-azure)

Al registrar la aplicación, intenta diferenciar tu *Nombre de la aplicación* (para evitar conflictos de nomenclatura con otras personas que soliciten variaciones la dirección URL *http://memorygameapi.azurewebsites.net*).

Si has llegado hasta aquí y Azure te sirve ahora la interfaz de usuario Swagger, queda solo un paso final para el back-end del juego de memoria. Desde [Azure Portal](https://portal.azure.com), selecciona tu *App Service* recién creado y, a continuación, selecciona o busca la opción **CORS** (uso compartido de recursos entre orígenes). En **Orígenes permitidos**, agrega un asterisco (`*`) y haz clic en **Guardar**. Esto te permite realizar llamadas entre orígenes a tu servicio de API desde el front-end del juego de memoria mientras lo desarrollas en tu máquina local. Después de finalizar el front-end del juego de memoria e implementarlo en Azure, puedes reemplazar esta entrada por la dirección URL específica de la aplicación web.

### <a name="going-further"></a>Ir más allá

Para que la API del juego de memoria sea un servicio de back-end viable para una aplicación de producción, puedes extender el código para admitir varios jugadores y juegos. Para esto, probablemente tendrás que asociar [autenticación](http://swagger.io/docs/specification/authentication/) (para la administración de identidades de jugadores), una [base de datos NoSQL](https://docs.microsoft.com/en-us/azure/documentdb/) (para el seguimiento de los jugadores y los juegos) y algunas [pruebas unitarias](https://apigee.com/about/blog/developer/swagger-test-templates-test-your-apis) básicas para la API.

Estos son algunos recursos útiles para ir más allá:

 - [Depuración avanzada de Node.js con Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-debugging)

 - [Documentos web y móvil de Azure](https://docs.microsoft.com/en-us/azure/#pivot=services&panel=web)

 - [Documentos Azure DocumentDB](https://docs.microsoft.com/en-us/azure/documentdb/index)

## <a name="part-ii-build-a-single-page-web-appl"></a>Parte II: crear una aplicación web de una sola página

Ahora que has creado (o [descargado](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/backend)) el [back-end de la API de REST](#part-i-build-a-rest-api-backend) de la parte I, estás listo para crear el front-end del juego de memoria de una sola página con [Node](https://nodejs.org/en/), [Express](http://expressjs.com/) y [Bootstrap](http://getbootstrap.com/).

En la parte II de este tutorial adquirirás experiencia con: 

* [Node.js](https://nodejs.org/en/): para crear el servidor que hospeda el juego
* [jQuery](http://jquery.com/): una biblioteca JavaScript
* [Express](http://expressjs.com/): para el marco de trabajo de la aplicación web
* [Pug](https://pugjs.org/): (formalmente Jade) para el motor de plantillas
* [Bootstrap](http://getbootstrap.com/): para el diseño dinámico
* [Visual Studio Code](https://code.visualstudio.com/): para la creación, markdown, visualización y depuración del código

### <a name="1-create-a-nodejs-application-by-using-express"></a>1. Crear una aplicación de Node.js mediante Express

Empecemos por crear el proyecto de Node.js con Express.

1. Abre un símbolo del sistema y navega hasta el directorio donde quieres almacenar el juego. 
2. Usa el generador Express para crear una nueva aplicación llamada *memory* usando el motor de plantillas, *Pug*.

    ```
    express --view=pug memory
    ```

3. En el directorio memory, instala las dependencias enumeradas en el archivo package.json. El archivo package.json se crea en la raíz del proyecto. Este archivo contiene los módulos que son necesarios para tu aplicación de Node.js.  

    ```
    cd memory
    npm install
    ```

    Después de ejecutar este comando, verás una carpeta denominada node_modules que contiene todos los módulos necesarios para esta aplicación. 

4. Ahora, ejecuta la aplicación.

    ```
    npm start
    ```

5. Para ver la aplicación, ve a [http://localhost:3000/](http://localhost:3000/).

    ![Captura de pantalla de http://localhost:3000/](./images/express.png)

6. Cambia el título predeterminado de la aplicación web editando index.js en el directorio memory\routes. Cambia `Express` en la línea de abajo a `Memory Game` (u otro título de tu elección).

    ``` javascript
    res.render('index', { title: 'Express' });
    ```

7. Para actualizar la aplicación y ver el título nuevo, detén la aplicación presionando **Ctrl + C**, **Y** en el símbolo del sistema y, a continuación, reinicia con `npm start`.

### <a name="2-add-client-side-game-logic-code"></a>2. Agregar el código de la lógica del juego del lado cliente
Puedes encontrar los archivos que necesitas para esta mitad del tutorial en la carpeta [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start). Si te pierdes, el código final está disponible en la carpeta [Final](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Final). 

1. Copia el archivo scripts.js dentro de la carpeta [Start](https://github.com/Microsoft/Windows-tutorials-web/tree/master/Single-Page-App-with-REST-API/frontend/Start) y pégalo en memory\public\javascripts. Este archivo contiene toda la lógica del juego necesaria para ejecutar el juego, entre lo que se incluye:

    * Creación o inicio de un nuevo juego.
    * Restauración de un juego almacenado en el servidor.
    * Dibujo del tablero del juego y las cartas en función de la selección del usuario.
    * Volteo de las cartas.

2. En script.js, empieza por modificar la función `newGame()`. Esta función:

    * Controla la selección que el usuario hace del tamaño del juego.
    * Captura la [matriz del tablero del juego](#part-i-build-a-rest-api-backend) del servidor.
    * Llama a la función `drawGameBoard()` para colocar el tablero del juego en la pantalla.

    Agrega el siguiente código dentro de `newGame()` después del comentario `// Add code from Part 2.2 here`.

    ``` javascript
    // extract game size selection from user
    var size = $("#selectGameSize").val();

    // parse game size value
    size = parseInt(size, 10);
    ```

    Este código recupera el valor del menú desplegable con `id="selectGameSize"` (que se creará más adelante) y lo almacena en una variable (`size`).  Usamos la función [`parseInt()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt) para analizar el valor de la cadena en la lista desplegable y devolver un entero, para que podamos pasar el elemento `size` del tablero del juego solicitado al servidor. 

    Usamos el método [`/new`](#part-i-build-a-rest-api-backend) creado en la parte I de este tutorial para publicar el tamaño elegido del tablero del juego en el servidor. El método devuelve una única matriz JSON de cartas y valores `true/false` que indican si se han emparejado las cartas. 

3. A continuación, rellena la función `restoreGame()` que restaura el último juego que se ha jugado. Para simplificar, la aplicación siempre carga el último juego. Si no hay ningún juego almacenado en el servidor, usa el menú desplegable para iniciar un nuevo juego. 

Copia y pega este código en `restoreGame()`.

    ``` javascript 
    // reset the game
    gameBoardSize = 0;
    cardsFlipped = 0;

    // fetch the game state from the server 
    $.get("http://localhost:8000/game", function (response) {
        // store game board size
        gameBoardSize = response.length;

        // draw the game board
        drawGameBoard(response);
    });
    ```

    The game will now fetch the game state from the server. For more information about the [`/game`](#part-i-build-a-rest-api-backend) method being used in this step, see Part I of this tutorial. If you are using Azure (or another service) to host the backend API, replace the *localhost* address above with your production URL.

4. Ahora crearemos la función `drawGameBoard()`.  Esta función:

    * Detecta el tamaño del juego y aplica estilos CSS específicos.
    * Genera las cartas en HTML.
    * Agrega las cartas a la página.

    Copia y pega este código en la función `drawGameBoard()` en *scripts.js*:

    ``` javascript
    // create output
    var output = "";
    // detect board size CSS class
    var css = "";
    switch (board.length / 4) {
        case 1:
            css = "rows1";
            break;
        case 2:
            css = "rows2";
            break;
        case 3:
            css = "rows3";
            break;
        case 4:
            css = "rows4";
            break;   
    }
    // generate HTML for each card and append to the output
    for (var i = 0; i < board.length; i++) {
        if (board[i].cleared == "true") {
            // if the card has been cleared apply the .flip class
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards flip matched\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\">" + lookUpGlyphicon(board[i].value) + "</div>\
                </div></div>";
        } else {
            output += "<div class=\"flipContainer col-xs-3 " + css + "\"><div class=\"cards\" id=\"" + i + "\" onClick=\"flipCard(this)\">\
                <div class=\"front\"><span class=\"glyphicon glyphicon-question-sign\"></span></div>\
                <div class=\"back\"></div>\
                </div></div>";
        }
    }
    // place the output on the page
    $("#game-board").html(output);
    ```

5. Después, tenemos que completar la función `flipCard()`.  Esta función controla la mayoría de la lógica del juego, incluida la obtención de los valores de las cartas seleccionadas desde el servidor con el método [`/guess`](#part-i-build-a-rest-api-backend) desarrollado en la parte I del tutorial. No te olvides de reemplazar la dirección *localhost* por la dirección URL de producción si hospedas en la nube el back-end de la API de REST.

    En la función `flipCard()`, quita la marca de comentario de este código:

    ``` javascript
    // post this guess to the server and get this card's value
    $.ajax({
        url: "http://localhost:8000/guess?card=" + selectedCards[0],
        type: 'PUT',
        success: function (response) {
            // display first card value
            $("#" + selectedCards[0] + " .back").html(lookUpGlyphicon(response[0].value));

            // store the first card value
            selectedCardsValues.push(response[0].value);
        }
    });
    ```

> [!TIP] 
> Si usas Visual Studio Code, selecciona todas las líneas de código de las que quieras quitar las marcas de comentario y presiona Ctrl + K, U

Aquí usamos [`jQuery.ajax()`](http://api.jquery.com/jQuery.ajax/) y el método **PUT** [`/guess`](#part-i-build-a-rest-api-backend) creado en la parte I. 

Este código se ejecuta en el siguiente orden.

* El elemento `id` de la primera carta que el usuario selecciona se agrega como primer valor a la matriz selectedCards[]: `selectedCards[0]` 
* El valor (`id`) de `selectedCards[0]` se publica en el servidor con el método [`/guess`](#part-i-build-a-rest-api-backend)
* El servidor responde con el elemento `value` de la carta (un entero)
* Se añade un [icono de glifo de Bootstrap](http://getbootstrap.com/components/) a la parte posterior de la carta cuyo elemento `id` es `selectedCards[0]`
* El elemento `value` de la primera carta (del servidor) se almacena en la matriz `selectedCardsValues[]`: `selectedCardsValues[0]`. 

El segundo intento del usuario sigue la misma lógica. Si las cartas que seleccionó el usuario tienen los mismos identificadores, (por ejemplo, `selectedCards[0] == selectedCards[1]`), las tarjetas concuerdan. La clase CSS `.matched` se agrega a las cartas emparejadas (que se vuelven de color verde) y las cartas que quedan se voltean.

Ahora, tenemos que agregar lógica para comprobar si el usuario ha emparejado todas las cartas y ha ganado el juego. Dentro de la función `flipCard()`, agrega las siguientes líneas de código en el comentario `//check if the user won the game`. 

``` javascript
if (cardsFlipped == gameBoardSize) {
    setTimeout(function () {
        output = "<div id=\"playAgain\"><p>You Win!</p><input type=\"button\" onClick=\"location.reload()\" value=\"Play Again\" class=\"btn\" /></div>";
        $("#game-board").html(output);
    }, 1000);
}   
```

Si el número de cartas volteadas coincide con el tamaño del tablero del juego (por ejemplo, `cardsFlipped == gameBoardSize`), no hay más cartas que se puedan voltear y el usuario ha ganado el juego. Vamos a agregar algo de HTML sencillo al elemento `div` con `id="game-board"` para que el usuario sepa que ha ganado y que puede volver a jugar.  

### <a name="3-create-the-user-interface"></a>3. Crear la interfaz de usuario 
Ahora vamos a ver todo este código en acción mediante la creación de la interfaz de usuario. En este tutorial, usamos el motor de plantillas [Pug](https://pugjs.org/) (formalmente Jade).  La sintaxis de *Pug* es limpia y distingue los espacios en blanco para escribir HTML. Aquí tienes un ejemplo. 

```
body
    h1 Memory Game
    #container
        p We love tutorials!
```

se convierte en

``` html
<body>
    <h1>Memory Game</h1>
    <div id="container">
        <p>We love tutorials!</p>
    </div>
</body>
```


1. Reemplaza el archivo layout.pug en memory\views por el archivo layout.pug proporcionado en la carpeta Start. Dentro de layout.pug, verás vínculos a:

    * Bootstrap
    * jQuery
    * Un archivo CSS personalizado
    * El archivo JavaScript que acabamos de modificar

2. Abre el archivo llamado index.pug en el directorio memory\views.
Este archivo extiende el archivo layout.pug y representará nuestro juego. Dentro de layout.pug, pega las siguientes líneas de código:

    ```
    extends layout
    block content  
        div
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board
                script restoreGame();
    ```

> [!TIP] 
> Recuerda: Pug distingue los espacios en blanco. Asegúrate de que todas tus sangrías sean correctas.

### <a name="4-use-bootstraps-grid-system-to-create-a-responsive-layout"></a>4. Usar el sistema de cuadrículas de Bootstrap para crear un diseño dinámico
El [sistema de cuadrículas](http://getbootstrap.com/css/#grid) de Bootstrap es un sistema de cuadrículas que escala una cuadrícula cuando cambia la ventanilla de un dispositivo. Las cartas de este juego usan clases del sistema de cuadrículas predefinidas de Bootstrap para el diseño, entre las que se encuentran:
* `.container-fluid`: especifica el contenedor fluido de la cuadrícula
* `.row-fluid`: especifica las filas fluidas
* `.col-xs-3`: especifica el número de columnas

El sistema de cuadrículas de Bootstrap permite que un sistema de cuadrículas se contraiga en una columna vertical, como se vería en un menú de navegación de un dispositivo móvil.  Sin embargo, ya que queremos que nuestro juego siempre tenga columnas, usamos la clase predefinida `.col-xs-3`, que conserva la cuadrícula horizontal en todo momento. 

El sistema de cuadrículas permite hasta 12 columnas. Dado que queremos solo 4 columnas en nuestro juego, usaremos la clase `.col-xs-3`. Esta clase especifica que necesitamos cada una de nuestras columnas para ocupar todo el ancho de 3 de las 12 columnas disponibles antes mencionadas. En esta imagen se muestra una cuadrícula de 12 columnas y una cuadrícula de 4 columnas, como la que se usa en este juego.

![Cuadrícula de Bootstrap con 12 columnas y 4 columnas](./images/grid.png)

1. Abre scripts.js y busca la función `drawGameBoard()`.  En el bloque de código donde se genera el HTML para cada carta, ¿puedes detectar el elemento `div` con `class="col-xs-3"`? 

2. Dentro de index.pug, vamos a agregar las clases de Bootstrap predefinidas mencionadas anteriormente para crear nuestro diseño fluido.  Cambia index.pug por lo siguiente.

    ```
    extends layout

    block content   

        .container-fluid
            form(method="GET")
            select(id="selectGameSize" class="form-control" onchange="newGame()")
                option(value="0") New Game
                option(value="2") 2 Matches
                option(value="4") 4 Matches
                option(value="6") 6 Matches
                option(value="8") 8 Matches         
            #game-board.row-fluid 
                script restoreGame();
    ```

### <a name="5-add-a-card-flip-animation-with-css-transforms"></a>5. Agregar una animación de volteo de carta con transformaciones CSS
Reemplaza el archivo style.css en memory\public\stylesheets por el archivo style.css de la carpeta Start.

La adición de un movimiento de volteo con [transformaciones CSS](https://docs.microsoft.com/en-us/microsoft-edge/dev-guide/css/transforms) proporciona a las cartas un movimiento de volteo 3D realista. Las cartas del juego se crean con la siguiente estructura HTML y se agregan mediante programación al tablero del juego (en la función `drawGameBoard()` mostrada anteriormente).

``` html
<div class="flipContainer">
    <div class="cards">
        <div class="front"></div>
        <div class="back"></div>
    </div>
</div>
```

1. Para empezar, proporciona [perspectiva](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) al contenedor principal de la animación (`.flipContainer`).  Esto crea la ilusión de profundidad para sus elementos secundarios: cuanto mayor sea el valor, más alejado el usuario aparecerá el elemento. Vamos a agregar la siguiente perspectiva a la clase `.flipContainer` en style.css.

    ``` css
    perspective: 1000px; 
    ```

2. Ahora, agrega las siguientes propiedades para la clase `.cards` en style.css. `.cards` `div` es el elemento que realmente realizará la animación de volteo y mostrará la parte frontal o la posterior de la carta. 

    ``` css
    transform-style: preserve-3d;
    transition-duration: 1s;
    ```

    La propiedad [`transform-style`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform-style) establece un contexto de representación 3D y los elementos secundarios de la clase `.cards` (`.front` y `.back`) son miembros del espacio 3D. La adición de la propiedad [`transition-duration`](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-duration) especifica el número de segundos que la animación tardará en finalizar. 

3.  Con la propiedad [`transform`](https://developer.mozilla.org/en-US/docs/Web/CSS/transform), podemos girar la carta alrededor del eje Y.  Agrega la siguiente CSS a `cards.flip`.

    ``` css
    transform: rotateY(180deg);
    ```

    El estilo definido en `cards.flip` se activa y desactiva en la función `flipCard` mediante [`.toggleClass()`](http://api.jquery.com/toggleClass/). 

    ``` javascript
    $(card).toggleClass("flip");
    ```

    Ahora cuando un usuario hace clic en una carta, esta se gira 180 grados.

### <a name="6-test-and-play"></a>6. Probar y jugar
¡Enhorabuena! Has terminado de crear la aplicación web. Vamos a probarla. 

1. Abre un símbolo del sistema en el directorio memory y escribe el siguiente comando: `npm start`

2. En el explorador, ve a [http://localhost:3000/](http://localhost:3000/) y juega.

3. Si se producen errores, puedes usar las herramientas de depuración de Node.js de Visual Studio Code, presionando F5 en el teclado y escribiendo `Node.js`. Para más información sobre la depuración en Visual Studio Code, echa un vistazo a este [artículo](http://code.visualstudio.com/docs/editor/debugging#_launch-configurations). 

    También puedes comparar tu código con el código proporcionado en la carpeta Final.

4. Para detener el juego, en el símbolo del sistema, escribe: **Ctrl + C**, **Y**. 

### <a name="going-further"></a>Ir más allá

Ahora puedes implementar la aplicación en Azure (o en cualquier otro servicio de hospedaje en la nube) para probar diferentes factores de forma de dispositivo, como móvil, tableta y escritorio. (No olvides probar en diferentes exploradores también) Una vez que la aplicación está lista para producción, puedes empaquetarla fácilmente como *Aplicación web hospedada* (HWA) para la *Plataforma universal de Windows* (UWP) y distribuirla desde la Tienda Windows.

Los pasos básicos para publicarla en la Tienda Windows son:

 1. Crear una cuenta de [desarrollador de Windows](https://developer.microsoft.com/en-us/store/register).
 2. Usar la [lista de comprobación de envío de aplicación](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions).
 3. Enviar la aplicación para su [certificación](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process).

Estos son algunos recursos útiles para ir más allá:

 - [Implementar el proyecto de desarrollo de aplicaciones en Azure Websites](https://docs.microsoft.com/en-us/azure/documentdb/documentdb-nodejs-application#a-nametoc395783182aPart-7-deploy-your-application-development-project-to-azure-websites)

 - [Convertir tu aplicación web en una aplicación para la Plataforma universal de Windows (UWP)](https://docs.microsoft.com/en-us/windows/uwp/porting/hwa-create-windows)

 - [Publicar aplicaciones de Windows](https://developer.microsoft.com/en-us/store/publish-apps)