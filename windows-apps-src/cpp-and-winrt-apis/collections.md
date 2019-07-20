---
description: C++/WinRT proporciona funciones y clases base que te ahorrarán mucho tiempo y esfuerzo cuando quieras implementar o pasar colecciones.
title: Colecciones con C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, proyección, colección
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 4f1b15ec377b030a467dded634abe3fdde717896
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270149"
---
# <a name="collections-with-cwinrt"></a>Colecciones con C++/WinRT

Internamente, una colección de Windows Runtime tiene muchas partes móviles complicadas. Pero cuando quieres pasar un objeto de colección a una función de Windows Runtime o implementar tus propias propiedades de la colección y los tipos de colección, existen funciones y clases base en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para ayudarte. Estas características te quitan la complejidad de las manos y te ahorran una gran sobrecarga en tiempo y esfuerzo.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) es la interfaz de Windows Runtime que implementa cualquier colección de elementos de acceso aleatorio. Si tuvieras que implementar **IVector** tú mismo, también tendrías que implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_) e [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Aunque *necesites* un tipo de colección personalizada, eso es mucho trabajo. Pero si tienes datos en un **std::vector** (o un **std::map**, o un **std::unordered_map**) y lo único que quieres hacer es pasarlos a una API de Windows Runtime, preferirás evitar ese nivel de trabajo, si es posible. Y evitarlo *es* posible, porque C++/WinRT le ayuda a crear colecciones de forma eficaz y con poco esfuerzo.

Consulta también [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md)

## <a name="helper-functions-for-collections"></a>Funciones auxiliares en colecciones

### <a name="general-purpose-collection-empty"></a>Colección de uso general, vacía

En esta sección se describe el escenario en el que se quiere crear una colección que inicialmente está vacía; y rellenarla *después* de crearla.

Para recuperar un nuevo objeto de un tipo que implementa una colección de uso general, puedes llamar a la plantilla de función [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector). El objeto se devuelve como [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_), y esa es la interfaz a través de la que se llama a las funciones y propiedades del objeto devuelto.

Si quieres copiar y pegar los ejemplos de código siguientes directamente en el archivo de código fuente principal de un proyecto de **aplicación de consola de Windows (C++/WinRT)** , establece primero **No utilizar encabezados precompilados** en las propiedades del proyecto.

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;

int main()
{
    winrt::init_apartment();

    Windows::Foundation::Collections::IVector<int> coll{ winrt::single_threaded_vector<int>() };
    coll.Append(1);
    coll.Append(2);
    coll.Append(3);

    for (auto const& el : coll)
    {
        std::cout << el << std::endl;
    }

    Windows::Foundation::Collections::IVectorView<int> view{ coll.GetView() };
}
```

Como puedes ver en el ejemplo de código anterior, después de crear la colección puedes anexar elementos, iterar en ellos y, en general, tratar el objeto como lo harías con cualquier objeto de colección de Windows Runtime que hayas recibido de una API. Si necesitas una vista inmutable de la colección, puedes llamar a [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), tal y como se muestra. El patrón que se muestra antes (de creación y consumo de una colección) es adecuado para escenarios sencillos en los que quieres pasar datos a una API u obtener datos de ella. Puedes pasar un **IVector** o un **IVectorView**, allí donde se espere un [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_).

En el ejemplo de código anterior, la llamada a **winrt::init_apartment** inicializa el subproceso en Windows Runtime; de forma predeterminada, en un apartamento multiproceso. La llamada también inicializa COM.

### <a name="general-purpose-collection-primed-from-data"></a>Colección de uso general, preparada a partir de datos

En esta sección se describe el escenario en el que se quiere crear una colección y rellenarla al mismo tiempo.

Puedes evitar la sobrecarga de las llamadas a **Append** en el ejemplo de código anterior. Puede que ya tengas los datos de origen o quizás prefieras rellenar los datos de origen antes de crear el objeto de colección de Windows Runtime. Aquí te mostramos cómo hacerlo.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Puedes pasar un objeto temporal que contenga los datos a **winrt::single_threaded_vector**, igual que con `coll1`, anteriormente. O puedes mover un **std::vector** (suponiendo que no vuelvas a acceder a él) a la función. En ambos casos, se pasa un *rvalue* a la función. Esto permite que el compilador sea eficiente y no haya que copiar los datos. Si quieres saber más sobre *rvalues*, consulta [Categorías de valor y referencias a ellas](cpp-value-categories.md).

Si quieres enlazar un control de elementos XAML a la colección, puedes hacerlo. Pero ten en cuenta que para establecer correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource), has de establecerla en un valor de tipo **IVector** de **IInspectable** (o de un tipo de interoperabilidad tal como [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)).

Este es un ejemplo de código que genera una colección de un tipo adecuado para el enlace y le anexa un elemento. Puedes encontrar el contexto para este ejemplo de código en [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md).

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Puedes crear una colección de Windows Runtime a partir de datos y obtener una vista de ellos lista para pasarla a una API, todo ello sin tener que copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
Windows::Foundation::Collections::IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

En los ejemplos anteriores, la colección que se crea *puede* enlazarse a un control de elementos XAML; pero la colección no es observable.

### <a name="observable-collection"></a>Colección observable

Para recuperar un nuevo objeto de un tipo que implementa una colección *observable*, puedes llamar a la plantilla de función [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) con cualquier tipo de elemento. Pero para hacer una colección observable adecuada para unirse a un control de elementos XAML, utiliza **IInspectable** como tipo de elemento.

El objeto se devuelve como [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), y esa es la interfaz a través de la que el usuario (o el control al que está enlazado) llama a las funciones y propiedades del objeto devuelto.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obtener más detalles, y ejemplos de código, sobre cómo enlazar los controles de la interfaz de usuario (UI) con una colección observable, consulta [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md).

### <a name="associative-collection-map"></a>Colección asociativa (mapa)

Hay versiones de colección asociativa de las dos funciones que hemos analizado.

- La plantilla de función [**winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) devuelve una colección asociativa no observable como [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- La plantilla de función [**winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) devuelve una colección asociativa observable como [ **IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

También puedes preparar estas colecciones con datos pasando a la función un *rvalue*de tipo **std::map** o **std::unordered_map**

```cppwinrt
auto coll1{
    winrt::single_threaded_map<winrt::hstring, int>(std::map<winrt::hstring, int>{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    })
};

std::map<winrt::hstring, int> values{
    { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
};
auto coll2{ winrt::single_threaded_map<winrt::hstring, int>(std::move(values)) };
```

### <a name="single-threaded"></a>Uniproceso

El término "uniproceso" en los nombres de estas funciones indica que no ofrecen ninguna simultaneidad; en otras palabras, no son seguras para subprocesos. La mención de subprocesos no está relacionada con los apartamentos, porque los objetos devueltos desde estas funciones son todos ágiles (ver [Objetos ágiles en C++/WinRT](agile-objects.md)). Solo se trata de que los objetos son uniproceso. Y eso es completamente adecuado si tan solo quieres pasar datos de una manera u otra a través de la interfaz binaria de aplicaciones (ABI).

## <a name="base-classes-for-collections"></a>Clases base en colecciones

Si, para una completa flexibilidad, quieres implementar tu propia colección personalizada, querrás evitar hacerlo de la manera más difícil. Por ejemplo, este es el aspecto que tendría una vista vectorial personalizada *sin ayuda de las clases base de C++/WinRT*.

```cppwinrt
...
using namespace winrt;
using namespace Windows::Foundation::Collections;
...
struct MyVectorView :
    implements<MyVectorView, IVectorView<float>, IIterable<float>>
{
    // IVectorView
    float GetAt(uint32_t const) { ... };
    uint32_t GetMany(uint32_t, winrt::array_view<float>) const { ... };
    bool IndexOf(float, uint32_t&) { ... };
    uint32_t Size() { ... };

    // IIterable
    IIterator<float> First() const { ... };
};
...
IVectorView<float> view{ winrt::make<MyVectorView>() };
```

En su lugar, resulta mucho más fácil derivar la vista vectorial personalizada desde la plantilla de estructura [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) y simplemente implementar la función **get_container** para exponer el contenedor que contiene los datos.

```cppwinrt
struct MyVectorView2 :
    implements<MyVectorView2, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView2, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

El contenedor devuelto por **get_container** debe proporcionar la interfaz **begin** y **end** que **winrt::vector_view_base** espera. Como se muestra en el ejemplo anterior, **std::vector** la proporciona. Pero puedes devolver cualquier contenedor que cumpla el mismo contrato, incluido tu propio contenedor personalizado.

```cppwinrt
struct MyVectorView3 :
    implements<MyVectorView3, IVectorView<float>, IIterable<float>>,
    winrt::vector_view_base<MyVectorView3, float>
{
    auto get_container() const noexcept
    {
        struct container
        {
            float const* const first;
            float const* const last;

            auto begin() const noexcept
            {
                return first;
            }

            auto end() const noexcept
            {
                return last;
            }
        };

        return container{ m_values.data(), m_values.data() + m_values.size() };
    }

private:
    std::array<float, 3> m_values{ 0.2f, 0.3f, 0.4f };
};
```

Estas son las clases base que C++/WinRT ofrece para ayudarte a implementar colecciones personalizadas.

### <a name="winrtvectorviewbaseuwpcpp-ref-for-winrtvector-view-base"></a>[winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Vea los ejemplos de código anteriores.

### <a name="winrtvectorbaseuwpcpp-ref-for-winrtvector-base"></a>[winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

```cppwinrt
struct MyVector :
    implements<MyVector, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::vector_base<MyVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtobservablevectorbaseuwpcpp-ref-for-winrtobservable-vector-base"></a>[winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

```cppwinrt
struct MyObservableVector :
    implements<MyObservableVector, IObservableVector<float>, IVector<float>, IVectorView<float>, IIterable<float>>,
    winrt::observable_vector_base<MyObservableVector, float>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::vector<float> m_values{ 0.1f, 0.2f, 0.3f };
};
```

### <a name="winrtmapviewbaseuwpcpp-ref-for-winrtmap-view-base"></a>[winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

```cppwinrt
struct MyMapView :
    implements<MyMapView, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_view_base<MyMapView, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtmapbaseuwpcpp-ref-for-winrtmap-base"></a>[winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

```cppwinrt
struct MyMap :
    implements<MyMap, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::map_base<MyMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

### <a name="winrtobservablemapbaseuwpcpp-ref-for-winrtobservable-map-base"></a>[winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

```cppwinrt
struct MyObservableMap :
    implements<MyObservableMap, IObservableMap<winrt::hstring, int>, IMap<winrt::hstring, int>, IMapView<winrt::hstring, int>, IIterable<IKeyValuePair<winrt::hstring, int>>>,
    winrt::observable_map_base<MyObservableMap, winrt::hstring, int>
{
    auto& get_container() const noexcept
    {
        return m_values;
    }

    auto& get_container() noexcept
    {
        return m_values;
    }

private:
    std::map<winrt::hstring, int> m_values{
        { L"AliceBlue", 0xfff0f8ff }, { L"AntiqueWhite", 0xfffaebd7 }
    };
};
```

## <a name="important-apis"></a>API importantes
* [Propiedad ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interfaz IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interfaz IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [Plantilla de estructura winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [Plantilla de estructura winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [Plantilla de estructura winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [Plantilla de estructura winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [Plantilla de función winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [Plantilla de función winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [Plantilla de función winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Plantilla de función winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [Plantilla de estructura winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [Plantilla de estructura winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Temas relacionados
* [Categorías de valor y referencias a ellas](cpp-value-categories.md)
* [Controles de elementos de XAML; enlazar a una colección C++/WinRT](binding-collection.md)
