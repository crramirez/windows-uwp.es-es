---
title: Solucionador de problemas de compatibilidad de programas en ARM
author: msatranjr
description: Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, always connected, siempre conectado, compatibility troubleshooter, solucionador de problemas de conectividad, windows on ARM, windows en ARM
ms.localizationpriority: medium
ms.openlocfilehash: 4765ad324e90167c7279c9245bccd840bce1163d
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972223"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Solucionador de problemas de compatibilidad de programas en ARM
La emulación para admitir aplicaciones x86 es una nueva funcionalidad creada para Windows 10 en ARM64. A veces la emulación realiza optimizaciones que no proporcionan la mejor experiencia. Puedes usar el Solucionador de problemas de compatibilidad de programas para activar o desactivar la configuración de emulación para tu aplicación x86, reduciendo las optimizaciones predeterminadas y aumentando posiblemente la compatibilidad.

## <a name="start-the-program-compatibility-troubleshooter"></a>Iniciar el Solucionador de problemas de compatibilidad de programas
El [Solucionador de problemas de compatibilidad de programas](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible) se inicia manualmente de la misma manera en cualquier equipo Windows 10: haz clic en un archivo ejecutable (.exe) y selecciona **Solucionar problemas de compatibilidad**. Aparece esta pantalla.

![Captura de pantalla de la opción Solucionar problemas de compatibilidad](images/arm/Capture4.png)

Si haces clic en **Programa de solución de problemas** aparecerá con las opciones siguientes.

![Captura de pantalla de la opción para solucionar problemas de compatibilidad](images/arm/Capture5.png)

Todas las opciones habilitan la configuración aplicable y aplicada en todos los equipos de escritorio de Windows 10. Además, las opciones primera, segunda y cuarta aplican la configuración de emulación [Deshabilitar la memoria caché de aplicaciones](#disable-app-cache) y [Deshabilitar el modo de ejecución híbrida](#disable-hybrid-exec-mode).

## <a name="toggling-emulation-settings"></a>Alternancia de configuración de emulación
> [!WARNING]
> Cambiar la configuración de emulación puede provocar que la aplicación se bloquee de forma inesperada o no se inicie en absoluto.

Puedes alternar la configuración de emulación haciendo clic con el botón derecho en el archivo ejecutable y seleccionando **Propiedades**.

En ARM, una sección titulada **Windows 10 en ARM** estará disponibles en la pestaña **Compatibilidad**. Haga clic en **Cambiar la configuración de emulación** para iniciar una segunda ventana como aquí.

![Pantalla de cambio de la configuración de emulación](images/arm/Capture.png)

Esta ventana proporciona dos formas de modificar la configuración de emulación. Puedes seleccionar un grupo definido previamente de la configuración de emulación, o puedes hacer clic en la opción **Usar configuración avanzada** para habilitar la elección de opciones de configuración individuales.

La configuración de emulación agrupada reduce las optimizaciones del rendimiento en favor de la calidad. A continuación encontrarás algunas opciones de configuración agrupadas que puedes seleccionar.

![Pantalla de cambio de la configuración de emulación 2](images/arm/Capture2.png)

Selecciona **Usar configuración avanzada** para elegir opciones de configuración individuales como se describe en esta tabla.

| Configuración de emulación | Resultado |
| ----------------- | ----------- |
| <p id="disable-app-cache">Deshabilitar la memoria caché de aplicaciones</p> | El sistema operativo almacenará en caché bloques de código compilados para reducir la sobrecarga de emulación en ejecuciones posteriores. Este ajuste requiere que el emulador vuelva a compilar todo el código de la aplicación en tiempo de ejecución. |
| <p id="disable-hybrid-exec-mode">Deshabilitar el modo de ejecución híbrida</p> | Los archivos binarios Compiled Hybrid Portable Executable (CHPE) son archivos binarios compatibles con x86 que incluyen código nativo ARM64 para mejorar el rendimiento, pero es posible que no sean compatibles con algunas aplicaciones. Esta configuración fuerza el uso de archivos binarios solo de x86. |
| Soporte de código automática modificando estricta | Esta opción se activa para asegurarse de que cualquier código de modificación automática se admita correctamente en la emulación. Los escenarios más habituales de código de modificación automática están cubiertos por el comportamiento predeterminado del emulador. Al habilitar esta opción significativamente se reduce el rendimiento del código de modificación automática durante la ejecución. |
| Deshabilitar la optimización de rendimiento de página RWX | Esta optimización mejora el rendimiento del código en las páginas legibles, escribibles y ejecutables (RWX), pero pueden ser incompatibles con algunas aplicaciones. |

También puedes seleccionar la configuración de varios núcleos, como se muestra aquí.

![Captura de pantalla de configuración de varios núcleos](images/arm/Capture3.png)

Estos ajustes cambian el número de barreras de memoria utilizadas para sincronizar los accesos a la memoria entre núcleos en aplicaciones durante la emulación. **Rápida** es el modo predeterminado, pero las opciones **estricta** y **muy estricta** aumentarán el número de barreras. Esto ralentiza la aplicación, pero reduce el riesgo de errores de la aplicación. La opción **Núcleo único** elimina todas las barreras pero hace que todos los subprocesos de la aplicación se ejecuten en un solo núcleo.

Si cambiar un ajuste específico resuelve el problema, envía un correo electrónico a *woafeedback@microsoft.com* con detalles para poder incorporar tus comentarios.