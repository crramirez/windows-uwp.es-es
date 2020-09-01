---
title: Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime
description: La compatibilidad de .NET con los componentes de Windows Runtime facilita la declaración de componentes de eventos ocultando las diferencias entre el patrón de eventos Plataforma universal de Windows (UWP) y el patrón de eventos de .NET.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b1666d938a83f8c8725523d3e5a1b14e416ca4e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174279"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime

La compatibilidad de .NET con los componentes de Windows Runtime facilita la declaración de componentes de eventos ocultando las diferencias entre el patrón de eventos Plataforma universal de Windows (UWP) y el patrón de eventos de .NET. Sin embargo, cuando declare descriptores de acceso de eventos personalizados en un componente de Windows Runtime, debe seguir el patrón usado en el UWP.

## <a name="registering-events"></a>Registrando eventos

Al registrar para controlar un evento en la UWP, el descriptor de acceso "add" devuelve un token. Para anular el registro, debe pasar este token al descriptor de acceso "remove". Esto significa que los descriptores de acceso "add" y "remove" para eventos de la UWP no tienen las mismas firmas que los descriptores de acceso habituales.

Afortunadamente, los compiladores de Visual Basic y C# simplifican este proceso: cuando se declara un evento con descriptores de acceso personalizados en un componente de Windows Runtime, los compiladores usan automáticamente el patrón de UWP. Por ejemplo, aparecerá un error del compilador si tu descriptor de acceso "add" no devuelve un token. .NET proporciona dos tipos para admitir la implementación:

-   La estructura [EventRegistrationToken](/uwp/api/windows.foundation.eventregistrationtoken) representa el token.
-   La clase [EventRegistrationTokenTable&lt;T&gt;](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1) crea tokens y mantiene una correlación entre los tokens y los controladores de eventos. El argumento de tipo genérico es el tipo de argumento de evento. Creas una instancia de esta clase para cada evento la primera vez que se registra un controlador de eventos para ese evento.

El siguiente código para el evento NumberChanged muestra el patrón básico para eventos de la UWP. En este ejemplo, el constructor para el objeto "event argument", NumberChangedEventArgs, usa un solo parámetro de número entero que representa el valor numérico modificado.

> **Nota:**    Este es el mismo patrón que los compiladores usan para los eventos ordinarios que se declaran en un componente de Windows Runtime.

 
> [!div class="tabbedCodeSnippets"]
> ```csharp
> private EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>     m_NumberChangedTokenTable = null;
>
> public event EventHandler<NumberChangedEventArgs> NumberChanged
> {
>     add
>     {
>         return EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .AddEventHandler(value);
>     }
>     remove
>     {
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>             .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>             .RemoveEventHandler(value);
>     }
> }
>
> internal void OnNumberChanged(int newValue)
> {
>     EventHandler<NumberChangedEventArgs> temp =
>         EventRegistrationTokenTable<EventHandler<NumberChangedEventArgs>>
>         .GetOrCreateEventRegistrationTokenTable(ref m_NumberChangedTokenTable)
>         .InvocationList;
>     if (temp != null)
>     {
>         temp(this, new NumberChangedEventArgs(newValue));
>     }
> }
> ```
> ```vb
> Private m_NumberChangedTokenTable As  _
>     EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs))
>
> Public Custom Event NumberChanged As EventHandler(Of NumberChangedEventArgs)
>
>     AddHandler(ByVal handler As EventHandler(Of NumberChangedEventArgs))
>         Return EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             AddEventHandler(handler)
>     End AddHandler
>
>     RemoveHandler(ByVal token As EventRegistrationToken)
>         EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             RemoveEventHandler(token)
>     End RemoveHandler
>
>     RaiseEvent(ByVal sender As Class1, ByVal args As NumberChangedEventArgs)
>         Dim temp As EventHandler(Of NumberChangedEventArgs) = _
>             EventRegistrationTokenTable(Of EventHandler(Of NumberChangedEventArgs)).
>             GetOrCreateEventRegistrationTokenTable(m_NumberChangedTokenTable).
>             InvocationList
>         If temp IsNot Nothing Then
>             temp(sender, args)
>         End If
>     End RaiseEvent
> End Event
> ```

El método ("Shared" en Visual Basic) GetOrCreateEventRegistrationTokenTable estático crea la instancia del evento del objeto EventRegistrationTokenTable&lt;T&gt; de forma diferida. Pasa el campo de nivel de clase que contendrá la instancia de la tabla de tokens a este método. Si el campo está vacío, el método crea la tabla, almacena una referencia en la tabla del campo y devuelve una referencia a la tabla. Si el campo ya contiene una referencia a la tabla de tokens, el método devuelve solo esa referencia.

> **Importante**    Para garantizar la seguridad para subprocesos, el campo que contiene la instancia del evento de EventRegistrationTokenTable &lt; T &gt; debe ser un campo de nivel de clase. Si se trata de un campo de nivel de clase, el método GetOrCreateEventRegistrationTokenTable garantiza que cuando varios subprocesos intentan crear la tabla de tokens, todos los subprocesos obtienen la misma instancia de la tabla. En el caso de un evento determinado, todas las llamadas al método GetOrCreateEventRegistrationTokenTable deben utilizar el mismo campo de nivel de clase.

Llamar al método GetOrCreateEventRegistrationTokenTable en el descriptor de acceso "remove" y en el método [RaiseEvent](/dotnet/articles/visual-basic/language-reference/statements/raiseevent-statement) (método OnRaiseEvent en C#) garantiza que no se produce ninguna excepción si estos métodos se llaman antes de que se hayan agregado los delegados del controlador de eventos.

Los otros miembros de la clase EventRegistrationTokenTable&lt;T&gt; que se utilizan en el patrón de eventos de la UWP incluyen lo siguiente:

-   El método [AddEventHandler](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.addeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_AddEventHandler__0_) genera un token para el delegado del controlador de eventos, almacena el delegado en la tabla, lo agrega a la lista de invocación y devuelve el token.
-   La sobrecarga del método [RemoveEventHandler(EventRegistrationToken)](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.removeeventhandler#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_RemoveEventHandler_System_Runtime_InteropServices_WindowsRuntime_EventRegistrationToken_) elimina el delegado de la tabla y de la lista de invocación.

    >**Nota:**    Los métodos AddEventHandler y RemoveEventHandler (EventRegistrationToken) bloquean la tabla para ayudar a garantizar la seguridad para subprocesos.

-   La propiedad [InvocationList](/dotnet/api/system.runtime.interopservices.windowsruntime.eventregistrationtokentable-1.invocationlist#System_Runtime_InteropServices_WindowsRuntime_EventRegistrationTokenTable_1_InvocationList) devuelve un delegado que incluye todos los controladores de eventos que están registrados actualmente para controlar el evento. Usa este delegado para generar el evento o usa los métodos de la clase delegada para invocar los controladores de forma individual.

    >**Nota:**    Se recomienda seguir el patrón que se muestra en el ejemplo que se ha proporcionado anteriormente en este artículo y copiar el delegado en una variable temporal antes de invocarlo. Esto evita una condición de carrera en la que un subproceso elimina el último controlador, lo que reduce el delegado a nulo justo antes de que otro subproceso intente invocar el delegado. Los delegados son inmutables, por lo que la copia sigue siendo válida.

Coloca tu propio código en los descriptores de acceso según corresponda. Si la seguridad de los subprocesos es un problema, debes proporcionar tu propio bloqueo para que el código.

Usuarios de C#: cuando escribes descriptores de acceso de eventos personalizados en el patrón de eventos de la UWP, el compilador no proporciona los accesos directos sintácticos habituales. Genera errores si utilizas el nombre del evento en tu código.

Visual Basic usuarios: en .NET, un evento es simplemente un delegado de multidifusión que representa todos los controladores de eventos registrados. Generar el evento simplemente implica invocar el delegado. La sintaxis de Visual Basic, por lo general, oculta las interacciones con el delegado, y el compilador copia el delegado antes de invocarlo, tal como se describe en la nota sobre seguridad para subprocesos. Cuando crea un evento personalizado en un componente de Windows Runtime, tiene que tratar directamente con el delegado. Esto también significa que puedes, por ejemplo, usar el método [MulticastDelegate.GetInvocationList](/dotnet/api/system.multicastdelegate.getinvocationlist#System_MulticastDelegate_GetInvocationList) para obtener una matriz que contenga un delegado independiente para cada controlador de eventos, si quieres invocar los controladores por separado.

## <a name="related-topics"></a>Temas relacionados

* [Eventos (Visual Basic)](/dotnet/articles/visual-basic/programming-guide/language-features/events/index)
* [Eventos (Guía de programación de C#)](/dotnet/articles/csharp/programming-guide/events/index)
* [Información general de .NET para aplicaciones para UWP](/previous-versions/windows/apps/br230302(v=vs.140))
* [.NET para aplicaciones UWP](/dotnet/api/index?view=dotnet-uwp-10.0)
* [Tutorial para crear un componente de Windows Runtime de C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)