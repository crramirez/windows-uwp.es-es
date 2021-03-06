---
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: Optimización de la actividad en segundo plano
description: Crea aplicaciones para UWP que colaboren con el sistema para usar tareas en segundo plano de una manera que produzca un consumo eficiente de la batería.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5da6dcced0dfa563b5baf69d2b2cb1f64843af1b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173579"
---
# <a name="optimize-background-activity"></a>Optimización de la actividad en segundo plano

Las aplicaciones universales de Windows deben tener un buen rendimiento en todas las familias de dispositivos por igual. En los dispositivos alimentados por batería, el consumo de energía es un factor determinante en la experiencia global del usuario de la aplicación. Una duración de la batería de todo el día es una característica deseable para todos los usuarios, pero requiere la eficiencia de todo el software instalado en el dispositivo, incluido el tuyo. 

El comportamiento de las tareas en segundo plano es posiblemente el factor más importante en el gasto total de energía de una aplicación. Una tarea en segundo plano es cualquier actividad del programa que se haya registrado en el sistema para ejecutarse sin que la aplicación esté abierta. Para más información, consulta [Crear y registrar una tarea en segundo plano fuera de proceso](../launch-resume/create-and-register-a-background-task.md).

## <a name="background-activity-permissions"></a>Permisos de actividad en segundo plano

En dispositivos de escritorio y móviles que ejecutan Windows 10, versión 1607 o versiones posteriores, los usuarios puede ver el uso de batería por aplicación en la sección Batería de la aplicación Configuración. Allí verán una lista de aplicaciones y el porcentaje de la duración de la batería (de la cantidad de duración de la batería que se ha gastado desde la última carga) que cada aplicación ha consumido. En el caso de las aplicaciones para UWP de esta lista, los usuarios pueden seleccionar la aplicación para abrir los controles relacionados con la actividad en segundo plano.

![uso de batería por aplicación](images/battery-usage-by-app.png)

### <a name="background-permissions-on-mobile"></a>Permisos en segundo plano en el móvil

En los dispositivos móviles, los usuarios verán una lista de los botones de radio que especifican la configuración de permisos de la tarea en segundo plano para esa aplicación. La actividad en segundo plano puede establecerse en "Siempre permitido", "Nunca permitido" o "Administrado por Windows", lo que significa la que actividad en segundo plano de la aplicación la regula el sistema según una serie de factores. 

![Botones de radio de permisos de tareas en segundo plano](images/background-task-permissions.png)

### <a name="background-permissions-on-desktop"></a>Permisos de segundo plano en el escritorio

En dispositivos de escritorio, la opción configuración "Administrado por Windows" se presenta como un modificador para alternar, establecido en **Activado** de manera predeterminada. Si el usuario cambia a **Desactivado**, se presenta una casilla con la que puede definir manualmente los permisos de la actividad en segundo plano. Cuando se activa la casilla, la aplicación podrá ejecutar tareas en segundo plano en todo momento. Cuando se desactiva la casilla, la actividad en segundo plano se deshabilitará.

![activar permisos de tareas en segundo plano](images/background-task-permissions-on.png)

![desactivar permisos de tareas en segundo plano](images/background-task-permissions-off.png)

En la aplicación, puedes usar el valor de enumeración [**BackgroundAccessStatus**](/uwp/api/windows.applicationmodel.background.backgroundaccessstatus) que devuelve una llamada al método [**BackgroundExecutionManager.RequestAccessAsync()** ](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar su configuración actual de permisos de actividad en segundo plano.

Con todo esto queremos decir que si la aplicación no implementa una administración responsable de la actividad en segundo plano, el usuario puede denegar todos los permisos para segundo plano a tu aplicación, que no es lo más conveniente para ninguna de las partes. Si a la aplicación se le ha denegado permiso para ejecutarse en segundo plano, pero requiere actividad en segundo plano para completar una acción del usuario, puedes notificar al usuario y dirigirlo a la aplicación Configuración. Para ello, [inicia la aplicación Configuración](../launch-resume/launch-settings-app.md) en la página de detalles de uso de la batería o de aplicaciones en segundo plano.

## <a name="work-with-the-battery-saver-feature"></a>Trabajar con la característica de ahorro de batería
El ahorro de batería es una característica de nivel del sistema que los usuarios pueden configurar en Configuración. Corta toda la actividad en segundo plano de todas las aplicaciones cuando el nivel de la batería desciende por debajo de un umbral definido por el usuario, *excepto* en el caso de la actividad en segundo plano de las aplicaciones que se hayan establecido en "Siempre permitida".

Comprueba el estado del modo de ahorro de batería desde dentro de la aplicación haciendo referencia a la propiedad [**PowerManager.EnergySaverStatus**](/uwp/api/windows.system.power.energysaverstatus). Se trata de un valor de enumeración: **EnergySaverStatus.Disabled**, **EnergySaverStatus.Off** o **EnergySaverStatus.On**. Si la aplicación requiere actividad en segundo plano y no se establece en "Siempre permitido", debería controlar **EnergySaverStatus.On** al notificar al usuario que las tareas en segundo plano dadas no se ejecutarán hasta que se desactive el modo de ahorro de batería. Aunque la administración de la actividad en segundo plano es el propósito principal de la característica de ahorro de batería, la aplicación puede realizar ajustes adicionales para ahorrar más energía cuando el ahorro de batería está activado.  En caso de que el ahorro de batería esté activado, la aplicación podría reducir el uso de animaciones, dejar de sondear las ubicaciones o retrasar las sincronizaciones y las copias de seguridad. 

## <a name="further-optimize-background-tasks"></a>Optimizar aún más las tareas en segundo plano
Los siguientes son pasos adicionales que puede realizar al registrar las tareas en segundo plano para que gasten menos batería.

### <a name="use-a-maintenance-trigger"></a>Usar un desencadenador de mantenimiento 
Puede usarse un objeto [**MaintenanceTrigger**](/uwp/api/windows.applicationmodel.background.maintenancetrigger) en lugar de un objeto [**SystemTrigger**](/uwp/api/windows.applicationmodel.background.systemtrigger) para determinar cuándo se inicia una tarea en segundo plano. Las tareas que usan desencadenadores de mantenimiento solo se ejecutan cuando el dispositivo está conectado a la corriente alterna y se permite que se ejecuten durante más tiempo. Consulta [Usar un desencadenador de mantenimiento](../launch-resume/use-a-maintenance-trigger.md) para obtener instrucciones.

### <a name="use-the-backgroundworkcostnothigh-system-condition-type"></a>Usa el tipo de condición del sistema **BackgroundWorkCostNotHigh**.
Deben cumplirse las condiciones del sistema para ejecutar tareas en segundo plano (consulta [Establecer condiciones para ejecutar una tarea en segundo plano](../launch-resume/set-conditions-for-running-a-background-task.md) para obtener más información). El coste del trabajo en segundo plano es una medida que denota el impacto *relativo* sobre la energía de ejecutar la tarea en segundo plano. Una tarea que se ejecuta cuando el dispositivo está conectado a la corriente alterna se marca como **bajo** (poco o ningún impacto en la batería). Una tarea que se ejecuta cuando el dispositivo funciona con batería y con la pantalla desactivada se marca como **alto**, porque probablemente haya poca actividad de programas ejecutándose en el dispositivo en el momento, por lo que la tarea en segundo plano tendría un mayor coste relativo. Una tarea que se ejecuta cuando el dispositivo funciona con batería y con la pantalla *activada* se marca como **medio**, ya que probablemente ya exista alguna actividad de programas en ejecución y la tarea en segundo sumaría un poco más al coste de la energía. La condición del sistema **BackgroundWorkCostNotHigh** simplemente retrasa la capacidad de ejecución de la tarea hasta que la pantalla se active o el dispositivo esté conectado a la corriente alterna.

## <a name="test-battery-efficiency"></a>Probar la eficiencia de la batería

Asegúrate de probar tu aplicación en dispositivos reales para los escenarios de alto consumo de energía. Es una buena idea probar la aplicación en diferentes dispositivos, con el ahorro de batería activado y desactivado, así como en entornos con distintas intensidades de red.

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano fuera del proceso](../launch-resume/create-and-register-a-background-task.md)  
* [Planificación del rendimiento](./planning-and-measuring-performance.md)