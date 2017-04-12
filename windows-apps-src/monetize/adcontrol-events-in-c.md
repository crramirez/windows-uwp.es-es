---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: Aprende a controlar los eventos de la clase AdControl.
title: Eventos de AdControl en C#
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, eventos
ms.openlocfilehash: a0b1130f8177166eef5b524ac4c4f1b065176eeb
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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

 

 
