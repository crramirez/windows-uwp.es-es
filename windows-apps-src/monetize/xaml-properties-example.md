---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Aprende a asignar propiedades **AdControl** a valores.
title: Ejemplo de propiedades XAML de AdControl
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, AdControl, XAML, propiedades
ms.openlocfilehash: 3c5dae93ab6ee58fac7d4593257d357f268c241a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="adcontrol-xaml-properties-example"></a>Ejemplo de propiedades XAML de AdControl

En el siguiente ejemplo de lenguaje XAML, se muestra cómo asignar propiedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) a los valores. Si no se establece una propiedad, **AdControl** usará los valores predeterminados para crear un anuncio que sea coherente con la experiencia del usuario de la aplicación.

Los valores son ejemplos. En el código se establecen los valores de estas funciones y las propiedades adecuadas para tu aplicación.

> [!div class="tabbedCodeSnippets"]
``` xml
<UI:AdControl Width="300",
    Height="250",
    AdUnitId="10865270",
    ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
    IsAutoRefreshEnabled="false",
    AdRefreshed="OnAdRefresh",
    ErrorOcurred="OnAdError",
    IsEngagedChanged="OnAdEngagedChanged" />
```

## <a name="related-topics"></a>Temas relacionados

* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [Ejemplos de publicidad de GitHub](http://aka.ms/githubads)

 
