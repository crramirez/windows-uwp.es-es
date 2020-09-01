---
title: Solución de problemas de aplicaciones de escritorio x86
description: Obtenga información acerca de cómo solucionar problemas comunes con una aplicación de escritorio x86 que se ejecuta en ARM64, incluida información acerca de los controladores, las extensiones de Shell y la depuración.
ms.date: 05/09/2018
ms.topic: article
keywords: Windows 10 s, siempre conectado, emulación x86 en ARM, solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 91e142eedc54e6c05f4bbb51e49eb8e516411b48
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155419"
---
# <a name="troubleshooting-x86-desktop-apps"></a>Solución de problemas de aplicaciones de escritorio x86
>[!IMPORTANT]
> El SDK de ARM64 ahora está disponible como parte de Visual Studio 15,8 Preview 1. Se recomienda volver a compilar la aplicación en ARM64 para que la aplicación se ejecute con una velocidad nativa completa. Para obtener más información, consulte la entrada [de blog sobre la versión preliminar temprana de la compatibilidad de Visual Studio con el desarrollo de Windows 10 en ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) .

Si una aplicación de escritorio x86 no funciona de la forma que hace en un equipo x86, aquí tiene algunas instrucciones para ayudarle a solucionar problemas.

|Incidencia|Solución|
|-----|--------|
| La aplicación se basa en un controlador que no está diseñado para ARM. | Vuelva a compilar el controlador x86 en ARM64. Vea [Building ARM64 drivers with the WDK](/windows-hardware/drivers/develop/building-arm64-drivers). |
| La aplicación solo está disponible para x64. | Si desarrolla para Microsoft Store, envíe una versión ARM de la aplicación. Para obtener más información, consulte [arquitecturas de paquetes de aplicaciones](/windows/msix/package/device-architecture). Si es un desarrollador de Win32, se recomienda volver a compilar la aplicación en ARM64. Para obtener más información, consulte [vista previa temprana de la compatibilidad de Visual Studio con Windows 10 en el desarrollo en ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/). |
| La aplicación usa una versión de OpenGL posterior a 1,1 o requiere OpenGL acelerado por hardware. | Use el modo DirectX de la aplicación, si está disponible. las aplicaciones x86 que usan DirectX 9, DirectX 10, DirectX 11 y DirectX 12 funcionarán en ARM. Para obtener más información, consulte [gráficos y juegos de DirectX](/windows/desktop/directx). |
| La aplicación x86 no funciona según lo esperado. | Pruebe a usar el solucionador de problemas de compatibilidad siguiendo las instrucciones del [solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md). Para ver otros pasos para la solución de problemas, consulte el artículo [solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md) . |

## <a name="best-practices-for-wow"></a>Prácticas recomendadas para WOW
Un problema común se produce cuando una aplicación detecta que se está ejecutando bajo WOW y, a continuación, supone que se encuentra en un sistema x64. Una vez hecho esto, la aplicación puede hacer lo siguiente:

- Intente instalar la versión x64 de sí mismo, que no se admite en ARM.
- Compruebe si hay otro software en la vista del registro nativo.
- Suponga que hay disponible una versión de .NET Framework de 64 bits.

Por lo general, una aplicación no debe realizar suposiciones sobre el sistema host cuando se determina que se ejecuta bajo WOW. Evite interactuar tanto como sea posible con componentes nativos del sistema operativo.

Una aplicación puede colocar las claves del registro en la vista del registro nativo o realizar funciones según la presencia de WOW. El **IsWow64Process**  original indica solo si la aplicación se ejecuta en un equipo x64. Ahora, las aplicaciones deben usar [IsWow64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2) para determinar si se están ejecutando en un sistema con compatibilidad con wow. 

## <a name="drivers"></a>Controladores 
Se deben compilar todos los controladores de modo kernel, los controladores de [marco de trabajo de controlador de modo de usuario (UMDF)](/windows-hardware/drivers/wdf/overview-of-the-umdf) y los controladores de impresión para que coincidan con la arquitectura del sistema operativo. Si una aplicación x86 tiene un controlador, dicho controlador debe volver a compilarse para ARM64. La aplicación x86 puede ejecutarse correctamente en la emulación. sin embargo, su controlador tendrá que volver a compilarse para ARM64 y cualquier experiencia de aplicación que dependa del controlador no estará disponible. Para obtener más información sobre cómo compilar el controlador para ARM64, consulte [Building ARM64 drivers with the WDK](/windows-hardware/drivers/develop/building-arm64-drivers).

## <a name="shell-extensions"></a>Extensiones de Shell de : 
Las aplicaciones que intentan enlazar componentes de Windows o cargar sus archivos dll en procesos de Windows deberán volver a compilar esos archivos DLL para que coincidan con la arquitectura del sistema. es decir, ARM64. Normalmente, se usan en los editores de métodos de entrada (IME), las tecnologías de asistencia y las aplicaciones de extensión de Shell (por ejemplo, para mostrar los iconos de almacenamiento en la nube en el explorador o en un menú contextual). Para obtener información sobre cómo volver a compilar las aplicaciones o los archivos dll en ARM64, consulte la entrada [de blog sobre la versión preliminar temprana de la compatibilidad de Visual Studio con Windows 10 en el desarrollo de ARM](https://blogs.windows.com/buildingapps/2018/05/08/visual-studio-support-for-windows-10-on-arm-development/) . 

## <a name="debugging"></a>Depuración
Para investigar el comportamiento de la aplicación con más detalle, consulte [depuración en ARM](/windows-hardware/drivers/debugger/debugging-arm64) para más información sobre las herramientas y las estrategias de depuración en ARM.

## <a name="virtual-machines"></a>Virtual Machines
La plataforma del hipervisor de Windows no se admite en la plataforma de PC móvil Qualcomm Snapdragon 835. Por lo tanto, la ejecución de máquinas virtuales con Hyper-V no funcionará. Seguimos realizando inversiones en estas tecnologías en los conjuntos de chips de Qualcomm futuros. 

## <a name="dynamic-code-generation"></a>Generación dinámica de código
Las aplicaciones de escritorio x86 se emulan en ARM64 por el sistema que genera instrucciones de ARM64 en tiempo de ejecución. Esto significa que, si una aplicación de escritorio x86 impide la generación o modificación dinámica de código en su proceso, no se admite la ejecución de esa aplicación como x86 en ARM64. 

Se trata de una mitigación de seguridad que algunas aplicaciones habilitan en su proceso mediante la API de [SetProcessMitigationPolicy](/windows/desktop/api/processthreadsapi/nf-processthreadsapi-setprocessmitigationpolicy) con la `ProcessDynamicCodePolicy` marca. Para ejecutarse correctamente en ARM64 como un proceso x86, esta directiva de mitigación tendrá que estar deshabilitada.