---
title: Crear componentes de Windows Runtime en C# y Visual Basic
description: A partir de .NET Framework 4.5, puedes usar código administrado para crear tus propios tipos de Windows Runtime, empaquetados en un componente de Windows Runtime.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a20f8e85927015d6aa69dbbf4381cb2d7daafc36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372028"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Crear componentes de Windows Runtime en C# y Visual Basic
A partir de .NET Framework 4.5, puede usar código administrado para crear sus propios tipos en tiempo de ejecución de Windows y empaquetarlos en un componente de Windows en tiempo de ejecución. Puede usar el componente en aplicaciones de plataforma Universal de Windows (UWP) que están escritas en C++, JavaScript, Visual Basic, o C#. En este artículo se describen las reglas de creación de un componente y se analizan algunos aspectos de la compatibilidad con .NET Framework para Windows Runtime. En general, esa compatibilidad está diseñada para ser transparente para los programadores de .NET Framework. Sin embargo, cuando creas un componente para su uso con JavaScript o C++, debes tener en cuenta las diferencias en la forma en que esos lenguajes son compatibles con Windows Runtime.

Si va a crear un componente para usarlo solo en aplicaciones UWP que se escriben en Visual Basic o C#, y el componente no contiene controles UWP, a continuación, considere la posibilidad utilizando el **biblioteca de clases** en lugar de la **Windows Componente de tiempo de ejecución** plantilla de proyecto en Microsoft Visual Studio. Hay menos restricciones en una biblioteca de clase simple.

## <a name="declaring-types-in-windows-runtime-components"></a>Declarar tipos en componentes de Windows Runtime

Internamente, los tipos en tiempo de ejecución de Windows en el componente pueden usar cualquier funcionalidad de .NET Framework que se permite en una aplicación para UWP. Para obtener más información, consulte [.NET para aplicaciones UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).

Externamente, los miembros de sus tipos pueden exponer solo los tipos en tiempo de ejecución de Windows para sus parámetros y valores devueltos. En la lista siguiente se describe las limitaciones en los tipos de .NET Framework que se exponen desde un componente de Windows en tiempo de ejecución.

- Los campos, los parámetros y los valores devueltos de todos los tipos públicos y miembros en tu componente deben ser tipos de Windows Runtime. Esta restricción incluye los tipos en tiempo de ejecución de Windows que cree, así como los tipos proporcionados por el propio tiempo de ejecución de Windows. También incluye varios tipos de .NET Framework. La inclusión de estos tipos forma parte de la compatibilidad de .NET Framework proporciona para habilitar el uso natural de Windows Runtime en código administrado&mdash;tu código parece que utiliza tipos de .NET Framework familiares en lugar de tipos en tiempo de ejecución de Windows subyacentes. Por ejemplo, puede usar los tipos primitivos de .NET Framework como **Int32** y **doble**, algunos tipos fundamentales como **DateTimeOffset** y **Uri** , y algunos normalmente usan tipos de interfaz genérica como **IEnumerable&lt;T&gt;**  (IEnumerable (Of T) en Visual Basic) y **IDictionary&lt; TKey, TValue&gt;** . Tenga en cuenta que los argumentos de tipo de estos tipos genéricos deben ser tipos de Windows en tiempo de ejecución. Esto se explica en las secciones [tipos pasando Windows en tiempo de ejecución a código administrado](#passing-windows-runtime-types-to-managed-code) y [pasar tipos administrados a Windows Runtime](#passing-managed-types-to-the-windows-runtime), más adelante en este tema.

- Las interfaces y clases públicas pueden contener métodos, propiedades y eventos. Puede declarar delegados para sus eventos o usar el **EventHandler&lt;T&gt;**  delegar. Una clase pública o una interfaz no puede:
    - Ser genérica.
    - Implementar una interfaz que no es una interfaz de Windows en tiempo de ejecución (sin embargo, puede crear sus propias interfaces de Windows en tiempo de ejecución y su implementación).
    - Derivar de tipos que no están en el tiempo de ejecución de Windows, como **System.Exception** y **System.EventArgs**.

- Todos los tipos públicos deben tener un espacio de nombres raíz que coincida con el nombre del ensamblado y el nombre del ensamblado no puede empezar por "Windows".

    > **Sugerencia**. De forma predeterminada, los proyectos de Visual Studio tienen espacios de nombres que coinciden con el nombre del ensamblado. En Visual Basic, la declaración de espacio de nombres para este espacio de nombres predeterminado no se muestra en tu código.

- Las estructuras públicas no pueden tener miembros que no sean campos públicos y los campos deben ser tipos de valor o cadenas.
- Las clases públicas deben ser **sealed** (**NotInheritable** en Visual Basic). Si su modelo de programación requiere polimorfismo, puede crear una interfaz pública e implementar esa interfaz en las clases que deben ser polimórficas.

## <a name="debugging-your-component"></a>Depurar tu componente

Si su aplicación para UWP y su componente se compilan con código administrado, puede depurar ambos al mismo tiempo.

Cuando pruebe su componente como parte de una aplicación para UWP con C++, puede depurar código administrado y nativo al mismo tiempo. De forma predeterminada, es solo código nativo.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>Para depurar tanto código C++ nativo como código administrado
1.  Abre el menú contextual para tu proyecto de Visual C++ y elige **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Solo nativo** por **Mixto (administrado y nativo)** . Elige **Aceptar**.
4.  Establecer puntos de interrupción en el código nativo y administrado.

Cuando pruebe su componente como parte de una aplicación para UWP con JavaScript, de forma predeterminada la solución está en modo de depuración de JavaScript. En Visual Studio, no puedes depurar código administrado y JavaScript al mismo tiempo.

## <a name="to-debug-managed-code-instead-of-javascript"></a>Para depurar código administrado en lugar de JavaScript
1.  Abre el menú contextual de tu proyecto de JavaScript y selecciona **Propiedades**.
2.  En las páginas de propiedades, en **Propiedades de configuración**, elige **Depuración**.
3.  Elige **Tipo de depurador**y en el cuadro de la lista desplegable cambia **Script solo** por **Sólo administrado**. Elige **Aceptar**.
4.  Establecer puntos de interrupción en código administrado y depurar de la forma habitual.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Pasar los tipos de Windows Runtime a código administrado
Como se mencionó anteriormente en la sección [declarar tipos en componentes de Windows en tiempo de ejecución de](#declaring-types-in-windows-runtime-components), ciertos tipos de .NET Framework pueden aparecer en las firmas de miembros de clases públicas. Esto forma parte de la compatibilidad que proporciona .NET Framework para habilitar el uso natural de Windows Runtime en código administrado. Incluye tipos primitivos y algunas clases e interfaces. Cuando se utiliza el componente desde JavaScript o desde el código de C++, es importante saber cómo se muestran los tipos de .NET Framework al llamador. Consulte [Tutorial: Crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) para obtener ejemplos con JavaScript. En este apartado se describen los tipos usados habitualmente.

En .NET Framework, tipos primitivos como la **Int32** estructura tienen muchas propiedades y métodos útiles, como el **TryParse** método. Por el contrario, las estructuras y los tipos primitivos en Windows Runtime solo tienen campos. Cuando pasas estos tipos a código administrado, parecen tipos de .NET Framework y puedes usar las propiedades y métodos de los tipos de .NET Framework como lo harías normalmente. En la siguiente lista se resumen las sustituciones que se realizan automáticamente en el IDE:

-   Para las primitivas de Windows en tiempo de ejecución **Int32**, **Int64**, **único**, **doble**, **booleano**,  **Cadena** (una colección inmutable de caracteres Unicode), **Enum**, **UInt32**, **UInt64**, y **Guid**, utilice el tipo del mismo nombre en el espacio de nombres del sistema.
-   Para **UInt8**, utilice **System.Byte**.
-   Para **Char16**, utilice **System.Char**.
-   Para el **IInspectable** interfaz, use **System.Object**.

Si C# o Visual Basic ofrece una palabra clave del lenguaje para cualquiera de estos tipos, a continuación, puede usar la palabra clave del lenguaje en su lugar.

Además de los tipos primitivos, algunos tipos básicos de Windows Runtime de uso habitual aparecen en código administrado como sus equivalentes de .NET Framework. Por ejemplo, suponga que el código de JavaScript usa la **Windows.Foundation.Uri** clase y desea pasar a un C# o método de Visual Basic. El tipo equivalente en código administrado es .NET Framework **System.Uri** (clase), y que es el tipo que se utilizará para el parámetro de método. Puedes saber cuándo un tipo de Windows Runtime aparece como un tipo de .NET Framework, porque IntelliSense en Visual Studio oculta el tipo de Windows Runtime cuando estás escribiendo código administrado y presenta el tipo de .NET Framework equivalente. (Normalmente los dos tipos tienen el mismo nombre. Sin embargo, tenga en cuenta que el **Windows.Foundation.DateTime** estructura aparece en código administrado como **System.DateTimeOffset** y no como **System.DateTime**.)

Para algunos tipos de colección usados con frecuencia, la asignación es entre las interfaces implementadas por un tipo de Windows Runtime y las interfaces implementadas por el tipo de .NET Framework correspondiente. Del mismo modo que con los tipos mencionados anteriormente, declaras tipos de parámetros mediante el uso del tipo de .NET Framework. Este oculta algunas diferencias entre los tipos y logra que la escritura de .NET Framework sea más natural.

En la siguiente tabla se enumeran los tipos de interfaz genérica más frecuentes, junto con otras asignaciones de clase e interfaz habituales. Para obtener una lista completa de tipos en tiempo de ejecución de Windows que .NET Framework asigna, vea [asignaciones de .NET Framework de tipos de Windows Runtime](net-framework-mappings-of-windows-runtime-types.md).

| Windows en tiempo de ejecución                                  | .NET Framework                                    |
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

Cuando un tipo implementa más de una interfaz, puedes usar cualquiera de las interfaces que implementa como un tipo de parámetro o tipo devuelto de un miembro. Por ejemplo, puede pasar o devolver un **diccionario&lt;int, string&gt;**  (**Dictionary (Of Integer, String)** en Visual Basic) como **IDictionary&lt;int, string&gt;** , **IReadOnlyDictionary&lt;int, string&gt;** , o **IEnumerable&lt; System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;** .

> [!IMPORTANT]
> JavaScript usa la interfaz que aparece en primer lugar en la lista de interfaces que implementa un tipo administrado. Por ejemplo, si devuelves **diccionario&lt;int, string&gt;**  al código de JavaScript, aparecerá como **IDictionary&lt;int, string&gt;**  , independientemente de qué interfaz que se especifica como el tipo de valor devuelto. Esto significa que si la primera interfaz no incluye a un miembro que aparece en las últimas interfaces, ese miembro no es visible para JavaScript.

En el tiempo de ejecución de Windows, **IMap&lt;K, V&gt;**  y **IMapView&lt;K, V&gt;**  se iteran mediante IKeyValuePair. Cuando las pasas a código administrado, aparecen como **IDictionary&lt;TKey, TValue&gt;**  y **IReadOnlyDictionary&lt;TKey, TValue&gt;** , por lo que Naturalmente utilice **System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;**  para enumerarlas.

La forma en la que las interfaces aparecen en código administrado afecta a la forma en que aparecen los tipos que implementan estas interfaces. Por ejemplo, el **PropertySet** la clase implementa **IMap&lt;K, V&gt;** , que aparece en código administrado como **IDictionary&lt;TKey, TValue&gt;** . **PropertySet** aparece como si se implementara **IDictionary&lt;TKey, TValue&gt;**  en lugar de **IMap&lt;K, V&gt;** , en administrado código parece tener un **agregar** método, que se comporta como el **agregar** método en diccionarios de .NET Framework. No parece tener un **insertar** método. Puede ver este ejemplo en el tema [Tutorial: Crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Pasar los tipos administrados a Windows Runtime

Como se explicó en el apartado anterior, algunos tipos de Windows Runtime pueden aparecer como tipos de .NET Framework en las signaturas de los miembros de tu componente o en las signaturas de miembros de Windows Runtime cuando las utilizas en el IDE. Al pasar los tipos de .NET Framework a estos miembros o al usarlos como valores devueltos de los miembros de tu componente, aparecen en el código en el otro lado, como el tipo de Windows Runtime correspondiente. Para obtener ejemplos de los efectos que esto puede tener cuando se llama a su componente desde JavaScript, vea la sección "Devolver tipos administrados desde el componente" en [Tutorial: Crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Métodos sobrecargados

En Windows Runtime, los métodos se pueden sobrecargar. Sin embargo, si declara varias sobrecargas con el mismo número de parámetros, debe aplicar el [ **Windows.Foundation.Metadata.DefaultOverloadAttribute** ](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) atribuir a solo una de dichas sobrecargas. Esa sobrecarga es la única a la que puedes llamar desde JavaScript. Por ejemplo, en el siguiente código la sobrecarga que toma un valor **int** (**Integer** en Visual Basic) es la sobrecarga predeterminada.

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

> [IMPORTANTE] JavaScript le permite pasar cualquier valor a **OverloadExample**y convierte el valor al tipo requerido por el parámetro. Puede llamar a **OverloadExample** con "cuarenta y dos", "42" o "42,3", pero todos esos valores se pasan a la sobrecarga predeterminada. La sobrecarga predeterminada en el ejemplo anterior devuelve 0, 42 y 42, respectivamente.

No se puede aplicar el **DefaultOverloadAttribut**atributo e a constructores. Todos los constructores en una clase deben tener números de parámetros diferentes.

## <a name="implementing-istringable"></a>Implementar IStringable

A partir de Windows 8.1, el tiempo de ejecución de Windows incluye un **IStringable** interfaz cuyo único método, **IStringable.ToString**, proporciona compatibilidad básica de formato comparable a la que ofrece  **Object.ToString**. Si decide implementar **IStringable** en un tipo público administrado que se exporte en un componente de Windows en tiempo de ejecución, se aplican las restricciones siguientes:

-   Puede definir la **IStringable** interfaz sólo en una relación "la clase implementa", como el siguiente código en C#:

    ```cs
    public class NewClass : IStringable
    ```

    O el siguiente código Visual Basic:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   No se puede implementar **IStringable** en una interfaz.
-   No se puede declarar un parámetro de tipo **IStringable**.
-   **IStringable** no puede ser el tipo de valor devuelto de un método, propiedad o campo.
-   No se pueden ocultar su **IStringable** implementación de las clases base mediante una definición de método como la siguiente:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    En su lugar, el **IStringable.ToString** siempre debe invalidar la implementación de clase base. Puede ocultar un **ToString** implementación invocándola en una instancia de clase fuertemente tipada.

> [!NOTE]
> En determinadas condiciones, las llamadas desde código nativo a un tipo administrado que implementa **IStringable** u oculta su **ToString** implementación puede generar un comportamiento inesperado.

## <a name="asynchronous-operations"></a>Operaciones asincrónicas

Para implementar un método asincrónico en el componente, agregue "Async" al final del nombre del método y devolver una de las interfaces de Windows en tiempo de ejecución que representan acciones u operaciones asincrónicas: **IAsyncAction**, **IAsyncActionWithProgress&lt;TProgress&gt;** , **IAsyncOperation&lt;TResult&gt;** , o **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** .

Puede usar tareas de .NET Framework (la [ **tarea** ](/dotnet/api/system.threading.tasks.task) genéricas y clase [ **tarea&lt;TResult&gt;**  ](/dotnet/api/system.threading.tasks.task-1) clase) para Implemente el método asincrónico. Debe devolver una tarea que representa una operación en curso, por ejemplo, una tarea que se devuelve desde un método asincrónico escrito C# o Visual Basic, o una tarea que se devuelve desde el [ **Task.Run** ](/dotnet/api/system.threading.tasks.task.run) método. Si usas un constructor para crear la tarea, debes llamar a su método [Task.Start](/dotnet/api/system.threading.tasks.task.start) antes de devolverlo.

Un método que utiliza `await` (`Await` en Visual Basic) requiere el `async` palabra clave (`Async` en Visual Basic). Si expone un método de este tipo desde un componente en tiempo de ejecución de Windows, aplicar el `async` palabra clave para el delegado que se pasa a la **ejecutar** método.

Para realizar acciones y operaciones asincrónicas que no son compatibles con la cancelación o los informes de progreso, puedes usar el método de extensión [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) o [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) para encapsular la tarea en la interfaz adecuada. Por ejemplo, el código siguiente implementa un método asincrónico mediante el uso de la **Task.Run&lt;TResult&gt;**  método para iniciar una tarea. El **AsAsyncOperation&lt;TResult&gt;**  método de extensión devuelve la tarea como una operación asincrónica de Windows en tiempo de ejecución.

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

El siguiente código JavaScript muestra cómo se podría llamar al método mediante el uso de un [ **WinJS.Promise** ](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) objeto. La función que se pasa al método a continuación se ejecuta cuando se completa la llamada asincrónica. El parámetro stringList contiene la lista de cadenas devuelta por la **DownloadAsStringAsync** método y la función realiza el procesamiento que sea necesario.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Para las acciones asincrónicas y las operaciones que admiten la cancelación o informes de progreso, utilice el [ **AsyncInfo** ](/dotnet/api/system.runtime.interopservices.windowsruntime) clase para generar una tarea iniciada y enlazar la cancelación y los informes de progreso características de la tarea con la cancelación y características de la interfaz de Windows en tiempo de ejecución adecuada para informar del progreso. Para obtener un ejemplo que admite la cancelación y notificación sobre el progreso, consulte [Tutorial: Crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Tenga en cuenta que puede usar los métodos de la **AsyncInfo** clase incluso si el método asincrónico no admite cancelación o informes de progreso. Si usa una función lambda de Visual Basic o un C# método anónimo, no proporcionar parámetros al token y [ **IProgress&lt;T&gt;**  ](https://docs.microsoft.com/dotnet/api/system.iprogress-1?redirectedfrom=MSDN) interfaz. Si usas una función lambda de C#, facilita un parámetro de token, pero omítelo. El ejemplo anterior, que usa el AsAsyncOperation&lt;TResult&gt; método, este aspecto cuando se usa el [ **AsyncInfo.Run&lt;TResult&gt;(Func&lt; CancellationToken, Task&lt;TResult&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime?redirectedfrom=MSDN)) sobrecarga del método en su lugar.

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

Si crea un método asincrónico que admite opcionalmente cancelación o informes de progreso, considere la posibilidad de agregar sobrecargas que no tengan parámetros para un token de cancelación o el **IProgress&lt;T&gt;**  interfaz.

## <a name="throwing-exceptions"></a>Iniciar excepciones

Se puede iniciar cualquier tipo de excepción que esté incluida en .NET para las aplicaciones de Windows. No puedes declarar tus propios tipos de excepción públicos en un componente de Windows Runtime, pero puedes declarar e iniciar tipos no públicos.

Si tu componente no controla la excepción, se genera la excepción correspondiente en el código que llamó a tu componente. La apariencia de la excepción para el llamador depende de la manera en que el lenguaje de la llamada sea compatible con Windows Runtime.

-   En JavaScript, la excepción aparecerá como un objeto en el que el mensaje de excepción se reemplaza por un seguimiento de la pila. Cuando depuras tu aplicación en Visual Studio, puedes ver el texto del mensaje original en el cuadro de diálogo de excepción del depurador, identificado como "WinRT Information". No puedes acceder al texto del mensaje original desde el código de JavaScript.

    > **Sugerencia**. Actualmente, el seguimiento de pila contiene el tipo de excepción administrada, pero no recomendamos analizar el seguimiento para identificar el tipo de excepción. En su lugar, usa un valor de HRESULT según se describe más adelante en este apartado.

-   En C++, la excepción aparece como una excepción de la plataforma. Si la propiedad HResult de la excepción administrada se puede asignar al valor HRESULT de una excepción de plataforma concreta, se usa la excepción específica; en caso contrario, un [ **Platform:: COMException** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) es una excepción. El texto del mensaje de la excepción administrada no está disponible para el código C++. Si se inició una excepción de plataforma específica, aparece el texto del mensaje predeterminado para ese tipo de excepción; de lo contrario, no se muestra ningún texto de mensaje. Consulta [Excepciones (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx).
-   En C# o Visual Basic, la excepción es una excepción administrada normal.

Cuando inicias una excepción desde tu componente, puedes facilitar a un llamador de JavaScript o C++ el control de la excepción iniciando un tipo de excepción no pública cuyo valor de propiedad de HResult sea específico para tu componente. El valor HRESULT está disponible para un llamador de JavaScript a través de la propiedad número del objeto de excepción y a un llamador de C++ a través de la [ **COMException** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) propiedad.

> [!NOTE]
> Utilice un valor negativo para el valor de HRESULT. Un valor positivo se interpreta como éxito y no se inicia ninguna excepción en el llamador de JavaScript o C++.

## <a name="declaring-and-raising-events"></a>Declarar y generar eventos

Cuando se declara un tipo para retener los datos para tu evento, deriva de Object en lugar de EventArgs, ya que EventArgs no es un tipo de Windows Runtime. Usar [ **EventHandler&lt;TEventArgs&gt;**  ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN) como el tipo de evento y use el argumento de evento tipo como argumento de tipo genérico. Genera el evento del mismo modo que en una aplicación de .NET Framework.

Cuando se usa tu componente de Windows Runtime desde JavaScript o C++, el evento sigue el modelo de eventos de Windows Runtime que esperan estos lenguajes. Cuando usas el componente de C# o Visual Basic, el evento aparece como un evento de .NET Framework normal. Se proporciona un ejemplo en [Tutorial: Crear un componente simple en C# o Visual Basic y llamarlo desde JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Si implementas descriptores de acceso de eventos personalizados (declaras un evento con la palabra clave **Custom** en Visual Basic), debes seguir el patrón de eventos de Windows Runtime en tu implementación. Consulta [Eventos y descriptores de acceso de eventos personalizados en componentes de Windows Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Ten en cuenta que cuando controlas el evento desde el código de C# o Visual Basic, seguirá pareciendo un evento de .NET Framework ordinario.

## <a name="next-steps"></a>Pasos siguientes

Una vez que hayas creado un componente de Windows Runtime para tu propio uso, es posible que observes que la funcionalidad que encapsula es útil para otros desarrolladores. Tienes dos opciones para empaquetar un componente para su distribución a otros desarrolladores. Consulta [Distribuir un componente de Windows Runtime administrado](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140)).

Para obtener más información acerca de las características de los lenguajes C# y Visual Basic, así como de la compatibilidad de .NET Framework con Windows Runtime, consulta [Referencia de los lenguajes Visual Basic y C#](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).

## <a name="related-topics"></a>Temas relacionados
* [.NET para aplicaciones UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Tutorial: Crear un componente de tiempo de ejecución de Windows Simple y llamarlo desde JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
