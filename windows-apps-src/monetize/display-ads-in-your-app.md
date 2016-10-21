---
author: mcleanbyron
ms.assetid: 63A9EDCF-A418-476C-8677-D8770B45D1D7
description: "Microsoft Store Services SDK proporciona varios métodos para rentabilizar la aplicación con anuncios."
title: "Mostrar anuncios en tu aplicación"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 35dfe2864958a15cf01133d6017b7dd03f382e4a

---

# Mostrar anuncios en tu aplicación


La Plataforma universal de Windows (UWP) y la Tienda Windows proporcionan varios métodos para rentabilizar la aplicación con anuncios.

## Mostrar anuncios intersticiales en vídeo y en banner con las bibliotecas de Microsoft Advertising.

Gana más dinero con las aplicaciones para UWP y las aplicaciones para Windows 8.1 y Windows Phone 8.x agregando anuncios intersticiales en vídeo y en banner. Los anuncios se muestran en las aplicaciones de Windows para PC, tabletas y teléfonos. Puedes supervisar el rendimiento de los anuncios en tiempo real mediante el [informe de rendimiento de la publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo de Windows.

Para incluir estos tipos de anuncios en tus aplicaciones, usa los controles **AdControl** e **InterstitialAd** en las bibliotecas de publicidad que se distribuyen en [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (para aplicaciones para UWP) y [Microsoft Advertising SDK para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicaciones de Windows 8.1 y Windows Phone 8.x).


En los siguientes temas se proporciona información sobre tareas comunes relacionadas con las bibliotecas de Windows Advertising.

|  Tarea    | Tema |               
|----------|-------|
| Instalar y comenzar a usar las bibliotecas de Microsoft Advertising.     | Consulta [Introducción a las bibliotecas de Microsoft Advertising](get-started-with-microsoft-advertising-libraries.md).        |
| Mostrar anuncios en banners en la aplicación XAML o C#.     | Consulta [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md).        |
| Mostrar anuncios en banner en la aplicación HTML o JavaScript.     | Consulta [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md).        |
| Mostrar un anuncios en banner en la aplicación de Windows Phone 8.x Silverlight.     | Consulta [AdControl en Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).        |
| Mostrar un anuncio intersticial en vídeo en la aplicación.     | Consulta [Anuncios intersticiales](interstitial-ads.md).       |
| Agregar anuncios a contenido de vídeo en una aplicación para Plataforma universal de Windows (UWP) que se ha escrito mediante JavaScript con HTML.   |  Consulta [Agregar anuncios a contenido de vídeo en HTML 5 y JavaScript](add-advertisements-to-video-content.md).  |
| Descargar proyectos de ejemplo que muestran cómo agregar anuncios intersticiales en vídeo y en banner a las aplicaciones.     |Consulta las [Muestras de publicidad en GitHub](http://aka.ms/githubads).       |
| Controla errores de [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) en la aplicación.     | Consulta [Control de errores](error-handling-with-advertising-libraries.md) y los tutoriales de [control de errores de AdControl](adcontrol-error-handling.md).       |
| Notificar un error en las bibliotecas de Microsoft Advertising.     | Visita la [página de soporte técnico](https://go.microsoft.com/fwlink/p/?LinkId=331508).        |
| Obtener soporte técnico de la comunidad.     | Visita el [foro](http://go.microsoft.com/fwlink/p/?LinkId=401266).       |

                            

## Usar la mediación de anuncios para anuncios en banner (Windows 8.1 y Windows Phone 8.x)

Para las aplicaciones de Windows 8.1 y Windows Phone 8.x, puedes usar la clase **AdMediatorControl** para optimizar los ingresos por anuncios al mostrar anuncios en banner provenientes de varias redes de anuncios. Después de agregar este control a la aplicación, establece la configuración de mediación de anuncios en el panel del Centro de desarrollo de Windows y nosotros nos encargamos de arbitrar solicitudes de anuncios de banner de las redes de anuncios que elijas. Para obtener más información, consulta [Usar la mediación de anuncios para maximizar los ingresos por anuncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864359.aspx).

>**Nota**&nbsp;&nbsp;La mediación de anuncios que usa la clase **AdMediatorControl** actualmente no se admite para aplicaciones para UWP en Windows10. La mediación del lado del servidor próximamente estará disponible para aplicaciones para UWP mediante las mismas API de anuncios en banner (**AdControl**) y de anuncios intersticiales en vídeo (**InterstitialAd**). Para obtener instrucciones sobre la migración de **AdMediatorControl** a **AdControl** en tu aplicación para UWP, consulta [Migrar de AdMediatorControl a AdControl para las aplicaciones para UWP](migrate-from-admediatorcontrol-to-adcontrol.md).

<span id="silverlight_support"/>
## Compatibilidad de publicidad para proyectos de Windows Phone 8.x Silverlight

Algunos escenarios de desarrollado ya no se admiten en proyectos de Windows Phone 8.x Silverlight. Para obtener más información, consulta la tabla siguiente.

|  Versión de plataforma  |  Proyectos existentes    |   Proyectos nuevos  |
|-----------------|----------------|--------------|
| Windows Phone 8.0 Silverlight     |  Si tienes un proyecto existente de Windows Phone 8.0 Silverlight que ya usa clase **AdControl** o **AdMediatorControl** de una versión anterior de Microsoft Universal Ad Client o Microsoft Advertising SDK y esta aplicación ya está publicada en la Tienda Windows, puedes modificar y volver a compilar el proyecto y puedes depurar o probar los cambios en un dispositivo. No se admite la depuración o la prueba del proyecto en el emulador.  |  No se admite.  |
| Windows Phone 8.1 Silverlight    |  Si tienes un proyecto existente de Windows Phone 8.1 Silverlight que usa una clase **AdControl** o **AdMediatorControl** de una versión anterior del SDK, puedes modificar y volver a compilar el proyecto. Sin embargo, para depurar o probar la aplicación, debes ejecutarla en el emulador y usar [valores del modo de prueba](test-mode-values.md) para el identificador de aplicación y el identificador de unidad de anuncio. No se admite la depuración o la prueba de la aplicación en un dispositivo.  |   Puedes agregar una clase **AdControl** o **AdMediatorControl** a un nuevo proyecto nuevo de Windows Phone 8.1 Silverlight. Sin embargo, para depurar o probar la aplicación, debes ejecutarla en el emulador y usar [valores del modo de prueba](test-mode-values.md) para el identificador de aplicación y el identificador de unidad de anuncio. No se admite la depuración o la prueba de la aplicación en un dispositivo. |

## Temas relacionados

* [Microsoft Store Services SDK](microsoft-store-services-sdk.md)
* [Monetiza tu aplicación con anuncios](http://go.microsoft.com/fwlink/p/?LinkId=699559)
* [Informe de rendimiento de la publicidad](../publish/advertising-performance-report.md)



<!--HONumber=Sep16_HO2-->


