---
author: jnHs
Description: "Si la aplicación usa la mediación de anuncios o muestra banners o anuncios intersticiales de Microsoft Advertising, usa la página Monetización &gt; Monetizar con anuncios para administrar el uso que haces de los anuncios."
title: Monetizar con anuncios
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 6418fe1b47ac89e8decb135aa9a2108b3b95ef82
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="monetize-with-ads"></a>Monetizar con anuncios


Si la aplicación usa un control **AdMediatorControl**, **AdControl** o **InterstitialAd** para mostrar banners o anuncios intersticiales, usa la página **Monetización** &gt; **Monetizar con anuncios** para administrar el uso de anuncios.

## <a name="windows-ad-mediation"></a>Mediación de anuncios de Windows


Si la aplicación usa la mediación de anuncios, usa esta sección para establecer la configuración de mediación y agrega los parámetros necesarios para cada una de las redes de anuncios que tu aplicación usa. Para más información sobre las opciones de esta sección, consulta [Enviar la aplicación y configurar la mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/mt219689).

La mediación de anuncios permite optimizar los ingresos generados por la publicidad desde la aplicación al arbitrar solicitudes de pancartas desde varias redes de anuncios. Para más información sobre la mediación de anuncios, consulta [Usar la mediación de anuncios para maximizar los ingresos](https://msdn.microsoft.com/library/windows/apps/mt219691).

## <a name="coppa-compliance"></a>Cumplimiento de COPPA

Para cumplir con la Ley de protección de la privacidad infantil en línea ("COPPA"), debes notificar a Microsoft si la aplicación se dirige a los niños menores de 13 años. Si usas centro de desarrollo para indicar a Microsoft que la aplicación se dirige a los niños menores de 13 años, Microsoft tomará medidas para deshabilitar sus servicios de publicidad conductual al ofrecer publicidad en tu aplicación. Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA.

Para obtener más información sobre tus obligaciones en virtud de la COPPA, consulta [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).

## <a name="microsoft-affiliate-ads"></a>Anuncios de filiales de Microsoft

Activa la casilla de esta sección si quieres mostrar anuncios de filiales de Microsoft en tu aplicación. Si activas esta casilla, se ofrecerán anuncios de productos de la Tienda, como música, juegos, películas, aplicaciones, hardware y software a la aplicación cuando no haya otras redes de anuncios disponibles. Cuando los usuarios hagan clic en los anuncios y compra productos en la Tienda dentro de una ventana de atribución determinada, conseguirás una comisión sobre las compras aprobadas.

Si cambias esta selección, no es necesario volver a publicar la aplicación para que los cambios surtan efecto. Para obtener más información sobre los anuncios de filiales de Microsoft, consulta el tema [Acerca de los anuncios de filiales](about-affiliate-ads.md).

> **Nota**  Si la aplicación usa la mediación de anuncios (es decir, usa un objeto **AdMediatorControl** para mostrar anuncios), la aplicación puede mostrar anuncios de filiales solo si las opciones de mediación de anuncios están configuradas para mostrar anuncios de Microsoft.

## <a name="community-ads"></a>Anuncios de la comunidad

Activa la casilla de esta sección si quieres promocionar de manera cruzada tu aplicación con aplicaciones de otros desarrolladores. Si activas esta casilla y después [creas una campaña publicitaria de la comunidad](create-an-ad-campaign-for-your-app.md), tu aplicación mostrará anuncios para las aplicaciones publicadas por otros desarrolladores que también crean campañas publicitarias de la comunidad y los anuncios para sus aplicaciones se mostrarán en la aplicación. Los anuncios de la comunidad son gratuitos y se muestran solamente cuando no haya anuncios de otras redes de publicidad disponibles.

Si cambias esta selección, no es necesario volver a publicar la aplicación para que los cambios surtan efecto. Para obtener más información sobre los anuncios de la comunidad, consulta [Acerca de los anuncios de la comunidad](about-community-ads.md).

## <a name="microsoft-advertising-ad-units"></a>Unidades de anuncio de Microsoft Advertising

Usa esta sección para crear una unidad de anuncio de Microsoft Advertising. Solo hace falta crear unidades de anuncios en los siguientes escenarios:

-   La aplicación muestra pancartas mediante un objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   La aplicación muestra anuncios intersticiales mediante un objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

Para crear una unidad de anuncio para estos escenarios:

1.  Otorga un nombre a la unidad de anuncio.
2.  Selecciona el tipo de unidad de anuncio (**Banner**, **Banner intersticial** o **Vídeo intersticial**).
3.  Selecciona el tipo de dispositivo (**Móvil** o **PC/Tableta**).
4.  Haz clic en **Crear unidad de anuncios**.

Las unidades de anuncio aparecen en una tabla en la parte inferior de esta sección. Para cada unidad de anuncio, verás un **Id. de la aplicación** y un **Id. de unidad de anuncio**. Para mostrar anuncios en tu aplicación, debes usar estos valores en el código:

-   Si la aplicación muestra pancartas, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) y [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) del objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Para obtener más información, consulta [AdControl en XAML y .NET](../monetize/adcontrol-in-xaml-and--net.md) y [AdControl en HTML5 y JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
-   Si la aplicación muestra anuncios intersticiales, pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) del objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Para más información, consulta [Anuncios intersticiales](../monetize/interstitial-ads.md).

> **Nota**  Si la aplicación usa un objeto **AdMediatorControl** para mostrar anuncios en banners de Microsoft Advertising, no es necesario solicitar unidades de anuncio. En este escenario, las unidades de anuncios de Microsoft Advertising se generan automáticamente.

 

 

 
