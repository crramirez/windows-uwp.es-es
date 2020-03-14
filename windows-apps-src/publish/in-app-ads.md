---
Description: Si la aplicación muestra anuncios mediante el SDK de Microsoft Advertising, use la página ADS en la aplicación del centro de partners para administrar el uso de los anuncios.
title: Anuncios en aplicaciones
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 03/25/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 96994566d19e03f1d85b751242331f04fef098ad
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210701"
---
# <a name="in-app-ads"></a>Anuncios en aplicaciones

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Use la página **monetizar** &gt; **ADS en la aplicación** del [centro de Partners](https://partner.microsoft.com/dashboard) para crear y administrar unidades de anuncios para:

* Aplicaciones de la Plataforma universal de Windows (UWP) que usan el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).
* Aplicaciones de Windows 8. x y Windows Phone 8. x publicadas previamente que usan el [SDK de Microsoft Advertising para Windows y Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).

> [!IMPORTANT]
> A partir del 31 de octubre de 2018, los productos recién creados no pueden incluir paquetes destinados a Windows 8. x/Windows Phone 8. x o una versión anterior. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para obtener más información sobre cómo integrar estos SDK con tus aplicaciones para mostrar anuncios, consulta [Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Crear unidades de anuncios

Para crear una unidad de anuncios para un [anuncio de banner](../monetize/banner-ads.md), [anuncio intersticial](../monetize/interstitial-ads.md) o [anuncio nativo](../monetize/native-ads.md) en tu app:

1.  Vaya a la página **monetizar** &gt; **anuncios en la aplicación** del centro de Partners y haga clic en **crear unidad de anuncio**.
2.  En la lista desplegable **Nombre de la aplicación**, selecciona la aplicación en la que se usará la unidad de anuncios.
3.  En el campo **Nombre de la unidad de anuncios**, escribe un nombre para esta. Esto puede ser cualquier cadena descriptiva que quieras usar para identificar la unidad de anuncios para fines informativos.
4.  En la lista desplegable **Tipo de unidad de anuncio**, selecciona el tipo de anuncio.

    * Si va a mostrar un banner en la aplicación, seleccione **banner**.
    * Si se muestra un anuncio de vídeo intersticial o un banner intersticial en la aplicación, seleccione **vídeo intersticial** o **banner intersticial** (Asegúrese de seleccionar la opción adecuada para el tipo de anuncio intersticial que desea mostrar).
    * Si va a mostrar un anuncio nativo en la aplicación, seleccione **nativo**.

5. En la lista desplegable **Familia de dispositivos**, selecciona la familia de dispositivos a la que está dirigida la aplicación en la que se usará la unidad de anuncios. Las opciones disponibles son: **UWP (Windows 10)** , **PC o tableta (Windows 8.1)** o **Móvil (Windows Phone 8.x)** .

6. Configura los siguientes valores adicionales según tus preferencias:

    * Si seleccionas la familia de dispositivos **UWP (Windows 10)** para la unidad de anuncio, también puedes configurar la [configuración de mediación](#mediation) para la unidad de anuncio.
    * Si seleccionas la familia de dispositivos **PC o tableta (Windows 8.1)** o **Móvil (Windows Phone 8.x)** para una unidad de anuncios de banner, también puedes seleccionar **Show community ads in your app** para participar en los [anuncios de la comunidad](about-community-ads.md).

7.  Si aún no has configurado el cumplimiento de COPPA para la app seleccionada, elige una opción en la sección [Cumplimiento de COPPA](#coppa).
8.  Haz clic en **Crear unidad de anuncios**.

Después de crear la nueva unidad de anuncio, ésta aparece en la tabla de unidades de anuncios disponibles en la página **monetizar** &gt; **anuncios de la aplicación** .

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Revisar y editar unidades de anuncios

Después de crear unidades de anuncio para una o varias aplicaciones de la cuenta, estas unidades de anuncio aparecen en una tabla en la parte inferior de la página de **monetización** &gt; **en la aplicación ADS** . Esta tabla muestra el **Id. de aplicación** y el **Id. de unidad de anuncio** para cada unidad de anuncio, junto con otra información. Para mostrar anuncios en tu aplicación, debes usar estos valores en el código. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](../monetize/set-up-ad-units-in-your-app.md).

* Si la aplicación muestra [anuncios de banner](../monetize/banner-ads.md), asigna estos valores a las propiedades [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) y [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) del objeto [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol).
* Si la aplicación muestra [anuncios intersticiales](../monetize/interstitial-ads.md), pasa estos valores al método [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) del objeto [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad).
* Si la aplicación muestra [anuncios nativos](../monetize/native-ads.md), pase estos valores al constructor **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

  > [!NOTE]
  > Puedes usar varios controles de anuncios de banners, intersticiales y nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

Para editar la [configuración de mediación](#mediation) para una unidad de anuncios para UWP o el [Cumplimiento de COPPA](#coppa) para la aplicación en la que se usa la unidad de anuncio, haz clic en el nombre de la unidad de anuncios.

> [!NOTE]
> Si una unidad de anuncio no tiene actividad durante los últimos seis meses, se etiquetará como **inactiva**y, finalmente, se quitará del centro de Partners. Puedes usar filtros para mostrar solo unidades de anuncios que sean de tipo **Activo** o **Inactivo**. Si ves unidades de anuncios que crees que se han marcado erróneamente como **Inactivo**, [ponte en contacto con soporte técnico](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Configuración de la mediación

Cuando [cree una nueva unidad de ad de UWP](#create-ad-unit) o [edite una existente](#available-ad-units), use las opciones de esta sección para configurar la [mediación de ad](../monetize/ad-mediation-service.md) para la unidad de anuncio. La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios que no generan ingresos para las campañas de promoción de las aplicaciones de Microsoft. Nosotros nos encargamos de arbitrar solicitudes de anuncios de banner desde las redes de anuncios que elijas. Si ya tienes una unidad de anuncios para UWP asociada a un anuncio de banner, intersticial o nativo en tu aplicación, al habilitar la mediación de anuncios no se necesitan cambios de código alguno en tu aplicación.

> [!NOTE]
> Al habilitar la mediación de anuncios para una unidad de anuncios para UWP, no es necesario obtener una unidad de anuncios de redes de anuncios de terceros. Nuestro servicio de mediación de anuncios creará automáticamente las unidades de anuncios de terceros que sean necesarias.

Configurar la configuración de mediación de anuncios para UWP en tu aplicación:

1. [Crear una unidad de anuncio](#create-ad-unit) o [seleccionar una unidad de anuncio existente](#available-ad-units).
2. En la página **ADS en la aplicación** , vaya a la sección **configuración de mediación** y configuración de la configuración.

    * De forma predeterminada, la casilla **permitir a Microsoft optimizar mi configuración** está activada. Te recomendamos que uses esta opción. Esta opción usa algoritmos de aprendizaje automático para elegir automáticamente la configuración de mediación de anuncios para tu aplicación que te ayudará a maximizar los ingresos por anuncios en todos los mercados admitidos por la aplicación. Al usar esta opción, también puede elegir las redes de ad que quiere usar en la configuración. Desactive las redes de ad que no quiera que formen parte de la configuración y nuestro algoritmo garantizará que la aplicación solo reciba anuncios de las redes de ad seleccionadas.
    * Si quiere elegir su propia configuración de la mediación de anuncios, elija **modificar la configuración predeterminada**.

    > [!NOTE]
    > Los pasos restantes de esta sección solo se aplican si elige **modificar la configuración predeterminada**.

3. En la lista desplegable **Dirigida** elige **Base de referencia** para configurar los ajustes predeterminados de la configuración de la mediación de anuncios. Esta configuración predeterminada se aplicará a todos los mercados, excepto los mercados donde establezcas las configuraciones específicas del mercado.
4. Después, especifica la relación de anuncios que quieras mostrar en el control de las redes de pago (de los que obtienes ingresos por las impresiones de anuncios) y otras redes de anuncios (de los que no obtienes ingresos por las impresiones de anuncios). Para ello, escribe un valor entre 0 y 100 en los campos **Peso** para **Redes de anuncios de pago** y **Otras redes de anuncios**.  
5. En la sección **Redes de anuncios de pagos**, selecciona la casilla de verificación en la columna **Activa** para cada [red de pago](#paid-networks) que quieras usar y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red).
6. Si has seleccionado una unidad de anuncios **Banner** o **Banner intersticial**, también verás una sección denominada **Otras redes de anuncios**. Las redes en esta sección no obtienen ingresos por las impresiones de anuncios. En su lugar, estas redes muestran anuncios que proceden de la campañas de promoción de la aplicación, entre otras.

    En la sección **Otras redes de anuncios**, selecciona la casilla de verificación en la columna **Activa** para cada [red que quieras usar](#other-networks) y, a continuación, usa las flechas en la columna **Clasificación** para ordenar las redes por clasificación (especifica la frecuencia de uso por parte de tu control de cada red). Actualmente se admiten las redes siguientes:

7. Puedes reemplazar la configuración de mediación de forma predeterminada en los mercados y para ello, selecciona **Destinado** en la lista desplegable y actualiza las selecciones de red de anuncios y la clasificación.
8. Haz clic en **Crear unidad de anuncios** (si vas a crear una nueva unidad de anuncio) o en **Guardar** (si estás editando una unidad de anuncios existente).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Redes de anuncios de pago admitidas

En la tabla siguiente se muestran las redes de pago que admitimos actualmente para cada tipo de anuncio. Ten en cuenta que algunas de estas redes [no están disponibles en todos los mercados](#network-markets).

|  Red de anuncios  |  Descripción  |  Tipos de anuncios admitidos  |
|--------------|---------------|---------------------|
| Oath y AppNexus |  Se trata de una red ad administrada por Microsoft que ofrece anuncios a través de nuestras redes de asociados, Oath y AppNexus.<p/>**Nota**: Oath y AppNexus siempre se clasifican en primer lugar en la lista de **redes de anuncios de pago** para las unidades de anuncio de banner y no se pueden cambiar a una clasificación inferior para estos tipos de anuncios. | Banner, Vídeo intersticial |
| AppNexus (directo) | Seleccione esta opción para servir anuncios desde [AppNexus](https://www.appnexus.com). | Vídeo intersticial, Nativo  |
| Anuncios para instalación de aplicaciones de Microsoft | Selecciona esta opción para proporcionar anuncios para instalación de aplicaciones o anuncios para volver a interactuar con la aplicación que han sido creados por otros desarrolladores del ecosistema de Windows que [crean campañas de anuncios promocionales para sus aplicaciones](create-an-ad-campaign-for-your-app.md).  |  Banner, Banner intersticial, Nativo  |
| Recomendaciones de contenido de MSN |  Seleccione esta opción para servir anuncios de recomendaciones de contenido de MSN. |  Banner, Banner intersticial  |
| Outbrain |  Selecciona esta opción para proporcionar anuncios desde [Outbrain](https://www.outbrain.com/). |  Banner, Banner intersticial  |
| Revcontent |  Selecciona esta opción para proporcionar anuncios desde [Revcontent](https://www.revcontent.com/). |  Banner, nativo  |
| Smaato |  Selecciona esta opción para proporcionar anuncios desde [Smaato](https://www.smaato.com/). |  Pancarta  |
| smartclip |  Selecciona esta opción para proporcionar anuncios desde [smartclip](http://www.smartclip.com/). |  Vídeo intersticial  |
| SpotX |  Selecciona esta opción para proporcionar anuncios desde [SpotX](https://www.spotx.tv/). |  Vídeo intersticial  |
| Taboola |  Selecciona esta opción para proporcionar anuncios desde [Taboola](https://www.taboola.com/). |  Pancarta  |
| Vungle | Seleccione esta opción para servir anuncios de [Vungle](https://vungle.com/) | Vídeo intersticial |
| Undertone | Seleccione esta opción para servir anuncios desde [undertone](https://www.undertone.com/). | Banner intersticial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Otras redes de anuncios

En la tabla siguiente se muestran las demás redes que admitimos actualmente para cada tipo de anuncio.

|  Red de anuncios  |  Descripción  |  Tipos de anuncios admitidos  |
|--------------|---------------|---------------------|
| Anuncios de la comunidad Microsoft |  Si creas una [campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncio de la comunidad](about-community-ads.md), selecciona esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial |
| Anuncios internos de Microsoft | Si [creas una campaña de anuncios internos para una de las aplicaciones](create-an-ad-campaign-for-your-app.md) y la configuras como una [campaña de anuncios internos](about-house-ads.md), selecciona esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Mercados admitidos para las redes de anuncios

Las redes de anuncios disponibles proporcionan anuncios en todos los [mercados admitidos](define-market-selection.md#microsoft-store-consumer-markets), con las siguientes excepciones.

|  Red de anuncios  |  Mercados admitidos  |
|--------------|---------------------|
| Revcontent | Brasil, Canadá, Francia, Italia, Japón, España, Reino Unido, Estados Unidos  |
| Smaato | Brasil, Canadá, Francia, Italia, Japón, España, Reino Unido, Estados Unidos |
| smartclip | Austria, Bélgica, Dinamarca, Finlandia, Francia, Italia, Países Bajos, Noruega, Suecia, Suiza  |
| Undertone | Estados Unidos |

<span id="coppa" />

## <a name="coppa-compliance"></a>Cumplimiento de COPPA

Al [crear una unidad de anuncio](#create-ad-unit) o [seleccionar una unidad de anuncio existente](#available-ad-units), la sección **cumplimiento de COPPA** aparece en la parte inferior de la página si la aplicación seleccionada para la unidad de anuncio tiene al menos un envío que ha alcanzado el [en el](../publish/the-app-certification-process.md#in-the-store) paso de la tienda en el proceso de certificación de la aplicación.

Para cumplir con la Ley de protección de la privacidad infantil en línea ("COPPA"), debes seleccionar **This application is directed at children under the age of 13** de esta sección si tu aplicación está destinada a niños menores de 13 años. Si seleccionas esta opción, Microsoft tomará medidas para deshabilitar sus servicios de publicidad conductual al ofrecer publicidad en tu aplicación.

La configuración **Cumplimiento de COPPA** que elijas se aplica automáticamente a todas las unidades de anuncios de la aplicación seleccionada.

> [!IMPORTANT]
> Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA. Para obtener más información sobre tus obligaciones, consulta [esta página](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
