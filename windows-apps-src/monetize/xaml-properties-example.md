---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: Aprende a asignar propiedades **AdControl** a valores.
title: Ejemplo de propiedades XAML de AdControl
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: f33e4048a1a9aa68ecc627d81ca15858027720c1

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

 



<!--HONumber=Dec16_HO2-->


