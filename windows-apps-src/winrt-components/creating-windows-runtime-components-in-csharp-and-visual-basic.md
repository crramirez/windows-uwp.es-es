---
title: Componentes de Windows Runtime con C# y Visual Basic
description: A partir de .NET 4,5, puede usar código administrado para crear sus propios tipos de Windows Runtime, empaquetados en un componente Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
ms.openlocfilehash: 57d46ea1f88395624943135247a8f610112aaf90
ms.sourcegitcommit: 21eb13a50402bf5442a5f0a4bf34800d1dc679c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2020
ms.locfileid: "90804735"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>Componentes de Windows Runtime con C# y Visual Basic

Puede usar código administrado para crear sus propios tipos de Windows Runtime y empaquetarlos en un componente Windows Runtime. Puede usar el componente en aplicaciones Plataforma universal de Windows (UWP) escritas en C++, JavaScript, Visual Basic o C#. En este tema se describen las reglas de creación de un componente y se analizan algunos aspectos de la compatibilidad con .NET para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime.

Si va a crear un componente para usarlo solo en aplicaciones UWP escritas en Visual Basic o en C#, y el componente no contiene controles de UWP, considere la posibilidad de usar la plantilla de **biblioteca de clases** en lugar de la plantilla de proyecto de **componente de Windows Runtime** en Microsoft Visual Studio. Hay menos restricciones en una biblioteca de clase simple.

## <a name="declaring-types-in-windows-runtime-components"></a>Declarar tipos en componentes de Windows Runtime

Internamente, los tipos de Windows Runtime del componente pueden usar cualquier funcionalidad de .NET que se permita en una aplicación de UWP. Para obtener más información, vea [.net para aplicaciones para UWP](/dotnet/api/index?view=dotnet-uwp-10.0).

Externamente, los miembros de los tipos pueden exponer solo Windows Runtime tipos de sus parámetros y valores devueltos. En la lista siguiente se describen las limitaciones en los tipos de .NET que se exponen de un componente de Windows Runtime.

- Los campos, los parámetros y los valores devueltos de todos los tipos públicos y miembros en tu componente deben ser tipos de Windows Runtime. Esta restricción incluye los tipos de Windows Runtime que se crean, así como los tipos proporcionados por el Windows Runtime mismo. También incluye varios tipos de .NET. La inclusión de estos tipos forma parte de la compatibilidad que .NET proporciona para habilitar el uso natural del Windows Runtime en código administrado &mdash; . el código parece usar tipos conocidos de .net en lugar de los tipos de Windows Runtime subyacentes. Por ejemplo, puede utilizar tipos primitivos de .NET como **Int32** y **Double**, algunos tipos fundamentales como **DateTimeOffset** y **URI**, y algunos tipos de interfaz genérica de uso frecuente como **ienumerable &lt; T &gt; ** (IEnumerable (of T) en Visual Basic) y **IDictionary &lt; TKey, TValue &gt; **. Tenga en cuenta que los argumentos de tipo de estos tipos genéricos deben ser Windows Runtime tipos. Esto se explica en las secciones [pasar Windows Runtime tipos a código administrado](#passing-windows-runtime-types-to-managed-code) y [pasar tipos administrados al Windows Runtime](#passing-managed-types-to-the-windows-runtime), más adelante en este tema.

- Las interfaces y clases públicas pueden contener métodos, propiedades y eventos. Puede declarar delegados para los eventos o usar el **delegado &lt; EventHandler &gt; T** . Una clase o interfaz pública no puede:
    - Ser genérica.
    - Implementar una interfaz que no sea una interfaz Windows Runtime (sin embargo, puede crear sus propias interfaces de Windows Runtime e implementarlas).
    - Derive de tipos que no están en el Windows Runtime, como **System. Exception** y **System. EventArgs**.

- Todos los tipos públicos deben tener un espacio de nombres raíz que coincida con el nombre del ensamblado y el nombre del ensamblado no puede empezar por "Windows".

    > **Sugerencia**. De forma predeterminada, los proyectos de Visual Studio tienen espacios de nombres que coinciden con el nombre de ensamblado. En Visual Basic, la declaración de espacio de nombres para este espacio de nombres predeterminado no se muestra en tu código.

- Las estructuras públicas no pueden tener miembros que no sean campos públicos y los campos deben ser tipos de valor o cadenas.
- Las clases públicas deben ser **sealed** (**NotInheritable** en Visual Basic). Si el modelo de programación requiere polimorfismo, puede crear una interfaz pública e implementar esa interfaz en las clases que deben ser polimórficas.

## <a name="debugging-your-component"></a>Depurar tu componente

Si su aplicación para UWP y su componente se compilan con código administrado, puede depurarlos al mismo tiempo.

Al probar el componente como parte de una aplicación de UWP con C++, puede depurar código administrado y nativo al mismo tiempo. De forma predeterminada, es solo código nativo.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar tanto código C++ nativo como código administrado
1.  Abre el menú contextual para tu proyecto de Visual C++ y elige **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Solo nativo** por **Mixto (administrado y nativo)**. Elija **Aceptar**.
4.  Establecer puntos de interrupción en el código nativo y administrado.

Al probar el componente como parte de una aplicación de UWP con JavaScript, de forma predeterminada la solución está en modo de depuración de JavaScript. En Visual Studio, no puedes depurar código administrado y JavaScript al mismo tiempo.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código administrado en lugar de JavaScript
1.  Abre el menú contextual de tu proyecto de JavaScript y selecciona **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Script solo** por **Sólo administrado**. Elija **Aceptar**.
4.  Establecer puntos de interrupción en código administrado y depurar de la forma habitual.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Pasar los tipos de Windows Runtime a código administrado
Como se mencionó anteriormente en la sección [declarar tipos en componentes de Windows Runtime](#declaring-types-in-windows-runtime-components), algunos tipos de .net pueden aparecer en las firmas de miembros de clases públicas. Esto forma parte de la compatibilidad que .NET proporciona para habilitar el uso natural del Windows Runtime en código administrado. Incluye tipos primitivos y algunas clases e interfaces. Cuando el componente se utiliza desde JavaScript o desde código de C++, es importante saber cómo los tipos de .NET aparecen en el autor de la llamada. Vea [el tutorial sobre cómo crear un componente de Windows Runtime de C# o Visual Basic y cómo llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obtener ejemplos con JavaScript. En este apartado se describen los tipos usados habitualmente.

En .NET, los tipos primitivos como la estructura **Int32** tienen muchas propiedades y métodos útiles, como el método **TryParse** . Por el contrario, las estructuras y los tipos primitivos en Windows Runtime solo tienen campos. Al pasar estos tipos a código administrado, parecen ser tipos .NET, y puede usar las propiedades y los métodos de tipos .NET como lo haría normalmente. En la siguiente lista se resumen las sustituciones que se realizan automáticamente en el IDE:

-   En el caso de los primitivos de Windows Runtime **Int32**, **Int64**, **Single**, **Double**, **Boolean**, **String** (una colección inmutable de caracteres Unicode), **enum**, **UInt32**, **UInt64**y **GUID**, use el tipo del mismo nombre en el espacio de nombres System.
-   Para **UInt8**, use **System. Byte**.
-   En el caso de **Char16**, use **System. Char**.
-   Para la interfaz **IInspectable** , use **System. Object**.

Si C# o Visual Basic proporciona una palabra clave de lenguaje para cualquiera de estos tipos, puede usar la palabra clave de lenguaje en su lugar.

Además de los tipos primitivos, algunos tipos de Windows Runtime básicos que se usan con frecuencia aparecen en código administrado como sus equivalentes de .NET. Por ejemplo, supongamos que el código de JavaScript usa la clase **Windows. Foundation. Uri** y desea pasarlo a un método de C# o Visual Basic. El tipo equivalente en código administrado es la clase **System. Uri** de .net y es el tipo que se va a usar para el parámetro de método. Puede saber cuándo se muestra un tipo de Windows Runtime como un tipo .NET, porque IntelliSense en Visual Studio oculta el tipo de Windows Runtime al escribir código administrado y presenta el tipo .NET equivalente. (Normalmente los dos tipos tienen el mismo nombre. Sin embargo, tenga en cuenta que la estructura **Windows. Foundation. DateTime** aparece en código administrado como **System. DateTimeOffset** y no como **System. DateTime**.)

Para algunos tipos de colección de uso común, la asignación se realiza entre las interfaces implementadas por un tipo de Windows Runtime y las interfaces implementadas por el tipo .NET correspondiente. Al igual que con los tipos mencionados anteriormente, los tipos de parámetros se declaran mediante el tipo .NET. Esto oculta algunas diferencias entre los tipos y hace que la escritura de código .NET sea más natural.

En la siguiente tabla se enumeran los tipos de interfaz genérica más frecuentes, junto con otras asignaciones de clase e interfaz habituales. Para obtener una lista completa de los tipos de Windows Runtime que .NET asigna, consulte [asignaciones de .net de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

| Windows en tiempo de ejecución                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable &lt; T&gt;                              |
| IVector&lt;T&gt;                                 | IList &lt; T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList &lt; T&gt;                            |
| IMap &lt; K, V&gt;                                 | IDictionary &lt; TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary &lt; TKey, TValue&gt;           |
| IKeyValuePair &lt; K, V&gt;                        | KeyValuePair &lt; TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVector                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

Cuando un tipo implementa más de una interfaz, puedes usar cualquiera de las interfaces que implementa como un tipo de parámetro o tipo devuelto de un miembro. Por ejemplo, puede pasar o devolver un **diccionario &lt; int, String &gt; ** (**Dictionary (of Integer, String)** en Visual Basic) como **IDictionary &lt; int, String &gt; **, **IReadOnlyDictionary &lt; int, String &gt; **o **IEnumerable &lt; System. Collections. Generic. KeyValuePair &lt; TKey, TValue &gt; &gt; **.

> [!IMPORTANT]
> JavaScript usa la interfaz que aparece en primer lugar en la lista de interfaces que un tipo administrado implementa. Por ejemplo, si se devuelve el **diccionario &lt; int, &gt; la cadena** al código JavaScript, aparece como **IDictionary &lt; int, &gt; cadena,** con independencia de la interfaz que se especifique como tipo de valor devuelto. Esto significa que si la primera interfaz no incluye a un miembro que aparece en las últimas interfaces, ese miembro no es visible para JavaScript.

En el Windows Runtime, **IMap &lt; k, v &gt; ** y **IMapView &lt; k, v &gt; ** se recorren en iteración mediante IKeyValuePair. Cuando se pasan a código administrado, aparecen como **IDictionary &lt; TKey, TValue &gt; ** y **IReadOnlyDictionary &lt; TKey, &gt; TValue**, por lo que de forma natural se usa **System. Collections. Generic. KeyValuePair &lt; TKey, TValue &gt; ** para enumerarlos.

La forma en que las interfaces aparecen en código administrado afecta a la forma en que aparecen los tipos que implementan estas interfaces. Por ejemplo, la clase **PropertySet** implementa **IMap &lt; K, V &gt; **, que aparece en código administrado como **IDictionary &lt; TKey, TValue &gt; **. **PropertySet** aparece como si implementara **IDictionary &lt; TKey, TValue &gt; ** en lugar de **IMap &lt; K, V &gt; **, por lo que en código administrado parece tener un método **Add** , que se comporta como el método **Add** en los diccionarios de .net. No parece tener un método **Insert** . Puede ver este ejemplo en el tema [tutorial sobre cómo crear un componente de Windows Runtime de C# o Visual Basic y cómo llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Pasar los tipos administrados a Windows Runtime

Como se explicó en la sección anterior, algunos tipos de Windows Runtime pueden aparecer como tipos .NET en las firmas de los miembros del componente, o en las firmas de los miembros de Windows Runtime cuando se usan en el IDE. Al pasar tipos .NET a estos miembros o usarlos como valores devueltos de los miembros de su componente, aparecen en el código del otro lado como el tipo de Windows Runtime correspondiente. Para obtener ejemplos de los efectos que esto puede tener cuando se llama a su componente desde JavaScript, consulte la sección "devolver tipos administrados desde el componente" en el [tutorial de creación de un componente de Windows Runtime de C# o Visual Basic, y cómo llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecargados

En Windows Runtime, los métodos se pueden sobrecargar. Sin embargo, si declara varias sobrecargas con el mismo número de parámetros, debe aplicar el atributo [**Windows. Foundation. Metadata. DefaultOverloadAttribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) a solo una de esas sobrecargas. Esa sobrecarga es la única a la que puedes llamar desde JavaScript. Por ejemplo, en el siguiente código la sobrecarga que toma un valor **int** (**Integer** en Visual Basic) es la sobrecarga predeterminada.

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

> AÚN JavaScript le permite pasar cualquier valor a **OverloadExample**y convierte el valor al tipo requerido por el parámetro. Puede llamar a **OverloadExample** con "42", "42" o 42,3, pero todos estos valores se pasan a la sobrecarga predeterminada. La sobrecarga predeterminada del ejemplo anterior devuelve 0, 42 y 42, respectivamente.

No se puede aplicar el atributo **DefaultOverloadAttribut**e a los constructores. Todos los constructores en una clase deben tener números de parámetros diferentes.

## <a name="implementing-istringable"></a>Implementar IStringable

A partir de Windows 8.1, el Windows Runtime incluye una interfaz **IStringable** cuyo único método, **IStringable. ToString**, proporciona compatibilidad de formato básica comparable a la proporcionada por **Object. ToString**. Si decide implementar **IStringable** en un tipo público administrado que se exporte en un Windows Runtime componente, se aplican las restricciones siguientes:

-   Solo puede definir la interfaz **IStringable** en una relación "la clase implementa", como el código siguiente en C#:

    ```cs
    public class NewClass : IStringable
    ```

    O el siguiente código Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   No se puede implementar **IStringable** en una interfaz.
-   No se puede declarar un parámetro como de tipo **IStringable**.
-   **IStringable** no puede ser el tipo de valor devuelto de un método, propiedad o campo.
-   No se puede ocultar la implementación de **IStringable** de las clases base mediante una definición de método como la siguiente:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    En su lugar, la implementación de **IStringable. ToString** debe invalidar siempre la implementación de la clase base. Puede ocultar una implementación de **ToString** solo si la invoca en una instancia de clase fuertemente tipada.

> [!NOTE]
> En una variedad de condiciones, las llamadas de código nativo a un tipo administrado que implementa **IStringable** u oculta su implementación de **ToString** pueden producir un comportamiento inesperado.

## <a name="asynchronous-operations"></a>Operaciones asincrónicas

Para implementar un método asincrónico en el componente, agregue "Async" al final del nombre del método y devuelva una de las interfaces Windows Runtime que representen acciones o operaciones asincrónicas: **IAsyncAction**, **IAsyncActionWithProgress &lt; TProgress &gt; **, **IAsyncOperation &lt; TResult &gt; **o **IAsyncOperationWithProgress &lt; TResult, TProgress &gt; **.

Puede usar tareas de .NET (clase de [**tarea**](/dotnet/api/system.threading.tasks.task) y clase [** &lt; TResult &gt; de tarea**](/dotnet/api/system.threading.tasks.task-1) genérica) para implementar el método asincrónico. Debe devolver una tarea que represente una operación en curso, como una tarea que se devuelve desde un método asincrónico escrito en C# o Visual Basic, o una tarea que se devuelve desde el método [**Task. Run**](/dotnet/api/system.threading.tasks.task.run) . Si usas un constructor para crear la tarea, debes llamar a su método [Task.Start](/dotnet/api/system.threading.tasks.task.start) antes de devolverlo.

Un método que usa `await` ( `Await` en Visual Basic) requiere la `async` palabra clave ( `Async` en Visual Basic). Si expone un método de este tipo desde un componente de Windows Runtime, aplique la `async` palabra clave al delegado que pasa al método **Run** .

Para realizar acciones y operaciones asincrónicas que no son compatibles con la cancelación o los informes de progreso, puedes usar el método de extensión [WindowsRuntimeSystemExtensions.AsAsyncAction](/dotnet/api/system) o [AsAsyncOperation&lt;TResult&gt;](/dotnet/api/system) para encapsular la tarea en la interfaz adecuada. Por ejemplo, el código siguiente implementa un método asincrónico mediante el método **Task. Run &lt; TResult &gt; ** para iniciar una tarea. El método de extensión ** &lt; &gt; TResult de AsAsyncOperation** devuelve la tarea como una operación asincrónica Windows Runtime.

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

El siguiente código JavaScript muestra cómo se podría llamar al método mediante un objeto [**WinJS. Promise**](/previous-versions/windows/apps/br211867(v=win.10)) . La función que se pasa al método a continuación se ejecuta cuando se completa la llamada asincrónica. El parámetro stringList contiene la lista de cadenas devuelta por el método **DownloadAsStringAsync** y la función realiza cualquier procesamiento necesario.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para las acciones y operaciones asincrónicas que admiten cancelación o informes de progreso, utilice la clase [**AsyncInfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) para generar una tarea iniciada y enlazar las características de cancelación y de informes de progreso de la tarea con las características de cancelación y de informes de progreso de la interfaz de Windows Runtime adecuada. Para ver un ejemplo que admite la cancelación y la generación de informes de progreso, vea [tutorial de creación de un componente de C# o Visual Basic Windows Runtime y su llamada desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Tenga en cuenta que puede usar los métodos de la clase **AsyncInfo** incluso si el método asincrónico no admite cancelación o informes de progreso. Si usa una función lambda Visual Basic o un método anónimo de C#, no proporcione parámetros para la interfaz token [**y &lt; IProgress &gt; t**](/dotnet/api/system.iprogress-1) . Si usas una función lambda de C#, facilita un parámetro de token, pero omítelo. El ejemplo anterior, que usaba el &lt; método TResult de AsAsyncOperation &gt; , tiene este aspecto cuando se usa la sobrecarga del método [**AsyncInfo. Run &lt; TResult &gt; (Func &lt; CancellationToken, Task &lt; TResult &gt; &gt; **](/dotnet/api/system.runtime.interopservices.windowsruntime)) en su lugar.

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

Si crea un método asincrónico que admite opcionalmente cancelación o informes de progreso, considere la posibilidad de agregar sobrecargas que no tengan parámetros para un token de cancelación o la interfaz **IProgress &lt; t &gt; ** .

## <a name="throwing-exceptions"></a>Iniciar excepciones

Se puede iniciar cualquier tipo de excepción que esté incluida en .NET para las aplicaciones de Windows. No puedes declarar tus propios tipos de excepción públicos en un componente de Windows Runtime, pero puedes declarar e iniciar tipos no públicos.

Si tu componente no controla la excepción, se genera la excepción correspondiente en el código que llamó a tu componente. La apariencia de la excepción para el llamador depende de la manera en que el lenguaje de la llamada sea compatible con Windows Runtime.

-   En JavaScript, la excepción aparecerá como un objeto en el que el mensaje de excepción se reemplaza por un seguimiento de la pila. Cuando depuras tu aplicación en Visual Studio, puedes ver el texto del mensaje original en el cuadro de diálogo de excepción del depurador, identificado como "WinRT Information". No puedes acceder al texto del mensaje original desde el código de JavaScript.

    > **Sugerencia**.Actualmente, el seguimiento de la pila contiene el tipo de excepción administrada, pero no recomendamos analizar el seguimiento para identificar el tipo de excepción. En su lugar, usa un valor de HRESULT según se describe más adelante en este apartado.

-   En C++, la excepción aparece como una excepción de la plataforma. Si la propiedad HResult de la excepción administrada se puede asignar al valor HRESULT de una excepción de plataforma concreta, se usa la excepción concreta; de lo contrario, se produce una excepción [**Platform:: COMException**](/cpp/cppcx/platform-comexception-class) . El texto del mensaje de la excepción administrada no está disponible para el código C++. Si se inició una excepción de plataforma específica, aparece el texto del mensaje predeterminado para ese tipo de excepción; de lo contrario, no se muestra ningún texto de mensaje. Consulta [Excepciones (C++/CX)](/cpp/cppcx/exceptions-c-cx).
-   En C# o Visual Basic, la excepción es una excepción administrada normal.

Cuando inicias una excepción desde tu componente, puedes facilitar a un llamador de JavaScript o C++ el control de la excepción iniciando un tipo de excepción no pública cuyo valor de propiedad de HResult sea específico para tu componente. El valor HRESULT está disponible para un llamador de JavaScript a través de la propiedad de número del objeto de excepción y a un llamador de C++ a través de la propiedad [**COMException:: HRESULT**](/cpp/cppcx/platform-comexception-class#hresult) .

> [!NOTE]
> Use un valor negativo para el HRESULT. Un valor positivo se interpreta como éxito y no se inicia ninguna excepción en el llamador de JavaScript o C++.

## <a name="declaring-and-raising-events"></a>Declarar y generar eventos

Cuando se declara un tipo para retener los datos para tu evento, deriva de Object en lugar de EventArgs, ya que EventArgs no es un tipo de Windows Runtime. Use [**EventHandler &lt; TEventArgs &gt; **](/dotnet/api/system.eventhandler-1) como el tipo del evento y use el tipo de argumento de evento como argumento de tipo genérico. Genere el evento tal como lo haría en una aplicación .NET.

Cuando se usa tu componente de Windows Runtime desde JavaScript o C++, el evento sigue el modelo de eventos de Windows Runtime que esperan estos lenguajes. Cuando se utiliza el componente de C# o Visual Basic, el evento aparece como un evento de .NET ordinario. En el [tutorial de creación de un componente de Windows Runtime de C# o Visual Basic, se proporciona un ejemplo y se llama desde JavaScript](./walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Si implementas descriptores de acceso de eventos personalizados (declaras un evento con la palabra clave **Custom** en Visual Basic), debes seguir el patrón de eventos de Windows Runtime en tu implementación. Consulte [eventos personalizados y descriptores de acceso de eventos en Windows Runtime componentes](custom-events-and-event-accessors-in-windows-runtime-components.md). Tenga en cuenta que cuando controla el evento desde el código de C# o Visual Basic, sigue siendo un evento de .NET ordinario.

## <a name="next-steps"></a>Pasos siguientes

Una vez que hayas creado un componente de Windows Runtime para tu propio uso, es posible que observes que la funcionalidad que encapsula es útil para otros desarrolladores. Tienes dos opciones para empaquetar un componente para su distribución a otros desarrolladores. Consulta [Distribuir un componente de Windows Runtime administrado](/previous-versions/windows/apps/jj614475(v=vs.140)).

Para obtener más información sobre Visual Basic y las características del lenguaje C#, así como la compatibilidad de .NET con la Windows Runtime, vea [Visual Basic y referencia del lenguaje c#](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).


## <a name="troubleshooting"></a>Solución de problemas

| Síntoma | Solución |
|---------|--------|
|En una aplicación/WinRT de C++, al consumir un [componente de Windows Runtime de C#](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic) que usa XAML, el compilador genera un error con el formato ""*MyNamespace_XamlTypeInfo ': no es un miembro de "WinRT:: myNameSpace*" ", &mdash; donde *myNameSpace* es el nombre del espacio de nombres del componente Windows Runtime. | En `pch.h` , en la aplicación de C++/WinRT de consumo, agregue `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` &mdash; *myNameSpace* , según corresponda. |

## <a name="related-topics"></a>Temas relacionados
* [.NET para aplicaciones UWP](/dotnet/api/index?view=dotnet-uwp-10.0)
* [Tutorial para crear un componente de Windows Runtime de C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)