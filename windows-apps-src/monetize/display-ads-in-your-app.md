---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "El SDK de Microsoft Advertising te ofrece varios métodos para monetizar la aplicación con anuncios."
title: "Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anuncios, publicidad, banner, control de anuncios, intersticial
ms.openlocfilehash: 4730ebaf55af8e7063c444d5b3bbd973b0508db2
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/21/2017
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising

Aumenta las oportunidades de ingresos poniendo anuncios en tus aplicaciones mediante el SDK de Microsoft Advertising. Nuestra plataforma de monetización de anuncios ofrece distintos tipos de anuncios y admite mediación con muchas redes de anuncios populares.

Para mostrar anuncios en tus aplicaciones, sigue estos pasos.

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Paso 1: Instalar el SDK de Microsoft Advertising

El SDK de Microsoft Advertising proporciona una variedad de controles que puedes usar en tu aplicación para mostrar diferentes tipos de anuncios. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-choose-your-ad-type-and-add-code-to-display-test-ads-in-your-app"></a>Paso 2: Elegir el tipo de anuncio y agregar código para mostrar anuncios de prueba en tu aplicación

El SDK de Microsoft Advertising ofrece varios tipos de anuncios distintos que puede mostrar en la aplicación. Elige qué tipos de anuncios son mejores para tu escenario y, a continuación, agrega código a la aplicación para mostrar esos anuncios.

Debes especificar un identificador de aplicación y el id. de la unidad de anuncio en el código para proporcionar anuncios a tu aplicación. Mientras desarrolles la aplicación, debes usar los [valores de identificador de aplicación y de identificador de anuncios de prueba](test-mode-values.md) para ver cómo la aplicación representa los anuncios durante las pruebas.

### <a name="banner-ads"></a>Anuncios de banner

Estos son anuncios estáticos que usan una parte de la página de la aplicación, a menudo la superior o inferior.

Para ver instrucciones y ejemplos de código, consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md) y [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md). Para obtener proyectos de muestra completos que muestran cómo agregar anuncios de banner a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

![addreferences](images/banner-ad.png)

### <a name="interstitial-ads"></a>Anuncios intersticiales

Estos son anuncios de pantalla completa que requieren normalmente que el usuario vea un vídeo o haga clic en ellos para continuar en la aplicación o el juego. Admitimos dos tipos de anuncios intersticiales: vídeo y banner.

Para ver instrucciones y ejemplos de código, consulta [Anuncios intersticiales](interstitial-ads.md). Para obtener proyectos de muestra completos que muestran cómo agregar anuncios intersticiales a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Anuncios nativos

Se trata de anuncios basados en componentes. Cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual que puedes integrar en la aplicación con tus propias fuentes, colores y otros componentes de interfaz de usuario, para unir una experiencia de usuario discreta.

Para ver instrucciones y ejemplos de código, consulta [Anuncios nativos](native-ads.md).

![addreferences](images/native-ad.png)

## <a name="step-3-get-an-ad-unit-from-dev-center-and-configure-your-app-to-receive-live-ads"></a>Paso 3: Obtener una unidad de anuncio del Centro de desarrollo y configurar la aplicación para que pueda recibir anuncios dinámicos

Una vez que termines de probar la aplicación y ya estás listo para enviarla a la Tienda, crea una unidad de anuncio en la página [Monetizar con anuncios](../publish/monetize-with-ads.md) en el panel del Centro de desarrollo de Windows. A continuación, actualiza el código de tu aplicación para que use los valores del id. de la aplicación y el id. de la unidad de anuncio para esta unidad de anuncio. Si intentas usar los valores del id. de la aplicación de prueba y del id. de la unidad de anuncios de tu aplicación en la Tienda, tu aplicación no recibirá anuncios dinámicos. Para más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

<span id="ad-mediation"/>
### <a name="configure-ad-mediation"></a>Configurar la mediación de anuncios

De manera predeterminada, los anuncios de banner, los anuncios intersticiales y los anuncios nativos muestran anuncios de la red de Microsoft para los anuncios de pago. Para maximizar tus ingresos por anuncios, puedes habilitar la mediación de anuncios para tu unidad de anuncios, para mostrar anuncios de redes de anuncios de pago adicionales, como Taboola y Smaato. También puedes aumentar tus capacidades de promoción de la aplicación ofreciendo anuncios de las campañas de promoción de aplicaciones de Microsoft.

Para empezar a usar la mediación de anuncios en tu aplicación para UWP, [define la configuración de mediación de anuncios](../publish/monetize-with-ads.md#mediation) para tu unidad de anuncio. De manera predeterminada, configuramos automáticamente las opciones de mediación con algoritmos de aprendizaje automático que te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita. Sin embargo, también tienes la opción de elegir manualmente las redes que quieras usar. De cualquier manera, las opciones de mediación se configuran por completo en el servicios; no es necesario hacer ningún cambio en el código de tu aplicación.    

<span id="8.x-mediation"/>
### <a name="mediation-in-windows-81-and-windows-phone-8x-apps"></a>Mediación en aplicaciones para Windows8.1 y Windows Phone8.x

En las aplicaciones para Windows8.1 y Windows Phone8.x, la mediación de anuncios solo está disponible para anuncios de banner. Para usar la mediación de anuncios, debes usar la clase **AdMediatorControl** en el [SDK de Microsoft Advertising para Windows y Windows Phone8.x](http://aka.ms/store-8-sdk) en lugar de la clase **AdControl**. Después de agregar este control a tu aplicación, puedes configurar manualmente las opciones de mediación de anuncios en el panel.

Para obtener más información sobre el uso de la mediación de anuncios en una aplicación para Windows8.1 o Windows Phone8.x, consulta [este artículo](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

> [!NOTE]
> La mediación de anuncios para las aplicaciones para Windows8.1 y Windows Phone8.x ya no está en desarrollo activo. Para maximizar tu potencial de ingresos publicitarios, te recomendamos compilar las aplicaciones para UWP que usen la mediación de anuncios con anuncios de banner, anuncios intersticiales o anuncios nativos.

## <a name="step-4-submit-your-app-and-review-performance"></a>Paso 4: Enviar la aplicación y revisar el rendimiento

Tras finalizar el desarrollo de la aplicación con anuncios, puedes [enviar tu aplicación actualizada](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) al panel del Centro de desarrollo para que esté disponible en la Tienda. Las aplicaciones que muestran anuncios deben cumplir los requisitos adicionales que se especifican en la [sección 10.10 de las Directivas de la Tienda Windows](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_10) y en el [anexo E del Acuerdo para desarrolladores de aplicaciones](https://msdn.microsoft.com/library/windows/apps/hh694058.aspx).

Después de que la aplicación esté publicada y disponible en la tienda, puedes revisar los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel y seguir realizando cambios a la configuración de mediación para optimizar el rendimiento de tus anuncios. Tus ingresos publicitarios se incluyen en tu [resumen de pago](../publish/payout-summary.md).

<span id="additional-help" />
## <a name="additional-help"></a>Ayuda adicional

Para obtener más ayuda con el SDK de Microsoft Advertising, usa los siguientes recursos.

|  Tarea    | Recurso |               
|----------|-------|
| Informar de un error u obtener soporte técnico asistido para la publicidad     | Visita la [página de soporte técnico](https://go.microsoft.com/fwlink/p/?LinkId=331508) y elige **Publicidad desde la aplicación**.        |
| Obtener soporte técnico de la comunidad     | Visita el [foro](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |
| Descarga proyectos de ejemplo que muestran cómo agregar anuncios intersticiales y de banner a las aplicaciones.     | Consulta los [ejemplos de publicidad de GitHub](http://aka.ms/githubads).       |
| Obtener información sobre las oportunidades de monetización más recientes para aplicaciones de Windows     | Visita [Rentabiliza las aplicaciones](https://developer.microsoft.com/store/monetize).        |

## <a name="related-topics"></a>Temas relacionados

* [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp)
* [Monetizar tu aplicación con anuncios](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md)
