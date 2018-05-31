---
author: mcleanbyron
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Aprende a agregar valores de identificador de aplicación y de identificador de unidad de anuncios a la aplicación desde el panel del Centro de desarrollo de Windows antes de enviar la aplicación a la Store.
title: Configurar unidades de anuncios en la aplicación
ms.author: mcleans
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, unidades de anuncios, pruebas
ms.localizationpriority: medium
ms.openlocfilehash: 6473d571ed44f60f8001b8a565d70c58d43407d1
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
ms.locfileid: "1654664"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anuncios en la aplicación

Cada control de anuncio de la aplicación de Plataforma universal de Windows (UWP) tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control. Cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación* que debes asignar a [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx), [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) o [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) en tu aplicación.

Proporcionamos [valores de unidad de anuncios de prueba](#test-ad-units) que puedes usar durante las pruebas para confirmar que la aplicación muestra anuncios de prueba. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Si intentas usar valores de prueba en tu aplicación después de publicarla, la aplicación dinámica no recibirá anuncios.

Cuando termines de probar la aplicación para UWP y estés listo para enviarla al Centro de desarrollo de Windows, debes [crear una unidad de anuncios dinámicos](#live-ad-units) en la página [Anuncios desde la aplicación](../publish/in-app-ads.md) en el panel de información del Centro de desarrollo de Windows y actualizar el código de la aplicación para usar el Id. de la aplicación y el Id. de unidad de anuncios.

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Probar unidades de anuncios

Mientras desarrollas la aplicación, usa los valores de identificador de aplicación y de identificador de anuncios de prueba de esta sección para ver cómo la aplicación representa los anuncios durante las pruebas.

### <a name="banner-ads-using-the-adcontrol-class"></a>Anuncios de banner (mediante la clase AdControl)

* Id. de unidad de anuncio: ```test```
* Id. de aplicación:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Para **AdControl**, el tamaño de un anuncio dinámico se define por las propiedades **Width** y **Height**. Para obtener resultados óptimos, asegúrate de que las propiedades **Width** y **Height** del código corresponden a uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md). Las propiedades **Width** y **Height** no cambiarán en función del tamaño de un anuncio dinámico.

### <a name="interstitial-ads-and-native-ads"></a>Anuncios intersticiales y anuncios nativos

* Id. de unidad de anuncio: ```test```
* Id. de aplicación:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unidades de anuncios dinámicos

Para obtener una unidad de anuncios dinámicos desde el panel de información del Centro de desarrollo y usarla en tu aplicación:

1.  [Crear una unidad de anuncio](../publish/in-app-ads.md#create-ad-unit) en la página **Anuncios desde la aplicación** en el panel. Asegúrate de especificar el tipo correcto de la unidad de anuncio para el control de anuncios que usas en tu aplicación.
    > [!NOTE]
    > Tienes la opción de habilitar la mediación de anuncios tu la unidad de anuncios mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation). La mediación de anuncios te permite maximizar las funcionalidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios para las campañas de promoción de las aplicaciones de Microsoft. De manera predeterminada, elegimos automáticamente la configuración de la mediación de anuncios para tu aplicación con algoritmos de aprendizaje automático que te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita, pero puedes elegir configurar manualmente las opciones de mediación.

2.  Después de crear la nueva unidad de anuncio, recupera el **Id. de aplicación** e **Id. de unidad de anuncio** para la unidad de anuncio en la página **Monetizar** &gt; **Anuncios desde la aplicación**.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicas de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Al crear una unidad de anuncio dinámico para UWP en el panel de información, el valor de id. de la aplicación de la unidad de anuncios siempre coincide con el id. de Microsoft Store de la aplicación (un ejemplo de valor de id. de Microsoft Store aparece de forma semejante a 9NBLGGH4R315).

3.  Asigna los valores de **Id. de aplicación** e **Id. de unidad de anuncio** en el código de tu aplicación:

    * Si la aplicación muestra anuncios de banner, asigna estos valores a las propiedades [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) y [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) del objeto [AdControl](https://msdn.microsoft.com/library/mt313154.aspx). Para obtener más información, consulta [AdControl en XAML y .NET](../monetize/adcontrol-in-xaml-and--net.md) y [AdControl en HTML5 y JavaScript](../monetize/adcontrol-in-html-5-and-javascript.md).

    * Si la aplicación muestra anuncios intersticiales, pasa estos valores al método [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) del objeto [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx). Para obtener más información, consulta [Anuncios intersticiales](../monetize/interstitial-ads.md).

    * Si la aplicación muestra anuncios nativos, pase estos valores a los parámetros *applicationId* y *adUnitId* del constructor [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx). Para obtener más información, consulta [Anuncios nativos](../monetize/native-ads.md).

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de anuncios en tu aplicación

Puedes usar varios controles de anuncios de banners, intersticiales y nativos en una sola aplicación. En este escenario, te recomendamos que asignes una unidad de anuncios diferente para cada control. Con unidades de anuncios diferentes para cada control podrás, por separado, [configurar las opciones de mediación](../publish/in-app-ads.md#mediation) y obtener discretos [datos de informes](../publish/advertising-performance-report.md) para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que publicamos en tu aplicación.

> [!IMPORTANT]
> Puedes usar cada unidad de anuncios solo en una aplicación. Si usas una unidad de anuncios en más de una aplicación, no se publicarán anuncios para esa unidad de anuncios.

## <a name="related-topics"></a>Temas relacionados

* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](interstitial-ads.md)
* [Anuncios nativos](native-ads.md)


 

 
