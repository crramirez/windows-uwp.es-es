---
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: Lee sobre las soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones de JavaScript y HTML.
title: Guía de solución de problemas de HTML y JavaScript
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, ADS, Advertising, AdControl, solución de problemas, HTML, JavaScript
ms.localizationpriority: medium
ms.openlocfilehash: 53c2d18c56626d4a71b4326b1ab7e292a2267dca
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174989"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>Guía de solución de problemas de HTML y JavaScript

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En este tema encontrarás soluciones a problemas comunes de desarrollo con las bibliotecas de Microsoft Advertising en aplicaciones de JavaScript y HTML.

* [HTML](#html)
  * [AdControl no aparece](#html-notappearing)
  * [La caja negra parpadea y desaparece](#html-blackboxblinksdisappears)
  * [Los anuncios no se actualizan](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [AdControl no aparece](#js-adcontrolnotappearing)
  * [La caja negra parpadea y desaparece](#js-blackboxblinksdisappears)
  * [Los anuncios no se actualizan](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Asegúrate de que la referencia de JavaScript esté presente. Sin la referencia de ad.js en la sección &lt;head&gt; (después de la referencia de default.js), **AdControl** no podrá mostrarse y se producirá un error durante la compilación.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el identificador de la aplicación y el identificador de unidad de ad que obtuvo en el centro de Partners. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  Comprueba las propiedades **height** y **width**. Estos deben establecerse en uno de los [tamaños de anuncio admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  Comprueba el posicionamiento de elementos. [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) debe estar dentro del área visible.

6.  Compruebe la propiedad **Visibility** . Esta propiedad no debe establecerse en "collapsed" ni "hidden". Esta propiedad puede establecerse en línea (como se muestra a continuación) o en una hoja de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  Comprueba la propiedad **position**. La propiedad "position" debe establecerse en un valor adecuado en función de las otras propiedades del elemento (por ejemplo, los márgenes en el elemento primario y el índice z). Esta propiedad puede establecerse en línea (como se muestra a continuación) o en una hoja de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  Comprueba la propiedad **z-index**. La propiedad **z-index** debe establecerse en un valor lo suficiente alto para que **AdControl** siempre aparezca encima de otros elementos. Esta propiedad puede establecerse en línea (como se muestra a continuación) o en una hoja de estilos externa.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  Comprueba las hojas de estilos externas. Si las propiedades en el elemento **AdControl** se establecen mediante una hoja de estilos externa, asegúrate de todas las propiedades anteriores se establezcan correctamente.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. Comprueba el elemento primario de **AdControl**. Si **AdControl** reside en un elemento primario, el elemento primario debe estar activo y visible.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. Asegúrate de que **AdControl** no esté oculto en la ventanilla. **AdControl** debe ser visible para que los anuncios se muestren correctamente.

12. Los valores activos para [ApplicationId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) y [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) no deben probarse en el emulador. Para asegurarse de que el **control** está funcionando según lo previsto, use los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationID** y **AdUnitId**.

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos de la sección [AdControl no aparece](#html-notappearing) anterior.

2.  Controla el evento **onErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y qué tipo de error se ha producido. Para leer más detalles, consulta [Error handling in JavaScript walkthrough](error-handling-in-javascript-walkthrough.md) (Tutorial de control de errores en JavaScript).

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  **AdControl** se comporta con normalidad. De manera predeterminada, **AdControl** se contraerá cuando no pueda mostrar un anuncio. Si otros elementos son elementos secundarios del mismo elemento principal, pueden moverse para rellenar el espacio de **AdControl** contraído y expandirse cuando se realice la siguiente solicitud.

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Los anuncios no se actualizan

1.  Compruebe la propiedad **isAutoRefreshEnabled** . Esta propiedad opcional está establecida en true de manera predeterminada. Cuando se establece en false, se debe usar el método **Refresh** para recuperar otro anuncio.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  Comprueba las llamadas al método **refresh**. Al usar la actualización automática, no se puede usar **Actualizar** para recuperar otro anuncio. Al usar la actualización manual, debe llamarse a **refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    En este ejemplo se muestra cómo usar el método **refresh**. El siguiente código HTML muestra un ejemplo de cómo crear instancias de **AdControl** con **isAutoRefreshEnabled** establecido en false.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    En este ejemplo se demuestra cómo usar la función **refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl no aparece

1.  Asegúrate de que la funcionalidad **Internet (Client)** esté seleccionada en Package.appxmanifest.

2.  Asegúrate de crear la instancia de **AdControl**. Si no se creó la instancia de **AdControl**, no estará disponible.

    Los siguientes fragmentos de código muestran un ejemplo de creación de instancias de **AdControl**. Este código HTML muestra un ejemplo de cómo configurar la interfaz de usuario para **AdControl**

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    El siguiente código de JavaScript muestra un ejemplo de creación de instancias de **AdControl**

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  Comprueba el elemento primario. El ** &lt; div &gt; ** primario debe estar correctamente asignado, activo y visible.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  Comprueba el id. de aplicación y el id. de unidad de anuncios. Estos identificadores deben coincidir con el identificador de la aplicación y el identificador de unidad de ad que obtuvo en el centro de Partners. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  Comprueba el elemento primario de **AdControl**. El elemento primario debe estar activo y visible.

6.  Los valores activos para **ApplicationId** y **AdUnitId** no deben probarse en el emulador. Para asegurarse de que el **control** está funcionando según lo previsto, use los [valores de prueba](set-up-ad-units-in-your-app.md#test-ad-units) para **ApplicationID** y **AdUnitId**.

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>La caja negra parpadea y desaparece

1.  Vuelve a comprobar todos los pasos en la sección [AdControl no aparece](#js-adcontrolnotappearing).

2.  Controla el evento **onErrorOccurred** y usa el mensaje que se pasa al controlador de eventos para determinar si se produjo un error y qué tipo de error se ha producido. Para leer más detalles, consulta [Error handling in JavaScript walkthrough](error-handling-in-javascript-walkthrough.md) (Tutorial de control de errores en JavaScript).

    En este ejemplo se muestra cómo implementar un controlador de error que informa mensajes de error. Este fragmento de código HTML proporciona un ejemplo de cómo configurar la interfaz de usuario para mostrar mensajes de error.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    En este ejemplo se muestra cómo crear instancias de **AdControl**. Esta función se insertará en el archivo app.onactivated.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    En este ejemplo se muestra cómo informar de errores. Esta función se insertará debajo de la función automática en el archivo default.js.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    El error más común que provoca una caja negra es "No ad available". Este error significa que no hay ningún anuncio disponible para devolver desde la solicitud.

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>Los anuncios no se actualizan

1.  Comprueba si la propiedad [IsAutoRefreshEnabled](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) de tu **AdControl** está establecida en false. De forma predeterminada, esta propiedad opcional está establecida en **true**. Cuando se establece en **false**, se debe usar el método **Refresh** para recuperar otro anuncio.

2.  Compruebe las llamadas al método [Refresh](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) . Al usar la actualización automática (**IsAutoRefreshEnabled** es **true**), no puede usarse **Refresh** para recuperar otro anuncio. Al usar la actualización manual (**IsAutoRefreshEnabled** es **false**), debe llamarse a **Refresh** solo después de un mínimo de 30 a 60 segundos, en función de la conexión de datos actual del dispositivo.

    En este ejemplo se muestra cómo crear el elemento **div** para **AdControl**.

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    En este ejemplo se muestra cómo usar la función **Refresh**.

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** se comporta con normalidad. A veces el mismo anuncio aparecerá más de una vez en una fila, lo que da la apariencia de que los anuncios no se actualizan.

 

 