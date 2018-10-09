---
author: TylerMSFT
title: Desencadenar una tarea en segundo plano desde dentro de la aplicación
description: Describe cómo desencadenar una tarea en segundo plano desde dentro de una aplicación
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: desencadenador de tarea en segundo plano, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "4423947"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Desencadenar una tarea en segundo plano desde dentro de la aplicación

Aprende a usar el [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para activar una tarea en segundo plano desde dentro de la aplicación.

Para obtener un ejemplo de cómo crear un desencadenador de aplicación, consulta este [ejemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

En este tema se da por hecho que tienes una tarea en segundo plano que quieres activar desde la aplicación. Si aún no tienes una tarea en segundo plano, hay una tarea en segundo plano de ejemplo en [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, sigue los pasos de [crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md) para crear uno.

## <a name="why-use-an-application-trigger"></a>¿Por qué usar un desencadenador de aplicación

Usar un **ApplicationTrigger** para ejecutar código en un proceso independiente de la aplicación en primer plano. Un **ApplicationTrigger** es adecuado si la aplicación tiene trabajo que se debe hacer en segundo plano, incluso si el usuario cierra la aplicación en primer plano. Si se debe detener el trabajo en segundo plano cuando la aplicación se cierra o debe estar vinculada al estado del proceso en primer plano, a continuación, se debe usar [La ejecución extendida](run-minimized-with-extended-execution.md) , en su lugar.

## <a name="create-an-application-trigger"></a>Crear un desencadenador de aplicación

Crear un nuevo [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Se puede almacenar en un campo como se realiza en el siguiente fragmento de. Esto es para mayor comodidad para que no tenemos que crear una nueva instancia más adelante cuando queremos que señale el desencadenador. Pero puedes usar cualquier instancia **ApplicationTrigger** para indicar el desencadenador.

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

Puedes crear una condición de tarea en segundo plano al control cuando se ejecuta la tarea. Una condición evita que se ejecute hasta que se cumpla la condición de la tarea en segundo plano. Para obtener más información, consulta [establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo que la condición se establece en **InternetAvailable** , por lo que, al desencadenarse, la tarea solo se ejecuta una vez que el acceso a internet está disponible. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

Para obtener información más detallada sobre las condiciones y los tipos de desencadenadores en segundo plano, consulta la [compatibilidad con la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

Antes de registrar la tarea en segundo plano **ApplicationTrigger** , llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) para determinar el nivel de actividad en segundo plano que permite que el usuario porque es posible que el usuario ha deshabilitado la actividad en segundo plano de la aplicación. Consulta la [actividad en segundo plano de optimizar](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de maneras puede controlar la configuración de la actividad en segundo plano.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano y para ver la definición del método **RegisterBackgroundTask()** en el siguiente código de muestra, consulta [registrar una tarea en segundo plano](register-a-background-task.md).

Si estás pensando usar un desencadenador de aplicación para ampliar la duración de su proceso en primer plano, considera la posibilidad de usar la [Ejecución ampliada](run-minimized-with-extended-execution.md) en su lugar. El desencadenador de la aplicación está diseñado para la creación de un proceso hospedado por separado para que funcione. El siguiente fragmento de código, registra un desencadenador de fuera de proceso en segundo plano.

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

Antes de desencadenar la tarea en segundo plano, usa [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para comprobar que se registra la tarea en segundo plano. Es un buen momento para comprobar que todas las tareas en segundo plano se registran durante el inicio de la aplicación.

Desencadenar la tarea en segundo plano mediante una llamada a [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). Cualquier instancia **ApplicationTrigger** lo hará.

Ten en cuenta que no se puede llamar **ApplicationTrigger.RequestAsync** desde la propia tarea en segundo plano, o cuando la aplicación está en segundo plano estado de ejecución (consulta el [ciclo de vida de aplicación](app-lifecycle.md) para obtener más información acerca de los Estados de la aplicación).
Si el usuario ha establecido directivas de privacidad o de energía que la aplicación de realizar la actividad en segundo plano que puede devolver [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) .
Además, puede ejecutar solo AppTrigger a la vez. Si intentas ejecutar una AppTrigger mientras otro ya se está ejecutando, la función devolverá [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Administrar los recursos para la tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Consulta la [actividad en segundo plano de optimizar](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de maneras puede controlar la configuración de la actividad en segundo plano.  

- Memoria: Ajuste de uso de memoria y energía de la aplicación es esencial para garantizar que el sistema operativo permitirá que ejecute la tarea en segundo plano. Usa las [API de administración de memoria](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) para ver cuánta memoria está usando la tarea en segundo plano. Más memoria use la tarea en segundo plano, más difícil será para que el sistema operativo que siga ejecutándose cuando otra aplicación está en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que obtienen según el tipo de desencadenador. Tareas en segundo plano desencadenadas por el desencadenador de aplicación están limitadas a 10 minutos aproximadamente.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de Windows 10, ya no es necesario para que el usuario agregue tu aplicación a la pantalla de bloqueo para poder usar tareas en segundo plano.

Una tarea en segundo plano solo se ejecutará mediante un **ApplicationTrigger** si has llamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Muestra de código de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso.](create-and-register-a-background-task.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Liberar memoria cuando la aplicación pase a segundo plano](reduce-memory-usage.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Aplazar la suspensión de la aplicación con ejecución ampliada](run-minimized-with-extended-execution.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
