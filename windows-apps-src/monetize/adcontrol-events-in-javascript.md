---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: Aprende a controlar los eventos de la clase AdControl.
title: Eventos de AdControl en JavaScript
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, anuncios, publicidad, AdControl, eventos, JavaScript
ms.openlocfilehash: 62363ebce006f8ad21645d907c3e63360472887d
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-events-in-javascript"></a>Eventos de AdControl en JavaScript

Los ejemplos siguientes muestran los controladores de eventos básicos para los siguientes eventos de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx): [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx), [AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) e [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx). En estos ejemplos, se da por hecho que ya se asignaron previamente los controladores de eventos para los eventos del formato HTML. Para obtener más información sobre cómo hacerlo, consulta [Ejemplo de propiedades HTML](html-properties-example.md).

En JavaScript, los eventos **AdControl** deben estar incluidos en la función [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx). Para obtener más información sobre el control de eventos en JavaScript, consulta [Codificar aplicaciones básicas (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers).

## <a name="examples"></a>Ejemplos

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#EventHandlers)]

## <a name="related-topics"></a>Temas relacionados

* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)
* [Control de errores de AdControl](adcontrol-error-handling.md)
* [Clase RoutedEventArgs](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 
