---
title: Supervisar el progreso y la finalización de tareas en segundo plano
description: Aprende el modo en que la aplicación puede reconocer el progreso y finalización notificados por una tarea en segundo plano.
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP, tarea en segundo plano
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 9c6fb40d64d0826a5cbbfa7c97694e6650df9dbe
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172979"
---
# <a name="monitor-background-task-progress-and-completion"></a>Supervisar el progreso y la finalización de tareas en segundo plano

**API importantes**

- [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)
- [**BackgroundTaskProgressEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskprogresseventhandler)
- [**BackgroundTaskCompletedEventHandler**](/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Aprende cómo la aplicación puede reconocer el progreso y la finalización notificados por una tarea en segundo plano que se ejecuta fuera del proceso. (Para tareas en segundo plano dentro del proceso, puedes establecer variables compartidas para indicar el progreso y finalización).

El código de la aplicación puede supervisar el progreso y la finalización de la tarea en segundo plano. Para ello, la aplicación se suscribe a eventos de las tareas en segundo plano que ha registrado con el sistema.

- En este tema se supone que tienes una aplicación que registra tareas en segundo plano. Para comenzar rápidamente a crear una tarea en segundo plano, consulta [Crear y registrar una tarea en segundo plano dentro de proceso](create-and-register-an-inproc-background-task.md) o [Crear y registrar una tarea en segundo plano fuera de proceso](create-and-register-a-background-task.md). Para obtener información más detallada acerca de las condiciones y los desencadenadores, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## <a name="create-an-event-handler-to-handle-completed-background-tasks"></a>Crear un controlador de eventos para controlar las tareas en segundo plano finalizadas

### <a name="step-1"></a>Paso 1
Crea una función de controlador de eventos para controlar las tareas en segundo plano finalizadas. Este código debe seguir una superficie específica, que toma un objeto [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) y un objeto [**BackgroundTaskCompletedEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskCompletedEventArgs) .

Use la siguiente superficie para el método de controlador de eventos de tarea en segundo plano **Alfinalizar** .

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    // TODO: Add code that deals with background task completion.
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task completion.
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    // TODO: Add code that deals with background task completion.
};
```

### <a name="step-2"></a>Paso 2
Agrega código al controlador de eventos que se encarga de la finalización de la tarea en segundo plano.

Por ejemplo, la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) actualiza la interfaz de usuario.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    UpdateUI();
}
```

```cppwinrt
auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    UpdateUI();
} };
```

```cpp
auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{    
    UpdateUI();
};
```

## <a name="create-an-event-handler-function-to-handle-background-task-progress"></a>Crear una función de controlador de eventos para controlar el progreso de las tareas en segundo plano

### <a name="step-1"></a>Paso 1
Crea una función de controlador de eventos para controlar las tareas en segundo plano finalizadas. Este código necesita seguir una superficie específica, que toma un objeto [**IBackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) y un objeto [**BackgroundTaskProgressEventArgs**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskProgressEventArgs):

Usa la siguiente superficie para el método OnProgress del controlador de eventos de tarea en segundo plano:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    // TODO: Add code that deals with background task progress.
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& /* args */)
{
    // TODO: Add code that deals with background task progress.
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    // TODO: Add code that deals with background task progress.
};
```

### <a name="step-2"></a>Paso 2
Agrega código al controlador de eventos que se encarga de la finalización de la tarea en segundo plano.

Por ejemplo, la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) actualiza la interfaz de usuario con el estado de progreso pasado a través del parámetro *args*:

```csharp
private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
{
    var progress = "Progress: " + args.Progress + "%";
    BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    UpdateUI();
}
```

```cppwinrt
auto progress{ [this](
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
    Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
{
    winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
} };
```

```cpp
auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
{
    auto progress = "Progress: " + args->Progress + "%";
    BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    UpdateUI();
};
```

## <a name="register-the-event-handler-functions-with-new-and-existing-background-tasks"></a>Registrar las funciones del controlador de eventos con tareas en segundo plano nuevas y existentes

### <a name="step-1"></a>Paso 1
Cuando la aplicación registra una tarea en segundo plano por primera vez, debería registrarla para recibir actualizaciones de progreso y finalización referentes a ella, en caso de que la tarea se ejecute mientras la aplicación sigue activa en primer plano.

Por ejemplo, la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) llama a la siguiente función en todas las tareas en segundo plano que registra:

```csharp
private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
{
    task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
}
```

```cppwinrt
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(Windows::ApplicationModel::Background::IBackgroundTaskRegistration const& task)
{
    auto progress{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskProgressEventArgs const& args)
    {
        winrt::hstring progress{ L"Progress: " + winrt::to_hstring(args.Progress()) + L"%" };
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    } };

    task.Progress(progress);

    auto completed{ [this](
        Windows::ApplicationModel::Background::BackgroundTaskRegistration const& /* sender */,
        Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
    {
        UpdateUI();
    } };

    task.Completed(completed);
}
```

```cpp
void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)
{
    auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    {
        auto progress = "Progress: " + args->Progress + "%";
        BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
        UpdateUI();
    };

    task->Progress += ref new BackgroundTaskProgressEventHandler(progress);

    auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        UpdateUI();
    };

    task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
}
```

### <a name="step-2"></a>Paso 2
Cuando la aplicación se ejecuta o navega a una página nueva en la que es relevante el estado de la tarea en segundo plano, debería obtener una lista de las tareas en segundo plano registradas actualmente y asociarlas con las funciones del controlador de eventos de progreso y finalización. La lista de tareas en segundo plano actualmente registradas por la aplicación se conserva en la propiedad [**BackgroundTaskRegistration**](/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration).[**AllTasks**](/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks).

Por ejemplo, la [muestra de tarea en segundo plano](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask) usa el siguiente código para adjuntar controladores de eventos cuando se navega por la página SampleBackgroundTask para:

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    foreach (var task in BackgroundTaskRegistration.AllTasks)
    {
        if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value);
            BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);
        }
    }

    UpdateUI();
}
```

```cppwinrt
void SampleBackgroundTask::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& /* e */)
{
    // A pointer back to the main page. This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    m_rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

    for (auto const& task : allTasks)
    {
        if (task.Value().Name() == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(task.Value());
            break;
        }
    }

    UpdateUI();
}
```

```cpp
void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
{
    // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    // as NotifyUser().
    rootPage = MainPage::Current;

    // Attach progress and completed handlers to any existing tasks.
    auto iter = BackgroundTaskRegistration::AllTasks->First();
    auto hascur = iter->HasCurrent;
    while (hascur)
    {
        auto cur = iter->Current->Value;

        if (cur->Name == SampleBackgroundTaskName)
        {
            AttachProgressAndCompletedHandlers(cur);
            break;
        }

        hascur = iter->MoveNext();
    }

    UpdateUI();
}
```

## <a name="related-topics"></a>Temas relacionados

* [Cree y registre una tarea en segundo plano en proceso](create-and-register-an-inproc-background-task.md).
* [Crear y registrar una tarea en segundo plano fuera del proceso](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)
* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar eventos de suspensión, reanudación y en segundo plano en aplicaciones UWP (durante la depuración)](/previous-versions/hh974425(v=vs.110))