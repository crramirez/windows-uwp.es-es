---
description: Una propiedad que se puede enlazar de forma eficaz a un control de XAML se conoce como una propiedad *observable*. En este tema se muestra cómo implementar y consumir una propiedad observable y cómo enlazar un control de XAML a dicha propiedad.
title: Controles de XAML; enlazar a una propiedad de C++/WinRT
ms.date: 06/21/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, control, binding, property
ms.localizationpriority: medium
ms.openlocfilehash: b6e663ec77c66d4a018d388da350794771312b77
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154389"
---
# <a name="xaml-controls-bind-to-a-cwinrt-property"></a>Controles de XAML; enlazar a una propiedad de C++/WinRT

Una propiedad que se puede enlazar de forma eficaz a un control de XAML se conoce como una propiedad *observable*. Esta idea se basa en el patrón de diseño de software conocido como *patrón observador*. En este tema se muestra cómo implementar propiedades observables en [C++/WinRT](./intro-to-using-cpp-with-winrt.md) y cómo enlazar controles de elementos XAML a dichas propiedades (para obtener información general, consulta [Enlace de datos](../data-binding/index.md)).

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="what-does-observable-mean-for-a-property"></a>¿Qué significa *observable* con respecto a una propiedad?

Supongamos que una clase en tiempo de ejecución denominada **BookSku** tiene una propiedad denominada **Title**. Si **BookSku** elige generar el evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) siempre que el valor de **Title** cambie, la propiedad **Title** será una propiedad observable. Es el comportamiento de **BookSku** (generar o no generar el evento) que determina cuáles de sus propiedades, si las hay, son observables.

Un elemento de texto XAML o control puede enlazarse a estos eventos y controlarlos mediante la recuperación de los valores actualizados y luego actualizarse automáticamente para mostrar el nuevo valor.

> [!NOTE]
> Para más información sobre cómo instalar y usar la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="create-a-blank-app-bookstore"></a>Crear una aplicación en blanco (Bookstore)

Para empezar, crea un proyecto en Microsoft Visual Studio. Crea un proyecto **Aplicación en blanco (C++/WinRT)** y asígnale el nombre *Bookstore*. Asegúrese de que la opción **Colocar la solución y el proyecto en el mismo directorio** esté desactivada. Elija como destino la versión más reciente disponible de manera general (es decir, no en versión preliminar) de Windows SDK.

Vamos a crear una nueva clase para representar un libro que tenga una propiedad de título observable. Vamos a crear y consumir la clase dentro de la misma unidad de compilación. Pero queremos poder enlazar a esta clase desde XAML y para ello tiene que ser una clase en tiempo de ejecución. Y vamos a usar C++/WinRT tanto para crearla como para consumirla.

El primer paso para crear una clase en tiempo de ejecución es agregar un nuevo elemento **Archivo MIDL (.idl)** al proyecto. Asígnale el nombre `BookSku.idl`. Elimina el contenido predeterminado de `BookSku.idl` y pégalo en esta declaración de clase en tiempo de ejecución.

```idl
// BookSku.idl
namespace Bookstore
{
    runtimeclass BookSku : Windows.UI.Xaml.Data.INotifyPropertyChanged
    {
        String Title;
    }
}
```

> [!NOTE]
> Las clases de tu modelo de vista, en realidad cualquier clase en tiempo de ejecución que declares en la aplicación, no deben derivar de una clase base. La clase **BookSku** declarada anteriormente es un ejemplo de ello. Implementa una interfaz, pero no deriva de ninguna clase base.
>
> Cualquier clase en tiempo de ejecución que declares en la aplicación y *que derive* de una clase base se conoce como una clase *que admite composición*. Las clases que admiten composición tienen restricciones. Para que una aplicación pase las pruebas del [kit de certificación de aplicaciones en Windows](../debug-test-perf/windows-app-certification-kit.md) que Visual Studio y Microsoft Store utilizan para validar los envíos (y para que la aplicación se incorpore correctamente a Microsoft Store), una clase que admite composición debe derivar en última instancia de una clase base de Windows. Eso significa que, en la raíz misma de la jerarquía de herencia, la clase debe ser un tipo que se origina en un espacio de nombres Windows.*. Si necesitas derivar una clase en tiempo de ejecución de una clase base, por ejemplo para implementar una clase **BindableBase** para todos los modelos de vista de los que puedes derivar, puedes derivar de [**Windows.UI.Xaml.DependencyObject**](/uwp/api/windows.ui.xaml.dependencyobject).
>
> Un modelo de vista es una abstracción de una vista y, por lo tanto, está enlazado directamente a la vista (el marcado XAML). Un modelo de datos es una abstracción de datos, solo lo consumen los modelos de vista y no se enlazan directamente a XAML. Por lo tanto, en lugar de declarar los modelos de datos como clases en tiempo de ejecución, puede hacerlo como estructuras o clases de C++. No es necesario declararlos en MIDL y tienes la libertad de usar cualquier jerarquía de herencia que prefieras.

Guarda el archivo y compila el proyecto. Durante el proceso de compilación, la herramienta `midl.exe` se ejecuta para crear un archivo de metadatos de Windows Runtime (`\Bookstore\Debug\Bookstore\Unmerged\BookSku.winmd`) que describe la clase en tiempo de ejecución. Después se ejecutará la herramienta `cppwinrt.exe` para generar archivos de código fuente y ayudarte a crear y consumir tu clase en tiempo de ejecución. Estos archivos incluyen códigos auxiliares para que puedas empezar a implementar la clase en tiempo de ejecución **BookSku** que declaraste en tu archivo IDL. Estos archivos de código auxiliar son `\Bookstore\Bookstore\Generated Files\sources\BookSku.h` y `BookSku.cpp`.

Haz clic con el botón derecho en el nodo del proyecto y haz clic en **Abrir carpeta en el Explorador de archivos**. Se abre la carpeta del proyecto en el Explorador de archivos. Ahí, copia los archivos de código auxiliar `BookSku.h` y `BookSku.cpp` de la carpeta `\Bookstore\Bookstore\Generated Files\sources\` a la carpeta del proyecto, que es `\Bookstore\Bookstore\`. En el **Explorador de soluciones** con el nodo de proyecto seleccionado, asegúrate de que **Mostrar todos los archivos** esté activado. Haz clic con el botón derecho en los archivos de código auxiliar que has copiado y luego haz clic en **Incluir en el proyecto**.

## <a name="implement-booksku"></a>Implementar **BookSku**
Ahora, vamos a abrir `\Bookstore\Bookstore\BookSku.h` y `BookSku.cpp` e implementar nuestra clase en tiempo de ejecución. Efectúe estos cambios en `BookSku.h`.

- Agregue un constructor que tome el valor [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Este valor es la cadena de título.
- Agregue un miembro privado para almacenar la cadena de título.
- Agregue otro miembro privado para el evento que se generará cuando el título cambie.

Después de hacer estos cambios, tu `BookSku.h` tendrá este aspecto.

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

        winrt::hstring Title();
        void Title(winrt::hstring const& value);
        winrt::event_token PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& value);
        void PropertyChanged(winrt::event_token const& token);
    
    private:
        winrt::hstring m_title;
        winrt::event<Windows::UI::Xaml::Data::PropertyChangedEventHandler> m_propertyChanged;
    };
}
```

Implementa las funciones en `BookSku.cpp` de este modo.

```cppwinrt
// BookSku.cpp
#include "pch.h"
#include "BookSku.h"
#include "BookSku.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookSku::BookSku(winrt::hstring const& title) : m_title{ title }
    {
    }

    winrt::hstring BookSku::Title()
    {
        return m_title;
    }

    void BookSku::Title(winrt::hstring const& value)
    {
        if (m_title != value)
        {
            m_title = value;
            m_propertyChanged(*this, Windows::UI::Xaml::Data::PropertyChangedEventArgs{ L"Title" });
        }
    }

    winrt::event_token BookSku::PropertyChanged(Windows::UI::Xaml::Data::PropertyChangedEventHandler const& handler)
    {
        return m_propertyChanged.add(handler);
    }

    void BookSku::PropertyChanged(winrt::event_token const& token)
    {
        m_propertyChanged.remove(token);
    }
}
```

En la función de mutación **Title**, comprobamos si se ha establecido un valor diferente del valor actual. Y, si es el caso, actualizamos el título y también generamos el evento [**INotifyPropertyChanged::PropertyChanged**](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged) con un argumento igual que el nombre de la propiedad que cambió. Esto sirve para que la interfaz de usuario sepa qué valor de propiedad tiene que volver a consultar.

## <a name="declare-and-implement-bookstoreviewmodel"></a>Declarar e implementar **BookstoreViewModel**
Nuestra página principal de XAML se enlazará a un modelo de vista principal. Y ese modelo de vista tendrá varias propiedades, incluida una de tipo **BookSku** . En este paso, declararemos e implementaremos la clase en tiempo de ejecución del modelo de vista principal.

Agrega un nuevo elemento **Midl File (.idl)** denominado `BookstoreViewModel.idl`. Pero consulta también [Factorizar clases en tiempo de ejecución en archivos Midl (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl).

```idl
// BookstoreViewModel.idl
import "BookSku.idl";

namespace Bookstore
{
    runtimeclass BookstoreViewModel
    {
        BookSku BookSku{ get; };
    }
}
```

Guarda y compila. Copia `BookstoreViewModel.h` y `BookstoreViewModel.cpp` desde la carpeta `Generated Files\sources` en la carpeta del proyecto e inclúyelos en el proyecto. Abre estos archivos e implementa la clase en tiempo de ejecución como se indica a continuación. Observa cómo, en `BookstoreViewModel.h`, hemos incluido `BookSku.h`, que declara el tipo de implementación para **BookSku** (que es **winrt::Bookstore::implementation::BookSku**). Y hemos quitado `= default` del constructor predeterminado.

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
#include "BookstoreViewModel.g.cpp"

namespace winrt::Bookstore::implementation
{
    BookstoreViewModel::BookstoreViewModel()
    {
        m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    }

    Bookstore::BookSku BookstoreViewModel::BookSku()
    {
        return m_bookSku;
    }
}
```

> [!NOTE]
> El tipo de `m_bookSku` es el tipo proyectado (**winrt::Bookstore::BookSku**) y el parámetro de plantilla que usas con [**winrt::make**](/uwp/cpp-ref-for-winrt/make) es el tipo de implementación (**winrt::Bookstore::implementation::BookSku**). Aun así, **make** devuelve una instancia del tipo proyectado.

## <a name="add-a-property-of-type-bookstoreviewmodel-to-mainpage"></a>Agrega una propiedad de tipo **BookstoreViewModel** a **MainPage**
Abre `MainPage.idl`, que declara la clase en tiempo de ejecución que representa nuestra página de interfaz de usuario principal. Agrega una instrucción de importación para importar `BookstoreViewModel.idl` y agrega una propiedad de solo lectura denominada MainViewModel de tipo **BookstoreViewModel** . Quita también la propiedad **MyProperty**. Observa también la directiva `import` en la lista siguiente.

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

Guarde el archivo. El proyecto no se compilará totalmente en este momento, pero compilar ahora resulta útil porque genera los archivos de código fuente en el que implementarás la clase en tiempo de ejecución **MainPage** (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` y `MainPage.cpp`). Así que compila ahora. En esta fase, el error de compilación que verás es **"MainViewModel": no es miembro de "winrt::Bookstore::implementation::MainPage"** .

Si omites la inclusión de `BookstoreViewModel.idl` (consulta la lista anterior de `MainPage.idl`), verás el error **se espera \< cerca de "MainViewModel"** . Otra sugerencia es asegurarte de que dejas todos los tipos en el mismo espacio de nombres, que se muestra en los listados de código.

Para resolver el error que esperamos ver, tendrás que copiar el código auxiliar del descriptor de acceso de la propiedad **MainViewModel** fuera de los archivos generados (`\Bookstore\Bookstore\Generated Files\sources\MainPage.h` y `MainPage.cpp`) y en `\Bookstore\Bookstore\MainPage.h` y `MainPage.cpp`. Estos son los pasos para hacerlo.

En `\Bookstore\Bookstore\MainPage.h`, incluye `BookstoreViewModel.h`, que declara el tipo de implementación para **BookstoreViewModel** (que es **winrt::Bookstore::implementation::BookstoreViewModel**). Agrega un miembro privado para almacenar el modelo de vista. Ten en cuenta que la función de descriptor de acceso de propiedad (y el miembro m_mainViewModel) se implementan en términos del tipo proyectado para **BookstoreViewModel** (que es **Bookstore::BookstoreViewModel**). El tipo de implementación está en el mismo proyecto (unidad de compilación) que la aplicación, por lo que construimos m_mainViewModel a través de la sobrecarga de constructor que toma **std::nullptr_t**. Quita también la propiedad **MyProperty**.

```cppwinrt
// MainPage.h
...
#include "BookstoreViewModel.h"
...
namespace winrt::Bookstore::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        Bookstore::BookstoreViewModel MainViewModel();

        void ClickHandler(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);

    private:
        Bookstore::BookstoreViewModel m_mainViewModel{ nullptr };
    };
}
...
```

En `\Bookstore\Bookstore\MainPage.cpp`, como se muestra en la lista siguiente, realice los cambios siguientes. Llame a [**winrt::make**](/uwp/cpp-ref-for-winrt/make) (con el tipo de implementación **BookstoreViewModel**) para asignar una nueva instancia del tipo **BookstoreViewModel** proyectado a *m_mainViewModel*. Como hemos visto anteriormente, el constructor **BookstoreViewModel** crea un nuevo objeto **BookSku** como miembro de datos privado, estableciendo su título inicialmente en `L"Atticus"`. En el controlador de eventos del botón (**ClickHandler**), actualice el título del libro a su título publicado. Por último, implemente el descriptor de acceso para la propiedad **MainViewModel**. Quita también la propiedad **MyProperty**.

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

using namespace winrt;
using namespace Windows::UI::Xaml;

namespace winrt::Bookstore::implementation
{
    MainPage::MainPage()
    {
        m_mainViewModel = winrt::make<Bookstore::implementation::BookstoreViewModel>();
        InitializeComponent();
    }

    void MainPage::ClickHandler(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* args */)
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
Abre `MainPage.xaml`, que contiene el marcado XAML de nuestra página principal de la interfaz de usuario. Tal y como muestra el listado siguiente, quita el nombre del el botón y cambia el valor de la propiedad **Content** de un literal a una expresión de enlace. Ten en cuenta la propiedad `Mode=OneWay` en la expresión de enlace (unidireccional desde el modelo de vista a la interfaz de usuario). Sin dicha propiedad, la interfaz de usuario no responderá a los eventos cambiados de propiedad.

```xaml
<Button Click="ClickHandler" Content="{x:Bind MainViewModel.BookSku.Title, Mode=OneWay}"/>
```

Ahora compila y ejecuta el proyecto. Haz clic en el botón para ejecutar el controlador de eventos **Clic**. Este controlador llama la función de mutación de título del libro; la mutación genera un evento para que la interfaz de usuario sepa que la propiedad **Title** ha cambiado; y el botón vuelve a consultar el valor de dicha propiedad para actualizar su propio valor **Content**.

## <a name="using-the-binding-markup-extension-with-cwinrt"></a>Uso de la extensión de marcado {Binding} con C++/WinRT
En la versión actualmente publicada de C++/WinRT, para poder usar la extensión de marcado {Binding} se deben implementar las interfaces [ICustomPropertyProvider](/uwp/api/windows.ui.xaml.data.icustompropertyprovider) y [ICustomProperty](/uwp/api/windows.ui.xaml.data.icustomproperty).

## <a name="element-to-element-binding"></a>Enlace de elemento a elemento

Se puede enlazar la propiedad de un elemento XAML a la propiedad de otro elemento XAML. En este ejemplo se muestra cómo queda en marcado.

```xaml
<TextBox x:Name="myTextBox" />
<TextBlock Text="{x:Bind myTextBox.Text, Mode=OneWay}" />
```

Debes declarar la entidad XAML con nombre `myTextBox` como una propiedad de solo lectura en el archivo Midl (.idl).

```idl
// MainPage.idl
runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
{
    MainPage();
    Windows.UI.Xaml.Controls.TextBox myTextBox{ get; };
}
```

Este es el motivo de tal necesidad. Todos los tipos que el compilador XAML debe validar (incluidas aquellas utilizadas en [{x: Bind}](../xaml-platform/x-bind-markup-extension.md)) se leen desde Metadatos de Windows (WinMD). Lo único que necesita hacer es agregar la propiedad de solo lectura al archivo Midl. No la implementes, porque el código XAML subyacente generado automáticamente proporciona la implementación por ti.

## <a name="consuming-objects-from-xaml-markup"></a>Consumir objetos a partir del marcado XAML

Todas las entidades consumidas por la [**extensión de marcado {x:Bind}** ](../xaml-platform/x-bind-markup-extension.md) de XAML deben exponerse públicamente en IDL. Además, si el marcado XAML contiene una referencia a otro elemento que también está en el marcado, el captador de dicho marcado debe estar presente en IDL.

```xaml
<Page x:Name="MyPage">
    <StackPanel>
        <CheckBox x:Name="UseCustomColorCheckBox" Content="Use custom color"
             Click="UseCustomColorCheckBox_Click" />
        <Button x:Name="ChangeColorButton" Content="Change color"
            Click="{x:Bind ChangeColorButton_OnClick}"
            IsEnabled="{x:Bind UseCustomColorCheckBox.IsChecked.Value, Mode=OneWay}"/>
    </StackPanel>
</Page>
```

El elemento *ChangeColorButton* hace referencia al elemento *UseCustomColorCheckBox* a través del enlace. Por lo tanto, el IDL de esta página debe declarar una propiedad de solo lectura denominada *UseCustomColorCheckBox* para que sea accesible para el enlace.

El delegado del controlador de eventos click para *UseCustomColorCheckBox* usa la sintaxis de delegado de XAML clásica, de modo que no necesita una entrada en el IDL; tan solo tiene que ser público en la clase de la implementación. Por otro lado, *ChangeColorButton* también tiene un controlador de eventos click `{x:Bind}`, que también debe entrar en el IDL.

```idl
runtimeclass MyPage : Windows.UI.Xaml.Controls.Page
{
    MyPage();

    // These members are consumed by binding.
    void ChangeColorButton_OnClick();
    Windows.UI.Xaml.Controls.CheckBox UseCustomColorCheckBox{ get; };
}
```

No es necesario que proporciones una implementación para la propiedad **UseCustomColorCheckBox.** El generador de código XAML lo hace automáticamente.

### <a name="binding-to-boolean"></a>Enlazar a un valor booleano

Puedes hacerlo en modo de diagnóstico.

<syntaxhighlight lang="xml">
<TextBlock Text="{Binding CanPair}"/>
</syntaxhighlight>

Esto muestra `true` o `false` en C++/CX, pero muestra **Windows.Foundation.IReference`1<Boolean>** en C++/WinRT.

Usa `x:Bind` al enlazar a un valor booleano.

```xaml
<TextBlock Text="{x:Bind CanPair}"/>
```

## <a name="important-apis"></a>API importantes
* [INotifyPropertyChanged::PropertyChanged](/uwp/api/windows.ui.xaml.data.inotifypropertychanged.PropertyChanged)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Temas relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Crear API con C++/WinRT](author-apis.md)