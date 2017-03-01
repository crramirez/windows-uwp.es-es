---
author: mcleanbyron
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: "Consulta las notas de la versión de las bibliotecas de Microsoft Advertising en el Microsoft Store Services SDK."
title: "Notas de la versión de las bibliotecas de Microsoft Advertising"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, ads, anuncios, advertising, publicidad, release notes, notas de la versión"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 666071bccd562f2f95e8ae69f50797fdc464548b
ms.lasthandoff: 02/07/2017

---

# <a name="release-notes-for-the-microsoft-advertising-libraries"></a>Notas de la versión de las bibliotecas de Microsoft Advertising




En esta sección se proporcionan notas de la versión destinadas a la versión actual de las bibliotecas de Microsoft Advertising incluidas en el Microsoft Store Services SDK (para aplicaciones para UWP) y el SDK de Microsoft Advertising para Windows y Windows Phone 8.x (para aplicaciones de Windows 8.1 y Windows Phone 8.x). Estas bibliotecas admiten aplicaciones de XAML y JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 y Windows Phone 8.

## <a name="installation"></a>Instalación


Las bibliotecas de Microsoft Advertising están disponibles como parte de [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) (para aplicaciones para UWP) y [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicaciones de Windows 8.1 y Windows Phone 8.x). Para obtener más información sobre cómo instalar los SDK y las bibliotecas incluidas en ellos, consulta [Instalar las bibliotecas de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

Para obtener los ensamblados de Microsoft Advertising para proyectos de Windows Phone 8.x Silverlight, instala el [SDK de Microsoft Advertising para Windows y Windows Phone 8.x](http://aka.ms/store-8-sdk), abre el proyecto en Visual Studio y, después, ve a **Proyecto** > **Agregar servicio conectado** > **Ad Mediator** para descargar automáticamente los ensamblados. Después de hacer esto, puedes quitar las referencias de Ad Mediator del proyecto si no quieres usar la mediación de anuncios. Para obtener más información, consulta [AdControl en Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).


## <a name="uninstall-previous-versions"></a>Desinstalar versiones anteriores

Antes de instalar Microsoft Store Services SDK (para aplicaciones para UWP) o el SDK de Microsoft Advertising para Windows y Windows Phone 8.x (para aplicaciones de Windows 8.1 y Windows Phone 8.x), se recomienda encarecidamente desinstalar todas las instancias anteriores del SDK de Microsoft Universal Ad Client o el SDK de Microsoft Advertising.

## <a name="target-architecture-specific-build-outputs"></a>Establecer el destino en las salidas de compilación específicas de la arquitectura

Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulta [Problemas conocidos](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Compatibilidad con C++

Las bibliotecas de Microsoft Advertising (que incluyen las clases **AdControl** e **InterstitialAd**) admiten aplicaciones escritas en C++ y DirectX mediante la interoperabilidad de Windows Runtime (*interoperabilidad*). Para obtener más información y ejemplos acerca de la programación con XAML y C++, consulta [Sistema de tipos](https://msdn.microsoft.com/library/windows/apps/xaml/hh755822.aspx).

## <a name="no-toolbox-control"></a>Ningún control de cuadro de herramientas

En la versión actual de las bibliotecas de Microsoft Advertising incluidas en Microsoft Store Services SDK o en el SDK de Microsoft Advertising para Windows y Windows Phone 8.x, no hay ningún control de cuadro de herramientas para arrastrar un objeto **AdControl** o **InterstitialAd** a una superficie de diseño de la aplicación. Para obtener instrucciones sobre cómo agregar estos controles en el marcado y el código, consulta los [tutoriales de desarrollo](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Las propiedades de latitud y longitud ya no está disponibles

La clase **AdControl** ya no incluye las propiedades **Latitude** y **Longitude** en las aplicaciones para UWP. En su lugar, el código integrado en el control de anuncios detectará y enviará estos valores a los servidores de anuncios en nombre de la aplicación.

## <a name="important-notice"></a>Aviso importante

Asegúrate de leer el contrato de licencia del usuario final (CLUF) en su totalidad. Consulta el tema [Aviso importante: CLUF](important-notice-eula.md).

 

 

