---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: El SDK de Microsoft Advertising te ofrece varios métodos para monetizar la aplicación con anuncios.
title: Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp, anuncios, publicidad, banner, control de anuncios, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: 48aacf03d6e424b7b53052e2c292d4a8589a8d9f
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684573"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising

Aumenta las oportunidades de ingresos poniendo anuncios en tu aplicación para la Plataforma universal de Windows (UWP) para Windows 10 mediante el SDK de Microsoft Advertising. Nuestra plataforma de monetización de anuncios ofrece una variedad de formatos de ad que se pueden integrar perfectamente en sus aplicaciones y admiten la mediación con muchas redes de anuncios populares. Nuestra plataforma es compatible con los estándares OpenRTB, VAST 2.x, MRAID 2 y VPAID 3, y con MOAT e IAS. 

<br/>

<table style="border: none !important;">
<colgroup>
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
<col width="10%" />
<col width="23%" />
</colgroup>
<tbody>
<tr>
<td align="left"><img src="images/install-sdk.png" alt="Install SDK icon" /></td>
<td align="left"><b>Introducción</b><br/><br/>
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Instale el SDK de Microsoft Advertising</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guías para desarrolladores</b><br/><br/>
    <a href="banner-ads.md">Anuncios de banner</a>
    <br/>
    <a href="interstitial-ads.md">Anuncios intersticiales</a>
    <br/>
     de 
    <a href="native-ads.md">anuncios nativos</a></td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Otros recursos</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Configurar unidades de anuncios en la aplicación</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Prácticas recomendadas</a>
    <br/>
    <a href="https://docs.microsoft.com/uwp/api/overview/advertising">Referencia de API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Paso 1: Instalar el SDK de Microsoft Advertising

Para empezar, instala el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) en el equipo de desarrollo que usar para crear la aplicación. Para obtener instrucciones de instalación, consulta [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Paso 2: Implementar anuncios en tu aplicación

El SDK de Microsoft Advertising ofrece varios tipos de anuncios distintos de controles de anuncios que puedes usar en la aplicación. Elige qué tipos de anuncios son mejores para tu escenario y después agrega código a la aplicación para mostrar esos anuncios. Durante este paso, usarás una unidad de anuncios de prueba para que puedas ver cómo la aplicación representa los anuncios durante las pruebas.

### <a name="banner-ads"></a>Anuncios de banner

Estos anuncios de banner son los anuncios de visualización estática que usan una parte rectangular de una página en la aplicación para mostrar contenido promocional. Estos anuncios se pueden actualizar automáticamente a intervalos regulares. Este es un buen punto de partida si estás empezando a usar la publicidad en tu aplicación.

Para obtener instrucciones y ejemplos de código, consulta [este artículo](adcontrol-in-xaml-and--net.md).

![addreferences](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Anuncios de banner intersticiales y vídeos intersticiales

Estos son anuncios de pantalla completa que requieren normalmente que el usuario vea un vídeo o haga clic en ellos para continuar en la aplicación o el juego. Admitimos dos tipos de anuncios intersticiales: vídeo y banner.

Para obtener instrucciones y ejemplos de código, consulta [este artículo](interstitial-ads.md).

![addreferences](images/interstitial-ad.png)

### <a name="native-ads"></a>Anuncios nativos

Se trata de anuncios basados en componentes. Cada parte del anuncio creativo (por ejemplo, el título, la imagen, la descripción y el texto de llamada a la acción) se entrega a la aplicación como un elemento individual que puedes integrar en la aplicación con tus propias fuentes, colores y otros componentes de interfaz de usuario.

Para obtener instrucciones y ejemplos de código, consulta [este artículo](native-ads.md).

![addreferences](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Paso 3: Crear una unidad de anuncios y configurar la mediación

Después de finalizar la prueba de la aplicación y de que esté listo para enviarla a la tienda, cree una unidad de anuncio en la página [anuncios en la aplicación](../publish/in-app-ads.md) del centro de Partners. Luego actualiza el código de tu aplicación para usar esta unidad de anuncios de modo que tu aplicación recibirá anuncios dinámicos. Para más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

De manera predeterminada, la aplicación mostrará anuncios de la red de Microsoft, por lo relativo a anuncios de pago. Para maximizar tus ingresos por anuncios, puedes habilitar la [mediación de anuncios](ad-mediation-service.md) para tu unidad de anuncios, para mostrar anuncios de redes de anuncios de pago adicionales, como Taboola y Smaato. También puedes aumentar tus capacidades de promoción de la aplicación ofreciendo anuncios de las campañas de promoción de aplicaciones de Microsoft.

Para empezar a usar la mediación de anuncios en tu aplicación para UWP, [define la configuración de mediación de anuncios](../publish/in-app-ads.md#mediation-settings) para tu unidad de anuncio. De manera predeterminada, configuramos automáticamente las opciones de mediación con algoritmos de aprendizaje automático que te ayudarán a maximizar los ingresos por anuncios en los mercados que tu aplicación admita. Sin embargo, también tienes la opción de elegir manualmente las redes que quieras usar. De cualquier manera, las opciones de mediación se configuran por completo nuestros servidores; no es necesario hacer ningún cambio en el código de tu aplicación.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Paso 4: Enviar la aplicación y revisar el rendimiento

Cuando termine de desarrollar la aplicación con ADS, puede [enviar la aplicación actualizada](https://docs.microsoft.com/windows/uwp/publish/app-submissions) en el centro de partners para que esté disponible en la tienda. Las aplicaciones que muestran anuncios deben cumplir los requisitos adicionales que se especifican en la [sección 10.10 de las Directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) y en el [anexo E del Acuerdo para desarrolladores de aplicaciones](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement).

Una vez publicada la aplicación y disponible en la tienda, puede revisar los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners y seguir realizando cambios en la configuración de mediación para optimizar el rendimiento de los anuncios. Tus ingresos publicitarios se incluyen en tu [resumen de pago](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Ayuda adicional

Para obtener más ayuda con el SDK de Microsoft Advertising, usa los siguientes recursos.

|  Tarea    | Recurso |               
|----------|-------|
| Informar de un error u obtener soporte técnico asistido para la publicidad     | Visita la [página de soporte técnico](https://developer.microsoft.com/windows/support) y elige **Anuncios en aplicaciones**.        |
| Obtener soporte técnico de la comunidad     | Visita el [foro](https://go.microsoft.com/fwlink/?LinkID=401264).       |
| Descarga proyectos de ejemplo que muestran cómo agregar anuncios intersticiales y de banner a las aplicaciones.     | Consulta las [Muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Obtener información sobre las oportunidades de monetización más recientes para aplicaciones de Windows     | Visita [Monetizar las aplicaciones](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Aplicaciones de Windows 8.1 y Windows Phone 8.x

Para aplicaciones de Windows 8.1 y Windows Phone 8.x, ofrecemos el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Para obtener más información sobre el uso de este SDK para mostrar anuncios en aplicaciones de Windows 8.1 o Windows Phone 8.x, consulta [este artículo](https://docs.microsoft.com/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Temas relacionados

* [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Informe de rendimiento de publicidad](../publish/advertising-performance-report.md)
* [Programa de publicadores de Windows Premium ADS](windows-premium-ads-publishers-program.md)
