---
title: Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime
description: La compatibilidad de .NET Framework con los componentes de Windows Runtime facilita la tarea de declarar componentes de eventos al ocultar las diferencias entre el patrón de eventos de la plataforma universal de Windows (UWP) y el patrón de eventos de .NET Framework.
ms.assetid: 6A66D80A-5481-47F8-9499-42AC8FDA0EB4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: b8c4777e1c34bca36200bf6e8a96c35d6a0b1079
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "8872111"
---
# <a name="custom-events-and-event-accessors-in-windows-runtime-components"></a>Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime



La compatibilidad de .NET Framework con los componentes de Windows Runtime facilita la tarea de declarar componentes de eventos al ocultar las diferencias entre el patrón de eventos de la Plataforma universal de Windows (UWP) y el patrón de eventos de .NET Framework. Sin embargo, al declarar descriptores de acceso de eventos personalizados en un componente de Windows Runtime, debe seguir el patrón usado en la UWP.

## <a name="registering-events"></a>Registrando eventos


Al registrar para controlar un evento en la UWP, el descriptor de acceso "add" devuelve un token. Para anular el registro, debe pasar este token al descriptor de acceso "remove". Esto significa que los descriptores de acceso "add" y "remove" para eventos de la UWP no tienen las mismas firmas que los descriptores de acceso habituales.

Por suerte, los compiladores de Visual Basic y C# simplifican este proceso: cuando declaras un evento con descriptores de acceso personalizados en un componente de Windows Runtime, los compiladores utilizan automáticamente el patrón de la UWP. Por ejemplo, aparecerá un error del compilador si tu descriptor de acceso "add" no devuelve un token. .NET Framework proporciona dos tipos para facilitar la implementación:

-   La estructura [EventRegistrationToken](https://msdn.microsoft.com/library/windows/apps/windows.foundation.eventregistrationtoken.aspx) representa el token.
-   La clase [EventRegistrationTokenTable&lt;T&gt;](https://msdn.microsoft.com/library/hh138412.aspx) crea tokens y mantiene una correlación entre los tokens y los controladores de eventos. El argumento de tipo genérico es el tipo de argumento de evento. Creas una instancia de esta clase para cada evento la primera vez que se registra un controlador de eventos para ese evento.

El siguiente código para el evento NumberChanged muestra el patrón básico para eventos de la UWP. En este ejemplo, el constructor para el objeto "event argument", NumberChangedEventArgs, usa un solo parámetro de número entero que representa el valor numérico modificado.

> **Nota**este es el mismo patrón que usan los compiladores para los eventos habituales que Declaras en un componente de Windows Runtime.

 
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

> **Importante**para garantizar que los subprocesos, el campo que contiene la instancia del evento de EventRegistrationTokenTable&lt;T&gt; debe ser un campo de nivel de clase. Si se trata de un campo de nivel de clase, el método GetOrCreateEventRegistrationTokenTable garantiza que cuando varios subprocesos intentan crear la tabla de tokens, todos los subprocesos obtienen la misma instancia de la tabla. En el caso de un evento determinado, todas las llamadas al método GetOrCreateEventRegistrationTokenTable deben utilizar el mismo campo de nivel de clase.

Llamar al método GetOrCreateEventRegistrationTokenTable en el descriptor de acceso "remove" y en el método [RaiseEvent](https://msdn.microsoft.com/library/fwd3bwed.aspx) (método OnRaiseEvent en C#) garantiza que no se produce ninguna excepción si estos métodos se llaman antes de que se hayan agregado los delegados del controlador de eventos.

Los otros miembros de la clase EventRegistrationTokenTable&lt;T&gt; que se utilizan en el patrón de eventos de la UWP incluyen lo siguiente:

-   El método [AddEventHandler](https://msdn.microsoft.com/library/hh138458.aspx) genera un token para el delegado del controlador de eventos, almacena el delegado en la tabla, lo agrega a la lista de invocación y devuelve el token.
-   La sobrecarga del método [RemoveEventHandler(EventRegistrationToken)](https://msdn.microsoft.com/library/hh138425.aspx) elimina el delegado de la tabla y de la lista de invocación.

    >**Nota**en la tabla para ayudar a garantizar la seguridad de los subprocesos de bloqueo de los métodos AddEventHandler y RemoveEventHandler (eventregistrationtoken).

-   La propiedad [InvocationList](https://msdn.microsoft.com/library/hh138465.aspx) devuelve un delegado que incluye todos los controladores de eventos que están registrados actualmente para controlar el evento. Usa este delegado para generar el evento o usa los métodos de la clase delegada para invocar los controladores de forma individual.

    >**Nota**te recomendamos que sigas el patrón del ejemplo proporcionado anteriormente en este artículo y copia el delegado en una variable temporal antes de invocarlo. Esto evita una condición de carrera en la que un subproceso elimina el último controlador, lo que reduce el delegado a nulo justo antes de que otro subproceso intente invocar el delegado. Los delegados son inmutables, por lo que la copia sigue siendo válida.

Coloca tu propio código en los descriptores de acceso según corresponda. Si la seguridad de los subprocesos es un problema, debes proporcionar tu propio bloqueo para que el código.

Usuarios de C#: cuando escribes descriptores de acceso de eventos personalizados en el patrón de eventos de la UWP, el compilador no proporciona los accesos directos sintácticos habituales. Genera errores si utilizas el nombre del evento en tu código.

Usuarios de Visual Basic: en .NET Framework, un evento es simplemente un delegado de multidifusión que representa todos los controladores de eventos registrados. Generar el evento simplemente implica invocar el delegado. La sintaxis de Visual Basic, por lo general, oculta las interacciones con el delegado, y el compilador copia el delegado antes de invocarlo, tal como se describe en la nota sobre seguridad para subprocesos. Cuando creas un evento personalizado en un componente de Windows Runtime, tienes que tratar directamente con el delegado. Esto también significa que puedes, por ejemplo, usar el método [MulticastDelegate.GetInvocationList](https://msdn.microsoft.com/library/system.multicastdelegate.getinvocationlist.aspx) para obtener una matriz que contenga un delegado independiente para cada controlador de eventos, si quieres invocar los controladores por separado.

## <a name="related-topics"></a>Temas relacionados

* [Eventos (Visual Basic)](https://msdn.microsoft.com/library/ms172877.aspx)
* [Eventos (Guía de programación de C#)](https://msdn.microsoft.com/library/awbftdfh.aspx)
* [.NET para introducción de las aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/br230302.aspx)
* [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501.aspx)
* [Tutorial: Creación de un componente simple de Windows Runtime y llamada al mismo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
