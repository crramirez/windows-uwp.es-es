---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "Aprende a crear mediante programación un objeto ** AdControl ** con JavaScript."
title: Crear un objeto AdControl en JavaScript
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 481f9d785181ca197debdb807bb0b0c7b4168632


---

# Crear un objeto AdControl en JavaScript


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este ejemplo se muestra cómo crear mediante programación un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) con JavaScript.

## Div HTML para un objeto AdControl

Un objeto **AdControl** debe incluir **div** en la página html que mostrará el anuncio. El siguiente código proporciona un ejemplo de **div**.

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## JavaScript para crear un objeto AdControl

En el siguiente ejemplo se da por hecho que estás usando un valor **div** existente en el código HTML con el identificador **MyAd**.

Crea una instancia del objeto **AdControl** en la función **app.onactivated**.

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

Los valores son ejemplos. En el código se establecen los valores de estas funciones y las propiedades adecuadas para tu aplicación.

Si usas este código y no ves anuncios, puedes intentar insertar un atributo **position:relative** en **div** que contenga **AdControl**. Esto invalidará la configuración predeterminada de **IFrame**. Los anuncios se mostrarán correctamente, a menos que no se muestren debido al valor de este atributo. Ten en cuenta que es posible que las nuevas unidades de anuncios no estén disponibles hasta 30 minutos.

## Temas relacionados

* [Ejemplos de publicidad en GitHub](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


