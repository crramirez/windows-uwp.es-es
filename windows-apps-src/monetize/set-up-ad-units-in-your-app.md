---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Aprende a agregar valores de identificador de aplicación y de identificador de unidad de anuncios a la aplicación desde el panel del Centro de desarrollo de Windows antes de enviar la aplicación a la Tienda."
title: "Configurar unidades de anuncios en la aplicación"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, unidades de anuncios
ms.openlocfilehash: daf0887462a4c84aa827a6261793a0eaf4d512ca
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anuncios en la aplicación




Si usas un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para mostrar anuncios en tu aplicación, debes especificar un identificador de aplicación y un identificador de unidad de anuncios. Mientras desarrolles la aplicación, usa los [valores de identificador de aplicación y de identificador de anuncios de prueba](test-mode-values.md) para ver cómo la aplicación representa los anuncios durante las pruebas.

Cuando termines de probar la aplicación y estés listo para enviarla al Centro de desarrollo de Windows, debes actualizar el código de la aplicación para usar los valores de identificador de aplicación y de identificador de anuncios desde el [panel del Centro de desarrollo de Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si intentas usar valores de prueba en la aplicación dinámica la aplicación no recibirá anuncios dinámicos.

Para configurar el identificador de aplicación y las unidades de anuncios de la aplicación dinámica:

1.  En el panel del Centro de desarrollo de Windows, selecciona la aplicación y, a continuación, haga clic en **Monetización > Monetizar con anuncios**.

2.  En la sección **Unidades de anuncios de Microsoft Advertising** de esta página, crea una unidad de anuncios. Para el tipo de unidad de anuncios, selecciona una de las siguientes opciones:

  * Si usas **AdControl** en tu aplicación para mostrar anuncios de banner, selecciona **Banner** para el tipo de unidad de anuncios.

  * Si usas **InterstitialAd** en tu aplicación para mostrar anuncios de banner intersticial o de vídeo intersticial, selecciona **Vídeo intersticial** o **Banner intersticial** (asegúrate de seleccionar la opción adecuada para el tipo de anuncio intersticial que quieras mostrar).

3.  Para cada unidad de anuncios generada, verás un **Id. de aplicación** y un **Id. de la unidad de anuncios** en esta página. Para mostrar anuncios en tu aplicación, tendrás que usar estos valores en el código de la aplicación:

  * Si la aplicación muestra anuncios de banner, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) y [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) del objeto **AdControl**. Para obtener más información, consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md) y [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md).

  * Si la aplicación muestra anuncios intersticiales, pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) del objeto **InterstitialAd**. Para más información, consulta [Anuncios intersticiales](interstitial-ads.md).

Para más información sobre la página **Monetizar con anuncios**, consulta [Monetizar con anuncios](../publish/monetize-with-ads.md).

## <a name="related-topics"></a>Temas relacionados

* [Valores del modo de prueba](test-mode-values.md)
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](interstitial-ads.md)


 

 
