---
description: Una colección que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una colección *observable*. Este tema muestra cómo implementar y consumir una colección observable y cómo enlazar un control de elementos XAML a dicha colección.
title: Controles de elementos XAML; enlazar a una colección C++/WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, XAML, control, enlace, colección
ms.localizationpriority: medium
ms.openlocfilehash: c3551ebcc59ebfe426b0be8d5bd20f7578517a25
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649210"
---
# <a name="xaml-items-controls-bind-to-a-cwinrt-collection"></a>Controles de elementos XAML; enlazar a una colección C++/WinRT

Una colección que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una colección *observable*. Esta idea se basa en el modelo de diseño de software conocido como el *patrón observador*. En este tema se muestra cómo implementar las colecciones observables en [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), y cómo enlazar XAML elementos controla a ellos.

Este tutorial se basa en el proyecto creado en [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md) y se agrega a los conceptos que se explican en este tema.

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>¿Qué significa *observable* con respecto a una colección?
Si una clase en tiempo de ejecución que representa una colección elige generar el evento [**IObservableVector&lt;T&gt;:: VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) cada vez que un elemento se agregue a ella o se quite de ella, la clase en tiempo de ejecución es una colección observable. El control de elementos XAML puede enlazarse a estos eventos y controlarlos mediante la recuperación de la colección actualizada y luego actualizarse automáticamente para mostrar los elementos actuales.

> [!NOTE]
> Para obtener información sobre cómo instalar y usar C++ / c++ / WinRT extensión de Visual Studio (VSIX) (que proporciona compatibilidad con plantillas de proyecto) vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Agregar una colección **BookSkus** a **BookstoreViewModel**

En [Controles XAML; enlaza a una propiedad C++/WinRT](binding-property.md), hemos agregado una propiedad de tipo **BookSku** a nuestro modelo de vista principal. En este paso, vamos a usar el [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) plantilla de función de generador que nos ayudarán a implementar una colección observable de **BookSku** en el mismo modelo de vista.

> [!NOTE]
> Si no ha instalado el SDK de Windows versión 10.0.17763.0 (Windows 10, versión 1809) o una versión posterior, a continuación, consulte [si tiene una versión anterior del SDK de Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) para obtener una lista de una plantilla de vector observable que puede usar en lugar de **winrt::single_threaded_observable_vector**.

Declara una nueva propiedad en `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IObservableVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> En la lista anterior de MIDL 3.0, tenga en cuenta que el tipo de la **BookSkus** propiedad es [ **IObservableVector** ](/uwp/api/windows.foundation.collections.ivector_t_) de [ **IInspectable** ](/windows/desktop/api/inspectable/nn-inspectable-iinspectable). En la siguiente sección de este tema, se va a enlazar el origen de los elementos de un [ **ListBox** ](/uwp/api/windows.ui.xaml.controls.listbox) a **BookSkus**. Un cuadro de lista es un control de elementos y establecer correctamente la [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propiedad, deberá establecerlo en un valor de tipo **IObservableVector** (o **IVector**) de **IInspectable**, o de un tipo de interoperabilidad como [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Guarda y compila. Copia los códigos auxiliares de descriptor desde `BookstoreViewModel.h` y `BookstoreViewModel.cpp` en la carpeta `Generated Files` e impleméntalos.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = winrt::make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="bind-a-listbox-to-the-bookskus-property"></a>Enlaza un ListBox a la propiedad **BookSkus**.
Abre `MainPage.xaml`, que contiene la marcación XAML de nuestra página principal de la interfaz de usuario. Agrega la siguiente marcación dentro del mismo **StackPanel** como el **Button**.

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

Ahora compila y ejecuta el proyecto. Haz clic en el botón para ejecutar el controlador de eventos **Clic**. Observamos que la implementación de **Append** genera un evento para informar a la interfaz de usuario de que ha cambiado la colección; y el **ListBox** vuelve a consultar la colección para actualizar sus propios valores de **Items**. Igual que antes, el título de uno de los libros cambia. Dicho cambio de título se refleja en el botón y en el cuadro de lista.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;::VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [plantilla de función winrt::Make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Temas relacionados
* [Consumo de API con C / c++ / WinRT](consume-apis.md)
* [Crear las API con C++ / c++ / WinRT](author-apis.md)
