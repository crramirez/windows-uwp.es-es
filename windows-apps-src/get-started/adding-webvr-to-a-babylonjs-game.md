---
title: Agregar compatibilidad con WebVR a un juego 3D de Babylon.js
description: Siga este tutorial para obtener información sobre cómo agregar compatibilidad de realidad virtual con WebVR a un juego 3D Babylon.js existente.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr;edge;desarrollo web;babylon;babylonjs;babylon.js;javascript;web development
ms.localizationpriority: medium
ms.openlocfilehash: a01e459160025e9ed1b83fbe81da6d562340691e
ms.sourcegitcommit: 45dec3dc0f14934b8ecf1ee276070b553f48074d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2020
ms.locfileid: "89094552"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Agregar compatibilidad con WebVR a un juego 3D de Babylon.js

Si has creado un juego 3D con Babylon.js y piensas que puede tener un aspecto estupendo en realidad virtual (VR), sigue los sencillos pasos de este tutorial para hacerlo realidad.

En este tutorial, te guiaremos a través de los pocos pasos que se necesitan para que un juego 3D funcione con WebVR. Vamos a usar un casco de [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) que puede aprovechar la compatibilidad agregada para WebVR en Microsoft Edge. Después de aplicar estos cambios al juego, es posible que también funcione con otras combinaciones de explorador y casco de realidad mixta compatibles con WebVR.



## <a name="prerequisites"></a>Requisitos previos

- Un editor de texto (como [Visual Studio Code](https://code.visualstudio.com/download)).
- Un mando de Xbox conectado al equipo.
- Windows 10 Creators Update.
- Un equipo con las [especificaciones mínimas necesarias para ejecutar Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality/immersive_headset_setup)
- Un dispositivo Windows Mixed Reality (opcional). 



## <a name="getting-started"></a>Introducción

La forma más sencilla de empezar es visitar el [repositorio de GitHub Windows-tutorials-web](https://github.com/Microsoft/Windows-tutorials-web), pulsar en el botón verde **Clone or download** (clonar o descargar) y seleccionar **Abrir en Visual Studio**.

![botón para clonar o descargar](images/3dclone.png)

Si no quieres clonar el proyecto, puedes descargarlo como un archivo ZIP.
Tendrás dos carpetas, [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) y [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). La carpeta "before" es nuestro juego antes de agregar las características de VR y la carpeta "after" es el juego terminado con compatibilidad con VR.

Las carpetas before y after contienen estos archivos:
-   **textures/** : carpeta que contiene las imágenes usadas en el juego.
-   **css/** : carpeta que contiene el código CSS para el juego.
-   **js/** : una carpeta que contiene archivos JavaScript. El archivo main.js es nuestro juego y los demás archivos son las bibliotecas usadas.
-   **models/** : carpeta que contiene los modelos 3D. En este juego solo tenemos un modelo: el del dinosaurio.
-   **index.html**: página web que hospeda al representador del juego. Al abrir esta página en Microsoft Edge, se inicia el juego.

Puedes probar ambas versiones del juego si abres sus archivos index.html respectivos en Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Portal de realidad mixta

Si estás familiarizado con Windows Mixed Reality y has instalado Windows 10 Creators Update en un equipo con una tarjeta gráfica compatible, intenta abrir la aplicación **Portal de realidad mixta** desde el menú Inicio de Windows 10.

![Búsqueda en el Portal de realidad mixta](images/mixed-reality-portal.png)

Si cumples todos los requisitos, puedes activar las características de desarrollador y simular un casco de Windows Mixed Reality conectado al equipo. Si tienes la suerte de tener un casco real a mano, conéctalo y ejecuta el programa de instalación.

> [!IMPORTANT]
> El Portal de realidad mixta debe estar abierto en todo momento durante este tutorial.

Ahora estás listo para experimentar WebVR con Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interfaz de usuario 2D en un mundo virtual

>[!NOTE]
> Abre la carpeta [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obtener el ejemplo de inicio.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) es una biblioteca compatible con realidad virtual que te permite crear interfaces de usuario sencillas e interactivas que funcionan bien para pantallas con y sin VR.
La biblioteca `GUI`, que es una extensión de Babylon.js, se usa durante el ejemplo para crear elementos 2D.


Un elemento `GUI` de texto en 2D puede crearse con unas pocas líneas dependiendo de cuántos atributos quieras modificar.
El siguiente fragmento de código ya se encuentra en nuestro ejemplo [**before**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before), pero examinaremos qué ocurre paso a paso.
Primero, creamos un objeto [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) para establecer la extensión de la GUI. En este ejemplo, el valor es `CreateFullScreenUI()`; es decir que la interfaz de usuario cubrirá toda la pantalla. Tras crear al objeto `AdvancedDynamicTexture`, creamos un cuadro de texto 2D que se muestra al iniciar el juego mediante los métodos `GUI.Rectanlge()` y `GUI.TextBlock()`.


Este código se agrega en [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


Esta interfaz de usuario está visible cuando se crea, pero puede activarse y desactivarse con la propiedad `isVisible` dependiendo de lo que sucede en el juego.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detección de los cascos

Es recomendable que las aplicaciones VR tengan dos tipos de cámaras para admitir varios escenarios. En este juego, se admitirá una cámara que requiera que esté conectado un casco de realidad mixta, y otra que no requiere del casco. Para determinar cuál de ellas usará el juego, primero tenemos que comprobar si se ha detectado un casco de realidad mixta. Para ello, usaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Agrega el código antes de `window.addEventListener('DOMContentLoaded')` en **main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Con la información almacenada en la variable `headset`, ahora podremos elegir la cámara adecuada para el usuario.


## <a name="creating-and-selecting-the-initial-camera"></a>Creación y selección de la cámara inicial

Con Babylon.js, se puede agregar WebVR rápidamente mediante la clase [`WebVRFreeCamera`](https://doc.babylonjs.com/api/classes/babylon.webvrfreecamera). Esta cámara puede interpretar la entrada del teclado y permite usar un casco de realidad mixta VR para controlar la rotación de la "cabeza".


### <a name="step-1-checking-for-headsets"></a>Paso 1: comprobación del casco

Como cámara de reserva, usaremos la cámara [`UniversalCamera`](https://doc.babylonjs.com/api/classes/babylon.universalcamera) que se usa actualmente en el juego original.

Comprobaremos la variable `headset` para determinar si podemos usar la cámara `WebVRFreeCamera`.

Reemplaza `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` por el siguiente código.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>Paso 2: activación de la cámara WebVRFreeCamera
Para activar esta cámara en la mayoría de los exploradores, el usuario tendrá que realizar alguna interacción solicitada por la experiencia virtual.
Vincularemos esta funcionalidad con un clic del mouse.


Pega el código dentro de la función `createScene()` después de `camera.applyGravity = true;`.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Ahora, un clic en el juego crea una petición de confirmación como la siguiente, o muestra el juego dentro del casco inmediatamente si el usuario ya había recibido antes la petición de confirmación.

![petición de confirmación envolvente](images/immersiveview.png)

También podemos agregar un fragmento de código que muestre la vista `UniversalCamera` antes de cambiar a `WebVRFreeCamera`. Esto permite que el usuario vea el juego en vez de una ventana azul. 

Agrega el código siguientes después de `engine.runRenderLoop(function () {`.
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>Paso 3: Agregar compatibilidad con el controlador para juegos

Dado que la cámara `WebVRFreeCamera` no es compatible inicialmente con los controladores para juegos, tendremos que asignar los botones del controlador a las teclas de dirección del teclado. Lo haremos profundizando en la propiedad `inputs` de la cámara. El controlador para juegos estará listo cuando agregues los códigos correspondientes para que el stick analógico izquierdo hacia arriba, abajo, izquierda y derecha coincida con las teclas de dirección.


Agregue este código bajo la llamada `scene.onPointerDown = function() {...}`.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Paso 4: Pruébalo

Si abrimos **index.html** después de conectar el casco y el dispositivo de juego, bastará un clic izquierdo en la ventana de juego azul para cambiar nuestro juego al modo VR. Adelante, ponte el casco para echar un vistazo a los resultados. 



## <a name="conclusion"></a>Conclusión

Enhorabuena. Ahora tienes un juego completo de Babylon.js compatible con WebVR. Desde aquí puedes usar lo que has aprendido para crear un juego aún mejor o basarlo en este.
