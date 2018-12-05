---
title: Solución de problemas de aplicaciones de escritorio x86
description: Problemas comunes con aplicaciones x86 cuando se ejecutan en ARM y cómo solucionarlos.
ms.date: 05/09/2018
ms.topic: article
keywords: windows 10 s, siempre conectado, emulación x86 en ARM, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 396bb0bf2c5ba5236e0e46e7b474867ffacb8c75
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8729749"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Solución de problemas de aplicaciones de escritorio x86
>[!IMPORTANT]
> El SDK de ARM64 ahora está disponible como parte de Visual Studio 15.8 Preview 1. Te recomendamos que vuelvas a compilar la aplicación para ARM64 para que la aplicación se ejecute a velocidad nativa total. Para obtener más información, consulta entrada de blog [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) (vista previa de compatibilidad de Visual Studio para Windows 10 en el desarrollo de ARM).

Si una aplicación de escritorio x86 no funciona de forma que lo hace en un equipo x86, aquí te ofrecemos algunas indicaciones para ayudarte a solucionar problemas.

|Problema|Solución|
|-----|--------|
| La aplicación se basa en un controlador que no está diseñado para ARM. | Volver a compilar el controlador x86 para ARM64. Consulta [Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| La aplicación solo está disponible para x64. | Si desarrollas para Microsoft Store, envía una versión ARM de tu aplicación. Consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md) para más información. Si eres un desarrollador de Win32, te recomendamos que vuelvas a compilar tu aplicación para ARM64. Para obtener más información, consulta [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) (Vista previa de compatibilidad de Visual Studio para Windows 10 en el desarrollo de ARM). |
| La aplicación usa una versión de OpenGL posterior a 1.1 o requiere OpenGL acelerado por hardware. | Usa el modo DirectX de la aplicación, si está disponible. Las aplicaciones x86 que usan DirectX 9, DirectX 10, DirectX 11 y DirectX 12 funcionarán en ARM. Para obtener más información, consulta [Juegos y gráficos DirectX](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Tu aplicación x86 no funciona como esperabas. | Prueba a usar el Solucionador de problemas de compatibilidad siguiendo las instrucciones del [Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md). Para otros pasos de solución de problemas, consulta el artículo [Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md). |

## <a name="best-practices-for-wow"></a>Prácticas recomendadas para WOW
Un problema común se produce cuando una aplicación detecta que se ejecuta en WOW y, a continuación, se da por hecho que se encuentra en un sistema x64. Después de hacer esta suposición, la aplicación puede hacer lo siguiente:

- Intenta instalar la versión x64 de sí misma, que no se admite en ARM.
- Comprueba otro software en la vista del registro nativo.
- Supongamos que está disponible un .NET Framework de 64 bits.

Por lo general, una aplicación no debe hacer suposiciones sobre el sistema host cuando tiene la determinación de ejecutarse en WOW. Evita interactuar con componentes nativos del sistema operativo tanto como sea posible.

Una aplicación puede colocar las claves del registro en la vista del registro nativo o realizar funciones en función de la presencia de WOW. El **IsWow64Process** original solo indica si la aplicación se ejecuta en un equipo x64. Las aplicaciones deben usar ahora [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) para determinar si se está ejecutando en un sistema con compatibilidad WOW. 

## <a name="drivers"></a>Controladores 
Todos los controladores modo kernel, los controladores [Marco de controlador de modo usuario (UMDF)](https://docs.microsoft.com/windows-hardware/drivers/wdf/overview-of-the-umdf) y los controladores de impresión deben compilarse para que coincidan con la arquitectura del sistema operativo. Si una aplicación x86 tiene un controlador, ese controlador debe compilarse para ARM64. La aplicación x86 puede funcionar bien en modo de emulación, sin embargo, su controlador tendrá que recompilarse para ARM64 y cualquier experiencia de aplicación que dependa del controlador no estará disponible. Para obtener más información sobre la compilación del controlador para ARM64, consulta [Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>Extensiones de Shell de : 
Aplicaciones que intentan enlazar componentes de Windows o cargar sus archivos DLL en procesos de Windows tendrán que volver a compilar los archivos DLL para que coincida con la arquitectura del sistema; es decir, ARM64. Por lo general, estos los usan los editores de métodos de entrada (IME), las tecnologías de asistencia y aplicaciones del shell de extensión (por ejemplo, para mostrar los iconos de almacenamiento en la nube en el Explorador o un menú contextual). Para obtener información sobre cómo volver a compilar las aplicaciones o archivos DLL para ARM64, consulta la entrada de blog [Early preview of Visual Studio support for Windows 10 on ARM development](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) (Vista previa de compatibilidad de Visual Studio para Windows 10 en el desarrollo de ARM). 

## <a name="debugging"></a>Depuración
Para investigar el comportamiento de la aplicación con más detalle, consulta [Depuración en ARM](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-arm64) para obtener más información sobre herramientas y estrategias para la depuración en ARM.

## <a name="virtual-machines"></a>Máquinas virtuales
La plataforma de hipervisor de Windows no se admite en la plataforma de PC de Qualcomm Snapdragon 835 Mobile. Además, ejecutar máquinas virtuales con Hyper-V no funcionará. Continuamos invirtiendo en estas tecnologías en futuros conjuntos de chips de Qualcomm. 