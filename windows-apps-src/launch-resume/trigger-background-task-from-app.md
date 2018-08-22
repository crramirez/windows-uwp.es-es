---
author: TylerMSFT
title: Desencadenar una tarea en segundo plano desde dentro de la aplicación
description: Describe los procedimientos para desencadenar una tarea en segundo plano desde dentro de una aplicación
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: desencadenador de tarea de fondo, tarea en segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 5ccd171f53795ef71830ffb022d0468facb3ac4f
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2800226"
---
# <a name="trigger-a-background-task-from-within-your-app"></a>Desencadenar una tarea en segundo plano desde dentro de la aplicación

Aprende a usar el [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger) para activar una tarea en segundo plano desde dentro de la aplicación.

Para obtener un ejemplo de cómo crear un desencadenador de aplicación, vea en este [ejemplo](https://github.com/Microsoft/Windows-universal-samples/blob/v2.0.0/Samples/BackgroundTask/cs/BackgroundTask/Scenario5_ApplicationTriggerTask.xaml.cs).

En este tema se da por supuesto que tiene una tarea en segundo plano que desea activar desde la aplicación. Si aún no tiene una tarea en segundo plano, hay una tarea de segundo plano de ejemplo en [BackgroundActivity.cs](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/BackgroundActivation/cs/BackgroundActivity.cs). O bien, siga los pasos descritos en [crear y registrar una tarea en segundo plano de fuera del proceso](create-and-register-a-background-task.md) para crear uno.

## <a name="why-use-an-application-trigger"></a>¿Por qué usar un desencadenador de aplicación

Use una **ApplicationTrigger** para ejecutar código en un proceso independiente desde la aplicación de primer plano. Un **ApplicationTrigger** es adecuada si la aplicación tiene trabajo que debe realizarse en segundo plano, incluso si el usuario cierra la aplicación de primer plano. Si se debe detener el trabajo de fondo cuando la aplicación está cerrada o debe estar relacionado con el estado del proceso de primer plano, a continuación, se debe usar [La ejecución ampliado](run-minimized-with-extended-execution.md) , en su lugar.

## <a name="create-an-application-trigger"></a>Crear un desencadenador de aplicación

Crear un nuevo [ApplicationTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.ApplicationTrigger). Ésta se puede almacenar en un campo tal y como se realiza en el fragmento de código que aparece a continuación. Esto es así para comodidad, para que no tenemos que crear una nueva instancia más adelante cuando queremos señalar el desencadenador. Pero se puede utilizar cualquier instancia **ApplicationTrigger** para indicar el desencadenador.

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

Puede crear una condición de tarea de fondo para el control cuando se ejecuta la tarea. Una condición impide que se ejecuten hasta que se cumpla la condición de la tarea en segundo plano. Para obtener más información, vea [establecer las condiciones para la ejecución de una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo que se establece la condición en **InternetAvailable** que, una vez activado, la tarea se ejecuta sólo una vez que el acceso a internet está disponible. Para obtener una lista de posibles condiciones, consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835).

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

Para obtener información más detallada en las condiciones y los tipos de desencadenadores de fondo, vea [compatibilidad con la aplicación con tareas en segundo plano](support-your-app-with-background-tasks.md).

##  <a name="call-requestaccessasync"></a>Llamar a RequestAccessAsync()

Antes de registrar la tarea en segundo plano **ApplicationTrigger** , llame a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700494) para determinar el nivel de actividad en segundo plano permite que el usuario debido a que es posible que el usuario haya deshabilitado actividad en segundo plano para su aplicación. Vea [optimizar actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de formas puede controlar la configuración de la actividad en segundo plano.

```csharp
var requestStatus = await Windows.ApplicationModel.Background.BackgroundExecutionManager.RequestAccessAsync();
if (requestStatus != BackgroundAccessStatus.AlwaysAllowed)
{
   // Depending on the value of requestStatus, provide an appropriate response
   // such as notifying the user which functionality won't work as expected
}
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano y para ver la definición del método **RegisterBackgroundTask()** en el siguiente ejemplo de código, vea [registrar una tarea en segundo plano](register-a-background-task.md).

Si está considerando la posibilidad de utilizar un desencadenador de aplicación para ampliar la duración del proceso de primer plano, considere el uso [Extendido de ejecución](run-minimized-with-extended-execution.md) en su lugar. El desencadenador de aplicación está diseñado para la creación de un proceso hospedado por separado para que funcionen en. El fragmento de código siguiente registra un desencadenador de fondo fuera de proceso.

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

Antes de activar la tarea en segundo plano, use [BackgroundTaskRegistration](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) para comprobar que se ha registrado la tarea en segundo plano. Es un buen momento para comprobar que todas las tareas de fondo están registradas durante el inicio de la aplicación.

Desencadenar la tarea en segundo plano mediante una llamada a [ApplicationTrigger.RequestAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtrigger). Cualquier instancia **ApplicationTrigger** hará.

Tenga en cuenta que no se puede llamar **ApplicationTrigger.RequestAsync** desde la propia tarea de fondo, o cuando la aplicación está en el estado de ejecución de fondo (vea [ciclo de vida de aplicación](app-lifecycle.md) para obtener más información acerca de Estados de la aplicación).
Puede devolver [DisabledByPolicy](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult) si el usuario ha establecido las directivas de privacidad o de energía que impiden que la aplicación realiza la actividad en segundo plano.
Además, un solo AppTrigger puede ejecutar a la vez. Si intenta ejecutar una AppTrigger mientras que otro ya se está ejecutando, la función devolverá [CurrentlyRunning](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.applicationtriggerresult).

```csharp
var result = await _AppTrigger.RequestAsync();
```

## <a name="manage-resources-for-your-background-task"></a>Administrar los recursos para la tarea en segundo plano

Usa [BackgroundExecutionManager.RequestAccessAsync](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.backgroundexecutionmanager.aspx) para determinar si el usuario ha decidido que la actividad en segundo plano de la aplicación debe ser limitada. Ten en cuenta el uso de la batería y ejecuta aplicaciones en segundo plano solo cuando sea necesario completar una acción que requiera el usuario. Vea [optimizar actividad en segundo plano](https://docs.microsoft.com/windows/uwp/debug-test-perf/optimize-background-activity) para obtener más información acerca de los usuarios de formas puede controlar la configuración de la actividad en segundo plano.  

- Memoria: Ajuste de uso de memoria y energía de su aplicación es clave para garantizar que el sistema operativo permitirá la tarea para que se ejecute en segundo plano. Use las [API de administración de memoria](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.aspx) para ver la cantidad de memoria está usando la tarea en segundo plano. Usa la tarea en segundo plano de más memoria, más difícil es para que el sistema operativo que siga ejecutándose cuando otra aplicación está en primer plano. El usuario es, en última instancia, quien controla toda la actividad en segundo plano que la aplicación puede llevar a cabo y quien tiene visibilidad sobre el impacto que la aplicación tiene sobre el uso de la batería.  
- Tiempo de CPU: tareas en segundo plano están limitadas por la cantidad de tiempo de uso de reloj de pared que obtengan basados en tipo de desencadenador. Tareas en segundo plano desencadenadas por el desencadenador de aplicación están limitadas a aproximadamente 10 minutos.

Consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md) para conocer las restricciones de recursos que se aplican a las tareas en segundo plano.

## <a name="remarks"></a>Observaciones

A partir de 10 de Windows, ya no es necesario para el usuario agregar la aplicación a la pantalla de bloqueo para poder utilizar tareas en segundo plano.

Una tarea en segundo plano sólo se ejecutará con un **ApplicationTrigger** si ha llamado [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) en primer lugar.

## <a name="related-topics"></a>Temas relacionados

* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Ejemplo de código de tarea de fondo](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso.](create-and-register-a-background-task.md)
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
