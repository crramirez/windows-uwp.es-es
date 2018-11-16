---
author: Xansky
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Obtén información sobre cómo agregar los valores de Id. de unidad de ad y el Id. de aplicación del centro de partners a la aplicación antes de enviar la aplicación a la tienda.
title: Configurar unidades de anuncios en la aplicación
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, unidades de anuncios, pruebas
ms.localizationpriority: medium
ms.openlocfilehash: 11c66756d95e041a45fbc075b02eb744bf542871
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6842364"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anuncios en la aplicación

Cada control de anuncio de la aplicación de Plataforma universal de Windows (UWP) tiene una *unidad de anuncio* correspondiente que se usa por nuestros servicios para proporcionar anuncios al control. Cada unidad de anuncio consta de un *Id. de unidad de anuncio* e *Id. de aplicación* que debes asignar al código en tu aplicación.

Proporcionamos [valores de unidad de anuncios de prueba](#test-ad-units) que puedes usar durante las pruebas para confirmar que la aplicación muestra anuncios de prueba. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Si intentas usar valores de prueba en tu aplicación después de publicarla, la aplicación dinámica no recibirá anuncios.

Cuando termines de probar tu aplicación para UWP y estás listo para enviarla al centro de partners, debes [crear una unidad de anuncios dinámicos](#live-ad-units) desde la página de [anuncios en la aplicación](../publish/in-app-ads.md) en el centro de partners y actualizar el código de tu aplicación para usar los valores de Id. de la aplicación y el Id. unidad para esta unidad de anuncios.

Para obtener más información sobre cómo asignar los valores de Id. de aplicación e Id. de unidad de anuncio en el código de la aplicación, consulta los siguientes artículos:
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](../monetize/interstitial-ads.md)
* [Anuncios nativos](../monetize/native-ads.md)

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

Para obtener una unidad de anuncios dinámicos desde el centro de partners y usarla en tu aplicación:

1.  [Crear una unidad de anuncio](../publish/in-app-ads.md#create-ad-unit) en la página de **anuncios en la aplicación** en el centro de partners. Asegúrate de especificar el tipo correcto de la unidad de anuncio para el control de anuncios que usas en tu aplicación.
    > [!NOTE]
    > Tienes la opción de habilitar la mediación de anuncios tu la unidad de anuncios mediante la configuración de las opciones de la sección [Configuración de la mediación](../publish/in-app-ads.md#mediation). La mediación de anuncios te permite maximizar las funcionalidades de ingresos por anuncios y de promoción de la aplicación al mostrar anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios para las campañas de promoción de las aplicaciones de Microsoft. De manera predeterminada, elegimos automáticamente la configuración de la mediación de anuncios para tu aplicación con algoritmos de aprendizaje automático que te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita, pero puedes elegir configurar manualmente las opciones de mediación.

2.  Después de crear la nueva unidad de anuncio, recupera el **Id. de aplicación** e **Id. de unidad de anuncio** para la unidad de anuncio en la página **Monetizar** &gt; **Anuncios desde la aplicación**.
    > [!NOTE]
    > Los valores de id. de la aplicación para unidades de anuncios de prueba y unidades de anuncios dinámicas de UWP tienen diferentes formatos. Los valores de id. de la aplicación de prueba son GUID. Cuando se crea una unidad de anuncios dinámica de UWP en el centro de partners, el valor de Id. de aplicación de la unidad de anuncios siempre coincide con el identificador de la tienda de la aplicación (un valor de Id. de Store parece a 9NBLGGH4R315).

3.  Asigna los valores de Id. de aplicación e Id. de unidad de anuncio en el código de tu aplicación. Para obtener más información, consulta los artículos siguientes:
    * [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
    * [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Anuncios intersticiales](../monetize/interstitial-ads.md)
    * [Anuncios nativos](../monetize/native-ads.md)

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


 

 
