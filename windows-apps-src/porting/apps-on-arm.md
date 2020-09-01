---
title: Windows 10 en ARM
description: En este artículo se proporciona información general sobre cómo las experiencias y las aplicaciones se ejecutarán en ARM, cuáles son las limitaciones y dónde puede ir para obtener más información.
ms.date: 05/22/2020
ms.topic: article
keywords: Windows 10 s, siempre conectado, ARM, ARM64, emulación x86
ms.localizationpriority: medium
ms.openlocfilehash: 39ff5b2aa6c72feaeaea0a7a61100196c109257c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162299"
---
# <a name="windows-10-on-arm"></a>Windows 10 en ARM
Originalmente Windows 10 (como se diferenciaba de Windows 10 Mobile) solo podía ejecutarse en equipos con procesadores x86 y x64. Ahora, el escritorio de Windows 10 puede ejecutarse en equipos con tecnología de procesadores ARM64 con la actualización de Fall Creators o versiones más recientes. La naturaleza de ahorro de energía de la arquitectura de CPU de ARM permite que estos equipos tengan una duración de la batería de todo el día y soporte técnico para redes de datos móviles. Estos equipos proporcionarán una excelente compatibilidad con las aplicaciones y le permitirán ejecutar las aplicaciones Win32 existentes de x86 sin modificar. Para obtener más información o una demostración, consulte el [vídeo de Channel 9 para el equipo conectado siempre](https://channel9.msdn.com/Events/Build/2017/P4171).

Usamos el término *ARM* aquí como una abreviatura para equipos que ejecutan la versión de escritorio de Windows 10 en ARM64 (también denominados comúnmente *AArch64*).  Usamos el término *ARM32* aquí como una forma abreviada de la arquitectura arm de 32 bits (normalmente denominada *ARM* en otra documentación).

## <a name="apps-and-experiences-on-arm"></a>Aplicaciones y experiencias en ARM

### <a name="built-in-windows-10-experiences-apps-and-drivers"></a>Experiencias, aplicaciones y Controladores integrados de Windows 10
Las experiencias integradas de Windows 10 como Edge, Cortana, menú Inicio y explorador son todas nativas y se ejecutan como ARM64. También se incluyen todos los controladores de dispositivos, como gráficos, redes o el disco duro. Esto garantiza que obtendrá la mejor experiencia del usuario y la duración de la batería del dispositivo que se ejecuta a la velocidad total nativa del procesador Qualcomm Snapdragon.

### <a name="universal-windows-platform-uwp-apps"></a>Aplicaciones Plataforma universal de Windows (UWP)
Windows 10 en ARM ejecuta todas las [aplicaciones de UWP](../get-started/universal-application-platform-guide.md) x86, ARM32 y ARM64 desde el Microsoft Store. Las aplicaciones ARM32 y ARM64 se ejecutan de forma nativa sin ninguna emulación, mientras que las aplicaciones x86 se ejecutan en la emulación. Si es un desarrollador de UWP, asegúrese de que envía un paquete ARM para la aplicación, ya que esto le proporcionará la mejor experiencia de usuario para el dispositivo. Para obtener más información, consulte [arquitecturas de paquetes de aplicaciones](/windows/msix/package/device-architecture).

>[!NOTE]
> Para compilar la aplicación de UWP de manera nativa como destino de la plataforma ARM64, debe tener Visual Studio 2017 versión 15,9 o posterior, o Visual Studio 2019. Para más información, vea [esta entrada de blog](https://blogs.windows.com/buildingapps/2018/11/15/official-support-for-windows-10-on-arm-development).


>[!IMPORTANT]
> Windows 10 en ARM admite aplicaciones de UWP x86, ARM32 y ARM64 desde la tienda en dispositivos ARM64. Cuando un usuario descarga la aplicación para UWP en un dispositivo ARM64, el sistema operativo instalará automáticamente la versión óptima de la aplicación que está disponible. Si envía las versiones x86, ARM32 y ARM64 de la aplicación a la tienda, el sistema operativo instalará automáticamente la versión ARM64 de la aplicación. Si solo envía versiones x86 y ARM32 de la aplicación, el sistema operativo instalará la versión de ARM32. Si solo envía la versión x86 de la aplicación, el sistema operativo instalará esa versión y la ejecutará en la emulación. Para obtener más información sobre las arquitecturas, consulte [arquitecturas de paquetes de aplicaciones](/windows/msix/package/device-architecture).

### <a name="win32-apps"></a>Aplicaciones Win32
Además de las aplicaciones para UWP, Windows 10 en ARM también puede ejecutar las aplicaciones Win32 x86 sin modificar, con un buen rendimiento y una experiencia de usuario perfecta, al igual que cualquier equipo. Estas aplicaciones Win32 x86 no tienen que volver a compilarse para ARM y no se deben tener en cuenta si se ejecutan en un procesador ARM. Tenga en cuenta que las aplicaciones Win32 de 64 bits x64 no se admiten, pero la gran mayoría de las aplicaciones tienen versiones x86 disponibles.  Cuando se proporciona la opción de arquitectura de la aplicación, solo tiene que elegir la versión x86 de 32 bits para ejecutar la aplicación en un equipo con Windows 10 en ARM.

## <a name="downloads"></a>Descargas

Visual Studio 2019 proporciona varias descargas de herramientas para Windows 10 en ARM. Los usuarios que utilizan Visual Studio 2017 pueden usar el instalador para buscar e instalar herramientas y paquetes comparables. Tenga en cuenta que, para seguir estos pasos, debe usar Visual Studio 2019.

### <a name="visual-c-redistributable"></a>Visual C++ Redistributable

El paquete Redist Visual C++ está disponible para las aplicaciones ARM. Visite la [Página de descargas de Visual Studio](https://visualstudio.microsoft.com/downloads/) Desplácese hacia abajo hasta **todas las descargas**, Abra **otras herramientas y marcos y**navegue hasta la entrada **Microsoft Visual C++ redistribuible para Visual Studio 2019** . Seleccione el botón de opción **ARM64** y, a continuación, **Descargar**.

### <a name="remote-tools"></a>Herramientas remotas

Herramientas remotas para Visual Studio están disponibles para las aplicaciones ARM. Visite la [Página de descargas de Visual Studio](https://visualstudio.microsoft.com/downloads/) Desplácese hacia abajo hasta **todas las descargas**, Abra **herramientas para Visual Studio 2019**y, a continuación, navegue hasta la entrada **herramientas remotas para Visual Studio 2019** . Seleccione el botón de opción **ARM64* y, a continuación, **Descargar**.


## <a name="in-this-section"></a>En esta sección
|Tema | Descripción |
|-----|-----|
|[Cómo funciona la emulación de x86 en ARM](apps-on-arm-x86-emulation.md)|Información general que detalla cómo se emulan las aplicaciones x86 en ARM.|
|[Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md)|Problemas comunes con las aplicaciones x86 cuando se ejecutan en ARM y cómo corregirlos. |
|[Solución de problemas de aplicaciones ARM en ARM](apps-on-arm-troubleshooting-arm32.md)|Problemas comunes con las aplicaciones ARM32 y ARM64 cuando se ejecutan en ARM y cómo corregirlos. |
|[Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md)|Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM. |

## <a name="related-topics"></a>Temas relacionados
|Tema | Descripción |
|-----|-----|
|[Compilar controladores de ARM64 con el WDK](/windows-hardware/drivers/develop/building-arm64-drivers)|Instrucciones para crear un controlador de ARM64. |
| [Depuración de aplicaciones x86 en ARM](/windows-hardware/drivers/debugger/debugging-arm64) | Instrucciones para depurar aplicaciones x86 en ARM. |