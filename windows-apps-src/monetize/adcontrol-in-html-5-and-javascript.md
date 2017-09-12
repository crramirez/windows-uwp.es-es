---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: Aprende a usar la clase AdControl para mostrar anuncios en banner en aplicaciones JavaScript y HTML para Windows10 (UWP), Windows8.1 o Windows Phone8.1.
title: AdControl en HTML 5 y JavaScript
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, control de anuncios, javaScript, HTML
ms.openlocfilehash: 44417516d773ea4faf103f6d4cdaf0bc8f290921
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-html-5-and-javascript"></a>AdControl en HTML 5 y JavaScript

En este tutorial se muestra cómo usar la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) para mostrar anuncios en banner en una aplicación JavaScript o HTML para Windows10 (UWP), Windows8.1 o Windows Phone8.1.

Para obtener un proyecto de muestra completo que explica cómo agregar anuncios de banner a una aplicación JavaScript o HTML, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## <a name="prerequisites"></a>Requisitos previos


* Para aplicaciones para UWP: instala el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp) con Visual Studio2015 o una versión posterior.
* En el caso de las aplicaciones de Windows8.1 o Windows Phone 8.1, instala [el SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) con Visual Studio 2015 o Visual Studio 2013.

> [!NOTE]
> Si vas a agregar anuncios de banners a una aplicación para UWP y has instalado el SDK de Windows 10 versión 10.0.14393 (Actualización de aniversario) o una versión posterior del Windows SDK, también debes instalar la biblioteca WinJS. Antes, esta biblioteca estaba incluida en versiones anteriores de Windows SDK para Windows 10, pero desde la versión 10.0.14393 del SDK de Windows 10 (Actualización de aniversario), debe instalarse por separado. Para instalar WinJS, consulta cómo [obtener WinJS](http://try.buildwinjs.com/download/GetWinJS/).

## <a name="code-development"></a>Programación de código

1. En Visual Studio, abre el proyecto o crea uno nuevo.

2. Si el destino del proyecto es **Cualquier CPU**, actualiza el proyecto para que use una salida de compilación específica por arquitectura (por ejemplo, **x86**). Si el destino del proyecto es **Cualquier CPU**, no podrás agregar una referencia a la biblioteca de publicidad de Microsoft correctamente a través de los siguientes pasos. Para obtener más información, consulta [Errores de referencia derivados de orientar el proyecto a Cualquier CPU](known-issues-for-the-advertising-libraries.md#reference_errors).

3.  Desde la ventana del **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y, a continuación, selecciona **Agregar referencia...**

4.  En el **Administrador de referencias**, selecciona una de las siguientes referencias según el tipo de proyecto:

    -   Para un proyecto de la Plataforma universal de Windows (UWP): expande **Windows Universal**, haz clic en **Extensiones**y luego selecciona la casilla junto a **SDK de Microsoft Advertising para JavaScript** (versión 10.0).

    -   Para un proyecto de Windows 8.1: expande **Windows 8.1**, haz clic en **Extensiones** y luego selecciona la casilla junto a **SDK de Microsoft Advertising para Windows 8.1 (JS) nativo**.

    -   Para un proyecto de Windows Phone8.1: expande **Windows Phone 8.1**, haz clic en **Extensiones** y después selecciona la casilla junto a **SDK de Microsoft Advertising para Windows Phone 8.1 (JS) nativo**.

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

5.  En el **Administrador de referencias**, haz clic en Aceptar.

6.  Abre el archivo index.html (u otro archivo html adecuado para el proyecto).

7.  En la sección **&lt;head&gt;**, después de las referencias de JavaScript del proyecto de default.css y main.js, agrega la referencia a ad.js.

    En un proyecto de UWP, agrega el código siguiente.

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    En un proyecto de Windows8.1 o Windows Phone8.1, agrega el siguiente código.

    ``` HTML
    <!-- Advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > [!NOTE]
    > Esta línea debe colocarse en la sección **&lt;head&gt;** después de incluir main.js. De lo contrario, se producirá un error al compilar el proyecto. Si el proyecto está destinado a Windows 8.1 o Windows Phone 8.1, el archivo HTML predeterminado del proyecto se denomina default.html en lugar de index.html, y el archivo de JavaScript predeterminado del proyecto se denomina default.js en lugar de main.js.

8.  Modifica la sección **&lt;body&gt;** en el archivo default.html (u otro archivo html adecuado para el proyecto) para que incluya el elemento div para **AdControl**. Asigna las propiedades **applicationId** y **adUnitId** de **AdControl** a los valores de prueba proporcionados en [Valores del modo de prueba](test-mode-values.md). También ajusta el alto y el ancho del control para que se corresponda con uno de los [Tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!NOTE]
    > Cada **AdControl** tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control y cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación*. En estos pasos, asignas los valores del Id. de la unidad de anuncios de prueba y del Id. de aplicación a tu control. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Antes de publicar la aplicación en la Tienda, debes [reemplazar estos valores de prueba con valores dinámicos](#release) desde el Centro de desarrollo de Windows.

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  Compila y ejecuta la aplicación para verla con un anuncio.

El siguiente ejemplo muestra el archivo index.html completo para una aplicación simple para UWP.

``` HTML
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
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>Publicar la aplicación con anuncios dinámicos mediante el Centro de desarrollo de Windows

1.  En el panel del Centro de desarrollo, ve a la página [Monetizar con anuncios](../publish/monetize-with-ads.md) correspondiente a la aplicación y [crea una unidad de anuncios](../monetize/set-up-ad-units-in-your-app.md). Especifica el tipo de unidad de anuncio **Banner**. Anota el id. de la unidad de anuncios y el id. de la aplicación.

2. Si la aplicación es una aplicación para UWP para Windows10, tienes la opción de habilitar la mediación de anuncios para **AdControl** mediante la configuración de las opciones en la sección [Mediación de anuncios](../publish/monetize-with-ads.md#mediation) de la página [Monetizar con anuncios](../publish/monetize-with-ads.md). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft.

3.  En el código, reemplaza los valores de unidades de anuncio de prueba (**applicationId** y **adUnitId**) con los valores dinámicos generados en el Centro de desarrollo.

4.  [Envía la aplicación](../publish/app-submissions.md) a la Tienda desde el panel del Centro de desarrollo.

5.  Revisa los [informes de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.             

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios objetos **AdControl** en una sola aplicación (por ejemplo, cada página de la aplicación puede hospedar un objeto **AdControl** diferente). En este escenario, te recomendamos que asignes una unidad de anuncio diferente para cada control. Con unidades de anuncio diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/monetize-with-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas un unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
* [Configurar unidades de anuncios para la aplicación](../monetize/set-up-ad-units-in-your-app.md)
