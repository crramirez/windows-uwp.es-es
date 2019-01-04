---
title: Solución de problemas de aplicaciones para UWP de ARM32
description: Problemas comunes con aplicaciones ARM32 cuando se ejecutan en ARM y cómo solucionarlos.
ms.date: 01/03/2019
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM32 apps on ARM, aplicaciones ARM32 en ARM, windows 10 on ARM, windows 10 en ARM, troubleshooting, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 75019df4b7d70dad20aea1a256abac05c93a481d
ms.sourcegitcommit: 62bc4936ca8ddf1fea03d43a4ede5d14a5755165
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991641"
---
# <a name="troubleshooting-arm-uwp-apps"></a>Solución de problemas de ARM aplicaciones para UWP

Si la aplicación para UWP de ARM64 o ARM32 no funciona correctamente en ARM, aquí es orientativa que puede ayudarte.

>[!NOTE]
> Para compilar la aplicación para UWP de destino de forma nativa la plataforma de ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior. Para obtener más información, consulta [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).

## <a name="common-issues"></a>Problemas comunes
Estos son algunos problemas comunes a tener en cuenta al solucionar problemas de aplicaciones ARM32 y ARM64.

### <a name="using-windows-10-mobile-only-apis-on-arm-based-processors"></a>Utilizar API solo de Windows 10 Mobile en procesadores basados en ARM
Las aplicaciones ARM podrán enfrentarse a problemas al usar API solo móviles (por ejemplo, **HardwareButtons**). Para mitigarlo, puedes detectar dinámicamente si la aplicación se ejecuta en Windows 10 Mobile antes de llamar a estas API. Sigue las instrucciones de la entrada de blog, [Detección dinámica de funciones con contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/).

### <a name="including-dependencies-not-supported-by-uwp-apps"></a>Incluir dependencias no compatibles con aplicaciones para UWP
Aplicaciones universales de la plataforma de Windows (UWP) que no compiladas debidamente con Visual Studio y el SDK de UWP pueden tener dependencias en componentes de sistema operativo que no están disponibles para las aplicaciones ARM que se ejecutan en un sistema de ARM64. Entre los ejemplos de estas dependencias se incluyen:

- Esperar que estén disponibles partes de.NET Framework.
- Hacer referencia a los componentes .NET de terceros que no son compatibles con UWP.

Pueden resolverse estos problemas: eliminando las dependencias no disponibles y recompilar la aplicación mediante el uso de las versiones más recientes de Microsoft Visual Studio y SDK de UWP; o como último recurso, eliminar la aplicación ARM de Microsoft Store, para que la versión de la aplicación (si está disponible) se descargue en los equipos de los usuarios de x86.

Para obtener más información sobre las API de .NET disponibles para aplicaciones para UWP, consulta [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/mt185501.aspx)

### <a name="compiling-an-app-with-an-older-version-of-visual-studio-and-sdk"></a>Compilar una aplicación con una versión anterior de Visual Studio y SDK
Si estás teniendo problemas, asegúrate de usar las versiones más recientes de Microsoft Visual Studio y Windows SDK para compilar la aplicación. Las aplicaciones compiladas con una versión anterior de Visual Studio y SDK pueden tener problemas corregidos en versiones posteriores.

## <a name="debugging"></a>Depuración
Puedes usar las herramientas existentes para desarrollar aplicaciones para la plataforma ARM. Aquí tienes algunos recursos útiles.

- Visual Studio 15,5 Preview 1 y posterior admite la ejecución de aplicaciones ARM32 utilizando el modo de autenticación universal. Esto arranca automáticamente las herramientas de depuración remota necesarias.
- Consulta [Depuración en ARM64](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre herramientas y estrategias para depurar en ARM.
