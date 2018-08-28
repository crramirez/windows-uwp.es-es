---
author: TylerMSFT
title: Ejecutar una tarea en segundo plano en un temporizador
description: Aprende a programar una tarea en segundo plano única o a ejecutar una tarea en segundo plano periódica.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, tareas en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 25e3c76ae09ed6835f89f0d98c308f11c7a99624
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "2890764"
---
# <a name="run-a-background-task-on-a-timer"></a>Ejecutar una tarea en segundo plano en un temporizador

Obtenga información sobre cómo usar el [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843) para programar una tarea en segundo plano única o ejecutar una tarea de periódico en segundo plano.

Vea **Scenario4** en el [ejemplo de activación de fondo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) para ver un ejemplo de cómo implementar el tiempo de tarea en segundo plano desencadenadas que se describe en este tema.

En este tema se da por supuesto que tiene una tarea en segundo plano que necesita para ejecutarse periódicamente o a una hora específica. Si aún no tiene una tarea en segundo plano, hay una tarea de segundo plano de ejemplo en [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, siga los pasos descritos en [crear y registrar una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md) o [crear y registrar una tarea en segundo plano de fuera del proceso](create-and-register-a-background-task.md) para crear uno.

## <a name="create-a-time-trigger"></a>Crear un desencadenador de hora

Crea un nuevo [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843). El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará solo una vez o seguirá ejecutándose periódicamente. Si *OneShot* se establece en True, el primer parámetro (*FreshnessTime*) especifica el número de minutos que deben esperarse antes de programar la tarea en segundo plano. Si el elemento *OneShot* se establece en False, el elemento *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

El temporizador integrado de las aplicaciones para la Plataforma universal de Windows (UWP) destinadas a la familia de dispositivos de escritorio o móviles ejecuta tareas en segundo plano en intervalos de 15minutos. (El temporizador se ejecuta en intervalos de 15 minutos para que el sistema sólo necesita reactivar una vez cada 15 minutos para las aplicaciones que han solicitado TimerTriggers--que ahorra energía de reactivación.)

- Si *FreshnessTime* se establece en 15minutos y *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 15 y 30minutos después de que se registre. Si se establece a 25 minutos y el elemento *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 25 y 40minutos después de que se registre.

- Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada 15 minutos comenzando entre el minuto 15 y el 30 desde el momento en que se registre. Si se ha establecido en n minutos y el elemento *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada n minutos comenzando entre el minuto n y n + 15 después de registrarse.

> [!NOTE]
> Si *FreshnessTime* está establecida en menos de 15 minutos, se produce una excepción cuando se intenta registrar la tarea en segundo plano.
 
Por ejemplo, este desencadenador hará que una tarea en segundo plano para que se ejecute una vez cada hora.

```cs
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);
```

```cppwinrt
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };
```

```cpp
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);
```

## <a name="optional-add-a-condition"></a>(Opcional) Agregar una condición

Puede crear una condición de tarea de fondo para el control cuando se ejecuta la tarea. Una condición impide que se ejecuten hasta que se cumpla la condición de la tarea en segundo plano. Para obtener más información, vea [establecer las condiciones para la ejecución de una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo, la condición se establece en **UserPresent** de forma que, al desencadenarse, la tarea solo se ejecuta cuando el usuario está activo. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

```cs
SystemCondition userCondition = new SystemCondition(SystemConditionType.UserPresent);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
```

```cpp
SystemCondition ^ userCondition = ref new SystemCondition(SystemConditionType::UserPresent);
```

Para obtener información más detallada en las condiciones y los tipos de desencadenadores de fondo, vea [compatibilidad con la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

Antes de registrar la tarea en segundo plano **ApplicationTrigger** , llame a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) para determinar el nivel de actividad en segundo plano permite que el usuario debido a que es posible que el usuario haya deshabilitado actividad en segundo plano para su aplicación. Vea [optimizar actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de formas puede controlar la configuración de la actividad en segundo plano.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano y para ver la definición del método **RegisterBackgroundTask()** en el siguiente ejemplo de código, vea [registrar una tarea en segundo plano](register-a-background-task.md).

> [!IMPORTANT]
> Para tareas que se ejecutan en el mismo proceso que la aplicación de fondo, no establezca `entryPoint`. Para tareas de fondo que se ejecutan en un proceso independiente de la aplicación, establezca `entryPoint` a ser el espacio de nombres, '.' y el nombre de la clase que contiene la implementación de la tarea de fondo.

```cs
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example hourly background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example hourly background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example hourly background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, hourlyTrigger, userCondition);
```

Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se ocupe correctamente de los escenarios en los que se produce un error en el registro de tareas en segundo plano. Si, en cambio, la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

## <a name="manage-resources-for-your-background-task"></a>Administrar los recursos para la tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Vea [optimizar actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de formas puede controlar la configuración de la actividad en segundo plano.

- Memoria: Ajuste de uso de memoria y energía de su aplicación es clave para garantizar que el sistema operativo permitirá la tarea para que se ejecute en segundo plano. Use las [API de administración de memoria](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) para ver la cantidad de memoria está usando la tarea en segundo plano. Usa la tarea en segundo plano de más memoria, más difícil es para que el sistema operativo que siga ejecutándose cuando otra aplicación está en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj de pared que obtengan basados en tipo de desencadenador.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de 10 de Windows, ya no es necesario para el usuario agregar la aplicación a la pantalla de bloqueo para poder utilizar tareas en segundo plano.

Una tarea en segundo plano sólo se ejecutará con un **TimeTrigger** si ha llamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Ejemplo de código de tarea de fondo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md)
* [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memoria cuando la aplicación pasa a segundo plano](reduce-memory-usage.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Aplazar la suspensión de la aplicación con ejecución ampliada](run-minimized-with-extended-execution.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
