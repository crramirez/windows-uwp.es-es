---
ms.assetid: bb105fbe-bbbd-4d78-899b-345af2757720
description: Obtenga información sobre cómo agregar valores de ID. de aplicación y de identificador de unidad de ad del centro de partners a la aplicación antes de enviar la aplicación a la tienda.
title: Configurar unidades de anuncios en la aplicación
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, unidades de anuncios, pruebas
ms.localizationpriority: medium
ms.openlocfilehash: c7bafdc7d21814a03d6f7da7132d8017d7f238e5
ms.sourcegitcommit: c2e4bbe46c7b37be1390cdf3fa0f56670f9d34e9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/20/2020
ms.locfileid: "92253637"
---
# <a name="set-up-ad-units-in-your-app"></a>Configurar unidades de anuncios en la aplicación

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Cada control de ad de la aplicación Plataforma universal de Windows (UWP) tiene una *unidad de anuncio* correspondiente que los servicios usan para servir anuncios al control. Cada unidad de anuncio está formada por un *identificador de unidad de ad* y un identificador de *aplicación* que se deben asignar al código de la aplicación.

Proporcionamos [valores de unidad de ad de prueba](#test-ad-units) que puede usar durante las pruebas para confirmar que la aplicación muestra los anuncios de prueba. Estos valores de prueba solo se pueden usar en una versión de prueba de la aplicación. Si intentas usar valores de prueba en tu aplicación después de publicarla, la aplicación dinámica no recibirá anuncios.

Después de finalizar la prueba de la aplicación para UWP y de que esté listo para enviarla al centro de Partners, debe [crear una unidad de AD en directo](#live-ad-units) desde la página [ADS en la aplicación](../publish/in-app-ads.md) del centro de Partners y actualizar el código de la aplicación para que use los valores ID. de la aplicación y identificador de unidad de AD para esta unidad de anuncio.

Para obtener más información sobre la asignación de los valores de ID. de aplicación y identificador de unidad de AD en el código de la aplicación, consulte los siguientes artículos:
* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](../monetize/interstitial-ads.md)
* [Anuncios nativos](../monetize/native-ads.md)

<span id="test-ad-units" />

## <a name="test-ad-units"></a>Unidades de anuncio de prueba

Mientras desarrolla la aplicación, use los valores ID. de aplicación de prueba y identificador de unidad de ad de esta sección para ver cómo la aplicación representa los anuncios durante las pruebas.

### <a name="banner-ads-using-the-adcontrol-class"></a>Anuncios de banner (mediante la clase AdControl)

* IDENTIFICADOR de unidad de ad: ```test```
* IDENTIFICADOR de la aplicación:  ```3f83fe91-d6be-434d-a0ae-7351c5a997f1```

    > [!IMPORTANT]
    > Para un **control AdControl**, el tamaño de un anuncio en directo se define mediante las propiedades **width** y **height** . Para obtener resultados óptimos, asegúrate de que las propiedades **Width** y **Height** del código corresponden a uno de los [tamaños de anuncios admitidos para anuncios de banner](supported-ad-sizes-for-banner-ads.md). Las propiedades **Width** y **Height** no cambiarán en función del tamaño de un anuncio dinámico.

### <a name="interstitial-ads-and-native-ads"></a>Anuncios intersticiales y anuncios nativos

* IDENTIFICADOR de unidad de ad: ```test```
* IDENTIFICADOR de la aplicación:  ```d25517cb-12d4-4699-8bdc-52040c712cab```

<span id="live-ad-units" />

## <a name="live-ad-units"></a>Unidades de anuncios en directo

Para obtener una unidad de AD en directo del centro de Partners y usarla en la aplicación:

1.  [Cree una unidad de anuncio](../publish/in-app-ads.md#create-ad-unit) en la página **ADS en la aplicación** del centro de Partners. Asegúrese de especificar el tipo correcto de unidad de anuncio para el control de ad que está usando en la aplicación.
    > [!NOTE]
    > Opcionalmente, puede habilitar la mediación de anuncios para su unidad de anuncio configurando los valores en la sección [configuración de mediación](../publish/in-app-ads.md#mediation) . La mediación de anuncios permite maximizar las funcionalidades de promoción de ingresos y de las aplicaciones de anuncios mediante la visualización de anuncios de varias redes de anuncios, incluidos los anuncios de otras redes de anuncios de pago y anuncios para las campañas de promoción de aplicaciones de Microsoft. De forma predeterminada, elegimos automáticamente la configuración de la mediación de anuncios para tu aplicación con algoritmos de aprendizaje automático para ayudarte a maximizar tus ingresos de publicidad en los mercados que admita la aplicación, pero puedes configurar manualmente las opciones de mediación.

2.  Después de crear la nueva unidad de anuncio, recupere el identificador de la **aplicación** y el identificador de la **unidad** de anuncio de la unidad de anuncios en la tabla de unidades de anuncios disponibles en la página **monetizar** &gt; **en la aplicación** .
    > [!NOTE]
    > Los valores de ID. de aplicación para las unidades de anuncios de prueba y las unidades de anuncio de UWP en directo tienen distintos formatos. Los valores de ID. de aplicación de prueba son GUID. Cuando se crea una unidad de ad de UWP activa en el centro de Partners, el valor de identificador de la aplicación de la unidad de anuncio coincide siempre con el identificador de almacén de la aplicación (un valor de identificador de almacén de ejemplo es similar a 9NBLGGH4R315).

3.  Asigne los valores ID. de aplicación y identificador de unidad de AD en el código de la aplicación. Para más información, consulte los siguientes artículos.
    * [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
    * [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
    * [Anuncios intersticiales](../monetize/interstitial-ads.md)
    * [Anuncios nativos](../monetize/native-ads.md)

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>Administrar unidades de anuncios para varios controles de AD en la aplicación

Puede usar varios controles Banner, intersticiales y de ad nativo en una sola aplicación. En este escenario, se recomienda asignar una unidad de anuncio diferente a cada control. El uso de diferentes unidades de anuncio para cada control le permite [configurar por separado los valores de mediación](../publish/in-app-ads.md#mediation) y obtener [datos de informes](../publish/advertising-performance-report.md) discretos para cada control. Esto también permite a nuestros servicios optimizar mejor los anuncios que sirven a su aplicación.

> [!IMPORTANT]
> Puede usar cada unidad de anuncio en una sola aplicación. Si usa una unidad de anuncio en más de una aplicación, los anuncios no se servirán para esa unidad de ad.

## <a name="related-topics"></a>Temas relacionados

* [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
* [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
* [Anuncios intersticiales](interstitial-ads.md)
* [Anuncios nativos](native-ads.md)


 

 
