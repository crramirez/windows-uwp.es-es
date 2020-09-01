---
ms.assetid: AAE467F9-B3C7-4366-99A2-8A880E5692BE
title: Enviar un elemento de trabajo con un temporizador
description: Obtenga información sobre cómo crear un temporizador que envíe un elemento de trabajo cuando el temporizador transcurre en una aplicación Plataforma universal de Windows (UWP) mediante la API de ThreadPoolTimer.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, temporizador y subprocesos
ms.localizationpriority: medium
ms.openlocfilehash: a93b023120957f6335c14a4d40013f51e4e7be2a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164119"
---
# <a name="use-a-timer-to-submit-a-work-item"></a>Enviar un elemento de trabajo con un temporizador


<b>API importantes</b>

-   [**Espacio de nombres Windows.UI.Core**](/uwp/api/Windows.UI.Core)
-   [**Espacio de nombres Windows.System.Threading**](/uwp/api/Windows.System.Threading)

Obtén información acerca de cómo crear un elemento de trabajo que se ejecute después de que transcurra un temporizador.

## <a name="create-a-single-shot-timer"></a>Crear un temporizador de único disparo

Usa el método [**CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) para crear un temporizador para el elemento de trabajo. Envía un lambda que realice el trabajo y usa el parámetro *delay* para especificar cuánto tiempo espera el grupo de subprocesos antes de poder asignar el elemento de trabajo a un subproceso disponible. El retraso se especifica con una estructura [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan).

> **Nota:**    Puede usar [**CoreDispatcher. RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para tener acceso a la interfaz de usuario y mostrar el progreso del elemento de trabajo.

En el siguiente ejemplo se crea un elemento de trabajo que se ejecuta en tres minutos:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, delay);
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([this](ThreadPoolTimer^ source)
>         {
>             //
>             // TODO: Work
>             //
>             
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([this]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     ExampleUIUpdateMethod("Timer completed.");
>
>                 }));
>
>         }), delay);
> ```

## <a name="provide-a-completion-handler"></a>Proporcionar un controlador de finalización

Si es necesario, controla la cancelación y la finalización del elemento de trabajo con un [**TimerDestroyedHandler**](/uwp/api/windows.system.threading.timerdestroyedhandler). Usa la sobrecarga [**CreateTimer**](/uwp/api/windows.system.threading.threadpooltimer.createtimer) para enviar un lambda adicional. Este se ejecuta cuando se cancela el temporizador o cuando se completa el elemento de trabajo.

En el siguiente ejemplo se crea un temporizador que envía el elemento de trabajo y llama a un método cuando finaliza el elemento de trabajo o cuando se cancela el temporizador:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> TimeSpan delay = TimeSpan.FromMinutes(3);
>             
> bool completed = false;
>
> ThreadPoolTimer DelayTimer = ThreadPoolTimer.CreateTimer(
>     (source) =>
>     {
>         //
>         // TODO: Work
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>                 CoreDispatcherPriority.High,
>                 () =>
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 });
>
>         completed = true;
>     },
>     delay,
>     (source) =>
>     {
>         //
>         // TODO: Handle work cancellation/completion.
>         //
>
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(
>             CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>                 if (completed)
>                 {
>                     // Timer completed.
>                 }
>                 else
>                 {
>                     // Timer cancelled.
>                 }
>
>             });
>     });
> ```
> ``` cpp
> TimeSpan delay;
> delay.Duration = 3 * 60 * 10000000; // 10,000,000 ticks per second
>
> completed = false;
>
> ThreadPoolTimer ^ DelayTimer = ThreadPoolTimer::CreateTimer(
>         ref new TimerElapsedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Work
>             //
>
>             //
>             // Update the UI thread by using the UI core dispatcher.
>             //
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                 }));
>
>             completed = true;
>
>         }),
>         delay,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle work cancellation/completion.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // Update the UI thread by using the UI core dispatcher.
>                     //
>
>                     if (completed)
>                     {
>                         // Timer completed.
>                     }
>                     else
>                     {
>                         // Timer cancelled.
>                     }
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Cancelar el temporizador

Si el temporizador sigue contando el tiempo restante pero ya no se necesita el elemento de trabajo, llama a [**Cancel**](/uwp/api/windows.system.threading.threadpooltimer.cancel). Se cancela el temporizador y el elemento de trabajo no se envía al grupo de subprocesos.

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> DelayTimer.Cancel();
> ```
> ``` cpp
> DelayTimer->Cancel();
> ```

## <a name="remarks"></a>Observaciones

Las aplicaciones de la Plataforma universal de Windows (UWP) no pueden usar **Thread.Sleep** porque puede bloquear el subproceso de interfaz de usuario. En su lugar, puedes usar un [**ThreadPoolTimer**](/uwp/api/Windows.System.Threading.ThreadPoolTimer) para crear un elemento de trabajo, y esto retrasará la tarea realizada por el elemento de trabajo sin bloquear el subproceso de interfaz de usuario.

Consulta la [muestra de grupo de subprocesos](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Thread%20pool%20sample) para obtener una muestra de código completa de elementos de trabajo, elementos de trabajo de temporizador y elementos de trabajo periódicos. El ejemplo de código se escribió originalmente para Windows 8.1, pero el código se puede reutilizar en Windows 10.

Para obtener información sobre cómo repetir temporizadores, consulta [Crear un elemento de trabajo periódico](create-a-periodic-work-item.md).

## <a name="related-topics"></a>Temas relacionados

* [Enviar un elemento de trabajo al grupo de subprocesos](submit-a-work-item-to-the-thread-pool.md)
* [Procedimientos recomendados para usar el grupo de subprocesos](best-practices-for-using-the-thread-pool.md)
* [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md)
 

 