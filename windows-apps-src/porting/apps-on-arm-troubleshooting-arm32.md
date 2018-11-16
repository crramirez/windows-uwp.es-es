---
title: Solución de problemas de aplicaciones para UWP de ARM32
author: msatranjr
description: Problemas comunes con aplicaciones ARM32 cuando se ejecutan en ARM y cómo solucionarlos.
ms.author: misatran
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM32 apps on ARM, aplicaciones ARM32 en ARM, windows 10 on ARM, windows 10 en ARM, troubleshooting, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 4eaeccb8b4003bb835ee4700d1df57cd8cefcda0
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103879"
---
# <a name="troubleshooting-arm32-uwp-apps"></a>Solución de problemas de aplicaciones para UWP de ARM32
>[!IMPORTANT]
> El SDK de ARM64 ahora está disponible como parte de Visual Studio 15.8 Preview 1. Te recomendamos que vuelvas a compilar la aplicación para ARM64 para que la aplicación se ejecute a velocidad nativa total. Para obtener más información, consulta entrada de blog [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) (Vista previa de compatibilidad de Visual Studio para Windows 10 en el desarrollo de ARM).

Si tu aplicación para UWP de ARM32 no funciona correctamente en ARM, aquí te ofrecemos algunas indicaciones que pueden ayudarte. 

## <a name="common-issues"></a>Problemas comunes
Aquí incluimos algunos problemas comunes a tener en cuenta al solucionar problemas en aplicaciones ARM32.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilizar API solo de Windows 10 Mobile en procesadores basados en ARM 
Las aplicaciones ARM32 pueden tener problemas al usar API solo móviles (por ejemplo, **HardwareButtons**). Para mitigarlo, puedes detectar dinámicamente si la aplicación se ejecuta en Windows 10 Mobile antes de llamar a estas API. Sigue las instrucciones de la entrada de blog, [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluir dependencias no compatibles con aplicaciones para UWP
Las aplicaciones de Plataforma universal de Windows (UWP) no compiladas debidamente con Visual Studio y el SDK de UWP pueden tener dependencias en componentes de sistema operativo que no están disponibles para aplicaciones ARM32 que se ejecutan en un sistema de ARM64. Entre los ejemplos de estas dependencias se incluyen:

- Esperar que estén disponibles partes de.NET Framework.
- Hacer referencia a los componentes .NET de terceros que no son compatibles con UWP.

Pueden resolverse estos problemas: eliminando las dependencias no disponibles y recompilar la aplicación con las versiones más recientes de Microsoft Visual Studio y el SDK de UWP; o como último recurso, eliminar la aplicación ARM32 de Microsoft Store, para que la versión x86 de la aplicación (si está disponible) se descargue en los equipos de los usuarios. 

Para obtener más información sobre las API de .NET disponibles para aplicaciones para UWP, consulta [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilar una aplicación con una versión anterior de Visual Studio y SDK
Si estás teniendo problemas, asegúrate de usar las versiones más recientes de Microsoft Visual Studio y Windows SDK para compilar la aplicación. Las aplicaciones compiladas con una versión anterior de Visual Studio y SDK pueden tener problemas corregidos en versiones posteriores.

## <a name="debugging"></a>Depuración
Puedes usar las herramientas existentes para desarrollar aplicaciones ARM32 para la plataforma ARM. Aquí tienes algunos recursos útiles.

- Visual Studio 15,5 Preview 1 y posterior admite la ejecución de aplicaciones ARM32 utilizando el modo de autenticación universal. Esto arranca automáticamente las herramientas de depuración remota necesarias.
- Consulta [Depuración en ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre herramientas y estrategias para depurar en ARM.