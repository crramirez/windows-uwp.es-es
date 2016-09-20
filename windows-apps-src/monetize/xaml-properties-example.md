---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "Aprende cómo asignar propiedades ** AdControl ** a valores."
title: Ejemplo de las propiedades XAML
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 43d579d2d0a92a8f03f17efa2ec42707357e99f9


---

# Ejemplo de las propiedades XAML


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El siguiente ejemplo XAML muestra cómo asignar propiedades [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) a valores. Si no se establece una propiedad, **AdControl** usará los valores predeterminados para crear un anuncio que sea coherente con la experiencia del usuario de la aplicación.

Los valores son ejemplos. En el código se establecen los valores de estas funciones y las propiedades adecuadas para tu aplicación.

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## Temas relacionados

* [Muestras de publicidad en GitHub](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


