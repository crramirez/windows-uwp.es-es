---
title: Crear componentes de Windows Runtime en C# y Visual Basic
description: A partir de .NET Framework 4.5, puedes usar código administrado para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5a7f2d2db5670b0102f589fcd6d764a239d3bb3f
ms.sourcegitcommit: b975c8fc8cf0770dd73d8749733ae5636f2ee296
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9058826"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Crear componentes de Windows Runtime en C# y Visual Basic
A partir de .NET Framework 4.5, puedes usar código administrado para crear tus propios tipos de Windows Runtime y empaquetarlos en un componente de Windows Runtime. Puedes usar tu componente en las aplicaciones de la plataforma Universal de Windows (UWP) que se escriben en C++, JavaScript, Visual Basic o C#. En este tema se describe las reglas para crear un componente y se describe algunos aspectos de la compatibilidad de .NET Framework para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET Framework. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime.

Si vas a crear un componente para su uso solo en aplicaciones para UWP que se escriben en C# o Visual Basic, y el componente no contiene controles UWP, a continuación, onsider con la plantilla de la **Biblioteca de clases** en lugar de la plantilla de proyecto de **Componente de Windows Runtime** en Microsoft Visual Studio. Hay menos restricciones en una biblioteca de clase simple.

## <a name="declaring-types-in-windows-runtime-components"></a>Declarar tipos en componentes de Windows Runtime

Internamente, los tipos de Windows Runtime en tu componente pueden usar cualquier funcionalidad de .NET Framework permitida en una aplicación para UWP. Para obtener más información, consulta [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/mt185501).

Externamente, los miembros de los tipos pueden exponer solo los tipos de Windows Runtime para sus parámetros y valores devueltos. La siguiente lista describe las limitaciones sobre los tipos de .NET Framework que se exponen desde un componente de Windows Runtime.

- Los campos, los parámetros y los valores devueltos de todos los tipos públicos y miembros en tu componente deben ser tipos de Windows Runtime. Esta restricción incluye los tipos de Windows Runtime que creas, así como los tipos proporcionados por el propio Windows Runtime. También incluye varios tipos de .NET Framework. La inclusión de estos tipos forma parte de la compatibilidad de .NET Framework proporciona para habilitar el uso natural de Windows Runtime en código administrado&mdash;tu código aparentemente usa los tipos de .NET Framework familiares en lugar de los tipos de Windows Runtime subyacentes. Por ejemplo, puedes usar tipos primitivos de .NET Framework como **Int32** y **Double**, ciertos tipos fundamentales como **DateTimeOffset** y **Uri**, y algunos tipos de interfaz genérica como **IEnumerable habitualmente&lt;T &gt; ** (IEnumerable (Of T) en Visual Basic) y **IDictionary&lt;TKey, TValue&gt;**. Ten en cuenta que los argumentos de tipo de estos tipos genéricos deben ser tipos de Windows Runtime. Esto se explica en las secciones [tipos de aprobación de Windows Runtime a código administran](#passing-windows-runtime-types-to-managed-code) y [pasar tipos administrados a Windows Runtime](#passing-managed-types-to-the-windows-runtime), más adelante en este tema.

- Las interfaces y clases públicas pueden contener métodos, propiedades y eventos. Puedes declarar a delegados para tus eventos o usar el **EventHandler&lt;T&gt; ** delegado. Una clase o interfaz pública no puede:
    - Ser genérica.
    - Implementar una interfaz que no es una interfaz de Windows Runtime (sin embargo, puedes crear tus propias interfaces de Windows Runtime e implementarlos).
    - Derivar de tipos que no están en el tiempo de ejecución de Windows, como **System.Exception** y **System.EventArgs**.

- Todos los tipos públicos deben tener un espacio de nombres raíz que coincida con el nombre del ensamblado y el nombre del ensamblado no puede empezar por "Windows".

    > **Sugerencia**. De manera predeterminada, los proyectos de Visual Studio tienen nombres de espacio de nombres que coinciden con el nombre del ensamblado. En Visual Basic, la declaración de espacio de nombres para este espacio de nombres predeterminado no se muestra en tu código.

- Las estructuras públicas no pueden tener miembros que no sean campos públicos y los campos deben ser tipos de valor o cadenas.
- Las clases públicas deben ser **sealed** (**NotInheritable** en Visual Basic). Si el modelo de programación requiere polimorfismo, a continuación, puedes crear una interfaz pública e implementar esta interfaz en las clases que deben ser polimórficas.

## <a name="debugging-your-component"></a>Depurar tu componente

Si tu aplicación para UWP y el componente se compilan con código administrado, a continuación, puedes depurarlos ambos al mismo tiempo.

Cuando pruebas tu componente como parte de una aplicación para UWP con C++, puedes depurar código administrado y nativo al mismo tiempo. De forma predeterminada, es solo código nativo.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar tanto código C++ nativo como código administrado
1.  Abre el menú contextual para tu proyecto de Visual C++ y elige **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Solo nativo** por **Mixto (administrado y nativo)**. Elige **Aceptar**.
4.  Establecer puntos de interrupción en el código nativo y administrado.

Cuando pruebas tu componente como parte de una aplicación para UWP con JavaScript, de forma predeterminada la solución está en modo de depuración de JavaScript. En Visual Studio, no puedes depurar código administrado y JavaScript al mismo tiempo.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código administrado en lugar de JavaScript
1.  Abre el menú contextual de tu proyecto de JavaScript y selecciona **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Script solo** por **Sólo administrado**. Elige **Aceptar**.
4.  Establecer puntos de interrupción en código administrado y depurar de la forma habitual.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Pasar los tipos de Windows Runtime a código administrado
Como se mencionó anteriormente en la sección [declarar tipos en componentes de Windows Runtime](#declaring-types-in-windows-runtime-components), ciertos tipos de .NET Framework pueden aparecer en las firmas de miembros de las clases públicas. Esto forma parte de la compatibilidad que proporciona .NET Framework para habilitar el uso natural de Windows Runtime en código administrado. Incluye tipos primitivos y algunas clases e interfaces. Cuando se usa tu componente desde JavaScript o desde el código de C++, es importante saber cómo aparecen tus tipos de .NET Framework para el llamador. Consulta [Tutorial: crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obtener ejemplos con JavaScript. En este apartado se describen los tipos usados habitualmente.

En .NET Framework, los tipos primitivos como la estructura **Int32** tienen muchas propiedades y métodos útiles, como el método **TryParse** . Por el contrario, las estructuras y los tipos primitivos en Windows Runtime solo tienen campos. Cuando pasas estos tipos a código administrado, parecen tipos de .NET Framework y puedes usar las propiedades y métodos de los tipos de .NET Framework como lo harías normalmente. En la siguiente lista se resumen las sustituciones que se realizan automáticamente en el IDE:

-   Para los primitivos de Windows Runtime **Int32**, **Int64**, **Single**, **Double**, **Boolean**, **cadena** (una colección inmutable de caracteres Unicode), **Enum**, **UInt32**, **UInt64**y **Guid **, usa el tipo del mismo nombre en el espacio de nombres del sistema.
-   Para **UInt8**, utiliza **System.Byte**.
-   Para **Char16**, utiliza **System.Char**.
-   Para la interfaz **IInspectable** , utiliza **System.Object**.

Si C# o Visual Basic proporciona una palabra clave del lenguaje para alguno de estos tipos, a continuación, puedes usar la palabra clave del lenguaje en su lugar.

Además de los tipos primitivos, algunos tipos básicos de Windows Runtime de uso habitual aparecen en código administrado como sus equivalentes de .NET Framework. Por ejemplo, supongamos que el código JavaScript usa la clase **Windows.Foundation.Uri** y quieres pasarlo a un método C# o Visual Basic. El tipo equivalente en código administrado es la clase **System.Uri** de .NET Framework y ese es el tipo que se usará para el parámetro del método. Puedes saber cuándo un tipo de Windows Runtime aparece como un tipo de .NET Framework, porque IntelliSense en Visual Studio oculta el tipo de Windows Runtime cuando estás escribiendo código administrado y presenta el tipo de .NET Framework equivalente. (Normalmente los dos tipos tienen el mismo nombre. Sin embargo, ten en cuenta que la estructura **Windows.Foundation.DateTime** aparece en código administrado como **System.DateTimeOffset** y no como **System.DateTime**).

Para algunos tipos de colección usados con frecuencia, la asignación es entre las interfaces implementadas por un tipo de Windows Runtime y las interfaces implementadas por el tipo de .NET Framework correspondiente. Del mismo modo que con los tipos mencionados anteriormente, declaras tipos de parámetros mediante el uso del tipo de .NET Framework. Este oculta algunas diferencias entre los tipos y logra que la escritura de .NET Framework sea más natural.

En la siguiente tabla se enumeran los tipos de interfaz genérica más frecuentes, junto con otras asignaciones de clase e interfaz habituales. Para obtener una lista completa de los tipos de Windows Runtime que asigna .NET Framework, consulta [las asignaciones de .NET Framework de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

| Windows Runtime                                  | .NET Framework                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

Cuando un tipo implementa más de una interfaz, puedes usar cualquiera de las interfaces que implementa como un tipo de parámetro o tipo devuelto de un miembro. Por ejemplo, puedes pasar o devolver un **diccionario&lt;int, string&gt; ** (**Dictionary (Of Integer, String)** en Visual Basic) como **IDictionary&lt;int, string&gt;**, **IReadOnlyDictionary&lt;int, string &gt; **, o **IEnumerable&lt;System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;**.

> [!IMPORTANT]
> JavaScript usa la interfaz que aparece en primer lugar en la lista de interfaces que implementa un tipo administrado. Por ejemplo, si devuelves **diccionario&lt;int, string&gt; ** al código JavaScript, aparece como **IDictionary&lt;int, string&gt; ** independientemente de qué interfaz especifiques como el tipo devuelto. Esto significa que si la primera interfaz no incluye a un miembro que aparece en las últimas interfaces, ese miembro no es visible para JavaScript.

En Windows Runtime, **IMap&lt;K, V&gt; ** y **IMapView&lt;K, V&gt; ** se recorren en iteración mediante el uso de IKeyValuePair. Cuando pasas a código administrado, aparecen como **IDictionary&lt;TKey, TValue&gt; ** y **IReadOnlyDictionary&lt;TKey, TValue&gt;**, por lo tanto, naturalmente, utilizas System.Collections.Generic.KeyValuePair **&lt;TKey, TValue&gt; ** para enumerarlos.

La forma en la que las interfaces aparecen en código administrado afecta a la forma en que aparecen los tipos que implementan estas interfaces. Por ejemplo, la clase **PropertySet** implementa **IMap&lt;K, V&gt;**, que aparece en código administrado como **IDictionary&lt;TKey, TValue&gt;**. **PropertySet** aparece como si implementara **IDictionary&lt;TKey, TValue&gt; ** en lugar de **IMap&lt;K, V&gt;**, de modo que en código administrado, parece tener un método **Add** , que se comporta como el método **Add** en los diccionarios de .NET Framework. No parece tener un método **Insert** . Puedes ver este ejemplo en el tema [Tutorial: crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Pasar los tipos administrados a Windows Runtime

Como se explicó en el apartado anterior, algunos tipos de Windows Runtime pueden aparecer como tipos de .NET Framework en las signaturas de los miembros de tu componente o en las signaturas de miembros de Windows Runtime cuando las utilizas en el IDE. Al pasar los tipos de .NET Framework a estos miembros o al usarlos como valores devueltos de los miembros de tu componente, aparecen en el código en el otro lado, como el tipo de Windows Runtime correspondiente. Para ver ejemplos de los efectos que esto puede tener cuando el componente se llama desde JavaScript, consulta el apartado "Devolver tipos administrados desde tu componente" en [Tutorial: crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecargados

En Windows Runtime, los métodos se pueden sobrecargar. Sin embargo, si se declaran varias sobrecargas con el mismo número de parámetros, que se debe aplicar el atributo [**Windows.Foundation.Metadata.DefaultOverloadAttribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) solo a una de estas sobrecargas. Esa sobrecarga es la única a la que puedes llamar desde JavaScript. Por ejemplo, en el siguiente código la sobrecarga que toma un valor **int** (**Integer** en Visual Basic) es la sobrecarga predeterminada.

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> [IMPORTANTE] JavaScript permite pasar cualquier valor a **OverloadExample**y convierte el valor al tipo que necesita el parámetro. Puedes llamar a **OverloadExample** con "cuarenta y dos", "42" o 42.3, pero todos estos valores se pasan a la sobrecarga predeterminada. La sobrecarga predeterminada en el ejemplo anterior devuelve 0, 42 y 42, respectivamente.

No se puede aplicar el atributo de e **DefaultOverloadAttribut**a los constructores. Todos los constructores en una clase deben tener números de parámetros diferentes.

## <a name="implementing-istringable"></a>Implementar IStringable

A partir de Windows 8.1, Windows Runtime incluye una interfaz **IStringable** cuyo método único, **IStringable.ToString**, proporciona compatibilidad básica de formato comparable a la que proporciona **Object.ToString**. Si decides implementar **IStringable** en un tipo administrado público que se exporta en un componente de Windows Runtime, se aplican las siguientes restricciones:

-   Puedes definir la interfaz **IStringable** solo en una relación de "clase implementa", como el siguiente código en C#:

    ```cs
    public class NewClass : IStringable
    ```

    O el siguiente código Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   No puedes implementar **IStringable** en una interfaz.
-   No puedes declarar un parámetro de tipo **IStringable**.
-   **IStringable** no puede ser el tipo devuelto de un método, una propiedad o un campo.
-   No se puede ocultar la implementación **IStringable** de clases base mediante el uso de una definición de método, como los siguientes:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    En su lugar, la implementación de **IStringable.ToString** siempre debe invalidar la implementación de la clase base. Puedes ocultar una implementación **ToString** solo al invocarla en una instancia de clase fuertemente tipada.

> [!NOTE]
> En una gran variedad de condiciones, las llamadas de código nativo a un tipo administrado que implementa **IStringable** u oculta su implementación **ToString** pueden producir un comportamiento inesperado.

## <a name="asynchronous-operations"></a>Operaciones asincrónicas

Para implementar un método asincrónico en tu componente, agrega "Async" al final del nombre del método y devolver una de las interfaces de Windows Runtime que representan operaciones o acciones asincrónicas: **IAsyncAction**, IAsyncActionWithProgress **&lt; TProgress&gt;**, **IAsyncOperation&lt;TResult&gt;**, o **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**.

Puedes usar tareas en .NET Framework (la clase de [**tarea**](/dotnet/api/system.threading.tasks.task) y genérico [**tarea&lt;TResult&gt; **](/dotnet/api/system.threading.tasks.task-1) clase) para implementar el método asincrónico. Debe devolver una tarea que represente una operación en curso, como una tarea que se devuelve de un método asincrónico escrito en C# o Visual Basic, o una tarea que se devuelve desde el método [**Task.Run**](/dotnet/api/system.threading.tasks.task.run) . Si usas un constructor para crear la tarea, debes llamar a su método [Task.Start](/dotnet/api/system.threading.tasks.task.start) antes de devolverlo.

Un método que use `await` (`Await` en Visual Basic) requiere el `async` palabra clave (`Async` en Visual Basic). Si expones este método desde un componente de Windows Runtime, aplicar la `async` palabra clave al delegado que pasas para el método **Run** .

Para realizar acciones y operaciones asincrónicas que no son compatibles con la cancelación o los informes de progreso, puedes usar el método de extensión [WindowsRuntimeSystemExtensions.AsAsyncAction](https://msdn.microsoft.com/library/system.windowsruntimesystemextensions.asasyncaction.aspx) o [AsAsyncOperation&lt;TResult&gt;](https://msdn.microsoft.com/library/hh779745.aspx) para encapsular la tarea en la interfaz adecuada. Por ejemplo, el siguiente código implementa un método asincrónico mediante el uso de la **Task.Run&lt;TResult&gt; ** método para iniciar una tarea. El **AsAsyncOperation&lt;TResult&gt; ** método de extensión devuelve la tarea como una operación asincrónica de Windows Runtime.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

El siguiente código de JavaScript muestra cómo se puede llamar al método mediante el uso de un objeto [**WinJS.Promise**](https://msdn.microsoft.com/library/windows/apps/br211867.aspx) . La función que se pasa al método a continuación se ejecuta cuando se completa la llamada asincrónica. El parámetro stringList contiene la lista de cadenas que se devuelve el método **DownloadAsStringAsync** y la función hace que el procesamiento que sea necesario.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para acciones y operaciones asincrónicas que admiten la cancelación o los informes de progreso, usa la clase [**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) para generar una tarea iniciada y para enlazar la cancelación y características de la tarea con la cancelación y el progreso de informes de progreso características de informe de la interfaz de Windows Runtime adecuada. Para obtener un ejemplo compatible tanto con la cancelación como con los informes de progreso, consulta [Tutorial: crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Ten en cuenta que puedes usar los métodos de la clase **AsyncInfo** incluso si tu método asincrónico no compatible con la cancelación o los informes de progreso. Si usas una función lambda de Visual Basic o un método anónimo de C#, no proporciona los parámetros para el token y [**IProgress&lt;T&gt; **](https://msdn.microsoft.com/library/hh138298.aspx) interfaz. Si usas una función lambda de C#, facilita un parámetro de token, pero omítelo. El ejemplo anterior, que usa el AsAsyncOperation&lt;TResult&gt; método, tiene este aspecto cuando usas la [**AsyncInfo.Run&lt;TResult&gt;(Func&lt;CancellationToken, Task&lt;TResult&gt;**](https://msdn.microsoft.com/library/hh779740.aspx)) método sobrecarga en su lugar.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

Si creas un método asincrónico que, opcionalmente, admite la cancelación o los informes de progreso, considera la posibilidad de agregar sobrecargas que no tengan parámetros para un token de cancelación o los **IProgress&lt;T&gt; ** interfaz.

## <a name="throwing-exceptions"></a>Iniciar excepciones

Se puede iniciar cualquier tipo de excepción que esté incluida en .NET para las aplicaciones de Windows. No puedes declarar tus propios tipos de excepción públicos en un componente de Windows Runtime, pero puedes declarar e iniciar tipos no públicos.

Si tu componente no controla la excepción, se genera la excepción correspondiente en el código que llamó a tu componente. La apariencia de la excepción para el llamador depende de la manera en que el lenguaje de la llamada sea compatible con Windows Runtime.

-   En JavaScript, la excepción aparecerá como un objeto en el que el mensaje de excepción se reemplaza por un seguimiento de la pila. Cuando depuras tu aplicación en Visual Studio, puedes ver el texto del mensaje original en el cuadro de diálogo de excepción del depurador, identificado como "WinRT Information". No puedes acceder al texto del mensaje original desde el código de JavaScript.

    > **Sugerencia**.Actualmente, el seguimiento de la pila contiene el tipo de excepción administrado, pero no recomendamos analizar el seguimiento para identificar el tipo de excepción. En su lugar, usa un valor de HRESULT según se describe más adelante en este apartado.

-   En C++, la excepción aparece como una excepción de la plataforma. Si la propiedad de HResult de la excepción administrada se puede asignar al valor de HRESULT de una excepción de plataforma específica, se utiliza la excepción específica; de lo contrario, se produce una excepción de [**Platform:: COMException**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710414.aspx) . El texto del mensaje de la excepción administrada no está disponible para el código C++. Si se inició una excepción de plataforma específica, aparece el texto del mensaje predeterminado para ese tipo de excepción; de lo contrario, no se muestra ningún texto de mensaje. Consulta [Excepciones (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699896.aspx).
-   En C# o Visual Basic, la excepción es una excepción administrada normal.

Cuando inicias una excepción desde tu componente, puedes facilitar a un llamador de JavaScript o C++ el control de la excepción iniciando un tipo de excepción no pública cuyo valor de propiedad de HResult sea específico para tu componente. El valor HRESULT está disponible para un llamador de JavaScript a través de la propiedad de número del objeto de excepción y para un llamador de C++ a través de la propiedad [**COMException**](https://msdn.microsoft.com/library/windows/apps/xaml/hh710415.aspx) .

> [!NOTE]
> Usa un valor negativo para tu HRESULT. Un valor positivo se interpreta como éxito y no se inicia ninguna excepción en el llamador de JavaScript o C++.

## <a name="declaring-and-raising-events"></a>Declarar y generar eventos

Cuando se declara un tipo para retener los datos para tu evento, deriva de Object en lugar de EventArgs, ya que EventArgs no es un tipo de Windows Runtime. Usar [**EventHandler&lt;TEventArgs&gt; **](https://msdn.microsoft.com/library/db0etb8x.aspx) como el tipo de evento y usa el argumento de evento tipo como argumento de tipo genérico. Genera el evento del mismo modo que en una aplicación de .NET Framework.

Cuando se usa tu componente de Windows Runtime desde JavaScript o C++, el evento sigue el modelo de eventos de Windows Runtime que esperan estos lenguajes. Cuando usas el componente de C# o Visual Basic, el evento aparece como un evento de .NET Framework normal. Se proporciona un ejemplo en [Tutorial: crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Si implementas descriptores de acceso de eventos personalizados (declaras un evento con la palabra clave **Custom** en Visual Basic), debes seguir el patrón de eventos de Windows Runtime en tu implementación. Consulta [Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Ten en cuenta que cuando controlas el evento desde el código de C# o Visual Basic, seguirá pareciendo un evento de .NET Framework ordinario.

## <a name="next-steps"></a>Pasos siguientes

Una vez que hayas creado un componente de Windows Runtime para tu propio uso, es posible que observes que la funcionalidad que encapsula es útil para otros desarrolladores. Tienes dos opciones para empaquetar un componente para su distribución a otros desarrolladores. Consulta [Distribuir un componente de Windows Runtime administrado](https://msdn.microsoft.com/library/jj614475.aspx).

Para obtener más información acerca de las características de los lenguajes C# y Visual Basic, así como de la compatibilidad de .NET Framework con Windows Runtime, consulta [Referencia de los lenguajes Visual Basic y C#](https://msdn.microsoft.com/library/windows/apps/xaml/br212458.aspx).

## <a name="related-topics"></a>Temas relacionados
* [.NET para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/mt185501)
* [Tutorial: Creación de un componente simple de Windows Runtime y llamada al mismo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
