---
title: Responder a eventos del sistema con tareas en segundo plano
description: Aprende a crear una tarea en segundo plano que responda a eventos SystemTrigger.
ms.assetid: 43C21FEA-28B9-401D-80BE-A61B71F01A89
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: c3773a486a1b7a29fc2a171c473edf38f6f3a7f1
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8712099"
---
# <a name="respond-to-system-events-with-background-tasks"></a>Responder a eventos del sistema con tareas en segundo plano

**API importantes**

- [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
- [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838)

Aprende a crear una tarea en segundo plano que responda a eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839).

En este tema se supone que tienes una clase de tareas en segundo plano escrita para tu aplicación y que esta tarea necesita ejecutarse en respuesta a un evento desencadenado por el sistema, como cuando la disponibilidad de Internet cambia o el usuario inicia sesión. Este tema se centra en la clase [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839). Para obtener más información sobre cómo escribir una clase de tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) o [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

## <a name="create-a-systemtrigger-object"></a>Crear un objeto SystemTrigger

En el código de tu aplicación, crea un nuevo objeto [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838). El primer parámetro, *triggerType*, especifica el tipo de desencadenador de eventos del sistema que activará esta tarea en segundo plano. Para ver una lista de los tipos de eventos, consulta [**SystemTriggerType**](https://msdn.microsoft.com/library/windows/apps/br224839).

El segundo parámetro, *OneShot*, especifica si la tarea en segundo plano se ejecutará solo una vez la próxima ocasión que ocurra el evento del sistema o cada vez que ocurra el evento del sistema hasta que se elimine el registro de la tarea.

El siguiente código especifica que la tarea en segundo plano se ejecuta siempre que Internet vuelva a estar disponible:

```csharp
SystemTrigger internetTrigger = new SystemTrigger(SystemTriggerType.InternetAvailable, false);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemTrigger internetTrigger{
    Windows::ApplicationModel::Background::SystemTriggerType::InternetAvailable, false};
```

```cpp
SystemTrigger ^ internetTrigger = ref new SystemTrigger(SystemTriggerType::InternetAvailable, false);
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

El siguiente código registra la tarea en segundo plano para un proceso en segundo plano que se ejecuta fuera de proceso. Si estuvieras llamando a una tarea en segundo plano que se ejecuta en el mismo proceso que la aplicación host, no establecerías `entrypoint`:

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass"; // Namespace name, '.', and the name of the class containing the background task
string taskName   = "Internet-based background task";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" }; // don't set for in-process background tasks.
std::wstring taskName{ L"Internet-based background task" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass"; // don't set for in-process background tasks
String ^ taskName   = "Internet-based background task";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, internetTrigger, exampleCondition);
```

> [!NOTE]
> Aplicaciones de la plataforma de Windows universales deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquiera de los tipos de desencadenadores en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

> [!NOTE]
> Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.
 
## <a name="remarks"></a>Observaciones

Para ver el registro de una tarea en segundo plano en acción, descarga la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).

Las tareas en segundo plano pueden ejecutarse en respuesta a los eventos [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) y [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517), pero aún debes [declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md). También debes llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de tarea en segundo plano.

Las aplicaciones pueden registrar tareas en segundo plano que respondan a los eventos [**TimeTrigger**](https://msdn.microsoft.com/library/windows/apps/br224843), [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543) y [**NetworkOperatorNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/br224831), lo que les permite proporcionar una comunicación en tiempo real con el usuario aunque la aplicación no esté en el primer plano. Para obtener más información, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)
