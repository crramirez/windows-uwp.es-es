---
ms.assetid: FA25562A-FE62-4DFC-9084-6BD6EAD73636
title: Mantener la capacidad de respuesta del subproceso de la interfaz de usuario
description: Los usuarios esperan que las aplicaciones sigan respondiendo mientras realizan cálculos, independientemente del tipo de equipo.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0bc555030c2f5202e5c128c1d1a2fe45b5b71b4b
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8347432"
---
# <a name="keep-the-ui-thread-responsive"></a>Mantener la capacidad de respuesta del subproceso de la interfaz de usuario


Los usuarios esperan que las aplicaciones sigan respondiendo mientras realizan cálculos, independientemente del tipo de equipo. Esto significa cosas distintas en función de la aplicación. En el caso de algunas aplicaciones, esto se traduce en la necesidad de proporcionar una física más realista, cargar los datos desde el disco o desde la Web con mayor rapidez, presentar escenas complejas y navegar entre páginas velozmente, encontrar direcciones al instante o procesar datos con rapidez. Independientemente del tipo de cálculo, los usuarios quieren que las aplicaciones respondan a sus entrada y que no haya casos en que estas parezcan que dejen de responder mientras “están pensando”.

La aplicación se controla mediante eventos, lo que significa que el código realiza el trabajo en respuesta a eventos y, a continuación, permanece inactivo hasta el siguiente evento. El código de la plataforma de la interfaz de usuario (diseño, entradas, generación de eventos, etc.) y el código de la aplicación de la interfaz de usuario se ejecutan en el mismo subproceso de interfaz de usuario. Solo una instrucción puede ejecutarse en dicho subproceso a la vez, por lo que si el código de la aplicación tarda demasiado en procesar un evento, el marco no podrá ejecutar el diseño ni generar nuevos eventos de interacción del usuario. La capacidad de respuesta de tu aplicación depende de la disponibilidad del subproceso de la interfaz de usuario para procesar trabajo.

Deberás usar el subproceso de la interfaz de usuario para que realice casi todos los cambios en el subproceso de la interfaz de usuario, incluida la creación de tipos de interfaz de usuario y el acceso a sus miembros. La interfaz de usuario no se puede actualizar desde un subproceso, aunque puedes publicar un mensaje con [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) para hacer que el código se ejecute allí.

> **Nota**la única excepción es que hay un subproceso de representación separado que puede aplicar cambios de la interfaz de usuario que no afectan a cómo se controla la entrada o el diseño básico. Por ejemplo, muchas animaciones y transiciones que no afectan al diseño pueden ejecutarse en este subproceso de representación.

## <a name="delay-element-instantiation"></a>Retrasar la creación de instancias de elementos

Entre las fases más lentas de una aplicación se incluyen el inicio y el cambio de vistas. No hagas más de lo necesario para que se muestre la interfaz de usuario que el usuario verá inicialmente. Por ejemplo, no crees la interfaz de usuario para que se muestre de manera progresiva y para mostrar el contenido de elementos emergentes.

-   Usa [x:Load attribute](../xaml-platform/x-load-attribute.md) o [x: DeferLoadStrategy](https://msdn.microsoft.com/library/windows/apps/Mt204785) para retrasar la creación de instancias de los elementos.
-   Inserta mediante programación los elementos en el árbol a petición.

[**CoreDispatcher.RunIdleAsync**](https://msdn.microsoft.com/library/windows/apps/Hh967918) pone el trabajo en cola para que el subproceso de la interfaz de usuario lo procese cuando no esté ocupado.

## <a name="use-asynchronous-apis"></a>Usar API asincrónicas

Para ayudar a mantener la capacidad de respuesta de la aplicación, la plataforma proporciona versiones asincrónicas de muchas de las API que usa. Una API asincrónica asegura que el subproceso de ejecución activa nunca se bloquee durante un período de tiempo largo. Cuando llames a una API desde el subproceso de interfaz de usuario, usa la versión asincrónica si está disponible. Para obtener más información sobre cómo programar con patrones **async**, consulta [Programación asincrónica](https://msdn.microsoft.com/library/windows/apps/Mt187335) o el tema [Llamada a API asincrónicas en C# o Visual Basic](https://msdn.microsoft.com/library/windows/apps/Mt187337).

## <a name="offload-work-to-background-threads"></a>Descargar el trabajo a subprocesos en segundo plano

Programa los controladores de eventos para volver rápidamente. En los casos en los que se deba realizar una cantidad de trabajo no trivial, prográmalo en un subproceso en segundo plano que vuelva.

Puedes programar el trabajo de manera asincrónica mediante el operador **await** en C#, el operador **Await** en Visual Basic o delegados en C++. Pero esto no garantiza que el trabajo que programes se ejecutará en un subproceso en segundo plano. Muchas de las API de la Plataforma universal de Windows (UWP) programan el trabajo en el subproceso en segundo plano automáticamente, pero si llamas al código de tu aplicación usando únicamente **await** o un delegado, ejecutarás dicho método o delegado en el subproceso de interfaz de usuario. Debes indicar de manera explícita cuándo quieres ejecutar el código de tu aplicación en un subproceso en segundo plano. En C# y Visual Basic puedes hacerlo pasando código a [**Task.Run**](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.task.run.aspx).

Recuerda que solo se puede tener acceso a los elementos de la interfaz de usuario desde el subproceso de interfaz de usuario. Usa el subproceso de interfaz de usuario para tener acceso a los elementos de interfaz de usuario antes de iniciar el trabajo en segundo plano, o bien usa [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) o [**CoreDispatcher.RunIdleAsync**](https://msdn.microsoft.com/library/windows/apps/Hh967918) en el subproceso en segundo plano.

Un ejemplo de trabajo que puede ejecutarse en un subproceso en segundo plano es el cálculo de IA en un juego. El código que calcula el próximo movimiento del equipo puede tardar mucho en ejecutarse.

```csharp
public class AsyncExample
{
    private async void NextMove_Click(object sender, RoutedEventArgs e)
    {
        // The await causes the handler to return immediately.
        await System.Threading.Tasks.Task.Run(() => ComputeNextMove());
        // Now update the UI with the results.
        // ...
    }

    private async System.Threading.Tasks.Task ComputeNextMove()
    {
        // Perform background work here.
        // Don't directly access UI elements from this method.
    }
}
```

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public class Example
> {
>     // ...
>     private async void NextMove_Click(object sender, RoutedEventArgs e)
>     {
>         await Task.Run(() => ComputeNextMove());
>         // Update the UI with results
>     }
> 
>     private async Task ComputeNextMove()
>     {
>         // ...
>     }
>     // ...
> }
> ```
> ```vb
> Public Class Example
>     ' ...
>     Private Async Sub NextMove_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
>         Await Task.Run(Function() ComputeNextMove())
>         ' update the UI with results
>     End Sub
> 
>     Private Async Function ComputeNextMove() As Task
>         ' ...
>     End Function
>     ' ...
> End Class
> ```

En este ejemplo, el controlador `NextMove_Click` vuelve a **await** para mantener la capacidad de respuesta del subproceso de la interfaz de usuario. Sin embargo, la ejecución se retoma en dicho controlador después de completar `ComputeNextMove` (que se ejecuta en un subproceso en segundo plano). El código restante del controlador actualiza la interfaz de usuario con los resultados.

> **Nota**también existe una API de [**grupo de subprocesos**](https://msdn.microsoft.com/library/windows/apps/BR229621) y [**ThreadPoolTimer**](https://msdn.microsoft.com/library/windows/apps/windows.system.threading.threadpooltimer.aspx) para la UWP, que puede usarse para escenarios similares. Para obtener más información, consulta [Subprocesamiento y programación asincrónica](https://msdn.microsoft.com/library/windows/apps/Mt187340).

## <a name="related-topics"></a>Temas relacionados

* [Interacciones del usuario personalizadas](https://msdn.microsoft.com/library/windows/apps/Mt185599)
