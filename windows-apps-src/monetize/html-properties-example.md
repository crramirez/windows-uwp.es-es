---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "Aprende cómo asignar propiedades de **AdControl** en HTML."
title: Ejemplo de propiedades HTML de AdControl
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, AdControl, HTML, propiedades
ms.openlocfilehash: efc94b723b3bfe97b35484882ae784e9ef4d6807
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-html-properties-example"></a>Ejemplo de propiedades HTML de AdControl

En el siguiente ejemplo se muestra cómo asignar propiedades de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en HTML. Las propiedades **applicationId** y **adUnitId** son obligatorias. Las otras propiedades y eventos son opcionales. Si no proporcionas valores para las propiedades opcionales, el control usará los valores predeterminados que creen un anuncio coherente con la experiencia de usuario de la aplicación.

Las últimas cinco líneas son un ejemplo del registro de funciones para eventos de **AdControl**. Solo puedes registrar funciones que se hayan definido en el código JavaScript.

Estos valores son ejemplos. En el código se establecen los valores de estas funciones y propiedades para que sean adecuadas para tu aplicación.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl"
    data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                       adUnitId: '10865270',
                       isAutoRefreshEnabled: false,
                       onAdRefreshed: myAdRefreshed,
                       onErrorOccurred: myAdError,
                       onEngagedChanged: myAdEngagedChanged,
                       onPointerDown: myPointerDown,
                       onPointerUp: myPointerUp }" />
```

## <a name="related-topics"></a>Temas relacionados

* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 
