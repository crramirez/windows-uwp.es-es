---
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: El SDK de Microsoft Advertising ofrece varias maneras de rentabilizar la aplicación con los anuncios.
title: Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, Banner, control de anuncios, intersticial
ms.localizationpriority: medium
ms.openlocfilehash: c12d79b97010826b05bf42a9de46780dd2f93756
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933126"
---
# <a name="display-ads-in-your-app-with-the-microsoft-advertising-sdk"></a>Mostrar anuncios en tu aplicación con el SDK de Microsoft Advertising

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Aumente las oportunidades de ingresos colocando anuncios en la aplicación Plataforma universal de Windows (UWP) para Windows 10 mediante el SDK de Microsoft Advertising. Nuestra plataforma de monetización de anuncios ofrece una variedad de formatos de ad que se pueden integrar perfectamente en sus aplicaciones y admiten la mediación con muchas redes de anuncios populares. Nuestra plataforma es compatible con los estándares OpenRTB, VAST 2.x, MRAID 2 y VPAID 3, y con MOAT e IAS. 

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
    <a href="https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK">Instalación del SDK de Microsoft Advertising</a>
</td>
<td align="left"><img src="images/write-code.png" alt="Develop icon" /></td>
<td align="left"><b>Guías para desarrolladores</b><br/><br/>
    <a href="banner-ads.md">Anuncios de banner</a>
    <br/>
    <a href="interstitial-ads.md">Anuncios intersticiales</a>
    <br/>
    <a href="native-ads.md">Anuncios nativos</a>
    </td>
<td align="left"><img src="images/api-reference.png" alt="API ref icon" /></td>
<td align="left"><b>Otros recursos</b><br/><br/>
    <a href="set-up-ad-units-in-your-app.md">Configurar unidades de anuncios en la aplicación</a>
    <br/>
    <a href="best-practices-for-ads-in-apps.md">Procedimientos recomendados</a>
    <br/>
    <a href="/uwp/api/overview/advertising">Referencia de API</a>
    </td>
</tr>
</tbody>
</table>

## <a name="step-1-install-the-microsoft-advertising-sdk"></a>Paso 1: instalación del SDK de Microsoft Advertising

Para empezar, instale el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) en el equipo de desarrollo que usa para compilar la aplicación. Para obtener instrucciones de instalación, consulte [este artículo](install-the-microsoft-advertising-libraries.md).

## <a name="step-2-implement-ads-in-your-app"></a>Paso 2: implementar anuncios en la aplicación

El SDK de Microsoft Advertising proporciona varios tipos diferentes de controles de ad que puede usar en la aplicación. Elija los tipos de anuncios que mejor se adapten a su escenario y agregue código a la aplicación para mostrar esos anuncios. Durante este paso, usará una unidad de anuncio de prueba para que pueda ver cómo la aplicación representa los anuncios durante las pruebas.

### <a name="banner-ads"></a>Anuncios de banner

Se trata de anuncios de visualización estáticos que usan una parte rectangular de una página de la aplicación para mostrar el contenido promocional. Estos anuncios pueden actualizarse automáticamente a intervalos regulares. Este es un buen punto de partida si no está familiarizado con la publicidad en la aplicación.

Para obtener instrucciones y ejemplos de código, consulte [este artículo](adcontrol-in-xaml-and--net.md).

![Imagen que muestra un anuncio de banner en una tableta.](images/banner-ad.png)

### <a name="interstitial-video-and-interstitial-banner-ads"></a>Anuncios de banners intersticiales y de vídeo intersticial

Se trata de anuncios de pantalla completa que normalmente requieren que el usuario vea un vídeo o haga clic en ellos para continuar en la aplicación o el juego. Se admiten dos tipos de anuncios intersticiales: vídeo y Banner.

Para obtener instrucciones y ejemplos de código, consulte [este artículo](interstitial-ads.md).

![Imagen que muestra un anuncio intersticial en un juego que se está reproduciendo en una tableta.](images/interstitial-ad.png)

### <a name="native-ads"></a>Anuncios nativos

Se trata de anuncios basados en componentes. Cada parte de la creatividad del anuncio (como el título, la imagen, la descripción y el texto de la llamada a la acción) se entrega a la aplicación como un elemento individual que se puede integrar en la aplicación con sus propias fuentes, colores y otros componentes de la interfaz de usuario.

Para obtener instrucciones y ejemplos de código, consulte [este artículo](native-ads.md).

![Imagen que muestra un anuncio nativo que se puede mostrar en varios dispositivos.](images/native-ad.png)

<span id="ad-mediation"/>

## <a name="step-3-create-an-ad-unit-and-configure-mediation"></a>Paso 3: crear una unidad de anuncio y configurar la mediación

Después de finalizar la prueba de la aplicación y de que esté listo para enviarla a la tienda, cree una unidad de anuncio en la página [anuncios en la aplicación](../publish/in-app-ads.md) del centro de Partners. A continuación, actualice el código de la aplicación para usar esta unidad de anuncio para que la aplicación reciba anuncios en directo. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md#live-ad-units).

De forma predeterminada, la aplicación mostrará anuncios de la red de Microsoft para anuncios de pago. Para maximizar los ingresos de anuncios, puede habilitar la [mediación de anuncios](ad-mediation-service.md) para que la unidad de anuncio muestre anuncios de redes de anuncios de pago adicionales, como Taboola y Smaato. También puede aumentar las funcionalidades de promoción de aplicaciones atendiendo a los anuncios de las campañas de promoción de aplicaciones de Microsoft.

Para empezar a usar la mediación de anuncios en la aplicación para UWP, [Configure los valores de mediación de ad](../publish/in-app-ads.md#mediation-settings) para la unidad de anuncio. De forma predeterminada, se configuran automáticamente los valores de mediación con algoritmos de aprendizaje automático para ayudarle a maximizar los ingresos de publicidad en los mercados que admite la aplicación. Sin embargo, también tiene la opción de elegir manualmente las redes que quiere usar. En cualquier caso, la configuración de la mediación se configura completamente en nuestros servidores; no es necesario realizar ningún cambio de código en la aplicación.    

## <a name="step-4-submit-your-app-and-review-performance"></a>Paso 4: enviar la aplicación y revisar el rendimiento

Cuando termine de desarrollar la aplicación con ADS, puede [enviar la aplicación actualizada](../publish/app-submissions.md) en el centro de partners para que esté disponible en la tienda. Las aplicaciones que muestran anuncios deben cumplir los requisitos adicionales que se especifican en [la sección 10,10 de las directivas de Microsoft Store](/legal/windows/agreements/store-policies#1010-advertising-conduct-and-content) y el [anexo e del acuerdo de desarrollador de aplicaciones](/legal/windows/agreements/app-developer-agreement).

Una vez publicada la aplicación y disponible en la tienda, puede revisar los [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el centro de Partners y seguir realizando cambios en la configuración de mediación para optimizar el rendimiento de los anuncios. Los ingresos de publicidad se incluyen en el [Resumen de pagos](../publish/payout-summary.md).

<span id="additional-help" />

## <a name="additional-help"></a>Ayuda adicional

Para obtener más ayuda sobre el uso del SDK de Microsoft Advertising, utilice los siguientes recursos.

|  Tarea    | Resource |               
|----------|-------|
| Informar de un error u obtener soporte técnico asistido para la publicidad     | Visite la [Página de soporte técnico](https://developer.microsoft.com/windows/support) y elija **ADS-in-apps**.        |
| Obtener soporte técnico de la comunidad     | Visita el [foro](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).       |
| Descargar proyectos de ejemplo que muestran cómo agregar anuncios intersticiales en vídeo y en banner a las aplicaciones.     | Consulta las [Muestras de publicidad en GitHub](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Advertising).       |
| Obtener información sobre las oportunidades de monetización más recientes para aplicaciones de Windows     | Visita [Rentabiliza las aplicaciones](https://developer.microsoft.com/store/monetize).        |

## <a name="windows-81-and-windows-phone-8x-apps"></a>Aplicaciones Windows 8.1 y Windows Phone 8. x

En el caso de las aplicaciones Windows 8.1 y Windows Phone 8. x, se proporciona el [SDK de Microsoft Advertising para Windows y Windows Phone 8. x](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDKforWindowsandWindowsPhone8x). Para obtener más información sobre cómo usar este SDK para mostrar anuncios en Windows 8.1 y Windows Phone aplicaciones 8. x, consulte [este artículo](/previous-versions/windows/apps/dn792120(v=win.10)).

## <a name="related-topics"></a>Temas relacionados

* [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK)
* [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md)
* [Programa de editores de anuncios de Windows Premium](windows-premium-ads-publishers-program.md)