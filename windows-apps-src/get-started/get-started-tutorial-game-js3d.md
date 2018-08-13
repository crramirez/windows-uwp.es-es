---
title: 'Tutorial de introducción: un juego para UWP en 3D en JavaScript'
description: Un juego para UWP para la Tienda Windows escrito en JavaScript con three.js
author: abbycar
ms.author: abigailc
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fb4249b2-f93c-4993-9e4d-57a62c04be66
ms.openlocfilehash: bb72e7787764fd549891651df47794dfe1948247
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: es-ES
ms.locfileid: "240364"
---
# <a name="creating-a-3d-javascript-game-using-threejs"></a>Crear un juego en 3D en JavaScript con three.js

## <a name="introduction"></a>Introducción

Para los desarrolladores web o los apasionados de JavaScript, desarrollar aplicaciones para UWP con JavaScript es una manera sencilla de ofrecer aplicaciones globalmente. No es necesario preocuparse por aprender un lenguaje como C# o C++.

Para esta muestra, vamos a aprovechar la biblioteca de **three.js**. Esta biblioteca se basa en WebGL, una API que se usa para representar gráficos 2D y 3D para exploradores web. **three.js** toma esta API complicada y la simplifica, facilitando el desarrollo en 3D. 


¿Quieres hacerte una idea de la aplicación que vamos a crear antes de seguir leyendo? Échale un vistazo en CodePen.

<p data-height="300" data-theme-id="23761" data-slug-hash="NpKejy" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Dino game final" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/NpKejy/">Dino game final</a> (Juego de dinosaurio) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

> [!NOTE] 
> Este no es un juego completo; sino que está diseñado para mostrar como usar JavaScript y una biblioteca de terceros para preparar una aplicación lista para su publicación en la Tienda Windows.


## <a name="requirements"></a>Requisitos

Para jugar con este proyecto, necesitarás lo siguiente:
-    Un equipo de Windows (o una máquina virtual) que ejecute la versión actual de Windows 10.
-    Una copia de Visual Studio. Puedes descargar la copia gratuita de Visual Studio Community Edition desde la [página principal de Visual Studio](http://visualstudio.com/).
Este proyecto hace uso de la biblioteca de JavaScript **three.js**. **three.js** se lanza bajo la licencia MIT. Esta biblioteca ya está presente en el proyecto (busca `js/libs` en la vista del explorador de soluciones). Para obtener más información acerca de esta biblioteca consulta la página principal de [**three.js**](https://threejs.org/).

## <a name="getting-started"></a>Introducción

El código fuente completo de la aplicación se almacena en [GitHub](https://github.com/Microsoft/Windows-appsample-get-started-js3d).

La forma más sencilla de empezar es visitar GitHub, haz clic en el botón verde Clonar o descargar y selecciona Abrir en Visual Studio. 

![botón de clonar o descargar](images/3dclone.png)

Si no quieres clonar el proyecto, puedes descargarlo como un archivo ZIP.
Una vez que se ha cargado la solución en Visual Studio, verás varios archivos, incluidos:
-    Images/: carpeta que contiene diferentes iconos que requieren las aplicaciones para UWP.
- css/: carpeta que contiene el CSS que se va a usar.
-    js/: carpeta que contiene archivos JavaScript. El archivo main.js es nuestro juego, mientras que los demás archivos son las bibliotecas de terceros.
-    models/: carpeta que contiene modelos 3D. En este juego, solo tenemos uno para el dinosaurio.
-    index.html: página web que hospeda al representador del juego.

Ya puedes ejecutar el juego.

Presiona F5 para iniciar la aplicación. Deberías ver una ventana abierta que te solicitará hacer clic en la pantalla. También verás un dinosaurio moviéndose en segundo plano. Cierra el juego y comenzaremos a examinar la aplicación y sus componentes clave.

> [!NOTE] 
> ¿Hubo algún problema? Asegúrate de haber instalado Visual Studio con el soporte web. Puedes crear un proyecto nuevo para comprobarlo; si no existe compatibilidad con JavaScript, tendrás que volver a instalar Visual Studio y marcar el cuadro Microsoft Web Developer Tools.

## <a name="walkthrough"></a>Tutorial

Cuando inicies este juego, verás un mensaje que te solicita hacer clic en la pantalla. La [API de bloqueo de puntero]([Pointer Lock API](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock)) sirve para que puedas moverte con el mouse. El movimiento se realiza presionando W, A, S, D o las teclas de dirección.
El objetivo de este juego es mantenerse lejos del dinosaurio. Si el dinosaurio está lo suficientemente cerca de ti, empezará a perseguirte hasta que salgas de su rango o te acerques demasiado y pierdas.

### <a name="1-setting-up-your-initial-html-file"></a>1. Configurar el archivo HTML inicial

En **index.html**, deberás agregar un pequeño HTML para comenzar. Este archivo es la página web predeterminada que contiene nuestra aplicación.

Ahora, lo configuraremos con las bibliotecas que usaremos y el `div` (denominado `container`) que se usará para renderizar nuestros gráficos. También lo configuraremos para que apunte a nuestro **main.js** (nuestro código de juego).


```html
<!DOCTYPE html>
<html lang='en'>

<head>
    <link rel="stylesheet" type="text/css" href="css/stylesheet.css" />
</head>

    <body>
        <div id='container'></div>
        <script src='js/libs/three.js'></script>
        <script src="js/controls/PointerLockControls.js"></script>
        <script src="js/main.js"></script>
    </body>

</html>
```


Ahora que tenemos el HTML de inicio listo para ejecutarse, vayamos a **main.js** y creemos algunos gráficos.

### <a name="2-creating-your-scene"></a>2. Crear el escenario

En esta sección del tutorial, vamos a agregar la base del juego.

Empezaremos por desarrollar un `scene`. Un `scene` en **three.js** es donde se agregarán la cámara, los objetos y las luces. También necesitarás a un representador que tomará lo que ve la cámara en la escena para mostrarlo.

En **main.js** crearemos una función denominada `init()` que se encarga de todo esto y llama a algunas funciones adicionales:

```javascript
var UNITWIDTH = 90; // Width of a cubes in the maze
var UNITHEIGHT = 45; // Height of the cubes in the maze

var camera, scene, renderer;

init();
animate();

function init() {
    // Create the scene where everything will go
    scene = new THREE.Scene();

    // Add some fog for effects
    scene.fog = new THREE.FogExp2(0xcccccc, 0.0015);

    // Set render settings
    renderer = new THREE.WebGLRenderer();
    renderer.setClearColor(scene.fog.color);
    renderer.setPixelRatio(window.devicePixelRatio);
    renderer.setSize(window.innerWidth, window.innerHeight);

    // Get the HTML container and connect renderer to it
    var container = document.getElementById('container');
    container.appendChild(renderer.domElement);

    // Set camera position and view details
    camera = new THREE.PerspectiveCamera(60, window.innerWidth / window.innerHeight, 1, 2000);
    camera.position.y = 20; // Height the camera will be looking from
    camera.position.x = 0;
    camera.position.z = 0;

    // Add the camera
    scene.add(camera);

    // Add the walls(cubes) of the maze
    createMazeCubes();

    // Add lights to the scene
    addLights();

    // Listen for if the window changes sizes and adjust
    window.addEventListener('resize', onWindowResize, false);
}

```

Entre el resto de funciones que debemos crear se incluyen:
- `createMazeCubes()`
- `addLights()`
- `onWindowResize()`
- `animate()` / `render()`
- Funciones de conversión de unidades

#### <a name="createmazecubes"></a>createMazeCubes()

La función `createMazeCubes()` agregará un cubo sencillo al escenario. Más adelante haremos que la función agregue más cubos para crear nuestro laberinto.

```javascript
function createMazeCubes() {

  // Make the shape of the cube that is UNITWIDTH wide/deep, and UNITHEIGHT tall
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  // Make the material of the cube and set it to blue
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });
  
  // Combine the geometry and material to make the cube
  var cube = new THREE.Mesh(cubeGeo, cubeMat);

  // Add the cube to the scene
  scene.add(cube);

  // Update the cube's position
  cube.position.y = UNITHEIGHT / 2;
  cube.position.x = 0;
  cube.position.z = -100;
  // rotate the cube by 30 degrees
  cube.rotation.y = degreesToRadians(30);
}

```

#### <a name="addlights"></a>addLights()

La función `addLights()` es una función sencilla que agrupa la creación de nuestras luces y las agrega al escenario.

```javascript
function addLights() {
  var lightOne = new THREE.DirectionalLight(0xffffff);
  lightOne.position.set(1, 1, 1);
  scene.add(lightOne);

  // Add a second light with half the intensity
  var lightTwo = new THREE.DirectionalLight(0xffffff, .5);
  lightTwo.position.set(1, -1, -1);
  scene.add(lightTwo);
}
```

#### <a name="onwindowresize"></a>onWindowResize()

La función `onWindowResize` se llama siempre que nuestro detector de eventos escucha que se desencadenó un evento `resize`. Esto sucede cuando el usuario ajusta el tamaño de la ventana. Si esto ocurre, queremos asegurar que la imagen permanece proporcional y puede verse en la ventana completa.

```javascript
function onWindowResize() {

  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();

  renderer.setSize(window.innerWidth, window.innerHeight);
}
```

#### <a name="animate"></a>animate()

Lo último que necesitaremos es la función `animate()`, que también llamará a la función `render()`. La función [`requestAnimationFrame()`](https://developer.mozilla.org/docs/Web/API/window/requestAnimationFrame) se usa para actualizar constantemente nuestro representador. Más tarde, usaremos estas funciones para actualizar nuestro representador con animaciones fantásticas como moverte por el laberinto.

```javascript
function animate() {
    render();
    // Keep updating the renderer
    requestAnimationFrame(animate);
}

function render() {
    renderer.render(scene, camera);
}
```

#### <a name="unit-conversion-functions"></a>Funciones de conversión de unidades

En **three.js**, las rotaciones se miden en radianes. Para facilitar la cosas, agregaremos algunas funciones para que podemos convertir fácilmente entre grados y radianes. 


```javascript
function degreesToRadians(degrees) {
  return degrees * Math.PI / 180;
}

function radiansToDegrees(radians) {
  return radians * 180 / Math.PI;
}
```

¿Quién va a recordar que 30 grados son 0,523 radianes? Es mucho más sencillo hacer `degreesToRadians(30)` para obtener la cantidad de rotación que se usa en nuestra función `createMazeCubes()`.

___

Este fue un fragmento de código considerable, pero ahora tenemos un hermoso cubo renderizado en nuestro `container`. Echa un vistazo a los resultados en CodePen.

Puedes copiar y pegar todo el JavaScript de este CodePen para saber si hay algún problema, o modificarlo para ajustar algunas luces y cambiar algunos colores. 

<p data-height="300" data-theme-id="23761" data-slug-hash="648faf11da72fb302b1396ec14e19cfe" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Cube and player camera" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/648faf11da72fb302b1396ec14e19cfe/">Cube and player camera</a> (Cubo y cámara del jugador) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


### <a name="3-making-the-maze"></a>3. Crear el laberinto

Aunque observar un cubo es impresionante, espera a ver un laberinto creado con cubos. Es un secreto muy conocido en la comunidad de juegos que una de las formas más rápidas de crear un nivel es colocar cubos con una matriz 2D.
 
![laberinto creado con una matriz 2D](images/dinomap.png)

Colocar un 1 donde están los cubos y un 0 donde hay espacio vacío es una forma sencilla y manual de crear y modificar el laberinto.

Esto se consigue al reemplazar la antigua función `createMazeCubes()` con una que usa un bucle anidado para crear y colocar varios cubos. También crearemos un nombre de matriz `collidableObjects` y le agregaremos los cubos para detectar colisiones más adelante en este tutorial:

```javascript
var totalCubesWide; // How many cubes wide the maze will be
var collidableObjects = []; // An array of collidable objects used later

function createMazeCubes() {
  // Maze wall mapping, assuming even square
  // 1's are cubes, 0's are empty space
  var map = [
    [0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, ],
    [0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 1, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 1, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 1, 0, 0, 0, 0, 0, ],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ],
    [1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ],
    [0, 0, 1, 1, 1, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, ]
  ];

  // wall details
  var cubeGeo = new THREE.BoxGeometry(UNITWIDTH, UNITHEIGHT, UNITWIDTH);
  var cubeMat = new THREE.MeshPhongMaterial({
    color: 0x81cfe0,
  });

  // Keep cubes within boundry walls
  var widthOffset = UNITWIDTH / 2;
  // Put the bottom of the cube at y = 0
  var heightOffset = UNITHEIGHT / 2;
  
  // See how wide the map is by seeing how long the first array is
  totalCubesWide = map[0].length;

  // Place walls where 1`s are
  for (var i = 0; i < totalCubesWide; i++) {
    for (var j = 0; j < map[i].length; j++) {
      // If a 1 is found, add a cube at the corresponding position
      if (map[i][j]) {
        // Make the cube
        var cube = new THREE.Mesh(cubeGeo, cubeMat);
        // Set the cube position
        cube.position.z = (i - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        cube.position.y = heightOffset;
        cube.position.x = (j - totalCubesWide / 2) * UNITWIDTH + widthOffset;
        // Add the cube
        scene.add(cube);
        // Used later for collision detection
        collidableObjects.push(cube);
      }
    }
  }
    // The size of the maze will be how many cubes wide the array is * the width of a cube
    mapSize = totalCubesWide * UNITWIDTH;
}

```

Ahora que sabemos cuántos cubos se están usando (y su tamaño), podemos usar la variable `mapSize` calculada para configurar las dimensiones del plano de tierra:

```javascript
var mapSize;    // The width/depth of the maze

function createGround() {
    // Create ground geometry and material
    var groundGeo = new THREE.PlaneGeometry(mapSize, mapSize);
    var groundMat = new THREE.MeshPhongMaterial({ color: 0xA0522D, side: THREE.DoubleSide});

    var ground = new THREE.Mesh(groundGeo, groundMat);
    ground.position.set(0, 1, 0);
    // Rotate the place to to ground level
    ground.rotation.x = degreesToRadians(90);
    scene.add(ground);
}
```

La última parte del laberinto serán los muros del perímetro para encuadrar todo. Usaremos un bucle para crear dos planos (nuestros muros) a la vez, con la variable `mapSize` que calculamos en `createGround()` para determinar el ancho. También se agregarán las paredes nuevas a nuestra matriz `collidableObjects` para detectar colisiones en el futuro:

```javascript
function createPerimWalls() {
    var halfMap = mapSize / 2;  // Half the size of the map
    var sign = 1;               // Used to make an amount positive or negative

    // Loop through twice, making two perimeter walls at a time
    for (var i = 0; i < 2; i++) {
        var perimGeo = new THREE.PlaneGeometry(mapSize, UNITHEIGHT);
        // Make the material double sided
        var perimMat = new THREE.MeshPhongMaterial({ color: 0x464646, side: THREE.DoubleSide });
        // Make two walls
        var perimWallLR = new THREE.Mesh(perimGeo, perimMat);
        var perimWallFB = new THREE.Mesh(perimGeo, perimMat);

        // Create left/right wall
        perimWallLR.position.set(halfMap * sign, UNITHEIGHT / 2, 0);
        perimWallLR.rotation.y = degreesToRadians(90);
        scene.add(perimWallLR);
        // Used later for collision detection
        collidableObjects.push(perimWallLR);
        // Create front/back wall
        perimWallFB.position.set(0, UNITHEIGHT / 2, halfMap * sign);
        scene.add(perimWallFB);

        // Used later for collision detection
        collidableObjects.push(perimWallFB);

        sign = -1; // Swap to negative value
    }
}
```


No olvides agregar una llamada a `createGround()` y `createPerimWalls` después de `createMazeCubes()` en la función `init()` para que se compilen.
___

Ya podemos observar un hermoso laberinto, pero no podemos hacernos una idea de su magnitud porque la cámara está atascada en un punto. Ya es hora de pasar al siguiente nivel en este juego y agregar controles de cámara.

No dudes en probar cosas en el CodePen, como cambiar los colores de los cubos o quitar el suelo comentando `createGround()` en la función `init()`.


<p data-height="300" data-theme-id="23761" data-slug-hash="b3d668e78b6c8e1a5130d3276ecb054f" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Maze building" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/b3d668e78b6c8e1a5130d3276ecb054f/">Maze building</a> (Crear un laberinto) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### <a name="4-allowing-the-player-to-look-around"></a>4. Permitir que el jugador mire a su alrededor

Es hora de incluir el laberinto y empezar a echar un vistazo. Para ello, usaremos la biblioteca **PointerLockControls.js** y la cámara.

La biblioteca **PoinerLockControls.js** usa el mouse para rotar la cámara en la dirección en la que se mueve el mismo, lo que permite que el jugador mire a su alrededor. 

Primero, vamos a agregar algunos elementos nuevos en nuestro archivo **index.html**:

```html
<div id="blocker">
    <div id="instructions">
    <strong>Click to look!</strong>
    </div>
</div>

<script src="main.js"></script>
```

También necesitarás todo el CSS del CodePen en la parte inferior de esta sección. Debería pegarse en el archivo **stylesheet.css**.

Volviendo a **main.js**, agrega algunas nuevas variables globales; `controls` para almacenar el controlador, `controlsEnabled` para realizar un seguimiento del estado del controlador y `blocker` para seleccionar el elemento `blocker` en **index.html**:

```javascript
var controls;
var controlsEnabled = false;

// HTML elements to be changed
var blocker = document.getElementById('blocker');
```


Ahora en nuestra función `init()` podemos generar un nuevo objeto `PoinerLockControls`, pasarlo a `camera` y agregar `camera` (se accede con `controls.getObject()`).

```javascript
controls = new THREE.PointerLockControls(camera);
scene.add(controls.getObject());
```

La cámara ya está conectada, pero debemos dejar que el mouse y el controlador interactúen para poder mirar alrededor. 

Para esta situación, la [API de bloqueo de puntero](https://docs.microsoft.com/microsoft-edge/dev-guide/dom/pointer-lock) acude al rescate y nos permite conectar los movimientos del mouse a la cámara. La API de bloqueo de puntero también hace que el mouse desaparezca para ofrecer una experiencia más envolvente. Al presionar la tecla ESC, finalizamos la conexión de la cámara con el mouse y este último vuelve a aparecer. Las adiciones de las funciones `getPointerLock()` y `lockChange()` nos ayudarán a hacer esto.

La función `getPointerLock()` escucha cuando se produce un clic con el mouse. Después del clic, nuestro juego representado (en el elemento `container`) intenta obtener el control del mouse. También agregamos un detector de eventos para detectar cuando el jugador activa o desactiva el bloqueo que luego llama a `lockChange()`. 

```javascript
function getPointerLock() {
  document.onclick = function () {
    container.requestPointerLock();
  }
  document.addEventListener('pointerlockchange', lockChange, false); 
}

```

Nuestra función `lockChange()` necesita deshabilitar o habilitar los controles y el elemento `blocker`. Podemos determinar el estado de bloqueo de puntero, comprobando si el destino de la propiedad [`pointerLockElement`](https://developer.mozilla.org/docs/Web/API/Document/pointerLockElement) para eventos de mouse está configurada en `container`.

```javascript
function lockChange() {
    // Turn on controls
    if (document.pointerLockElement === container) {
        // Hide blocker and instructions
        blocker.style.display = "none";
        controls.enabled = true;
    // Turn off the controls
    } else {
      // Display the blocker and instruction
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

Ahora podemos agregar una llamada a `getPointerLock()` justo antes de la función `init()`.
```javascript
// Get the pointer lock state
getPointerLock();
init();
animate();
```

---

En este momento, tenemos la capacidad de **mirar** alrededor, pero el factor sorprendente será poder **moverse**. Las cosas se van a poner un poco matemáticas con los vectores, ¿pero qué son los gráficos en 3D sin un poco de matemáticas?

<p data-height="300" data-theme-id="23761" data-slug-hash="7672409f7218b18e13adb370fd2cf61d" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Look around" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/7672409f7218b18e13adb370fd2cf61d/">Look around</a> (Mirar alrededor) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


### <a name="5-adding-player-movement"></a>5. Agregar movimiento al jugador

Para profundizar en cómo dar movimiento al jugador, tenemos que volver a nuestros días de cálculo. Queremos aplicar velocidad (movimiento) a la `camera` a lo largo de un determinado vector (dirección).

Vamos a agregar algunas variables globales más para realizar un seguimiento de la dirección en la que se mueve el jugador y configurar un vector de velocidad inicial:

```javascript
// Flags to determine which direction the player is moving
var moveForward = false;
var moveBackward = false;
var moveLeft = false;
var moveRight = false;

// Velocity vector for the player
var playerVelocity = new THREE.Vector3();

// How fast the player will move
var PLAYERSPEED = 800.0;

var clock;
```

Al principio de la función `init()`, configura `clock` a un nuevo objeto `Clock`. Usaremos esto para realizar un seguimiento de la variación en tiempo (delta) que se tarda en representar fotogramas nuevos. También tendrás que agregar una llamada a `listenForPlayerMovement()`, que recopila la entrada del usuario. 

```
clock = new THREE.Clock();
listenForPlayerMovement();
```

La función `listenForPlayerMovement()` es la encargada de voltear los estados de dirección. En la parte inferior de la función, tenemos dos detectores de eventos que están esperando a que se pulsen y suelten las teclas. Una vez que uno de estos eventos se desencadena, comprobaremos si se trata de una tecla que queremos desencadenar o si queremos detener el movimiento.

En este juego, lo hemos configurado para que el jugador pueda moverse con las teclas W, A, S y D, o las teclas de dirección.

```javascript
function listenForPlayerMovement() {
    
    // A key has been pressed
    var onKeyDown = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = true;
        break;

      case 37: // left
      case 65: // a
        moveLeft = true;
        break;

      case 40: // down
      case 83: // s
        moveBackward = true;
        break;

      case 39: // right
      case 68: // d
        moveRight = true;
        break;
    }
  };

  // A key has been released
    var onKeyUp = function(event) {

    switch (event.keyCode) {

      case 38: // up
      case 87: // w
        moveForward = false;
        break;

      case 37: // left
      case 65: // a
        moveLeft = false;
        break;

      case 40: // down
      case 83: // s
        moveBackward = false;
        break;

      case 39: // right
      case 68: // d
        moveRight = false;
        break;
    }
  };

  // Add event listeners for when movement keys are pressed and released
  document.addEventListener('keydown', onKeyDown, false);
  document.addEventListener('keyup', onKeyUp, false);
}
```

Ahora que podemos determinar la dirección a la que quiere ir el usuario (que se almacena como `true` en una de las marcas globales de dirección), es hora de pasar a la acción. Esta acción se produce en forma de la función `animatePlayer()`.

Se llama a esta función desde `animate()`, pasando `delta` para obtener el cambio en el tiempo que transcurre entre fotogramas, para que el movimiento no parezca estar desincronizado durante los cambios en la velocidad de fotogramas:

```javascript
function animate() {
  render();
  requestAnimationFrame(animate);
  // Get the change in time between frames
  var delta = clock.getDelta();
  animatePlayer(delta);
}
```

¡Ahora es el momento de divertirse! El vector de impulso (`playerVeloctiy`) tiene tres parámetros `(x, y, z)`, e `y` es el impulso vertical. Como no habrá saltos en este juego, solo trabajaremos con los parámetros `x` y `z`. Este vector se establece inicialmente en (0, 0, 0).

Tal como se muestra en el siguiente código, se realizan una serie de comprobaciones para ver qué marca de dirección está en `true`. Una vez que tenemos la dirección, sumaremos o restaremos de `x` e `y` para aplicar impulso en esa dirección. Si no se presiona ninguna tecla de movimiento, el vector volverá a establecerse en `(0, 0, 0)`.


```javascript

function animatePlayer(delta) {
  // Gradual slowdown
  playerVelocity.x -= playerVelocity.x * 10.0 * delta;
  playerVelocity.z -= playerVelocity.z * 10.0 * delta;

  if (moveForward) {
    playerVelocity.z -= PLAYERSPEED * delta;
  } 
  if (moveBackward) {
    playerVelocity.z += PLAYERSPEED * delta;
  } 
  if (moveLeft) {
    playerVelocity.x -= PLAYERSPEED * delta;
  } 
  if (moveRight) {
    playerVelocity.x += PLAYERSPEED * delta;
  }
  if( !( moveForward || moveBackward || moveLeft ||moveRight)) {
    // No movement key being pressed. Stop movememnt
    playerVelocity.x = 0;
    playerVelocity.z = 0;
  }
  controls.getObject().translateX(playerVelocity.x * delta);
  controls.getObject().translateZ(playerVelocity.z * delta);
}
```

Al final, aplicamos cualquiera de los valores actualizados `x` e `y` a la cámara como traducciones para hacer que el jugador se mueva realmente.

---

¡Enhorabuena! Ya tienes una cámara controlada por el jugador que puede moverse y mirar alrededor. Todavía podemos colarnos por las paredes, pero eso lo solucionaremos más tarde. A continuación, agregaremos el dinosaurio.

<p data-height="300" data-theme-id="23761" data-slug-hash="ab804473fa3545d1153061a6078b346d" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Player movement" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/ab804473fa3545d1153061a6078b346d">Player movement</a> (Movimiento del jugador) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

> [!NOTE]
> Si usas estos controles en tu aplicación para UWP, puede que notes retrasos en el movimiento y eventos de `keyUp` sin registrar. Estamos investigando este problema y esperamos arreglar esta parte de la muestra en breves.

### <a name="6-load-that-dino"></a>6. Cargar el dinosaurio

Si clonaste o descargarte este repositorio de proyectos, verás una carpeta `models` con un archivo `dino.json` dentro. Este archivo JSON es un modelo de dinosaurio en 3D que se ha creado y exportado desde Blender.


Tendremos que agregar más variables globales para cargar este dinosaurio:

```javascript
var DINOSCALE = 20;  // How big our dino is scaled to

var clock;
var dino;
var loader = new THREE.JSONLoader();

var instructions = document.getElementById('instructions');
```

Ahora que tenemos `JSONLoader` creado, pasaremos la ruta de acceso al archivo **dino.json** y una devolución de llamada con la geometría y los materiales recopilados del archivo.
Cargar el dinosaurio es una tarea asincrónica, lo que significa que nada se representará hasta que el dinosaurio esté completamente cargado. En el **index.html** cambiamos la cadena del elemento `instructions` a `"Loading..."` para informar al jugador de que las cosas están en curso.

Una vez cargado el dinosaurio, actualizamos el elemento `instructions` con las instrucciones reales del juego y movemos la función `animate()` desde el final de `init()` al final de la devolución de llamada de la función que se muestra a continuación:

```javascript
   // load the dino JSON model and start animating once complete
    loader.load('./models/dino.json', function (geometry, materials) {


        // Get the geometry and materials from the JSON
        var dinoObject = new THREE.Mesh(geometry, new THREE.MultiMaterial(materials));

        // Scale the size of the dino
        dinoObject.scale.set(DINOSCALE, DINOSCALE, DINOSCALE);
        dinoObject.rotation.y = degreesToRadians(-90);
        dinoObject.position.set(30, 0, -400);
        dinoObject.name = "dino";
        scene.add(dinoObject);
        
        // Store the dino
        dino = scene.getObjectByName("dino"); 

        // Model is loaded, switch from "Loading..." to instruction text
        instructions.innerHTML = "<strong>Click to Play!</strong> </br></br> W,A,S,D or arrow keys = move </br>Mouse = look around";

        // Call the animate function so that animation begins after the model is loaded
        animate();
    });
```

---

Ya tenemos el modelo de dinosaurio cargado. Échale un vistazo.

<p data-height="300" data-theme-id="23761" data-slug-hash="a90ba279ace9773635870d47c80400c4" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Adding the dino" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/a90ba279ace9773635870d47c80400c4/">Adding the dino</a> (Agregar el dinosaurio) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### <a name="7-move-that-dino"></a>7. Mover el dinosaurio

Crear inteligencia artificial para un juego puede ser extremadamente complejo, así que para este ejemplo haremos que el dinosaurio tenga un comportamiento de movimiento sencillo. Nuestra dinosaurio se moverá en línea recta, deslizándose a través de las paredes y a través de la niebla lejana.

Para ello, primero agrega la variable global `dinoVelocity`.

```javascript
var DINOSPEED = 400.0;

var dinoVelocity = new THREE.Vector3();
```
 A continuación, llama a la función `animateDino()` desde la función `animation()` y agrega el código siguiente:

```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;

    dinoVelocity.z += DINOSPEED * delta;
    // Move the dino
    dino.translateZ(dinoVelocity.z * delta);
}
```
---

Ver al dinosaurio moverse por ahí no es muy divertido, pero cuando agreguemos la detección de colisión todo será más divertido.

<p data-height="300" data-theme-id="23761" data-slug-hash="65245a3abd2232ec0dbbfa153f309e7d" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Moving the dino - no collision" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/65245a3abd2232ec0dbbfa153f309e7d/">Moving the dino - no collision</a> (Mover el dinosaurio, sin ninguna colisión) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### <a name="8-collision-detection-for-the-player"></a>8. Detección de colisiones para el jugador

Ya tenemos al jugador y al dinosaurio en movimiento, pero todavía queda un problema: todo el mundo puede atravesar las paredes. Cuando aprendimos a agregar cubos y paredes en este tutorial, los insertamos en la matriz `collidableObjects`. Esta matriz es la que usaremos para indicar si un jugador está demasiado cerca de algo que no puede atravesar.

Usaremos raycasters para determinar cuándo se va a producir una intersección. Puedes imaginarte un raycaster como un rayo láser que sale de la cámara hacia una determinada dirección, informando de si golpea un objeto e indicando la distancia a la que está.

```javascript
var PLAYERCOLLISIONDISTANCE = 20;
```

Crearemos una función nueva denominada `detectPlayerCollision()` que devolverá `true` si el jugador está demasiado cerca de un objeto que colisiona.
Para el jugador, vamos a aplicarle un raycaster, cambiando a qué dirección apunta en función de la dirección a la que se dirige.

Para hacerlo, creamos `rotationMatrix`, una matriz no definida. Al comprobar en qué dirección nos movemos, terminaremos con un `rotationMatrix` definido o, si te mueves hacia adelante, indefinido.
Si está definido, `rotationMatrix` se aplicará a la dirección de los controles. 

A continuación, se creará un raycaster, desde la cámara a la dirección de `cameraDirection`.


```javascript
function detectPlayerCollision() {
    // The rotation matrix to apply to our direction vector
    // Undefined by default to indicate ray should coming from front
    var rotationMatrix;
    // Get direction of camera
    var cameraDirection = controls.getDirection(new THREE.Vector3(0, 0, 0)).clone();

    // Check which direction we're moving (not looking)
    // Flip matrix to that direction so that we can reposition the ray
    if (moveBackward) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(180));
    }
    else if (moveLeft) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(90));
    }
    else if (moveRight) {
        rotationMatrix = new THREE.Matrix4();
        rotationMatrix.makeRotationY(degreesToRadians(270));
    }

    // Player is not moving forward, apply rotation matrix needed
    if (rotationMatrix !== undefined) {
        cameraDirection.applyMatrix4(rotationMatrix);
    }

    // Apply ray to player camera
    var rayCaster = new THREE.Raycaster(controls.getObject().position, cameraDirection);

    // If our ray hit a collidable object, return true
    if (rayIntersect(rayCaster, PLAYERCOLLISIONDISTANCE)) {
        return true;
    } else {
        return false;
    }
}
```

La función `detectPlayerCollision()` se basa en la función auxiliar `rayIntersect()`.
Esto toma un raycaster y el valor que representa la proximidad a la que podemos acercarnos a un objeto de la matriz `collidableObjects` antes de determinar que se ha producido una colisión.

```javascript
function rayIntersect(ray, distance) {
    var intersects = ray.intersectObjects(collidableObjects);
    for (var i = 0; i < intersects.length; i++) {
        // Check if there's a collision
        if (intersects[i].distance < distance) {
            return true;
        }
    }
    return false;
}
```

Ahora que podemos determinar cuándo se va producir una colisión, podemos actualizar la función `animatePlayer()`:

```javascript
function animatePlayer(delta) {
    // Gradual slowdown
    playerVelocity.x -= playerVelocity.x * 10.0 * delta;
    playerVelocity.z -= playerVelocity.z * 10.0 * delta;

    // If no collision and a movement key is being pressed, apply movement velocity
    if (detectPlayerCollision() == false) {
        if (moveForward) {
            playerVelocity.z -= PLAYERSPEED * delta;
        }
        if (moveBackward) {
            playerVelocity.z += PLAYERSPEED * delta;
        } 
        if (moveLeft) {
            playerVelocity.x -= PLAYERSPEED * delta;
        }
        if (moveRight) {
            playerVelocity.x += PLAYERSPEED * delta;
        }

        controls.getObject().translateX(playerVelocity.x * delta);
        controls.getObject().translateZ(playerVelocity.z * delta);
    } else {
        // Collision or no movement key being pressed. Stop movememnt
        playerVelocity.x = 0;
        playerVelocity.z = 0;
    }
}
```

---

Ahora ya podemos detectar las colisiones del jugador, así que ya puedes intentar atravesar las paredes.

<p data-height="300" data-theme-id="23761" data-slug-hash="106301953a2128c02283532026be9ab4" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Moving the player - collision" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/106301953a2128c02283532026be9ab4/">Moving the player - no collision</a> (Mover el jugador, sin ninguna colisión) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a></p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


### <a name="9-collision-detection-and-animation-for-dino"></a>9. Detección de colisiones y animación del dinosaurio

Ahora vamos a evitar que el dinosaurio atraviese las paredes y que se mueva hacia otra dirección aleatoria cuando esté demasiado cerca de un objeto contra el que puede colisionar.

Primero, vamos a averiguar cuando colisiona el dinosaurio. 

Necesitamos establecer otra variable global para la distancia de colisión:

```javascript
var DINOCOLLISIONDISTANCE = 55;     
```

Ahora que hemos especificado a qué distancia queremos que colisione nuestro dinosaurio, vamos a agregar una función similar a `detectPlayerCollision()`, pero un poco más sencilla.
La función `detectDinoCollision` es muy sencilla, ya que siempre tenemos un raycaster que sale directamente de la parte frontal del dinosaurio. No es necesario girarla como en el caso de la colisión del jugador.

```javascript
function detectDinoCollision() {
    // Get the rotation matrix from dino
    var matrix = new THREE.Matrix4();
    matrix.extractRotation(dino.matrix);
    // Create direction vector
    var directionFront = new THREE.Vector3(0, 0, 1);

    // Get the vectors coming from the front of the dino
    directionFront.applyMatrix4(matrix);

    // Create raycaster
    var rayCasterF = new THREE.Raycaster(dino.position, directionFront);
    // If we have a front collision, we have to adjust our direction so return true
    if (rayIntersect(rayCasterF, DINOCOLLISIONDISTANCE))
        return true;
    else
        return false;
}
```

Echemos un vistazo al aspecto de nuestra función final `animateDino()` cuando se conecte con la detección de colisiones:


```javascript
function animateDino(delta) {
    // Gradual slowdown
    dinoVelocity.x -= dinoVelocity.x * 10.0 * delta;
    dinoVelocity.z -= dinoVelocity.z * 10.0 * delta;


    // If no collision, apply movement velocity
    if (detectDinoCollision() == false) {
        dinoVelocity.z += DINOSPEED * delta;
        // Move the dino
        dino.translateZ(dinoVelocity.z * delta);

    } else {
        // Collision. Adjust direction
        var directionMultiples = [-1, 1, 2];
        var randomIndex = getRandomInt(0, 2);
        var randomDirection = degreesToRadians(90 * directionMultiples[randomIndex]);

        dinoVelocity.z += DINOSPEED * delta;
        dino.rotation.y += randomDirection;
    }
}
```

Siempre queremos que nuestro dinosaurio gire -90, 90 o 180 grados. Para que esto sea más claro, creamos la matriz `directionMultiples` que producirá estos números cuando se multipliquen por 90.
Para que la selección de los grados de rotación sea aleatoria, agregamos la función auxiliar `getRandomInt()` para detectar un valor 0, 1 o 2, que represente un índice aleatorio de la matriz.

```javascript
function getRandomInt(min, max) {
    min = Math.ceil(min);
    max = Math.floor(max);
    return Math.floor(Math.random() * (max - min)) + min;
}
```

Una vez que esté todo listo, se multiplicará el índice aleatorio de la matriz por 90 para obtener los grados (convertidos en radianes) de la rotación.
Al agregar este valor a la rotación `y` del dinosaurio con `dino.rotation.y += randomDirection;`, el dinosaurio gira aleatoriamente tras colisionar.


---

¡Lo logramos! Ya tenemos un dinosaurio con inteligencia artificial que puede moverse por el laberinto.

<p data-height="300" data-theme-id="23761" data-slug-hash="dd6e3a8f7df08851034aa470fea5d208" data-default-tab="js,result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="Moving the dino - collision and animation" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/dd6e3a8f7df08851034aa470fea5d208/">Moving the dino - collision and animation</a> (Mover el dinosaurio, colisión y animación) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### <a name="10-starting-the-chase"></a>10. Iniciar la persecución

Cuando el dinosaurio está a una determinada distancia del jugador, queremos que comience a perseguirlo. Dado que esto es solo un ejemplo, no hay algoritmos avanzados aplicados para que el dinosaurio persiga al jugador. En su lugar, el dinosaurio mirará al jugador y se dirigirá hacia él. En una parte abierta del laberinto esto funcionará bien, pero el dinosaurio puede quedarse atrapado contra una pared.

En nuestra función `animate()`, agregaremos una variable booleana que viene determinada por lo que devuelve `triggerChase()`:

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();

    animateDino(delta);
    animatePlayer(delta);
}
```

La función `triggerChase` comprobará si el jugador está en el rango de persecución del dinosaurio y, después, hará que el dinosaurio siempre esté cara al jugador, lo que le permite moverse hacia la dirección del jugador. 

```javascript
function triggerChase() {
    // Check if in dino detection range of the player
    if (dino.position.distanceTo(controls.getObject().position) < 300) {
        // Set the dino target's y value to the current y value. We only care about x and z for movement.
        var lookTarget = new THREE.Vector3();
        lookTarget.copy(controls.getObject().position);
        lookTarget.y = dino.position.y;

        // Make dino face camera
        dino.lookAt(lookTarget);

        // Get distance between dino and camera with a unit offset
        // Game over when dino is the value of CATCHOFFSET units away from camera
        var distanceFrom = Math.round(dino.position.distanceTo(controls.getObject().position)) - CATCHOFFSET;
        // Alert and display distance between camera and dino
        dinoAlert.innerHTML = "Dino has spotted you! Distance from you: " + distanceFrom;
        dinoAlert.style.display = '';
        return true;
        // Not in agro range, don't start distance countdown
    } else {
        dinoAlert.style.display = 'none';
        return false;
    }
}
```

La segunda mitad de `triggerChase` aborda la visualización de texto que permite que el jugador sepa a qué distancia está el dinosaurio. También introducimos `CATCHOFFSET` para especificar a qué distancia debería estar `0`. Si no tenemos el desplazamiento, `0` debería estar justo encima del jugador, lo que no proporcionará un final muy cinematográfico para todo esto.



```javascript
var dinoAlert = document.getElementById('dino-alert');
dinoAlert.style.display = 'none';
```

---

En este punto, tenemos un dinosaurio salvaje que comienza a perseguir al jugador si se acerca demasiado y que no parará hasta que su posición se encuentre sobre la del jugador
El último paso es agregar algunas condiciones de fin del juego una vez que el dinosaurio esté a unas unidades de `CATCHOFFSET` de distancia.

<p data-height="300" data-theme-id="23761" data-slug-hash="fa75ffb13070dd4245cc152cb513509a" data-default-tab="result" data-user="MicrosoftEdgeDocumentation" data-embed-version="2" data-pen-title="The chase" data-preview="true" data-editable="true" class="codepen">Consulta el Pen <a href="https://codepen.io/MicrosoftEdgeDocumentation/pen/fa75ffb13070dd4245cc152cb513509a/">The chase</a> (La persecución) de Microsoft Edge Docs (<a href="http://codepen.io/MicrosoftEdgeDocumentation">@MicrosoftEdgeDocumentation</a>) en <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


### <a name="11-ending-the-game"></a>11. Finalización del juego


Hemos recorrido un largo partiendo de un sencillo cubo y ahora es el momento de finalizar las cosas.

Configuremos primero una variable para realizar un seguimiento de si el juego finalizó o no:

```javascript
var gameOver = false;
```

Ahora necesitamos actualizar la función `animate()` una última vez para comprobar si el dinosaurio está demasiado cerca del jugador.
Si el dinosaurio está demasiado cerca, iniciaremos una nueva función denominada `caught()` e impediremos que el jugador y el dinosaurio se muevan; en caso contrario, dejamos que el juego prosiga y que el dinosaurio y el jugador sigan moviéndose.

```javascript
function animate() {
    render();
    requestAnimationFrame(animate);

    // Get the change in time between frames
    var delta = clock.getDelta();
    // Update our frames per second monitor

    // If the player is in dino's range, trigger the chase
    var isBeingChased = triggerChase();
    // If the player is too close, trigger the end of the game
    if (dino.position.distanceTo(controls.getObject().position) < CATCHOFFSET) {
        caught();
    // Player is at an undetected distance
    // Keep the dino moving and let the player keep moving too
    } else {
        animateDino(delta);
        animatePlayer(delta);
    }
}
```

Si la dinosaurio caza al jugador, `caught()` mostrará el elemento `blocker` y actualizará el texto para indicar que el juego se ha perdido.
La variable `gameOver` también se configura en `true`, que nos permitirá saber que el juego se acabó.  


```javascript
function caught() {
    blocker.style.display = '';
    instructions.innerHTML = "GAME OVER </br></br></br> Press ESC to restart";
    gameOver = true;
    instructions.style.display = '';
    dinoAlert.style.display = 'none';
}
```


Ahora que sabemos si el juego se acabó o no, podemos agregar una comprobación de fin del juego a la función `lockChange()`.
Ahora, cuando el usuario presiona la tecla ESC al finalizar el juego, podemos agregar `location.reload` para reiniciarlo.

```javascript
function lockChange() {
    if (document.pointerLockElement === container) {
        blocker.style.display = "none";
        controls.enabled = true;
    } else {
        if (gameOver) {
            location.reload();
        }
        blocker.style.display = "";
        controls.enabled = false;
    }
}
```

---

Eso es todo. Ha sido un viaje largo, pero ya tenemos nuestro juego creado con **three.js**.

Consulta la parte superior de la página para ver el [último CodePen](#introduction).


## <a name="publishing-to-the-windows-store"></a>Publicar en la Tienda Windows
Ahora ya tienes una aplicación para UWP, puedes publicar en Tienda Windows (siempre que la hayas mejorado). Este proceso tiene diferentes pasos.

1.    Tienes que estar [registrado](https://developer.microsoft.com/store/register) como desarrollador de Windows.
2.    Debes usar la [lista de comprobación](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) del envío de la aplicación.
3.    La aplicación debe enviarse para su [certificación](https://msdn.microsoft.com/windows/uwp/publish/the-app-certification-process).
Para obtener más información, consulta [Publishing your Windows Store app (Publicar tu aplicación de la Tienda Windows)](https://developer.microsoft.com/store/publish-apps).

