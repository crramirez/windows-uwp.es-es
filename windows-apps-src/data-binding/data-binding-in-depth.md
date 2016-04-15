---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Enlace de datos en profundidad
description: El enlace de datos es una forma de que la interfaz de usuario de la aplicación muestre los datos y, opcionalmente, se mantenga sincronizada con dichos datos.
---
# Enlace de datos en profundidad

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


** API importantes **

-   [**Clase de enlace**](https://msdn.microsoft.com/library/windows/apps/BR209820)
-   [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713)
-   [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899)

**Nota**  En este tema se describen detalladamente las características del enlace de datos. Para obtener una introducción breve y práctica, consulta [Introducción al enlace de datos](data-binding-quickstart.md).

 

El enlace de datos es una forma en que la interfaz de usuario de la aplicación muestra los datos y, opcionalmente, se mantiene sincronizada con dichos datos. Enlace de datos permite separar la preocupación de los datos de la preocupación de la interfaz de usuario y que da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación.

Puedes usar el enlace de datos para simplemente mostrar los valores de un origen de datos aparece en primer lugar la interfaz de usuario, pero no para responder a cambios en dichos valores. Esto se denomina enlace único y funciona de forma perfecta para datos cuyos valores no cambian durante el tiempo de ejecución. Además, puedes elegir "observar" los valores y actualizar la interfaz de usuario cuando cambien. Esto se denomina enlace unidireccional y funciona bien para datos de solo lectura. En última instancia, puedes elegir observar y actualizar para que los cambios que realiza el usuario a los valores de la interfaz de usuario se envíen automáticamente al origen de datos. Esto se denomina enlace bidireccional y funciona bien para datos de lectura/escritura. A continuación se muestran algunos ejemplos.

-   Puedes usar el enlace único para enlazar una [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) a la foto del usuario actual.
-   Puedes usar el enlace unidireccional para enlazar una [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a una colección de artículos de noticias en tiempo real agrupados por sección periódicos.
-   Puedes usar un enlace bidireccional para enlazar un [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) al nombre de un cliente en un formulario.

Existen dos tipos de enlaces y generalmente ambos se declaran en el marcado de interfaz de usuario. Puedes usar la [extensión de marcado {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) o [extensión de marcado {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Incluso puedes usar una combinación de ambos en la misma aplicación, aun en el mismo elemento de la interfaz de usuario. {x:Bind} es nuevo para Windows 10 y tiene un mejor rendimiento. {Binding} tiene más características. Todos los detalles que se describe en este tema se aplican a ambos tipos de enlace, a menos que especifiquemos explícitamente lo contrario.

**Aplicaciones de ejemplo que muestran {x:Bind}**

-   [Ejemplo de {x:Bind}](http://go.microsoft.com/fwlink/p/?linkid=619989).
-   [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame).
-   [Ejemplo de conceptos básicos de interfaz de usuario de XAML](http://go.microsoft.com/fwlink/p/?linkid=619992).

**Aplicaciones de ejemplo que muestran {Binding}**

-   Descarga la aplicación [Bookstore1](http://go.microsoft.com/fwlink/?linkid=532950).
-   Descarga la aplicación [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952).

Cada enlace implica estas piezas
------------------------------------

-   Un *origen de enlace*. Este es el origen de los datos para el enlace y puede ser una instancia de cualquier clase que tenga miembros cuyos valores que quieres mostrar en la interfaz de usuario.
-   Un *destino de enlace*. Se trata de una [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362) del [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) en la interfaz de usuario que muestra los datos.
-   Un *objeto de enlace*. Este es el fragmento que transfiere los valores de datos del origen al destino y, opcionalmente, del destino nuevamente al origen. Se crea el objeto de enlace en tiempo de carga de XAML desde tu [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) o la extensión de marcado [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

En las siguientes secciones, se echaremos un vistazo al origen de enlace, el destino de enlace y el objeto de enlace. Además, vincularemos las secciones junto con el ejemplo de enlace de contenido de un botón a una propiedad de cadena denominada **NextButtonText**, que pertenece a una clase denominada **HostViewModel**.

Origen de enlace
--------------

Esta es una implementación muy rudimentaria de una clase que podríamos usar como origen de enlace.

**Nota**  Si estás usando [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) con extensiones de componentes de Visual C++ (C++/CX), tendrás que agregar el atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) a la clase de origen de enlace. Si estás usando [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), no necesitas este atributo. Consulta [Agregar una vista de detalles](data-binding-quickstart.md#adding-a-details-view) para un fragmento de código.

```csharp
public class HostViewModel
{
    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText { get; set; }
}
```

La implementación de **HostViewModel** y su propiedad **NextButtonText** solo es apropiada para el enlace único. Sin embargo, los enlace unidireccionales y bidireccionales son muy comunes y, en dichos tipos de enlaces, la interfaz de usuario se actualiza automáticamente en respuesta a cambios en los valores de datos del origen del enlace. Para que esos tipos de enlace funcionen correctamente, debes colocar al origen de enlace como "observable" en el objeto de enlace. Así, en nuestro ejemplo, si queremos un enlace unidireccional o bidireccional con la propiedad **NextButtonText** y, a continuación, cualquier cambio que se produzca en tiempo de ejecución en el valor de esa propiedad debe hacerse observable para el objeto de enlace.

Una manera de hacerlo es derivar la clase que representa el origen de enlace de [**DependencyObject**](https://msdn.microsoft.com/library/windows/apps/BR242356) y exponer un valor de datos a través de una [**DependencyProperty**](https://msdn.microsoft.com/library/windows/apps/BR242362). De este modo, un [**FrameworkElement**](https://msdn.microsoft.com/library/windows/apps/BR208706) pasa a ser observable. **FrameworkElements** son orígenes de enlace correctos sin necesidad de personalizarlos.

Una manera más ligera de hacer que una clase sea observable, y necesaria para las clases que ya tienen una clase base, es implementar [**System.ComponentModel.INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged). Esto simplemente implica la implementación de un solo evento denominado **PropertyChanged**. A continuación, se incluye un ejemplo usando **HostViewModel**.

**Nota**  Para c++ / CX, implementa [**Windows::UI::Xaml::Data::INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899), y la clase de origen de enlace debe tener atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) o implementa [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).

``` csharp
public class HostViewModel : INotifyPropertyChanged
{
    private string nextButtonText;

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set
        {
            this.nextButtonText = value;
            this.OnPropertyChanged();
        }
    }

    public void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        // Raise the PropertyChanged event, passing the name of the property whose value has changed.
        this.PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Ahora la propiedad **NextButtonText** es observable. Cuando creas una enlace unidireccional o bidireccional con esa propiedad (te mostraremos cómo más adelante), el objeto de enlace resultante se suscribe al evento **PropertyChanged**. Cuando se genera el evento, controlador del objeto de enlace recibe un argumento que contiene el nombre de la propiedad que ha cambiado. Así es cómo el objeto de enlace sabe a qué valor de propiedad dirigirse y volver a leer.

Para que no tengas que implementar el patrón mostrado anteriormente varias veces, solo puedes derivar de la clase base **BindableBase** que encontrarás en el ejemplo de [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) (en la carpeta "Common"). Este es un ejemplo de cómo queda.

``` csharp
public class HostViewModel : BindableBase
{
    private string nextButtonText;

    public HostViewModel()
    {
        this.NextButtonText = "Next";
    }

    public string NextButtonText
    {
        get { return this.nextButtonText; }
        set { this.SetProperty(ref this.nextButtonText, value); }
    }
}
```

Generar el evento **PropertyChanged** con un argumento de [**String.Empty**](T:System.String) o **null** indica que se deben volver a leer todas las propiedades no indexadoras del objeto. Puedes generar el evento para indica que las propiedades de indizador del objeto han cambiado mediante un argumento de "Item\[*indexer*\]" para indexadores específicos (donde *indexer* es el valor de índice) o el valor de "Item\[\]" para todos los indexadores.

Un objeto de enlace puede tratarse como un solo objeto cuyas propiedades contienen datos, o bien, como una colección de objetos. En el código de C# y Visual Basic, puedes enlazar por única vez a un objeto que implementa [**List(Of T)**](T:System.Collections.Generic.List%601) para mostrar una colección que no cambie en el tiempo de ejecución. Para una colección observable (observar cuando se agregan o quitan los elementos de la colección), se realiza un enlace unidireccional a [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) en su lugar. En el código de C++, puedes enlazar a [**Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/dn858385.aspx) para las colecciones observables y no observables. Para enlazar tus propias clases de colecciones, usa las directrices de la siguiente tabla.

| Situación                                                        | C# y VB (CLR)                                                                                                                                                                                                                                                                                                                                                                                                                   | C++/CX                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|-----------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Enlazar a un objeto.                                              | Puede ser cualquier objeto.                                                                                                                                                                                                                                                                                                                                                                                                                 | El objeto debe tener el atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) o implementa [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878).                                                                                                                                                                                                                                                                                                             |
| Obtener actualizaciones de cambios de propiedades de un objeto enlazado.                | El objeto debe implementar [**System.ComponentModel. INotifyPropertyChanged**](T:System.ComponentModel.INotifyPropertyChanged).                                                                                                                                                                                                                                                                                                         | El objeto debe implementar [**Windows.UI.Xaml.Data. INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/BR209899).                                                                                                                                                                                                                                                                                                                                                           |
| Enlazar a una colección.                                           | [**List(Of T)**](T:System.Collections.Generic.List%601)                                                                                                                                                                                                                                                                                                                                                                            | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Obtener actualizaciones de cambios de colecciones de una colección enlazada.          | [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601)                                                                                                                                                                                                                                                                                                                                        | [**Platform::Collections::Vector&lt;T&gt;**](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh441570.aspx)                                                                                                                                                                                                                                                                                                                                                                                         |
| Implementar una colección compatible con enlaces.                   | Extiende [**List(Of T)**](T:System.Collections.Generic.List%601) o implementa [**IList**](T:System.Collections.IList), [**IList**](T:System.Collections.Generic.IList%601)(Of [**Object**](T:System.Object)), [**IEnumerable**](T:System.Collections.IEnumerable), o [**IEnumerable**](T:System.Collections.Generic.IEnumerable%601)(Of **Object**). No se admite el enlace a **IList(Of T)** e **IEnumerable(Of T)** genéricos. | Implementa [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableIterable**](https://msdn.microsoft.com/library/windows/apps/Hh701957), [**IVector**](https://msdn.microsoft.com/library/windows/apps/BR206631)&lt;[**Object**](T:System.Object)^&gt;, [**IIterable**](https://msdn.microsoft.com/library/windows/apps/BR226024)&lt;**Object**^&gt; **IVector**&lt;[**IInspectable**](https://msdn.microsoft.com/library/BR205821)\*&gt; o **IIterable**&lt;**IInspectable**\*&gt;. No se admite el enlace a **IVector&lt;T&gt;** e **IIterable&lt;T&gt;** genéricos. |
| Implementar una colección que admita actualizaciones de cambios de colecciones. | Extiende [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) o implementa [**IList**](T:System.Collections.IList) e [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged) (no genéricos).                                                                                                                                                               | Implementa [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979) e [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974).                                                                                                                                                                                                                                                                                                                       |
| Implementar una colección compatible con la carga incremental.       | Extiende [**ObservableCollection(Of T)**](T:System.Collections.ObjectModel.ObservableCollection%601) o implementa [**IList**](T:System.Collections.IList) e [**INotifyCollectionChanged**](T:System.Collections.Specialized.INotifyCollectionChanged) (no genéricos). Además, implementa [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                          | Implementa [**IBindableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701979), [**IBindableObservableVector**](https://msdn.microsoft.com/library/windows/apps/Hh701974) e [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916).                                                                                                                                                                                                                                         |

 
Con la carga incremental, puedes enlazar controles de lista a orígenes de datos que son arbitrariamente de gran tamaño y aun así lograr un alto rendimiento. Por ejemplo, puedes enlazar controles de lista a resultados de consulta de imágenes de Bing sin tener que cargarlos a todos de una vez. Solo cargas algunos resultados inmediatamente y después cargas otros, según sea necesario. Para admitir la carga incremental, debes implementar [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916) en un origen de datos compatible con la notificación de cambios de colección. Cuando el motor de enlace de datos solicite más datos, tu origen de datos debe realizar las solicitudes apropiadas, integrar los resultados y después enviar las debidas notificaciones para actualizar la interfaz de usuario.

Destino de enlace
--------------

En los dos ejemplos siguientes, la propiedad **Button.Content** es el destino de enlace y su valor se establece en una extensión de marcado que declara el objeto de enlace. Se muestra el primer [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) y luego [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782). Declarar enlaces en el marcado es el caso común (es cómodo, legible y administrable). Sin embargo, puedes evitar el marcado y de manera imperativa (mediante programación) crear una instancia de la clase [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) en su lugar, si necesitas.

<!-- XAML lang specifier not yet supported in OP. Using XML for now. -->
``` xml
<Button Content="{x:Bind ...}" ... />
```

``` xml
<Button Content="{Binding ...}" ... />
```

Binding object declared using {x:Bind}
--------------------------------------

There's one step we need to do before we author our [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) markup. We need to expose our binding source class from the class that represents our page of markup. We do that by adding a property (of type **HostViewModel** in this case) to our **HostView** page class.

``` csharp
namespace QuizGame.View
{
    public sealed partial class HostView : Page
    {
        public HostView()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

Una vez que hayas hecho esto, puedes analizar más minuciosamente el marcado que declara el objeto de enlace. El siguiente ejemplo usa el mismo destino de enlace **Button.Content** que usamos en la sección "Destino de enlace" anteriormente, y muestra que está enlazado a la propiedad **HostViewModel.NextButtonText**.

``` xml
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page itself, and in this case the path begins by referencing the **ViewModel** property that we just added to the **HostView** page. That property returns a **HostViewModel** instance, and so we can dot into that object to access the **HostViewModel.NextButtonText** property. And we specify **Mode**, to override the [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) default of one-time.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). For other settings, see [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783).

**Note**  Changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus, and not after every user keystroke.

**DataTemplate and x:DataType**

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) (whether used as an item template, a content template, or a header template), the value of **Path** is not interpreted in the context of the page, but in the context of the data object being templated. So that its bindings can be validated (and efficient code generated for them) at compile-time, a **DataTemplate** needs to declare the type of its data object using **x:DataType**. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of **SampleDataGroup** objects.

``` xml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Weakly-typed objects in your Path**

Consider for example that you have a type named SampleDataGroup, which implements a string property named Title. And you have a property MainPage.SampleDataGroupAsObject, which is of type object but which actually returns an instance of SampleDataGroup. The binding `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` will result in a compile error because the Title property is not found on the type object. The remedy for this is to add a cast to your Path syntax like this: `<TextBlock Text="{x:Bind SampleDataGroupAsObject.(data:SampleDataGroup.Title)}"/>`. Here's another example where Element is declared as object but is actually a TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. And a cast remedies the issue: `<TextBlock Text="{x:Bind Element.(TextBlock.Text)}"/>`.

**Si los datos se cargan de forma asincrónica**

Se genera el código para admitir **{x:Bind}** en tiempo de compilación en las clases parciales para tus páginas. Estos archivos pueden encontrarse en la carpeta `obj`, con nombres como (para C#) `<view name>.g.cs`. The generated code includes a handler for your page's [**Loading**](https://msdn.microsoft.com/library/windows/apps/BR208706-loading) event, and that handler calls the **Initialize** method on a generated class that represent's your page's bindings. **Initialize** in turn calls **Update** to begin moving data between the binding source and the target. **Loading** is raised just before the first measure pass of the page or user control. So if your data is loaded asynchronously it may not be ready by the time **Initialize** is called. So, after you've loaded data, you can force one-time bindings to be initialized by calling `esto ->Bindings-> Update();`. Si solo necesitas enlaces de un solo uso para los datos cargados de forma asincrónica es mucho más barato inicializarlos así que es tener enlaces unidireccionales y escuchar los cambios. Si los datos no sufren cambios específicos y es probable que se actualicen como parte de una acción específica, puedes hacer que tus enlaces sean de un solo uso y forzar una actualización manual en cualquier momento con una llamada a **Update**.

**Limitaciones**

**{x:Bind}** no es apropiado para escenarios enlazados en tiempo de ejecución, como navegar por la estructura de diccionario de un objeto JSON, ni para duck typing (escritura de pato) que es una forma poco segura de escribir basada en coincidencias léxicas en los nombres de propiedad ("si camina, nada y grazna como un pato, es un pato"). Con duck typing, un enlace a la propiedad Edad se cumpliría por igual con un objeto Persona o un objeto Vino. Para estos escenarios, usa **{Binding}**.

Objeto de enlace declarado usando {Binding}
---------------------------------------

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) supone que, de manera predeterminada, estás enlazando al [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) de la página de marcado. Por lo tanto, estableceremos el **DataContext** de nuestra página para que sea una instancia de la clase de origen de enlace (de tipo **HostViewModel** en este caso). El siguiente ejemplo muestra el marcado que declara el objeto de enlace. Usamos el mismo destino de enlace **Button.Content** que usamos en la sección "Destino de enlace" anterior, y enlazamos a la propiedad **HostViewModel.NextButtonText**.

``` xml
<Page xmlns:viewmodel="using:QuizGame.ViewModel" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

Notice the value that we specify for **Path**. This value is interpreted in the context of the page's [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713), which in this example is set to an instance of **HostViewModel**. The path references the **HostViewModel.NextButtonText** property. We can omit **Mode**, because the [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) default of one-way works here.

The default value of [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) for a UI element is the inherited value of its parent. You can of course override that default by setting **DataContext** explicitly, which is in turn inherited by children by default. Setting **DataContext** explicitly on an element is useful when you want to have multiple bindings that use the same source.

A binding object has a **Source** property, which defaults to the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) of the UI element on which the binding is declared. You can override this default by setting **Source**, **RelativeSource**, or **ElementName** explicitly on the binding (see [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) for details).

Inside a [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348), the [**DataContext**](https://msdn.microsoft.com/library/windows/apps/BR208713) is set to the data object being templated. The example given below could be used as the **ItemTemplate** of an items control bound to a collection of any type that has string properties named **Title** and **Description**.

``` xml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

**Note**  By default, changes to [**TextBox.Text**](https://msdn.microsoft.com/library/windows/apps/BR209683-text) are sent to a two-way bound source when the [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) loses focus. To cause changes to be sent after every user keystroke, set **UpdateSourceTrigger** to **PropertyChanged** on the binding in markup. You can also completely take control of when changes are sent to the source by setting **UpdateSourceTrigger** to **Explicit**. You then handle events on the text box (typically [**TextBox.TextChanged**](https://msdn.microsoft.com/library/windows/apps/BR209683-textchanged)), call [**GetBindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR208706-getbindingexpression) on the target to get a [**BindingExpression**](https://msdn.microsoft.com/library/windows/apps/BR209820expression) object, and finally call [**BindingExpression.UpdateSource**](https://msdn.microsoft.com/library/windows/apps/BR209820expression-updatesource) to programmatically update the data source.

The [**Path**](https://msdn.microsoft.com/library/windows/apps/BR209820-path) property supports a variety of syntax options for binding to nested properties, attached properties, and integer and string indexers. For more info, see [Property-path syntax](https://msdn.microsoft.com/library/windows/apps/Mt185586). Binding to string indexers gives you the effect of binding to dynamic properties without having to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878). The [**ElementName**](https://msdn.microsoft.com/library/windows/apps/BR209820-elementname) property is useful for element-to-element binding. The [**RelativeSource**](https://msdn.microsoft.com/library/windows/apps/BR209820-relativesource) property has several uses, one of which is as a more powerful alternative to template binding inside a [**ControlTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209391). For other settings, see [{Binding} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204782) and the [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820) class.

What if the source and the target are not the same type?
--------------------------------------------------------

If you want to control the visibility of a UI element based on the value of a boolean property, or if you want to render a UI element with a color that's a function of a numeric value's range or trend, or if you want to display a date and/or time value in a UI element property that expects a string, then you'll need to convert values from one type to another. There will be cases where the right solution is to expose another property of the right type from your binding source class, and keep the conversion logic encapsulated and testable there. But that isn't flexible nor scalable when you have large numbers, or large combinations, of source and target properties. In that case you'll want to use something known as a value converter. This section describes how to implement and consume a value converter.

Here's a value converter, suitable for a one-time or a one-way binding, that converts a [**DateTime**](T:System.DateTime) value to a string value containing the month. The class implements [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

``` csharp
public class DateToStringConverter : IValueConverter
{
    // Define the Convert method to convert a DateTime value to 
    // a month string.
    public object Convert(object value, Type targetType, 
        object parameter, string language)
    {
        // value is the data from the source object.
        DateTime thisdate = (DateTime)value;
        int monthnum = thisdate.Month;
        string month;
        switch (monthnum)
        {
            case 1:
                month = "January";
                break;
            case 2:
                month = "February";
                break;
            default:
                month = "Month not found";
                break;
        }
        // Return the value to pass to the target.
        return month;
    }

    // ConvertBack is not implemented for a OneWay binding.
    public object ConvertBack(object value, Type targetType, 
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

``` vbnet
Public Class DateToStringConverter
    Implements IValueConverter

    ' Define the Convert method to change a DateTime object to
    ' a month string.
    Public Function Convert(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.Convert

        ' value is the data from the source object.
        Dim thisdate As DateTime = CType(value, DateTime)
        Dim monthnum As Integer = thisdate.Month
        Dim month As String
        Select Case (monthnum)
            Case 1
                month = "January"
            Case 2
                month = "February"
            Case Else
                month = "Month not found"
        End Select
        ' Return the value to pass to the target.
        Return month

    End Function

    ' ConvertBack is not implemented for a OneWay binding.
    Public Function ConvertBack(ByVal value As Object, -
        ByVal targetType As Type, ByVal parameter As Object, -
        ByVal language As String) As Object -
        Implements IValueConverter.ConvertBack

        Throw New NotImplementedException

    End Function
End Class
```

Y esta es la forma de consumir ese convertidor de valores en el marcado del objeto de enlace.

``` xml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>

...

<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>

<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

The binding engine calls the [**Convert**](https://msdn.microsoft.com/library/windows/apps/BR209903-convert) and [**ConvertBack**](https://msdn.microsoft.com/library/windows/apps/BR209903-convertback) methods if the [**Converter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converter) parameter is defined for the binding. When data is passed from the source, the binding engine calls **Convert** and passes the returned data to the target. When data is passed from the target (for a two-way binding), the binding engine calls **ConvertBack** and passes the returned data to the source.

The converter also has optional parameters: [**ConverterLanguage**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterlanguage), which allows specifying the language to be used in the conversion, and [**ConverterParameter**](https://msdn.microsoft.com/library/windows/apps/BR209820-converterparameter), which allows passing a parameter for the conversion logic. For an example that uses a converter parameter, see [**IValueConverter**](https://msdn.microsoft.com/library/windows/apps/BR209903).

**Note**  If there is an error in the conversion, do not throw an exception. Instead, return [**DependencyProperty.UnsetValue**](https://msdn.microsoft.com/library/windows/apps/BR242362-unsetvalue), which will stop the data transfer.

To display a default value to use whenever the binding source cannot be resolved, set the **FallbackValue** property on the binding object in markup. This is useful to handle conversion and formatting errors. It is also useful to bind to source properties that might not exist on all objects in a bound collection of heterogeneous types.

If you bind a text control to a value that is not a string, the data binding engine will convert the value to a string. If the value is a reference type, the data binding engine will retrieve the string value by calling [**ICustomPropertyProvider.GetStringRepresentation**](https://msdn.microsoft.com/library/windows/apps/BR209878-getstringrepresentation) or [**IStringable.ToString**](https://msdn.microsoft.com/library/Dn302136) if available, and will otherwise call [**Object.ToString**](M:System.Object.ToString). Note, however, that the binding engine will ignore any **ToString** implementation that hides the base-class implementation. Subclass implementations should override the base class **ToString** method instead. Similarly, in native languages, all managed objects appear to implement [**ICustomPropertyProvider**](https://msdn.microsoft.com/library/windows/apps/BR209878) and [**IStringable**](https://msdn.microsoft.com/library/Dn302135). However, all calls to **GetStringRepresentation** and **IStringable.ToString** are routed to **Object.ToString** or an override of that method, and never to a new **ToString** implementation that hides the base-class implementation.

Resource dictionaries with {x:Bind}
-----------------------------------

The [{x:Bind} markup extension](https://msdn.microsoft.com/library/windows/apps/Mt204783) depends on code generation, so it needs a code-behind file containing a constructor that calls **InitializeComponent** (to initialize the generated code). You re-use the resource dictionary by instantiating its type (so that **InitializeComponent** is called) instead of referencing its filename. Here's an example of what to do if you have an existing resource dictionary and you want to use {x:Bind} in it.

TemplatesResourceDictionary.xaml

``` xml
<ResourceDictionary
    x:Class="ExampleNamespace.TemplatesResourceDictionary"
    .....
    xmlns:examplenamespace="using:ExampleNamespace">
    
    <DataTemplate x:Key="EmployeeTemplate" x:DataType="examplenamespace:IEmployee">
        <Grid>
            <TextBlock Text="{x:Bind Name}"/>
        </Grid>
    </DataTemplate>
</ResourceDictionary>
```

TemplatesResourceDictionary.xaml.cs

``` csharp
using Windows.UI.Xaml.Data;
 
namespace ExampleNamespace
{
    public partial class TemplatesResourceDictionary
    {
        public TemplatesResourceDictionary()
        {
            InitializeComponent();
        }
    }
}
```

MainPage.xaml

``` xml
<Page x:Class="ExampleNamespace.MainPage"
    ....
    xmlns:examplenamespace="using:ExampleNamespace">

    <Page.Resources>
        <ResourceDictionary>
            .... 
            <ResourceDictionary.MergedDictionaries>
                <examplenamespace:TemplatesResourceDictionary/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Page.Resources>
</Page>
```

Event binding and ICommand
--------------------------

[{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) supports a feature called event binding. With this feature, you can specify the handler for an event using a binding, which is an additional option on top of handling events with a method on the code-behind file. Let's say you have a **RootFrame** property on your **MainPage** class.

``` csharp
    public sealed partial class MainPage : Page
    {
        ....    
        public Frame RootFrame { get { return Window.Current.Content as Frame; } }
    }
```

Se puede enlazar el evento **Click** de un botón a un método en el objeto **Frame** devuelto por la propiedad **RootFrame** como esta. Ten en cuenta que también enlazamos la propiedad **IsEnabled** del botón a otro miembro del mismo **Frame**.

``` xml
    <AppBarButton Icon="Forward" IsCompact="True"
    IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
    Click="{x:Bind RootFrame.GoForward}"/>
```

Overloaded methods cannot be used to handle an event with this technique. Also, if the method that handles the event has parameters then they must all be assignable from the types of all of the event's parameters, respectively. In this case, [**Frame.GoForward**](https://msdn.microsoft.com/library/windows/apps/BR242693) is not overloaded and it has no parameters (but it would still be valid even if it took two **object** parameters). [**Frame.GoBack**](https://msdn.microsoft.com/library/windows/apps/Dn996568) is overloaded, though, so we can't use that method with this technique.

The event binding technique is similar to implementing and consuming commands (a command is a property that returns an object that implements the [**ICommand**](T:System.Windows.Input.ICommand) interface). Both [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) work with commands. So that you don't have to implement the command pattern multiple times, you can use the **DelegateCommand** helper class that you'll find in the [QuizGame](https://github.com/Microsoft/Windows-appsample-quizgame) sample (in the "Common" folder).

## Binding to a collection of folders or files

You can use the APIs in the [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346) namespace to retrieve folder and file data. However, the various **GetFilesAsync**, **GetFoldersAsync**, and **GetItemsAsync** methods do not return values that are suitable for binding to list controls. Instead, you must bind to the return values of the [**GetVirtualizedFilesVector**](https://msdn.microsoft.com/library/windows/apps/Hh701422), [**GetVirtualizedFoldersVector**](https://msdn.microsoft.com/library/windows/apps/Hh701428), and [**GetVirtualizedItemsVector**](https://msdn.microsoft.com/library/windows/apps/Hh701430) methods of the [**FileInformationFactory**](https://msdn.microsoft.com/library/windows/apps/BR207501) class. The following code example from the [StorageDataSource and GetVirtualizedFilesVector sample](http://go.microsoft.com/fwlink/p/?linkid=228621) shows the typical usage pattern. Remember to declare the **picturesLibrary** capability in your app package manifest, and confirm that there are pictures in your Pictures library folder.

``` csharp
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var library = Windows.Storage.KnownFolders.PicturesLibrary;
            var queryOptions = new Windows.Storage.Search.QueryOptions();
            queryOptions.FolderDepth = Windows.Storage.Search.FolderDepth.Deep;
            queryOptions.IndexerOption = Windows.Storage.Search.IndexerOption.UseIndexerWhenAvailable;

            var fileQuery = library.CreateFileQueryWithOptions(queryOptions);

            var fif = new Windows.Storage.BulkAccess.FileInformationFactory(
                fileQuery,
                Windows.Storage.FileProperties.ThumbnailMode.PicturesView,
                190,
                Windows.Storage.FileProperties.ThumbnailOptions.UseCurrentScale,
                false
                );

            var dataSource = fif.GetVirtualizedFilesVector();
            this.PicturesListView.ItemsSource = dataSource;
        }
```

Normalmente usarás este enfoque para crear una vista de solo lectura de la información de archivo y carpeta. Puedes crear enlaces bidireccionales a las propiedades de archivo y carpeta; por ejemplo, para permitir que los usuarios califiquen una canción en una vista de música. No obstante, ningún cambio persiste hasta que no llames al método **SavePropertiesAsync** apropiado (por ejemplo, [**MusicProperties.SavePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/BR207760)). Debes confirmar cambios cuando el elemento pierde el foco, porque esto desencadena el restablecimiento de la selección.

Ten en cuenta que el enlace bidireccional con esta técnica solo funciona con ubicaciones indizadas, como Música. Puedes determinar si una ubicación es indizada llamando al método [**FolderInformation.GetIndexedStateAsync**](https://msdn.microsoft.com/library/windows/apps/BR207627).

Ten en cuenta también que un vector virtualizado puede devolver **null** para algunos elementos antes de que rellene su valor. Por ejemplo, debes comprobar **null** antes de usar el valor [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) de un control de lista enlazado a un vector virtualizado o, en su lugar, usar [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/BR209768).

Enlace a datos agrupados por una clave
--------------------------------

Si tomas una colección plana de elementos (libros, por ejemplo, representados por una clase **BookSku**) y agrupas los elementos mediante una propiedad común, como una clave (la propiedad **BookSku.AuthorName**, por ejemplo), a continuación, el resultado se denomina datos agrupados. Al agrupar los datos, ya no es una colección plana. Datos agrupados están una colección de objetos de grupo, donde cada objeto de grupo tiene un) una clave y (b) una colección de elementos cuya propiedad coincide con dicha clave. Para realizar el ejemplo de libros de nuevo, el resultado de agrupar los libros por nombre del autor genera una colección de grupos de nombre del autor donde cada grupo tiene (a) una clave, que es un nombre de autor, y (b) una colección de los **BookSku** cuya propiedad **AuthorName** coincide con la clave del grupo.

En general, para mostrar una colección, enlazas el [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) de un control de elementos (como [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) o [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705)) directamente a una propiedad que devuelve una colección. Si es una colección de elementos plana no necesitas hacer nada especial. Pero si es una colección de objetos de grupo (como al enlazar a datos agrupados), a continuación, necesitas los servicios de un objeto intermediario llamado [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), que se encuentra entre el control de elementos y el origen del enlace. Enlaza el **CollectionViewSource** a la propiedad que devuelve datos agrupados y enlaza el control de elementos a **CollectionViewSource**. Un valor agregado adicional de un **CollectionViewSource** es que realiza el seguimiento del elemento actual, para poder mantener más de un control de elementos sincronizados al enlazarlos todos al mismo **CollectionViewSource**. Puedes acceder también al elemento actual mediante programación a través de la propiedad [**ICollectionView.CurrentItem**](https://msdn.microsoft.com/library/windows/apps/BR209857) del objeto devuelto por la propiedad [**CollectionViewSource.View**](https://msdn.microsoft.com/library/windows/apps/BR209833-view).

Para activar la función de agrupación de un [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), establece [**IsSourceGrouped**](https://msdn.microsoft.com/library/windows/apps/BR209833-issourcegrouped) en **true**. Si también debes establecer la propiedad [**ItemsPath**](https://msdn.microsoft.com/library/windows/apps/BR209833-itemspath) depende de cómo crees exactamente los objetos de grupo. Existen dos formas para crear un objeto de grupo: el patrón de "es un grupo" y el "tiene un grupo". En el modelo "es un grupo", el objeto de grupo se deriva de un tipo de colección (por ejemplo, **List&lt;T&gt;**), de modo que el objeto de grupo en realidad es el grupo de elementos. Con este modelo, es necesario establecer **ItemsPath**. En el modelo "tiene un grupo", el objeto de grupo tiene una o más propiedades de un tipo de colección (como **List&lt;T&gt;**), por lo que el grupo "tiene un" grupo de elementos en forma de una propiedad (o varios grupos de elementos en forma de varias propiedades). Con este modelo, debes establecer **ItemsPath** en el nombre de la propiedad que contiene el grupo de elementos.

El siguiente ejemplo muestra el modelo de "tiene un grupo". La clase de página tiene una propiedad denominada [**ViewModel**](https://msdn.microsoft.com/library/windows/apps/BR208713), que devuelve una instancia de nuestro modelo de vista. El [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) enlaza a la propiedad **Authors** del modelo de vista (**Authors** es la colección de objetos de grupo) y también especifica que es la propiedad **Author.BookSkus** que contiene los elementos agrupados. Por último, la [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) está enlazada al **CollectionViewSource**, y su estilo de grupo se define para que pueda representar los elementos en grupos.

``` csharp
    <Page.Resources>
        <CollectionViewSource
        x:Name="AuthorHasACollectionOfBookSku"
        Source="{x:Bind ViewModel.Authors}"
        IsSourceGrouped="true"
        ItemsPath="BookSkus"/>
    </Page.Resources>
    ...

    <GridView
    ItemsSource="{Binding Source={StaticResource AuthorHasACollectionOfBookSku}}" ...>
        <GridView.GroupStyle>
            <GroupStyle
                HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
        </GridView.GroupStyle>
    </GridView>
```

Note that the [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/BR242828) must use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) (and not [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) because it needs to set the **Source** property to a resource. To see the above example in the context of the complete app, download the [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app. Unlike the markup shown above, [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) uses {Binding} exclusively.

You can implement the "is-a-group" pattern in one of two ways. One way is to author your own group class. Derive the class from **List&lt;T&gt;** (where *T* is the type of the items). For example, `public class Author : List<BookSku>`. La segunda manera es usar un expresión [LINQ](http://msdn.microsoft.com/library/bb397926.aspx) para crear dinámicamente objetos de grupo (y una clase de grupo) desde valores de propiedad similares de los elementos **BookSku**. Este enfoque, mantener solo una lista de elementos y agrupar sobre la marcha, es típico de una aplicación que accede a los datos de un servicio de nube. Obtienes la flexibilidad de agrupar libros según el autor o el género (por ejemplo), sin necesidad de disponer de clases de grupo especiales, como **Author** y **Genre**.

El siguiente ejemplo muestra el modelo "es un grupo" usando [LINQ](http://msdn.microsoft.com/library/bb397926.aspx). Esta vez agrupamos libros por género, mostrados con el nombre de género en los encabezados de grupo. Esto se indica en la ruta de acceso de la propiedad "Key" en referencia al valor [**Key**](P:System.Linq.IGrouping%602.Key) del grupo.

``` csharp
    using System.Linq;

    ...

    private IOrderedEnumerable<IGrouping<string, BookSku>> genres;

    public IOrderedEnumerable<IGrouping<string, BookSku>> Genres
    {
        get
        {
            if (this.genres == null)
            {
                this.genres = from book in this.bookSkus
                group book by book.genre into grp
                orderby grp.Key select grp;
            }
            return this.genres;
        }
    }
```

Remember that when using [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) with data templates we need to indicate the type being bound to by setting an **x:DataType** value. If the type is generic then we can't express that in markup so we need to use [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) instead in the group style header template.

``` xml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{Binding Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{Binding Source={StaticResource GenreIsACollectionOfBookSku}}">
        <GridView.ItemTemplate x:DataType="local:BookTemplate">
            <DataTemplate>
                <TextBlock Text="{x:Bind Title}"/>
            </DataTemplate>
        </GridView.ItemTemplate>
        <GridView.GroupStyle>
            <GroupStyle>
                <GroupStyle.HeaderTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Key}"/>
                    </DataTemplate>
                </GroupStyle.HeaderTemplate>
            </GroupStyle>
        </GridView.GroupStyle>
    </GridView>
```

A [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/Hh702601) control is a great way for your users to view and navigate grouped data. The [Bookstore2](http://go.microsoft.com/fwlink/?linkid=532952) sample app illustrates how to use the **SemanticZoom**. In that app, you can view a list of books grouped by author (the zoomed-in view) or you can zoom out to see a jump list of authors (the zoomed-out view). The jump list affords much quicker navigation than scrolling through the list of books. The zoomed-in and zoomed-out views are actually **ListView** or **GridView** controls bound to the same **CollectionViewSource**.

![An illustration of a SemanticZoom](images/sezo.png)

When you bind to hierarchical data—such as subcategories within categories—you can choose to display the hierarchical levels in your UI with a series of items controls. A selection in one items control determines the contents of subsequent items controls. You can keep the lists synchronized by binding each list to its own [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) and binding the **CollectionViewSource** instances together in a chain. This is called a master/details (or list/details) view. For more info, see [How to bind to hierarchical data and create a master/details view](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

Diagnosing and debugging data binding problems
-----------------------------------------------

Your binding markup contains the names of properties (and, for C#, sometimes fields and methods). So when you rename a property, you'll also need to change any binding that references it. Forgetting to do that leads to a typical example of a data binding bug, and your app either won't compile or won't run correctly.

The binding objects created by [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) and [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) are largely functionally equivalent. But {x:Bind} has type information for the binding source, and it generates source code at compile-time. With {x:Bind} you get the same kind of problem detection that you get with the rest of your code. That includes compile-time validation of your binding expressions, and debugging by setting breakpoints in the source code generated as the partial class for your page. These classes can be found in the files in your `obj` folder, with names like (for C#) `<view name>.g.cs`). Si tiene un problema con un enlace y activar **interrumpir en las excepciones no controladas** en el depurador de Microsoft Visual Studio. El depurador interrumpirá la ejecución en ese punto y, a continuación, se pueden depurar lo que ha funcionado. El código generado por {x:Bind} sigue el mismo patrón para cada parte del gráfico de nodos de origen de enlace, y puedes usar la información de la ventana **Pila de llamadas** para ayudar a determinar la secuencia de llamadas que condujeron al problema.

[{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) no tiene información de tipo para el origen de enlace. Cuando ejecutas la aplicación con el depurador adjunto, los errores de enlace aparecen en la ventana **Resultado** en Visual Studio.

Crear enlaces en el código
-------------------------

**Nota**  Esta sección solo se aplica a [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782), porque no puedes crear enlaces [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) en el código. Sin embargo, algunas de las ventajas de {x:Bind} pueden conseguirse con [**DependencyProperty.RegisterPropertyChangedCallback**](https://msdn.microsoft.com/library/windows/apps/BR242356-registerpropertychangedcallback), que te permite registrar notificaciones de cambio en cualquier propiedad de dependencia.

También puedes conectar elementos de la interfaz de usuario a datos usando código de procedimientos en lugar de XAML. Para ello, crea un nuevo objeto [**Binding**](https://msdn.microsoft.com/library/windows/apps/BR209820), establece las propiedades correspondientes y luego llama a [**FrameworkElement.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR208706-setbinding) o [**BindingOperations.SetBinding**](https://msdn.microsoft.com/library/windows/apps/BR209820operations-setbinding). Crear enlaces mediante programación te resultará útil si quieres elegir los valores de propiedad de los enlaces en tiempo de ejecución o compartir un único enlace entre varios controles. Pero ten en cuenta que no puedes cambiar los valores de propiedad de los enlaces después de llamar a **SetBinding**.

En el siguiente ejemplo se muestra cómo implementar un enlace anterior en el código.

``` xml
<TextBox x:Name="MyTextBox" Text="Text"/>
```

```csharp
// Create an instance of the MyColors class 
// that implements INotifyPropertyChanged.
MyColors textcolor = new MyColors();

// Brush1 is set to be a SolidColorBrush with the value Red.
textcolor.Brush1 = new SolidColorBrush(Colors.Red);

// Set the DataContext of the TextBox MyTextBox.
MyTextBox.DataContext = textcolor;

// Create the binding and associate it with the text box.
Binding binding = new Binding() { Path = new PropertyPath("Brush1") };
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding);
```

``` vbnet
' Create an instance of the MyColors class 
' that implements INotifyPropertyChanged. 
Dim textcolor As New MyColors()

' Brush1 is set to be a SolidColorBrush with the value Red. 
textcolor.Brush1 = New SolidColorBrush(Colors.Red)

' Set the DataContext of the TextBox MyTextBox. 
MyTextBox.DataContext = textcolor

' Create the binding and associate it with the text box.
Dim binding As New Binding() With {.Path = New PropertyPath("Brush1")}
MyTextBox.SetBinding(TextBox.ForegroundProperty, binding)
```

Comparación entre las características {x:Bind} y {Binding}
------------------------------------------

| Característica | {x:Bind} | {Binding} | Notas | 
|---------|----------|-----------|-------|
| Path es la propiedad predeterminada | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propiedad Path | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | En x:Bind, la raíz de Path está en Page de forma predeterminada, no en DataContext. | 
| Indexador | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Enlaza al elemento especificado en la colección. Se admiten solamente en números enteros índices. | 
| Propiedades adjuntas | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Las propiedades adjuntas se especifican con paréntesis. Si la propiedad no está declarada en un espacio de nombres XAML, agrega un prefijo con un espacio de nombres XML, que debe estar asignado a un espacio de nombres de código al principio del documento. | 
| Conversión | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | No se necesita < | Las conversiones se especifican mediante paréntesis. Si la propiedad no está declarada en un espacio de nombres XAML, agrega un prefijo con un espacio de nombres XML, que debe estar asignado a un espacio de nombres de código al principio del documento. | 
| Convertidor | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Los convertidores deben declararse en la raíz de Page/ResourceDictionary o en App.xaml. | 
| ConverterParameter, ConverterLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Los convertidores deben declararse en la raíz de Page/ResourceDictionary o en App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Se usa cuando la hoja de la expresión de enlace es null. Usar comillas simples para un valor de cadena. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Se usa cuando cualquier parte de la ruta de acceso para el enlace (excepto para la hoja) es null. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Con {x:Bind} enlazas a un campo; la raíz de Path está en Page de forma predeterminada, por lo que se puede acceder a cualquier elemento con nombre a través de su campo. | 
| RelativeSource: Self | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | With {x:Bind}, name the element and use its name in Path. | 
| RelativeSource: TemplatedParent | Not supported | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | El enlace de plantilla normal puede usarse en las plantillas de control para la mayoría de los usos. Pero usa TemplatedParent donde necesites usar un convertidor o un enlace bidireccional.< | 
| Origen | No se admite | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | For {x:Bind} use a property or a static path instead. | 
| Mode | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | Mode can be OneTime, OneWay, or TwoWay. {x:Bind} defaults to OneTime; {Binding} defaults to OneWay. | 
| UpdateSourceTrigger | Not supported | `<Binding UpdateSourceTrigger="[Default | PropertyChanged | Explicit]"/>` | {x:Bind} usa el comportamiento PropertyChanged para todos los casos, excepto TextBox.Text, donde espera que la pérdida de enfoque actualice el origen. | 




<!--HONumber=Mar16_HO1-->


