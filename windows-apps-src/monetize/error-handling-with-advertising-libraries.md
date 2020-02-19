---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Aprende a controlar los errores generados por la clase AdControl en las bibliotecas de Microsoft Advertising.
title: Controlar errores de anuncios
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, control de errores, javascript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 1804bc6b44069dccdd92d0a33fcfd48567363a33
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463847"
---
# <a name="handle-ad-errors"></a>Controlar errores de anuncios

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://aka.ms/ad-monetization-shutdown)

Las clases [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol),  [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) y [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) tienen cada una un evento **ErrorOccurred** que se genera si se produce un error relacionado con los anuncios. El código de la aplicación puede controlar este evento y examinar las propiedades [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) y [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) del objeto de argumentos de evento para ayudar a determinar la causa del error.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>Aplicaciones para XAML

Para controlar errores relacionados con los anuncios en una aplicación XAML:

1. Asigna el evento **ErrorOccurred** del objeto **AdControl**, **InterstitialAd** o **NativeAdsManagerV2** al nombre de un delegado de controlador de eventos.

2. Codifica el delegado de controlador de eventos de error para que use dos parámetros: un elemento **Object** para el remitente y un objeto [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs).

Este es un ejemplo que asigna un delegado denominado **OnAdError** al evento **ErrorOccurred** de un objeto **AdControl** denominado *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

Esta es una definición de ejemplo del delegado **OnAdError** que escribe la información del error en la ventana Resultados en Visual Studio.

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

Consulta [Error handling in XAML/C# walkthrough](error-handling-in-xamlc-walkthrough.md) (Tutorial de control de errores en XALM y C#) para ver un tutorial que muestra cómo controlar errores de **AdControl** en XAML y C#.

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>Aplicaciones JavaScript y HTML

Para controlar los errores de **ErrorOccur** en una aplicación de JavaScript:

1.  Asignar el evento **onErrorOccurred** a un controlador de eventos.

2.  Codifica el controlador de eventos.

Este es un ejemplo que asigna un controlador de eventos denominado **errorLogger** al evento **ErrorOccurred** de un objeto **AdControl**.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

La función de control de errores es declarativo y deben ir en la [markSupportedForProcessing](https://docs.microsoft.com/previous-versions/windows/apps/hh967819(v=win.10)) función.

El controlador de errores detecta el objeto de error de JavaScript cuando se produce un error. El objeto de error proporciona dos argumentos al controlador de errores. Para obtener más información, consulta [propiedades especiales de error en métodos asincrónicos de Windows Runtime](https://docs.microsoft.com/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods).

Este es un ejemplo de una función de control de errores denominada **errorLogger**, que controla el evento **onErrorOccurred**.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

Consulta [Error handling in JavaScript walkthrough](error-handling-in-javascript-walkthrough.md) (Tutorial de control de errores en JavaScript) para ver un tutorial que muestra cómo controlar errores de **AdControl** en JavaScript.
