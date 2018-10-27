---
author: normesta
ms.assetid: 1B077801-0A58-4A34-887C-F1E85E9A37B0
title: Crear un elemento de trabajo periódico
description: Obtén información sobre cómo crear un elemento de trabajo que se repita periódicamente.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, elemento de trabajo periódico, subprocesos, temporizadores
ms.localizationpriority: medium
ms.openlocfilehash: 4afa137b01738c42f8e15c95ef09ec921d1e44ae
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5702593"
---
# <a name="create-a-periodic-work-item"></a>Crear un elemento de trabajo periódico


** API importantes **

-   [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915)
-   [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587)

Obtén información sobre cómo crear un elemento de trabajo que se repita periódicamente.

## <a name="create-the-periodic-work-item"></a>Crear el elemento de trabajo periódico

Usa el método [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) para crear un elemento de trabajo periódico. Envía un lambda que realice el trabajo y usa el parámetro *period* para especificar el intervalo entre los envíos. El período se especifica con una estructura [**TimeSpan**](https://msdn.microsoft.com/library/windows/apps/BR225996). El elemento de trabajo se volverá a enviar cada vez que transcurra el período, de manera que debes asegurarte de que el período sea lo suficientemente largo como para que se complete el trabajo.

[**CreateTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.createtimer.aspx) devuelve un objeto [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/BR230587). Almacena este objeto en caso de que se deba cancelar el temporizador.

> **Nota**evita especificar un valor de cero (o cualquier valor inferior a 1 milisegundo) para el intervalo. Esto hará que el temporizador periódico se comporte como un temporizador de único disparo.

> **Nota** [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) puedes usar para acceder a la interfaz de usuario y mostrar el progreso del elemento de trabajo.

En el siguiente ejemplo se crea un elemento de trabajo que se ejecuta una vez cada 60 segundos:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> TimeSpan period = TimeSpan.FromSeconds(60);
>
> ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>
>     }, period);
> ```
> ``` cpp
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }), period);
> ```

## <a name="handle-cancellation-of-the-periodic-work-item-optional"></a>Controlar la cancelación del elemento de trabajo periódico (opcional)

Si es necesario, puedes controlar la cancelación del temporizador periódico con un elemento [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926). Usa la sobrecarga [**CreatePeriodicTimer**](https://msdn.microsoft.com/library/windows/apps/Hh967915) para proporcionar un lambda adicional que controle la cancelación del elemento de trabajo periódico.

En el siguiente ejemplo se crea un elemento de trabajo periódico que se repite cada 60 segundos y también proporciona un controlador de cancelación:

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> using Windows.System.Threading;
>
>     TimeSpan period = TimeSpan.FromSeconds(60);
>
>     ThreadPoolTimer PeriodicTimer = ThreadPoolTimer.CreatePeriodicTimer((source) =>
>     {
>         //
>         // TODO: Work
>         //
>         
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher.RunAsync(CoreDispatcherPriority.High,
>             () =>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //
>
>             });
>     },
>     period,
>     (source) =>
>     {
>         //
>         // TODO: Handle periodic timer cancellation.
>         //
>
>         //
>         // Update the UI thread by using the UI core dispatcher.
>         //
>         Dispatcher->RunAsync(CoreDispatcherPriority.High,
>             ()=>
>             {
>                 //
>                 // UI components can be accessed within this scope.
>                 //                 
>
>                 // Periodic timer cancelled.
>
>             }));
>     });
> ```
> ``` cpp
> using namespace Windows::System::Threading;
> using namespace Windows::UI::Core;
>
> TimeSpan period;
> period.Duration = 60 * 10000000; // 10,000,000 ticks per second
>
> ThreadPoolTimer ^ PeriodicTimer = ThreadPoolTimer::CreatePeriodicTimer(
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
>                 }));
>
>         }),
>         period,
>         ref new TimerDestroyedHandler([&](ThreadPoolTimer ^ source)
>         {
>             //
>             // TODO: Handle periodic timer cancellation.
>             //
>
>             Dispatcher->RunAsync(CoreDispatcherPriority::High,
>                 ref new DispatchedHandler([&]()
>                 {
>                     //
>                     // UI components can be accessed within this scope.
>                     //
>
>                     // Periodic timer cancelled.
>
>                 }));
>         }));
> ```

## <a name="cancel-the-timer"></a>Cancelar el temporizador

Cuando sea necesario, llama al método [**Cancel**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.cancel.aspx) para interrumpir la repetición del elemento de trabajo periódico. Si el elemento de trabajo se está ejecutando cuando se cancela el temporizador periódico, puede completarse. Cuando se han completado todas las instancias del elemento de trabajo periódico, se llama a [**TimerDestroyedHandler**](https://msdn.microsoft.com/library/windows/apps/Hh967926) (si existe).

> [!div class="tabbedCodeSnippets"]
> ``` csharp
> PeriodicTimer.Cancel();
> ```
> ``` cpp
> PeriodicTimer->Cancel();
> ```

## <a name="remarks"></a>Observaciones

Para obtener información sobre los temporizadores de un solo uso, consulta [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md).

## <a name="related-topics"></a>Temas relacionados

* [Enviar un elemento de trabajo al grupo de subprocesos](submit-a-work-item-to-the-thread-pool.md)
* [Procedimientos recomendados para usar el grupo de subprocesos](best-practices-for-using-the-thread-pool.md)
* [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md)
 
