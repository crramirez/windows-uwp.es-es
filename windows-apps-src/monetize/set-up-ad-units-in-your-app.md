---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: "Aprende a agregar valores de identificador de aplicación y de identificador de unidad de anuncios a la aplicación desde el panel del Centro de desarrollo de Windows antes de enviar la aplicación a la Tienda."
title: "Configurar unidades de anuncios en la aplicación"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, unidades de anuncios
ms.openlocfilehash: f96e81079764682a9f603fe93a9c123a69690507
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2017
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anuncios en la aplicación

Cada control de anuncio de banner, anuncio intersticial o anuncio nativo de la aplicación tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control. Cada unidad de anuncio consta de un *Id. de unidad de anuncio* y un *Id. de la aplicación*. Mientras desarrolles la aplicación, asigna los [valores de identificador de aplicación y de identificador de anuncios de prueba](test-mode-values.md) a tu control para confirmar que tu aplicación muestra anuncios de prueba. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación.

Cuando termines de probar la aplicación y estés listo para enviarla al Centro de desarrollo de Windows, debes actualizar el código de la aplicación para usar los valores de identificador de aplicación y de identificador de unidad de anuncios desde la página [Monetizar con anuncios](../publish/monetize-with-ads.md) del panel del Centro de desarrollo de Windows. Si intentas usar valores de prueba en la aplicación dinámica, la aplicación no recibirá anuncios dinámicos.

Para configurar las unidades de anuncios de la aplicación dinámica:

1.  En el panel del Centro de desarrollo de Windows, selecciona la aplicación y, a continuación, haga clic en **Monetización > Monetizar con anuncios**.

2.  En la sección **Crear unidades de anuncios** escribe un nombre para la unidad de anuncio en el campo **Nombre de la unidad de anuncios**.

3. En la lista desplegable **Tipo de unidad de anuncios**, selecciona el tipo de unidad de anuncios que corresponde a los anuncios que se muestran en el control:

    -   Si usas [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en tu aplicación para mostrar anuncios de banner, selecciona **Banner**.

    -   Si usas [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) en tu aplicación para mostrar anuncios de banner intersticial o de vídeo intersticial, selecciona **Vídeo intersticial** o **Banner intersticial** (asegúrate de seleccionar la opción adecuada para el tipo de anuncio intersticial que quieras mostrar).

    -   Si usas un [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) en tu aplicación para mostrar anuncios nativos, selecciona **Nativo**.
      > [!NOTE]
      > La capacidad para crear unidades de anuncios **Nativo** está solo disponible actualmente para seleccionar los desarrolladores que participan en un programa piloto, pero queremos que esta característica esté disponible para todos los desarrolladores pronto. Si estás interesado en unirte a nuestro programa piloto, ponte en contacto con nosotros en aiacare@microsoft.com.

4.  En la lista desplegable **Familia de dispositivos**, selecciona la familia de dispositivos a la que está dirigida la aplicación en la que se usará la unidad de anuncios. Las opciones disponibles son: **UWP (Windows10)**, **PC o tableta (Windows8.1)** o **Móvil (Windows Phone8.x)**.

5.  Haz clic en **Crear unidad de anuncios**. La nueva unidad de anuncios aparece en la parte superior de la lista en la sección **Unidades de anuncios disponibles** en esta página.

6.  Para cada unidad de anuncio generada, verás un **Id. de la aplicación** y un **Id. de unidad de anuncio**. Para mostrar anuncios en tu aplicación, tendrás que usar estos valores en el código de la aplicación:

    -   Si la aplicación muestra anuncios de banner, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) y [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) del objeto **AdControl**. Para obtener más información, consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md) y [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md).

    -   Si la aplicación muestra anuncios intersticiales, pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) del objeto **InterstitialAd**. Para obtener más información, consulta [Anuncios intersticiales](interstitial-ads.md).

    -   Si la aplicación muestra anuncios nativos, pase estos valores a los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obtener más información, consulta [Anuncios nativos](../monetize/native-ads.md).

7. Si tu aplicación es una aplicación para UWP para Windows10, tienes la opción de habilitar la mediación de anuncios para **AdControl** mediante la configuración de las opciones de la sección [Mediación de anuncios](../publish/monetize-with-ads.md#mediation). La mediación de anuncios te permite maximizar las capacidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago, como Taboola y Smaato, y anuncios de campañas de promoción de aplicaciones de Microsoft. De manera predeterminada, elegimos automáticamente la configuración de la mediación de anuncios para tu aplicación con algoritmos de aprendizaje automático que te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita, pero puedes elegir configurar manualmente las opciones de mediación.

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios controles de anuncios de banners, intersticiales y nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/monetize-with-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [Valores del modo de prueba](test-mode-values.md)
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](interstitial-ads.md)
* [Anuncios nativos](../monetize/native-ads.md)


 

 
