---
title: Solucionador de problemas de compatibilidad de programas en ARM
description: Instrucciones para ajustar la configuración de compatibilidad si la aplicación no funciona correctamente en ARM
ms.date: 02/15/2018
ms.topic: article
keywords: Windows 10 s, siempre conectado, solucionador de problemas de compatibilidad, Windows en ARM
ms.localizationpriority: medium
ms.openlocfilehash: 24ae6e7c12fde1dfbb9e5395b3fa3aa4901adb3d
ms.sourcegitcommit: 53c00939b20d4b0a294936df3d395adb0c13e231
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2020
ms.locfileid: "91933026"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Solucionador de problemas de compatibilidad de programas en ARM
La emulación para admitir aplicaciones x86 es una característica nueva creada para Windows 10 en ARM64. A veces, la emulación realiza optimizaciones que no dan como resultado la mejor experiencia. Puede usar el solucionador de problemas de compatibilidad de programas para alternar la configuración de emulación de la aplicación x86, lo que reduce las optimizaciones predeterminadas y puede aumentar la compatibilidad.

## <a name="start-the-program-compatibility-troubleshooter"></a>Iniciar el solucionador de problemas de compatibilidad de programas
El [solucionador de problemas de compatibilidad de programas](https://support.microsoft.com/help/15078/windows-make-older-programs-compatible) se inicia manualmente de la misma manera en cualquier equipo con Windows 10: haga clic con el botón secundario en un archivo ejecutable (. exe) y seleccione **solucionar problemas de compatibilidad**. Aparece esta pantalla.

![Captura de pantalla de las opciones de compatibilidad de solución de problemas.](images/arm/Capture4.png)

Si hace clic en **solucionar problemas de programa** , se le presentarán las siguientes opciones.

![Captura de pantalla de los problemas que se observan en las opciones.](images/arm/Capture5.png)

Todas las opciones permiten configurar las opciones que son aplicables y se aplican en todos los equipos de escritorio de Windows 10. Además, las opciones primera, segunda y cuarta aplican la configuración de emulación [deshabilitar la caché](#disable-app-cache) de la aplicación y [deshabilitar el modo de ejecución híbrida](#disable-hybrid-exec-mode) .

## <a name="toggling-emulation-settings"></a>Alternar la configuración de emulación
> [!WARNING]
> Cambiar la configuración de emulación puede dar lugar a que la aplicación se bloquee de forma inesperada o no se inicie.

Para alternar la configuración de emulación, haga clic con el botón secundario en el archivo ejecutable y seleccione **propiedades**.

En ARM, en la pestaña **compatibilidad** estará disponible una sección titulada **Windows 10 en ARM** . Haga clic en **Cambiar configuración de emulación** para iniciar una segunda ventana como se muestra aquí.

![Captura de pantalla cambiar configuración de emulación](images/arm/Capture.png)

En esta ventana se proporcionan dos maneras de modificar la configuración de emulación. Puede seleccionar un grupo predefinido de configuración de emulación, o puede hacer clic en la opción **Usar configuración avanzada** para habilitar la elección de la configuración individual.

La configuración de emulación agrupada reduce las optimizaciones de rendimiento en favor de la calidad. A continuación se muestran algunos valores de configuración agrupados que puede seleccionar.

![Cambiar la configuración de emulación screenshot2](images/arm/Capture2.png)

Seleccione **Usar configuración avanzada** para elegir la configuración individual tal y como se describe en esta tabla.

| Configuración de emulación | Resultado |
| ----------------- | ----------- |
| <p id="disable-app-cache">Deshabilitar caché de aplicaciones</p> | El sistema operativo almacenará en caché los bloques de código compilados para reducir la sobrecarga de emulación en las ejecuciones posteriores. Esta configuración requiere que el emulador vuelva a compilar todo el código de aplicación en tiempo de ejecución. |
| <p id="disable-hybrid-exec-mode">Deshabilitar el modo de ejecución híbrida</p> | Archivo ejecutable híbrido híbrido compilado (CHPE), los binarios son archivos binarios compatibles con x86 que incluyen código ARM64 nativo para mejorar el rendimiento, pero es posible que no sean compatibles con algunas aplicaciones. Esta configuración fuerza el uso de archivos binarios solo de x86. |
| Compatibilidad estricta con el código de modificación automática | Habilite esta configuración para asegurarse de que cualquier código automodificable se admita correctamente en la emulación. Los escenarios de código que se modifican de forma automática más comunes están incluidos en el comportamiento predeterminado del emulador. Al habilitar esta opción, se reduce significativamente el rendimiento del código automodificable durante la ejecución. |
| Deshabilitar la optimización del rendimiento de la página RWX | Esta optimización mejora el rendimiento del código en páginas de lectura, escritura y ejecución (RWX), pero puede ser incompatible con algunas aplicaciones. |

También puede seleccionar la configuración de varios núcleos, como se muestra aquí.

![Captura de pantalla de configuración de varios núcleos](images/arm/Capture3.png)

Esta configuración cambia el número de barreras de memoria que se usan para sincronizar los accesos de memoria entre los núcleos de las aplicaciones durante la emulación. **Fast** es el modo predeterminado, pero las **strict** opciones STRICT **y STRICT aumentan** el número de barreras. Esto ralentiza la aplicación, pero reduce el riesgo de errores de la aplicación. La opción **de un solo núcleo** quita todas las barreras pero obliga a que todos los subprocesos de la aplicación se ejecuten en un solo núcleo.

Si al cambiar una configuración específica se resuelve el problema, envíe un correo electrónico *woafeedback@microsoft.com* con los detalles para que podamos incorporar sus comentarios.