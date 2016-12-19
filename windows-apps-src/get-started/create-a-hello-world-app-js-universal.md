---
author: GrantMeStrength
ms.assetid: CFB3601D-3459-465F-80E2-520F57B88F62
title: Create a "Hello, world" app (JS)
description: "Este tutorial te enseña a usar JavaScript y HTML para crear una aplicación sencilla Hello, world para la Plataforma universal de Windows (UWP) en Windows 10."
translationtype: Human Translation
ms.sourcegitcommit: 1a4aea3d31bad97fa0933e1274c037a4bb8d81bb
ms.openlocfilehash: ad34b1bc62abf6c93f5124e774ad374f5b767f2c

---
# <a name="create-a-hello-world-app-js"></a>Crear una aplicación "Hello, world" (JS)

Este tutorial te enseña a usar JavaScript y HTML para crear una aplicación sencilla "Hello, world" para la Plataforma universal de Windows (UWP) en Windows 10. Con un único proyecto en Microsoft Visual Studio, puedes compilar una aplicación que se ejecute en cualquier dispositivo de Windows 10.

Aquí aprenderás a:

-   Crear un nuevo proyecto de **Visual Studio 2015** diseñado para **Windows 10** y **UWP**.
-   Agregar contenido HTML a tu página de inicio
-   Controlar la entrada táctil, de pluma y mouse
-   Ejecutar el proyecto en el escritorio local y en el emulador de teléfono de Visual Studio
-   Usar un control de la Biblioteca de Windows para JavaScript

## <a name="before-you-start"></a>Antes de comenzar...

-   [¿Qué es una aplicación universal de Windows](whats-a-uwp.md)?
-   Para realizar este tutorial, debes tener Windows 10 y Visual Studio 2015. [Prepárate](get-set-up.md).
-   También se supone que estás usando el diseño de ventana predeterminado de Visual Studio. Si cambias el diseño predeterminado, puedes restablecerlo en el menú **Ventana** con el comando **Restablecer diseño de la ventana**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Paso 1: Crear un nuevo proyecto en Visual Studio

Vamos a crear una nueva aplicación llamada `HelloWorld`. A continuación se muestra cómo hacerlo:
1.  Inicia Visual Studio 2015.

2.  En el menú **Archivo**, selecciona **Nuevo > Proyecto...** para abrir el cuadro de diálogo *Nuevo proyecto*.

3.  En la lista de plantillas del lado izquierdo, expande **Instalado > Plantillas > JavaScript > Windows** y, a continuación, elige **Universal** para ver la lista de plantillas de proyecto para UWP. Elige **WinJS App (Windows universal)**.

    ![La ventana Nuevo proyecto ](images/winjs-tut-newproject.png)

    Para este tutorial, usamos la plantilla **WinJS App**. Esta plantilla crea una aplicación para UWP mínima que se compila y ejecuta, pero que no contiene datos ni controles de interfaz de usuario. A lo largo de estos tutoriales, agregarás controles y datos a la aplicación.

   (Si no ves estas opciones, asegúrate de tener instaladas las herramientas de desarrollo de las aplicaciones universales de Windows. Consulta [Preparación](get-set-up.md) para obtener más información).

4.  En el cuadro de texto **Nombre**, escribe "HelloWorld".
5.  Haz clic en **Aceptar** para crear el proyecto.
6.  Se te pedirá que selecciones una **Versión de destino** y una **Versión mínima** que Windows pueda admitir. La configuración predeterminada es precisa, por lo tanto, haz clic en **Aceptar**.

    Visual Studio crea tu proyecto y lo muestra en el **Explorador de soluciones**.

    ![Explorador de soluciones de Visual Studio para el proyecto HelloWorld](images/winjs-tut-helloworld.png)

Aunque **WinJS App** sea una plantilla mínima, contiene muchos archivos:

-   Un archivo de manifiesto (package.appxmanifest) que describe tu aplicación (nombre, descripción, icono, página de inicio, página de presentación, etc.) y enumera los archivos que contiene la aplicación.
-   Un conjunto de imágenes de logotipo (images/Square150x150Logo.scale-200.png, images/Square44x44Logo.scale-200.png e images/Wide310x150Logo.scale-200.png) que se pueden mostrar en el menú Inicio.
-   Una imagen (images/StoreLogo.png) para representar tu aplicación en la Tienda Windows.
-   Una pantalla de presentación (images/SplashScreen.scale-200.png) que se muestra cuando se inicia la aplicación.
-   Una página de inicio (index.html) y el archivo JavaScript correspondiente (main.js) que se ejecutan cuando se inicia tu aplicación.

Para ver y editar los archivos, haz doble clic en el archivo que quieras en el **Explorador de soluciones**.

Estos archivos son esenciales para todas las aplicaciones para UWP que usan JavaScript. Cualquier proyecto que crees en Visual Studio contendrá estos archivos.

## <a name="step-2-launch-the-app"></a>Paso 2: Iniciar la aplicación


En este punto, has creado una aplicación muy sencilla. Este es un buen momento para compilar, implementar e iniciar tu aplicación para ver su aspecto. Puedes depurar la aplicación en el equipo local, en un simulador, un emulador o en un dispositivo remoto. Este es el menú del dispositivo de destino de Visual Studio.

![Lista desplegable de destinos de dispositivo para depurar la aplicación](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>Iniciar la aplicación en un dispositivo de escritorio

De forma predeterminada, la aplicación se ejecuta en el equipo local. El menú del dispositivo de destino proporciona varias opciones para depurar la aplicación en dispositivos de la familia de dispositivos de escritorio.

-   **Simulador**
-   **Equipo local**
-   **Equipo remoto**

**Para empezar la depuración en el equipo local**

1.  En el menú del dispositivo de destino (![menú Iniciar depuración](images/startdebug-full.png)) de la barra de herramientas **Estándar**, asegúrate de que **Equipo local** esté seleccionado. (Es la selección predeterminada).
2.  Haz clic en el botón **Iniciar depuración** (![botón Iniciar depuración](images/startdebug-sm.png)) en la barra de herramientas.

   O bien

   En el menú **Depurar**, haz clic en **Iniciar depuración**.

   O bien

   Presiona F5.

La aplicación se abre en una ventana y, en primer lugar, aparece una pantalla de presentación predeterminada. Esta pantalla se define mediante una imagen (SplashScreen.png) y un color de fondo (especificado en el archivo de manifiesto de la aplicación).

La pantalla de presentación desaparece y, a continuación, aparece tu aplicación. Contiene una pantalla negra con el texto "Aquí se debe incluir el contenido".

![La aplicación HelloWorld en tu PC](images/helloworld-1-winjs.png)

Presiona la tecla Windows para abrir el menú **Inicio** y mostrar todas las aplicaciones. Ten en cuenta que al implementar la aplicación localmente se agrega su icono al menú **Inicio**. Para ejecutar la aplicación de nuevo (no en modo de depuración), pulsa o haz clic en su icono en el menú **Inicio**.

No hace muchas cosas (todavía), pero te felicitamos, has compilado tu primera aplicación para UWP.

**Para detener la depuración**

-   Haz clic en el botón **Detener depuración** (![botón Detener depuración](images/stopdebug.png)) en la barra de herramientas.

   O bien

   En el menú **Depurar**, haz clic en **Detener depuración**.

   O bien

   Cierra la ventana de la aplicación.

### <a name="start-the-app-on-a-mobile-device-emulator"></a>Iniciar la aplicación en un emulador de dispositivos móviles

La aplicación se ejecuta en cualquier dispositivo de Windows 10, así que vamos a ver su aspecto en un Windows Phone.

Además de las opciones para realizar la depuración en un dispositivo de escritorio, Visual Studio ofrece opciones para implementar y depurar la aplicación en un dispositivo móvil físico conectado al equipo o en un emulador de dispositivos móviles. Puedes elegir entre varios emuladores para dispositivos con diferentes configuraciones de memoria y pantalla.

-   **Dispositivo**
-   **Emulador <SDK version> WVGA de 4 pulgadas y 512 MB**
-   **Emulador <SDK version> WVGA de 4 pulgadas y 1 GB**
-   Etc. (Varios emuladores en otras configuraciones)

(Si no ves los emuladores, asegúrate de tener instaladas las herramientas de desarrollo de las aplicaciones universales de Windows. Consulta [Preparación](get-set-up.md) para obtener más información).

Se recomienda probar la aplicación en un dispositivo con una pantalla pequeña y de memoria limitada, por lo tanto, usa la opción **Emulador 10.0.14393.0 WVGA de 4 pulgadas y 512 MB**.

**Para iniciar la depuración en un emulador de dispositivo móvil**

1.  En el menú del dispositivo de destino (![menú Iniciar depuración](images/startdebug-full.png)) en la barra de herramientas **Estándar**, elige **Emulador 10.0.10240.0 WVGA de 4 pulgadas y 512 MB**.
2.  Haz clic en el botón **Iniciar depuración** (![botón Iniciar depuración](images/startdebug-sm.png)) en la barra de herramientas.

   O bien

   En el menú **Depurar**, haz clic en **Iniciar depuración**.


Visual Studio inicia el emulador seleccionado y, a continuación, implementa e inicia la aplicación. En el lanzamiento inicial, el emulador puede tardar un rato en empezar. Es posible que veas un error en relación con HyperV, pero debería solucionarse haciendo clic en **Reintentar**. En el emulador de dispositivos móviles, la aplicación tiene este aspecto.

![Pantalla inicial de la aplicación en un dispositivo móvil](images/helloworld-1-winjs-phone.png)

## <a name="step-3-modify-your-start-page"></a>Paso 3: Modificar tu página de inicio

Uno de los archivos que ha creado Visual Studio para ti es **index.html**, la página de inicio de tu aplicación. Cuando se ejecuta la aplicación, muestra el contenido de la página de inicio. Esta también contiene referencias a los archivos de código y hojas de estilo de la aplicación. Esta es la página de inicio que Visual Studio ha creado para ti:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>HelloWorld</title>

    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>

    <!-- HelloWorld references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/main.js"></script>
</head>
<body class="win-type-body">
    <div>Content goes here!</div>
</body>
</html>
```

Vamos a agregar contenido nuevo a tu archivo default.html. Al igual que harías para agregar contenido a cualquier otro archivo HTML, agregas tu contenido dentro del elemento [body](https://msdn.microsoft.com/library/windows/apps/Hh453011). Puedes usar elementos de HTML5 para crear tu aplicación (con unas [pocas excepciones](https://msdn.microsoft.com/library/windows/apps/Hh465380)). Esto significa que puedes usar elementos de HTML5, como [h1](https://msdn.microsoft.com/library/windows/apps/Hh441078), [p](https://msdn.microsoft.com/library/windows/apps/Hh453431), [button](https://msdn.microsoft.com/library/windows/apps/Hh453017), [div](https://msdn.microsoft.com/library/windows/apps/Hh453133) e [img](https://msdn.microsoft.com/library/windows/apps/Hh466114).

**Editar la página de inicio**

1.  Reemplaza el contenido existente en el elemento **body** por un encabezado de primer nivel que diga "Hello, world!", algún texto que solicite el nombre del usuario, un elemento **input** para aceptar el nombre del usuario, un elemento **button** y un elemento **div**. Asigna identificadores a los elementos **input**, **button** y **div**.

 ```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
    </body>
 ```

2.  Ejecuta la aplicación en el equipo local. Tiene esta apariencia.

![La aplicación HelloWorld con el nuevo contenido](images/helloworld-2-winjs.png)

   Puedes escribir en el elemento **input**, pero, ahora mismo, si haces clic en **button**, no se realiza ninguna acción. Algunos objetos, como **button**, pueden enviar mensajes cuando se producen ciertos eventos. Estos mensajes de eventos te dan la oportunidad de realizar alguna acción como respuesta al evento. El código para responder al evento lo pones en un método de controlador de eventos.

   En los siguientes pasos, crearemos un controlador de eventos para el elemento **button** que muestre un saludo personalizado. Agregamos el código de nuestro controlador de eventos a nuestro archivo main.js.

## <a name="step-4-create-an-event-handler"></a>Paso 4: Crear un controlador de eventos

Cuando creamos nuestro proyecto, Visual Studio creó un archivo /js/main.js. Este archivo contiene código para controlar el ciclo de vida de tu aplicación. También es donde escribes código adicional que proporciona interactividad para tu archivo index.html.

Abre el archivo main.js.

Antes de empezar a agregar nuestro propio código, echemos un vistazo a las primeras y últimas líneas de código del archivo:

```javascript
(function () {
    "use strict";

     // Omitted code

 })();
```

Te estarás preguntando qué sucede con ellas. Estas líneas de código encapsulan el resto del código de main.js en una función anónima que se ejecuta automáticamente. Una función anónima que se ejecuta automáticamente hace que sea más sencillo evitar conflictos de nomenclatura o situaciones en las que modifiques accidentalmente un valor que no tenías intención de modificar. Además, mantiene los identificadores innecesarios lejos del espacio de nombres global, lo que contribuye a mejorar el rendimiento. Parece una cosa un poco rara, pero es un procedimiento recomendado de programación.

La siguiente línea de código activa el [modo strict](https://msdn.microsoft.com/library/windows/apps/br230269.aspx) para tu código JavaScript. El modo strict proporciona una comprobación adicional de errores para el código. Por ejemplo, impide que uses variables declaradas implícitamente o que asignes un valor a una propiedad de solo lectura.

Echa un vistazo al resto del código en main.js. Controla los eventos [activated](https://msdn.microsoft.com/library/windows/apps/BR212679) y [checkpoint](https://msdn.microsoft.com/library/windows/apps/BR229839) de tu aplicación. Más adelante, veremos con más detalle esos eventos. Por ahora, solo debes saber que el evento **activated** se desencadena cuando se inicia tu aplicación.

```javascript
   (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
          if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
          else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
              if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
              }
              else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);
            args.setPromise(WinJS.UI.processAll());
        }

        isFirstActivation = false;
    };
```

Vamos a definir un controlador de eventos para [button](https://msdn.microsoft.com/library/windows/apps/Hh453017). Nuestro nuevo controlador de eventos obtiene el nombre de usuario del control `nameInput` [input](https://msdn.microsoft.com/library/windows/apps/Hh453271) y lo usa para mostrar un saludo al elemento `greetingOutput` **div** que creaste en la sección anterior.

### <a name="using-events-that-work-for-touch-mouse-and-pen-input"></a>Uso de eventos que funcionan para entradas táctiles, de mouse y de lápiz

En una aplicación para UWP, no debes preocuparte por las diferencias entre táctil, mouse y otras formas de entrada de puntero. Puedes usar los eventos que conoces, como [click](https://msdn.microsoft.com/library/windows/apps/Hh441312) y funcionarán para todos los tipos de entrada.

**Sugerencia**   Tu aplicación también puede usar los nuevos eventos *MSPointer\** y *MSGesture\**, que funcionan para la entrada táctil, de mouse y de lápiz, y que pueden proporcionar información adicional sobre el dispositivo que activó el evento. Para obtener más información, consulta [Responder a la interacción del usuario](https://msdn.microsoft.com/library/windows/apps/Hh700412) y [Gestos, manipulaciones e interacciones](https://msdn.microsoft.com/library/windows/apps/Hh761498).

Continuemos y vamos a crear el controlador de eventos.

**Crear el controlador de eventos**

1.  En el archivo main.js, después del controlador de eventos [**app.oncheckpoint**](https://msdn.microsoft.com/library/windows/apps/BR229839) y antes de llamar a [**app.start**](https://msdn.microsoft.com/library/windows/apps/BR229705), crea una función de controlador de eventos [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312) denominada `buttonClickHandler` que tome un único parámetro llamado `eventInfo`.
```javascript
    function buttonClickHandler(eventInfo) {

        }
```

2.  Dentro de nuestro controlador de eventos, recupera el nombre de usuario del control `nameInput` [**input**](https://msdn.microsoft.com/library/windows/apps/Hh453271) y úsalo para crear un saludo. Usa `greetingOutput` [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) para mostrar el resultado.
```javascript
    function buttonClickHandler(eventInfo) {
            var userName = document.getElementById("nameInput").value;
            var greetingString = "Hello, " + userName + "!";
            document.getElementById("greetingOutput").innerText = greetingString;
        }
 ```

Has agregado tu controlador de eventos a main.js. Ahora debes registrarlo.

## <a name="step-5-register-the-event-handler-when-your-app-launches"></a>Paso 5: Registrar el controlador de eventos cuando se inicia la aplicación


Lo único que necesitas hacer ahora es registrar el controlador de eventos con el botón. La forma recomendada de registrar un controlador de eventos es llamar a [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) desde nuestro código. Un buen lugar para registrar el controlador de eventos es cuando se activa nuestra aplicación. Afortunadamente, como ya viste, Visual Studio generó algo de código en nuestro archivo main.js que controla la activación de nuestra aplicación.


Dentro del controlador [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679), el código comprueba qué tipo de activación se ha producido. Existen diferentes tipos de activación. Por ejemplo, tu aplicación se activa cuando el usuario inicia tu aplicación y cuando el usuario quiere abrir un archivo que está asociado con tu aplicación. (Para obtener más información, consulta [Ciclo de vida de la aplicación](https://msdn.microsoft.com/library/windows/apps/Mt243287)).

La que nos interesa a nosotros es la de activación [launch](https://msdn.microsoft.com/library/windows/apps/BR224693). Una aplicación se *inicia* cuando un usuario la activa cuando no está en ejecución. Llama a [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975), con independencia de si se cerró la aplicación en el pasado o si es la primera vez que se inicia. **WinJS.UI.processAll** está incluido en una llamada al método [setPromise](https://msdn.microsoft.com/library/windows/apps/JJ215609), que se asegura de que la pantalla de presentación no desaparezca hasta que la página de la aplicación esté lista.

**Sugerencia**   La función **WinJS.UI.processAll** examina si en el archivo default.html hay controles WinJS y los inicializa. Hasta ahora, no hemos agregado ninguno de estos controles, pero te recomendamos dejar este código en caso de que quieras agregarlos más adelante.

Un buen lugar donde registrar los controladores de eventos para controles que no sean de WinJS es justo después de la llamada a **WinJS.UI.processAll**.

**Registrar tu controlador de eventos**

-   En el controlador de eventos [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) de main.js, recupera `helloButton` y usa [**addEventListener**](https://msdn.microsoft.com/library/windows/apps/Hh441145) para registrar nuestro controlador de eventos para el evento [**click**](https://msdn.microsoft.com/library/windows/apps/Hh441312). Agrega este código después de llamar a [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975).

```javascript
   app.onactivated = function (args) {
           // Omitted code
           if (isFirstActivation) {
              document.addEventListener("visibilitychange", onVisibilityChanged);
              args.setPromise(WinJS.UI.processAll());

              // Add your code to retrieve the button and register the event handler.
              var helloButton = document.getElementById("helloButton");
              helloButton.addEventListener("click", buttonClickHandler, false);
            }

```    



Ejecuta la aplicación. Cuando escribes tu nombre en el cuadro de texto y haces clic en el botón, la aplicación muestra un saludo personalizado.

**Nota**   Si te preguntas por qué usamos [addEventListener](https://msdn.microsoft.com/library/windows/apps/Hh441145) para registrar nuestro evento en código en lugar de configurar el evento [onclick](https://msdn.microsoft.com/library/windows/apps/Hh441312) en nuestro HTML, consulta [Codificar aplicaciones básicas](https://msdn.microsoft.com/library/windows/apps/Hh780660), donde encontrarás una explicación más detallada.

## <a name="step-6-add-a-windows-library-for-javascript-control"></a>Paso 6: Agregar un control de la Biblioteca de Windows para JavaScript


Además de los controles HTML estándar, tu aplicación puede usar cualquiera de los controles de la [Biblioteca de Windows para JavaScript](https://msdn.microsoft.com/library/windows/apps/BR229782), como los controles [WinJS.UI.DatePicker](https://msdn.microsoft.com/library/windows/apps/BR211681), [WinJS.UI.FlipView](https://msdn.microsoft.com/library/windows/apps/BR211711), [WinjS.UI.ListView](https://msdn.microsoft.com/library/windows/apps/BR211837) y [WinJS.UI.Rating](https://msdn.microsoft.com/library/windows/apps/BR211895).

A diferencia de los controles HTML, los controles de WinJS no tienen elementos de marcado dedicados; así, por ejemplo, no puedes agregar un elemento `<rating />` para crear un control [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895). Para agregar un control de WinJS, crea un elemento **div** y usa el atributo [data-win-control](https://msdn.microsoft.com/library/windows/apps/Hh440969) para especificar el tipo de control que quieras. Para agregar un control **Rating**, establece el atributo en "WinJS.UI.Rating".

**Agrega un control Rating a la aplicación.**

1.  En el archivo index.html, agrega un elemento [label](https://msdn.microsoft.com/library/windows/apps/Hh453321) y un control [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895) después de `greetingOutput` **div**.

```html
    <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
    </body>
```

2.  Ejecuta la aplicación en el equipo local. Observa el nuevo control [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895).

   ![La aplicación Hello world con un control de la Biblioteca de Windows para JavaScript](images/helloworld-4-winjs.png)

> Para que se cargue **Rating**, la página debe llamar a [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975). Puesto que la aplicación está usando una de las plantillas de Visual Studio, tu archivo main.js ya incluye una llamada a **WinJS.UI.processAll**, como se describió antes, de modo que no es necesario agregar más código.

Ahora mismo, al hacer clic en el control **Rating**, se cambia la clasificación, pero no ocurre nada más. Vamos a usar un controlador de eventos para que cuando el usuario cambie la clasificación ocurra algo.

## <a name="step-7-register-an-event-handler-for-a-windows-library-for-javascript-control"></a>Paso 7: Registrar un controlador de eventos para un control de la biblioteca de Windows para JavaScript


Registrar un controlador de eventos para un control de WinJS es algo distinto de registrar un controlador de eventos para un control HTML estándar. Antes hemos mencionado que el controlador de eventos **onactivated** llama al método **WinJS.UI.processAll** para inicializar WinJS en el marcado. La llamada **WinJS.UI.processAll** está incluida en una llamada al método **setPromise**, de la siguiente manera:

```javascript
            args.setPromise(WinJS.UI.processAll());           
```

Si **Rating** fuese un control HTML estándar, podrías agregar el controlador de eventos después de esta llamada a **WinJS.UI.processAll**. Pero resulta algo más complicado cuando se trata de un control de WinJS, como nuestro **Rating**. Dado que **WinJS.UI.processAll** crea el control **Rating**, no podemos agregar el controlador de eventos a **Rating** hasta después de que **WinJS.UI.processAll** haya finalizado su procesamiento.

Si **WinJS.UI.processAll** fuese un método típico, podríamos registrar el controlador de eventos **Rating** justo después de llamarlo. Pero el método **WinJS.UI.processAll** es asincrónico, por lo que cualquier código que le siga se puede ejecutar antes de que **WinJS.UI.processAll** se complete. Entonces, ¿qué debemos hacer? Usamos un objeto [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) para recibir una notificación cuando **WinJS.UI.processAll** se complete.

Como todos los métodos asincrónicos de WinJS, **WinJS.UI.processAll** devuelve un objeto **Promise**. Un objeto **Promise** es la "promesa" de que algo ocurrirá en el futuro; cuando tal cosa ocurra, se habrá completado el objeto **Promise**.

Los objetos [Promise](https://msdn.microsoft.com/library/windows/apps/BR211867) tienen un método [then](https://msdn.microsoft.com/library/windows/apps/BR229728) que toma una función "completed" como parámetro. El objeto **Promise** llama a esta función cuando se completa.

Si agregas el código a una función "completed" y la pasas al método **then** del objeto **Promise**, puedes estar seguro de que el código se ejecutará después de que se complete **WinJS.UI.processAll**.

**Mostrar el valor de clasificación que selecciona el usuario**

1.  En el archivo index.html, crea un elemento [**div**](https://msdn.microsoft.com/library/windows/apps/Hh453133) para mostrar el valor de clasificación y asígnale el **id** "ratingOutput".

```html
        <body class="win-type-body">
        <h1>Hello, world!</h1>
        <p>What's your name?</p>
        <input id="nameInput" type="text" />
        <button id="helloButton">Say "Hello"</button>
        <div id="greetingOutput"></div>
        <label for="ratingControlDiv">
            Rate this greeting:
        </label>
        <div id="ratingControlDiv" data-win-control="WinJS.UI.Rating">
        </div>
        <div id="ratingOutput"></div>
    </body>
```

2.  En nuestro archivo main.js, crea un controlador de eventos para el evento [change](https://msdn.microsoft.com/library/windows/apps/BR211891) del control **Rating**, denominado `ratingChanged`. El parámetro [eventInfo](https://msdn.microsoft.com/library/windows/apps/Hh465776) contiene una propiedad **detail.tentativeRating** que proporciona una nueva clasificación de usuario. Recupera este valor y muéstralo en la salida **div**.

```javascript
        function ratingChanged(eventInfo) {

            var ratingOutput = document.getElementById("ratingOutput");
            ratingOutput.innerText = eventInfo.detail.tentativeRating;
        }
```

3.  Actualiza el código del controlador de eventos [onactivated](https://msdn.microsoft.com/library/windows/apps/BR212679) que llama a [WinJS.UI.processAll](https://msdn.microsoft.com/library/windows/apps/Hh440975). Para ello, agrega una llamada al método [then](https://msdn.microsoft.com/library/windows/apps/BR229728) y pasa una función `completed`. En la función `completed`, recupera el elemento `ratingControlDiv` que hospeda el control [Rating](https://msdn.microsoft.com/library/windows/apps/BR211895). Después, usa la propiedad [winControl](https://msdn.microsoft.com/library/windows/apps/Hh770814) para recuperar el control **Rating** real. (Este ejemplo define la función `completed` en línea.)

```javascript
           args.setPromise(WinJS.UI.processAll().then(function completed() {

                    // Retrieve the div that hosts the Rating control.
                    var ratingControlDiv = document.getElementById("ratingControlDiv");

                    // Retrieve the actual Rating control.
                    var ratingControl = ratingControlDiv.winControl;

                    // Register the event handler.
                    ratingControl.addEventListener("change", ratingChanged, false);

                }));
```

4.  Si bien es correcto registrar los controladores de eventos para los controles HTML después de la llamada a [**WinJS.UI.processAll**](https://msdn.microsoft.com/library/windows/apps/Hh440975), también es correcto registrarlos dentro de la función `completed`. Para hacerlo más sencillo, sigamos adelante y movamos todos nuestros registros de controladores de eventos al controlador de eventos [**then**](https://msdn.microsoft.com/library/windows/apps/BR229728).

    Este es el controlador de eventos [**onactivated**](https://msdn.microsoft.com/library/windows/apps/BR212679) actualizado:

```javascript
    (function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;
    var isFirstActivation = true;

    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.voiceCommand) {
            // TODO: Handle relevant ActivationKinds. For example, if your app can be started by voice commands,
            // this is a good place to decide whether to populate an input field or choose a different initial view.
        }
        else if (args.detail.kind === activation.ActivationKind.launch) {
            // A Launch activation happens when the user launches your app via the tile
            // or invokes a toast notification by clicking or tapping on the body.
            if (args.detail.arguments) {
                // TODO: If the app supports toasts, use this value from the toast payload to determine where in the app
                // to take the user in response to them invoking a toast notification.
            }
            else if (args.detail.previousExecutionState === activation.ApplicationExecutionState.terminated) {
                // TODO: This application had been suspended and was then terminated to reclaim memory.
                // To create a smooth user experience, restore application state here so that it looks like the app never stopped running.
                // Note: You may want to record the time when the app was last suspended and only restore state if they've returned after a short period.
            }
        }

        if (!args.detail.prelaunchActivated) {
            // TODO: If prelaunchActivated were true, it would mean the app was prelaunched in the background as an optimization.
            // In that case it would be suspended shortly thereafter.
            // Any long-running operations (like expensive network or disk I/O) or changes to user state which occur at launch
            // should be done here (to avoid doing them in the prelaunch case).
            // Alternatively, this work can be done in a resume or visibilitychanged handler.
        }

        if (isFirstActivation) {
            // TODO: The app was activated and had not been running. Do general startup initialization here.
            document.addEventListener("visibilitychange", onVisibilityChanged);

            args.setPromise(WinJS.UI.processAll().then(function completed() {
                var ratingControlDiv = document.getElementById("ratingControlDiv");
                var ratingControl = ratingControlDiv.winControl;
                ratingControl.addEventListener("change",ratingChanged, false);
            }));

            var helloButton = document.getElementById("helloButton");
            helloButton.addEventListener("click", buttonClickHandler, false);

        }

        isFirstActivation = false;
    };

```        

    Run the app. When you select a rating value, it outputs the numeric value below the [**Rating**](https://msdn.microsoft.com/library/windows/apps/BR211895) control.

![La aplicación Hello world completada en un PC](images/helloworld-5-winjs.png)

## <a name="summary"></a>Resumen

Enhorabuena, has creado tu primera aplicación para Windows 10 y la UWP con JavaScript y HTML.

A continuación Los documentos de [WinJS](https://developer.microsoft.com/en-us/windows/develop/winjs) te ayudarán a trabajar con la Biblioteca de Windows para JavaScript.



<!--HONumber=Dec16_HO1-->


