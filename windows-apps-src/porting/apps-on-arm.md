---
title: Windows 10 en ARM
description: Este artículo proporciona una descripción general de cómo se ejecutarán experiencias y aplicaciones en ARM, cuáles son las limitaciones y dónde acudir para obtener más información.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM, ARM64, x86 emulation, emulación x86
ms.localizationpriority: medium
ms.openlocfilehash: 7450b469f77fec4288ad6dff01ee7673affc8dd9
ms.sourcegitcommit: f3c1a81b50f4a372a15996ac71b3f408a8ee1409
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72237532"
---
# <a name="windows-10-on-arm"></a>Windows 10 en ARM
Originalmente Windows 10 (a diferencia de Windows 10 Mobile) podría ejecutarse únicamente en equipos equipados con procesadores x86 y x64. Ahora, el escritorio de Windows 10 puede ejecutarse en equipos con tecnología de procesadores ARM64 con la actualización de Fall Creators o versiones más recientes. La naturaleza de ahorro de energía de la arquitectura de CPU ARM permite a estos equipos tener una batería que dura todo el día y compatibilidad con redes de datos móviles. Estos equipos proporcionarán una gran compatibilidad de aplicaciones excelentes y permiten ejecutar tus aplicaciones win32 x86 existentes sin modificarse. Para obtener más información o una demostración, consulte el [vídeo de Channel 9 para el equipo conectado siempre](https://channel9.msdn.com/Events/Build/2017/P4171).

Usamos el término *ARM* aquí como un método abreviado para los equipos que ejecutan la versión de escritorio de Windows 10 en procesadores ARM64 (también comúnmente denominados *AArch64*).  Usamos el término *ARM32* aquí como un método abreviado para la arquitectura ARM de 32 bits (comúnmente denominada *ARM* en otra documentación).

## <a name="apps-and-experiences-on-arm"></a>Aplicaciones y experiencias en ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiencias, aplicaciones y controladores integrados de Windows 10
Las experiencias integradas de Windows 10 como Edge, Cortana, menú Inicio y explorador son todas nativas y se ejecutan como ARM64. También se incluyen todos los controladores de dispositivos, como gráficos, redes o el disco duro. Esto garantiza que obtendrá la mejor experiencia del usuario y la duración de la batería del dispositivo que se ejecuta a la velocidad total nativa del procesador Qualcomm Snapdragon.

### <a name="universal-windows-platform-uwp-apps"></a>Aplicaciones para la Plataforma universal de Windows (UWP).
Windows 10 en ARM ejecuta todas las [aplicaciones de UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 y ARM64 desde el Microsoft Store. Las aplicaciones ARM32 y ARM64 se ejecutan de forma nativa sin ninguna emulación, mientras que las aplicaciones x86 se ejecutan en la emulación. Si eres un desarrollador UWP, asegúrate de enviar un paquete ARM para tu aplicación ya que proporcionará la mejor experiencia de usuario para el dispositivo. Paras más información, consulta [Arquitecturas de paquete de aplicación](/windows/msix/package/device-architecture).

>[!NOTE]
> Para compilar la aplicación de UWP de manera nativa como destino de la plataforma ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior, o Visual Studio 2019. Para obtener más información, vea [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Cuando un usuario descarga una aplicación para UWP de Microsoft Store, se instalará la versión ARM32 en un dispositivo ARM64, a menos que solo esté disponible una versión x86. Paras más información sobre arquitecturas, consulta [Arquitecturas de paquete de aplicación](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Aplicaciones de Win32
Además de las aplicaciones para UWP, Windows 10 en ARM también puede ejecutar las aplicaciones Win32 x86 sin modificar, con un buen rendimiento y una experiencia de usuario perfecta, al igual que cualquier equipo. Estas aplicaciones Win32 x86 no tienen que volver a compilarse para ARM y no se deben tener en cuenta si se ejecutan en un procesador ARM. Tenga en cuenta que las aplicaciones Win32 de 64 bits x64 no se admiten, pero la gran mayoría de las aplicaciones tienen versiones x86 disponibles.  Cuando se proporciona la opción de arquitectura de la aplicación, solo tiene que elegir la versión x86 de 32 bits para ejecutar la aplicación en un equipo con Windows 10 en ARM.

## <a name="in-this-section"></a>En esta sección
|Tema | Descripción |
|-----|-----|
|[Cómo funciona la emulación de x86 en ARM](apps-on-arm-x86-emulation.md)|Una introducción que detalla cómo se emulan las aplicaciones x86 en ARM.|
|[Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comunes con aplicaciones x86 cuando se ejecutan en ARM y cómo solucionarlos. |
|[Solución de problemas de aplicaciones ARM en ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comunes con las aplicaciones ARM32 y ARM64 cuando se ejecutan en ARM y cómo corregirlos. |
|[Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md)|Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM. |

## <a name="related-topics"></a>Temas relacionados
|Tema | Descripción |
|-----|-----|
|[Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instrucciones para compilar un controlador ARM64. |
| [Depuración de aplicaciones x86 en ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Instrucciones para la depuración de aplicaciones x86 en ARM. |
