---
description: Si la aplicación muestra anuncios mediante el SDK de Microsoft Advertising, use la página ADS en la aplicación del centro de partners para administrar el uso de los anuncios.
title: Anuncios en aplicaciones
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0951a26f0475d9565e783720263a6c6be9f2f08
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033938"
---
# <a name="in-app-ads"></a>Anuncios en aplicaciones

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Use la página de anuncios de **monetización** &gt; **en la aplicación** del [centro de Partners](https://partner.microsoft.com/dashboard) para crear y administrar unidades de anuncios para:

* Aplicaciones Plataforma universal de Windowss (UWP) que usan el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK).
* Aplicaciones de Windows 8. x y Windows Phone 8. x publicadas previamente que usan el [SDK de Microsoft Advertising para Windows y Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x).

> [!IMPORTANT]
> Ya no se pueden cargar nuevos paquetes XAP compilados con los SDK de Windows Phone 8. x. Las aplicaciones que ya se encuentran en el almacén con paquetes XAP seguirán funcionando en dispositivos Windows 10 Mobile. Para obtener más información, consulte esta [entrada de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para obtener más información sobre cómo integrar estos SDK con las aplicaciones para mostrar anuncios, consulte [mostrar anuncios en la aplicación con el SDK de Microsoft Advertising](../monetize/display-ads-in-your-app.md).

<span id="create-ad-unit" />

## <a name="create-ad-units"></a>Crear unidades de anuncio

Para crear una unidad de anuncio para un [banner](../monetize/banner-ads.md), anuncio [intersticial](../monetize/interstitial-ads.md)o [ad nativo](../monetize/native-ads.md) en la aplicación:

1.  Vaya a la **Monetize** &gt; página **de anuncios de** monetización en la aplicación del centro de Partners y haga clic en **crear unidad de anuncio** .
2.  En el menú desplegable nombre de la **aplicación** , seleccione la aplicación en la que se usará la unidad de anuncio.
3.  En el campo **nombre de unidad de ad** , escriba un nombre para la unidad de anuncio. Puede ser cualquier cadena descriptiva que desee utilizar para identificar la unidad de anuncio con fines informativos.
4.  En la lista desplegable **tipo de unidad de anuncio** , seleccione el tipo de anuncio.

    * Si va a mostrar un banner en la aplicación, seleccione **banner** .
    * Si se muestra un anuncio de vídeo intersticial o un banner intersticial en la aplicación, seleccione **vídeo intersticial** o **banner intersticial** (Asegúrese de seleccionar la opción adecuada para el tipo de anuncio intersticial que desea mostrar).
    * Si va a mostrar un anuncio nativo en la aplicación, seleccione **nativo** .

5. En la lista desplegable **familia de dispositivos** , seleccione la familia de dispositivos de destino de la aplicación en la que se usará la unidad de anuncio. Las opciones disponibles son: **UWP (Windows 10)** , **PC/tableta (Windows 8.1)** o **móvil (Windows Phone 8. x)** .

6. Configure las siguientes opciones adicionales según sea necesario:

    * Si selecciona la familia de dispositivos **UWP (Windows 10)** para la unidad de anuncio, puede configurar opciones de [mediación](#mediation) para la unidad de anuncio.
    * Si selecciona la familia de dispositivos **PC/tableta (Windows 8.1)** o **móvil (Windows Phone 8. x)** para una unidad de anuncio de Banner, también puede seleccionar **mostrar anuncios de la comunidad en la aplicación** para participar en los anuncios de la [comunidad](../monetize/index.md).

7.  Si aún no ha establecido el cumplimiento de COPPA para la aplicación seleccionada, elija una opción en la sección [cumplimiento de COPPA](#coppa) .
8.  Haz clic en **Crear unidad de anuncios** .

Después de crear la nueva unidad de anuncio, ésta aparece en la tabla de unidades de anuncios disponibles en la página **monetizar** &gt; **en la aplicación ADS** .

<span id="available-ad-units" />

## <a name="review-and-edit-ad-units"></a>Revisar y editar unidades de anuncio

Después de crear unidades de anuncio para una o varias aplicaciones en su cuenta, estas unidades de anuncio aparecen en una tabla en la parte inferior de la página de los anuncios de **monetización** &gt; **de la aplicación** . En esta tabla se muestra el identificador de la **aplicación** y el identificador de la **unidad de anuncio** para cada unidad de anuncio, junto con otra información. Para mostrar los anuncios en la aplicación, debe usar estos valores en el código. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](../monetize/set-up-ad-units-in-your-app.md).

* Si la aplicación muestra [anuncios de banner](../monetize/banner-ads.md), asigne estos valores a las propiedades [ApplicationID](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) y [AdUnitId](/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) del objeto [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) .
* Si la aplicación muestra [anuncios intersticiales](../monetize/interstitial-ads.md), pase estos valores al método [RequestAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) del objeto [InterstitialAd](/uwp/api/microsoft.advertising.winrt.ui.interstitialad) .
* Si la aplicación muestra [anuncios nativos](../monetize/native-ads.md), pase estos valores al constructor **NativeAdsManagerV2** .
  > [!IMPORTANT]
  > Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

  > [!NOTE]
  > Puede usar varios controles Banner, intersticiales y de ad nativo en una sola aplicación. En este escenario, se recomienda asignar una unidad de anuncio diferente a cada control. El uso de diferentes unidades de anuncio para cada control le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

Para editar la [configuración de mediación](#mediation) de una unidad de anuncio de UWP o el [cumplimiento de COPPA](#coppa) para la aplicación en la que se usa la unidad de anuncios, haga clic en el nombre de la unidad de anuncio.

> [!NOTE]
> Si una unidad de anuncio no tiene actividad durante los últimos seis meses, se etiquetará como **inactiva** y, finalmente, se quitará del centro de Partners. Puede usar filtros para mostrar solo las unidades de anuncios **activas** o **inactivas** . Si ve que las unidades de anuncios que cree están marcadas incorrectamente como **inactivas** , [póngase en contacto con el soporte técnico](https://developer.microsoft.com/windows/support).

<span id="mediation" />

## <a name="mediation-settings"></a>Configuración de mediación

Cuando [cree una nueva unidad de ad de UWP](#create-ad-unit) o [edite una existente](#available-ad-units), use las opciones de esta sección para configurar la [mediación de ad](../monetize/ad-mediation-service.md) para la unidad de anuncio. La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios de no generación de ingresos para las campañas de promoción de aplicaciones de Microsoft. Nos encargamos de la mediación de solicitudes de Banner publicitario desde las redes de ad que elija. Si tiene una unidad de anuncio de UWP que ya está asociada a un banner, un mensaje intersticial o un anuncio nativo en la aplicación, la habilitación de la mediación de anuncios no requiere ningún cambio de código en la aplicación.

> [!NOTE]
> Cuando se habilita la mediación de anuncios para una unidad de anuncio de UWP, no es necesario obtener una unidad de anuncio de redes de ad de terceros. El servicio de mediación de anuncios crea automáticamente las unidades de anuncio de terceros necesarias.

Para configurar los valores de mediación de AD para una unidad de anuncio de UWP en la aplicación:

1. [Cree una unidad de anuncio](#create-ad-unit) o [Seleccione una unidad de anuncio existente](#available-ad-units).
2. En la página **ADS en la aplicación** , vaya a la sección **configuración de mediación** y configuración de la configuración.

    * De forma predeterminada, la casilla **permitir a Microsoft optimizar mi configuración** está activada. Se recomienda usar esta opción. Esta opción usa algoritmos de aprendizaje automático para elegir automáticamente la configuración de la mediación de anuncios de la aplicación para ayudarle a maximizar los ingresos de publicidad en los mercados que admite la aplicación. Al usar esta opción, también puede elegir las redes de ad que quiere usar en la configuración. Desactive las redes de ad que no quiera que formen parte de la configuración y nuestro algoritmo garantizará que la aplicación solo reciba anuncios de las redes de ad seleccionadas.
    * Si quiere elegir su propia configuración de la mediación de anuncios, elija **modificar la configuración predeterminada** .

    > [!NOTE]
    > Los pasos restantes de esta sección solo se aplican si elige **modificar la configuración predeterminada** .

3. En el menú desplegable **destino** , elija **línea de base** para establecer la configuración predeterminada para la configuración de la mediación de anuncios. Esta configuración predeterminada se aplicará a todos los mercados, excepto para los mercados en los que se definen las configuraciones específicas del mercado.
4. A continuación, especifique la proporción de anuncios que desea mostrar en el control de las redes de pago (que pagan sus ingresos por impresiones) y otras redes de anuncios (que no pagan sus ingresos por impresiones). Para ello, escriba un valor entre 0 y 100 en los campos **peso** de las **redes de anuncios de pago** y **otras redes de anuncios** .  
5. En la **sección redes de anuncios de pago** , active la casilla de la columna **activo** para cada [red de pago](#paid-networks) que desee usar y, a continuación, use las flechas de la columna **rango** para ordenar las redes por rango (especifica la frecuencia con que el control debe usar cada red).
6. Si ha seleccionado una unidad de anuncio **intersticial** de **banner o** Banner, también verá una sección denominada **otras redes de ad** . Las redes de esta sección no obtienen ingresos por impresiones de anuncios. En su lugar, estas redes muestran anuncios de orígenes como campañas de promoción de aplicaciones.

    En la sección **otras redes de ad** , active la casilla en la columna **activa** para cada una de las [demás redes](#other-networks) que desee usar y, a continuación, use las flechas de la columna **rango** para ordenar las redes por rango (especifica con qué frecuencia debe usarse cada red el control). Actualmente se admiten las siguientes redes:

7. Para cada mercado en el que quiera invalidar la configuración de mediación predeterminada, seleccione el mercado en el menú desplegable **destino** y actualice las selecciones y la clasificación de la red de ad.
8. Haga clic en **crear unidad de anuncio** (si va a crear una nueva unidad de anuncio) o en **Guardar** (si está editando una unidad de anuncio existente).

<span id="paid-networks" />

### <a name="supported-paid-ad-networks"></a>Redes de anuncios de pago compatibles

En la tabla siguiente se enumeran las redes de pago que actualmente se admiten para cada tipo de anuncio. Tenga en cuenta que algunas de estas redes [no están disponibles en todos los mercados](#network-markets).

|  Red de anuncios  |  Description  |  Tipos de ad compatibles  |
|--------------|---------------|---------------------|
| Oath y AppNexus |  Se trata de una red ad administrada por Microsoft que ofrece anuncios a través de nuestras redes de asociados, Oath y AppNexus.<p/>**Nota** : Oath y AppNexus siempre se clasifican en primer lugar en la lista de **redes de anuncios de pago** para las unidades de anuncio de banner y no se pueden cambiar a una clasificación inferior para estos tipos de anuncios. | Banner, vídeo intersticial |
| AppNexus (directo) | Seleccione esta opción para servir anuncios desde [AppNexus](https://www.appnexus.com). | Vídeo intersticial, nativo  |
| Anuncios de instalación de aplicaciones de Microsoft | Seleccione esta opción para servir anuncios de instalación de aplicaciones o anuncios de renegociación de aplicaciones creados por otros desarrolladores del ecosistema de Windows que [crean campañas publicitarias promocionales para sus aplicaciones](../monetize/index.md).  |  Banner, Banner intersticial, nativo  |
| Recomendaciones de contenido de MSN |  Seleccione esta opción para servir anuncios de recomendaciones de contenido de MSN. |  Banner, Banner intersticial  |
| Outcerebro |  Seleccione esta opción para servir anuncios desde [outcerebro](https://www.outbrain.com/). |  Banner, Banner intersticial  |
| Revcontent |  Seleccione esta opción para servir anuncios desde [Revcontent](https://www.revcontent.com/). |  Banner, nativo  |
| Smaato |  Seleccione esta opción para servir anuncios desde [Smaato](https://www.smaato.com/). |  Banner  |
| smartclip |  Seleccione esta opción para servir anuncios desde [smartclip](http://www.smartclip.com/). |  Vídeo intersticial  |
| SpotX |  Seleccione esta opción para servir anuncios desde [SpotX](https://www.spotx.tv/). |  Vídeo intersticial  |
| Taboola |  Seleccione esta opción para servir anuncios desde [Taboola](https://www.taboola.com/). |  Banner  |
| Vungle | Seleccione esta opción para servir anuncios de [Vungle](https://vungle.com/) | Vídeo intersticial |
| Undertone | Seleccione esta opción para servir anuncios desde [undertone](https://www.undertone.com/). | Banner intersticial |


<span id="other-networks" />

### <a name="other-ad-networks"></a>Otras redes de anuncios

En la tabla siguiente se enumeran las otras redes que actualmente se admiten para cada tipo de anuncio.

|  Red de anuncios  |  Description  |  Tipos de ad compatibles  |
|--------------|---------------|---------------------|
| Anuncios de la comunidad de Microsoft |  Si [crea una campaña de publicidad promocional para una de sus aplicaciones](../monetize/index.md) y configura esta campaña como una [campaña de anuncios](../monetize/index.md)de la comunidad, seleccione esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial |
| Anuncios de Microsoft House | Si [crea una campaña de publicidad promocional para una de sus aplicaciones](../monetize/index.md) y configura esta campaña como una [campaña de anuncios de casa](../monetize/index.md), seleccione esta opción para mostrar anuncios de esta campaña. | Banner, Banner intersticial  |


<span id="network-markets" />

### <a name="supported-markets-for-ad-networks"></a>Mercados admitidos para redes de ad

Las redes de anuncios disponibles sirven anuncios en todos los [mercados admitidos](define-market-selection.md#microsoft-store-consumer-markets), con las siguientes excepciones.

|  Red de anuncios  |  Mercados admitidos  |
|--------------|---------------------|
| Revcontent | Brasil, Canadá, Francia, Alemania, Italia, Japón, España, Reino Unido, Estados Unidos  |
| Smaato | Brasil, Canadá, Francia, Alemania, Italia, Japón, España, Reino Unido, Estados Unidos |
| smartclip | Austria, Bélgica, Dinamarca, Finlandia, Alemania, Italia, Países Bajos, Noruega, Suecia, Suiza  |
| Undertone | Estados Unidos |

<span id="coppa" />

## <a name="coppa-compliance"></a>Cumplimiento de COPPA

Al [crear una unidad de anuncio](#create-ad-unit) o [seleccionar una unidad de anuncio existente](#available-ad-units), la sección **cumplimiento de COPPA** aparece en la parte inferior de la página si la aplicación seleccionada para la unidad de anuncio tiene al menos un envío que ha alcanzado el [en el](../publish/the-app-certification-process.md#in-the-store) paso de la tienda en el proceso de certificación de la aplicación.

En lo que respecta a la ley de protección de la privacidad en línea de los niños ("COPPA"), debe seleccionar **esta aplicación se dirige a los niños con una antigüedad inferior a 13** en esta sección si la aplicación se dirige a niños menores de 13 años. Si selecciona esta opción, Microsoft llevará a cabo los pasos necesarios para deshabilitar los servicios de publicidad de comportamiento al entregar anuncios en la aplicación.

La configuración de **cumplimiento de COPPA** que elija se aplicará automáticamente a todas las unidades de anuncios de la aplicación seleccionada.

> [!IMPORTANT]
> Si la aplicación se dirige a niños menores de 13 años, tienes determinadas obligaciones en virtud de la COPPA. Para obtener más información sobre sus obligaciones, consulte [esta página](https://www.ftc.gov/enforcement/rules/rulemaking-regulatory-reform-proceedings/childrens-online-privacy-protection-rule).
