---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: Aprenda cómo asignar propiedades de ** AdControl ** en HTML.
title: Ejemplo de las propiedades HTML

---

# Ejemplo de las propiedades HTML


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

 


<!--HONumber=May16_HO2-->


