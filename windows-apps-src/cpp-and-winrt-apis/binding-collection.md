---
description: Una colección que se puede enlazar de forma eficaz a un control de elementos de XAML se conoce como una colección *observable*. En este tema se muestra cómo implementar y consumir una colección observable y cómo enlazar un control de elementos de XAML a dicha colección.
title: Controles de elementos de XAML; enlazar a una colección C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, XAML, control, binding, collection
ms.localizationpriority: medium
ms.openlocfilehash: 999238d72017b92f1eb64c2e3089305166f993f2
ms.sourcegitcommit: bf32c7ea6ca94b60dbd01cae279b31c6e0e5f338
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2019
ms.locfileid: "67348639"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Controles de elementos de XAML; enlazar a una colección C++/WinRT

Una colección que se puede enlazar de forma eficaz a un control de elementos de XAML se conoce como una colección *observable*. Esta idea se basa en el patrón de diseño de software conocido como *patrón observador*. En este tema se muestra cómo implementar colecciones observables en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) y cómo enlazar controles de elementos XAML a dichas colecciones.

Si deseas seguir este tema, es mejor que primero crees el proyecto que se describe en [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md). En este tema se agrega más código a ese proyecto, y se agrega a los conceptos explicados en ese tema.

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>¿Qué significa *observable* con respecto a una colección?
Si una clase en tiempo de ejecución que representa una colección elige generar el evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) cada vez que un elemento se agregue a ella o se quite de ella, la clase en tiempo de ejecución es una colección observable. El control de elementos XAML puede enlazarse a estos eventos, y controlarlos, mediante la recuperación de la colección actualizada y luego actualizarse automáticamente para mostrar los elementos actuales.

> [!NOTE]
> Para más información sobre cómo instalar y usar la Extensión de Visual Studio (VSIX) para C++/WinRT y el paquete de NuGet (que juntos proporcionan la plantilla de proyecto y compatibilidad de la compilación), consulta [Compatibilidad de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Adición de una colección **BookSkus** a **BookstoreViewModel**

En [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md), hemos agregado una propiedad de tipo **BookSku** a nuestro modelo de vista principal. En este paso, usaremos la plantilla de función de fábrica [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) para que nos ayude a implementar una colección observable de **BookSku** en el mismo modelo de vista.

Declara una nueva propiedad en `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
}
...
```

> [!NOTE]
> En la lista anterior de MIDL 3.0, observa que el tipo de la propiedad **BookSkus** es [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de **BookSku**. En la siguiente sección de este tema, vamos a enlazar el origen de elementos de [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) a **BookSkus**. Un cuadro de lista es un control de elementos, y para establecer correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), debes hacerlo con un valor de tipo **IObservableVector** (o **IVector**), o de un tipo de interoperabilidad como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

> [!WARNING]
> El código que se muestra en este tema se aplica a C++/WinRT versión 2.0.190530.8 y versiones posteriores. Si usas una versión anterior, tendrás que realizar algunos ajustes menores en el código mostrado. En el listado de MIDL 3.0 anterior, cambia la propiedad **BookSkus** a [**IObservableVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). Y, luego, también usa **IInspectable** (en lugar de **BookSku**) en la implementación.

Guarda y compila. Copia los códigos auxiliares del descriptor de acceso de `BookstoreViewModel.h` y `BookstoreViewModel.cpp` en la carpeta `\Bookstore\Bookstore\Generated Files\sources` (para más información, consulta el tema anterior, [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md)). Implementa esos códigos auxiliares de descriptor de acceso de esta manera.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Enlaza un elemento ListBox a la propiedad **BookSkus**.
Abre `MainPage.xaml`, que contiene el marcado XAML de nuestra página principal de la interfaz de usuario. Agrega el siguiente marcado dentro del mismo elemento **StackPanel** que **Button**.

```xaml
<ListBox ItemsSource="{x:Bind MainViewModel.BookSkus}">
    <ItemsControl.ItemTemplate>
        <DataTemplate x:DataType="local:BookSku">
            <TextBlock Text="{x:Bind Title, Mode=OneWay}"/>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ListBox>
```

En `MainPage.cpp`, agrega una línea de código al controlador de eventos **Clic** para anexar un libro a la colección.

```cppwinrt
// MainPage.cpp
...
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    MainViewModel().BookSku().Title(L"To Kill a Mockingbird");
    MainViewModel().BookSkus().Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Ahora compila y ejecuta el proyecto. Haz clic en el botón para ejecutar el controlador de eventos **Clic**. Observamos que la implementación de **Append** genera un evento para informar a la interfaz de usuario de que ha cambiado la colección; y el elemento **ListBox** vuelve a consultar la colección para actualizar su propio valor de **Items**. Igual que antes, el título de uno de los libros cambia. Dicho cambio de título se refleja en el botón y en el cuadro de lista.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Temas relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Crear API con C++/WinRT](author-apis.md)
