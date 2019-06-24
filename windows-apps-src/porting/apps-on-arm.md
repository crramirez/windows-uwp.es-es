---
title: Windows 10 en ARM
description: Este artículo proporciona una descripción general de cómo se ejecutarán experiencias y aplicaciones en ARM, cuáles son las limitaciones y dónde acudir para obtener más información.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado ARM, ARM64, x86 emulation, emulación x86
ms.localizationpriority: medium
ms.openlocfilehash: 24f33ffc1c5661c5450c24c6fa271e59788e5229
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319704"
---
# <a name="windows-10-on-arm"></a>Windows 10 en ARM
Originalmente Windows 10 (a diferencia de Windows 10 Mobile) podría ejecutarse únicamente en equipos equipados con procesadores x86 y x64. Ahora, el escritorio de Windows 10 (ediciones Pro y S) pueden ejecutarse en equipos que están equipados con procesadores ARM64 con Fall Creators Update. La naturaleza de ahorro de energía de la arquitectura de CPU ARM permite a estos equipos tener una batería que dura todo el día y compatibilidad con redes de datos móviles. Estos equipos proporcionarán una gran compatibilidad de aplicaciones excelentes y permiten ejecutar tus aplicaciones win32 x86 existentes sin modificarse. P. ej. Adobe Reader. Para obtener más información o una demostración, mira el [vídeo de Channel 9 para el PC siempre conectado](https://channel9.msdn.com/Events/Build/2017/P4171).

Usamos el término *ARM* aquí como un método abreviado para los equipos que ejecutan la versión de escritorio de Windows 10 en procesadores ARM64 (también comúnmente denominados *AArch64*).  Usamos el término *ARM32* aquí como un método abreviado para la arquitectura ARM de 32 bits (comúnmente denominada *ARM* en otra documentación).

## <a name="apps-and-experiences-on-arm"></a>Aplicaciones y experiencias en ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiencias, aplicaciones y controladores integrados de Windows 10
Las experiencias integradas de Windows 10, como Edge, Cortana, menú Inicio y el Explorador son todas nativas y se ejecutan como ARM64 (o ARM32). Estas también incluye todos los controladores de dispositivo como los elementos gráficos, funciones de red o el disco duro. Esto garantiza que se obtendrá la mejor experiencia del usuario y la mejor duración de la batería del dispositivo que se ejecuta a la velocidad nativa total del procesador Qualcomm Snapdragon

### <a name="universal-windows-platform-uwp-apps"></a>Aplicaciones para la Plataforma universal de Windows (UWP).
Windows 10 en ARM se ejecuta todos los x86, ARM32 y ARM64 [aplicaciones para UWP](../get-started/universal-application-platform-guide.md) desde la Microsoft Store. ARM32 y ARM64 aplicaciones se ejecuten de forma nativa sin ninguna emulación, mientras las aplicaciones se ejecutan bajo la emulación de x86. Si eres un desarrollador UWP, asegúrate de enviar un paquete ARM para tu aplicación ya que proporcionará la mejor experiencia de usuario para el dispositivo. Paras más información, consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md).

>[!NOTE]
> Para compilar la aplicación para UWP como destino la plataforma ARM64 de forma nativa, debe tener Visual Studio 2017 versión 15,9 o posterior. Para obtener más información, consulte [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development/).

>[!IMPORTANT]
> Cuando un usuario descarga una aplicación para UWP de Microsoft Store, se instalará la versión ARM32 en un dispositivo ARM64, a menos que solo esté disponible una versión x86. Paras más información sobre arquitecturas, consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md).

### <a name="win32-apps"></a>Aplicaciones de Win32
Además de las aplicaciones UWP, Windows 10 en ARM también puede ejecutar su x86 Win32 aplicaciones (por ejemplo, Adobe Reader) sin modificar, con buen rendimiento y experiencia de usuario uniforme, al igual que cualquier equipo. Estos x86 aplicaciones Win32 no tienen que volver a compilar para ARM y no se dan cuenta incluso se ejecutan en el procesador ARM. Ten en cuenta que las aplicaciones x64 Win32 de 64 bits no se admiten, pero la mayoría de aplicaciones tienen versiones x86 de sus aplicaciones; por lo tanto, desde la perspectiva del usuario, elige el instalador de x86 de 32 bits para ejecutarlo en el equipo Windows en ARM.

## <a name="in-this-section"></a>En esta sección
|Tema | Descripción |
|-----|-----|
|[Cómo funciona la emulación de x86 en ARM](apps-on-arm-x86-emulation.md)|Una introducción que detalla cómo se emulan las aplicaciones x86 en ARM.|
|[Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comunes con aplicaciones x86 cuando se ejecutan en ARM y cómo solucionarlos. |
|[Solución de problemas de aplicaciones ARM en ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comunes con las aplicaciones de ARM32 y ARM64 cuando se ejecuta en ARM y cómo corregirlos. |
|[Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md)|Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM. |

## <a name="related-topics"></a>Temas relacionados
|Tema | Descripción |
|-----|-----|
|[Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers)|Instrucciones para compilar un controlador ARM64. |
| [Depuración x86 aplicaciones en ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) | Instrucciones para la depuración de aplicaciones x86 en ARM. |
