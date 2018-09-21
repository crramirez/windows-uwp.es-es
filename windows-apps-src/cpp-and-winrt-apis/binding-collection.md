---
author: stevewhims
description: Una colección que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una colección *observable*. Este tema muestra cómo implementar y consumir una colección observable y cómo enlazar un control de elementos XAML a dicha colección.
title: Controles de elementos XAML; enlazar a una colección C++/WinRT
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, XAML, control, enlace, colección
ms.localizationpriority: medium
ms.openlocfilehash: 9ba935b1a5316c2d7af9c7681705595efea7ca08
ms.sourcegitcommit: a160b91a554f8352de963d9fa37f7df89f8a0e23
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/21/2018
ms.locfileid: "4122758"
---
# <a name="xaml-items-controls-bind-to-a-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-collection"></a>Controles de elementos XAML; enlazar a una colección [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Una colección que se puede enlazar de forma eficaz a un control de elementos XAML se conoce como una colección *observable*. Esta idea se basa en el modelo de diseño de software conocido como el *patrón observador*. Este tema muestra cómo implementar colecciones observables en C++/WinRT y cómo enlazar controles de elementos XAML a dichas colecciones.

Este tutorial se basa en el proyecto creado en [Controles XAML; enlazar a una propiedad C++/WinRT](binding-property.md) y se agrega a los conceptos que se explican en este tema.

> [!IMPORTANT]
> Para conocer los conceptos y términos esenciales que te ayuden a entender cómo consumir y crear clases en tiempo de ejecución con C++/WinRT, consulta [Consumir API con C++/WinRT](consume-apis.md) y [Crear API con C++/WinRT ](author-apis.md).

## <a name="what-does-observable-mean-for-a-collection"></a>¿Qué significa *observable* con respecto a una colección?
Si una clase en tiempo de ejecución que representa una colección elige generar el evento [**IObservableVector&lt;T&gt;:: VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged) cada vez que un elemento se agregue a ella o se quite de ella, la clase en tiempo de ejecución es una colección observable. El control de elementos XAML puede enlazarse a estos eventos y controlarlos mediante la recuperación de la colección actualizada y luego actualizarse automáticamente para mostrar los elementos actuales.

> [!NOTE]
> Para obtener información sobre la instalación y uso de la extensión de Visual Studio (VSIX) de C++/WinRT (la cual ofrece soporte para plantillas de proyectos, así como propiedades y destinos de MSBuild de C++/WinRT), consulta el [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix).

## <a name="implement-singlethreadedobservablevectorlttgt"></a>Implementar **single_threaded_observable_vector&lt;T&gt;**
Es aconsejable tener una plantilla de vector observable para que actúe como una implementación útil de uso general de [**IObservableVector&lt;T &gt;**](/uwp/api/windows.foundation.collections.iobservablevector_t_). A continuación se incluye una lista de una clase denominada **single_threaded_observable_vector\<T\>**.

> [!NOTE]
> Si has instalado el [Windows SDK versión preliminar 10 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)o una versión posterior, a continuación, puedes directamente usar la función de fábrica **single_threaded_observable_vector\ < T\ >** en lugar de la siguiente lista de códigos (te mostraremos el código exacto más adelante en este tema). Si aún no eres en esa versión del SDK, será fácil cambiar de usar la versión del listado de código a la función **winrt** cuando estás.

```cppwinrt
// single_threaded_observable_vector.h
#pragma once

namespace winrt::Bookstore::implementation
{
    using namespace Windows::Foundation::Collections;

    template <typename T>
    struct single_threaded_observable_vector : implements<single_threaded_observable_vector<T>,
        IObservableVector<T>,
        IVector<T>,
        IVectorView<T>,
        IIterable<T>>
    {
        event_token VectorChanged(VectorChangedEventHandler<T> const& handler)
        {
            return m_changed.add(handler);
        }

        void VectorChanged(event_token const cookie)
        {
            m_changed.remove(cookie);
        }

        T GetAt(uint32_t const index) const
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            return m_values[index];
        }

        uint32_t Size() const noexcept
        {
            return static_cast<uint32_t>(m_values.size());
        }

        IVectorView<T> GetView()
        {
            return *this;
        }

        bool IndexOf(T const& value, uint32_t& index) const noexcept
        {
            index = static_cast<uint32_t>(std::find(m_values.begin(), m_values.end(), value) - m_values.begin());
            return index < m_values.size();
        }

        void SetAt(uint32_t const index, T const& value)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values[index] = value;
            m_changed(*this, make<args>(CollectionChange::ItemChanged, index));
        }

        void InsertAt(uint32_t const index, T const& value)
        {
            if (index > m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.insert(m_values.begin() + index, value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, index));
        }

        void RemoveAt(uint32_t const index)
        {
            if (index >= m_values.size())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.erase(m_values.begin() + index);
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, index));
        }

        void Append(T const& value)
        {
            ++m_version;
            m_values.push_back(value);
            m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
        }

        void RemoveAtEnd()
        {
            if (m_values.empty())
            {
                throw hresult_out_of_bounds();
            }

            ++m_version;
            m_values.pop_back();
            m_changed(*this, make<args>(CollectionChange::ItemRemoved, Size()));
        }

        void Clear() noexcept
        {
            ++m_version;
            m_values.clear();
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        uint32_t GetMany(uint32_t const startIndex, array_view<T> values) const
        {
            if (startIndex >= m_values.size())
            {
                return 0;
            }

            uint32_t actual = static_cast<uint32_t>(m_values.size() - startIndex);

            if (actual > values.size())
            {
                actual = values.size();
            }

            std::copy_n(m_values.begin() + startIndex, actual, values.begin());
            return actual;
        }

        void ReplaceAll(array_view<T const> value)
        {
            ++m_version;
            m_values.assign(value.begin(), value.end());
            m_changed(*this, make<args>(CollectionChange::Reset, 0));
        }

        IIterator<T> First()
        {
            return make<iterator>(this);
        }

    private:

        std::vector<T> m_values;
        event<VectorChangedEventHandler<T>> m_changed;
        uint32_t m_version{};

        struct args : implements<args, IVectorChangedEventArgs>
        {
            args(CollectionChange const change, uint32_t const index) :
                m_change(change),
                m_index(index)
            {
            }

            CollectionChange CollectionChange() const
            {
                return m_change;
            }

            uint32_t Index() const
            {
                return m_index;
            }

        private:

            Windows::Foundation::Collections::CollectionChange const m_change{};
            uint32_t const m_index{};
        };

        struct iterator : implements<iterator, IIterator<T>>
        {
            explicit iterator(single_threaded_observable_vector<T>* owner) noexcept :
            m_version(owner->m_version),
                m_current(owner->m_values.begin()),
                m_end(owner->m_values.end())
            {
                m_owner.copy_from(owner);
            }

            void abi_enter() const
            {
                if (m_version != m_owner->m_version)
                {
                    throw hresult_changed_state();
                }
            }

            T Current() const
            {
                if (m_current == m_end)
                {
                    throw hresult_out_of_bounds();
                }

                return*m_current;
            }

            bool HasCurrent() const noexcept
            {
                return m_current != m_end;
            }

            bool MoveNext() noexcept
            {
                if (m_current != m_end)
                {
                    ++m_current;
                }

                return HasCurrent();
            }

            uint32_t GetMany(array_view<T> values)
            {
                uint32_t actual = static_cast<uint32_t>(std::distance(m_current, m_end));

                if (actual > values.size())
                {
                    actual = values.size();
                }

                std::copy_n(m_current, actual, values.begin());
                std::advance(m_current, actual);
                return actual;
            }

        private:

            com_ptr<single_threaded_observable_vector<T>> m_owner;
            uint32_t const m_version;
            typename std::vector<T>::const_iterator m_current;
            typename std::vector<T>::const_iterator const m_end;
        };
    };
}
```

La función **Append** muestra cómo generar el evento [**IObservableVector&lt;T&gt;::VectorChanged**](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged).

```cppwinrt
m_changed(*this, make<args>(CollectionChange::ItemInserted, Size() - 1));
```

Los argumentos del evento indican que se insertó un elemento y también cuál es su índice (el último elemento, en este caso). Estos argumentos permiten que un control de elementos XAML responda al evento y se actualice de forma óptima.

## <a name="add-a-bookskus-collection-to-bookstoreviewmodel"></a>Agregar una colección **BookSkus** a **BookstoreViewModel**
En [Controles XAML; enlaza a una propiedad C++/WinRT](binding-property.md), hemos agregado una propiedad de tipo **BookSku** a nuestro modelo de vista principal. En este paso, utilizaremos **single_threaded_observable_vector&lt;T&gt;** que nos ayudará a implementar una colección observable de **BookSku** en el mismo modelo de vista.

Declara una nueva propiedad en `BookstoreViewModel.idl`.

```idl
// BookstoreViewModel.idl
...
runtimeclass BookstoreViewModel
{
    BookSku BookSku{ get; };
    Windows.Foundation.Collections.IVector<IInspectable> BookSkus{ get; };
}
...
```

> [!IMPORTANT]
> En la descripción de MIDL 3.0 anterior, ten en cuenta que el tipo de la propiedad **BookSkus** es [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) de [**IInspectable**](https://msdn.microsoft.com/library/windows/desktop/br205821). En la siguiente sección de este tema, tendrás que enlaza el origen de los elementos de un [**ListBox**](/uwp/api/windows.ui.xaml.controls.listbox) a **BookSkus**. Un cuadro de lista es un control de elementos, y para configurar correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , debes establecer en un valor de tipo **IVector** de **IInspectable**, o de un tipo de interoperabilidad como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector).

Guarda y compila. Copia los códigos auxiliares de descriptor desde `BookstoreViewModel.h` y `BookstoreViewModel.cpp` en la carpeta `Generated Files` e impleméntalos.

```cppwinrt
// BookstoreViewModel.h
...
#include "single_threaded_observable_vector.h"
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel();

    Bookstore::BookSku BookSku();

    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookSkus();

private:
    Bookstore::BookSku m_bookSku{ nullptr };
    Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

```cppwinrt
// BookstoreViewModel.cpp
...
BookstoreViewModel::BookstoreViewModel()
{
    m_bookSku = make<Bookstore::implementation::BookSku>(L"Atticus");
    m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
    m_bookSkus.Append(m_bookSku);
}

Bookstore::BookSku BookstoreViewModel::BookSku()
{
    return m_bookSku;
}

Windows::Foundation::Collections::IVector<Windows::Foundation::IInspectable> BookstoreViewModel::BookSkus()
{
    return m_bookSkus;
}
...
```

## <a name="if-you-have-a-windows-10-sdk-preview-build"></a>Si tienes un Windows 10 SDK Preview Build.
Si has instalado el [Windows SDK versión preliminar 10 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK), o una versión posterior, a continuación, reemplaza esta línea de código

```cppwinrt
m_bookSkus = winrt::make<single_threaded_observable_vector<Windows::Foundation::IInspectable>>();
```

con este.

```cppwinrt
m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
```

En lugar de llamar a [**winrt:: Make**](https://docs.microsoft.com/en-us/uwp/cpp-ref-for-winrt/make), crear el objeto de colección adecuada mediante una llamada a la función de fábrica **single_threaded_observable_vector\ < T\ >** .

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
    MainViewModel().BookSkus().Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
}
...
```

Ahora compila y ejecuta el proyecto. Haz clic en el botón para ejecutar el controlador de eventos **Clic**. Observamos que la implementación de **Append** genera un evento para informar a la interfaz de usuario de que ha cambiado la colección; y el **ListBox** vuelve a consultar la colección para actualizar sus propios valores de **Items**. Igual que antes, el título de uno de los libros cambia. Dicho cambio de título se refleja en el botón y en el cuadro de lista.

## <a name="important-apis"></a>API importantes
* [IObservableVector&lt;T&gt;:: VectorChanged](/uwp/api/windows.foundation.collections.iobservablevector-1.vectorchanged)
* [Plantilla de función winrt::make](/uwp/cpp-ref-for-winrt/make)

## <a name="related-topics"></a>Artículos relacionados
* [Consumir API con C++/WinRT](consume-apis.md)
* [Crear API con C++/WinRT](author-apis.md)
