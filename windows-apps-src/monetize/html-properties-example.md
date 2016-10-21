---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "Aprende cómo asignar propiedades de ** AdControl ** en HTML."
title: Ejemplo de propiedades HTML
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 1898ed2ccad74ac33c5130c627363e0a9daebceb


---

# Ejemplo de propiedades HTML




En el siguiente ejemplo se muestra cómo asignar propiedades de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en HTML. Las propiedades **applicationId** y **adUnitId** son obligatorias. Las otras propiedades y eventos son opcionales. Si no proporcionas valores para las propiedades opcionales, el control usará los valores predeterminados que creen un anuncio coherente con la experiencia de usuario de la aplicación.

Las últimas cinco líneas son un ejemplo del registro de funciones para eventos de **AdControl**. Solo puedes registrar funciones que se hayan definido en el código JavaScript.

Estos valores son ejemplos. En el código se establecen los valores de estas funciones y propiedades para que sean adecuadas para tu aplicación.

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


