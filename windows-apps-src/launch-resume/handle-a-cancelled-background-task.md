---
author: TylerMSFT
title: Controlar una tarea en segundo plano cancelada
description: "Aprende a crear una tarea en segundo plano que reconozca solicitudes de cancelación y detenga el trabajo, y que informe de la cancelación a la aplicación a través del almacenamiento persistente."
ms.assetid: B7E23072-F7B0-4567-985B-737DD2A8728E
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: ab575415e5e6a091fb45dab49af21d0552834406

---

# Controlar una tarea en segundo plano cancelada

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**API importantes**

-   [**BackgroundTaskCanceledEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224775)
-   [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797)
-   [**ApplicationData.Current**](https://msdn.microsoft.com/library/windows/apps/br241619)

Aprende a crear una tarea en segundo plano que reconozca solicitudes de cancelación y detenga el trabajo, y que informe de la cancelación a la aplicación a través del almacenamiento persistente.

> **Nota**  Para todas las familias de dispositivos, excepto la de equipos de escritorio, si el dispositivo dispone de poca memoria, las tareas en segundo plano pueden finalizarse. Si no se expone una excepción de falta de memoria o la aplicación no la controla, la tarea en segundo plano finalizará sin que se muestre ninguna advertencia y sin que se genere el evento OnCanceled. Esto contribuye a garantizar la experiencia del usuario de la aplicación en primer plano. La tarea en segundo plano debe estar diseñada para controlar este escenario.

En este tema se da por hecho que ya has creado una clase de tarea en segundo plano, incluido el método Run que se usa como punto de entrada de la tarea en segundo plano. Para comenzar rápidamente a crear una tarea en segundo plano, consulta [Creación y registro de una tarea en segundo plano](create-and-register-a-background-task.md). Para obtener información más detallada sobre las condiciones y los desencadenadores, consulta [Dar soporte a una aplicación mediante tareas en segundo plano](support-your-app-with-background-tasks.md).

## Usar el método OnCanceled para reconocer las solicitudes de cancelación

Escribe un método para controlar el evento de cancelación.

Crea un método llamado OnCanceled que tenga la siguiente superficie. Este método es el punto de entrada al que llama Windows en tiempo de ejecución siempre que se realiza una solicitud de cancelación contra tu tarea en segundo plano.

El método OnCanceled necesita tener la siguiente superficie:

> [!div class="tabbedCodeSnippets"]
> ```cs
>    private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```
> ```cpp
>    void ExampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>    {
>        // TODO: Add code to notify the background task that it is cancelled.
>    }
> ```

[!div class="tabbedCodeSnippets"] Agrega una variable de marca denominada **\_CancelRequested** a la clase de la tarea en segundo plano.

> [!div class="tabbedCodeSnippets"]
> ```cs
>   volatile bool _CancelRequested = false;
> ```
> ```cpp
>   private:
>     volatile bool CancelRequested;
> ```

Esta variable se usará para indicar que se ha realizado una solicitud de cancelación.

[!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
>     private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         _cancelRequested = true;
>
>         Debug.WriteLine("Background " + sender.Task.Name + " Cancel Requested...");
>     }
> ```
> ```cpp
>     void SampleBackgroundTask::OnCanceled(IBackgroundTaskInstance^ taskInstance, BackgroundTaskCancellationReason reason)
>     {
>         //
>         // Indicate that the background task is canceled.
>         //
>
>         CancelRequested = true;
>     }
> ```

En el método OnCanceled que creaste en el paso 1, establece la variable de marca **\_CancelRequested** en **true**. El método OnCanceled del [ejemplo de tarea en segundo plano]( http://go.microsoft.com/fwlink/p/?linkid=227509) completo establece el valor **\_CancelRequested** en **true** y escribe resultados de depuración potencialmente útiles:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
> ```
> ```cpp
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
> ```

## [!div class="tabbedCodeSnippets"]


En el método Run de la tarea en segundo plano, registra el método del controlador de eventos OnCanceled antes de empezar a trabajar.

Por ejemplo, usa la siguiente línea de código: [!div class="tabbedCodeSnippets"]

Controlar la cancelación mediante una salida del método Run

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>
>         // TODO: Record whether the task completed or was cancelled.
>     }
> ```

> Cuando se recibe una solicitud de cancelación, el método Run necesita detener el trabajo y finalizar reconociendo cuándo se establece **\_cancelRequested** en **true**. Modifica el código de la clase de la tarea en segundo plano para comprobar la variable de marca mientras está funcionando.

Si el valor **\_cancelRequested** está establecido en true, detén el trabajo.

El [ejemplo de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) incluye una comprobación que detiene la devolución de llamada periódica al temporizador si se cancela la tarea en segundo plano:

> [!div class="tabbedCodeSnippets"]
> ```cs
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.TaskId.ToString();
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>
>         if (_cancelRequested)
>         {
>             settings.Values[key] = "Canceled";
>         }
>         else
>         {
>             settings.Values[key] = "Completed";
>         }
>         
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + (_cancelRequested ? " Canceled" : " Completed"));
>         
>         //
>         // Indicate that the background task has completed.
>         //
>
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     if ((CancelRequested == false) && (Progress < 100))
>     {
>         Progress += 10;
>         TaskInstance->Progress = Progress;
>     }
>     else
>     {
>         PeriodicTimer->Cancel();
>         
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         
>         auto settings = ApplicationData::Current->LocalSettings;
>         auto key = TaskInstance->Task->Name;
>         settings->Values->Insert(key, (Progress < 100) ? "Canceled" : "Completed");
>         
>         //
>         // Indicate that the background task has completed.
>         //
>         
>         Deferral->Complete();
>     }
> ```

## [!div class="tabbedCodeSnippets"]

**Nota**  El ejemplo de código anterior usa la propiedad [**IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/br224797).[**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800), que se usa para registrar el progreso de la tarea en segundo plano.

El progreso se notifica a la aplicación mediante la clase [**BackgroundTaskProgressEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224782).

## Modifica el método Run de forma que cuando se detenga el trabajo, registre si la tarea se completó o se canceló.

El [ejemplo de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) registra el estado en LocalSettings:

> [!div class="tabbedCodeSnippets"]
> ```cs
> //
> // The Run method is the entry point of a background task.
> //
> public void Run(IBackgroundTaskInstance taskInstance)
> {
>     Debug.WriteLine("Background " + taskInstance.Task.Name + " Starting...");
>
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     var cost = BackgroundWorkCost.CurrentBackgroundWorkCost;
>     var settings = ApplicationData.Current.LocalSettings;
>     settings.Values["BackgroundWorkCost"] = cost.ToString();
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance;
>     //
>     _deferral = taskInstance.GetDeferral();
>     _taskInstance = taskInstance;
>
>     _periodicTimer = ThreadPoolTimer.CreatePeriodicTimer(new TimerElapsedHandler(PeriodicTimerCallback), TimeSpan.FromSeconds(1));
> }
>
> //
> // Simulate the background task activity.
> //
> private void PeriodicTimerCallback(ThreadPoolTimer timer)
> {
>     if ((_cancelRequested == false) && (_progress < 100))
>     {
>         _progress += 10;
>         _taskInstance.Progress = _progress;
>     }
>     else
>     {
>         _periodicTimer.Cancel();
>
>         var settings = ApplicationData.Current.LocalSettings;
>         var key = _taskInstance.Task.Name;
>
>         //
>         // Write to LocalSettings to indicate that this background task ran.
>         //
>         settings.Values[key] = (_progress < 100) ? "Canceled with reason: " + _cancelReason.ToString() : "Completed";
>         Debug.WriteLine("Background " + _taskInstance.Task.Name + settings.Values[key]);
>
>         //
>         // Indicate that the background task has completed.
>         //
>         _deferral.Complete();
>     }
> }
> ```
> ```cpp
> void SampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
> {
>     //
>     // Query BackgroundWorkCost
>     // Guidance: If BackgroundWorkCost is high, then perform only the minimum amount
>     // of work in the background task and return immediately.
>     //
>     auto cost = BackgroundWorkCost::CurrentBackgroundWorkCost;
>     auto settings = ApplicationData::Current->LocalSettings;
>     settings->Values->Insert("BackgroundWorkCost", cost.ToString());
>
>     //
>     // Associate a cancellation handler with the background task.
>     //
>     taskInstance->Canceled += ref new BackgroundTaskCanceledEventHandler(this, &SampleBackgroundTask::OnCanceled);
>
>     //
>     // Get the deferral object from the task instance, and take a reference to the taskInstance.
>     //
>     TaskDeferral = taskInstance->GetDeferral();
>     TaskInstance = taskInstance;
>
>     auto timerDelegate = [this](ThreadPoolTimer^ timer)
>     {
>         if ((CancelRequested == false) &&
>             (Progress < 100))
>         {
>             Progress += 10;
>             TaskInstance->Progress = Progress;
>         }
>         else
>         {
>             PeriodicTimer->Cancel();
>
>             //
>             // Write to LocalSettings to indicate that this background task ran.
>             //
>             auto settings = ApplicationData::Current->LocalSettings;
>             auto key = TaskInstance->Task->Name;
>             settings->Values->Insert(key, (Progress < 100) ? "Canceled with reason: " + CancelReason.ToString() : "Completed");
>
>             //
>             // Indicate that the background task has completed.
>             //
>             TaskDeferral->Complete();
>         }
>     };
>
>     TimeSpan period;
>     period.Duration = 1000 * 10000; // 1 second
>     PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(ref new TimerElapsedHandler(timerDelegate), period);
> }
> ```

> [!div class="tabbedCodeSnippets"] Observaciones

## Puedes descargar el [ejemplo de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) para ver estos ejemplos de código en contexto dentro de los métodos.

* [Por propósitos de ilustración, la muestra de código anterior solo muestra algunas partes del método Run (y temporizador de devolución de llamadas) de la [muestra de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666).](create-and-register-a-background-task.md)
* [Ejemplo de método Run](declare-background-tasks-in-the-application-manifest.md)
* [El método Run completo y el código de la devolución de llamada del temporizador, del [ejemplo de tarea en segundo plano](http://go.microsoft.com/fwlink/p/?LinkId=618666) se muestra a continuación para ilustrar el contexto:](guidelines-for-background-tasks.md)
* [[!div class="tabbedCodeSnippets"]](monitor-background-task-progress-and-completion.md)
* [**Nota**  Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP).](register-a-background-task.md)
* [Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).](respond-to-system-events-with-background-tasks.md)
* [Temas relacionados](run-a-background-task-on-a-timer-.md)
* [Crear y registrar una tarea en segundo plano](set-conditions-for-running-a-background-task.md)
* [Declarar tareas en segundo plano en el manifiesto de la aplicación](update-a-live-tile-from-a-background-task.md)
* [Directrices para tareas en segundo plano](use-a-maintenance-trigger.md)

* [Supervisar el progreso y la finalización de tareas en segundo plano](debug-a-background-task.md)
* [Registrar una tarea en segundo plano](http://go.microsoft.com/fwlink/p/?linkid=254345)



<!--HONumber=Jun16_HO5-->


