---
author: Xansky
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Revisa las notas de la versión de las bibliotecas de Microsoft Advertising.
title: Notas de la versión de las bibliotecas de publicidad
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, Windows 10, uwp, UWP, ads, anuncios, advertising, publicidad, release notes, notas de la versión
ms.localizationpriority: medium
ms.openlocfilehash: dbe932eb9391a4de0304b4be42944b2bced3287a
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6857247"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notas de la versión de las bibliotecas de publicidad




En esta sección se proporcionan las notas de la versión para la versión actual de las bibliotecas de Microsoft Advertising. Estas bibliotecas admiten aplicaciones XAML y JavaScript/HTML para Windows 10, Windows8.1, Windows Phone 8.1 y WindowsPhone8.

## <a name="installation"></a>Instalación


Las bibliotecas de publicidad de Microsoft están disponibles como parte del [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp). Para más información sobre cómo instalar el SDK, consulta [Instalar el SDK de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Desinstalar versiones anteriores

Antes de instalar el SDK de Microsoft Advertising más reciente, se recomienda encarecidamente desinstalar todas las instancias anteriores del SDK. Para más información, consulta [Instalar el SDK de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Establecer el destino en las salidas de compilación específicas de la arquitectura

Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulta [Problemas conocidos](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Compatibilidad con C++

Las bibliotecas de Microsoft Advertising (que incluyen las clases **AdControl** e **InterstitialAd**) admiten aplicaciones escritas en C++ y DirectX mediante la interoperabilidad de Windows Runtime (*interoperabilidad*). Para obtener más información y ejemplos acerca de la programación con XAML y C++, consulta [Sistema de tipos](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Ningún control de cuadro de herramientas

En la versión actual de las bibliotecas de publicidad de Microsoft en el [SDK de Microsoft Advertising](http://aka.ms/ads-sdk-uwp), no hay ningún control de cuadro de herramientas para arrastrar una clase **AdControl** o **InterstitialAd** a una superficie de diseño de la aplicación. Para obtener instrucciones sobre cómo agregar estos controles en el marcado y el código, consulta los [tutoriales de desarrollo](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Las propiedades de latitud y longitud ya no está disponibles

La clase **AdControl** ya no incluye las propiedades **Latitude** y **Longitude** en las aplicaciones para UWP. En su lugar, el código integrado en el control de anuncios detectará y enviará estos valores a los servidores de anuncios en nombre de la aplicación.


 

 
