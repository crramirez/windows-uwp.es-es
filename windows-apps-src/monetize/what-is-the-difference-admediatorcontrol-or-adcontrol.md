---
author: mcleanbyron
ms.assetid: 9165f709-71d7-42cf-9b30-3190fe029fb4
description: Obtén información sobre las diferencias entre la clase AdControl en las bibliotecas de Microsoft Advertising y la clase AdMediatorControl en las bibliotecas de mediación de anuncios.
title: Cuál es la diferencia - AdMediator o AdControl
---

# Cuál es la diferencia: AdMediator o AdControl


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Usa las bibliotecas de Microsoft Advertising para XAML y JavaScript en tu aplicación cuando quieras mostrar anuncios de banner o intersticiales en vídeo de Microsoft. Estas bibliotecas son diferentes de las bibliotecas de mediación de anuncios, que se usan para mostrar anuncios de varias redes de anuncios. Usa esta documentación de las bibliotecas de Microsoft Advertising (XAML y JavaScript) si:

* Usas [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) en una aplicación de JavaScript o XAML en lugar de **AdMediatorControl**.
* Buscas información de referencia para la API de **AdControl** subyacente que se usa en la mediación de anuncios.

Las bibliotecas de Microsoft Advertising y las bibliotecas de mediación de anuncios se incluyen en el SDK de Microsoft Store Engagement and Monetization. Para obtener más información sobre la instalación de este SDK y las distintas bibliotecas de Microsoft Advertising que se incluyen en este SDK, consulta [Instalar las bibliotecas de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

>**Nota** Para mostrar anuncios intersticiales, usa el control **InterstitialAd**. **AdControl** y **AdMediatorControl** no pueden mostrar anuncios intersticiales. Para obtener más información, consulta [Anuncios intersticiales](interstitial-ads.md).

 

## Mediación de anuncios


La manera recomendada de mostrar anuncios de banner de Microsoft (no anuncios intersticiales) es usar **AdMediatorControl** en la aplicación. **AdMediatorControl** muestra anuncios de banner de varias redes de publicidad.

Si usas **AdMediatorControl** en un proyecto, debes especificar las redes de anuncios que se deben usar con la característica **Servicios conectados** de Visual Studio. Visual Studio intentará capturar los ensamblados necesarios mediante programación de algunas redes de anuncios. Si existen ensamblados que no se pueden recuperar automáticamente, debes instalarlos para cada red de anuncios. Para más información sobre la mediación de anuncios, consulta [Usar la mediación de anuncios para maximizar los ingresos](use-ad-mediation-to-maximize-revenue.md).

**AdMediatorControl** abstrae el uso de identificadores de unidad de anuncios e identificadores de aplicación. Esos identificadores los administra **AdMediatorControl**, junto con la configuración de mediación de anuncios en el panel del Centro de desarrollo de Windows. Para más información, consulta [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md).

**AdMediatorControl** admite las API para cada uno de los servicios de publicidad para los que realiza la medición con sus propios atributos y su sintaxis. Para más información, consulta [Agregar y usar el control de Ad Mediator](add-and-use-the-ad-mediator-control.md).

## AdControl


Si solo quieres mostrar anuncios de banner de Microsoft (no de otras redes de anuncios), puedes usar **AdControl** por sí solo en aplicaciones de XAML y JavaScript. Durante la prueba de una aplicación que use **AdControl**, usa los [valores del modo de prueba](test-mode-values.md) para el identificador de la aplicación y el identificador de la unidad de anuncios. Cuando termines de probar la aplicación y estés listo para enviarla al Centro de desarrollo de Windows, usa el panel del Centro de desarrollo de Windows para obtener el identificador de la unidad de anuncios dinámicos y el identificador de la aplicación, y actualiza el código para usar estos valores. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

 

 


<!--HONumber=May16_HO2-->


