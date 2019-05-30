---
title: Usar un desencadenador de mantenimiento
description: Aprende a usar la clase MaintenanceTrigger para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado.
ms.assetid: 727D9D84-6C1D-4DF3-B3B0-2204EA4D76DD
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: d59d5cd7a2ffbc55b36f0169939859bf1b6b9db5
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370565"
---
# <a name="use-a-maintenance-trigger"></a>Usar un desencadenador de mantenimiento

**API importantes**

- [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger)
- [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
- [**SystemCondition**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemCondition)

Aprende a usar la clase [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger) para ejecutar código ligero en segundo plano mientras el dispositivo está enchufado.

## <a name="create-a-maintenance-trigger-object"></a>Crear un objeto de desencadenador de mantenimiento

En este ejemplo se da por hecho que tienes un código ligero que puedes ejecutar en segundo plano para mejorar la aplicación mientras el dispositivo está enchufado. Este tema se centra en la clase [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger), que es similar a [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Para obtener más información sobre cómo escribir una clase de tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) o [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md).

Crea un nuevo objeto [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger). El segundo parámetro, *OneShot*, especifica si la tarea de mantenimiento se ejecutará solo una vez o si seguirá ejecutándose periódicamente. Si *OneShot* se establece en "true", el primer parámetro (*FreshnessTime*) especifica el número de minutos que se debe esperar antes de programar la tarea en segundo plano. Si *OneShot* se establece en "false", *FreshnessTime* especifica la frecuencia con la que se ejecutará la tarea en segundo plano.

> [!NOTE]
> Si *FreshnessTime* se establece en menos de 15 minutos, se produce una excepción al intentar registrar la tarea en segundo plano.

Este código de ejemplo crea un desencadenador que se ejecuta una vez cada hora.

```csharp
uint waitIntervalMinutes = 60;
MaintenanceTrigger taskTrigger = new MaintenanceTrigger(waitIntervalMinutes, false);
```

```cppwinrt
uint32_t waitIntervalMinutes{ 60 };
Windows::ApplicationModel::Background::MaintenanceTrigger taskTrigger{ waitIntervalMinutes, false };
```

```cpp
unsigned int waitIntervalMinutes = 60;
MaintenanceTrigger ^ taskTrigger = ref new MaintenanceTrigger(waitIntervalMinutes, false);
```

## <a name="optional-add-a-condition"></a>(Opcional) Agregar una condición

- Si es necesario, crea una condición a la tarea en segundo plano para controlar cuándo debe ejecutarse la tarea. Una condición evita que tu tarea en segundo plano se ejecute hasta que no se cumpla la condición. Para obtener más información, consulta [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md).

En este ejemplo, la condición se establece en **InternetAvailable** de forma que el mantenimiento se ejecuta cuando Internet está disponible (o cuando vuelva a estarlo). Para obtener una lista de posibles condiciones de tareas en segundo plano, consulta [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

El siguiente código agrega una condición al generador de tareas de mantenimiento:

```csharp
SystemCondition exampleCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition exampleCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ exampleCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="register-the-background-task"></a>Registrar la tarea en segundo plano

- Registra la tarea en segundo plano llamando a tu función de registro de tareas en segundo plano. Para obtener más información sobre el registro de tareas en segundo plano, consulta [Registrar una tarea en segundo plano](register-a-background-task.md).

El siguiente código registra la tarea de mantenimiento. Ten en cuenta que se supone que la tarea en segundo plano se ejecuta en un proceso independiente de la aplicación, ya que especifica el elemento `entryPoint`. Si la tarea en segundo plano se ejecuta en el mismo proceso que la aplicación, no se especifica `entryPoint`.

```csharp
string entryPoint = "Tasks.ExampleBackgroundTaskClass";
string taskName   = "Maintenance background task example";

BackgroundTaskRegistration task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

```cppwinrt
std::wstring entryPoint{ L"Tasks.ExampleBackgroundTaskClass" };
std::wstring taskName{ L"Maintenance background task example" };

Windows::ApplicationModel::Background::BackgroundTaskRegistration task{
    RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition) };
```

```cpp
String ^ entryPoint = "Tasks.ExampleBackgroundTaskClass";
String ^ taskName   = "Maintenance background task example";

BackgroundTaskRegistration ^ task = RegisterBackgroundTask(entryPoint, taskName, taskTrigger, exampleCondition);
```

> [!NOTE]
> Para todas las familias de dispositivos excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano podrían finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

> [!NOTE]
> Deben llamar la plataforma de Windows universales [ **RequestAccessAsync** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) antes de registrar cualquiera de los tipos de desencadenador en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización de la aplicación, se debe llamar a [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess) y a continuación a [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) una vez iniciada la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

> [!NOTE]
> Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se ocupe correctamente de los escenarios en los que se produce un error en el registro de tareas en segundo plano. Si, en cambio, la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano dentro del proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar suspender, reanudar y en segundo plano de los eventos en aplicaciones para UWP (al depurar)](https://go.microsoft.com/fwlink/p/?linkid=254345)
