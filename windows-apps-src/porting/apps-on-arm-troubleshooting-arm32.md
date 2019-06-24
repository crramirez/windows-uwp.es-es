---
title: Solución de problemas de aplicaciones para UWP de ARM32
description: Problemas comunes con aplicaciones ARM32 cuando se ejecutan en ARM y cómo solucionarlos.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM32 apps on ARM, aplicaciones ARM32 en ARM, windows 10 on ARM, windows 10 en ARM, troubleshooting, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: b93a4a3bdf4a6abd12a0df8bfc1871cb38647f63
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319754"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solución de problemas de aplicaciones para UWP de ARM

Si su aplicación ARM32 o ARM64 UWP no funciona correctamente en ARM, aquí tiene algunas instrucciones que pueden ayudar.

>[!NOTE]
> Para compilar la aplicación para UWP como destino la plataforma ARM64 de forma nativa, debe tener Visual Studio 2017 versión 15,9 o posterior. Para obtener más información, consulte [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

## <a name="common-issues"></a>Problemas comunes
Estos son algunos problemas habituales a tener en cuenta al solucionar problemas de aplicaciones de ARM32 y ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilizar API solo de Windows 10 Mobile en procesadores basados en ARM
Las aplicaciones ARM pueden tener problemas al usar API solo mobile (por ejemplo, **HardwareButtons**). Para mitigarlo, puedes detectar dinámicamente si la aplicación se ejecuta en Windows 10 Mobile antes de llamar a estas API. Sigue las instrucciones de la entrada de blog, [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluir dependencias no compatibles con aplicaciones para UWP
Aplicaciones universales de Windows Platform (UWP) que no se generan correctamente con Visual Studio y el SDK de UWP pueden tener dependencias en los componentes del sistema operativo que no están disponibles para aplicaciones ARM que se ejecutan en un sistema ARM64. Entre los ejemplos de estas dependencias se incluyen:

- Esperar que estén disponibles partes de.NET Framework.
- Hacer referencia a los componentes .NET de terceros que no son compatibles con UWP.

Pueden resolver estos problemas: quitar las dependencias no está disponible y volver a generar la aplicación mediante el uso de las versiones más recientes de Microsoft Visual Studio y UWP SDK; o como último recurso, quitar la aplicación ARM desde la Microsoft Store, por lo que la versión de la aplicación (si está disponible) se descarga en los equipos de los usuarios de x86.

Para obtener más información sobre las API de .NET disponibles para aplicaciones para UWP, consulta [.NET para aplicaciones para UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilar una aplicación con una versión anterior de Visual Studio y SDK
Si estás teniendo problemas, asegúrate de usar las versiones más recientes de Microsoft Visual Studio y Windows SDK para compilar la aplicación. Las aplicaciones compiladas con una versión anterior de Visual Studio y SDK pueden tener problemas corregidos en versiones posteriores.

## <a name="debugging"></a>Depuración
También puede usar herramientas existentes para el desarrollo de aplicaciones para la plataforma ARM. Aquí tienes algunos recursos útiles.

- Visual Studio 15,5 Preview 1 y posterior admite la ejecución de aplicaciones ARM32 utilizando el modo de autenticación universal. Esto arranca automáticamente las herramientas de depuración remota necesarias.
- Consulta [Depuración en ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre herramientas y estrategias para depurar en ARM.
