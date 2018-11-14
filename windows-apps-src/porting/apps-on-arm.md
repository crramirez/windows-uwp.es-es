---
title: Windows10 en ARM
author: msatranjr
description: Este artículo proporciona una descripción general de cómo se ejecutarán experiencias y aplicaciones en ARM, cuáles son las limitaciones y dónde acudir para obtener más información.
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM, ARM64, x86 emulation, emulación x86
ms.localizationpriority: medium
ms.openlocfilehash: 8f62a873e84f200a019bde23038ae10b21150072
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6671548"
---
# <a name="windows-10-on-arm"></a>Windows10 en ARM
Originalmente Windows 10 (a diferencia de Windows 10 Mobile) podría ejecutarse únicamente en equipos equipados con procesadores x86 y x64. Ahora, el escritorio de Windows 10 (ediciones Pro y S) pueden ejecutarse en equipos que están equipados con procesadores ARM64 con Fall Creators Update. La naturaleza de ahorro de energía de la arquitectura de CPU ARM permite a estos equipos tener una batería que dura todo el día y compatibilidad con redes de datos móviles. Estos equipos proporcionarán una gran compatibilidad de aplicaciones excelentes y permiten ejecutar tus aplicaciones win32 x86 existentes sin modificarse. P. ej. Adobe Reader. Para obtener más información o una demostración, mira el [vídeo de Channel 9 para el PC siempre conectado](https://channel9.msdn.com/Events/Build/2017/P4171). 

Usamos el término *ARM* aquí como un método abreviado para los equipos que ejecutan la versión de escritorio de Windows 10 en procesadores ARM64 (también comúnmente denominados *AArch64*).  Usamos el término *ARM32* aquí como un método abreviado para la arquitectura ARM de 32 bits (comúnmente denominada *ARM* en otra documentación).

## <a name="apps-and-experiences-on-arm"></a>Aplicaciones y experiencias en ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiencias, aplicaciones y controladores integrados de Windows 10
Las experiencias integradas de Windows 10, como Edge, Cortana, menú Inicio y el Explorador son todas nativas y se ejecutan como ARM64 (o ARM32). Estas también incluye todos los controladores de dispositivo como los elementos gráficos, funciones de red o el disco duro. Esto garantiza que se obtendrá la mejor experiencia del usuario y la mejor duración de la batería del dispositivo que se ejecuta a la velocidad nativa total del procesador Qualcomm Snapdragon

### <a name="universal-windows-platform-uwp-apps"></a>Aplicaciones para la Plataforma universal de Windows (UWP).
Windows 10 en ARM ejecuta todas las [aplicaciones para UWP](../get-started/universal-application-platform-guide.md) x86 y ARM32 de Microsoft Store. Las aplicaciones ARM32 se ejecutan de forma nativa sin ninguna emulación, mientras que las aplicaciones x86 se ejecutan en modo de emulación. Si eres un desarrollador UWP, asegúrate de enviar un paquete ARM para tu aplicación ya que proporcionará la mejor experiencia de usuario para el dispositivo. Paras más información, consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md).

>[!IMPORTANT] 
> Cuando un usuario descarga una aplicación para UWP de Microsoft Store, se instalará la versión ARM32 en un dispositivo ARM64, a menos que solo esté disponible una versión x86. Paras más información sobre arquitecturas, consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md).

### <a name="win32-apps"></a>Aplicaciones de Win32
Además de aplicaciones para UWP, Windows 10 en ARM también puede ejecutar tus aplicaciones x86 Sin32 (por ejemplo, Adobe Reader) sin modificarse, con un buen rendimiento y una experiencia de usuario transparente, igual que cualquier PC. Estas aplicaciones x86 win32 no tienen que volver a compilarse para ARM y ni siquiera se dan cuenta de que se ejecutan en procesadores ARM. Ten en cuenta que las aplicaciones x64 Win32 de 64 bits no se admiten, pero la mayoría de aplicaciones tienen versiones x86 de sus aplicaciones; por lo tanto, desde la perspectiva del usuario, elige el instalador de x86 de 32 bits para ejecutarlo en el equipo Windows en ARM.

## <a name="in-this-section"></a>En esta sección
|Tema | Descripción |
|-----|-----|
|[Cómo funciona la emulación x86 en ARM](apps-on-arm-x86-emulation.md)|Una introducción que detalla cómo se emulan las aplicaciones x86 en ARM.|
|[Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comunes con aplicaciones x86 cuando se ejecutan en ARM y cómo solucionarlos. |
|[Solución de problemas de aplicaciones ARM32 en ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comunes con aplicaciones ARM32 cuando se ejecutan en ARM y cómo solucionarlos. |
|[Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md)|Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM. |

## <a name="related-topics"></a>Artículos relacionados
|Tema | Descripción |
|-----|-----|
|[Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instrucciones para compilar un controlador ARM64. |
| [Depuración de aplicaciones x86 en ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Instrucciones para la depuración de aplicaciones x86 en ARM. |
