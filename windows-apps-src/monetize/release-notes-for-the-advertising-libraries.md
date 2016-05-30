---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Consulta las notas de la versión de las bibliotecas de Microsoft Advertising en el SDK de Microsoft Store Engagement and Monetization.
title: Notas de la versión de las bibliotecas de Microsoft Advertising
---

# Notas de la versión de las bibliotecas de Microsoft Advertising


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta sección incluye las notas de la versión para la versión actual de las bibliotecas de Microsoft Advertising en el SDK de Microsoft Store Engagement and Monetization. Estas bibliotecas admiten aplicaciones de XAML y JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 y Windows Phone 8.

## Instalación


Las bibliotecas de Microsoft Advertising están disponibles como parte del [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Para todos los tipos de proyecto distintos de Windows Phone 8.x Silverlight, los ensamblados de Microsoft Advertising que solían distribuirse en versiones independientes anteriores del SDK de cliente de anuncios universal de Microsoft y el SDK de Microsoft Advertising ahora se instalan con el SDK de Microsoft Store Engagement and Monetization. Para obtener más información acerca de cómo instalar el SDK y las bibliotecas que este incluye, consulta [Instalar las bibliotecas de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

Para obtener los ensamblados de Microsoft Advertising para proyectos de Windows Phone 8.x Silverlight, instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), abre el proyecto en Visual Studio y, después, ve a **Proyecto** > **Agregar servicio conectado** > **Ad Mediator** para descargar automáticamente los ensamblados. Después de hacer esto, puedes quitar las referencias de Ad Mediator del proyecto si no quieres usar la mediación de anuncios. Para obtener más información, consulta [AdControl in Windows Phone Silverlight (AdControl in Windows Phone Silverlight)](adcontrol-in-windows-phone-silverlight.md).

## Explicación de la diferencia entre las bibliotecas de Microsoft Advertising y la mediación de anuncios

Aunque tanto las bibliotecas de Microsoft Advertising como las de mediación de anuncios se incluyen en el SDK de Microsoft Store Engagement and Monetization, están destinadas a propósitos diferentes. Usa las clases [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) e [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) en las bibliotecas de Microsoft Advertising si quieres mostrar banners o anuncios intersticiales en vídeo de Microsoft en una aplicación XAML o JavaScript. Usa la clase **AdMediatorControl** en las bibliotecas de mediación de anuncios si quieres mostrar anuncios de banners de varias redes de anuncios en una aplicación XAML (la mediación de anuncios no se admite para aplicaciones JavaScript/HTML). Para obtener más información, consulta [Cuál es la diferencia: AdMediator o AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

## Desinstalar versiones anteriores

Antes de instalar el SDK de Microsoft Store Engagement and Monetization, se recomienda encarecidamente desinstalar todas las instancias anteriores del SDK de cliente de anuncios universal de Microsoft o el SDK de Microsoft Advertising.

## Establecer el destino en las salidas de compilación específicas de la arquitectura

Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulta [Problemas conocidos](known-issues-for-the-advertising-libraries.md).

## Compatibilidad con C++

Las bibliotecas de Microsoft Advertising (que incluyen las clases **AdControl** e **InterstitialAd**) admiten aplicaciones escritas en C++ y DirectX mediante la interoperabilidad de Windows Runtime (*interoperabilidad*). Para obtener más información y ejemplos acerca de la programación con XAML y C++, consulta [Sistema de tipos](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## Ningún control de cuadro de herramientas

En la versión actual de las bibliotecas de Microsoft Advertising en el SDK de Microsoft Store Engagement and Monetization, no hay ningún control de cuadro de herramientas para arrastrar una clase **AdControl** o **InterstitialAd** a una superficie de diseño de la aplicación. Para obtener instrucciones sobre cómo agregar estos controles en el marcado y el código, consulta los [tutoriales de desarrollo](developer-walkthroughs.md).

## Las propiedades de latitud y longitud ya no está disponibles

La clase **AdControl** ya no incluye las propiedades **Latitude** y **Longitude** en las aplicaciones para UWP. En su lugar, el código integrado en el control de anuncios detectará y enviará estos valores a los servidores de anuncios en nombre de la aplicación.

## Aviso importante

Asegúrate de leer el contrato de licencia del usuario final (CLUF) en su totalidad. Consulta el tema [Aviso importante: CLUF](important-notice-eula.md).

 

 


<!--HONumber=May16_HO2-->


