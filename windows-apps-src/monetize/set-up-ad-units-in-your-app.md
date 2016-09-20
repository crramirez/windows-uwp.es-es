---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Aprende a agregar valores de identificador de aplicación y de identificador de unidad de anuncios a la aplicación desde el panel del Centro de desarrollo de Windows antes de enviar la aplicación a la Tienda."
title: "Configurar unidades de anuncios en la aplicación"
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 96c09de9321f67dc26cc3538f2655bd598f134f9


---

# Configurar unidades de anuncios en la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Si usas un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para mostrar anuncios en tu aplicación, debes especificar un identificador de aplicación y un identificador de unidad de anuncios. Mientras desarrolles la aplicación, usa los [valores de identificador de aplicación y de identificador de anuncios de prueba](test-mode-values.md) para ver cómo la aplicación representa los anuncios durante las pruebas.

Cuando termines de probar la aplicación y estés listo para enviarla al Centro de desarrollo de Windows, debes actualizar el código de la aplicación para usar los valores de identificador de aplicación y de identificador de anuncios desde el [panel del Centro de desarrollo de Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si intentas usar valores de prueba en la aplicación dinámica la aplicación no recibirá anuncios dinámicos.

Para configurar el identificador de aplicación y las unidades de anuncios de la aplicación dinámica:

1.  En el panel del Centro de desarrollo de Windows, selecciona la aplicación y, a continuación, haga clic en **Monetización > Monetizar con anuncios**.
2.  En la sección **Unidades de anuncio de Microsoft Advertising** de esta página, crea una unidad de anuncio. Para el tipo de unidad de anuncio, selecciona **Banner** si usas un objeto **AdControl** o selecciona **Vídeo intersticial** si usas un objeto **InterstitialAd**. Para obtener más información acerca de esta página, consulta [Rentabilizar con anuncios](../publish/monetize-with-ads.md).

3.  Para cada unidad de anuncio generada, verás un **Id. de aplicación** y un **Id. de unidad de anuncio** en esta página. Para mostrar anuncios en tu aplicación, deberás usar estos valores en el código de la aplicación:

    * Si la aplicación muestra anuncios de banner, asigna estos valores a las propiedades **ApplicationId** y **AdUnitId** del objeto **AdControl**.

    * Si la aplicación muestra anuncios intersticiales en vídeos, pasa estos valores para el método **RequestAd** del objeto **InterstitialAd**.

> 
            **Importante** Si la aplicación usa la mediación de anuncios para mostrar anuncios de banner de Microsoft Advertising (es decir, usa un objeto **AdMediatorControl**), no tienes que solicitar unidades de anuncios. En este escenario, las unidades de anuncios se generan automáticamente. Para obtener más información, consulta [Cuál es la diferencia: AdMediator o AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

 

## Temas relacionados

[Valores del modo de prueba](test-mode-values.md)


 

 



<!--HONumber=Jun16_HO4-->


