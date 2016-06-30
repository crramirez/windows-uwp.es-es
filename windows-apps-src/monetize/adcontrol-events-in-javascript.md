---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Aprende a controlar los eventos de la clase AdControl.
title: Eventos de AdControl en JavaScript
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 5a47915b0dd2792ed50cc5d556b1181ee2c259e1


---

# Eventos de AdControl en JavaScript


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En los siguientes ejemplos se muestra cómo controlar los eventos de la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). En estos ejemplos, se da por hecho que se asignaron previamente los controladores de eventos para los eventos **AdControl**. Para obtener más información sobre cómo hacerlo, consulta [Ejemplo de propiedades HTML](html-properties-example.md).

En JavaScript, los eventos **AdControl** deben estar incluidos en la función [MarkSupportedForProcessing](http://msdn.microsoft.com/en-us/library/windows/apps/Hh967819.aspx). Para obtener más información sobre el control de eventos en JavaScript, consulta [Codificar aplicaciones básicas (HTML)](https://msdn.microsoft.com/en-us/library/windows/apps/hh780660.aspx#adding-event-handlers).

## Ejemplos

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## Temas relacionados

* [Ejemplos de publicidad en GitHub](http://aka.ms/githubads)
* [Control de errores de AdControl](adcontrol-error-handling.md)
* [Clase RoutedEventArgs](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Jun16_HO4-->


