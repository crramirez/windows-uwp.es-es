---
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Siga este tutorial para obtener información sobre cómo detectar y controlar los errores de un control en una aplicación de JavaScript y HTML5.
title: Tutorial de control de errores en JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, control de errores, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: f6567432714fb68618510923e49f2467daefdc54
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167689"
---
# <a name="error-handling-in-javascript-walkthrough"></a>Tutorial de control de errores en JavaScript

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tutorial se muestra cómo detectar errores relacionados con ad en la aplicación de JavaScript. En este tutorial se usa un [control AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) para mostrar un banner, pero los conceptos generales de también se aplican a los anuncios intersticiales y a los anuncios nativos.

En estos ejemplos se supone que tiene una aplicación de JavaScript que contiene una **AdControl**. Para obtener instrucciones paso a paso que muestran cómo agregar un objeto **AdControl** a la aplicación, consulta [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md). Para obtener un ejemplo de proyecto completo que muestre cómo agregar anuncios en banner a una aplicación JavaScript o HTML, consulta los [ejemplos de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).

1.  En el archivo default.html, agrega un valor para el evento **onErrorOccurred** en el que definas la propiedad **data-win-options** en el elemento **div** para el objeto **AdControl**. Encuentra el siguiente código en el archivo default.html.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Después del atributo **adUnitId**, agrega el valor para el evento **onErrorOccurred**.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  Crea un elemento **div** que mostrará el texto para que puedas ver los mensajes que se generen. Para ello, agrega el siguiente código después del elemento **div** para **MiAnuncio**.
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Crea un objeto **AdControl** que desencadene un evento de error. Solo puede haber un id. de la aplicación para todos los objetos **AdControl** de una aplicación. Por lo tanto, al crear uno adicional con un id. de la aplicación diferente, se desencadenará un error en tiempo de ejecución. Para ello, después de las secciones **div** anteriores que hayas agregado, agrega el siguiente código al cuerpo de la página default.html.
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  En el archivo default.js del proyecto, después de la función de inicialización predeterminada, agregarás el controlador de eventos para **errorLogger**. Desplázate hasta el final del archivo y, después del último punto y coma, colocarás el siguiente código.
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Compila y ejecuta el archivo. Verás el anuncio original desde la aplicación de muestra que compilaste anteriormente y el texto que describe el error bajo el anuncio. No verás el anuncio con el identificador de **liveAd**.

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising)