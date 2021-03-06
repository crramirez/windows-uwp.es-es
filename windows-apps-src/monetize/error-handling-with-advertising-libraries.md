---
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Aprende a controlar los errores generados por la clase AdControl en las bibliotecas de Microsoft Advertising.
title: Controlar errores de anuncios
ms.date: 02/18/2020
ms.topic: article
keywords: 'Windows 10, UWP, anuncios, publicidad, control de errores, JavaScript, XAML, c #'
ms.localizationpriority: medium
ms.openlocfilehash: 2a1a82c9977bfbe61712d39e4d23fdd68cd598ae
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171559"
---
# <a name="handle-ad-errors"></a>Controlar errores de anuncios

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cada una de las clases [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol),  [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad)y [NativeAdsManagerV2](/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) tiene un evento **ErrorOccurred** que se genera si se produce un error relacionado con ad. El código de la aplicación puede controlar este evento y examinar las propiedades [ErrorCode](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) y [errorMessage](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) del objeto args del evento para ayudar a determinar la causa del error.

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>Aplicaciones para XAML

Para controlar los errores relacionados con ad en una aplicación XAML:

1. Asigne el evento **ErrorOccurred** del objeto **AdControl**, **InterstitialAd**o **NativeAdsManagerV2** al nombre de un delegado de controlador de eventos.

2. Codifica el delegado de controlador de eventos de error para que use dos parámetros: un elemento **Object** para el remitente y un objeto [AdErrorEventArgs](/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs).

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

Este es un ejemplo que asigna un controlador de eventos denominado **errorLogger** al evento **ErrorOccurred** de un objeto **AdControl** .

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

La función de control de errores es declarativo y deben ir en la [markSupportedForProcessing](/previous-versions/windows/apps/hh967819(v=win.10)) función.

El controlador de errores detecta el objeto de error de JavaScript cuando se produce un error. El objeto de error proporciona dos argumentos al controlador de errores. Para obtener más información, consulta [propiedades especiales de error en métodos asincrónicos de Windows Runtime](/scripting/jswinrt/special-error-properties-from-asynchronous-windows-runtime-methods).

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