---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Crear una aplicación "Hello, world" (JS)
description: Este tutorial le enseña a usar JavaScript y HTML para crear una aplicación sencilla &\#0034;Hello, world&\#0034; para la Plataforma universal de Windows (UWP) en Windows 10.
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 88ae290bf88a21aa2846697833d099df663915f6
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492970"
---
# <a name="create-a-hello-world-app-js"></a>Creación de una aplicación "Hola mundo" (JS)

Este tutorial le enseña a usar JavaScript y HTML para crear una aplicación sencilla "Hello, world" para la Plataforma universal de Windows (UWP) en Windows 10. Con un único proyecto en Microsoft Visual Studio, puede compilar una aplicación que se ejecute en cualquier dispositivo Windows 10.

> [!NOTE]
> En este tutorial se usa Visual Studio Community 2017. Si usa otra versión de Visual Studio, es posible que tenga una apariencia un poco diferente.

> [!WARNING]
> El desarrollo de aplicaciones para UWP de JavaScript no se admite en Visual Studio 2019. Debes usar Visual Studio 2017 para desarrollar una aplicación para UWP de JavaScript.

En este artículo, aprenderás a:

-   Creación de un nuevo proyecto de **Visual Studio 2017** diseñado para **Windows 10** y **UWP**.
-   Agregar contenido HTML y JavaScript.
-   Ejecutar el proyecto en el escritorio local de Visual Studio.

## <a name="before-you-start"></a>Antes de empezar

-   [¿Qué es una aplicación para UWP?](universal-application-platform-guide.md)
-   Para realizar este tutorial, necesita Windows 10 y Visual Studio. [Prepárate](get-set-up.md).
-   También se supone que estás usando el diseño de ventana predeterminado de Visual Studio. Si cambias el diseño predeterminado, puedes restablecerlo en el menú **Ventana** con el comando **Restablecer diseño de la ventana**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Paso 1: Crear un nuevo proyecto en Visual Studio.

1.  Inicia Visual Studio 2017.

2.  En el menú **Archivo**, selecciona **Nuevo > Proyecto** para abrir el cuadro de diálogo **Crear nuevo proyecto**.

3.  Selecciona **Aplicación en blanco (Windows universal) de JavaScript** y luego **Siguiente**.

    (Si no ve ninguna plantilla Universal, es posible que falten los componentes para crear aplicaciones para UWP). Para repetir el proceso de instalación y agregar compatibilidad con UWP, selecciona **Abrir el instalador de Visual Studio** en el cuadro de diálogo **Crear un proyecto**. Consulta [Preparación](get-set-up.md).

4.  En el cuadro de diálogo **Configurar el nuevo proyecto**, escribe **HelloWorld** como **Nombre del proyecto** y, a continuación, selecciona **Crear**.

> [!NOTE]
> Si es la primera vez que usa Visual Studio, es posible que se le solicite habilitar el **Modo desarrollador** en el diálogo de configuración. El modo de desarrollador es una opción de configuración especial que habilita determinadas funciones, como permiso para ejecutar aplicaciones directamente, en lugar de solo desde la Store. Para obtener más información, lea [Habilitar el dispositivo para el desarrollo](enable-your-device-for-development.md). Para continuar con esta guía, selecciona **Modo desarrollador** y **Sí**. A continuación, cierra el cuadro de diálogo.

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
-   Assets/Square150x150Logo.scale-200.png representa tu aplicación en el menú **Inicio**.
-   Assets/StoreLogo.png representa su aplicación en la Microsoft Store.
-   Assets/SplashScreen.scale-200.png es la pantalla de presentación que se muestra cuando se inicia la aplicación.

## <a name="step-2-adding-a-button"></a>Paso 2: Agregar un botón

Selecciona **index.html** para seleccionarlo en el editor y cambia el código HTML que contiene como se indica a continuación.

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

Debe tener el siguiente aspecto.

 ![HTML del proyecto](images/win10-js-03.png)

Este código HTML hace referencia al archivo *main.js* que contendrá nuestro JavaScript y, después, agrega una sola línea de texto y un solo botón en el cuerpo de la página web. Se proporciona un *id.* al botón para que el JavaScript pueda hacer referencia a él.


## <a name="step-3-adding-some-javascript"></a>Paso 3: Agregar código JavaScript

Ahora agregaremos el código JavaScript. Selecciona **main.js** para seleccionarlo y, a continuación, agrega lo siguiente.

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

Debe tener el siguiente aspecto.

 ![JavaScript del proyecto](images/win10-js-04.png)

Este código JavaScript indica dos funciones. La función *window.onload* se llama automáticamente cuando se muestra *index.html*. Encuentra el botón (mediante el id. declarado) y agrega un controlador onclick: el método que se llamará cuando se haga clic en el botón.

La segunda función, *sayHello()* , crea y muestra un diálogo. Esta es muy similar a la función *Alert()* que recordará del desarrollo con JavaScript anterior.


## <a name="step-4-run-the-app"></a>Paso 4: Ejecute la aplicación.

Ahora puede ejecutar la aplicación presionando F5. La aplicación se cargará y se mostrará la página web. Selecciona el botón y, a continuación, se abrirá un cuadro de diálogo emergente con un mensaje.

 ![Ejecución del proyecto](images/win10-js-05.png)



## <a name="summary"></a>Resumen


Enhorabuena, ha creado una aplicación JavaScript para Windows 10 y la UWP. Este es un ejemplo extremadamente sencillo; sin embargo, ya puede empezar a agregar sus bibliotecas de JavaScript y marcos favoritos para crear su propia aplicación. Y como se trata de una aplicación para UWP, puede publicarla en la Store. Para obtener ejemplos de cómo agregar marcos de terceros, consulte estos proyectos:

* [Un sencillo juego en 2D para UWP para Microsoft Store, escrito en JavaScript y CreateJS](get-started-tutorial-game-js2d.md)
* [Un juego en 3D para UWP para Microsoft Store, escrito en JavaScript y threeJS](get-started-tutorial-game-js3d.md)
