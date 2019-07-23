---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Crear una aplicación "Hello, world" (JS)
description: Este tutorial le enseña a usar JavaScript y HTML para crear una aplicación sencilla &\#0034;Hello, world&\#0034; para la Plataforma universal de Windows (UWP) en Windows 10.
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b5423c9aae607d4f6ffe14b755c8f73e013d8b6
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820978"
---
# <a name="create-a-hello-world-app-js"></a>Crear una aplicación "Hello, world" (JS)

Este tutorial le enseña a usar JavaScript y HTML para crear una aplicación sencilla "Hello, world" para la Plataforma universal de Windows (UWP) en Windows 10. Con un único proyecto en Microsoft Visual Studio, puede compilar una aplicación que se ejecute en cualquier dispositivo Windows 10.

> [!NOTE]
> En este tutorial se usa Visual Studio Community 2019. Si usa otra versión de Visual Studio, es posible que tenga una apariencia un poco diferente.


Aquí aprenderás a:

-   Creación de un nuevo proyecto de **Visual Studio 2019** diseñado para **Windows 10** y **UWP**.
-   Agregar contenido HTML y JavaScript.
-   Ejecutar el proyecto en el escritorio local en Visual Studio

## <a name="before-you-start"></a>Antes de comenzar...

-   [¿Qué es una aplicación para UWP?](universal-application-platform-guide.md)
-   Para realizar este tutorial, necesita Windows 10 y Visual Studio. [Prepárate](get-set-up.md).
-   También se supone que estás usando el diseño de ventana predeterminado de Visual Studio. Si cambias el diseño predeterminado, puedes restablecerlo en el menú **Ventana** con el comando **Restablecer diseño de la ventana**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Paso 1: Crear un nuevo proyecto en Visual Studio.

1.  Inicia Visual Studio 2019.

2.  En el menú **Archivo**, selecciona **Nuevo > Proyecto...** para abrir el cuadro de diálogo *Crear nuevo proyecto*.

3.  Selecciona **Aplicación en blanco (Windows universal) de JavaScript** y luego **Siguiente**.

    (Si no ve ninguna plantilla Universal, es posible que falten los componentes para crear aplicaciones para UWP). Puedes repetir el proceso de instalación y agregar soporte técnico para UWP al hacer clic en **Abrir el instalador de Visual Studio** en el diálogo *Crear nuevo proyecto*. Consulte [Prepárate](get-set-up.md)

4.  En el cuadro de diálogo *Configura tu nuevo proyecto*, escribe "HelloWorld" como **nombre del proyecto** y selecciona **Crear**.

> [!NOTE]
> Si es la primera vez que usa Visual Studio, es posible que se le solicite habilitar el **Modo desarrollador** en el diálogo de configuración. El modo de desarrollador es una opción de configuración especial que habilita determinadas funciones, como permiso para ejecutar aplicaciones directamente, en lugar de solo desde la Store. Para obtener más información, lea [Habilitar el dispositivo para el desarrollo](enable-your-device-for-development.md). Para continuar con esta guía, seleccione **Modo desarrollador**, haga clic en **Sí** y cierre el diálogo.

 ![Diálogo para activar el modo de desarrollador.](images/win10-cs-00.png)

5.  Se visualiza el cuadro de diálogo de versión mínima/de destino. La configuración predeterminada es correcta para este tutorial, por lo tanto, seleccione **Aceptar** para crear el proyecto.

    ![La ventana de Explorador de soluciones](images/win10-cs-02.png)

6.  Cuando se abra el nuevo proyecto, sus archivos se muestran en el panel **Explorador de soluciones** de la derecha. Es posible que debas elegir la pestaña **Explorador de soluciones** en lugar de la pestaña **Propiedades** para ver los archivos.

    ![La ventana de Explorador de soluciones](images/win10-js-02.png)

Aunque **Aplicación vacía (Universal Window)** es una plantilla mínima, contiene muchos archivos. Estos archivos son esenciales para todas las aplicaciones para UWP que usan JavaScript. Todos los proyectos que crees en Visual Studio contendrán estos archivos.


### <a name="whats-in-the-files"></a>¿Qué hay en los archivos?

Para ver y editar un archivo de tu proyecto, haz doble clic en el archivo en el **Explorador de soluciones**.

*default.css*

-  La hoja de estilos predeterminada que usa la aplicación.

*main.js*

- El archivo JavaScript predeterminado. Se hace referencia en el archivo index.html.

*index.html*

- La página web de la aplicación, que se carga y muestra cuando se inicia la aplicación.

*Un conjunto de imágenes de logotipo*
-   Assets/Square150x150Logo.scale-200.png representa tu aplicación en el menú Inicio.
-   Assets/StoreLogo.png representa su aplicación en la Microsoft Store.
-   Assets/SplashScreen.scale-200.png es la pantalla de presentación que se muestra cuando se inicia la aplicación.

## <a name="step-2-adding-a-button"></a>Paso 2: Agregar un botón

Haga clic en *index.html* para seleccionarlo en el editor y cambie el código HTML que contiene para que ponga:

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

Este código HTML hace referencia al archivo *main.js* que contendrá nuestro JavaScript y, después, agrega una sola línea de texto y un solo botón en el cuerpo de la página web. Se proporciona un *id.* al botón para que el JavaScript pueda hacer referencia a él.


## <a name="step-3-adding-some-javascript"></a>Paso 3: Agregar código JavaScript

Ahora agregaremos el código JavaScript. Haga clic en *main.js* para seleccionarlo y agregue lo siguiente:

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

Este código JavaScript indica dos funciones. La función *window.onload* se llama automáticamente cuando se muestra *index.html*. Encuentra el botón (mediante el id. declarado) y agrega un controlador onclick: el método que se llamará cuando se haga clic en el botón.

La segunda función, *sayHello()* , crea y muestra un diálogo. Esta es muy similar a la función *Alert()* que recordará del desarrollo con JavaScript anterior.


## <a name="step-4-run-the-app"></a>Paso 4: Ejecute la aplicación.

Ahora puede ejecutar la aplicación presionando F5. La aplicación se cargará y se mostrará la página web. Haga clic en el botón, a continuación aparecerá el elemento emergente del cuadro de diálogo.

 ![Ejecución del proyecto](images/win10-js-05.png)



## <a name="summary"></a>Resumen


Enhorabuena, ha creado una aplicación JavaScript para Windows 10 y la UWP. Este es un ejemplo extremadamente sencillo; sin embargo, ya puede empezar a agregar sus bibliotecas de JavaScript y marcos favoritos para crear su propia aplicación. Y como se trata de una aplicación para UWP, puede publicarla en la Store. Para obtener ejemplos de cómo agregar marcos de terceros, consulte estos proyectos:

* [Un sencillo juego en 2D para UWP para Microsoft Store, escrito en JavaScript y CreateJS](get-started-tutorial-game-js2d.md)
* [Un juego en 3D para UWP para Microsoft Store, escrito en JavaScript y threeJS](get-started-tutorial-game-js3d.md)
