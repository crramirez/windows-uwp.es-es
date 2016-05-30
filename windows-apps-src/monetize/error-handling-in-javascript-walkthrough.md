---
author: mcleanbyron
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: Aprende a detectar errores de AdControl en la aplicación.
title: Tutorial de control de errores en JavaScript

---

# Tutorial de control de errores en JavaScript


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tema muestra cómo detectar errores de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en la aplicación.

En estos ejemplos se da por hecho que tienes una aplicación JavaScript o HTML que contiene un objeto **AdControl**. Para obtener instrucciones paso a paso que muestran cómo agregar un objeto **AdControl** a la aplicación, consulta [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md). Para un proyecto de muestra completo que muestra cómo agregar anuncios en banners a una aplicación JavaScript o HTML, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

1.  En el archivo default.html, agrega un valor para el evento **onErrorOccurred** en el que definas la propiedad **data-win-options** en el elemento **div** para el objeto **AdControl**. Encuentra el siguiente código en el archivo default.html.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

    Después de la propiedad **adUnitId**, agrega el valor para el evento **onErrorOccurred**.

    ``` syntax
    onErrorOccurred: errorLogger
    ```

    Este es el código completo para el elemento **div**.

    ``` syntax
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270', onErrorOccurred: errorLogger}">
    </div>
    ```

2.  Crea un elemento **div** que mostrará el texto para que puedas ver los mensajes que se generen. Para ello, agrega el siguiente código después del elemento **div** para **MiAnuncio**.

    ``` syntax
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  Crea un objeto **AdControl** que desencadene un evento de error. Solo puede haber un id. de la aplicación para todos los objetos **AdControl** de una aplicación. Por lo tanto, al crear uno adicional con un id. de la aplicación diferente, se desencadenará un error en tiempo de ejecución. Para ello, después de las secciones **div** anteriores que hayas agregado, agrega el siguiente código al cuerpo de la página default.html.

    ``` syntax
    <!-- since only one applicationId can be used, the following ad control will fire an error event -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000',
        adUnitId: '10865270', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  En el archivo default.js del proyecto, después de la función de inicialización predeterminada, agregarás el controlador de eventos para **errorLogger**. Desplázate hasta el final del archivo y, después del último punto y coma, colocarás el siguiente código.

    ``` syntax
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  Compila y ejecuta el archivo.

Verás el anuncio original desde la aplicación de muestra que compilaste anteriormente y el texto que describe el error bajo el anuncio. No verás el anuncio con el identificador de **liveAd**.

## Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->

