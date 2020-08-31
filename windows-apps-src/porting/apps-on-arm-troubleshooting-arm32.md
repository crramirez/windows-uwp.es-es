---
title: Solución de problemas de aplicaciones UWP de ARM32
description: Problemas comunes con las aplicaciones de ARM32 cuando se ejecutan en ARM y cómo corregirlos.
ms.date: 01/03/2019
ms.topic: article
keywords: Windows 10 s, siempre conectado, aplicaciones ARM32 en ARM, Windows 10 en ARM, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 60c7a129844d69d18287ea7885a0acaf01f1f625
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171229"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solución de problemas de aplicaciones de ARM para UWP

Si la aplicación de UWP de ARM32 o ARM64 no funciona correctamente en ARM, aquí tiene algunas instrucciones que pueden ayudarle.

>[!NOTE]
> Para compilar la aplicación de UWP de manera nativa como destino de la plataforma ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior, o Visual Studio 2019. Para más información, vea [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Problemas comunes
Estos son algunos problemas comunes que se deben tener en cuenta a la hora de solucionar problemas de aplicaciones de ARM32 y ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Uso de las API de Windows 10 Mobile solo en procesadores basados en ARM
Las aplicaciones ARM pueden tener problemas al usar API solo para dispositivos móviles (por ejemplo, **HardwareButtons**). Para mitigar esto, puede detectar dinámicamente si la aplicación se ejecuta en Windows 10 Mobile antes de llamar a estas API. Siga las instrucciones de la entrada de blog, [detección dinámica de características con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Inclusión de dependencias no admitidas por las aplicaciones de UWP
Las aplicaciones Plataforma universal de Windows (UWP) que no se compilan correctamente con Visual Studio y el SDK de UWP pueden tener dependencias de componentes del sistema operativo que no están disponibles para las aplicaciones ARM que se ejecutan en un sistema ARM64. Entre los ejemplos de estas dependencias se incluyen:

- Se espera que las partes de la .NET Framework estén disponibles.
- Hacer referencia a componentes .NET de terceros que no son compatibles con UWP.

Puede resolver estos problemas: quitar las dependencias no disponibles y volver a compilar la aplicación con las versiones más recientes del SDK de UWP y Microsoft Visual Studio. o bien, como último recurso, quitando la aplicación ARM del Microsoft Store, de modo que la versión x86 de la aplicación (si está disponible) se descarga en los equipos de los usuarios.

Para obtener más información sobre las API de .NET disponibles para las aplicaciones para UWP, vea [.net para aplicaciones para UWP.](/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilar una aplicación con una versión anterior de Visual Studio y SDK
Si tiene problemas, asegúrese de usar las versiones más recientes de Microsoft Visual Studio y el Windows SDK para compilar la aplicación. Las aplicaciones compiladas con una versión anterior de Visual Studio y el SDK pueden tener problemas corregidos en versiones posteriores.

## <a name="debugging"></a>Depuración
Puede usar las herramientas existentes para desarrollar aplicaciones para la plataforma ARM. Estos son algunos recursos útiles.

- Visual Studio 15,5 Preview 1 y versiones posteriores admiten la ejecución de aplicaciones de ARM32 mediante el modo de autenticación universal. Esto inicia automáticamente las herramientas de depuración remota necesarias.
- Consulte [depuración en ARM64](/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre las herramientas y las estrategias para la depuración en ARM.