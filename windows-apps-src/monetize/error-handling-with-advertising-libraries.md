---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: Aprende a controlar los errores generados por la clase AdControl en las bibliotecas de Microsoft Advertising.
title: Control de errores con las bibliotecas de Microsoft Advertising
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5e0c7e6328247e5f686b14b10c80d8aafc13e0e4

---

# Control de errores con las bibliotecas de Microsoft Advertising


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tema proporciona información básica sobre cómo controlar los errores generados por la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en las bibliotecas de Microsoft Advertising.

<span id="bkmk-javascript"/>
## Aplicaciones JavaScript y HTML

Para controlar los errores de **AdControl** en una aplicación de JavaScript:

1.  Asignar el evento **onErrorOccurred** a un controlador de eventos.

2.  Codifica el controlador de eventos.

El controlador de eventos **onErrorOccurred** está establecido en el atributo **data-win-options** para el elemento **div** de **AdControl**. En el siguiente ejemplo, el evento **onErrorOccurred** se establece mediante una función denominada **errorLogger**.

``` syntax
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

La función de control de errores es declarativo y deben ir en la [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) función.

El controlador de errores detecta el objeto de error de JavaScript cuando se produce un error de **AdControl**. El objeto de error proporciona dos argumentos al controlador de errores. Para obtener más información, consulta [propiedades especiales de error en métodos asincrónicos de Windows Runtime](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx).

Este es un ejemplo de una función de control de errores denominada **errorLogger**, que controla el evento **onErrorOccurred**.

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage + " error code: " + evt.errorCode + \n");
});
```

Consulta [Error handling in JavaScript walkthrough](error-handling-in-javascript-walkthrough.md) (Tutorial de control de errores en JavaScript) para ver un tutorial que muestra cómo controlar errores de **AdControl** en JavaScript.

<span id="bkmk-dotnet"/>
## Aplicaciones para XAML

Para controlar errores de **AdControl** en una aplicación XAML:

* Asignar el evento **ErrorOccurred** al nombre de un delegado de controlador de eventos.

* Codifica el delegado de controlador de eventos de error para que use dos parámetros: un elemento **Object** para el remitente y un objeto **AdErrorEventArgs**.

Este es un ejemplo que asigna un delegado denominado **OnAdError** al evento **ErrorOccurred**.

``` syntax
this.ErrorOccurred = OnAdError;
```

Esta es una definición de ejemplo del delegado **OnAdError** que escribe la información del error en la ventana Resultados en Visual Studio.

``` syntax
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error + " ErrorCode: " + e.ErrorCode.ToString());
}
```

Consulta [Error handling in XAML/C# walkthrough](error-handling-in-xamlc-walkthrough.md) (Tutorial de control de errores en XALM y C#) para ver un tutorial que muestra cómo controlar errores de **AdControl** en XAML y C#.

 

 



<!--HONumber=Jun16_HO4-->


