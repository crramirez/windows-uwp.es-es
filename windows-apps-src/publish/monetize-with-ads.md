---
author: jnHs
Description: "Si la aplicación usa la mediación de anuncios o muestra banners o anuncios intersticiales en vídeo de Microsoft Advertising, usa la página Rentabilidad &gt; Rentabilizar con anuncios para administrar el uso que haces de los anuncios."
title: Rentabilizar con anuncios
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: af322795be849bdaa2b1babeaec29d09f5d73fcf

---

# Rentabilizar con anuncios


Si la aplicación usa un control **AdMediatorControl**, **AdControl** o **InterstitialAd** para mostrar anuncios en banner o anuncios intersticiales en vídeo, usa la página **Monetización** &gt; **Monetizar con anuncios** para administrar el uso de los anuncios.

## Mediación de anuncios de Windows


Si la aplicación usa la mediación de anuncios, usa esta sección para establecer la configuración de mediación y agrega los parámetros necesarios para cada una de las redes de anuncios que tu aplicación usa. Para más información sobre las opciones de esta sección, consulta [Enviar la aplicación y configurar la mediación de anuncios](https://msdn.microsoft.com/library/windows/apps/mt219689).

La mediación de anuncios permite optimizar los ingresos generados por la publicidad desde la aplicación al arbitrar solicitudes de pancartas desde varias redes de anuncios. Para más información sobre la mediación de anuncios, consulta [Usar la mediación de anuncios para maximizar los ingresos](https://msdn.microsoft.com/library/windows/apps/mt219691).

## Cumplimiento de COPPA

Para cumplir con la Ley de protección de la privacidad infantil en línea ("COPPA"), debes notificar a Microsoft si la aplicación se dirige a los niños menores de 13 años. Si usas centro de desarrollo para indicar a Microsoft que la aplicación se dirige a los niños menores de 13 años, Microsoft tomará medidas para deshabilitar sus servicios de publicidad conductual al ofrecer publicidad en tu aplicación. Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA.

Para obtener más información sobre tus obligaciones en virtud de la COPPA, consulta [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).

## Anuncios de filiales de Microsoft

Activa la casilla de esta sección si quieres mostrar anuncios de filiales de Microsoft en tu aplicación. Si activas esta casilla, se ofrecerán anuncios de productos de la Tienda, como música, juegos, películas, aplicaciones, hardware y software a la aplicación cuando no haya otras redes de anuncios disponibles. Cuando los usuarios hagan clic en los anuncios y productos de bus en la Tienda dentro de una ventana de atribución determinada, conseguirás una comisión sobre las compras aprobadas.

Si cambias esta selección, no es necesario volver a publicar la aplicación para que los cambios surtan efecto. Para obtener más información sobre los anuncios de filiales de Microsoft , consulta el tema [Acerca de los anuncios de filiales](about-affiliate-ads.md).

> **Nota**  Si la aplicación usa la mediación de anuncios (es decir, usa un objeto **AdMediatorControl** para mostrar anuncios), la aplicación puede mostrar anuncios de filiales solo si las opciones de mediación de anuncios están configurada para mostrar anuncios de Microsoft.

## Anuncios de la comunidad

Activa la casilla de esta sección si quieres promocionar de manera cruzada tu aplicación con aplicaciones de otros desarrolladores. Si activas esta casilla y después [creas una campaña publicitaria de la comunidad](create-an-ad-campaign-for-your-app.md), tu aplicación mostrará anuncios para las aplicaciones publicadas por otros desarrolladores que también crean campañas publicitarias de la comunidad y los anuncios para sus aplicaciones se mostrarán en la aplicación. Los anuncios de la comunidad son gratuitos y se muestran solamente cuando no haya anuncios de otras redes de publicidad disponibles.

Si cambias esta selección, no es necesario volver a publicar la aplicación para que los cambios surtan efecto. Para obtener más información sobre los anuncios de la comunidad, consulta [Acerca de los anuncios de la comunidad](about-community-ads.md).

## Unidades de anuncio de Microsoft Advertising

Usa esta sección para crear una unidad de anuncio de Microsoft Advertising. Solo hace falta crear unidades de anuncios en los siguientes escenarios:

-   La aplicación muestra pancartas de Microsoft Advertising mediante un objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   La aplicación muestra anuncios intersticiales en vídeo de Microsoft Advertising mediante el objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

Para crear una unidad de anuncio para estos escenarios:

1.  Otorga un nombre a la unidad de anuncio.
2.  Selecciona el tipo de unidad (**Pancarta** o **Anuncios intersticiales en vídeo**).
3.  Selecciona el tipo de dispositivo (**Móvil** o **PC/Tableta**).
4.  Haz clic en **Crear unidad de anuncios**.

Las unidades de anuncio aparecen en una tabla en la parte inferior de esta sección. Para cada unidad de anuncio, verás un **Id. de la aplicación** y un **Id. de unidad de anuncio**. Para mostrar anuncios en tu aplicación, debes usar estos valores en el código:

-   Si la aplicación muestra pancartas, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) y [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) del objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).
-   Si la aplicación muestra anuncios intersticiales en vídeos, pasa estos valores para el método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) del objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

> **Nota**  Si la aplicación usa la mediación de anuncios para mostrar anuncios en banners de Microsoft Advertising (es decir, usa un objeto **AdMediatorControl**), no es necesario solicitar unidades de anuncio. En este escenario, las unidades de anuncios de Microsoft Advertising se generan automáticamente.

 

 

 



<!--HONumber=Aug16_HO3-->


