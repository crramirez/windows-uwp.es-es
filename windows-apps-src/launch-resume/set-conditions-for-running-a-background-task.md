---
author: TylerMSFT
title: Establecer condiciones para ejecutar una tarea en segundo plano
description: Aprende a establecer condiciones que controlan cuándo se ejecutará tu tarea en segundo plano.
ms.assetid: 10ABAC9F-AA8C-41AC-A29D-871CD9AD9471
ms.author: twhitney
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, uwp, tarea en segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: e64f36eb400d683da1cb52a819da5aa245a41ac4
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6264659"
---
# <a name="set-conditions-for-running-a-background-task"></a>Establecer condiciones para ejecutar una tarea en segundo plano

**API importantes**

- [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834)
- [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)
- [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Aprende a establecer condiciones que controlan cuándo se ejecutará tu tarea en segundo plano.

En ocasiones, las tareas en segundo plano requieren ciertas condiciones deben cumplirse para que la tarea en segundo plano se desarrolle correctamente. Puedes especificar una o más de las condiciones especificadas por [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) al registrar tu tarea en segundo plano. La condición se comprobará después de que se ha desencadenado el desencadenador. La tarea en segundo plano, a continuación, se pondrá en cola, pero no se ejecutará hasta que se satisfagan todas las condiciones.

Establecer condiciones sobre tareas en segundo plano, ahorra batería y la CPU evitando que las tareas se ejecuten innecesariamente. Por ejemplo, si una tarea en segundo plano se ejecuta en un temporizador y requiere conectividad a Internet, agrega la condición **InternetAvailable** al [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) antes de registrar la tarea. Esto ayudará a impedir que la tarea use recursos del sistema y consuma batería de forma innecesaria al ejecutarse en segundo plano solo cuando haya transcurrido el temporizador *e* Internet esté disponible.

También es posible combinar varias condiciones mediante una llamada a **AddCondition** varias veces en el mismo [**TaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768). Procura no agregar condiciones conflictivas, como **UserPresent** y **UserNotPresent**.

## <a name="create-a-systemcondition-object"></a>Crear un objeto SystemCondition

En este tema se supone que ya tienes una tarea en segundo plano asociada a tu aplicación y que esta ya incluye código que crea un objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) denominado **taskBuilder**.  Consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) o [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md) si necesitas crear primero una tarea en segundo plano.

Este tema se aplica a las tareas en segundo plano que se ejecutan fuera de proceso, así como las que se ejecutan en el mismo proceso que la aplicación en primer plano.

Antes de agregar la condición, crea un objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) para representar la condición que debe estar vigente para que ejecutar una tarea en segundo plano. En el constructor, especifica la condición que debe cumplirse con un valor de enumeración [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) .

El siguiente código crea un objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834) que especifica la condición de **InternetAvailable** :

```csharp
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);
```

```cppwinrt
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };
```

```cpp
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);
```

## <a name="add-the-systemcondition-object-to-your-background-task"></a>Agregar el objeto SystemCondition a la tarea en segundo plano

Para agregar una condición, llama al método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) en el objeto [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) y pásalo al objeto [**SystemCondition**](https://msdn.microsoft.com/library/windows/apps/br224834).

El siguiente código usa **taskBuilder** para agregar la condición de **InternetAvailable** .

```csharp
taskBuilder.AddCondition(internetCondition);
```

```cppwinrt
taskBuilder.AddCondition(internetCondition);
```

```cpp
taskBuilder->AddCondition(internetCondition);
```

## <a name="register-your-background-task"></a>Registro de tu tarea en segundo plano

Ahora puedes registrar la tarea en segundo plano con el método de [**registro**](https://msdn.microsoft.com/library/windows/apps/br224772) y la tarea en segundo plano no comenzará hasta que se cumpla la condición especificada.

El siguiente código registra la tarea y almacena el objeto BackgroundTaskRegistration resultante:

```csharp
BackgroundTaskRegistration task = taskBuilder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
BackgroundTaskRegistration ^ task = taskBuilder->Register();
```

> [!NOTE]
> Las aplicaciones universales de Windows deben llamar a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) antes de registrar cualquier tipo de desencadenador en segundo plano.

Para garantizar que la aplicación universal de Windows continúe funcionando correctamente después de publicar una actualización, se debe llamar a [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) y luego a [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) cuando se inicia la aplicación tras su actualización. Para obtener más información, consulta [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md).

> [!NOTE]
> Los parámetros de registro de tareas en segundo plano se validan en el momento en que se realiza el registro. Se devuelve un error si cualquiera de los parámetros de registro no es válido. Asegúrate de que la aplicación se enfrente correctamente a los escenarios en que se produce un error en el registro de tareas en segundo plano. Si la aplicación depende de que haya un objeto de registro válido después de intentar registrar una tarea, es posible que se bloquee.

## <a name="place-multiple-conditions-on-your-background-task"></a>Colocar varias condiciones en tu tarea en segundo plano

Para agregar varias condiciones, tu aplicación realiza varias llamadas al método [**AddCondition**](https://msdn.microsoft.com/library/windows/apps/br224769) . Estas llamadas deben recibirse antes de que el registro de la tarea entre en vigor.

> [!NOTE]
> Procura no para agregar condiciones conflictivas a una tarea en segundo plano.

El siguiente fragmento muestra varias condiciones en el contexto de crear y registrar una tarea en segundo plano.

```csharp
// Set up the background task.
TimeTrigger hourlyTrigger = new TimeTrigger(60, false);

var recurringTaskBuilder = new BackgroundTaskBuilder();

recurringTaskBuilder.Name           = "Hourly background task";
recurringTaskBuilder.TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition userCondition     = new SystemCondition(SystemConditionType.UserPresent);
SystemCondition internetCondition = new SystemCondition(SystemConditionType.InternetAvailable);

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.

BackgroundTaskRegistration task = recurringTaskBuilder.Register();
```

```cppwinrt
// Set up the background task.
Windows::ApplicationModel::Background::TimeTrigger hourlyTrigger{ 60, false };

Windows::ApplicationModel::Background::BackgroundTaskBuilder recurringTaskBuilder;

recurringTaskBuilder.Name(L"Hourly background task");
recurringTaskBuilder.TaskEntryPoint(L"Tasks.ExampleBackgroundTaskClass");
recurringTaskBuilder.SetTrigger(hourlyTrigger);

// Begin adding conditions.
Windows::ApplicationModel::Background::SystemCondition userCondition{
    Windows::ApplicationModel::Background::SystemConditionType::UserPresent };
Windows::ApplicationModel::Background::SystemCondition internetCondition{
    Windows::ApplicationModel::Background::SystemConditionType::InternetAvailable };

recurringTaskBuilder.AddCondition(userCondition);
recurringTaskBuilder.AddCondition(internetCondition);

// Done adding conditions, now register the background task.
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ recurringTaskBuilder.Register() };
```

```cpp
// Set up the background task.
TimeTrigger ^ hourlyTrigger = ref new TimeTrigger(60, false);

auto recurringTaskBuilder = ref new BackgroundTaskBuilder();

recurringTaskBuilder->Name           = "Hourly background task";
recurringTaskBuilder->TaskEntryPoint = "Tasks.ExampleBackgroundTaskClass";
recurringTaskBuilder->SetTrigger(hourlyTrigger);

// Begin adding conditions.
SystemCondition ^ userCondition     = ref new SystemCondition(SystemConditionType::UserPresent);
SystemCondition ^ internetCondition = ref new SystemCondition(SystemConditionType::InternetAvailable);

recurringTaskBuilder->AddCondition(userCondition);
recurringTaskBuilder->AddCondition(internetCondition);

// Done adding conditions, now register the background task.
BackgroundTaskRegistration ^ task = recurringTaskBuilder->Register();
```

## <a name="remarks"></a>Observaciones

> [!NOTE]
> Elige las condiciones adecuadas para tu tarea en segundo plano para que solo se ejecuta cuando es necesario y no se ejecute cuando no deba. Consulta [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835) para obtener descripciones de las diferentes condiciones de las tareas en segundo plano.

## <a name="related-topics"></a>Temas relacionados

* [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md)
* [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Supervisar el progreso y la finalización de tareas en segundo plano](monitor-background-task-progress-and-completion.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones para UWP (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)
