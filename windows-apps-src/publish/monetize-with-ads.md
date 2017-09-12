---
author: jnHs
Description: "Si la aplicación usa la mediación de anuncios o muestra banners o anuncios intersticiales usando Microsoft Store Services SDK, usa la página Monetizar con anuncios para administrar el uso que haces de los anuncios."
title: Monetizar con anuncios
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 6ecd37e54de266764570606ceaa575614dfb050c
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2017
---
# <a name="monetize-with-ads"></a>Monetizar con anuncios

Cada aplicación en el panel de información incluye una página **Monetización** &gt; **Monetizar con anuncios**. Usa esta página para administrar el uso de los anuncios para los siguientes escenarios de desarrollo en tu aplicación:

* Tu aplicación para UWP usa un [AdControl](https://msdn.microsoft.com/en-us/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) o [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) desde el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* Tu aplicación Windows8.x o WindowsPhone8.x usa un **AdControl** o **InterstitialAd** desde el [SDK de Microsoft Advertising y Windows Phone8.x](http://aka.ms/store-8-sdk).
* Tu aplicación de Windows8.x o WindowsPhone8.x usa un **AdMediatorControl** desde el [SDK de Microsoft Advertising y WindowsPhone8.x](http://aka.ms/store-8-sdk).

Para obtener más información sobre estos escenarios de desarrollo, consulta [Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>Crear unidades de anuncios

Esta sección se usa para crear una unidad de anuncios para los siguientes escenarios:

* La aplicación muestra anuncios de banner mediante un objeto **AdControl**. Para obtener más información, consulta [AdControl en XAML y .NET](../monetize/adcontrol-in-xaml-and--net.md) y [AdControl en HTML5 y JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
* La aplicación muestra anuncios intersticiales en vídeo o anuncios de banner intersticiales mediante un **InterstitialAd**. Para obtener más información, consulta [Anuncios intersticiales](../monetize/interstitial-ads.md).
* La aplicación muestra anuncios nativos mediante un **NativeAd**. Para obtener más información, consulta [Anuncios nativos](../monetize/native-ads.md).

Para crear una unidad de anuncios:

1.  En el campo **Nombre de la unidad de anuncios**, escribe un nombre para esta. Esto puede ser cualquier cadena descriptiva que quieras usar para identificar la unidad de anuncios para fines informativos.
2.  En la lista desplegable **Tipo de unidad de anuncios**, selecciona el tipo de unidad de anuncios que corresponde a los anuncios que se muestran en el control. Las opciones disponibles son: **Banner**, **Banner intersticial**, **Vídeo intersticial** y **Nativo**.
    > [!NOTE]
    > La capacidad para crear unidades de anuncios **Nativo** está solo disponible actualmente para seleccionar los desarrolladores que participan en un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto. Si estás interesado en unirte a nuestro programa piloto, ponte en contacto con nosotros en aiacare@microsoft.com.

3.  En la lista desplegable **Familia de dispositivos**, selecciona la familia de dispositivos a la que está dirigida la aplicación en la que se usará la unidad de anuncios. Las opciones disponibles son: **UWP (Windows10)**, **PC o tableta (Windows8.1)** o **Móvil (Windows Phone8.x)**.
4.  Haz clic en **Crear unidad de anuncios**.

La nueva unidad de anuncios aparece en la parte superior de la lista en la sección **Unidades de anuncios disponibles** en esta página. Para obtener más información sobre cómo trabajar con unidades de anuncios en tu aplicación, consulta [Configurar unidades de anuncios en la aplicación](../monetize/set-up-ad-units-in-your-app.md).

> [!NOTE]
> Si tu aplicación en Windows8.x o WindowsPhone8.x usa un **AdMediatorControl** para mostrar anuncios de banner, no tienes que solicitar unidades de anuncios aquí. En este escenario, las unidades de anuncios se generan automáticamente.

<span id="available-ad-units" />
## <a name="available-ad-units"></a>Unidades de anuncios disponibles

Las unidades de anuncio aparecen en una tabla en la parte inferior de esta sección. Para cada unidad de anuncio, verás un **Id. de la aplicación** y un **Id. de unidad de anuncio**. Para mostrar anuncios en tu aplicación, debes usar estos valores en el código:

-   Si la aplicación muestra pancartas, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) y [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) del objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Para obtener más información, consulta [AdControl en XAML y .NET](../monetize/adcontrol-in-xaml-and--net.md) y [AdControl en HTML5 y JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).
-   Si la aplicación muestra anuncios intersticiales, pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) del objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Para obtener más información, consulta [Anuncios intersticiales](../monetize/interstitial-ads.md).
-   Si la aplicación muestra anuncios nativos, pasa estos valores a los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obtener más información, consulta [Anuncios nativos](../monetize/native-ads.md).

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

> [!NOTE]
> Puedes usar varios controles de anuncios de banners, intersticiales y nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/monetize-with-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

<span id="mediation" />
## <a name="ad-mediation"></a>Mediación de anuncios

Si tu aplicación es una aplicación para UWP para Windows10, puedes usar las opciones de esta sección para habilitar la unidad de anuncios para UWP que está asociada a un anuncio de banner, intersticial o nativo. La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios que no generan ingresos para las campañas de promoción de las aplicaciones de Microsoft. Nosotros nos encargamos de arbitrar solicitudes de anuncios de banner desde las redes de anuncios que elijas.

Si ya tienes una unidad de anuncios para UWP asociada a un anuncio de banner, intersticial o nativo en tu aplicación, al habilitar la mediación de anuncios no se necesitan cambios de código alguno en tu aplicación.

> [!NOTE]
> Esta sección describe las opciones de **mediación de anuncios** para los paquetes de aplicaciones de UWP. Si el paquete de la aplicación está dirigido a Windows 8.x o Windows Phone 8.x y usa el **AdMediatorControl** desde el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk), la sección de **Mediación de anuncios** en el panel muestra un conjunto diferente de opciones. Para obtener más información acerca de cómo configurar la mediación para un paquete de aplicación de Windows 8.x o un Windows Phone 8.x que use **AdMediatorControl**, consulta [este artículo](https://msdn.microsoft.com/library/windows/apps/mt219689).

Configurar la configuración de mediación de anuncios para UWP en tu aplicación:

1. En la lista desplegable **Configurar mediación para**, selecciona el paquete de la aplicación para UWP que incluya el anuncio de banner, intersticial o nativo que quieras configurar.

2. En la lista desplegable **Tipo de unidad de anuncios**, selecciona el tipo de unidad de anuncios que deseas configurar (**Banner**, **Banner intersticial**, **Vídeo intersticial** o **Nativo**).

3. En lista desplegable **Unidad de anuncios**, selecciona el nombre de la unidad de anuncios para UWP que deseas configurar.
    > [!NOTE]
    > Al habilitar la mediación de anuncios para una unidad de anuncios para UWP, no es necesario obtener una unidad de anuncios de redes de anuncios de terceros. Nuestro servicio de mediación de anuncios creará automáticamente las unidades de anuncios de terceros que sean necesarias.

4. De manera predeterminada, activa la casilla **Permitir a Microsoft que elija la mejor configuración de mediación para tu aplicación** casilla está activada. Esta opción usa algoritmos de aprendizaje automático para elegir automáticamente la configuración de mediación de anuncios para tu aplicación que te ayudará a maximizar los ingresos por anuncios en todos los mercados admitidos por la aplicación. Te recomendamos que uses esta opción. De lo contrario, si deseas elegir tu propia configuración de mediación de anuncios, desactiva esta casilla.
    > [!NOTE]
    > El resto de pasos de esta sección, solo son aplicables si desactivas esta casilla y eliges tu propia configuración de mediación de anuncios.

5. En la lista desplegable **Dirigida** elige **Base de referencia** para configurar los ajustes predeterminados de la configuración de la mediación de anuncios. Esta configuración predeterminada se aplicará a todos los mercados, excepto los mercados donde establezcas las configuraciones específicas del mercado.

6. Después, especifica la relación de anuncios que quieras mostrar en el control de las redes de pago (de los que obtienes ingresos por las impresiones de anuncios) y otras redes de anuncios (de los que no obtienes ingresos por las impresiones de anuncios). Para ello, escribe un valor entre 0 y 100 en los campos **Peso** para **Redes de anuncios de pago** y **Otras redes de anuncios**.  

7. En la sección **Redes de anuncios de pago**, selecciona la casilla de verificación en la columna **Activa** para cada red de pago que quieras usar y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red).

    Actualmente se admiten las siguientes redes de pago. Ten en cuenta que algunas de estas redes [no están disponibles en todos los mercados](#network-markets).

    -   **AOL y AppNexus**. Se trata de una red de anuncios de administrados por Microsoft que proporciona anuncios a través de nuestro socio de redes, AOL y AppNexus. Esta red es compatible con unidades de anuncios **Banner** y **Vídeo intersticial**.
        > [!NOTE]
        > **AOL y AppNexus** siempre están clasificadas en el primer lugar en la lista de **Redes de anuncios de pago** para unidades de anuncios **Banner** y no se pueden cambiar a una clasificación menor para estos tipos de anuncios.

    -   **AppNexus (directo)**: selecciona esta opción para proporcionar anuncios intersticiales en vídeo de [AppNexus](https://www.appnexus.com). Esta red es compatible con unidades de anuncios **Vídeo intersticial** y **Nativo**.

    -   **Anuncios para instalación de aplicaciones de Microsoft** Selecciona esta opción para proporcionar anuncios para instalación de aplicaciones o anuncios para volver a interactuar con la aplicación que han sido creados por otros desarrolladores del ecosistema de Windows que [crean campañas de anuncios promocionales para sus aplicaciones](create-an-ad-campaign-for-your-app.md). Esta red es compatible con unidades de anuncios **Banner**, **Banner intersticial** y **Nativo**.

    -   **Smaato**: selecciona esta opción para proporcionar anuncios desde [Smaato](https://www.smaato.com/). Esta red es compatible con unidades de anuncios **Banner**.

    -   **smartclip**: selecciona esta opción para proporcionar anuncios desde [smartclip](http://www.smartclip.com/). Esta red es compatible con unidades de anuncios **Vídeo intersticial**.

    -   **SpotX**: selecciona esta opción para proporcionar anuncios desde [SpotX](https://www.spotx.tv/). Esta red es compatible con unidades de anuncios **Vídeo intersticial**.

    -   **Taboola**: selecciona esta opción para proporcionar anuncios desde [Taboola](https://www.taboola.com/). Esta red es compatible con unidades de anuncios **Banner**.

8. Si has seleccionado una unidad de anuncios **Banner** o **Banner intersticial**, también verás una sección denominada **Otras redes de anuncios**. Las redes en esta sección no obtienen ingresos por las impresiones de anuncios. En su lugar, estas redes muestran anuncios que proceden de la campañas de promoción de la aplicación, entre otras. 

    En la sección **Otras redes de anuncios**, selecciona la casilla de verificación en la columna **Activa** para cada red que quieras usar y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red). Actualmente se admiten las redes siguientes:

    -   **Anuncios de la comunidad Microsoft**. Si creas una [campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncio de la comunidad](about-community-ads.md), selecciona esta opción para mostrar anuncios de esta campaña. 

    -   **Anuncios internos de Microsoft**. Si [creas una campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncios internos](about-house-ads.md), selecciona esta opción para mostrar anuncios de esta campaña.

9. Puedes reemplazar la configuración de mediación de forma predeterminada en los mercados y para ello, selecciona **Destinado** en la lista desplegable y actualiza las selecciones de red de anuncios y la clasificación.

10. Haz clic en **Guardar**.

<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>Mercados admitidos para las redes de anuncios

Las redes de anuncios disponibles proporcionan anuncios en todos los [mercados admitidos](define-pricing-and-market-selection.md#windows-store-consumer-markets) por las aplicaciones para UWP, con las siguientes excepciones.

|  Red de anuncios  |  Mercados admitidos  |
|--------------|---------------------|
| Smaato | Brasil, Canadá, Francia, Italia, Japón, España, Reino Unido, Estados Unidos |
| smartclip | Austria, Bélgica, Dinamarca, Finlandia, Francia, Italia, Países Bajos, Noruega, Suecia, Suiza  |


## <a name="microsoft-affiliate-ads"></a>Anuncios de filiales de Microsoft

Activa la casilla de esta sección si quieres mostrar anuncios de filiales de Microsoft en tu aplicación. Si activas esta casilla, se ofrecerán anuncios de productos de la Tienda, como música, juegos, películas, aplicaciones, hardware y software a la aplicación cuando no haya otras redes de anuncios disponibles. Cuando los clientes hagan clic en los anuncios y compren productos en la Tienda en una ventana de atribución determinada, conseguirás una comisión sobre las compras aprobadas.

Si cambias esta selección, no es necesario volver a publicar la aplicación para que los cambios surtan efecto. Para obtener más información sobre los anuncios de filiales de Microsoft, consulta el tema [Acerca de los anuncios de filiales](about-affiliate-ads.md).

## <a name="coppa-compliance"></a>Cumplimiento de COPPA

Para cumplir con la Ley de protección de la privacidad infantil en línea ("COPPA"), debes notificar a Microsoft si la aplicación se dirige a los niños menores de 13 años. Si usas centro de desarrollo para indicar a Microsoft que la aplicación se dirige a los niños menores de 13 años, Microsoft tomará medidas para deshabilitar sus servicios de publicidad conductual al ofrecer publicidad en tu aplicación. Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA.

Para obtener más información sobre tus obligaciones en virtud de la COPPA, consulta [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).
