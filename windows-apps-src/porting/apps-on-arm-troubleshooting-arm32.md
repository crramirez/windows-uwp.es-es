---
title: Solución de problemas de aplicaciones para UWP de ARM32
description: Problemas comunes con aplicaciones ARM32 cuando se ejecutan en ARM y cómo solucionarlos.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM32 apps on ARM, aplicaciones ARM32 en ARM, windows 10 on ARM, windows 10 en ARM, troubleshooting, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 6213c8c69695d160d4e6fa362aa7aa322a0326fd
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2020
ms.locfileid: "75683948"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solución de problemas de aplicaciones de ARM para UWP

Si la aplicación de UWP de ARM32 o ARM64 no funciona correctamente en ARM, aquí tiene algunas instrucciones que pueden ayudarle.

>[!NOTE]
> Para compilar la aplicación de UWP de manera nativa como destino de la plataforma ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior, o Visual Studio 2019. Para más información, vea [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


## <a name="common-issues"></a>Problemas comunes
Estos son algunos problemas comunes que se deben tener en cuenta a la hora de solucionar problemas de aplicaciones de ARM32 y ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilizar API solo de Windows 10 Mobile en procesadores basados en ARM
Las aplicaciones ARM pueden tener problemas al usar API solo para dispositivos móviles (por ejemplo, **HardwareButtons**). Para mitigarlo, puedes detectar dinámicamente si la aplicación se ejecuta en Windows 10 Mobile antes de llamar a estas API. Sigue las instrucciones de la entrada de blog, [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluir dependencias no compatibles con aplicaciones para UWP
Las aplicaciones Plataforma universal de Windows (UWP) que no se compilan correctamente con Visual Studio y el SDK de UWP pueden tener dependencias de componentes del sistema operativo que no están disponibles para las aplicaciones ARM que se ejecutan en un sistema ARM64. Entre los ejemplos de estas dependencias se incluyen:

- Esperar que estén disponibles partes de.NET Framework.
- Hacer referencia a los componentes .NET de terceros que no son compatibles con UWP.

Puede resolver estos problemas: quitar las dependencias no disponibles y volver a compilar la aplicación con las versiones más recientes del SDK de UWP y Microsoft Visual Studio. o bien, como último recurso, quitando la aplicación ARM del Microsoft Store, de modo que la versión x86 de la aplicación (si está disponible) se descarga en los equipos de los usuarios.

Para obtener más información sobre las API de .NET disponibles para aplicaciones para UWP, consulta [.NET para aplicaciones para UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilar una aplicación con una versión anterior de Visual Studio y SDK
Si estás teniendo problemas, asegúrate de usar las versiones más recientes de Microsoft Visual Studio y Windows SDK para compilar la aplicación. Las aplicaciones compiladas con una versión anterior de Visual Studio y SDK pueden tener problemas corregidos en versiones posteriores.

## <a name="debugging"></a>Depuración
Puede usar las herramientas existentes para desarrollar aplicaciones para la plataforma ARM. Aquí tienes algunos recursos útiles.

- Visual Studio 15,5 Preview 1 y posterior admite la ejecución de aplicaciones ARM32 utilizando el modo de autenticación universal. Esto arranca automáticamente las herramientas de depuración remota necesarias.
- Consulta [Depuración en ARM64](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre herramientas y estrategias para depurar en ARM.
