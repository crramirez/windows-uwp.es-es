---
ms.assetid: 41E1B4F1-6CAF-4128-A61A-4E400B149011
title: Enlace de datos en profundidad
description: Obtenga más información sobre cómo usar el enlace de datos en las aplicaciones para la Plataforma universal de Windows (UWP).
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: a4fc9244c84b955b925fef9c3527778bdb3ab2ea
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133068"
---
# <a name="data-binding-in-depth"></a>Enlace de datos en profundidad

**API importantes**

-   [**Extensión de marcado {x:Bind}** ](../xaml-platform/x-bind-markup-extension.md)
-   [**Clase de enlace**](/uwp/api/Windows.UI.Xaml.Data.Binding)
-   [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext)
-   [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged)

> [!NOTE]
> En este tema se describen detalladamente las características del enlace de datos. Para obtener una introducción breve y práctica, consulta [Introducción al enlace de datos](data-binding-quickstart.md).

En este tema se describe el enlace de datos en las aplicaciones para la Plataforma universal de Windows (UWP). Las API que se describen aquí residen en el espacio de nombres [**Windows.UI.Xaml.Data**](/uwp/api/windows.ui.xaml.data).

El enlace de datos es una forma para que la interfaz de usuario de la aplicación muestre los datos y, opcionalmente, se mantenga sincronizada con dichos datos. El enlace de datos permite separar lo que concierne a los datos de lo que concierne a la interfaz de usuario, lo que da como resultado un modelo conceptual más sencillo y una mejor legibilidad, comprobación y mantenimiento de la aplicación.

Puedes usar el enlace de datos para simplemente mostrar los valores de un origen de datos aparece en primer lugar la interfaz de usuario, pero no para responder a cambios en dichos valores. Este es un modo de enlace denominado *único* y funciona bien para un valor que no cambia en tiempo de ejecución. También puedes elegir "observar" los valores y actualizar la interfaz de usuario cuando cambien. Este modo se denomina enlace *unidireccional* y funciona bien para datos de solo lectura. En última instancia, puedes elegir observar y actualizar para que los cambios que realiza el usuario a los valores de la interfaz de usuario se envíen automáticamente al origen de datos. Este modo se denomina enlace *bidireccional* y funciona bien para datos de lectura y escritura. A continuación se muestran algunos ejemplos.

-   Puedes usar el modo único para enlazar un elemento [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) con la foto del usuario actual.
-   Puedes usar el modo unidireccional para enlazar un elemento [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) con una colección de artículos de noticias en tiempo real agrupados por sección de periódico.
-   Puedes usar el modo bidireccional para enlazar un elemento [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) con el nombre de un cliente en un formulario.

Independientemente del modo, existen dos tipos de enlaces y, generalmente, ambos se declaran en el marcado de interfaz de usuario. Puedes usar la [extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md) o [extensión de marcado {Binding}](../xaml-platform/binding-markup-extension.md). Incluso puedes usar una combinación de ambos en la misma aplicación, aun en el mismo elemento de la interfaz de usuario. {x:Bind} es nuevo en Windows 10 y tiene un mejor rendimiento. Todos los detalles que se describen en este tema se aplican a ambos tipos de enlace, a menos que especifiquemos explícitamente lo contrario.

**Aplicaciones de ejemplo que muestran {x:Bind}**

-   [Ejemplo de {x:Bind}](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBind).
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper).
-   [Ejemplo de conceptos básicos de interfaz de usuario de XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics).

**Aplicaciones de ejemplo que muestran {Binding}**

-   Descarga la aplicación [Bookstore1](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).
-   Descarga la aplicación [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

## <a name="every-binding-involves-these-pieces"></a>Cada enlace implica estas piezas

-   Un *origen de enlace*. Este es el origen de los datos para el enlace y puede ser una instancia de cualquier clase que tenga miembros cuyos valores que quieres mostrar en la interfaz de usuario.
-   Un *destino de enlace*. Se trata de una [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) del [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) en la interfaz de usuario que muestra los datos.
-   Un *objeto de enlace*. Este es el fragmento que transfiere los valores de datos del origen al destino y, opcionalmente, del destino nuevamente al origen. Se crea el objeto de enlace en tiempo de carga de XAML desde tu [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) o la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md).

En las siguientes secciones, se echaremos un vistazo al origen de enlace, el destino de enlace y el objeto de enlace. Además, vincularemos las secciones junto con el ejemplo de enlace de contenido de un botón a una propiedad de cadena denominada **NextButtonText**, que pertenece a una clase denominada **HostViewModel**.

### <a name="binding-source"></a>Origen de enlace

Esta es una implementación muy rudimentaria de una clase que podríamos usar como origen de enlace.

Si usas [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), agrega nuevos elementos de **archivo MIDL (.idl)** al proyecto, con el nombre que se muestra en la lista del ejemplo de código C++/WinRT siguiente. Reemplaza el contenido de los archivos nuevos con el código [MIDL 3.0](/uwp/midl-3/intro) que se muestra en la lista, compila el proyecto para generar `HostViewModel.h` y `.cpp` y, a continuación, agrega el código a los archivos generados para que coincidan con la lista. Para obtener más información sobre esos archivos generados y cómo copiarlos en el proyecto, consulta [Controles de XAML; enlazar a una propiedad de C++/WinRT](../cpp-and-winrt-apis/binding-property.md).

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

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Implement the constructor like this, and add this field:
...
HostViewModel() : m_nextButtonText{ L"Next" } {}
...
private:
    std::wstring m_nextButtonText;
...

// HostViewModel.cpp
// Implement like this:
...
hstring HostViewModel::NextButtonText()
{
    return hstring{ m_nextButtonText };
}

void HostViewModel::NextButtonText(hstring const& value)
{
    m_nextButtonText = value;
}
...
```

La implementación de **HostViewModel** y su propiedad **NextButtonText** solo es apropiada para el enlace único. Sin embargo, los enlace unidireccionales y bidireccionales son muy comunes y, en dichos tipos de enlaces, la interfaz de usuario se actualiza automáticamente en respuesta a cambios en los valores de datos del origen del enlace. Para que esos tipos de enlace funcionen correctamente, debes colocar al origen de enlace como "observable" en el objeto de enlace. Así, en nuestro ejemplo, si queremos un enlace unidireccional o bidireccional con la propiedad **NextButtonText** y, a continuación, cualquier cambio que se produzca en tiempo de ejecución en el valor de esa propiedad debe hacerse observable para el objeto de enlace.

Una manera de hacerlo es derivar la clase que representa el origen de enlace de [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) y exponer un valor de datos a través de una [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty). De este modo, un [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) pasa a ser observable. **FrameworkElements** son orígenes de enlace correctos sin necesidad de personalizarlos.

Una manera más ligera de hacer que una clase sea observable, y necesaria para las clases que ya tienen una clase base, es implementar [**System.ComponentModel.INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged). Esto simplemente implica la implementación de un solo evento denominado **PropertyChanged**. A continuación te mostramos un ejemplo usando **HostViewModel**.

```csharp
...
using System.ComponentModel;
using System.Runtime.CompilerServices;
...
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
        this.PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

```cppwinrt
// HostViewModel.idl
namespace DataBindingInDepth
{
    runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        HostViewModel();
        String NextButtonText;
    }
}

// HostViewModel.h
// Add this field:
...
    winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler);
    void PropertyChanged(winrt::event_token const& token) noexcept;

private:
    winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
...

// HostViewModel.cpp
// Implement like this:
...
void HostViewModel::NextButtonText(hstring const& value)
{
    if (m_nextButtonText != value)
    {
        m_nextButtonText = value;
        m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"NextButtonText" });
    }
}

winrt::event_token HostViewModel::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
{
    return m_propertyChanged.add(handler);
}

void HostViewModel::PropertyChanged(winrt::event_token const& token) noexcept
{
    m_propertyChanged.remove(token);
}
...
```

Ahora la propiedad **NextButtonText** es observable. Cuando creas una enlace unidireccional o bidireccional con esa propiedad (te mostraremos cómo más adelante), el objeto de enlace resultante se suscribe al evento **PropertyChanged**. Cuando se genera el evento, controlador del objeto de enlace recibe un argumento que contiene el nombre de la propiedad que ha cambiado. Así es cómo el objeto de enlace sabe a qué valor de propiedad dirigirse y volver a leer.

Para que no tengas que implementar el patrón mostrado anteriormente varias veces, si usas C#, puedes derivar de la clase base **BindableBase** que encontrarás en el ejemplo de [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) (en la carpeta "Common"). Este es un ejemplo de cómo queda.

```csharp
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

```cppwinrt
// Your BindableBase base class should itself derive from Windows::UI::Xaml::DependencyObject. Then, in HostViewModel.idl, derive from BindableBase instead of implementing INotifyPropertyChanged.
```

> [!NOTE]
> Para C++/WinRT, cualquier clase en tiempo de ejecución que declares en la aplicación que derive de una clase base se conoce como una clase *que admite composición*. Las clases que admiten composición tienen restricciones. Para que una aplicación pase las pruebas del [kit de certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) que Visual Studio y Microsoft Store utilizan para validar los envíos (y para que la aplicación se incorpore correctamente a Microsoft Store), una clase que admite composición debe derivar en última instancia de una clase base de Windows. Eso significa que, en la raíz misma de la jerarquía de herencia, la clase debe ser un tipo que se origina en un espacio de nombres Windows.*. Si necesitas derivar una clase en tiempo de ejecución de una clase base, por ejemplo para implementar una clase **BindableBase** para todos los modelos de vista de los que puedes derivar, puedes derivar de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).

Generar el evento **PropertyChanged** con un argumento de [**String.Empty**](/dotnet/api/system.string.empty) o **null** indica que se deben volver a leer todas las propiedades no indexadoras del objeto. Puedes generar el evento para indica que las propiedades de indizador del objeto han cambiado mediante un argumento de "Item\[*indexer*\]" para indexadores específicos (donde *indexer* es el valor de índice) o el valor de "Item\[\]" para todos los indexadores.

Un objeto de enlace puede tratarse como un solo objeto cuyas propiedades contienen datos, o bien, como una colección de objetos. En el código de C# y Visual Basic, puedes enlazar por única vez a un objeto que implementa [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1) para mostrar una colección que no cambia en tiempo de ejecución. Para una colección observable (observar cuando se agregan o quitan los elementos de la colección), se realiza un enlace unidireccional a [**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) en su lugar. En el código de C++/CX, puedes enlazar con [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class) para las colecciones observables y no observables, y C++/WinRT tiene sus propios tipos. Para enlazar tus propias clases de colecciones, usa las directrices de la siguiente tabla.

|Escenario|C# y VB (CLR)|C++/WinRT|C++/CX|
|-|-|-|-|
|Enlazar a un objeto.|Puede ser cualquier objeto.|Puede ser cualquier objeto.|El objeto debe tener el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) o implementa [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider).|
|Obtener notificaciones de cambio de propiedades de un objeto enlazado.|El objeto debe implementar [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged).| El objeto debe implementar [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged).|El objeto debe implementar [**INotifyPropertyChanged**](/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged).|
|Enlazar a una colección.| [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1)|[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) o [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Consulta [Controles de elementos de XAML; enlazar a una colección C++/WinRT](../cpp-and-winrt-apis/binding-collection.md) y [Colecciones con C++/WinRT](../cpp-and-winrt-apis/collections.md).| [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class)|
|Obtener notificaciones de cambios de colecciones de una colección enlazada.|[**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1)|[**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Por ejemplo, [**winrt::single_threaded_observable_vector&lt;T&gt;** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector).|[**IObservableVector&lt;T&gt;** ](/uwp/api/Windows.Foundation.Collections.IObservableVector_T_).  [**Vector&lt;T&gt;** ](/cpp/cppcx/platform-collections-vector-class) implementa esta interfaz.|
|Implementar una colección compatible con enlaces.|Extiende [**List(Of T)** ](/dotnet/api/system.collections.generic.list-1) o implementa [**IList**](/dotnet/api/system.collections.ilist), [**IList**](/dotnet/api/system.collections.generic.ilist-1)(Of [**Object**](/dotnet/api/system.object)), [**IEnumerable**](/dotnet/api/system.collections.ienumerable) o [**IEnumerable**](/dotnet/api/system.collections.generic.ienumerable-1)(Of **Object**). No se admite el enlace a **IList(Of T)** e **IEnumerable(Of T)** genéricos.|Implementa [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Consulta [Controles de elementos de XAML; enlazar a una colección C++/WinRT](../cpp-and-winrt-apis/binding-collection.md) y [Colecciones con C++/WinRT](../cpp-and-winrt-apis/collections.md).|Implementa [**IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableIterable**](/uwp/api/Windows.UI.Xaml.Interop.IBindableIterable), [**IVector**](/uwp/api/Windows.Foundation.Collections.IVector_T_)&lt;[**Object**](/dotnet/api/system.object)^&gt;, [**IIterable**](/uwp/api/Windows.Foundation.Collections.IIterable_T_)&lt;**Object**^&gt;, **IVector**&lt;[**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable)\*&gt; o **IIterable**&lt;**IInspectable**\*&gt;. No se admite el enlace a **IVector&lt;T&gt;** e **IIterable&lt;T&gt;** genéricos.|
| Implementar una colección que admita notificaciones de cambios de colecciones. | Extiende [**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) o implementa [**IList**](/dotnet/api/system.collections.ilist) e [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) (no genéricos).|Implementa [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) o [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).|Implementa [**IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector) e [**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector).|
|Implementar una colección compatible con la carga incremental.|Extiende [**ObservableCollection(Of T)** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) o implementa [**IList**](/dotnet/api/system.collections.ilist) e [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged) (no genéricos). Además, implementa [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|Implementa [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) o [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector). Además, implementa [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|Implementa [**IBindableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableVector), [**IBindableObservableVector**](/uwp/api/Windows.UI.Xaml.Interop.IBindableObservableVector) e [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading).|

Con la carga incremental, puedes enlazar controles de lista a orígenes de datos que son arbitrariamente de gran tamaño y aun así lograr un alto rendimiento. Por ejemplo, puedes enlazar controles de lista a resultados de consulta de imágenes de Bing sin tener que cargarlos a todos de una vez. Solo cargas algunos resultados inmediatamente y después cargas otros, según sea necesario. Para admitir la carga incremental, debes implementar [**ISupportIncrementalLoading**](/uwp/api/Windows.UI.Xaml.Data.ISupportIncrementalLoading) en un origen de datos compatible con las notificaciones de cambios de colección. Cuando el motor de enlace de datos solicite más datos, tu origen de datos debe realizar las solicitudes apropiadas, integrar los resultados y después enviar las debidas notificaciones para actualizar la interfaz de usuario.

### <a name="binding-target"></a>Destino de enlace

En los dos ejemplos siguientes, la propiedad **Button.Content** es el destino de enlace y su valor se establece en una extensión de marcado que declara el objeto de enlace. Se muestra el primer [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) y luego [{Binding}](../xaml-platform/binding-markup-extension.md). Declarar enlaces en el marcado es el caso común (es cómodo, legible y administrable). Sin embargo, puedes evitar el marcado y de manera imperativa (mediante programación) crear una instancia de la clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) en su lugar, si necesitas.

```xaml
<Button Content="{x:Bind ...}" ... />
```

```xaml
<Button Content="{Binding ...}" ... />
```

Si usas extensiones de componentes de C++/WinRT o Visual C++ (C++/CX), deberás agregar el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) a cualquier clase en tiempo de ejecución con la que quieras usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md).

> [!IMPORTANT]
> Si estás usando [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) está disponible si has instalado la versión de Windows SDK 10.0.17763.0 (Windows 10, versión 1809) o posterior. Sin ese atributo, tendrás que implementar las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para poder usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md).

### <a name="binding-object-declared-using-xbind"></a>Objeto de enlace que se declaran usando {x:Bind}

Hay un solo paso que necesitamos ejecutar antes de crear nuestro marcado [{x:Bind}](../xaml-platform/x-bind-markup-extension.md). Tenemos que exponer nuestra clase de origen de enlace desde la clase que representa nuestra página de marcado. Eso lo haremos agregando una propiedad (de tipo **HostViewModel** en este caso) a nuestra clase de página **MainPage**.

```csharp
namespace DataBindingInDepth
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new HostViewModel();
        }
    
        public HostViewModel ViewModel { get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
import "HostViewModel.idl";

namespace DataBindingInDepth
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        HostViewModel ViewModel{ get; };
    }
}

// MainPage.h
// Include a header, and add this field:
...
#include "HostViewModel.h"
...
    DataBindingInDepth::HostViewModel ViewModel();

private:
    DataBindingInDepth::HostViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();

}

DataBindingInDepth::HostViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

Una vez que hayas hecho esto, puedes analizar más minuciosamente el marcado que declara el objeto de enlace. El siguiente ejemplo usa el mismo destino de enlace **Button.Content** que usamos en la sección "Destino de enlace" anteriormente, y muestra que está enlazado a la propiedad **HostViewModel.NextButtonText**.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="DataBindingInDepth.Mainpage" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Observa el valor que especificamos para **Path**. Este valor se interpreta en el contexto de la página en sí y, en este caso, la ruta comienza haciendo referencia a la propiedad **ViewModel** que acabamos de agregar a la página **MainPage**. Esa propiedad devuelve una instancia **HostViewModel**, así podemos usar el operador punto en ese objeto para acceder a la propiedad **HostViewModel.NextButtonText**. Además, especificamos **Mode** para invalidar el [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) predeterminado de una sola vez.

La propiedad [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) admite una variedad de opciones de sintaxis para enlazar a propiedades anidadas, propiedades adjuntas e indexadores de cadenas y de enteros. Para más información, consulta [Sintaxis de property-path](../xaml-platform/property-path-syntax.md). El enlace a indexadores de cadenas te ofrece el mismo efecto que el enlace a propiedades dinámicas sin tener que implementar [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider). Para otras opciones de configuración, consulta [Extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md).

Para ilustrar que la propiedad **HostViewModel.NextButtonText** es realmente observable, agrega un controlador de eventos **Click** al botón y actualiza el valor de **HostViewModel.NextButtonText**. Compila, ejecuta y haz clic en el botón para ver el valor de actualización de **Content** del botón.

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.ViewModel.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    ViewModel().NextButtonText(L"Updated Next button text");
}
```

> [!NOTE]
> Los cambios en [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text) se envían a un origen de enlace bidireccional cuando [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) pierde el foco y no después de cada presión de tecla del usuario.

**DataTemplate y x:DataType**

Dentro de [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate) (independientemente de si se usa como plantilla de elemento, plantilla de contenido o plantilla de encabezado), el valor de **Path** no se interpreta en el contexto de la página, sino en el contexto del objeto de datos al que se aplica la plantilla. Cuando usas {x:Bind} en la plantilla de datos para que sus enlaces pueden validarse (y un código eficaz generado para ellos) en tiempo de compilación, la clase **DataTemplate** debe declarar el tipo de su objeto de datos mediante **x:DataType**. El ejemplo siguiente puede usarse como **ItemTemplate** de un control de elementos enlazados a una colección de objetos **SampleDataGroup**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

**Objetos con establecimiento flexible de tipos en la ruta de acceso**

Consideremos, por ejemplo, que tienes un tipo denominado SampleDataGroup, que implementa una propiedad de cadena denominada Título. Y tienes una propiedad MainPage.SampleDataGroupAsObject, que es de tipo objeto, pero que devuelve realmente una instancia de SampleDataGroup. El enlace `<TextBlock Text="{x:Bind SampleDataGroupAsObject.Title}"/>` generará un error de compilación porque la propiedad Título no se encuentra en el objeto de tipo. La solución para esto es agregar una conversión a la sintaxis de la ruta de acceso, como esta: `<TextBlock Text="{x:Bind ((data:SampleDataGroup)SampleDataGroupAsObject).Title}"/>`. Este es otro ejemplo donde se declara el elemento como objeto, pero que realmente es TextBlock: `<TextBlock Text="{x:Bind Element.Text}"/>`. Y una conversión soluciona el problema: `<TextBlock Text="{x:Bind ((TextBlock)Element).Text}"/>`.

**Si los datos se cargan de forma asincrónica**

El código para admitir **{x:Bind}** se genera en tiempo de compilación en las clases parciales para tus páginas. Estos archivos pueden encontrarse en la carpeta `obj`, con nombres como (para C#) `<view name>.g.cs`. El código generado incluye un controlador para el evento [**Loading**](/uwp/api/Windows.UI.Xaml.FrameworkElement) de tu página y ese controlador llama al método **Initialize** en una clase generada que representa los enlaces de la página. **Initialize** llama a su vez a **Update** para empezar a mover datos entre el origen de enlace y el destino. **Loading** se genera justo antes del primer paso de medida de la página o del control de usuario. Por lo que si los datos se cargan de forma asincrónica puede que no esté listo en el momento en que se llama a **Initialize**. Por lo tanto, después de cargar los datos, puedes forzar la inicialización de enlaces de un solo uso llamando a `this.Bindings.Update();`. Si solo necesitas enlaces de un solo uso para los datos cargados de forma asincrónica es mucho más barato inicializarlos así que tener enlaces unidireccionales y escuchar los cambios. Si los datos no sufren cambios específicos y es probable que se actualicen como parte de una acción específica, puedes hacer que tus enlaces sean de un solo uso y forzar una actualización manual en cualquier momento con una llamada a **Update**.

> [!NOTE]
> **{x:Bind}** no es adecuado para los escenarios enlazados en tiempo de ejecución, como la navegación por la estructura de diccionario de un objeto JSON, como tampoco lo es el estilo duck typing. "Duck typing" es un formato de escritura no seguro basado en las correspondencias léxicas de los nombres de propiedad (p. ej., "si anda, nada y grazna como un pato, entonces es un pato"). Con duck typing, un enlace a la propiedad **Age** podría corresponder a un objeto **Person** o a un objeto **Wine** (asumiendo que ambos tipos tuvieran una propiedad de **edad**). Para estos escenarios, usa la extensión de marcado **{Binding}** .

### <a name="binding-object-declared-using-binding"></a>Objeto de enlace que se declara usando {Binding}

Si usas extensiones de componente de C++/WinRT o Visual C++ (C++/CX), para usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md), deberás agregar el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) a cualquier clase en tiempo de ejecución con la que quieras enlazar. Para usar [{x:Bind}](../xaml-platform/x-bind-markup-extension.md), no necesitas ese atributo.

```cppwinrt
// HostViewModel.idl
// Add this attribute:
[Windows.UI.Xaml.Data.Bindable]
runtimeclass HostViewModel : Windows.UI.Xaml.Data.INotifyPropertyChanged
...
```

> [!IMPORTANT]
> Si estás usando [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) está disponible si has instalado la versión de Windows SDK 10.0.17763.0 (Windows 10, versión 1809) o posterior. Sin ese atributo, tendrás que implementar las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para poder usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md).

[{Binding}](../xaml-platform/binding-markup-extension.md) supone que, de forma predeterminada, se está realizando un enlace a la propiedad [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) de la página de marcado. Por lo tanto, estableceremos el **DataContext** de nuestra página para que sea una instancia de la clase de origen de enlace (de tipo **HostViewModel** en este caso). El siguiente ejemplo muestra el marcado que declara el objeto de enlace. Usamos el mismo destino de enlace **Button.Content** que usamos en la sección "Destino de enlace" anterior, y enlazamos a la propiedad **HostViewModel.NextButtonText**.

```xaml
<Page xmlns:viewmodel="using:DataBindingInDepth" ... >
    <Page.DataContext>
        <viewmodel:HostViewModel x:Name="viewModelInDataContext"/>
    </Page.DataContext>
    ...
    <Button Content="{Binding Path=NextButtonText}" ... />
</Page>
```

```csharp
// MainPage.xaml.cs
private void Button_Click(object sender, RoutedEventArgs e)
{
    this.viewModelInDataContext.NextButtonText = "Updated Next button text";
}
```

```cppwinrt
// MainPage.cpp
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    viewModelInDataContext().NextButtonText(L"Updated Next button text");
}
```

Observa el valor que especificamos para **Path**. Este valor se interpreta en el contexto de [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) de la página, que en este ejemplo se establece en una instancia de **HostViewModel**. La ruta hace referencia a la propiedad **HostViewModel.NextButtonText**. Podemos omitir **Mode**, porque la [{Binding}](../xaml-platform/binding-markup-extension.md) predeterminada unidireccional funciona aquí.

El valor predeterminado de [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) para un elemento de la interfaz de usuario es el valor heredado de su elemento principal. Por supuesto, puedes anular ese valor predeterminado estableciendo **DataContext** explícitamente, que se hereda de forma predeterminada por los elementos secundarios. La configuración de **DataContext** explícitamente en un elemento es cuando quieres tener varios enlaces que usen el mismo origen.

Un objeto de enlace tiene una propiedad **Source**, que el valor predeterminado es el [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) del elemento de interfaz de usuario en el que se declara el enlace. Puedes invalidar este valor predeterminado estableciendo **Source**, **RelativeSource**, o **ElementName** explícitamente en el enlace (consulte [{Binding}](../xaml-platform/binding-markup-extension.md) para obtener más información).

Dentro de [**DataTemplate**](/uwp/api/Windows.UI.Xaml.DataTemplate), [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) se establece automáticamente en el objeto de datos con plantilla. El ejemplo siguiente puede usarse como la propiedad **ItemTemplate** de un control de elementos enlazado a una colección de cualquier tipo que tiene propiedades de cadena denominadas **Title** y **Description**.

```xaml
<DataTemplate x:Key="SimpleItemTemplate">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{Binding Title}"/>
      <TextBlock Text="{Binding Description"/>
    </StackPanel>
  </DataTemplate>
```

> [!NOTE]
> De forma predeterminada, los cambios en [**TextBox.Text**](/uwp/api/windows.ui.xaml.controls.textbox.text) se envían a un origen de enlace bidireccional cuando la clase [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) pierde el foco. Para hacer que los cambios se envíen después de cada presión de tecla de usuario, establece **UpdateSourceTrigger** en **PropertyChanged** en el enlace en el marcado. También puede tomar el control completo de cuándo se envían los datos al origen estableciendo **UpdateSourceTrigger** en **Explicit**. Luego puedes controlar eventos en el cuadro de texto (normalmente [**TextBox.TextChanged**](/uwp/api/Windows.UI.Xaml.Controls.TextBox)), llamar a [**GetBindingExpression**](/uwp/api/windows.ui.xaml.frameworkelement.getbindingexpression) en el destino para obtener un [**BindingExpression**](/uwp/api/windows.ui.xaml.data.bindingexpression) y finalmente llamar a [**BindingExpression.UpdateSource**](/uwp/api/windows.ui.xaml.data.bindingexpression.updatesource) para actualizar mediante programación el origen de datos.

La propiedad [**Path**](/uwp/api/windows.ui.xaml.data.binding.path) admite una variedad de opciones de sintaxis para enlazar a propiedades anidadas, propiedades adjuntas e indexadores de cadenas y de enteros. Para más información, consulta [Sintaxis de property-path](../xaml-platform/property-path-syntax.md). El enlace a indexadores de cadenas te ofrece el mismo efecto que el enlace a propiedades dinámicas sin tener que implementar [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider). La propiedad [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) es útil para el enlace de elemento a elemento. La propiedad [**RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) tiene varios usos, uno de los cuales es como una alternativa más eficaz para el enlace de plantilla dentro de un [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate). Para otras opciones de configuración, consulta [extensión de marcado {Binding}](../xaml-platform/binding-markup-extension.md) y la clase [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding).

## <a name="what-if-the-source-and-the-target-are-not-the-same-type"></a>¿Qué ocurre si el origen y el destino no son del mismo tipo?

Si desea controlar la visibilidad de un elemento de interfaz de usuario en función del valor de una propiedad booleana o si quieres representar un elemento de interfaz de usuario con un color que es una función de un intervalo o tendencia de un valor numérico o si deseas mostrar un valor de fecha y/u hora en una propiedad del elemento de interfaz de usuario que espera una cadena , necesitarás convertir los valores de un tipo a otro. Habrá casos en los que la solución correcta es exponer otra propiedad del tipo correcto desde la clase de origen de enlace y mantener la lógica de conversión encapsulada y comprobable allí. Sin embargo, no es escalable ni flexible cuando tienes muchos números o combinaciones grandes de propiedades de origen y destino. En ese caso, tienes un par de opciones:

* Si usas {x: Bind}, entonces puedes enlazar directamente a una función para hacer esa conversión.
* O bien, puedes especificar un convertidor de valores, que es un objeto que se ha diseñado para realizar la conversión. 

## <a name="value-converters"></a>Convertidores de valores

Este es un convertidor de valores adecuado para un enlace único o unidireccional, que convierte un valor [**DateTime**](/dotnet/api/system.datetime) en un valor de cadena que contiene el mes. La clase implementa [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

```csharp
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

```cppwinrt
// See the "Formatting or converting data values for display" section in the "Data binding overview" topic.
```

Y esta es la forma de consumir ese convertidor de valores en el marcado del objeto de enlace.

```xaml
<UserControl.Resources>
  <local:DateToStringConverter x:Key="Converter1"/>
</UserControl.Resources>
...
<TextBlock Grid.Column="0" 
  Text="{x:Bind ViewModel.Month, Converter={StaticResource Converter1}}"/>
<TextBlock Grid.Column="0" 
  Text="{Binding Month, Converter={StaticResource Converter1}}"/>
```

El motor de enlace llama a los métodos [**Convert**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convert) y [**ConvertBack**](/uwp/api/windows.ui.xaml.data.ivalueconverter.convertback) si se ha definido el parámetro [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) para el enlace. Cuando se pasan los datos del origen, el motor de enlace llama a **Convert** y pasa los datos devueltos al destino. Cuando se pasan los datos del destino (para un enlace bidireccional), el motor de enlace llama a **ConvertBack** y pasa los datos devueltos al origen.

El convertidor también tiene parámetros opcionales: [**ConverterLanguage**](/uwp/api/windows.ui.xaml.data.binding.converterlanguage), que permite especificar el lenguaje que se usará en la conversión, y [**ConverterParameter**](/uwp/api/windows.ui.xaml.data.binding.converterparameter), que permite pasar un parámetro para la lógica de conversión. Para obtener un ejemplo en el que se use un parámetro de convertidor, consulta [**IValueConverter**](/uwp/api/Windows.UI.Xaml.Data.IValueConverter).

> [!NOTE]
> Si hay un error en la conversión, no inicies una excepción. En lugar de eso, devuelve [**DependencyProperty.UnsetValue**](/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue), que detendrá la transferencia de datos.

Para mostrar un valor predeterminado para usar cuando no se pueda resolver el origen del enlace, establece la propiedad **FallbackValue** en el objeto de enlace en el marcado. Esto sirve para controlar los errores de formato y de conversión. También resulta útil para crear un enlace con propiedades de origen que quizás no existen en todos los objetos de una colección enlazada de tipos heterogéneos.

Si enlazas un control de texto con un valor que no es una cadena, el motor de enlace de datos lo convertirá en una. Si el valor es un tipo de referencia, dicho motor recuperará el valor de cadena llamando a [**ICustomPropertyProvider.GetStringRepresentation**](/uwp/api/windows.ui.xaml.data.icustompropertyprovider.getstringrepresentation) o a [**IStringable.ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring), si están disponibles, o bien llamando a [**Object.ToString**](/dotnet/api/system.object.tostring#System_Object_ToString), si no lo están. Pero ten en cuenta que el motor de enlace de datos pasará por alto las implementaciones de **ToString** que oculten la implementación de la clase base. Las implementaciones de la subclase deben invalidar, en cambio, el método **ToString** de la clase base. De forma similar, en los lenguajes nativos, todos los objetos administrados parecen implementar [**ICustomPropertyProvider**](/uwp/api/Windows.UI.Xaml.Data.ICustomPropertyProvider) e [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable). Pero todas las llamadas a **GetStringRepresentation** y a **IStringable.ToString** se enrutan a **Object.ToString** o a una invalidación de ese método y nunca a una implementación de **ToString** que oculta la implementación de la clase base.

> [!NOTE]
> A partir de Windows 10, versión 1607, el marco XAML proporciona un valor booleano integrado para el convertidor de visibilidad. El convertidor asigna **true** al valor de la enumeración **Visible** y **false** a **Collapsed**, para que puedas enlazar una propiedad Visibility a un valor booleano sin necesidad de crear un convertidor. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Version adaptive code (Código adaptativo para versiones)](../debug-test-perf/version-adaptive-code.md).

## <a name="function-binding-in-xbind"></a>Enlace de función en {x: Bind}

{x: Bind} permite que el paso final de una ruta de acceso de enlace sea una función. Esto puede usarse para realizar conversiones y para realizar enlaces que dependen de más de una propiedad. Consulta [**Funciones de x:Bind**](function-bindings.md)

<span id="resource-dictionaries-with-x-bind"/>

## <a name="element-to-element-binding"></a>Enlace de elemento a elemento

Se puede enlazar la propiedad de un elemento XAML a la propiedad de otro elemento XAML. En este ejemplo se muestra cómo queda en marcado.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

> [!IMPORTANT]
> Para obtener el flujo de trabajo necesario para el enlace de elemento a elemento mediante C++/WinRT, consulta [Enlace de elemento a elemento](../cpp-and-winrt-apis/binding-property.md#element-to-element-binding).

## <a name="resource-dictionaries-with-xbind"></a>Diccionarios de recursos con {x:Bind}

La [extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md) depende de la generación de códigos; por lo tanto, necesita un archivo de código subyacente que contenga un constructor que llame a **InitializeComponent** (para inicializar el código generado). Vuelves a usar el diccionario de recursos creando una instancia de su tipo (por lo que se llama a **InitializeComponent**) en lugar de hacer referencia a su nombre de archivo. Aquí un ejemplo de qué hacer si dispones de un diccionario de recursos existente y quieres usar {xBind} en este.

TemplatesResourceDictionary.xaml

```xaml
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

```csharp
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

```xaml
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

## <a name="event-binding-and-icommand"></a>Enlace de eventos e ICommand

[{x:Bind}](../xaml-platform/x-bind-markup-extension.md) admite una característica llamada enlace de eventos. Con esta característica, puede especificar el controlador para un evento con un enlace, que es una opción adicional sobre el control de eventos con un método en el archivo de código subyacente. Supongamos que tienes una propiedad **RootFrame** en tu clase **MainPage**.

```csharp
public sealed partial class MainPage : Page
{
    ...
    public Frame RootFrame { get { return Window.Current.Content as Frame; } }
}
```

Se puede enlazar el evento **Click** de un botón a un método en el objeto **Frame** devuelto por la propiedad **RootFrame como esta**. Ten en cuenta que también enlazamos la propiedad **IsEnabled** del botón a otro miembro del mismo **Frame**.

```xaml
<AppBarButton Icon="Forward" IsCompact="True"
IsEnabled="{x:Bind RootFrame.CanGoForward, Mode=OneWay}"
Click="{x:Bind RootFrame.GoForward}"/>
```

Los métodos sobrecargados no puede usarse para controlar un evento con esta técnica. Además, si el método que controla el evento tiene parámetros, todos deben ser asignables a partir de los tipos de todos los parámetros del evento, respectivamente. En este caso, [**Frame.GoForward**](/uwp/api/windows.ui.xaml.controls.frame.goforward) no esté sobrecargado y no tiene parámetros (pero podría ser válido incluso si usó dos parámetros **object**). [**Frame.GoBack**](/uwp/api/windows.ui.xaml.controls.frame.goback) está sobrecargado; por lo tanto, no podemos usar ese método con esta técnica.

La técnica de enlace de eventos es similar a implementar y consumir comandos (un comando es una propiedad que devuelve un objeto que implementa la interfaz [**ICommand**](/uwp/api/windows.ui.xaml.input.icommand)). Ambos [{x: enlace}](../xaml-platform/x-bind-markup-extension.md) y [{Binding}](../xaml-platform/binding-markup-extension.md) funcionan con comandos. Debido a que no tienes que implementar el patrón de comando varias veces, puedes usar la clase **DelegateCommand** auxiliar que encontrarás en la muestra [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper) (en la carpeta "Común").

## <a name="binding-to-a-collection-of-folders-or-files"></a>Enlace a una colección de carpetas o archivos

Para recuperar datos de archivos y carpetas, puedes usar las API en el espacio de nombres [**Windows.Storage**](/uwp/api/Windows.Storage). No obstante, los distintos métodos **GetFilesAsync**, **GetFoldersAsync** y **GetItemsAsync** no devuelven valores adecuados para enlaces a controles de lista. En cambio, debes enlazar a los valores devueltos de los métodos [**GetVirtualizedFilesVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfilesvector), [**GetVirtualizedFoldersVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizedfoldersvector) y [**GetVirtualizedItemsVector**](/uwp/api/windows.storage.bulkaccess.fileinformationfactory.getvirtualizeditemsvector) de la clase [**FileInformationFactory**](/uwp/api/Windows.Storage.BulkAccess.FileInformationFactory). En el siguiente ejemplo de código de la [muestra de StorageDataSource y GetVirtualizedFilesVector](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208.1%20Store%20app%20samples/99866-Windows%208.1%20Store%20app%20samples/StorageDataSource%20and%20GetVirtualizedFilesVector%20sample) encontrarás el patrón de uso más común. No olvides que declarar el manifiesto **picturesLibrary** del paquete de funcionalidad de la aplicación y confirmar que hay imágenes en la carpeta de la biblioteca de imágenes.

```csharp
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

Normalmente usarás este enfoque para crear una vista de solo lectura de la información de archivo y carpeta. Puedes crear enlaces bidireccionales a las propiedades de archivo y carpeta; por ejemplo, para permitir que los usuarios califiquen una canción en una vista de música. No obstante, ningún cambio persiste hasta que no llames al método **SavePropertiesAsync** apropiado (por ejemplo, [**MusicProperties.SavePropertiesAsync**](/uwp/api/windows.storage.fileproperties.musicproperties.savepropertiesasync)). Debes confirmar cambios cuando el elemento pierde el foco, porque esto desencadena el restablecimiento de la selección.

Ten en cuenta que el enlace bidireccional con esta técnica solo funciona con ubicaciones indizadas, como Música. Puedes determinar si una ubicación es indizada llamando al método [**FolderInformation.GetIndexedStateAsync**](/uwp/api/windows.storage.bulkaccess.folderinformation.getindexedstateasync).

Ten en cuenta también que un vector virtualizado puede devolver **null** para algunos elementos antes de que rellene su valor. Por ejemplo, debes comprobar **null** antes de usar el valor [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) de un control de lista enlazado a un vector virtualizado o, en su lugar, usar [**SelectedIndex**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectedindex).

## <a name="binding-to-data-grouped-by-a-key"></a>Enlace de datos agrupados por una clave

Si tomas una colección plana de elementos (libros, por ejemplo, representados por una clase **BookSku**) y agrupas los elementos mediante una propiedad común, como una clave (la propiedad **BookSku.AuthorName**, por ejemplo), el resultado son datos agrupados. Al agrupar los datos, ya no es una colección plana. Los datos agrupados son una colección de objetos de grupo, donde cada objeto de grupo tiene

- una clave y
- una colección de elementos cuya propiedad coincide con esa clave.

Si retomamos el ejemplo de los libros, la agrupación de los libros por nombre de autor da como resultado una colección de grupos de nombres de autor donde cada grupo tiene

- una clave, que es un nombre de autor, y
- una colección de **BookSku**s cuya propiedad **AuthorName** coincide con la clave del grupo.

En general, para mostrar una colección, enlazas el [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) de un control de elementos (como [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) o [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView)) directamente a una propiedad que devuelve una colección. Si es una colección de elementos plana no necesitas hacer nada especial. Pero si es una colección de objetos de grupo (como al enlazar a datos agrupados), a continuación, necesitas los servicios de un objeto intermediario llamado [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), que se encuentra entre el control de elementos y el origen del enlace. Enlaza el **CollectionViewSource** a la propiedad que devuelve datos agrupados y enlaza el control de elementos a **CollectionViewSource**. Un valor agregado adicional de un **CollectionViewSource** es que realiza el seguimiento del elemento actual, para poder mantener más de un control de elementos sincronizados al enlazarlos todos al mismo **CollectionViewSource**. Puedes acceder también al elemento actual mediante programación a través de la propiedad [**ICollectionView.CurrentItem**](/uwp/api/windows.ui.xaml.data.icollectionview.currentitem) del objeto devuelto por la propiedad [**CollectionViewSource.View**](/uwp/api/windows.ui.xaml.data.collectionviewsource.view).

Para activar la función de agrupación de un [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), establece [**IsSourceGrouped**](/uwp/api/windows.ui.xaml.data.collectionviewsource.issourcegrouped) en **true**. Si también debes establecer la propiedad [**ItemsPath**](/uwp/api/windows.ui.xaml.data.collectionviewsource.itemspath) depende de cómo crees exactamente los objetos de grupo. Existen dos formas para crear un objeto de grupo: el patrón de "es un grupo" y el "tiene un grupo". En el modelo "es un grupo", el objeto de grupo se deriva de un tipo de colección (por ejemplo, **List&lt;T&gt;** ), de modo que el objeto de grupo en realidad es el grupo de elementos. Con este modelo, es necesario establecer **ItemsPath**. En el modelo "tiene un grupo", el objeto de grupo tiene una o más propiedades de un tipo de colección (como **List&lt;T&gt;** ), por lo que el grupo "tiene un" grupo de elementos en forma de una propiedad (o varios grupos de elementos en forma de varias propiedades). Con este modelo, debes establecer **ItemsPath** en el nombre de la propiedad que contiene el grupo de elementos.

El siguiente ejemplo muestra el modelo de "tiene un grupo". La clase de página tiene una propiedad denominada [**ViewModel**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext), que devuelve una instancia de nuestro modelo de vista. El [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) enlaza a la propiedad **Authors** del modelo de vista (**Authors** es la colección de objetos de grupo) y también especifica que es la propiedad **Author.BookSkus** que contiene los elementos agrupados. Por último, la [**GridView**](/uwp/api/Windows.UI.Xaml.Controls.GridView) está enlazada al **CollectionViewSource**, y su estilo de grupo se define para que pueda representar los elementos en grupos.

```xaml
<Page.Resources>
    <CollectionViewSource
    x:Name="AuthorHasACollectionOfBookSku"
    Source="{x:Bind ViewModel.Authors}"
    IsSourceGrouped="true"
    ItemsPath="BookSkus"/>
</Page.Resources>
...
<GridView
ItemsSource="{x:Bind AuthorHasACollectionOfBookSku}" ...>
    <GridView.GroupStyle>
        <GroupStyle
            HeaderTemplate="{StaticResource AuthorGroupHeaderTemplateWide}" ... />
    </GridView.GroupStyle>
</GridView>
```

Para implementar el patrón de "es un grupo" en uno de dos maneras. Es una forma crear tu propia clase de grupo. Deriva la clase de **List&lt;T&gt;** (donde *T* es el tipo de los elementos). Por ejemplo, `public class Author : List<BookSku>`. La segunda manera es usar una expresión [LINK](/previous-versions/bb397926(v=vs.140)) para crear dinámicamente objetos de grupo (y una clase de grupo) desde valores de propiedad similares de los elementos **BookSku**. Este enfoque, mantener solo una lista de elementos y agrupar sobre la marcha, es típico de una aplicación que accede a los datos de un servicio de nube. Obtienes la flexibilidad de agrupar libros según el autor o el género (por ejemplo), sin necesidad de disponer de clases de grupo especiales, como **Author** y **Genre**.

El siguiente ejemplo muestra el uso del patrón "es un grupo" [LINQ](/previous-versions/bb397926(v=vs.140)). Esta vez agrupamos libros por género, mostrados con el nombre de género en los encabezados de grupo. Esto se indica en la ruta de acceso de la propiedad "Key" en referencia al valor [**Key**](/dotnet/api/system.linq.igrouping-2.key#System_Linq_IGrouping_2_Key) del grupo.

```csharp
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
                          orderby grp.Key
                          select grp;
        }
        return this.genres;
    }
}
```

Recuerda que, al usar [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) con plantillas de datos que necesitamos para indicar el tipo enlazado a estableciendo un valor **x:DataType**. Si el tipo es genérico y no podemos declaramos que en el marcado que necesitamos usar [{Binding}](../xaml-platform/binding-markup-extension.md) en su lugar en la plantilla de encabezado de estilo de grupo.

```xaml
    <Grid.Resources>
        <CollectionViewSource x:Name="GenreIsACollectionOfBookSku"
        Source="{x:Bind Genres}"
        IsSourceGrouped="true"/>
    </Grid.Resources>
    <GridView ItemsSource="{x:Bind GenreIsACollectionOfBookSku}">
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

Un control [**SemanticZoom**](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) es una manera ideal para que los usuarios vean y naveguen por los datos agrupados. La aplicación de muestra [Bookstore2](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10) ilustra cómo usar **SemanticZoom**. En esa aplicación, puedes ver una lista de libros agrupados por autor (la vista ampliada) o alejar para ver una lista de accesos directos de autores (la vista alejada). La lista de accesos directos ofrece una navegación mucho más rápida que un desplazamiento por la lista de libros. Las vistas acercada y alejada en realidad son controles **ListView** o **GridView** enlazados a la misma clase **CollectionViewSource**.

![Ilustración de SemanticZoom](images/sezo.png)

Cuando se enlaza a datos jerárquicos (como subcategorías dentro de las categorías), puedes mostrar los niveles jerárquicos en la interfaz de usuario con una serie de controles de elementos. Una selección en un control de elementos determina el contenido de los controles de elementos posteriores. Puedes mantener las listas sincronizadas, al enlazar cada una de ellas a su propio [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) y al enlazar las instancias de **CollectionViewSource** juntas en una cadena. Esto se denomina una vista maestro/detalles (o la lista y detalles). Para más información, consulta el tema [Cómo enlazar a datos jerárquicos y crear una vista maestro y detalles](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md).

<span id="debugging"/>

## <a name="diagnosing-and-debugging-data-binding-problems"></a>Diagnóstico y depuración de problemas de enlace de datos

El marcado de enlace contiene los nombres de propiedades (y para C#, a veces campos y métodos). Por lo tanto, cuando cambias el nombre de una propiedad, también tendrás que cambiar cualquier enlace que haga referencia a él. Olvidar hacer que conduce a un ejemplo típico de un error de enlace de datos y la aplicación no compile o no ejecutarse correctamente.

Los objetos de enlace creados por [{x: enlace}](../xaml-platform/x-bind-markup-extension.md) y [{Binding}](../xaml-platform/binding-markup-extension.md) son prácticamente funcionalmente equivalentes. Pero {x:Bind} tiene información de tipo de origen de enlace, y genera código fuente en tiempo de compilación. Con {x: enlace} obtener el mismo tipo de detección de problemas que se obtiene con el resto del código. Que incluye la validación de tiempo de compilación de las expresiones de enlace y depuración estableciendo puntos de interrupción en el código fuente que se generan como la clase parcial de la página. Estas clases pueden encontrarse en los archivos en tu `obj` carpetas con nombres como (para C#) `<view name>.g.cs`). Si tiene un problema con un enlace y activar **interrumpir en las excepciones no controladas** en el depurador de Microsoft Visual Studio. El depurador interrumpirá la ejecución en ese punto y, a continuación, se pueden depurar lo que ha funcionado. El código generado por {x:Bind} sigue el mismo patrón para cada parte del gráfico de nodos de origen de enlace, y puedes usar la información de la ventana **Pila de llamadas** para ayudar a determinar la secuencia de llamadas que condujeron al problema.

[{Binding}](../xaml-platform/binding-markup-extension.md) no tiene información de tipo de origen de enlace. Cuando ejecutas la aplicación con el depurador adjunto, los errores de enlace aparecen en la ventana **Resultado** en Visual Studio.

## <a name="creating-bindings-in-code"></a>Crear enlaces en el código

**Nota**  Esta sección solo se aplica a [{Binding}](../xaml-platform/binding-markup-extension.md), porque no puedes crear enlaces [{x:Bind}](../xaml-platform/x-bind-markup-extension.md) en el código. Sin embargo, algunas de las ventajas de {x:Bind} pueden conseguirse con [**DependencyObject.RegisterPropertyChangedCallback**](/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback), que te permite registrar notificaciones de cambio en cualquier propiedad de dependencia.

También puedes conectar elementos de la interfaz de usuario a datos usando código de procedimientos en lugar de XAML. Para ello, crea un nuevo objeto [**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding), establece las propiedades correspondientes y luego llama a [**FrameworkElement.SetBinding**](/uwp/api/windows.ui.xaml.frameworkelement.setbinding) o [**BindingOperations.SetBinding**](/uwp/api/windows.ui.xaml.data.bindingoperations.setbinding). Crear enlaces mediante programación te resultará útil si quieres elegir los valores de propiedad de los enlaces en tiempo de ejecución o compartir un único enlace entre varios controles. Pero ten en cuenta que no puedes cambiar los valores de propiedad de los enlaces después de llamar a **SetBinding**.

En el siguiente ejemplo se muestra cómo implementar un enlace en el código.

```xaml
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

```vbnet
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

## <a name="xbind-and-binding-feature-comparison"></a>{x: enlace} y {Binding} comparación de características

| Característica | {x:Bind} | {Binding} | Notas |
|---------|----------|-----------|-------|
| La ruta de acceso es la propiedad predeterminada | `{x:Bind a.b.c}` | `{Binding a.b.c}` | | 
| Propiedades de acceso | `{x:Bind Path=a.b.c}` | `{Binding Path=a.b.c}` | En Bind: x, ruta de acceso raíz está en la página de forma predeterminada, no DataContext. | 
| indizador | `{x:Bind Groups[2].Title}` | `{Binding Groups[2].Title}` | Enlaza con el elemento especificado en la colección. Se admiten solamente en números enteros índices. | 
| Propiedades adjuntas | `{x:Bind Button22.(Grid.Row)}` | `{Binding Button22.(Grid.Row)}` | Las propiedades adjuntas se especifican mediante paréntesis. Si la propiedad no está declarada en un espacio de nombres XAML, agrega un prefijo con un espacio de nombres XML, que debe estar asignado a un espacio de nombres de código al principio del documento. | 
| Conversión | `{x:Bind groups[0].(data:SampleDataGroup.Title)}` | No necesaria. | Las conversiones de tipos se especifican mediante paréntesis. Si la propiedad no está declarada en un espacio de nombres XAML, agrega un prefijo con un espacio de nombres XML, que debe estar asignado a un espacio de nombres de código al principio del documento. | 
| Converter | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}}` | Convertidores deben declararse en la raíz del página o ResourceDictionary o en App.xaml. | 
| ConvertidorParameter, ConvertidorLanguage | `{x:Bind IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | `{Binding IsShown, Converter={StaticResource BoolToVisibility}, ConverterParameter=One, ConverterLanguage=fr-fr}` | Convertidores deben declararse en la raíz del página o ResourceDictionary o en App.xaml. | 
| TargetNullValue | `{x:Bind Name, TargetNullValue=0}` | `{Binding Name, TargetNullValue=0}` | Se usa cuando la hoja de la expresión de enlace es null. Usar comillas simples para un valor de cadena. | 
| FallbackValue | `{x:Bind Name, FallbackValue='empty'}` | `{Binding Name, FallbackValue='empty'}` | Se usa cuando cualquier parte de la ruta de acceso para el enlace (excepto la hoja) es null. | 
| ElementName | `{x:Bind slider1.Value}` | `{Binding Value, ElementName=slider1}` | Con {x: enlace} que enlace a un campo; Ruta de acceso raíz está en la página de forma predeterminada, por lo que cualquier elemento con nombre se puede acceder mediante su campo. | 
| RelativeSource: Self (Automático) | `<Rectangle x:Name="rect1" Width="200" Height="{x:Bind rect1.Width}" ... />` | `<Rectangle Width="200" Height="{Binding Width, RelativeSource={RelativeSource Self}}" ... />` | Con {x: Bind}, nombre del elemento y usar su nombre en la ruta de acceso. | 
| RelativeSource: TemplatedParent | No necesaria | `{Binding <path>, RelativeSource={RelativeSource TemplatedParent}}` | Con {x:Bind}, TargetType en ControlTemplate indica un enlace con el elemento primario de la plantilla. Para {Binding}, se puede usar el enlace de plantilla normal para plantillas de control en mayoría de casos. Pero usa TemplatedParent donde necesites usar un convertidor o un enlace bidireccional.< | 
| Origen | No necesaria | `<ListView ItemsSource="{Binding Orders, Source={StaticResource MyData}}"/>` | Para {x:Bind}, puedes usar directamente el elemento con nombre y una propiedad o ruta de acceso estática. | 
| Modo | `{x:Bind Name, Mode=OneWay}` | `{Binding Name, Mode=TwoWay}` | El modo puede ser OneTime, OneWay o TwoWay. {x: enlace} el valor predeterminado es OneTime; {Binding} predeterminado OneWay. | 
| UpdateSourceTrigger | `{x:Bind Name, Mode=TwoWay, UpdateSourceTrigger=PropertyChanged}` | `{Binding UpdateSourceTrigger=PropertyChanged}` | UpdateSourceTrigger puede ser Default, LostFocus o PropertyChanged. {x:Bind} no admite UpdateSourceTrigger=Explicit. {x:Bind} usa el comportamiento PropertyChanged para todos los casos, excepto TextBox.Text, donde utiliza el comportamiento LostFocus. |