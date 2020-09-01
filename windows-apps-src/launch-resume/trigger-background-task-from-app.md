---
title: Desencadenar una tarea en segundo plano desde dentro de la aplicación
description: Obtenga información sobre cómo usar un desencadenador de aplicación para ejecutar una tarea en segundo plano que quiera activar desde la aplicación.
ms.date: 07/06/2018
ms.topic: article
keywords: desencadenador de tarea en segundo plano, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 446adc9921d2d124fb6e1304a06b70fa72da3d96
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155839"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Desencadenar una tarea en segundo plano desde dentro de la aplicación

Aprende a usar [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para activar una tarea en segundo plano desde dentro de la aplicación.

Para obtener un ejemplo de cómo crear un desencadenador de aplicación, vea este [ejemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

En este tema se supone que tiene una tarea en segundo plano que desea activar desde la aplicación. Si aún no tiene una tarea en segundo plano, hay una tarea en segundo plano de ejemplo en [BackgroundActivity.CS](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, siga los pasos descritos en [creación y registro de una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md) para crear una.

## <a name="why-use-an-application-trigger"></a>Por qué usar un desencadenador de aplicación

Use un **ApplicationTrigger** para ejecutar código en un proceso independiente de la aplicación en primer plano. Una **ApplicationTrigger** es adecuada si la aplicación tiene trabajo que debe realizarse en segundo plano, incluso si el usuario cierra la aplicación en primer plano. Si el trabajo en segundo plano debe detenerse cuando se cierra la aplicación o debe estar vinculado al estado del proceso en primer plano, se debe usar la [ejecución extendida](run-minimized-with-extended-execution.md) en su lugar.

## <a name="create-an-application-trigger"></a>Creación de un desencadenador de aplicación

Cree un nuevo [ApplicationTrigger](/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Podría almacenarlo en un campo tal y como se hace en el siguiente fragmento de código. Esto es así por comodidad, por lo que no tenemos que crear una nueva instancia más adelante cuando queremos señalar el desencadenador. Pero puede usar cualquier instancia de **ApplicationTrigger** para indicar el desencadenador.

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

Puede crear una condición de tarea en segundo plano para controlar el momento en que se ejecuta la tarea. Una condición impide que la tarea en segundo plano se ejecute hasta que se cumpla la condición. Para obtener más información, vea [establecer las condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo, la condición se establece en **InternetAvailable** para que, una vez desencadenada, la tarea solo se ejecute una vez que el acceso a Internet esté disponible. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

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

Antes de registrar la tarea en segundo plano **ApplicationTrigger** , llame a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) para determinar el nivel de actividad en segundo plano que el usuario permite porque el usuario puede haber deshabilitado la actividad en segundo plano de la aplicación. Consulte [optimizar la actividad en segundo plano](../debug-test-perf/optimize-background-activity.md) para obtener más información acerca de las formas en que los usuarios pueden controlar la configuración de la actividad en segundo plano.

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

Si está pensando en utilizar un desencadenador de aplicación para ampliar la duración del proceso en primer plano, considere la posibilidad de usar la [ejecución extendida](run-minimized-with-extended-execution.md) en su lugar. El desencadenador de aplicación está diseñado para crear un proceso hospedado de forma independiente para trabajar en. El fragmento de código siguiente registra un desencadenador en segundo plano fuera de proceso.

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

Antes de desencadenar la tarea en segundo plano, use [BackgroundTaskRegistration](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para comprobar que la tarea en segundo plano está registrada. Es un buen momento para comprobar que todas las tareas en segundo plano se registran durante el inicio de la aplicación.

Desencadenar la tarea en segundo plano mediante una llamada a [ApplicationTrigger. RequestAsync](/uwp/api/windows.applicationmodel.background.applicationtrigger). Cualquier instancia de **ApplicationTrigger** lo hará.

Tenga en cuenta que no se puede llamar a **ApplicationTrigger. RequestAsync** desde la propia tarea en segundo plano o cuando la aplicación está en el estado de ejecución en segundo plano (consulte [ciclo de vida](app-lifecycle.md) de la aplicación para obtener más información sobre los Estados de la aplicación).
Puede devolver [DisabledByPolicy](/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si el usuario ha establecido directivas de energía o de privacidad que impiden que la aplicación realice la actividad en segundo plano.
Además, solo se puede ejecutar un AppTrigger cada vez. Si intenta ejecutar un AppTrigger mientras otro ya se está ejecutando, la función devolverá [CurrentlyRunning](/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Administrar recursos para una tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Consulte [optimizar la actividad en segundo plano](../debug-test-perf/optimize-background-activity.md) para obtener más información acerca de las formas en que los usuarios pueden controlar la configuración de la actividad en segundo plano.  

- Memoria: el ajuste de la memoria y el uso energético de la aplicación es clave para asegurarse de que el sistema operativo permita la ejecución de la tarea en segundo plano. Use las [API de administración de memoria](/uwp/api/windows.system.memorymanager) para ver la cantidad de memoria que está usando la tarea en segundo plano. Cuanto mayor sea la memoria que use la tarea en segundo plano, más difícil será que el sistema operativo siga ejecutándose cuando otra aplicación esté en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: las tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj que se basa en el tipo de desencadenador. Las tareas en segundo plano desencadenadas por el desencadenador de la aplicación se limitan a unos 10 minutos.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de Windows 10, ya no es necesario que el usuario agregue la aplicación a la pantalla de bloqueo para poder utilizar tareas en segundo plano.

Una tarea en segundo plano solo se ejecutará con un **ApplicationTrigger** si se ha llamado a [**RequestAccessAsync**](/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Ejemplo de código de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Cree y registre una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md).
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