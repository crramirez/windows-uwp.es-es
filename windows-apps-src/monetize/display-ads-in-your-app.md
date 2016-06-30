---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "El SDK de Microsoft Store Engagement and Monetization proporciona varias maneras de rentabilizar tu aplicación con anuncios."
title: "Mostrar anuncios en tu aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 8a5b02dbc40f3f0cd9be32aa7d5184e60a3b2707
ms.openlocfilehash: c79ba96908cc7b52afefbe44c3f56ce009c87f16

---

# Mostrar anuncios en tu aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El [SDK de Microsoft Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md) proporciona varias maneras de rentabilizar la aplicación con anuncios.

## Muestra anuncios intersticiales en vídeo y banners con las bibliotecas de Microsoft Advertising.

Gana más dinero con tus aplicaciones de Windows agregando anuncios intersticiales en vídeo y banners. Los anuncios se muestran en las aplicaciones de Windows para equipos, tabletas y teléfonos. Puedes supervisar el rendimiento de los anuncios en tiempo real mediante el panel del Centro de desarrollo de Windows.

Para incluir anuncios en tus aplicaciones, usa los controles **AdControl** e **InterstitialAd** en las bibliotecas de publicidad que se distribuyen en el SDK de Microsoft Store Engagement and Monetization. Puedes usar estos controles para mostrar anuncios intersticiales en vídeo y banners de Microsoft en tus aplicaciones XAML o HTML/JavaScript para Windows 10, Windows 8.1, Windows Phone 8.1 y Windows Phone 8.

Para obtener más información, consulta [Mostrar anuncios mediante las bibliotecas de Microsoft Advertising](display-ads-using-the-microsoft-advertising-libraries.md). Después de publicar una aplicación con anuncios, usa el [informe de rendimiento de publicidad](../publish/advertising-performance-report.md) para realizar un seguimiento del rendimiento de los anuncios.                                           

## Usar la mediación de anuncios para anuncios en banners de varias redes de anuncios

Puedes usar la clase **AdMediatorControl** en las aplicaciones XAML para optimizar los ingresos publicitarios mostrando banners desde varias redes de anuncios. Después de agregar este control a la aplicación, establece la configuración de mediación de anuncios en el panel del Centro de desarrollo de Windows y nosotros nos encargamos de arbitrar solicitudes de anuncios de banner de las redes de anuncios que elijas. Para obtener más información, consulta [Uso de mediación de anuncios para maximizar los ingresos de los anuncios](use-ad-mediation-to-maximize-revenue.md).

## Diferencias entre las bibliotecas de Microsoft Advertising y la mediación de anuncios

El SDK de Microsoft Store Engagement and Monetization incluye las bibliotecas de Microsoft Advertising y mediación de anuncios. Sin embargo, estas bibliotecas proporcionan clases distintas y se usan con otros fines.

* Usa las clases **AdControl** e **InterstitialAd** en las bibliotecas de Microsoft Advertising si quieres mostrar banners o anuncios intersticiales en vídeo en una aplicación XAML o JavaScript.
* Usa la clase **AdMediatorControl** en las bibliotecas de mediación de anuncios si quieres mostrar anuncios en banners desde varias redes de anuncios en una aplicación XAML.

Para obtener más información, consulta [What is the difference: AdMediator or AdControl (Diferencias entre AdMediator y AdControl)](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Temas relacionados

* [SDK de Microsoft Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md)
* [Monetiza tu aplicación con anuncios]( http://go.microsoft.com/fwlink/p/?LinkId=699559)



<!--HONumber=Jun16_HO4-->


