---
title: Limitaciones de aplicaciones y experiencias en ARM
description: Pasos de solución de problemas para las aplicaciones que no están funcionando correctamente en ARM.
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado, limitations, limitaciones, windows 10 on ARM, windows 10 en ARM
ms.localizationpriority: medium
redirect_url: https://docs.microsoft.com/en-us/windows/uwp/porting/apps-on-arm-troubleshooting-x86
ms.openlocfilehash: 5fa05e1dfd04208ba547a692473fc3df136e6e4f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8919281"
---
# <a name="limitations-of-apps-and-experiences-on-arm"></a>Limitaciones de aplicaciones y experiencias en ARM
Windows 10 en ARM tiene las siguientes limitaciones necesarias:

- **Solo se admiten controladores ARM64**. Al igual que con todas las arquitecturas, los controladores modo kernel, los controladores [Marco de controlador de modo usuario (UMDF)](https://docs.microsoft.com/en-us/windows-hardware/drivers/wdf/overview-of-the-umdf) y los controladores de impresión deben compilarse para que coincidan con la arquitectura del sistema operativo. Mientras el sistema operativo de ARM tiene las funcionalidades para emular aplicaciones de modo usuario x86, los controladores implementados para otras arquitecturas (como x64 o x86) actualmente no se emulan y, por consiguiente, no se admiten en esta plataforma. Cualquier aplicación que funcione con su propio controlador personalizado debe migrarse a ARM64. En escenarios limitados, la aplicación se puede ejecutar como x86 en emulación, pero la parte del controlador de la aplicación debe migrarse a ARM64. Para obtener más información sobre la compilación del controlador para ARM64, consulta [Compilar controladores de ARM64 con el WDK](https://review.docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers?branch=rs4-arm64).

- **Las aplicaciones x64 no se admiten**. Windows 10 en ARM no admite la emulación de aplicaciones x64.

- **Algunos juegos no funcionan**. Los juegos y las aplicaciones que usan una versión de OpenGL posterior a 1.1 o que requieren OpenGL acelerado por hardware no funcionan. Además, los juegos que se basan en los controladores "a prueba de trampas" no se admiten en esta plataforma.

- **Las aplicaciones que personalizan la experiencia de Windows es posible que no funcionen correctamente**. Los componentes de sistema operativo nativos no pueden cargar componentes no nativos. Algunos ejemplos de aplicaciones que comúnmente hacen esto son algunos editores de métodos de entrada (IME), tecnologías de asistencia y aplicaciones de almacenamiento en la nube. IME y las tecnologías de asistencia a menudo para enlazar en la pila de entrada para la mayor parte de la funcionalidad de la aplicación. Las aplicaciones de almacenamiento en la nube usan extensiones de shell (por ejemplo, iconos en el Explorador y adiciones a menús contextuales); sus extensiones de shell pueden fallar y, si el error no se controla correctamente, es posible que la aplicación en sí no funcione en absoluto.

- **Las aplicaciones en las que se supone que todos los dispositivos basados en ARM ejecutan una versión móvil de Windows es posible que no funcionen correctamente**. Las aplicaciones de esta suposición pueden aparecer con una orientación incorrecta, presentar una presentación o un diseño de interfaz de usuario inesperado o no pueda iniciarse por completo al intentar invocar API solo móviles sin comprobar primero la disponibilidad del contrato.

- **La plataforma del hipervisor de Windows no se admite en ARM**. Ejecutar máquinas virtuales con Hyper-V no funcionará en un dispositivo ARM.

La siguiente tabla enumera algunos problemas comunes y ofrece sugerencias sobre cómo resolverlos.

|Problema|Solución|
|-----|--------|
| La aplicación se basa en un controlador que no está diseñado para ARM. | Volver a compilar el controlador x86 para ARM64. Consulta [Compilar controladores de ARM64 con el WDK](https://docs.microsoft.com/en-us/windows-hardware/drivers/develop/building-arm64-drivers). |
| La aplicación solo está disponible para x64. | Si desarrollas para Microsoft Store, envía una versión ARM de tu aplicación. Consulta [Arquitecturas de paquete de aplicación](../packaging/device-architecture.md) para más información. Si eres un desarrollador de Win32, distribuye una versión x86 de la aplicación. |
| La aplicación usa una versión de OpenGL posterior a 1.1 o requiere OpenGL acelerado por hardware. | Las aplicaciones x86 que usan DirectX 9, DirectX 10, DirectX 11 y DirectX 12 funcionarán en ARM. Para obtener más información, consulta [Juegos y gráficos DirectX](https://msdn.microsoft.com/en-us/library/windows/desktop/ee663274(v=vs.85).aspx). |
| Tu aplicación x86 no funciona como esperabas. | Prueba a usar el Solucionador de problemas de compatibilidad siguiendo las instrucciones del [Solucionador de problemas de compatibilidad de programas en ARM](apps-on-arm-program-compat-troubleshooter.md). Para otros pasos de solución de problemas, consulta el artículo [Solución de problemas de aplicaciones x86 en ARM](apps-on-arm-troubleshooting-x86.md). |
| La aplicación x86 no detecta que se está ejecutando en ARM. | Usa [IsWow64Process2](https://msdn.microsoft.com/en-us/library/windows/desktop/mt804318(v=vs.85).aspx) para determinar si la aplicación se ejecuta en ARM. |
| Tu aplicación ARM32 para UWP no funciona como esperabas. | Consulta [Solución de problemas de aplicaciones ARM32 en ARM](apps-on-arm-troubleshooting-arm32.md) para aprender a hacer que tu aplicación funcione correctamente en ARM. |
