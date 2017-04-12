---
author: PatrickFarley
ms.assetid: 24351dad-2ee3-462a-ae78-2752bb3374c2
title: "Aprovechamiento de las características de ahorro de batería"
description: "Crea aplicaciones para UWP que colaboren con el sistema para usar tareas en segundo plano con un consumo eficiente de la batería."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.openlocfilehash: 045dfeb4696a4854b114d88da2a2cbb75d621a58
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="optimize-background-activity"></a>Optimizar la actividad en segundo plano

Las aplicaciones universales de Windows deben tener un buen rendimiento en todas las familias de dispositivos por igual. En los dispositivos alimentados por batería, el consumo de energía es un factor determinante en la experiencia global del usuario de la aplicación. Una duración de la batería de todo el día es una característica deseable para todos los usuarios, pero requiere la eficiencia de todo el software instalado en el dispositivo, incluido el tuyo. 

El comportamiento de las tareas en segundo plano es posiblemente el factor más determinante en el gasto total de energía de una aplicación. Una tarea en segundo plano es cualquier actividad del programa que se haya registrado en el sistema para ejecutarse sin que la aplicación esté abierta. Consulta [Crear y registrar una tarea en segundo plano fuera del proceso](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) para obtener más información.

## <a name="background-activity-allowance"></a>Permiso para la actividad en segundo plano

En Windows10, versión 1607, los usuarios pueden ver el "Uso de batería por aplicación" en la sección **Batería** de la aplicación Configuración. Allí verán una lista de aplicaciones y el porcentaje de la duración de la batería (de la cantidad de duración de la batería que se ha gastado desde la última carga) que cada aplicación ha consumido. 

![uso de batería por aplicación](images/battery-usage-by-app.png)

En el caso de las aplicaciones para UWP de esta lista, los usuarios tienen cierto control sobre la manera en que el sistema trata su actividad en segundo plano. La actividad en segundo plano se puede especificar como "Siempre permitida", "Administrada por Windows" (valor predeterminado) o "Nunca permitida" (ofrecemos más detalles sobre esto más adelante). Usa el valor de enumeración **BackgroundAccessStatus** devuelto desde el método [**BackgroundExecutionManager.RequestAccessAsync()**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync.aspx) para ver el permiso para la actividad en segundo plano de tu aplicación.

![permisos para tareas en segundo plano](images/background-task-permissions.png)

Con todo esto queremos decir que si la aplicación no implementa una administración responsable de la actividad en segundo plano, el usuario puede denegar todos los permisos para segundo plano a tu aplicación, que no es lo más conveniente para ninguna de las partes.

## <a name="work-with-the-battery-saver-feature"></a>Trabajar con la característica de ahorro de batería
El ahorro de batería es una característica de nivel del sistema que los usuarios pueden configurar en Configuración. Corta toda la actividad en segundo plano de todas las aplicaciones cuando el nivel de la batería desciende por debajo de un umbral definido por el usuario, *excepto* en el caso de la actividad en segundo plano de las aplicaciones que se hayan establecido en "Siempre permitida".

Si la aplicación está marcada como "Administrada por Windows" y llamada a **BackgroundExecutionManager.RequestAccessAsync()** para registrar una actividad en segundo plano mientras el ahorro de batería está activado, devolverá un valor de **DeniedSubjectToSystemPolicy**. Para gestionar esto, la aplicación debería notificar al usuario que las tareas en segundo plano indicadas no se ejecutarán hasta que el ahorro de batería se desactive y se vuelvan a registrar en el sistema. Si ya se ha registrado una tarea en segundo plano para ejecutarse y el ahorro de batería está activado en el momento de su activación, dicha tarea no se ejecutará y el usuario no recibirá ninguna notificación. Para reducir la probabilidad de que esto ocurra, es recomendable programar la aplicación para volver a registrar sus tareas en segundo plano cada vez que se abra.

Aunque la administración de la actividad en segundo plano es el propósito principal de la característica de ahorro de batería, la aplicación puede realizar ajustes adicionales para ahorrar más energía cuando el ahorro de batería está activado. Compruebe el estado del modo de ahorro de batería desde dentro de la aplicación haciendo referencia a la propiedad [**PowerManager.PowerSavingMode**](https://msdn.microsoft.com/library/windows/apps/windows.phone.system.power.powermanager.powersavingmode.aspx). Se trata de un valor de enumeración: **PowerSavingMode.Off** o **PowerSavingMode.On**. En caso de que el ahorro de batería esté activado, la aplicación podría reducir el uso de animaciones, dejar de sondear las ubicaciones o retrasar las sincronizaciones y las copias de seguridad. 

## <a name="further-optimize-background-tasks"></a>Optimizar aún más las tareas en segundo plano
Los siguientes son pasos adicionales que puede realizar al registrar las tareas en segundo plano para que gasten menos batería.

Usar un desencadenador de mantenimiento. Puede usarse un objeto [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.maintenancetrigger.aspx) en lugar de un objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.systemtrigger.aspx) para determinar cuándo se inicia una tarea en segundo plano. Las tareas que usan desencadenadores de mantenimiento solo se ejecutan cuando el dispositivo está conectado a la corriente alterna y se permite que se ejecuten durante más tiempo. Consulta [Usar un desencadenador de mantenimiento](https://msdn.microsoft.com/windows/uwp/launch-resume/use-a-maintenance-trigger) para obtener instrucciones.

Usa el tipo de condición del sistema **BackgroundWorkCostNotHigh**. Deben cumplirse las condiciones del sistema para ejecutar tareas en segundo plano (consulta [Establecer condiciones para ejecutar una tarea en segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/set-conditions-for-running-a-background-task) para obtener más información). El coste del trabajo en segundo plano es una medida que denota el impacto *relativo* sobre la energía de ejecutar la tarea en segundo plano. Una tarea que se ejecuta cuando el dispositivo está conectado a la corriente alterna se marca como **bajo** (poco o ningún impacto en la batería). Una tarea que se ejecuta cuando el dispositivo funciona con batería y con la pantalla desactivada se marca como **alto**, porque probablemente haya poca actividad de programas ejecutándose en el dispositivo en el momento, por lo que la tarea en segundo plano tendría un mayor coste relativo. Una tarea que se ejecuta cuando el dispositivo funciona con batería y con la pantalla *activada* se marca como **medio**, ya que probablemente ya exista alguna actividad de programas en ejecución y la tarea en segundo sumaría un poco más al coste de la energía. La condición del sistema **BackgroundWorkCostNotHigh** simplemente retrasa la capacidad de ejecución de la tarea hasta que la pantalla se active o el dispositivo esté conectado a la corriente alterna.

## <a name="test-battery-efficiency"></a>Probar la eficiencia de la batería

Asegúrate de probar tu aplicación en dispositivos reales para los escenarios de alto consumo de energía. Es una buena idea probar la aplicación en diferentes dispositivos, con el ahorro de batería activado y desactivado, así como en entornos con distintas intensidades de red.

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano fuera del proceso](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)  
* [Planificación del rendimiento](https://msdn.microsoft.com/windows/uwp/debug-test-perf/planning-and-measuring-performance)  

