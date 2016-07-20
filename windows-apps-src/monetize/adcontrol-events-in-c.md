---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Aprende a controlar los eventos de la clase AdControl.
title: Eventos de AdControl en C#
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: f92cbbb00a064ce7569d44ad952838df4d21ac8c

---

# Eventos de AdControl en C\# #  


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En los siguientes ejemplos se muestra cómo controlar los eventos de la clase [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx). En estos ejemplos, se da por hecho que se asignaron previamente los controladores de eventos para los eventos **AdControl** en XAML. Para obtener más información sobre cómo hacerlo, consulta [Ejemplo de propiedades XAML](xaml-properties-example.md).

Para obtener más información sobre el control de eventos en C#, consulta [Introducción a eventos y eventos enrutados (aplicaciones universales de Windows con C#/VB/C++ y XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## Ejemplos


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## Temas relacionados

* [Ejemplos de publicidad en GitHub](http://aka.ms/githubads)
* [Control de errores de AdControl](adcontrol-error-handling.md)
* [Clase RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Jul16_HO2-->


