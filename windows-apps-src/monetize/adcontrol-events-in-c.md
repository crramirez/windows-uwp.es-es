---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Aprende a controlar los eventos de la clase AdControl.
title: Eventos de AdControl en C#
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: e25e0f915c0b9b6ec2423d2a95386b45b4502253

---

# <a name="adcontrol-events-in-c"></a>Eventos de AdControl en C\# #  


Los ejemplos siguientes muestran los controladores de eventos básicos para los siguientes eventos de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx): [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) e [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx). En estos ejemplos, se da por hecho que ya se asignaron previamente los controladores de eventos para los eventos del código XAML. Para obtener más información sobre cómo hacerlo, consulta [Ejemplo de propiedades XAML](xaml-properties-example.md).

Para obtener más información sobre el control de eventos en C#, consulta [Introducción a eventos y eventos enrutados (aplicaciones universales de Windows con C#/VB/C++ y XAML)](http://msdn.microsoft.com/library/windows/apps/hh758286).

## <a name="examples"></a>Ejemplos

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
* [Control de errores de AdControl](adcontrol-error-handling.md)
* [Clase RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Dec16_HO2-->


