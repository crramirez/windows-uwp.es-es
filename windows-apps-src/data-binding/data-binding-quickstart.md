---
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: Introducción al enlace de datos
description: En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP).
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cppcx
ms.openlocfilehash: 3f1aeb24a50d0e1f4d1e7c1ee702ea078582b4e5
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2020
ms.locfileid: "89412009"
---
# <a name="data-binding-overview"></a>Introducción al enlace de datos

En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o cómo enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, te mostramos cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. Para obtener información más detallada, consulta [Enlace de datos a profundidad](data-binding-in-depth.md).

## <a name="prerequisites"></a>Requisitos previos

En este tema suponemos que sabes cómo crear una aplicación básica para UWP. Para obtener instrucciones sobre cómo crear tu primera aplicación para UWP, consulta [Introducción a las aplicaciones de Windows](../get-started/index.md).

## <a name="create-the-project"></a>Crear el proyecto

Crea un nuevo proyecto de **Aplicación vacía (Windows Universal)** . Llámale "Quickstart".

## <a name="binding-to-a-single-item"></a>Enlace a un solo elemento

Cada enlace consta de un destino de enlace y de un origen de enlace. Normalmente, el destino es una propiedad de un control u otro elemento de interfaz de usuario y el origen es una propiedad de una instancia de clase (un modelo de datos o un modelo de vista). Este ejemplo muestra cómo enlazar un control a un solo elemento. El destino es la propiedad **Text** de un **TextBlock**. El origen es una instancia de una clase simple denominada **Recording** que representa una grabación de audio. Veamos primero la clase.

Si usas C# o C++/CX, agrega una nueva clase al proyecto y asigna a la clase el nombre **Recording**.

Si usas [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), agrega nuevos elementos de **archivo MIDL (.idl)** al proyecto, con el nombre que se muestra en la lista del ejemplo de código C++/WinRT siguiente. Reemplaza el contenido de los archivos nuevos por el código [MIDL 3.0](/uwp/midl-3/intro) que se muestra en la lista, compila el proyecto para generar `Recording.h` y `.cpp` y `RecordingViewModel.h` y `.cpp` y, a continuación, agrega el código a los archivos generados para que coincidan con la lista. Para obtener más información sobre esos archivos generados y cómo copiarlos en el proyecto, consulta [Controles de XAML; enlazar a una propiedad de C++/WinRT](../cpp-and-winrt-apis/binding-property.md).

```csharp
namespace Quickstart
{
    public class Recording
    {
        public string ArtistName { get; set; }
        public string CompositionName { get; set; }
        public DateTime ReleaseDateTime { get; set; }
        public Recording()
        {
            this.ArtistName = "Wolfgang Amadeus Mozart";
            this.CompositionName = "Andante in C for Piano";
            this.ReleaseDateTime = new DateTime(1761, 1, 1);
        }
        public string OneLineSummary
        {
            get
            {
                return $"{this.CompositionName} by {this.ArtistName}, released: "
                    + this.ReleaseDateTime.ToString("d");
            }
        }
    }
    public class RecordingViewModel
    {
        private Recording defaultRecording = new Recording();
        public Recording DefaultRecording { get { return this.defaultRecording; } }
    }
}
```

```cppwinrt
// Recording.idl
namespace Quickstart
{
    runtimeclass Recording
    {
        Recording(String artistName, String compositionName, Windows.Globalization.Calendar releaseDateTime);
        String ArtistName{ get; };
        String CompositionName{ get; };
        Windows.Globalization.Calendar ReleaseDateTime{ get; };
        String OneLineSummary{ get; };
    }
}

// RecordingViewModel.idl
import "Recording.idl";

namespace Quickstart
{
    runtimeclass RecordingViewModel
    {
        RecordingViewModel();
        Quickstart.Recording DefaultRecording{ get; };
    }
}

// Recording.h
// Add these fields:
...
#include <sstream>
...
private:
    std::wstring m_artistName;
    std::wstring m_compositionName;
    Windows::Globalization::Calendar m_releaseDateTime;
...

// Recording.cpp
// Implement like this:
...
Recording::Recording(hstring const& artistName, hstring const& compositionName, Windows::Globalization::Calendar const& releaseDateTime) :
    m_artistName{ artistName.c_str() },
    m_compositionName{ compositionName.c_str() },
    m_releaseDateTime{ releaseDateTime } {}

hstring Recording::ArtistName(){ return hstring{ m_artistName }; }
hstring Recording::CompositionName(){ return hstring{ m_compositionName }; }
Windows::Globalization::Calendar Recording::ReleaseDateTime(){ return m_releaseDateTime; }

hstring Recording::OneLineSummary()
{
    std::wstringstream wstringstream;
    wstringstream << m_compositionName.c_str();
    wstringstream << L" by " << m_artistName.c_str();
    wstringstream << L", released: " << m_releaseDateTime.MonthAsNumericString().c_str();
    wstringstream << L"/" << m_releaseDateTime.DayAsString().c_str();
    wstringstream << L"/" << m_releaseDateTime.YearAsString().c_str();
    return hstring{ wstringstream.str().c_str() };
}
...

// RecordingViewModel.h
// Add this field:
...
#include "Recording.h"
...
private:
    Quickstart::Recording m_defaultRecording{ nullptr };
...

// RecordingViewModel.cpp
// Implement like this:
...
Quickstart::Recording RecordingViewModel::DefaultRecording()
{
    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Year(1761);
    releaseDateTime.Month(1);
    releaseDateTime.Day(1);
    m_defaultRecording = winrt::make<Recording>(L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime);
    return m_defaultRecording;
}
...
```

```cppcx
// Recording.h
#include <sstream>
namespace Quickstart
{
    public ref class Recording sealed
    {
    private:
        Platform::String^ artistName;
        Platform::String^ compositionName;
        Windows::Globalization::Calendar^ releaseDateTime;
    public:
        Recording(Platform::String^ artistName, Platform::String^ compositionName,
            Windows::Globalization::Calendar^ releaseDateTime) :
            artistName{ artistName },
            compositionName{ compositionName },
            releaseDateTime{ releaseDateTime } {}
        property Platform::String^ ArtistName
        {
            Platform::String^ get() { return this->artistName; }
        }
        property Platform::String^ CompositionName
        {
            Platform::String^ get() { return this->compositionName; }
        }
        property Windows::Globalization::Calendar^ ReleaseDateTime
        {
            Windows::Globalization::Calendar^ get() { return this->releaseDateTime; }
        }
        property Platform::String^ OneLineSummary
        {
            Platform::String^ get()
            {
                std::wstringstream wstringstream;
                wstringstream << this->CompositionName->Data();
                wstringstream << L" by " << this->ArtistName->Data();
                wstringstream << L", released: " << this->ReleaseDateTime->MonthAsNumericString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->DayAsString()->Data();
                wstringstream << L"/" << this->ReleaseDateTime->YearAsString()->Data();
                return ref new Platform::String(wstringstream.str().c_str());
            }
        }
    };
    public ref class RecordingViewModel sealed
    {
    private:
        Recording ^ defaultRecording;
    public:
        RecordingViewModel()
        {
            Windows::Globalization::Calendar^ releaseDateTime = ref new Windows::Globalization::Calendar();
            releaseDateTime->Year = 1761;
            releaseDateTime->Month = 1;
            releaseDateTime->Day = 1;
            this->defaultRecording = ref new Recording{ L"Wolfgang Amadeus Mozart", L"Andante in C for Piano", releaseDateTime };
        }
        property Recording^ DefaultRecording
        {
            Recording^ get() { return this->defaultRecording; };
        }
    };
}

// Recording.cpp
#include "pch.h"
#include "Recording.h"
```

Después, expón la clase de origen de enlace desde la clase que representa la página de marcado. Eso lo haremos agregando una propiedad de tipo **RecordingViewModel** a **MainPage**.

Si usas [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), en primer lugar actualiza `MainPage.idl`. Compila el proyecto para que vuelva a generar `MainPage.h` y `.cpp`, y combina los cambios de los archivos generados en los del proyecto.

```csharp
namespace Quickstart
{
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new RecordingViewModel();
        }
        public RecordingViewModel ViewModel{ get; set; }
    }
}
```

```cppwinrt
// MainPage.idl
// Add this property:
import "RecordingViewModel.idl";
...
RecordingViewModel ViewModel{ get; };
...

// MainPage.h
// Add this property and this field:
...
#include "RecordingViewModel.h"
...
    Quickstart::RecordingViewModel ViewModel();

private:
    Quickstart::RecordingViewModel m_viewModel{ nullptr };
...

// MainPage.cpp
// Implement like this:
...
MainPage::MainPage()
{
    InitializeComponent();
    m_viewModel = winrt::make<RecordingViewModel>();
}
Quickstart::RecordingViewModel MainPage::ViewModel()
{
    return m_viewModel;
}
...
```

```cppcx
// MainPage.h
...
#include "Recording.h"

namespace Quickstart
{
    public ref class MainPage sealed
    {
    private:
        RecordingViewModel ^ viewModel;
    public:
        MainPage();

        property RecordingViewModel^ ViewModel
        {
            RecordingViewModel^ get() { return this->viewModel; };
        }
    };
}

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();
    this->viewModel = ref new RecordingViewModel();
}
```

La última parte es enlazar un **TextBlock** a la propiedad **ViewModel.DefaultRecording.OneLiner**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <TextBlock Text="{x:Bind ViewModel.DefaultRecording.OneLineSummary}"
    HorizontalAlignment="Center"
    VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Si estás utilizando [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), deberás quitar la función **MainPage::ClickHandler** para que el proyecto se compile.

Este es el resultado.

![Enlace de un cuadro de texto](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>Enlazar a una colección de elementos.

Un escenario común es enlazar a una colección de objetos profesionales. En C# y Visual Basic, la clase [**ObservableCollection&lt;T&gt;** ](/dotnet/api/system.collections.objectmodel.observablecollection-1) genérica es una buena elección de colección para el enlace de datos porque implementa las interfaces [**INotifyPropertyChanged**](/dotnet/api/system.componentmodel.inotifypropertychanged) y [**INotifyCollectionChanged**](/dotnet/api/system.collections.specialized.inotifycollectionchanged). Estas interfaces proporcionan notificación de cambios a los enlaces cuando los elementos se añaden o se eliminan o cuando cambia una propiedad de la lista en sí. Si quieres que los controles enlazados se actualicen con los cambios en las propiedades de objetos de la colección, el objeto profesional también debe implementar **INotifyPropertyChanged**. Para obtener más información, consulta el tema [Enlace de datos en profundidad](data-binding-in-depth.md).

Si usas [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), puedes obtener más información sobre cómo enlazar a una colección observable en [Controles de elementos de XAML; enlazar a una colección C++/WinRT](../cpp-and-winrt-apis/binding-collection.md). Si lees primero ese tema, la intención del listado de código de C++/WinRT que se muestra a continuación estará más clara.

El siguiente ejemplo enlaza una [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) a una colección de objetos `Recording`. Empecemos agregando la colección a nuestro modelo de vista. Solo tienes que agregar estos miembros nuevos a la clase **RecordingViewModel**.

```csharp
public class RecordingViewModel
{
    ...
    private ObservableCollection<Recording> recordings = new ObservableCollection<Recording>();
    public ObservableCollection<Recording> Recordings{ get{ return this.recordings; } }
    public RecordingViewModel()
    {
        this.recordings.Add(new Recording(){ ArtistName = "Johann Sebastian Bach",
            CompositionName = "Mass in B minor", ReleaseDateTime = new DateTime(1748, 7, 8) });
        this.recordings.Add(new Recording(){ ArtistName = "Ludwig van Beethoven",
            CompositionName = "Third Symphony", ReleaseDateTime = new DateTime(1805, 2, 11) });
        this.recordings.Add(new Recording(){ ArtistName = "George Frideric Handel",
            CompositionName = "Serse", ReleaseDateTime = new DateTime(1737, 12, 3) });
    }
}
```

```cppwinrt
// RecordingViewModel.idl
// Add this property:
...
#include <winrt/Windows.Foundation.Collections.h>
...
Windows.Foundation.Collections.IVector<IInspectable> Recordings{ get; };
...

// RecordingViewModel.h
// Change the constructor declaration, and add this property and this field:
...
    RecordingViewModel();
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> Recordings();

private:
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_recordings;
...

// RecordingViewModel.cpp
// Update/add implementations like this:
...
RecordingViewModel::RecordingViewModel()
{
    std::vector<Windows::Foundation::IInspectable> recordings;

    Windows::Globalization::Calendar releaseDateTime;
    releaseDateTime.Month(7); releaseDateTime.Day(8); releaseDateTime.Year(1748);
    recordings.push_back(winrt::make<Recording>(L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(11); releaseDateTime.Day(2); releaseDateTime.Year(1805);
    recordings.push_back(winrt::make<Recording>(L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime));

    releaseDateTime = Windows::Globalization::Calendar{};
    releaseDateTime.Month(3); releaseDateTime.Day(12); releaseDateTime.Year(1737);
    recordings.push_back(winrt::make<Recording>(L"George Frideric Handel", L"Serse", releaseDateTime));

    m_recordings = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>(std::move(recordings));
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> RecordingViewModel::Recordings() { return m_recordings; }
...
```

```cppcx
// Recording.h
...
public ref class RecordingViewModel sealed
{
private:
    ...
    Windows::Foundation::Collections::IVector<Recording^>^ recordings;
public:
    RecordingViewModel()
    {
        ...
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1748;
        releaseDateTime->Month = 7;
        releaseDateTime->Day = 8;
        Recording^ recording = ref new Recording{ L"Johann Sebastian Bach", L"Mass in B minor", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1805;
        releaseDateTime->Month = 2;
        releaseDateTime->Day = 11;
        recording = ref new Recording{ L"Ludwig van Beethoven", L"Third Symphony", releaseDateTime };
        this->Recordings->Append(recording);
        releaseDateTime = ref new Windows::Globalization::Calendar();
        releaseDateTime->Year = 1737;
        releaseDateTime->Month = 12;
        releaseDateTime->Day = 3;
        recording = ref new Recording{ L"George Frideric Handel", L"Serse", releaseDateTime };
        this->Recordings->Append(recording);
    }
    ...
    property Windows::Foundation::Collections::IVector<Recording^>^ Recordings
    {
        Windows::Foundation::Collections::IVector<Recording^>^ get()
        {
            if (this->recordings == nullptr)
            {
                this->recordings = ref new Platform::Collections::Vector<Recording^>();
            }
            return this->recordings;
        };
    }
};
```

Después, enlaza un control [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) a la propiedad **ViewModel.Recordings**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Aún no hemos proporcionado una plantilla de datos para la clase **Recording**, por lo que lo mejor que puede hacer el marco de trabajo de la interfaz de usuario es llamar a [**ToString**](/dotnet/api/system.object.tostring#System_Object_ToString) para cada elemento del control [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView). La implementación predeterminada de **ToString** es devolver el nombre del tipo.

![Enlazar una vista de lista 1](images/xaml-databinding1.png)

Para solucionar este problema, podemos tanto invalidar [**ToString**](/dotnet/api/system.object.tostring#System_Object_ToString) para devolver el valor de **OneLineSummary**, o podemos proporcionar una plantilla de datos. La opción de la plantilla de datos es una solución más habitual y más flexible. Estableces una plantilla de datos mediante la propiedad [**ContentTemplate**](/uwp/api/windows.ui.xaml.controls.contentcontrol.contenttemplate) de un control de contenido o la propiedad [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) de un control de elementos. Hay dos formas en las que podríamos diseñar una plantilla de datos para **Recording** junto con una ilustración del resultado.

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <TextBlock Text="{x:Bind OneLineSummary}"/>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Enlazar una vista de lista 2](images/xaml-databinding2.png)

```xml
<ListView ItemsSource="{x:Bind ViewModel.Recordings}"
HorizontalAlignment="Center" VerticalAlignment="Center">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:Recording">
            <StackPanel Orientation="Horizontal" Margin="6">
                <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                <StackPanel>
                    <TextBlock Text="{x:Bind ArtistName}" FontWeight="Bold"/>
                    <TextBlock Text="{x:Bind CompositionName}"/>
                </StackPanel>
            </StackPanel>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

![Enlazar una vista de lista 3](images/xaml-databinding3.png)

Para más información sobre la sintaxis XAML, consulta el tema [Crear una interfaz de usuario con XAML](../design/basics/xaml-basics-ui.md). Para obtener más información acerca del diseño de controles, consulta [Definir diseños con XAML](../design/layout/layouts-with-xaml.md).

## <a name="adding-a-details-view"></a>Agregar una vista de detalles

Puedes elegir mostrar todos los detalles de objetos **Recording** en los elementos de [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView). Pero eso ocupa una gran cantidad de espacio. En su lugar, puedes mostrar solo los datos suficientes en el elemento para identificarlo y luego, cuando el usuario realiza una selección, se pueden mostrar todos los detalles del elemento seleccionado en una parte independiente de la interfaz de usuario conocida como la vista de detalles. Este tipo de organización es también conocido como una vista maestra/detallada o una vista de lista/detalles.

Esto se puede llevar a cabo de dos maneras. Puedes enlazar la vista de detalles a la propiedad [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem) de la [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView). O puedes usar [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), en cuyo caso enlaza tanto **ListView** como la vista de detalles a **CollectionViewSource** (que se encargará en tu lugar del elemento actualmente seleccionado). A continuación se muestran ambas técnicas y proporcionan los mismos resultados (que se muestran en la ilustración).

> [!NOTE]
> Hasta ahora en este tema solo hemos usado la [Extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md), pero ambas técnicas que te mostramos a continuación requieren la extensión más flexible (pero menos eficaz) [Extensión de marcado {Binding}](../xaml-platform/binding-markup-extension.md).

Si usas extensiones de componente de C++/WinRT o Visual C++ (C++/CX), para usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md), deberás agregar el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) a cualquier clase en tiempo de ejecución con la que quieras enlazar. Para usar [{x:Bind}](../xaml-platform/x-bind-markup-extension.md), no necesitas ese atributo.

> [!IMPORTANT]
> Si estás usando [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md), el atributo [**BindableAttribute**](/uwp/api/Windows.UI.Xaml.Data.BindableAttribute) está disponible si has instalado la versión de Windows SDK 10.0.17763.0 (Windows 10, versión 1809) o posterior. Sin ese atributo, tendrás que implementar las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para poder usar la extensión de marcado [{Binding}](../xaml-platform/binding-markup-extension.md).

Primero, esta es la técnica [**SelectedItem**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selecteditem).

```csharp
// No code changes necessary for C#.
```

```cppwinrt
// Recording.idl
// Add this attribute:
...
[Windows.UI.Xaml.Data.Bindable]
runtimeclass Recording
...
```

```cppcx
[Windows::UI::Xaml::Data::Bindable]
public ref class Recording sealed
{
    ...
};
```

El único cambio adicional necesario es al marcado.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <StackPanel HorizontalAlignment="Center" VerticalAlignment="Center">
            <ListView x:Name="recordingsListView" ItemsSource="{x:Bind ViewModel.Recordings}">
                <ListView.ItemTemplate>
                    <DataTemplate x:DataType="local:Recording">
                        <StackPanel Orientation="Horizontal" Margin="6">
                            <SymbolIcon Symbol="Audio" Margin="0,0,12,0"/>
                            <StackPanel>
                                <TextBlock Text="{x:Bind CompositionName}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
            <StackPanel DataContext="{Binding SelectedItem, ElementName=recordingsListView}"
            Margin="0,24,0,0">
                <TextBlock Text="{Binding ArtistName}"/>
                <TextBlock Text="{Binding CompositionName}"/>
                <TextBlock Text="{Binding ReleaseDateTime}"/>
            </StackPanel>
        </StackPanel>
    </Grid>
</Page>
```

Para la técnica [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), primero agrega un **CollectionViewSource** como recurso de página.

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

Y luego ajusta los enlaces en la [**ListView**](/uwp/api/Windows.UI.Xaml.Controls.ListView) (a la que ya no necesitas llamar) y en la vista de detalles usa el [**CollectionViewSource**](/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource). Ten en cuenta al enlazar la lista de detalles directamente al **CollectionViewSource**, estás dando por sentado que quieres enlazar el elemento actual a los enlaces donde la ruta de acceso no se encuentra en la propia colección. No es necesario especificar la propiedad **CurrentItem** como la ruta de acceso del enlace, aunque sí puedes hacerlo si encuentras alguna ambigüedad.

```xml
...
<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">
...
<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

Y este es el resultado idéntico en cada caso.

> [!NOTE]
> Si utilizas C++, la interfaz de usuario no tendrá exactamente el mismo aspecto que la siguiente ilustración: la representación de la propiedad **ReleaseDateTime** es diferente. Consulta la siguiente sección para obtener más información sobre esto.

![Enlazar una vista de lista 4](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>Formato o conversión de valores de datos para mostrar

Hay un problema con la representación anterior. La propiedad **ReleaseDateTime** no es solo una fecha, es un valor [**DateTime**](/uwp/api/windows.foundation.datetime) (si usas C++, es un elemento [**Calendar**](/uwp/api/windows.globalization.calendar)). Por lo tanto, en C#, se muestra con más precisión de la que necesitamos. Y en C++ se representa como un nombre de tipo. Una solución es agregar una propiedad de cadena para la clase **Recording** que devuelve el equivalente de `this.ReleaseDateTime.ToString("d")`. Darle a esa propiedad el nombre **ReleaseDate** indicaría que devuelve una fecha, y no una fecha y hora. Darle el nombre **ReleaseDateAsString** indicaría, además, que devuelve una cadena.

Una solución más flexible es usar algo conocido como un convertidor de valores. Este es un ejemplo de cómo crear tu propio convertidor de valores. Si usas C#, agrega el código siguiente al archivo de código fuente de `Recording.cs`. Si usas C++/WinRT, agrega un nuevo elemento **Midl File (.idl)** al proyecto, denominado como se muestra en el ejemplo de código de C++/WinRT que aparece a continuación, compila el proyecto para generar `StringFormatter.h` y `.cpp`, agrega los archivos al proyecto y, a continuación, pega en ellos los listados de código. Agrega también `#include "StringFormatter.h"` a `MainPage.h`.

```csharp
public class StringFormatter : Windows.UI.Xaml.Data.IValueConverter
{
    // This converts the value object to the string to display.
    // This will work with most simple types.
    public object Convert(object value, Type targetType,
        object parameter, string language)
    {
        // Retrieve the format string and use it to format the value.
        string formatString = parameter as string;
        if (!string.IsNullOrEmpty(formatString))
        {
            return string.Format(formatString, value);
        }

        // If the format string is null or empty, simply
        // call ToString() on the value.
        return value.ToString();
    }

    // No need to implement converting back on a one-way binding
    public object ConvertBack(object value, Type targetType,
        object parameter, string language)
    {
        throw new NotImplementedException();
    }
}
```

```cppwinrt
// StringFormatter.idl
namespace Quickstart
{
    runtimeclass StringFormatter : [default] Windows.UI.Xaml.Data.IValueConverter
    {
        StringFormatter();
    }
}

// StringFormatter.h
#pragma once

#include "StringFormatter.g.h"
#include <sstream>

namespace winrt::Quickstart::implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter>
    {
        StringFormatter() = default;

        Windows::Foundation::IInspectable Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
        Windows::Foundation::IInspectable ConvertBack(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& targetType, Windows::Foundation::IInspectable const& parameter, hstring const& language);
    };
}

namespace winrt::Quickstart::factory_implementation
{
    struct StringFormatter : StringFormatterT<StringFormatter, implementation::StringFormatter>
    {
    };
}

// StringFormatter.cpp
#include "pch.h"
#include "StringFormatter.h"
#include "StringFormatter.g.cpp"

namespace winrt::Quickstart::implementation
{
    Windows::Foundation::IInspectable StringFormatter::Convert(Windows::Foundation::IInspectable const& value, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar valueAsCalendar{ value.as<Windows::Globalization::Calendar>() };

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar.MonthAsNumericString().c_str();
        wstringstream << L"/" << valueAsCalendar.DayAsString().c_str();
        wstringstream << L"/" << valueAsCalendar.YearAsString().c_str();
        return winrt::box_value(hstring{ wstringstream.str().c_str() });
    }

    Windows::Foundation::IInspectable StringFormatter::ConvertBack(Windows::Foundation::IInspectable const& /* value */, Windows::UI::Xaml::Interop::TypeName const& /* targetType */, Windows::Foundation::IInspectable const& /* parameter */, hstring const& /* language */)
    {
        throw hresult_not_implemented();
    }
}
```

```cppcx
...
public ref class StringFormatter sealed : Windows::UI::Xaml::Data::IValueConverter
{
public:
    virtual Platform::Object^ Convert(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        // Retrieve the value as a Calendar.
        Windows::Globalization::Calendar^ valueAsCalendar = dynamic_cast<Windows::Globalization::Calendar^>(value);

        std::wstringstream wstringstream;
        wstringstream << L"Released: ";
        wstringstream << valueAsCalendar->MonthAsNumericString()->Data();
        wstringstream << L"/" << valueAsCalendar->DayAsString()->Data();
        wstringstream << L"/" << valueAsCalendar->YearAsString()->Data();
        return ref new Platform::String(wstringstream.str().c_str());
    }

    // No need to implement converting back on a one-way binding
    virtual Platform::Object^ ConvertBack(Platform::Object^ value, TypeName targetType, Platform::Object^ parameter, Platform::String^ language)
    {
        throw ref new Platform::NotImplementedException();
    }
};
...
```

> [!NOTE]
> Para la lista de códigos C++/WinRT anterior, en `StringFormatter.idl` usamos el [atributo predeterminado](/windows/desktop/midl/default) para declarar **IValueConverter** como interfaz predeterminada. En la lista, **StringFormatter** solo tiene un constructor y ningún método, por lo que no se genera ninguna interfaz predeterminada para él. El atributo `default` es óptimo si no vas a agregar miembros de instancia a **StringFormatter**, porque no se requerirá QueryInterface para llamar a los métodos **IValueConverter**. Como alternativa, puedes solicitar que se genere una interfaz predeterminada **IStringFormatter** anotando la propia clase en tiempo de ejecución con el [atributo default_interface](/uwp/midl-3/predefined-attributes#the-default_interface-attribute). Esta opción es óptima si agregas miembros de instancia a **StringFormatter** a los que se llama con más frecuencia que a los métodos de **IValueConverter**, porque después no se requerirá QueryInterface para llamar a los miembros de instancia.

Ahora podemos agregar una instancia de **StringFormatter** como un recurso de página y usarla en el enlace de **TextBlock** que muestra la propiedad **ReleaseDateTime**.

```xml
<Page.Resources>
    <local:StringFormatter x:Key="StringFormatterValueConverter"/>
</Page.Resources>
...
<TextBlock Text="{Binding ReleaseDateTime,
    Converter={StaticResource StringFormatterValueConverter},
    ConverterParameter=Released: \{0:d\}}"/>
...
```

Como puedes ver más arriba, para la flexibilidad de los formatos, usamos el marcado para pasar una cadena de formato al convertidor mediante el parámetro de convertidor. En los ejemplos de código que se muestran en este tema, solo el convertidor de valores de C# hace uso de ese parámetro. Pero podrías pasar fácilmente una cadena de formato de estilo de C++ como parámetro de convertidor y usarla en el convertidor de valores con una función de formato como **wprintf** o **swprintf**.

Este es el resultado.

![visualización de una fecha con formato personalizado](images/xaml-databinding5.png)

> [!NOTE]
> A partir de Windows 10, versión 1607, el marco XAML proporciona un valor booleano integrado para el convertidor de visibilidad. El convertidor asigna **true** al valor de la enumeración **Visibility.Visible** y **false** a **Visibility.Collapsed**, para que puedas enlazar una propiedad Visibility a un valor booleano sin necesidad de crear un convertidor. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información sobre las versiones de destino, consulta [Código adaptativo para versiones](../debug-test-perf/version-adaptive-code.md).

## <a name="see-also"></a>Consulta también
* [Enlace de datos](index.md)