---
title: Desencadenar una tarea en segundo plano desde dentro de la aplicación
description: Describe cómo desencadenar una tarea en segundo plano desde dentro de una aplicación
ms.date: 07/06/2018
ms.topic: article
keywords: desencadenador de tarea en segundo plano, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 3d8917c6ed181607459d6126aa295d270cfea838
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259402"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Desencadenar una tarea en segundo plano desde dentro de la aplicación

Aprende a usar el [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para activar una tarea en segundo plano desde dentro de la aplicación.

Para obtener un ejemplo de cómo crear un desencadenador de aplicación, vea este [ejemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

En este tema suponemos que tienes una tarea en segundo plano que quieres activar desde la aplicación. Si aún no tienes una tarea en segundo plano, hay una tarea en segundo plano de muestra en [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, sigue los pasos de [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md) para crear una.

## <a name="why-use-an-application-trigger"></a>¿Por qué usar un desencadenador de aplicaciones

Usa un **ApplicationTrigger** para ejecutar código en un proceso independiente de la aplicación en primer plano. Un **ApplicationTrigger** es adecuado si la aplicación tiene trabajo que debe realizarse en segundo plano, incluso si el usuario cierra la aplicación en primer plano. Si se debe detener el trabajo en segundo plano cuando se cierra la aplicación o debe estar vinculado con el estado del proceso en primer plano, en su lugar se debe usar [Ejecución ampliada](run-minimized-with-extended-execution.md).

## <a name="create-an-application-trigger"></a>Crear un desencadenador de aplicaciones

Crea un nuevo [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Puedes almacenarlo en un campo como se hace en el fragmento de código siguiente. Esto es así por comodidad, por lo que no tenemos que crear una nueva instancia más adelante cuando queremos señalar el desencadenador. Pero puedes usar cualquier instancia de **ApplicationTrigger** para indicar el desencadenador.

```csharp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
_AppTrigger = new ApplicationTrigger();
```

```cppwinrt
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
Windows::ApplicationModel::Background::ApplicationTrigger _AppTrigger;
```

```cpp
// _AppTrigger is an ApplicationTrigger field defined at a scope that will keep it alive
// as long as you need to trigger the background task.
// Or, you could create a new ApplicationTrigger instance and use that when you want to
// trigger the background task.
ApplicationTrigger ^ _AppTrigger = ref new ApplicationTrigger();
```

## <a name="optional-add-a-condition"></a>(Opcional) Agregar una condición

Puedes crear una condición de la tarea en segundo plano para controlar cuándo debe ejecutarse la tarea. Una condición evita que se ejecute la tarea en segundo plano hasta que se cumpla la condición. Para obtener más información, consulta [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo, la condición se establece en **InternetAvailable** para que, una vez desencadenada, la tarea solo se ejecute una vez que el acceso a Internet esté disponible. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable)
```

Para obtener información más detallada sobre las condiciones y los tipos de desencadenadores en segundo plano, consulte [compatibilidad con la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

Antes de registrar la tarea en segundo plano **ApplicationTrigger**, llama a [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar el nivel de actividad en segundo plano que el usuario permite porque el usuario puede haber deshabilitado la actividad en segundo plano para tu aplicación. Consulta [Optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información sobre las formas en que los usuarios pueden controlar la configuración para la actividad en segundo plano.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano y ver la definición del método **RegisterBackgroundTask ()** en el código de ejemplo siguiente, vea [registrar una tarea en segundo plano](register-a-background-task.md).

Si está pensando en utilizar un desencadenador de aplicación para ampliar la duración del proceso en primer plano, considere la posibilidad de usar la [ejecución extendida](run-minimized-with-extended-execution.md) en su lugar. El desencadenador de aplicaciones está diseñado para crear un proceso hospedado por separado en el que realizar trabajos. El siguiente fragmento de código registra un desencadenador en segundo plano fuera de proceso.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Example application trigger";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Example application trigger" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Example application trigger";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, appTrigger, internetCondition);
```

Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se ocupe correctamente de los escenarios en los que se produce un error en el registro de tareas en segundo plano. Si, en cambio, la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

## <a name="trigger-the-background-task"></a>Desencadenar la tarea en segundo plano

Antes de desencadenar la tarea en segundo plano, usa [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para comprobar que la tarea en segundo plano está registrada. Un buen momento para comprobar que todas las tareas en segundo plano están registradas es durante el inicio de la aplicación.

Desencadena la tarea en segundo plano llamando a [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). Cualquier **ApplicationTrigger** servirá.

Tenga en cuenta que no se puede llamar a **ApplicationTrigger. RequestAsync** desde la propia tarea en segundo plano o cuando la aplicación está en el estado de ejecución en segundo plano (consulte [ciclo de vida](app-lifecycle.md) de la aplicación para obtener más información sobre los Estados de la aplicación).
Puede devolver [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si el usuario ha establecido directivas de energía o de privacidad que impiden que la aplicación realice la actividad en segundo plano.
Además, solo AppTrigger puede ejecutarse cada vez. Si intentas ejecutar un AppTrigger mientras que otro ya se está ejecutando, la función devolverá [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Administrar recursos para la tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Consulta [Optimizar la actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información sobre las formas en que los usuarios pueden controlar la configuración para la actividad en segundo plano.  

- Memoria: optimización del uso de la energía y la memoria de la aplicación para garantizar que el sistema operativo permitirá la ejecución de su tarea en segundo plano. Usa las [API de administración de memoria](https://docs.microsoft.com/uwp/api/windows.system.memorymanager) para ver cuánta memoria está usando la tarea en segunda plano. Cuando más memoria use la tarea en segundo plano, más difícil será para el sistema operativo mantenerla en ejecución cuando otra aplicación esté en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que obtienen según el tipo de desencadenador. Las tareas en segundo plano desencadenadas por el desencadenador de la aplicación se limitan a unos 10 minutos.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de Windows 10, ya no es necesario que el usuario agregue la aplicación a la pantalla de bloqueo para poder utilizar tareas en segundo plano.

Una tarea en segundo plano solo se ejecutará con un **ApplicationTrigger** si se ha llamado a [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Ejemplo de código de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memoria cuando la aplicación pasa a segundo plano](reduce-memory-usage.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Aplazar la suspensión de aplicaciones con ejecución ampliada](run-minimized-with-extended-execution.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
