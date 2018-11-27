---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Crear una aplicación Hello, world (JS)
description: Este tutorial te enseña a usar JavaScript y HTML para crear una sencilla \#0034; Hola, mundo & \#0034; aplicación destinada a la plataforma Universal de Windows (UWP) en Windows 10.
ms.date: 03/06/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c5b99c95167940c1ae51dbe96a3e43dc6fb0af34
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705806"
---
# <a name="create-a-hello-world-app-js"></a>Crear una aplicación Hello, world (JS)

Este tutorial te enseña a usar JavaScript y HTML para crear una sencilla "Hello, world" aplicación destinada a la plataforma Universal de Windows (UWP) en Windows 10. Con un único proyecto en Microsoft Visual Studio, puedes crear una aplicación que se ejecute en cualquier dispositivo Windows 10.

> [!NOTE]
> En este tutorial se usa Visual Studio Community 2017. Si usas otra versión de Visual Studio, es posible que tenga una apariencia un poco diferente.


Aquí aprenderás a:

-   Crear un nuevo proyecto de **Visual Studio 2017** que está destinada a **Windows 10** y la **UWP**.
-   Agregar contenido HTML y JavaScript
-   Ejecutar el proyecto en el escritorio local de Visual Studio

## <a name="before-you-start"></a>Antes de comenzar...

-   [¿Qué es una aplicación para UWP?](universal-application-platform-guide.md).
-   Para completar este tutorial, debes tener Windows 10 y Studio2017 Visual. [Prepárate](get-set-up.md).
-   También se supone que estás usando el diseño de ventana predeterminado de Visual Studio. Si cambias el diseño predeterminado, puedes restablecerlo en el menú **Ventana** con el comando **Restablecer diseño de la ventana**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Paso 1: Creación de un nuevo proyecto en Visual Studio.

1.  Inicia Visual Studio 2017.

2.  En el menú **Archivo**, selecciona **Nuevo > Proyecto...** para abrir el cuadro de diálogo *Nuevo proyecto*.

3.  En la lista de plantillas del lado izquierdo, abre **Instalado > Plantillas > JavaScript > Windows** y, después, elige **Windows Universal** para ver la lista de plantillas de proyecto para UWP.

    (Si no ves ninguna plantilla Universal, es posible que falten los componentes para crear aplicaciones para UWP. Puedes repetir el proceso de instalación y agregar compatibilidad con UWP haciendo clic en **Abrir el instalador de Visual Studio** en el diálogo *Nuevo proyecto*. Consulta [Prepárate](get-set-up.md).

4.  Elige la plantilla **Aplicación vacía (Windows Universal)** y escribe "HelloWorld" como **Nombre**. Selecciona **Aceptar**.

    ![La ventana Nuevo proyecto](images/win10-js-01.png)

> [!NOTE]
> Si es la primera vez que usas Visual Studio, es posible que se te solicite habilitar el **Modo de desarrollador** en un diálogo de configuración. El modo de desarrollador es una opción de configuración especial que habilita determinadas funciones, como permiso para ejecutar aplicaciones directamente, en lugar de solo desde la Tienda. Para obtener más información, lee [Habilitar el dispositivo para el desarrollo](enable-your-device-for-development.md). Para continuar con esta guía, selecciona **Modo de desarrollador**, haz clic en **Sí** y cierra el diálogo.

 ![Diálogo para activar el modo de desarrollador](images/win10-cs-00.png)

5.  Se visualiza el cuadro de diálogo de versión mínima/de destino. La configuración predeterminada es correcta para este tutorial, por lo tanto, selecciona **Aceptar** para crear el proyecto.

    ![La ventana de Explorador de soluciones](images/win10-cs-02.png)

6.  Cuando se abra el nuevo proyecto, sus archivos se muestran en el panel **Explorador de soluciones** de la derecha. Es posible que debas elegir la pestaña **Explorador de soluciones** en lugar de la pestaña **Propiedades** para ver los archivos.

    ![La ventana de Explorador de soluciones](images/win10-js-02.png)

Aunque **Aplicación vacía (Universal Window)** es una plantilla mínima, contiene muchos archivos. Estos archivos son esenciales para todas las aplicaciones para UWP que usan JavaScript. Todos los proyectos que crees en Visual Studio contendrán estos archivos.


### <a name="whats-in-the-files"></a>¿Qué hay en los archivos?

Para ver y editar un archivo de tu proyecto, haz doble clic en el archivo en el **Explorador de soluciones**. 

*default.css*

-  Hoja de estilos predeterminada que usa la aplicación.

*main.js*

- Archivo JavaScript predeterminado. Se hace referencia en el archivo index.html.

*index.html*

- Página web de la aplicación, que se carga y muestra cuando se inicia la aplicación.

*Conjunto de imágenes de logotipo*
-   Assets/Square150x150Logo.scale-200.png representa tu aplicación en el menú Inicio.
-   Assets/StoreLogo.png representa tu aplicación en Microsoft Store.
-   Assets/SplashScreen.scale-200.png es la pantalla de presentación que se muestra cuando se inicia la aplicación.

## <a name="step-2-adding-a-button"></a>Paso 2: Adición de un botón

Haz clic en *index.html* para seleccionarlo en el editor y cambia el código HTML que contiene para que ponga:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

Debe tener el siguiente aspecto:

 ![HTML del proyecto](images/win10-js-03.png)

Este código HTML hace referencia al archivo *main.js* que contendrá nuestro código JavaScript y, después, agrega una sola línea de texto y un solo botón en el cuerpo de la página web. Se proporciona un *id.* al botón para que el código JavaScript pueda hacer referencia a él.


## <a name="step-3-adding-some-javascript"></a>Paso 3: Agregar código JavaScript

Ahora, agregaremos el código JavaScript. Haz clic en *main.js* para seleccionarlo y agrega lo siguiente:

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

Debe tener el siguiente aspecto:

 ![JavaScript del proyecto](images/win10-js-04.png)

Este código JavaScript declara dos funciones. La función *window.onload* se llama automáticamente cuando se muestra *index.html*. Encuentra el botón (mediante el id. declarado) y agrega un controlador onclick: el método al que se llamará cuando se haga clic en el botón.

La segunda función, *sayHello()*, crea y muestra un diálogo. Esta es muy similar a la función *Alert()*, que recordarás del desarrollo con JavaScript anterior.


## <a name="step-4-run-the-app"></a>Paso 4: Ejecutar la aplicación

Ahora puedes ejecutar la aplicación presionando F5. La aplicación se cargará y la página web se mostrará. Haz clic en el botón y aparecerá el diálogo de mensaje.

 ![Ejecución del proyecto](images/win10-js-05.png)



## <a name="summary"></a>Resumen


Enhorabuena, has creado una aplicación de JavaScript para Windows 10 y la UWP. Este es un ejemplo extremadamente sencillo; sin embargo, ya puedes empezar a agregar tus bibliotecas de JavaScript y marcos favoritos para crear tu propia aplicación. Y como se trata de una aplicación para UWP, puedes publicarla en la Tienda. Para obtener ejemplos de cómo agregar marcos de terceros, consulta estos proyectos:

* [Un sencillo juego en 2D para UWP para Microsoft Store, escrito en JavaScript y CreateJS](get-started-tutorial-game-js2d.md)
* [Un juego en 3D para UWP para Microsoft Store, escrito en JavaScript y threeJS](get-started-tutorial-game-js3d.md)


