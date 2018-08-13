---
title: Adición de compatibilidad con WebVR a un juego de Babylon.js 3D
description: Obtenga información sobre cómo agregar compatibilidad con WebVR a un juego de Babylon.js 3D existente.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr, perimetral, desarrollo web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018655"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Adición de compatibilidad con WebVR a un juego de Babylon.js 3D

Si ha creado un juego 3D con Babylon.js y de elaboración de que puede tener una apariencia excelente en realidad virtual (VR), siga estos sencillos pasos en este tutorial para hacer una realidad.

Agregaremos compatibilidad con WebVR para el juego de los que se muestra aquí. ¡Vamos a conectar un controlador de Xbox probarlo!


<iframe height='300' scrolling='no' title='Juego de dino Babylon.js con Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino juego con Babylon.GUI</a> por Microsoft perimetral documentos (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

Este es un juego 3D que funciona bien en una pantalla plana, pero ¿qué acerca de VR?
En este tutorial, guiaremos a través de los pasos se lleve a cabo para obtener esta copia de seguridad y en ejecución con WebVR. Vamos a utilizar un auricular con micrófono de [La realidad mixto de Windows](https://developer.microsoft.com/en-us/windows/mixed-reality) que puede puntear en el se ha agregado compatibilidad con WebVR en Microsoft Edge. Después de que se aplicar estos cambios para el juego, puede esperar que también funcione en otras combinaciones de explorador o auriculares con micrófono que admiten WebVR.



## <a name="prerequisites"></a>Requisitos previos

- Un editor de texto (como el [Código de Visual Studio](https://code.visualstudio.com/download))
- Un controlador de Xbox que está conectado a su equipo
- Windows 10 Creators Update
- Un equipo con el [mínimas especificaciones necesarias para ejecutar la realidad mixto de Windows](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un dispositivo de la realidad mixto de Windows (opcional) 



## <a name="getting-started"></a>Introducción

La forma más sencilla para empezar a es visitar el [repo depósito de web de tutoriales de Windows](https://github.com/Microsoft/Windows-tutorials-web), presione el verde **clon o descargar** botón y seleccione **Abrir en Visual Studio**.

![botón de clonar o descargar](images/3dclone.png)

Si no quieres clonar el proyecto, puedes descargarlo como un archivo ZIP.
A continuación, tendrá dos carpetas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) y [después](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). La carpeta "before" es nuestro juego antes de que se agregan todas las características VR y la carpeta "después de" es el juego terminado con compatibilidad con VR.

El antes y después de las carpetas contienen estos archivos:
-   **texturas /** : una carpeta que contiene las imágenes utilizadas en el juego.
-   **css /** : una carpeta que contiene la CSS para el juego.
-   **js /** : una carpeta que contiene los archivos de JavaScript. El archivo main.js es nuestro juego y los demás archivos son las bibliotecas que se usa.
-   **modelos /** : una carpeta que contiene los modelos 3D. Para este juego tenemos un solo modelo, para el dinosaurio.
-   **index.html** - la página Web que hospeda a representador del juego. Abrir esta página en Microsoft Edge inicia el juego.

Puede probar ambas versiones del juego abriendo sus archivos respectivos index.html en Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>El Portal de la realidad mixto

Si está familiarizado con la realidad mixto de Windows y tiene la actualización de los creadores de 10 Windows instalado en un equipo con una tarjeta gráfica compatible, intente abrir la aplicación de **Portal de la realidad mixto** desde el menú Inicio de Windows 10.

![Búsqueda de Portal de la realidad mixto](images/mixed-reality-portal.png)

Si se cumplen todos los requisitos, a continuación, puede activar características para desarrolladores y simular la realidad mixto de Windows auriculares con micrófono conectados a su equipo. Si está suerte de contar con un auricular con micrófono real cercanos, conecte el dispositivo y ejecute el programa de instalación.

> [!IMPORTANT]
> El Portal de la realidad mixto debe estar abierto en todo momento durante este tutorial.

Ahora está listo para experimentar WebVR con Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interfaz de usuario 2D en un entorno virtual

>[!NOTE]
> Obtener la carpeta [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obtener el ejemplo de inicio.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) es una biblioteca de VR descriptivo, lo cual permite para crear simple, interfaces de usuario interactivas que funcionan bien para VR y VR no se muestra.
Extensión de Babylon.js, el `GUI` biblioteca es throuhout usado en el ejemplo para crear elementos 2D.


Un texto 2D `GUI` elemento puede crearse con unas pocas líneas dependiendo de cuántos atributos que desee ajustar.
El siguiente fragmento de código ya está en nuestro ejemplo [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , pero vamos a tutorial lo que sucede.
En primer lugar efectuamos una [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para establecer lo que se explicará en la interfaz gráfica de usuario. En el ejemplo se establece en `CreateFullScreenUI()`, lo que significa que nuestro la interfaz de usuario va a cubrir toda la pantalla. Con `AdvancedDynamicTexture` creado, a continuación, realizamos un cuadro de texto 2D que aparece al iniciar el uso de juego `GUI.Rectanlge()` y `GUI.TextBlock()`.


Este código se agrega dentro de [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Esta interfaz de usuario está visible una vez creado, pero puede activarse o desactivarse con `isVisible` dependiendo de lo que sucede en el juego.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detección de auriculares con micrófono

Es recomendable para que las aplicaciones VR tienen dos tipos de cámaras de modo que se pueden admitir varios escenarios. Para este juego se contará con soporte para una cámara que requiere un auricular con micrófono de trabajo para su conexión y otra que no usa ningún auriculares con micrófono. Para determinar cuál el juego utilizará, primero se debemos comprobar para ver si se ha detectado un auricular con micrófono. Para ello, utilizaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Agregue este código anterior `window.addEventListener('DOMContentLoaded')` en **main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Con la información almacenada en el `headset` variable, ahora podremos elegir la cámara que es el adecuado para el usuario.


## <a name="creating-and-selecting-the-initial-camera"></a>Creación y selección de la cámara inicial

Con Babylon.js, se puede agregar WebVR rápidamente mediante el uso de la [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Esta cámara puede tomar la entrada de teclado y le permite usar unos auriculares VR para controlar su giro "head".


### <a name="step-1-checking-for-headsets"></a>Paso 1: Comprobación de auriculares con micrófono

Para nuestra cámara reserva, usaremos la [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) que se utiliza actualmente en el juego original.

Se comprueba nuestro `headset` variable para determinar si podemos usar el `WebVRFreeCamera` cámara.

Reemplace `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` con el siguiente código.
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


### <a name="step-2-activating-the-webvrfreecamera"></a>Paso 2: Activar la WebVRFreeCamera
Para activar esta cámara en la mayoría de los exploradores, el usuario debe llevar a cabo alguna interacción que solicita la experiencia virtual.
Se va a enlazar esta funcionalidad hasta un clic del mouse.


Pegue el código dentro de `createScene()` funcionar después de `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic en el juego ahora crea un símbolo del sistema como el siguiente, o muestra el juego de los auriculares con micrófono inmediatamente si el usuario ha aceptado el símbolo del sistema antes de.

![símbolo del sistema de envolvente](images/immersiveview.png)

También podemos agregar un fragmento de código que se va a mostrar el el `UniversalCamera` ver antes de que se cambie a nuestro `WebVRFreeCamera`, que permite al usuario buscar en el juego en lugar de una ventana azul. 

Agregue la siguiente después de `engine.runRenderLoop(function () {`.
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

### <a name="step-3-adding-gamepad-support"></a>Paso 3: Agregar compatibilidad de controlador para juegos

Dado que el `WebVRFreeCamera` inicialmente no admite la configuración de los controles, se podrán asignar nuestros botones de controlador para juegos a las teclas de flecha del teclado. Lo haremos por seguir leyendo el `inputs` (propiedad) de la cámara. Mediante la adición de los códigos correspondientes de pincel analógico izquierdo hacia arriba, abajo, izquierda y derecha para hacer coincidir con las teclas de flecha, nuestro controlador para juegos es nuevo en acción.


Agregue este código a continuación la `scene.onPointerDown = function() {...}` de llamadas.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Paso 4: Pruébelo!

Si se puede abrir **index.html** con nuestro auriculares con micrófono y dispositivo de juego conectado, un haga clic en la ventana del juego azul cambiará nuestro juego al modo VR! Vamos a poner en los auriculares para desproteger los resultados. 


<iframe height='300' scrolling='no' title='Juego de dino Babylon.js con Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino juego con Babylon.GUI - WebVR</a> por Microsoft perimetral documentos (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusión

¡Enhorabuena! Ahora tiene un juego de Babylon.js completado con compatibilidad con WebVR. Desde aquí puede tardar lo que ha aprendido crear un juego incluso mejor, o crear desactivado este uno.
