---
title: Agregar compatibilidad WebVR a un juego 3D de Babylon.js
description: Aprende a agregar compatibilidad con WebVR a un juego 3D de Babylon.js existente.
ms.date: 11/29/2017
ms.topic: article
keywords: webvr, edge, desarrollo web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1d8029752790e19adc5eb4266615372fb346e001
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638560"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Agregar compatibilidad WebVR a un juego 3D de Babylon.js

Si has creado un juego 3D con Babylon.js y piensas que puede tener un aspecto estupendo en realidad virtual (VR), sigue los sencillos pasos de este tutorial para hacerlo realidad.

Agregaremos compatibilidad con WebVR al juego que se muestra aquí. ¡Adelante, conecta un controlador de Xbox!


<iframe height='300' scrolling='no' title='Babylon.js dino juego mediante Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino juego mediante Babylon.GUI</a> mediante Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

Es un juego 3D que funciona bien en una pantalla plana, pero ¿funcionará en VR?
En este tutorial, te guiaremos a través de los pocos pasos que se necesitan para que funcione con WebVR. Vamos a usar unos auriculares [Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality) que pueden aprovechar la compatibilidad agregada para WebVR en Microsoft Edge. Después de aplicar estos cambios al juego, es posible que también funcione con otras combinaciones de explorador y auriculares compatibles con WebVR.



## <a name="prerequisites"></a>Requisitos previos

- Un editor de texto (como [Visual Studio Code](https://code.visualstudio.com/download))
- Un Xbox Controller conectado al equipo
- Windows 10 Creators Update
- Un equipo con las [especificaciones mínimas necesarias para ejecutar Windows Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un dispositivo Windows Mixed Reality (opcional) 



## <a name="getting-started"></a>Introducción

La forma más sencilla de empezar es visitar el [repositorio de GitHub Windows-tutorials-web](https://github.com/Microsoft/Windows-tutorials-web), pulsar en el botón verde **Clone or download** y seleccionar **Abrir en Visual Studio**.

![botón de clonar o descargar](images/3dclone.png)

Si no quieres clonar el proyecto, puedes descargarlo como un archivo ZIP.
Tendrás dos carpetas, [before](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) y [after](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). La carpeta "before" es nuestro juego antes de agregar las características de VR y la carpeta "after" es el juego terminado con compatibilidad con VR.

Las carpetas before y after contienen estos archivos:
-   **las texturas /** : una carpeta que contiene las imágenes utilizadas en el juego.
-   **CSS /** : una carpeta que contiene el CSS para el juego.
-   **js /** : una carpeta que contiene los archivos de JavaScript. El archivo main.js es nuestro juego y los demás archivos son las bibliotecas usadas.
-   **modelos /** : una carpeta que contiene los modelos 3D. En este juego, solo tenemos un modelo, para el dinosaurio.
-   **index.HTML** : la página Web que hospeda el representador del juego. Abra esta página en Microsoft Edge, inicia el juego.

Puede probar ambas versiones del juego abriendo sus archivos respectivos index.html en Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Portal de realidad mixta

Si estás familiarizado con Windows Mixed Reality y has instalado Windows 10 Creators Update en un equipo con una tarjeta gráfica compatible, intenta abrir la aplicación **Portal de realidad mixta** desde el menú Inicio de Windows 10.

![Búsqueda en el Portal de realidad mixta](images/mixed-reality-portal.png)

Si cumples todos los requisitos, puedes activar las características de desarrollador y simular unos auriculares de Windows Mixed Reality conectados al equipo. Si tienes la suerte de tener unos auriculares reales a mano, conéctalos y ejecuta el programa de instalación.

> [!IMPORTANT]
> El Portal de realidad mixta debe estar abierto en todo momento durante este tutorial.

Ahora está listo para la experiencia WebVR con Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interfaz de usuario 2D en un mundo virtual

>[!NOTE]
> Agarre el [ **antes** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) carpeta para obtener el ejemplo de inicio.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) es una biblioteca compatible con la realidad virtual, lo que le permite para crear una sencilla, interfaces de usuario interactivas que funcionan bien para VR y no-VR muestra.
Extensión de Babylon.js, el `GUI` biblioteca es throuhout usa el ejemplo para crear elementos 2D.


Un texto 2D `GUI` elemento puede crearse con unas pocas líneas dependiendo de cuántos atributos que desea modificar.
El siguiente fragmento de código ya está en nuestro [ **antes** ](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) de ejemplo, pero vamos a tutorial lo que sucede.
Primero realizamos una [ `AdvancedDynamicTexture` ](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objeto para establecer qué tratará la GUI. El ejemplo lo establece en `CreateFullScreenUI()`, lo que significa que la interfaz de usuario se tratará toda la pantalla. Con `AdvancedDynamicTexture` creado, a continuación, realizamos un cuadro de texto 2D que aparece al iniciar el juego utilizando `GUI.Rectanlge()` y `GUI.TextBlock()`.


Este código se agrega en [ **main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
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


Esta interfaz de usuario está visible una vez creado, pero puede activar o desactivar con `isVisible` dependiendo de lo que sucede en el juego.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Detección de los auriculares

Es recomendable para que las aplicaciones de realidad virtual tienen dos tipos de cámaras para que se admiten varios escenarios. En este juego, se admitirá una cámara que requiera que estén conectados unos auriculares que funcionen y otra que no use auriculares. Para determinar cuál de ellas usará el juego, primero tenemos que comprobar si se han detectado unos auriculares. Para ello, vamos a usar [ `navigator.getVRDisplays()` ](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Agregue el código anterior `window.addEventListener('DOMContentLoaded')` en **main.js**.
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


## <a name="creating-and-selecting-the-initial-camera"></a>Creación y la selección de la cámara inicial

Con Babylon.js, se puede agregar WebVR rápidamente mediante el uso de la [ `WebVRFreeCamera` ](https://doc.babylonjs.com/classes/3.1/webvrfreecamera). Esta cámara puede tomar la entrada del teclado y permite que uses unos auriculares VR para controlar la rotación de la "cabeza".


### <a name="step-1-checking-for-headsets"></a>Paso 1: Comprobación de auriculares

Para nuestro cámara reserva, vamos a usar el [ `UniversalCamera` ](https://doc.babylonjs.com/classes/3.1/universalcamera) que actualmente se usa en el juego original.

Le enviaremos nuestra `headset` variable para determinar si se puede usar el `WebVRFreeCamera` cámara.

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


### <a name="step-2-activating-the-webvrfreecamera"></a>Paso 2: Activar el objeto WebVRFreeCamera
Para activar esta cámara en la mayoría de los exploradores, el usuario tendrá que realizar alguna interacción que solicite la experiencia virtual.
Se deberá enlazar esta funcionalidad hasta un clic del mouse.


Pega el código dentro de la función `createScene()` después de `camera.applyGravity = true;`.
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic en el juego ahora crea un mensaje similar al siguiente, o muestra el juego en los auriculares inmediatamente si el usuario ha aceptado el símbolo del sistema antes.

![petición de confirmación de envolvente](images/immersiveview.png)

También podemos agregar un fragmento de código que se mostrará el `UniversalCamera` ver antes de que se va a nuestro `WebVRFreeCamera`, que permite al usuario examine el juego en lugar de una ventana de azul. 

Agregue lo siguiente después `engine.runRenderLoop(function () {`.
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

### <a name="step-3-adding-gamepad-support"></a>Paso 3: Añadir compatibilidad con el controlador para juegos

Puesto que el `WebVRFreeCamera` inicialmente no admite la configuración de los controles, le asignaremos nuestros botones gamepad a las teclas de flecha del teclado. Haremos esto por profundizando en el `inputs` propiedad de la cámara. Al agregar los códigos correspondientes para el stick analógico izquierdo hacia arriba, abajo, izquierda y derecha para que coincidan con las teclas de dirección, el controlador para juegos volverá a la acción.


Agregue este código bajo el `scene.onPointerDown = function() {...}` llamar.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Paso 4: ¡Pruébelo!

Si se abre **index.html** con nuestros auriculares y el dispositivo de juego con corriente alterna, un clic en la ventana de juego azul izquierdo cambiará nuestro juego al modo VR! Adelante, ponte los auriculares para echar un vistazo a los resultados. 


<iframe height='300' scrolling='no' title='Babylon.js dino juego mediante Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino juego mediante Babylon.GUI - WebVR</a> mediante Microsoft Edge Docs (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusión

Enhorabuena. Ahora tienes un juego completo de Babylon.js compatible con WebVR. Desde aquí puedes usar lo que has aprendido para crear un juego aún mejor o basarlo en este.
