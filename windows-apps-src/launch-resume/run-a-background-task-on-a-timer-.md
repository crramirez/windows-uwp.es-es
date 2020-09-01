---
title: Ejecutar una tarea en segundo plano en un temporizador
description: Aprende a programar una tarea en segundo plano única o a ejecutar una tarea en segundo plano periódica.
ms.assetid: 0B7F0BFF-535A-471E-AC87-783C740A61E9
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 7f9bf2ad18402976a7e089c2b2273e473819af1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175139"
---
# <a name="run-a-background-task-on-a-timer"></a>Ejecutar una tarea en segundo plano en un temporizador

Obtenga información sobre cómo usar [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger) para programar una tarea en segundo plano de una sola vez o ejecutar una tarea en segundo plano periódica.

Consulte **Scenario4** en el [ejemplo de activación en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundActivation) para ver un ejemplo de cómo implementar la tarea en segundo plano desencadenada que se describe en este tema.

En este tema se supone que tiene una tarea en segundo plano que debe ejecutarse periódicamente o en un momento determinado. Si aún no tiene una tarea en segundo plano, hay una tarea en segundo plano de ejemplo en [BackgroundActivity.CS](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, siga los pasos descritos en [crear y registrar una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md) o [crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md) para crear una.

## <a name="create-a-time-trigger"></a>Crear un desencadenador de hora

Crea un nuevo [**TimeTrigger**](/uwp/api/Windows.ApplicationModel.Background.TimeTrigger). El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará solo una vez o seguirá ejecutándose periódicamente. Si *OneShot* se establece en "true", el primer parámetro (*FreshnessTime*) especifica el número de minutos que se debe esperar antes de programar la tarea en segundo plano. Si el elemento *OneShot* se establece en False, el elemento *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

El temporizador integrado de las aplicaciones para la Plataforma universal de Windows (UWP) destinadas a la familia de dispositivos de escritorio o móviles ejecuta tareas en segundo plano en intervalos de 15 minutos. (El temporizador se ejecuta en intervalos de 15 minutos, de modo que el sistema solo necesita reactivarse una vez cada 15 minutos para reactivar las aplicaciones que han solicitado TimerTriggers, lo que ahorra energía).

- Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 15 y 30 minutos después de que se registre. Si se establece a 25 minutos y el elemento *OneShot* tiene el valor True, la tarea se programará para ejecutarse una vez entre 25 y 40 minutos después de que se registre.

- Si *FreshnessTime* se establece en 15 minutos y *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada 15 minutos comenzando entre el minuto 15 y el 30 desde el momento en que se registre. Si se ha establecido en n minutos y el elemento *OneShot* tiene el valor False, la tarea se programará para ejecutarse cada n minutos comenzando entre el minuto n y n + 15 después de registrarse.

> [!NOTE]
> Si *FreshnessTime* se establece en menos de 15 minutos, se produce una excepción al intentar registrar la tarea en segundo plano.

Por ejemplo, este desencadenador hará que una tarea en segundo plano se ejecute una vez por hora.

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

Puede crear una condición de tarea en segundo plano para controlar el momento en que se ejecuta la tarea. Una condición impide que la tarea en segundo plano se ejecute hasta que se cumpla la condición. Para obtener más información, vea [establecer las condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo, la condición se establece en **UserPresent** de forma que, al desencadenarse, la tarea solo se ejecuta cuando el usuario está activo. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

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

Para obtener información más detallada sobre las condiciones y los tipos de desencadenadores en segundo plano, consulte [compatibilidad con la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

Antes de registrar la tarea en segundo plano **ApplicationTrigger** , llame a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar el nivel de actividad en segundo plano que el usuario permite porque el usuario puede haber deshabilitado la actividad en segundo plano de la aplicación. Consulte [optimizar la actividad en segundo plano](../debug-test-perf/optimize-background-activity.md) para obtener más información acerca de las formas en que los usuarios pueden controlar la configuración de la actividad en segundo plano.

```cs
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
    // Depending on the value of requestStatus, provide an appropriate response
    // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano y ver la definición del método **RegisterBackgroundTask ()** en el código de ejemplo siguiente, vea [registrar una tarea en segundo plano](register-a-background-task.md).

> [!IMPORTANT]
> En el caso de las tareas en segundo plano que se ejecutan en el mismo proceso que la aplicación, no establezca `entryPoint` . Para las tareas en segundo plano que se ejecutan en un proceso independiente de la aplicación, establezca `entryPoint` como el espacio de nombres "." y el nombre de la clase que contiene la implementación de la tarea en segundo plano.

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

## <a name="manage-resources-for-your-background-task"></a>Administrar recursos para una tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Consulte [optimizar la actividad en segundo plano](../debug-test-perf/optimize-background-activity.md) para obtener más información acerca de las formas en que los usuarios pueden controlar la configuración de la actividad en segundo plano.

- Memoria: el ajuste de la memoria y el uso energético de la aplicación es clave para asegurarse de que el sistema operativo permita la ejecución de la tarea en segundo plano. Use las [API de administración de memoria](/uwp/api/windows.system.memorymanager) para ver la cantidad de memoria que está usando la tarea en segundo plano. Cuanto mayor sea la memoria que use la tarea en segundo plano, más difícil será que el sistema operativo siga ejecutándose cuando otra aplicación esté en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que se basa en el tipo de desencadenador.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de Windows 10, ya no es necesario que el usuario agregue la aplicación a la pantalla de bloqueo para poder utilizar tareas en segundo plano.

Una tarea en segundo plano solo se ejecutará con un **TimeTrigger** si se ha llamado a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Ejemplo de código de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md)
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memoria cuando la aplicación pasa a segundo plano](reduce-memory-usage.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](/previous-versions/hh974425(v=vs.110))
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Aplazar la suspensión de aplicaciones con ejecución ampliada](run-minimized-with-extended-execution.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)