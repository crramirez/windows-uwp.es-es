---
author: jnHs
Description: If your app displays ads using the Microsoft Advertising SDK, use the In-app ads page of the Dev Center dashboard to manage your use of ads.
title: "Anuncios desde la aplicación"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.author: wdg-dev-content
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: high
ms.openlocfilehash: f0faa69cef0f98171c4679d6a94b01199b215cb4
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2017
---
# <a name="in-app-ads"></a>Anuncios desde la aplicación

Usa la página **Monetizar** &gt; **Anuncios desde la aplicación** del panel del Centro de desarrollo para crear y administrar unidades de anuncios para:

* Aplicaciones de la Plataforma universal de Windows (UWP) que usan el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp).
* Aplicaciones de Windows 8.x y Windows Phone 8.x, que usan el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk).

Para obtener más información sobre cómo integrar estos SDK con tus aplicaciones para mostrar anuncios, consulta [Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />
## <a name="create-ad-units"></a>Crear unidades de anuncios

Para crear una unidad de anuncios para un [anuncio de banner](../monetize/banner-ads.md), [anuncio intersticial](../monetize/interstitial-ads.md) o [anuncio nativo](../monetize/native-ads.md) en tu app:

1.  Ve a la página **Monetizar** &gt; **Anuncios desde la aplicación** del panel y haz clic en **Crear unidad de anuncios**.

2.  En la lista desplegable **Nombre de la aplicación**, selecciona la aplicación en la que se usará la unidad de anuncios.

3.  En el campo **Nombre de la unidad de anuncios**, escribe un nombre para esta. Esto puede ser cualquier cadena descriptiva que quieras usar para identificar la unidad de anuncios para fines informativos.

4.  En la lista desplegable **Tipo de unidad de anuncio**, selecciona el tipo de anuncio.

    * Si usas [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en tu aplicación para mostrar anuncios de banner, selecciona **Banner**.

    * Si usas [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) en tu aplicación para mostrar anuncios de banner intersticial o de vídeo intersticial, selecciona **Vídeo intersticial** o **Banner intersticial** (asegúrate de seleccionar la opción adecuada para el tipo de anuncio intersticial que quieras mostrar).

    * Si usas un [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) en tu aplicación para mostrar anuncios nativos, selecciona **Nativo**.
      > [!NOTE]
      > La capacidad para crear unidades de anuncios **Nativo** está solo disponible actualmente para seleccionar los desarrolladores que participan en un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto. Si estás interesado en unirte a nuestro programa piloto, ponte en contacto con nosotros en aiacare@microsoft.com.

5. En la lista desplegable **Familia de dispositivos**, selecciona la familia de dispositivos a la que está dirigida la aplicación en la que se usará la unidad de anuncios. Las opciones disponibles son: **UWP (Windows10)**, **PC o tableta (Windows8.1)** o **Móvil (Windows Phone8.x)**.

6. Configura los siguientes valores adicionales según tus preferencias:

    * Si seleccionas la familia de dispositivos **UWP (Windows 10)** para la unidad de anuncio, también puedes configurar la [configuración de mediación](#mediation) para la unidad de anuncio.

    * Si seleccionas la familia de dispositivos **PC o tableta (Windows8.1)** o **Móvil (Windows Phone8.x)** para una unidad de anuncios de banner, también puedes seleccionar **Show community ads in your app** para participar en los [anuncios de la comunidad](about-community-ads.md).

7.  Si aún no has configurado el cumplimiento de COPPA para la app seleccionada, elige una opción en la sección [Cumplimiento de COPPA](#coppa).

8.  Haz clic en **Crear unidad de anuncios**.

Después de crear la nueva unidad de anuncio, aparece en la tabla de unidades de anuncios disponibles en la página **Monetizar** &gt; **Anuncios desde la aplicación**.

<span id="available-ad-units" />
## <a name="review-and-edit-ad-units"></a>Revisar y editar unidades de anuncios

Después de crear unidades de anuncios para una o más aplicaciones en tu cuenta, estas unidades de anuncio aparecen en una tabla en la parte inferior de la página **Monetizar** &gt; **Anuncios desde la aplicación**. Esta tabla muestra el **Id. de aplicación** y el **Id. de unidad de anuncio** para cada unidad de anuncio, junto con otra información. Para mostrar anuncios en tu aplicación, debes usar estos valores en el código. Para más información, consulta [Configurar unidades de anuncios en la aplicación](../monetize/set-up-ad-units-in-your-app.md).

* Si la aplicación muestra [anuncios de banner](../monetize/banner-ads.md), asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) y [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) del objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx).

* Si la aplicación muestra [anuncios intersticiales](../monetize/interstitial-ads.md), pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) del objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx).

* Si la aplicación muestra [anuncios nativos](../monetize/native-ads.md), pasa estos valores a los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx).
  > [!IMPORTANT]
  > Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

  > [!NOTE]
  > Puedes usar varios controles de anuncios de banners, intersticiales y nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

Para editar la [configuración de mediación](#mediation) para una unidad de anuncios para UWP o el [Cumplimiento de COPPA](#coppa) para la aplicación en la que se usa la unidad de anuncio, haz clic en el nombre de la unidad de anuncios.

<span id="mediation" />
## <a name="mediation-settings"></a>Configuración de la mediación

Al [crear una nueva unidad de anuncios para UWP](#create-ad-unit) o [editar una unidad de anuncios para UWP existente](#available-ad-units), usa las opciones de esta sección para configurar la mediación de anuncios para la unidad de anuncios. La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios que no generan ingresos para las campañas de promoción de las aplicaciones de Microsoft. Nosotros nos encargamos de arbitrar solicitudes de anuncios de banner desde las redes de anuncios que elijas. Si ya tienes una unidad de anuncios para UWP asociada a un anuncio de banner, intersticial o nativo en tu aplicación, al habilitar la mediación de anuncios no se necesitan cambios de código alguno en tu aplicación.

> [!NOTE]
> Al habilitar la mediación de anuncios para una unidad de anuncios para UWP, no es necesario obtener una unidad de anuncios de redes de anuncios de terceros. Nuestro servicio de mediación de anuncios creará automáticamente las unidades de anuncios de terceros que sean necesarias.

Configurar la configuración de mediación de anuncios para UWP en tu aplicación:

1. [Crear una unidad de anuncio](#create-ad-unit) o [seleccionar una unidad de anuncio existente](#available-ad-units).

2. En la página **Anuncios desde la aplicación** , ve a la sección **Mediation settings**.

3. De manera predeterminada, activa la casilla **Permitir a Microsoft que elija la mejor configuración de mediación para tu aplicación** casilla está activada. Esta opción usa algoritmos de aprendizaje automático para elegir automáticamente la configuración de mediación de anuncios para tu aplicación que te ayudará a maximizar los ingresos por anuncios en todos los mercados admitidos por la aplicación. Te recomendamos que uses esta opción. De lo contrario, si deseas elegir tu propia configuración de mediación de anuncios, desactiva esta casilla.
    > [!NOTE]
    > El resto de pasos de esta sección, solo son aplicables si desactivas esta casilla y eliges tu propia configuración de mediación de anuncios.

4. En la lista desplegable **Dirigida** elige **Base de referencia** para configurar los ajustes predeterminados de la configuración de la mediación de anuncios. Esta configuración predeterminada se aplicará a todos los mercados, excepto los mercados donde establezcas las configuraciones específicas del mercado.

6. Después, especifica la relación de anuncios que quieras mostrar en el control de las redes de pago (de los que obtienes ingresos por las impresiones de anuncios) y otras redes de anuncios (de los que no obtienes ingresos por las impresiones de anuncios). Para ello, escribe un valor entre 0 y 100 en los campos **Peso** para **Redes de anuncios de pago** y **Otras redes de anuncios**.  

7. En la sección **Redes de anuncios de pagos**, selecciona la casilla de verificación en la columna **Activa** para cada [red de pago](#paid-networks) que quieras usar y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red).

8. Si has seleccionado una unidad de anuncios **Banner** o **Banner intersticial**, también verás una sección denominada **Otras redes de anuncios**. Las redes en esta sección no obtienen ingresos por las impresiones de anuncios. En su lugar, estas redes muestran anuncios que proceden de la campañas de promoción de la aplicación, entre otras.

    En la sección **Otras redes de anuncios**, selecciona la casilla de verificación en la columna **Activa** para cada [red que quieras usar](#other-networks) y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red). Actualmente se admiten las redes siguientes:

9. Puedes reemplazar la configuración de mediación de forma predeterminada en los mercados y para ello, selecciona **Destinado** en la lista desplegable y actualiza las selecciones de red de anuncios y la clasificación.

10. Haz clic en **Crear unidad de anuncios** (si vas a crear una nueva unidad de anuncio) o en **Guardar** (si estás editando una unidad de anuncios existente).

<span id="paid-networks" />
### <a name="supported-paid-ad-networks"></a>Redes de anuncios de pago admitidas

En la tabla siguiente se muestran las redes de pago que admitimos actualmente para cada tipo de anuncio. Ten en cuenta que algunas de estas redes [no están disponibles en todos los mercados](#network-markets).

|  Red de anuncios  |  Descripción  |  Tipos de anuncios admitidos  |
|--------------|---------------|---------------------|
| AOL y AppNexus |  Se trata de una red de anuncios de administrados por Microsoft que proporciona anuncios a través de nuestro socio de redes, AOL y AppNexus.<p/>**Nota**: AOL y AppNexus siempre están clasificadas en el primer lugar en la lista de **Redes de anuncios de pago** para unidades de anuncios Banner y no se pueden cambiar a una clasificación menor para estos tipos de anuncios. | Banner, Vídeo intersticial |
| AppNexus (directo) | Selecciona esta opción para proporcionar anuncios intersticiales en vídeo de [AppNexus](https://www.appnexus.com). | Vídeo intersticial, Nativo  |
| Anuncios para instalación de aplicaciones de Microsoft | Selecciona esta opción para proporcionar anuncios para instalación de aplicaciones o anuncios para volver a interactuar con la aplicación que han sido creados por otros desarrolladores del ecosistema de Windows que [crean campañas de anuncios promocionales para sus aplicaciones](create-an-ad-campaign-for-your-app.md).  |  Banner, Banner intersticial, Nativo  |
| Outbrain |  Selecciona esta opción para proporcionar anuncios desde [Outbrain](https://www.outbrain.com/). |  Banner  |
| Revcontent |  Selecciona esta opción para proporcionar anuncios desde [Revcontent](http://www.revcontent.com/). |  Banner  |
| Smaato |  Selecciona esta opción para proporcionar anuncios desde [Smaato](https://www.smaato.com/). |  Pancarta  |
| smartclip |  Selecciona esta opción para proporcionar anuncios desde [smartclip](http://www.smartclip.com/). |  Vídeo intersticial  |
| SpotX |  Selecciona esta opción para proporcionar anuncios desde [SpotX](https://www.spotx.tv/). |  Vídeo intersticial  |
| Taboola |  Selecciona esta opción para proporcionar anuncios desde [Taboola](https://www.taboola.com/). |  Pancarta  |


<span id="other-networks" />
### <a name="other-ad-networks"></a>Otras redes de anuncios

En la tabla siguiente se muestran las demás redes que admitimos actualmente para cada tipo de anuncio.

|  Red de anuncios  |  Descripción  |  Tipos de anuncios admitidos  |
|--------------|---------------|---------------------|
| Anuncios de la comunidad Microsoft |  Si creas una [campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncio de la comunidad](about-community-ads.md), selecciona esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial |
| Anuncios internos de Microsoft | Si [creas una campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncios internos](about-house-ads.md), selecciona esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial  |


<span id="network-markets" />
### <a name="supported-markets-for-ad-networks"></a>Mercados admitidos para las redes de anuncios

Las redes de anuncios disponibles proporcionan anuncios en todos los [mercados admitidos](define-pricing-and-market-selection.md#microsoft-store-consumer-markets), con las siguientes excepciones.

|  Red de anuncios  |  Mercados admitidos  |
|--------------|---------------------|
| Revcontent | Brasil, Canadá, Francia, Italia, Japón, España, Reino Unido, Estados Unidos  |
| Smaato | Brasil, Canadá, Francia, Italia, Japón, España, Reino Unido, Estados Unidos |
| smartclip | Austria, Bélgica, Dinamarca, Finlandia, Francia, Italia, Países Bajos, Noruega, Suecia, Suiza  |

<span id="coppa" />
## <a name="coppa-compliance"></a>Cumplimiento de COPPA

Al [crear una unidad de anuncios](#create-ad-unit) o [seleccionar una unidad de anuncios existente](#available-ad-units), la sección **Cumplimiento de COPPA** aparece en la parte inferior de la página de panel si la aplicación seleccionada de la unidad de anuncios tiene al menos un envío que ha llegado al paso [en Store](../publish/the-app-certification-process.md#in-the-store) del proceso de certificación de la aplicación.

Para cumplir con la Ley de protección de la privacidad infantil en línea ("COPPA"), debes seleccionar **This application is directed at children under the age of 13** de esta sección si tu aplicación está destinada a niños menores de 13 años. Si seleccionas esta opción, Microsoft tomará medidas para deshabilitar sus servicios de publicidad conductual al ofrecer publicidad en tu aplicación.

La configuración **Cumplimiento de COPPA** que elijas se aplica automáticamente a todas las unidades de anuncios de la aplicación seleccionada.

> [!IMPORTANT]
> Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA. Para obtener más información sobre tus obligaciones, consulta [esta página](http://go.microsoft.com/fwlink/p/?linkid=536558).
