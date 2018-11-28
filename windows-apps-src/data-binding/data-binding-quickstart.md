---
ms.assetid: A9D54DEC-CD1B-4043-ADE4-32CD4977D1BF
title: Introducción al enlace de datos
description: En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o enlazar un control de elemento a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP).
ms.date: 10/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cppcx
ms.openlocfilehash: cc8e4d1753333579b016a44adf9429d355d29fb6
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7968329"
---
# <a name="data-binding-overview"></a>Introducción al enlace de datos

En este tema se muestra cómo enlazar un control (o cualquier otro elemento de interfaz de usuario) a un solo elemento o enlazar un control de elementos a una colección de elementos en una aplicación para la Plataforma universal de Windows (UWP). Además, te mostramos cómo controlar la representación de los elementos, implementar una vista de detalles basada en una selección y convertir datos para mostrarlos. Para obtener información más detallada, consulta [Enlace de datos a profundidad](data-binding-in-depth.md).

## <a name="prerequisites"></a>Requisitos previos

En este tema suponemos que sabes cómo crear una aplicación básica para UWP. Para obtener instrucciones sobre cómo crear tu primera aplicación para UWP, consulta [Introducción a las aplicaciones de Windows](https://developer.microsoft.com/windows/getstarted).

## <a name="create-the-project"></a>Crear el proyecto

Crea un nuevo proyecto de **Aplicación vacía (Windows Universal)**. Llámale "Quickstart".

## <a name="binding-to-a-single-item"></a>Enlace a un solo elemento

Cada enlace consta de un destino de enlace y de un origen de enlace. Normalmente, el destino es una propiedad de un control u otro elemento de interfaz de usuario y el origen es una propiedad de una instancia de clase (un modelo de datos o un modelo de vista). Este ejemplo muestra cómo enlazar un control a un solo elemento. El destino es la propiedad **Text** de un **TextBlock**. El origen es una instancia de una clase simple denominada **Recording** que representa una grabación de audio. Veamos primero la clase.

Si estás usando C# o C++ / CX, a continuación, agrega una nueva clase al proyecto y el nombre de la clase de **grabación**.

Si estás usando [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), a continuación, agregar nuevos elementos de **Midl File (.idl)** para el proyecto, denominado como se muestra en C++ / WinRT listado de ejemplo de código siguiente. Reemplaza el contenido de los nuevos archivos con el código de [MIDL 3.0](/uwp/midl-3/intro) se muestra en la descripción, compila el proyecto para generar `Recording.h` y `.cpp` y `RecordingViewModel.h` y `.cpp`y, a continuación, agrega código a los archivos generados para que coincida con la descripción. Para que obtener más información sobre estos archivos generados y cómo copiarlas en el proyecto, consulta [controles XAML; enlazar a C++ / WinRT propiedad](/windows/uwp/cpp-and-winrt-apis/binding-property).

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

Si estás usando [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), a continuación, primera actualización `MainPage.idl`. Compila el proyecto para volver a generar `MainPage.h` y `.cpp`y combinar los cambios en esos archivos generados en el que aparecen en el proyecto.

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

Si estás usando [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), a continuación, deberás quitar la función de **MainPage:: clickHandler** en orden para generar el proyecto.

Este es el resultado.

![Enlace de un cuadro de texto](images/xaml-databinding0.png)

## <a name="binding-to-a-collection-of-items"></a>Enlazar a una colección de elementos.

Un escenario común es enlazar a una colección de objetos profesionales. En C# y Visual Basic, la clase [**ObservableCollection&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/xaml/ms668604.aspx) genérica es una buena elección de colección para el enlace de datos porque implementa las interfaces [**INotifyPropertyChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.componentmodel.inotifypropertychanged.aspx) y [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx). Estas interfaces proporcionan notificación de cambios a los enlaces cuando los elementos se añaden o se eliminan o cuando cambia una propiedad de la lista en sí. Si quieres que los controles enlazados se actualicen con los cambios en las propiedades de objetos de la colección, el objeto profesional también debe implementar **INotifyPropertyChanged**. Para obtener más información, consulta el tema [Enlace de datos en profundidad](data-binding-in-depth.md).

Si estás usando [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), a continuación, puedes aprender más sobre el enlace a una colección observable en [controles de elementos XAML; enlazar a C++ / WinRT colección](/windows/uwp/cpp-and-winrt-apis/binding-collection). Si leen ese tema en primer lugar, a continuación, la intención de C++ / WinRT listado de código se muestra a continuación serán más claro.

El siguiente ejemplo enlaza una [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a una colección de objetos `Recording`. Empecemos agregando la colección a nuestro modelo de vista. Solo tienes que agregar estos miembros nuevos a la clase **RecordingViewModel**.

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
// Implement like this:
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

Después, enlaza un control [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) a la propiedad **ViewModel.Recordings**.

```xml
<Page x:Class="Quickstart.MainPage" ... >
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <ListView ItemsSource="{x:Bind ViewModel.Recordings}"
        HorizontalAlignment="Center" VerticalAlignment="Center"/>
    </Grid>
</Page>
```

Aún no hemos proporcionado una plantilla de datos para la clase **Recording**, por lo que lo mejor que puede hacer el marco de trabajo de la interfaz de usuario es llamar a [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) para cada elemento del control [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). La implementación predeterminada de **ToString** es devolver el nombre del tipo.

![Enlazar una vista de lista](images/xaml-databinding1.png)

Para solucionar esto, ya sea que podemos omitir [**ToString**](https://msdn.microsoft.com/library/windows/apps/system.object.tostring.aspx) para devolver el valor de **OneLineSummary**o podemos proporcionar una plantilla de datos. La opción de plantilla de datos es una solución más habitual y uno más flexible. Estableces una plantilla de datos mediante la propiedad [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/BR209369) de un control de contenido o la propiedad [**ItemTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242830) de un control de elementos. Hay dos formas en las que podríamos diseñar una plantilla de datos para **Recording** junto con una ilustración del resultado.

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

![Enlazar una vista de lista](images/xaml-databinding2.png)

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

![Enlazar una vista de lista](images/xaml-databinding3.png)

Para más información sobre la sintaxis XAML, consulta el tema [Crear una interfaz de usuario con XAML](https://msdn.microsoft.com/library/windows/apps/Mt228349). Para obtener más información acerca del diseño de controles, consulta [Definir diseños con XAML](https://msdn.microsoft.com/library/windows/apps/Mt228350).

## <a name="adding-a-details-view"></a>Agregar una vista de detalles

Puedes elegir mostrar todos los detalles de objetos **Recording** en los elementos de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). Pero eso ocupa una gran cantidad de espacio. En su lugar, puedes mostrar solo los datos suficientes en el elemento para identificarlo y luego, cuando el usuario realiza una selección, se pueden mostrar todos los detalles del elemento seleccionado en una parte independiente de la interfaz de usuario conocida como la vista de detalles. Este tipo de organización es también conocido como una vista maestra/detallada o una vista de lista/detalles.

Esto se puede llevar a cabo de dos maneras. Puedes enlazar la vista de detalles a la propiedad [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770) de la [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878). O bien, puedes usar un [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), en cuyo caso enlaza tanto **ListView** como la vista de detalles a la **CollectionViewSource** (realiza por lo que se encarga del elemento seleccionado actualmente para que puedas). A continuación se muestran ambas técnicas y proporcionan los mismos resultados (que se muestra en la ilustración).

> [!NOTE]
> Hasta ahora en este tema solo hemos usado la [Extensión de marcado {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), pero ambas técnicas que te mostramos a continuación requieren la extensión más flexible (pero menos eficaz) [Extensión de marcado {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782).

Si estás usando C++ / WinRT o VisualC ++ extensiones de componentes (C++ / CX), para usar la extensión de marcado [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) , tendrás que agregar el atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) a cualquier clase en tiempo de ejecución que quieres enlazar a. Para usar [{X: Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783), no necesitas este atributo.

> [!IMPORTANT]
> Si estás usando [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), el atributo [**BindableAttribute**](https://msdn.microsoft.com/library/windows/apps/Hh701872) está disponible si has instalado Windows SDK versión 10.0.17763.0 (Windows 10, versión 1809), o una versión posterior. Sin este atributo, tendrás que implementan las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) para poder usar la extensión de marcado [{Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) .

Primero, esta es la técnica [**SelectedItem**](https://msdn.microsoft.com/library/windows/apps/BR209770).

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

Para la técnica [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), primero agrega un **CollectionViewSource** como recurso de página.

```xml
<Page.Resources>
    <CollectionViewSource x:Name="RecordingsCollection" Source="{x:Bind ViewModel.Recordings}"/>
</Page.Resources>
```

Y luego ajusta los enlaces en la [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) (a la que ya no necesitas llamar) y en la vista de detalles usa el [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833). Ten en cuenta al enlazar la lista de detalles directamente al **CollectionViewSource**, estás dando por sentado que quieres enlazar el elemento actual a los enlaces donde la ruta de acceso no se encuentra en la propia colección. No es necesario especificar la propiedad **CurrentItem** como la ruta de acceso para el enlace, aunque sí puedes hacerlo si encuentras alguna ambigüedad.

```xml
...
<ListView ItemsSource="{Binding Source={StaticResource RecordingsCollection}}">
...
<StackPanel DataContext="{Binding Source={StaticResource RecordingsCollection}}" ...>
...
```

Y este es el resultado idéntico en cada caso.

> [!NOTE]
> Si estás usando C++, a continuación, la interfaz de usuario no se verá exactamente igual que en la ilustración siguiente: la representación de la propiedad **ReleaseDateTime** es diferente. Consulta la sección siguiente para obtener una explicación de este.

![Enlazar una vista de lista](images/xaml-databinding4.png)

## <a name="formatting-or-converting-data-values-for-display"></a>Formato o conversión de valores de datos para mostrar

Hay un problema con la representación anterior. La propiedad **ReleaseDateTime** no es simplemente una fecha, es una [**fecha y hora**](/uwp/api/windows.foundation.datetime) (si estás usando C++, a continuación, se trata de un [**calendario**](/uwp/api/windows.globalization.calendar)). Por lo tanto, en C#, que se muestra con más precisión que necesitamos. Y en C++ que se represente como un nombre de tipo. Una solución es agregar una propiedad de cadena a la clase de **grabación** que se devuelve el equivalente de `this.ReleaseDateTime.ToString("d")`. Darle a esa propiedad **ReleaseDate** indicaría que devuelve una fecha y no una fecha y hora. Darle el nombre **ReleaseDateAsString** indicaría, además, que devuelve una cadena.

Una solución más flexible es usar algo conocido como un convertidor de valores. Este es un ejemplo de cómo crear tu propio convertidor de valores. Agrega este código al archivo de código fuente Recording.cs.

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
    runtimeclass StringFormatter : Windows.UI.Xaml.Data.IValueConverter
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

Ahora podemos agregar una instancia de **StringFormatter** como un recurso de página y usarla en nuestro enlace.

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

Como puedes ver anteriormente, para dar formato a flexibilidad usamos el marcado para pasar una cadena de formato en el convertidor mediante el parámetro de convertidor. En los ejemplos de código se muestra en este tema, solo el C# convertidor de valores hace uso de dicho parámetro. Pero, fácilmente podría pasar una cadena de formato de estilo de C++ como el parámetro de convertidor y usarlo en el convertidor de valores con una función de formato como **wprintf** o **swprintf**.

Este es el resultado.

![visualización de una fecha con formato personalizado](images/xaml-databinding5.png)

> [!NOTE]
> A partir de Windows 10, versión 1607, el marco XAML proporciona un convertidor de valor booleano a visibilidad integrado. El convertidor asigna **true** para el valor de enumeración **Visibility.Visible** y **false** en **Visibility.Collapsed** por lo que se puede enlazar una propiedad Visibility a un valor booleano sin tener que crear un convertidor. Para usar el convertidor integrado, la versión mínima del SDK de destino de la aplicación debe ser 14393 o posterior. No puedes usarlo si la aplicación está destinada a versiones anteriores de Windows 10. Para obtener más información acerca de las versiones de destino, consulta el [código adaptable para la versión](https://msdn.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

## <a name="see-also"></a>Consulta también
* [Enlace de datos](index.md)
