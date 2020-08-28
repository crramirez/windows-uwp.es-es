---
ms.assetid: ca92bed1-ad9e-4947-ad91-87d12de727c0
description: Vea las notas de la versión de las bibliotecas de Microsoft Advertising que admiten las aplicaciones XAML y JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8,1 y Windows Phone 8.
title: Notas de la versión de las bibliotecas de publicidad
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, anuncios, publicidad, notas de la versión
ms.localizationpriority: medium
ms.openlocfilehash: 10762d28191dfe59ae6f63f06cbeb0dd3e8a9f51
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "88969923"
---
# <a name="release-notes-for-the-advertising-libraries"></a>Notas de la versión de las bibliotecas de publicidad

>[!WARNING]
> A partir del 1 de junio de 2020, se cerrará la plataforma de monetización de Microsoft ad para aplicaciones UWP de Windows. [Más información](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

En esta sección se proporcionan las notas de la versión de la versión actual de las bibliotecas de Microsoft Advertising. Estas bibliotecas admiten aplicaciones de XAML y JavaScript/HTML para Windows 10, Windows 8.1, Windows Phone 8.1 y Windows Phone 8.

## <a name="installation"></a>Instalación


Las bibliotecas de Microsoft Advertising están disponibles como parte del [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK). Para obtener más información sobre cómo instalar el SDK, vea [instalar el SDK de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="uninstall-previous-versions"></a>Desinstalar versiones anteriores

Antes de instalar la versión más reciente del SDK de Microsoft Advertising, se recomienda encarecidamente que desinstale todas las instancias anteriores del SDK. Para obtener más información, vea [instalar el SDK de Microsoft Advertising](install-the-microsoft-advertising-libraries.md).

## <a name="target-architecture-specific-build-outputs"></a>Establecer el destino en las salidas de compilación específicas de la arquitectura

Al usar las bibliotecas de Microsoft Advertising, no puedes dirigirte a **Cualquier CPU** en tu proyecto. Si el proyecto se dirige a la plataforma **Cualquier CPU**, es posible que veas una advertencia en el proyecto después de agregar una referencia a las bibliotecas de Microsoft Advertising. Para quitar esta advertencia, actualiza el proyecto para usar una salida de compilación específica de la arquitectura (por ejemplo, **x86**). Para obtener más información, consulte [problemas conocidos](known-issues-for-the-advertising-libraries.md).

## <a name="c-support"></a>Compatibilidad con C++

Las bibliotecas de Microsoft Advertising (que incluyen las clases **AdControl** e **InterstitialAd**) admiten aplicaciones escritas en C++ y DirectX mediante la interoperabilidad de Windows Runtime (*interoperabilidad*). Para obtener más información y ejemplos acerca de la programación con XAML y C++, consulta [Sistema de tipos](https://docs.microsoft.com/cpp/cppcx/type-system-c-cx).

## <a name="no-toolbox-control"></a>Ningún control de cuadro de herramientas

En la versión actual de las bibliotecas de Microsoft Advertising en el [SDK de Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK), no hay ningún control de cuadro de herramientas para arrastrar un control o **InterstitialAd** a una superficie de diseño de la aplicación. **AdControl** Para obtener instrucciones sobre cómo agregar estos controles en el marcado y el código, consulta los [tutoriales de desarrollo](developer-walkthroughs.md).

## <a name="latitude-and-longitude-properties-no-longer-available"></a>Las propiedades de latitud y longitud ya no está disponibles

La clase **AdControl** ya no incluye las propiedades **Latitude** y **Longitude** en las aplicaciones para UWP. En su lugar, el código integrado en el control de anuncios detectará y enviará estos valores a los servidores de anuncios en nombre de la aplicación.


 

 
