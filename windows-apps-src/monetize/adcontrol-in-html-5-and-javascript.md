---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "Aprende a usar la clase AdControl para mostrar anuncios en banner en aplicaciones JavaScript y HTML para Windows 10 (UWP), Windows 8.1 o Windows Phone 8.1."
title: AdControl en HTML 5 y JavaScript
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 509cfe00050c5b5b3997af0e2906676f946d9278

---

# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 y JavaScript

En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para mostrar anuncios en banner en una aplicación JavaScript o HTML para Windows 10 (UWP), Windows 8.1 o Windows Phone 8.1. En este tutorial no se usa **AdMediatorControl** ni la mediación de anuncios.

Para obtener un ejemplo de proyecto completo que muestre cómo agregar anuncios en banner a una aplicación JavaScript o HTML, consulta los [ejemplos de publicidad en GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Requisitos previos


* En el caso de las aplicaciones para UWP, instala el [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) con Visual Studio 2015.
* En el caso de las aplicaciones de Windows 8.1 o Windows Phone 8.1, instala [el SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) con Visual Studio 2015 o Visual Studio 2013.

> **Nota**&nbsp;&nbsp;Si has instalado Windows 10 Anniversary SDK Preview, compilación 14295 o una versión posterior con Visual Studio 2015, también debes instalar la biblioteca WinJS. Antes, esta biblioteca estaba incluida en versiones anteriores de Windows SDK para Windows 10, pero, desde la compilación 14295 de Windows 10 Anniversary SDK Preview, debe instalarse por separado. Para instalar WinJS, consulta cómo [obtener WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## <a name="code-development"></a>Programación de código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

4.  En el **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows 8.1 (JS) nativo**.

    -   Para un proyecto de Windows 8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y, después, selecciona la casilla junto a **SDK de Microsoft Advertising para Windows Phone 8.1 (JS) nativo**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **Nota**&nbsp;&nbsp;Esta imagen corresponde a la creación de un proyecto para UWP para Windows 10 en Visual Studio 2015. Si estás creando una aplicación de Windows 8.1 o Windows Phone 8.1, o si usas Visual Studio 2013, la pantalla tendrá un aspecto diferente.

5.  En el **Administrador de referencias**, haz clic en Aceptar.

6.  Abre el archivo index.html (u otro archivo html adecuado para el proyecto).

7.  En la sección **&lt;head&gt;**, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js.

  En un proyecto de UWP, agrega el código siguiente.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  En un proyecto de Windows 8.1 o Windows Phone 8.1, agrega el siguiente código.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <!-- Microsoft advertising required references -->
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

  <span/>
  >**Nota**&nbsp;&nbsp;Esta línea debe colocarse en la sección **&lt;head&gt;** después de incluir main.js. De lo contrario, se producirá un error al compilar el proyecto. Si el proyecto está destinado a Windows 8.1 o Windows Phone 8.1, el archivo HTML predeterminado del proyecto se denomina default.html en lugar de index.html, y el archivo de JavaScript predeterminado del proyecto se denomina default.js en lugar de main.js.

8.  Modifica la sección **&lt;body&gt;** en el archivo default.html (u otro archivo html adecuado para el proyecto) para que incluya el elemento div para **AdControl**. Asigna las propiedades **applicationId** y **adUnitId** de **AdControl** a los valores de prueba proporcionados en los [Valores del modo de prueba](test-mode-values.md) y ajusta el alto y ancho del control para que tenga uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

  > **Nota**&nbsp;&nbsp;Deberás reemplazar los valores de **applicationId** y **adUnitId** de prueba por valores dinámicos antes de enviar la aplicación.

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
  </div>
  ```

9.  Compila y ejecuta la aplicación para verla con un anuncio.

## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows


1.  En el panel del Centro de desarrollo, ve a la página **Monetización** &gt; **Monetizar con anuncios** correspondiente a la aplicación y [crea una unidad de Microsoft Advertising independiente](../publish/monetize-with-ads.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el identificador de unidad de anuncio y el identificador de la aplicación.

2.  En el código, reemplaza los valores de unidades de anuncio de prueba (**applicationId** y **adUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

3.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

4.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

## <a name="complete-indexhtml-for-a-sample-uwp-project"></a>Completar index.html para un proyecto de UWP de ejemplo

> [!div class="tabbedCodeSnippets"]
``` html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
 

 



<!--HONumber=Dec16_HO2-->


