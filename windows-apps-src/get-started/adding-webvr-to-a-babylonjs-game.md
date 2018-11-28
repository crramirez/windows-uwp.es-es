---
title: Agregar compatibilidad con WebVR a un juego de Babylon.js 3D
description: Obtén información sobre cómo agregar compatibilidad con WebVR a un juego de Babylon.js 3D existente.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, desarrollo web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 3e2081f0dbe163dcbcf35d83ea111caf573dacfb
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7971318"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Agregar compatibilidad con WebVR a un juego de Babylon.js 3D

Si has creado un juego 3D con Babylon.js y considerar que es posible que sea excelente en realidad virtual (VR), sigue los pasos sencillos en este tutorial para hacer una realidad.

Vamos a agregar compatibilidad con WebVR del juego que se muestra aquí. Continuemos y conectar un mando de Xbox para probarlo.


<iframe height='300' scrolling='no' title='Juego de Babylon.js dinosaurio con Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulta el lápiz <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>juego de Babylon.js dinosaurio con Babylon.GUI</a> Microsoft Edge docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

Esto es un juego 3D que funciona bien en una pantalla plana, pero, ¿qué sobre en VR?
En este tutorial, te guiaremos por los pasos se toma para obtener este y ejecución con WebVR. Vamos a utilizar unos auriculares de [Realidad mixta de Windows](https://developer.microsoft.com/en-us/windows/mixed-reality) que pueden aprovechar la se agregó compatibilidad para WebVR en Microsoft Edge. Después de aplicar estos cambios para el juego, puedes esperar que también funcione en otras combinaciones de explorador/auriculares que admiten WebVR.



## <a name="prerequisites"></a>Requisitos previos

- Un editor de texto (por ejemplo, el [Código de Visual Studio](https://code.visualstudio.com/download))
- Un controlador de Xbox que está conectado al equipo
- Windows 10 Creators Update
- Un equipo con el [mínimo especificaciones necesarias para ejecutar Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un dispositivo de Windows Mixed Reality (opcional) 



## <a name="getting-started"></a>Tareas iniciales

La forma más sencilla de empezar es visitar el [repositorio de GitHub de web de tutoriales de Windows](https://github.com/Microsoft/Windows-tutorials-web), presione el verde **clonar o descarga** botón y selecciona **Abrir en Visual Studio**.

![botón de clonar o descargar](images/3dclone.png)

Si no quieres clonar el proyecto, puedes descargarlo como un archivo ZIP.
A continuación, tendrás dos carpetas, [antes](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) y [después](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). La carpeta "antes" es nuestro juego antes de que se agregan las características VR y la carpeta "después" es el juego terminado con compatibilidad VR.

El antes y después de las carpetas incluir estos archivos:
-   **texturas /** : una carpeta que contiene las imágenes que se usa en el juego.
-   **css /** : una carpeta que contiene el CSS del juego.
-   **js /** : una carpeta que contiene los archivos de JavaScript. El archivo main.js es nuestro juego, y los demás archivos son las bibliotecas que usa.
-   **modelos /** : una carpeta que contiene modelos 3D. Este juego tenemos un solo modelo para el dinosaurio.
-   **index.html** : la página Web que hospeda al representador del juego. Abrir esta página en Microsoft Edge, inicia el juego.

Puedes probar las dos versiones del juego abriendo sus archivos index.html correspondiente en Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>El Portal de realidad mixta

Si estás familiarizado con Windows Mixed Reality y tienes instalados en un equipo con una tarjeta gráfica compatible con Windows 10 Creators Update, intenta abrir la aplicación del **Portal de realidad mixta** desde el menú Inicio en Windows 10.

![Búsqueda de Portal de realidad mixta](images/mixed-reality-portal.png)

Si se cumplen todos los requisitos, a continuación, puede activar las funciones de desarrollador y simular unos auriculares de realidad mixta de Windows conectado al equipo. Si estás suerte de tener un auriculares real cercanos, conectarlo y ejecuta el programa de instalación.

> [!IMPORTANT]
> Portal de realidad mixta debe estar abierto en todo momento durante este tutorial.

Ahora estás listo para experimentar WebVR con Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interfaz de usuario 2D en un entorno virtual

>[!NOTE]
> Conseguir la carpeta [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) para obtener la muestra de inicio.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) es una biblioteca de VR cuentan con asistencia, lo que te permite para crear una sencilla, interfaces de usuario interactivas que funcionan bien para VR y no VR muestra.
Extensión de Babylon.js, el `GUI` biblioteca es throuhout usado la muestra para crear los elementos 2D.


Un texto 2D `GUI` elemento puede crearse con unas pocas líneas dependiendo de cuántos atributos que quieras modificar.
El siguiente fragmento de código ya está en nuestro ejemplo [**antes**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , pero vamos a tutorial lo qué está sucediendo.
Primero creamos una [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para establecer lo que se tratará la interfaz gráfica de usuario. El ejemplo establece esto a `CreateFullScreenUI()`, lo que significa que nuestra interfaz de usuario va a cubrir toda la pantalla. Con `AdvancedDynamicTexture` creado, a continuación, hacemos un cuadro de texto 2D que aparece al iniciar el juego usando `GUI.Rectanlge()` y `GUI.TextBlock()`.


Se agrega este código en [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Esta interfaz de usuario está visible una vez creado, pero se puede activar o desactivar con `isVisible` dependiendo de lo que sucede en el juego.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detección de auriculares

Es recomendable para aplicaciones de VR con dos tipos de cámaras para que se pueden admitir varios escenarios. Este juego, tendrás admitimos una cámara que requiere unos auriculares de trabajo para estar conectado y otro que no usa ningún auriculares. Para determinar cuál el juego se usa, primero debemos comprobamos para ver si se ha detectado unos auriculares. Para ello, usaremos [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Agrega este código anterior `window.addEventListener('DOMContentLoaded')` en el **archivo main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Con la información almacenada en el `headset` variable, ahora podrás elegir la cámara que es la adecuada para el usuario.


## <a name="creating-and-selecting-the-initial-camera"></a>Crear y seleccionando la cámara inicial

Con Babylon.js, se puede agregar WebVR rápidamente mediante el uso de la [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Esta cámara puede tomar la entrada de teclado y te permite usar unos auriculares VR para controlar la rotación de "head".


### <a name="step-1-checking-for-headsets"></a>Paso 1: Buscando auriculares

Para la cámara de reserva, usaremos el [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) que actualmente se usa en el juego original.

Comprobaremos nuestra `headset` variable para determinar si podemos usar el `WebVRFreeCamera` cámara.

Reemplazar `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` con el siguiente código.
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
Para activar la cámara en la mayoría de los exploradores, el usuario debe realizar alguna interacción que solicita la experiencia virtual.
Te enviaremos enlazar esta funcionalidad hasta un clic del mouse.


Pega el código dentro de `createScene()` funcionan después `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic en el juego ahora crea un mensaje similar al siguiente o muestra el juego en los auriculares inmediatamente si el usuario ha aceptado la solicitud antes.

![símbolo del sistema de envolvente](images/immersiveview.png)

También podemos agregar un fragmento de código que mostrará el `UniversalCamera` ver antes de que se ha cambiado a nuestro `WebVRFreeCamera`, lo que permite al usuario ver el juego en lugar de una ventana azul. 

Agrega el siguiente después `engine.runRenderLoop(function () {`.
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

Dado que la `WebVRFreeCamera` inicialmente no es compatible con los controladores para juegos, tendrás asignamos nuestros botones del controlador para juegos para las teclas de dirección del teclado. Esto haremos lo por seguir leyendo el `inputs` propiedad de la cámara. Al agregar los códigos correspondientes para el stick analógico izquierdo hacia arriba, abajo, izquierda y derecha para que coincida con las teclas de dirección, nuestro controlador para juegos es nuevo en acción.


Agrega este código a continuación la `scene.onPointerDown = function() {...}` llamar.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Paso 4: Probarla!

Si se abre el **archivo index.html** con nuestro auriculares y dispositivo de juego conectado, un clic izquierdo en la ventana del juego azul cambiará nuestro juego al modo VR! Continuemos y poner en los auriculares echar un vistazo a los resultados. 


<iframe height='300' scrolling='no' title='Juego de Babylon.js dinosaurio con Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulta el lápiz <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>juego de Babylon.js dinosaurio con Babylon.GUI - WebVR</a> Microsoft Edge docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusión

¡Enhorabuena! Ya tienes un juego de Babylon.js completado con WebVR compatibilidad. Desde aquí puede tardar aprendido a crear un juego aún mejor o generar desactivar esta.
