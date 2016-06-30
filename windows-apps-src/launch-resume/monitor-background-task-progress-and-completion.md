---
author: TylerMSFT
title: "Supervisar el progreso y la finalización de tareas en segundo plano"
description: "Aprende el modo en que la aplicación puede reconocer el progreso y finalización notificados por una tarea en segundo plano."
ms.assetid: 17544FD7-A336-4254-97DC-2BF8994FF9B2
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 07d69b63b272153bc784ed19166e649f80a98297

---

# Supervisar el progreso y la finalización de tareas en segundo plano


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)
-   [**BackgroundTaskProgressEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224785)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

Aprende el modo en que la aplicación puede reconocer el progreso y finalización notificados por una tarea en segundo plano. Las tareas en segundo plano están desacopladas de la aplicación y se ejecutan de forma independiente, pero el código de la aplicación puede supervisar el progreso y la finalización de una tarea en segundo plano. Para ello, la aplicación se suscribe a eventos de las tareas en segundo plano que ha registrado con el sistema.

-   En este tema se supone que tienes una aplicación que registra tareas en segundo plano. Para comenzar rápidamente a crear una tarea en segundo plano, consulta [Creación y registro de una tarea en segundo plano](create-and-register-a-background-task.md). Para obtener información más detallada acerca de condiciones y desencadenadores, consulta [Dar soporte a tu aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## Crear un controlador de eventos para controlar las tareas en segundo plano finalizadas


1.  Crea una función de controlador de eventos para controlar las tareas en segundo plano finalizadas. Este código necesita seguir una superficie específica, que toma un objeto [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) y un objeto [**BackgroundTaskCompletedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224778).

    Usa la siguiente superficie para el método OnCompleted del controlador de eventos de la tarea en segundo plano:

>  [!div class="tabbedCodeSnippets"]
>  ```cs
>  private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>  {
>      // TODO: Add code that deals with background task completion.
>  }
>  ```
>  ```cpp
>  auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>  {
>      // TODO: Add code that deals with background task completion.
>  };
>  ```

2.  Agrega código al controlador de eventos que se encarga de la finalización de la tarea en segundo plano.

    Por ejemplo, la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) actualiza la interfaz de usuario.

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    >     {
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >     {    
    >         UpdateUI();
    >     };
    >     ```

## Crear una función de controlador de eventos para controlar el progreso de las tareas en segundo plano


1.  Crea una función de controlador de eventos para controlar las tareas en segundo plano finalizadas. Este código necesita seguir una superficie específica, que toma un objeto [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224803) y un objeto [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782):

    Usa la siguiente superficie para el método OnProgress del controlador de eventos de la tarea en segundo plano:

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         // TODO: Add code that deals with background task progress.
    >     };
    >     ```

2.  Agrega código al controlador de eventos que se encarga de la finalización de la tarea en segundo plano.

    Por ejemplo, la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) actualiza la interfaz de usuario con el estado de progreso pasado a través del parámetro *args*:

    > [!div class="tabbedCodeSnippets"]     ```cs     private void OnProgress(IBackgroundTaskRegistration task, BackgroundTaskProgressEventArgs args)     {         var progress = "Progress: " + args.Progress + "%";         BackgroundTaskSample.SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >     {
    >         auto progress = "Progress: " + args->Progress + "%";
    >         BackgroundTaskSample::SampleBackgroundTaskProgress = progress;
    >
    >         UpdateUI();
    >     };
    >     ```

## Registra las funciones del controlador de eventos con tareas en segundo plano nuevas y existentes.


1.  Cuando la aplicación registra una tarea en segundo plano por primera vez, debería registrarla para recibir actualizaciones de progreso y finalización para ella, en caso de que la tarea se ejecute mientras la aplicación sigue activa en primer plano.

    Por ejemplo, la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) llama a la siguiente función en todas las tareas en segundo plano que registra:

    > [!div class="tabbedCodeSnippets"] ```cs
    >     private void AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration task)
    >     {
    >         task.Progress += new BackgroundTaskProgressEventHandler(OnProgress);
    >         task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    >     }
    >     ```
    >     ```cpp     void SampleBackgroundTask::AttachProgressAndCompletedHandlers(IBackgroundTaskRegistration^ task)     {         auto progress = [this](BackgroundTaskRegistration^ task, BackgroundTaskProgressEventArgs^ args)
    >                   {             auto progress = "Progress: " + args->Progress + "%";             BackgroundTaskSample::SampleBackgroundTaskProgress = progress;             UpdateUI();         };
    >
    >         task->Progress += ref new BackgroundTaskProgressEventHandler(progress);
    >         
    >
    >         auto completed = [this](BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    >         {
    >             UpdateUI();
    >         };
    >
    >         task->Completed += ref new BackgroundTaskCompletedEventHandler(completed);
    >     }
    >     ```

2.  Cuando la aplicación se ejecuta o navega a una página nueva en la que es relevante el estado de la tarea en segundo plano, debería obtener una lista de las tareas en segundo plano registradas actualmente y asociarlas con las funciones del controlador de eventos de progreso y finalización. La lista de tareas en segundo plano actualmente registradas por la aplicación se conserva en la propiedad [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786).[**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787).

    Por ejemplo, la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) usa el siguiente código para adjuntar controladores de eventos cuando se navega por la página SampleBackgroundTask para:

    > [!div class="tabbedCodeSnippets"]     ```cs     protected override void OnNavigatedTo(NavigationEventArgs e)     {         foreach (var task in BackgroundTaskRegistration.AllTasks)         {             if (task.Value.Name == BackgroundTaskSample.SampleBackgroundTaskName)             {                 AttachProgressAndCompletedHandlers(task.Value);                 BackgroundTaskSample.UpdateBackgroundTaskStatus(BackgroundTaskSample.SampleBackgroundTaskName, true);             }         }
    >
    >         UpdateUI();
    >     }
    >     ```
    >     ```cpp
    >     void SampleBackgroundTask::OnNavigatedTo(NavigationEventArgs^ e)
    >     {
    >         // A pointer back to the main page.  This is needed if you want to call methods in MainPage such
    >         // as NotifyUser()
    >         rootPage = MainPage::Current;
    >
    >         //
    >         // Attach progress and completed handlers to any existing tasks.
    >         //
    >         auto iter = BackgroundTaskRegistration::AllTasks->First();
    >         auto hascur = iter->HasCurrent;
    >         while (hascur)
    >         {
    >             auto cur = iter->Current->Value;
    >
    >             if (cur->Name == SampleBackgroundTaskName)
    >             {
    >                 AttachProgressAndCompletedHandlers(cur);
    >                 break;
    >             }
    >
    >             hascur = iter->MoveNext();
    >         }
    >
    >         UpdateUI();
    >     }
    >     ```

## Temas relacionados


****

* [Crear y registrar una tarea en segundo plano](create-and-register-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](declare-background-tasks-in-the-application-manifest.md)
* [Controlar una tarea en segundo plano cancelada](handle-a-cancelled-background-task.md)
* [Registrar una tarea en segundo plano](register-a-background-task.md)
* [Responder a eventos del sistema con tareas en segundo plano](respond-to-system-events-with-background-tasks.md)
* [Establecer condiciones para ejecutar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Actualizar un icono dinámico desde una tarea en segundo plano](update-a-live-tile-from-a-background-task.md)
* [Usar un desencadenador de mantenimiento](use-a-maintenance-trigger.md)
* [Ejecutar una tarea en segundo plano en un temporizador](run-a-background-task-on-a-timer-.md)
* [Directrices para tareas en segundo plano](guidelines-for-background-tasks.md)

****

* [Depurar una tarea en segundo plano](debug-a-background-task.md)
* [Cómo desencadenar los eventos suspender, reanudar y en segundo plano en aplicaciones de la Tienda Windows (al depurar)](http://go.microsoft.com/fwlink/p/?linkid=254345)

 

 



<!--HONumber=Jun16_HO4-->


