---
title: 'Pista de aprendizaje: mostrar clientes en una lista'
description: Información sobre lo que necesitas para mostrar una colección de objetos de cliente en una lista.
ms.date: 05/07/2018
ms.topic: article
keywords: introducción;uwp;windows 10;pista de aprendizaje;enlace de datos;lista;get started;learning track;data binding;list
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3cebf51bdf9fa9942a0b88ed7b4cf66204671781
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "71340337"
---
# <a name="display-customers-in-a-list"></a>Mostrar clientes en una lista

La visualización y manipulación de datos reales en la interfaz de usuario es fundamental para la funcionalidad de muchas aplicaciones. En este artículo se mostrará lo que necesitas saber para mostrar una colección de objetos de cliente en una lista.

Este no es un tutorial. Si estás buscando uno, consulta nuestro [tutorial de enlace de datos](../data-binding/xaml-basics-data-binding.md), que te proporcionará una experiencia paso a paso guiada.

Empezaremos con una descripción rápida del enlace de datos: qué es y cómo funciona. A continuación, agregaremos una **ListView** a la interfaz de usuario, agregaremos el enlace de datos y personalizaremos el enlace de datos con características adicionales.

## <a name="what-do-you-need-to-know"></a>¿Qué debes saber?

El enlace de datos es una forma de mostrar los datos de la aplicación en su interfaz de usuario. Esto permite la *separación de problemas* en tu aplicación, y se la interfaz de usuario se mantiene independiente del resto del código. Esto crea un modelo conceptual más limpio que es más fácil de leer y mantener.

Cada enlace de datos tiene dos partes:

* Un origen que proporciona los datos que se van a enlazar.
* Un destino en la interfaz de usuario donde se van a mostrar los datos.

Para implementar un enlace de datos, tendrás que agregar código al origen que proporciona datos para el enlace. También tendrás que agregar una de las dos extensiones de marcado al archivo XAML para especificar las propiedades de origen de datos. Esta es la diferencia principal entre las dos:

* [**x:Bind**](../xaml-platform/x-bind-markup-extension.md) está fuertemente tipado y genera código en tiempo de compilación para un mejor rendimiento. x:Bind se establece de forma predeterminada para enlaces de un solo uso, por lo que está optimizado para la visualización rápida de datos de solo lectura que no cambian.
* [**Binding**](../xaml-platform/binding-markup-extension.md) está poco tipado y se ensambla en tiempo de ejecución. Como resultado, su rendimiento es más pobre que el de x:Bind. En casi todos los casos, debes usar x:Bind en lugar de Binding. Sin embargo, es probable que lo encuentres en código antiguo. Binding se establece de forma predeterminada en transferencia de datos unidireccional, por lo que está optimizado para datos de solo lectura que pueden cambiar en el origen.

Te recomendamos que uses **x:Bind** siempre que sea posible y lo mostraremos en los fragmentos de código de este artículo. Para obtener más información sobre las diferencias, consulta [Comparación de características de {x:Bind} y {Binding}](../data-binding/data-binding-in-depth.md#xbind-and-binding-feature-comparison).

## <a name="create-a-data-source"></a>Crear un origen de datos

En primer lugar, necesitarás una clase para representar los datos del cliente. Como punto de referencia, te enseñaremos el proceso en este ejemplo básico:

```csharp
public class Customer
{
    public string Name { get; set; }
}
```

## <a name="create-a-list"></a>Crear una lista

Antes de mostrar a los clientes, debes crear la lista que va a contenerlos. La [vista de lista](../design/controls-and-patterns/listview-and-gridview.md) es un control XAML básico ideal para esta tarea. Tu ListView actualmente requiere una posición en la página y pronto necesitará un valor para su propiedad **ItemSource**.

```xaml
<ListView ItemsSource=""
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
```

Una vez que hayas enlazado datos a tu ListView, te animamos a volver a la documentación y experimentar con el proceso de personalización de su aspecto y diseño que mejor se ajuste a tus necesidades.

## <a name="bind-data-to-your-list"></a>Enlazar datos a la lista

Ahora que has realizado una interfaz de usuario básica para contener los enlaces, debes configurar el origen para proporcionarlos. Este es un ejemplo de cómo puede hacerse:

```csharp
public sealed partial class MainPage : Page
{
    public ObservableCollection<Customer> Customers { get; }
        = new ObservableCollection<Customer>();

    public MainPage()
    {
        this.InitializeComponent();
          // Add some customers
        this.Customers.Add(new Customer() { Name = "NAME1"});
        this.Customers.Add(new Customer() { Name = "NAME2"});
        this.Customers.Add(new Customer() { Name = "NAME3"});
    }
}
```
```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBlock Text="{x:Bind Name}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

La [Introducción al enlace de datos](../data-binding/data-binding-quickstart.md#binding-to-a-collection-of-items) te guía a través de un problema similar, en su sección sobre el enlace a una colección de elementos. Este ejemplo muestra los siguientes pasos fundamentales:

* En el código subyacente de la interfaz de usuario, crea una propiedad de tipo **ObservableCollection<T>** que contenga los objetos de cliente.
* Enlaza el elemento **ItemSource** de ListView a dicha propiedad.
* Proporciona una plantilla **ItemTemplate** básica para la ListView, que configurará cómo se muestra cada elemento de la lista.

No dudes en volver a examinar los documentos de la [vista de lista](../design/controls-and-patterns/listview-and-gridview.md) si quieres personalizar el diseño, agregar la selección de elementos o hacer ajustes a la **DataTemplate** que acabas de hacer. Pero, ¿qué ocurre si quieres editar a los clientes?

## <a name="edit-your-customers-through-the-ui"></a>Editar a tus clientes a través de la interfaz de usuario

Ya mostraste los clientes en una lista, pero el enlace de datos te permite hacer más. ¿Qué ocurriría si pudieras editar los datos directamente desde la interfaz de usuario? Para ello, primero hablemos sobre los tres modos de enlace de datos:

* *One-Time*: este enlace de datos solo se activa una vez y no reacciona ante cambios.
* *One-Way*: este enlace de datos actualizará la interfaz de usuario con los cambios realizados en el origen de datos.
* *Two-Way*: este enlace de datos actualizará la interfaz de usuario con los cambios realizados en el origen de datos y también actualizará los datos con los cambios realizados en la interfaz de usuario.

Si seguiste los fragmentos de código anteriores, el enlace creado usa x:Bind y no especifica un modo, por lo que es un enlace de un solo uso. Si quieres editar a tus clientes directamente desde la interfaz de usuario, deberás cambiarlo a un enlace bidireccional, a fin de que los cambios de los datos se pasen a los objetos de cliente. Consulta [Enlace de datos en profundidad](../data-binding/data-binding-in-depth.md) para más información.

El enlace bidireccional también actualizará la interfaz de usuario si se cambia el origen de datos. Para que esto funcione, debes implementar [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged) en el origen y asegurarte de que los establecedores de propiedades generan el evento **PropertyChanged**. Es una práctica habitual que llamen a un método auxiliar como **OnPropertyChanged**, tal como se muestra a continuación:

```csharp
public class Customer : INotifyPropertyChanged
{
    private string _name;
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
                {
                    _name = value;
                    this.OnPropertyChanged();
                }
        }
    }
    public event PropertyChangedEventHandler PropertyChanged;

    public void OnPropertyChanged([CallerMemberName] string propertyName = null) =>
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
}
```
A continuación, haz que el texto en tu ListView sea editable utilizando un **TextBox** en lugar de un **TextBlock** y asegúrate de establecer el valor de **Mode** de los enlaces de datos en **TwoWay**.

```xaml
<ListView ItemsSource="{x:Bind Customers}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Customer">
            <TextBox Text="{x:Bind Name, Mode=TwoWay}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

Una forma rápida de asegurarte de que funciona es agregar una segunda ListView con controles TextBox y enlaces OneWay. Los valores en la segunda lista cambiarán automáticamente mientras se edita la primera.

> [!NOTE]
> La edición directamente desde dentro de una ListView es una forma sencilla de mostrar un enlace bidireccional en acción, pero puede ocasionar a complicaciones de usabilidad. Si buscas llevar tu aplicación más lejos, considera usar [otros controles XAML](../design/controls-and-patterns/controls-and-events-intro.md) para editar tus datos y conservar tu ListView como de solo visualización.

## <a name="going-further"></a>Ir más allá

Ahora que has creado una lista de clientes con un enlace bidireccional, no dudes en examinar los documentos a los que te vinculamos y experimentar. También puedes ver nuestro [tutorial de enlace de datos](../data-binding/xaml-basics-data-binding.md) si quieres un tutorial paso a paso de enlaces básicos y avanzados, o investigar controles como el [patrón de maestro y detalles](../design/controls-and-patterns/master-details.md) para hacer una interfaz de usuario más sólida.

## <a name="useful-apis-and-docs"></a>API y documentos de utilidad

Este es un resumen rápido de las API y otra documentación útiles que te ayudarán a comenzar a trabajar con los enlaces de datos.

### <a name="useful-apis"></a>API útiles

| API | Descripción |
|------|---------------|
| [Plantilla de datos](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) | Describe la estructura visual de un objeto de datos, lo que permite la visualización de elementos específicos de la interfaz de usuario. |
| [x:Bind](../xaml-platform/x-bind-markup-extension.md) | Documentación sobre la extensión de marcado x:Bind recomendada. |
| [Binding](../xaml-platform/binding-markup-extension.md) | Documentación sobre la extensión de marcado Binding antigua. |
| [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) | Control de interfaz de usuario que muestra los elementos de datos en una pila vertical. |
| [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) | Control de texto básico para mostrar datos de texto editable en la interfaz de usuario. |
| [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged) | Interfaz para hacer que los datos sean observables al proporcionárselos a un enlace de datos. |
| [ItemsControl](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) | La propiedad **ItemsSource** de esta clase permite enlazar una ListView a un origen de datos. |

### <a name="useful-docs"></a>Documentos útiles

| Tema | Descripción |
|-------|----------------|
| [Enlace de datos en profundidad](../data-binding/data-binding-in-depth.md) | Introducción básica a los principios del enlace de datos. |
| [Introducción al enlace de datos](../data-binding/data-binding-quickstart.md) | Información conceptual detallada sobre el enlace de datos. |
| [Vista de lista](../design/controls-and-patterns/listview-and-gridview.md) | Información sobre cómo crear y configurar una ListView, incluida la implementación de una **DataTemplate** |

## <a name="useful-code-samples"></a>Ejemplos de código útiles

| Ejemplo de código | Descripción |
|-----------------|---------------|
| [Tutorial de enlace de datos](../data-binding/xaml-basics-data-binding.md) | Experiencia paso a paso guiada a través de los conceptos básicos del enlace de datos. |
| [ListView y GridView](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView) | Explora ListViews con enlace de datos más complejas. |
| [QuizGame](https://github.com/Microsoft/Windows-appsample-networkhelper) | Mira al enlace de datos en acción, incluida la clase **BindableBase** (en la carpeta "Common") para una implementación estándar de **INotifyPropertyChanged**. |
