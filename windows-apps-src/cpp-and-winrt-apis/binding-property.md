---
author: stevewhims
description: Una propiedad que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una propiedad *observable*. Este tema muestra cómo implementar y consumir una propiedad observable y cómo enlazar un control XAML a dicha propiedad.
title: Controles XAML; enlazar a una propiedad C++/WinRT.
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, XAML, control, enlace, propiedad
ms.localizationpriority: medium
ms.openlocfilehash: 367bf5d5d554bd094ce3d5b726b818c8c388d398
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787162"
---
# <a name="xaml-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-property"></a>Controles XAML; enlazar a una propiedad [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
Una propiedad que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una propiedad *observable*. Esta idea se basa en el modelo de diseño de software conocido como el *patrón observador*. Este tema muestra cómo implementar propiedades observables en C++/WinRT y cómo enlazar controles de elementos XAML a dichas propiedades.

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>¿Qué significa *observable* con respecto a una propiedad?
Supongamos que una clase en tiempo de ejecución denominada **BookSku** tiene una propiedad denominada **Title**. Si **BookSku** elige generar el evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) siempre que el valor de **Title** cambie, la propiedad **Title** será una propiedad observable. Es el comportamiento de **BookSku** (generar o no generar el evento) que determina cuáles de sus propiedades, si las hay, son observables.

Un elemento de texto XAML o control puede enlazarse a estos eventos y controlarlos mediante la recuperación de los valores actualizados y luego actualizarse automáticamente para mostrar el nuevo valor.

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="create-a-blank-app-bookstore"></a>Crear una aplicación en blanco (Bookstore)
Comienza creando un proyecto nuevo en Microsoft Visual Studio Crea un proyecto **Visual C++ Core App (C++/WinRT)** y asígnale el nombre *Bookstore*.

Vamos a crear una nueva clase para representar un libro que tenga una propiedad de título observable. Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos poder enlazar a esta clase desde XAML y para ello tiene que ser una clase en tiempo de ejecución. Y vamos a usar C++/WinRT para crearla y consumirla.

El primer paso para crear una nueva clase en tiempo de ejecución es agregar un nuevo elemento **Midl File (.idl)** al proyecto. Asígnale el nombre `BookSku.idl`. Elimina el contenido predeterminado de `BookSku.idl` y pégalo en esta declaración de clase en tiempo de ejecución.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.DependencyObject, Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!IMPORTANT]
> Para una aplicación pasar el [Kit de certificación de aplicación de Windows](../debug-test-perf/windows-app-certification-kit.md) pruebas utilizan por el Store Microsoft para validar los envíos y, por lo tanto, para recopilar correctamente en el Microsoft Store, la clase base definitiva de cada clase de tiempo de ejecución *declaradas en la aplicación de* debe ser un tipo que se originan en un espacio de nombres Windows.*.

Para cumplir este requisito, deriva tus clases del modelo de vista desde [**Windows.UI.Xaml.DependencyObject **](/uwp/api/windows.ui.xaml.dependencyobject). Como alternativa, declara una clase base enlazable derivada desde **DependencyObject** y deriva tus modelos de vista desde ahí. Puedes declarar tus modelos de datos como estructuras de C++; no necesitan estar declarados en MIDL (siempre y cuando vayas a consumirlos solo desde tus modelos de vista y no enlaces XAML directamente a ellos, en cuyo caso podrían ser modelos de vista por definición, de todos modos).

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecutará para crear un archivo de metadatos de Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución **BookSku** que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` y `BookSku.cpp`

Copia los archivos de código auxiliar `BookSku.h` y `BookSku.cpp` desde `\Bookstore\Bookstore\Generated Files\sources\` a la carpeta del proyecto, que es `\Bookstore\Bookstore\`. En el **Explorador de soluciones**, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y haz clic en **Incluir en el proyecto**.

## <a name="implement-booksku"></a>Implementar **BookSku**
Ahora, vamos a abrir `\Bookstore\Bookstore\BookSku.h` y `BookSku.cpp` e implementar nuestra clase en tiempo de ejecución. En `BookSku.h`, agrega un constructor que tome un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring), un miembro privado para almacenar la cadena de título y otro para el evento que generaremos cuando cambie el título. Una vez agregados, tu `BookSku.h` tendrá este aspecto.

```cppwinrt
// BookSku.h
#pragma once

#include "BookSku.g.h"

namespace winrt::Bookstore::implementation
{
    struct BookSku : BookSkuT<BookSku>
    {
        BookSku() = delete;
        BookSku(winrt::hstring const& title);

        hstring Title();
        void Title(winrt::hstring const& value);
        event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        hstring m_title;
        event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

Implementa las funciones en `BookSku.cpp` de este modo.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    hstring BookSku::Title()
    {
        return m_title;
    }

    BookSku::BookSku(winrt::hstring const& title)
    {
        Title(title);
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

En la función de mutación **Title**, comprobamos si se establece un valor diferente y, si es así, actualizamos el título y también generamos el evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) con un argumento igual al nombre de la propiedad que ha cambiado. Esto sirve para que la interfaz de usuario sepa qué valor de propiedad tiene que volver a consultar.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declarar e implementar **BookstoreViewModel**
Nuestra página principal de XAML se enlazará a un modelo de vista principal. Y ese modelo de vista tendrá varias propiedades, incluida una de tipo **BookSku **. En este paso, declararemos e implementaremos la clase en tiempo de ejecución del modelo de vista principal.

Agrega un nuevo elemento **Midl File (.idl)** denominado `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel : Windows.UI.Xaml.DependencyObject
    {
        BookSku BookSku{ get; };
    }
}
```

Guarda y compila Copia `BookstoreViewModel.h` y `BookstoreViewModel.cpp` desde la carpeta `Generated Files` en la carpeta del proyecto e inclúyelos en el proyecto. Abre estos archivos e implementa la clase en tiempo de ejecución de este modo.

```cppwinrt
// BookstoreViewModel.h
#pragma once

#include "BookstoreViewModel.g.h"
#include "BookSku.h"

namespace winrt::Bookstore::implementation
{
    struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
    {
        BookstoreViewModel();
        Bookstore::BookSku BookSku();

    private:
        Bookstore::BookSku m_bookSku{ nullptr };
    };
}
```

```cppwinrt
// BookstoreViewModel.cpp
#include "pch.h"
#include "BookstoreViewModel.h"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> El tipo de `m_bookSku` es el tipo proyectado (**winrt::Bookstore::BookSku**) y el parámetro de plantilla que usas con **make** es el tipo de implementación (**winrt::Bookstore::implementation::BookSku**) . Aun así, **make** devuelve una instancia del tipo proyectado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Agrega una propiedad de tipo **BookstoreViewModel** a **MainPage**
Abre `MainPage.idl`, que declara la clase en tiempo de ejecución que representa nuestra página de interfaz de usuario principal. Agrega una instrucción de importación para importar `BookstoreViewModel.idl` y agrega una propiedad de solo lectura denominada MainViewModel de tipo **BookstoreViewModel **. Quitar también la propiedad **MyProperty** . Tenga en cuenta también la directiva **import** en la siguiente lista.

```idl
// MainPage.idl
import "BookstoreViewModel.idl";

namespace Bookstore
{
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

Guarda el archivo. El proyecto no se compilará hasta su finalización en ese momento, pero ahora creación es algo útil se debe hacer porque vuelve a generar los archivos de código fuente en el que se implementa la clase de tiempo de ejecución de **MainPage** (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` y `MainPage.cpp`). Por lo tanto, vamos a crear ahora. El error de compilación puede esperar a que se vea en esta etapa es **'MainViewModel': no es un miembro de 'winrt::Bookstore::implementation::MainPage'**.

Si se omite la inclusión de `BookstoreViewModel.idl` (consulte el listado de `MainPage.idl` por encima), a continuación, verá el error **se espera \ < near "MainViewModel"**. Otra sugerencia es asegurarse de que deje de todos los tipos en el mismo espacio de nombres: espacio de nombres que se muestra en los listados de código.

Para solucionar el error que se esperaba, ahora que necesitará para copiar el código auxiliar de descriptor de acceso para la propiedad **MainViewModel** fuera de los archivos generados y en `\Bookstore\Bookstore\MainPage.h` y `MainPage.cpp`.

Para `\Bookstore\Bookstore\MainPage.h`, agrega un miembro privado para almacenar el modelo de vista. Ten en cuenta que la función de descriptor de acceso de propiedad (y el miembro m_mainViewModel) se implementan en términos de **Bookstore::BookstoreViewModel**, que es el tipo proyectado. El tipo de implementación está en el mismo proyecto (unidad de compilación), por lo que construimos m_mainViewModel a través de la sobrecarga de constructor que toma `nullptr_t`. Quitar también la propiedad **MyProperty** .

```cppwinrt
// MainPage.h
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& args);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

En `\Bookstore\Bookstore\MainPage.cpp`, incluye `BookstoreViewModel.h`, que declara el tipo de implementación. Llama a [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (con el tipo de implementación) para asignar una nueva instancia del tipo proyectado a m_mainViewModel. Asigna un valor inicial para el título del libro. Implementa el descriptor de acceso para la propiedad MainViewModel. Y, por último, actualiza el título del libro en el controlador de eventos del botón. Quitar también la propiedad **MyProperty** .

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "BookstoreViewModel.h"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
    {
        MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    }

    Bookstore::BookstoreViewModel MainPage::MainViewModel()
    {
        return m_mainViewModel;
    }
}
```

## <a name="bind-the-button-to-the-title-property"></a>Enlaza el botón a la propiedad **Title**
Abre `MainPage.xaml`, que contiene la marcación XAML de nuestra página principal de la interfaz de usuario. Quita el nombre desde el botón y cambia su valor de la propiedad **Content** desde un literal a una expresión de enlace. Ten en cuenta la propiedad `Mode=OneWay` en la expresión de enlace (unidireccional desde el modelo de vista a la interfaz de usuario). Sin dicha propiedad, la interfaz de usuario no responderá a los eventos cambiados de propiedad.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Ahora compila y ejecuta el proyecto. Haz clic en el botón para ejecutar el controlador de eventos **Clic**. Este controlador llama la función de mutación de título del libro; la mutación genera un evento para que la interfaz de usuario sepa que la propiedad **Title** ha cambiado; y el botón vuelve a consultar el valor de dicha propiedad para actualizar su propio valor **Content**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Uso de la extensión de marcado {Binding} con C + + / WinRT
Para la versión publicada actualmente de C + + / WinRT, para poder usar la extensión de marcado {Binding} que necesitará para implementar las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty) .

## <a name="important-apis"></a>API importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Artículos relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Crear API con C++/WinRT](author-apis.md)
