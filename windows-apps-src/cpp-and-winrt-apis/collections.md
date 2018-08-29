---
author: stevewhims
description: C++ / WinRT proporciona funciones y clases base que guardar una gran cantidad de tiempo y esfuerzo cuando quieras implementar o pasar colecciones.
title: Las colecciones con C++ / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, colección
ms.localizationpriority: medium
ms.openlocfilehash: dacfe4135402b85bac68b63c06f99f97001fa5b9
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2907709"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Las colecciones con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Internamente, una colección de Windows Runtime tiene una gran cantidad de partes complicadas. Sin embargo, cuando quieras pasar un objeto de la colección a una función de Windows Runtime, o para implementar tus propias propiedades de colección y tipos de colección, hay funciones y otras clases base en C++ / WinRT para ayudarte. Estas características tomar la complejidad de las manos y guardar una gran cantidad de sobrecarga de tiempo y esfuerzo.

> [!IMPORTANT]
> Las características descritas en este tema están disponibles si has instalado el [Windows SDK versión preliminar 10 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)o posterior.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) es la interfaz de Windows en tiempo de ejecución implementada por cualquier colección de acceso aleatorio de elementos. Si fueras a implementarla **IVector** , también sería necesario implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)y [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Incluso si *necesitas* una colección personalizada de tipos, es una gran cantidad de trabajo. Pero si tiene datos en un **std:: vector** (o un **std:: Map**o un **std::unordered_map**) y todo lo que quieras hacer es pasar a una API de Windows Runtime, a continuación, ¿quieres evitar realizar ese nivel de trabajo, si es posible. Y lo que evita *es* posible, dado que C++ / WinRT te ayuda a crear recopilaciones de manera eficaz y con muy poco esfuerzo.

## <a name="helper-functions-for-collections"></a>Funciones auxiliares de las colecciones

### <a name="general-purpose-collection-empty"></a>Colección de propósito general, vacía

Para recuperar un nuevo objeto de un tipo que implementa una colección de propósito general, puedes llamar a la plantilla de función [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . El objeto se devuelve como una [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)y que es la interfaz a través de la que se llama a las funciones y las propiedades del objeto devuelto.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
int main()
{
    init_apartment();

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

Como puedes ver en el ejemplo de código anterior, después de crear la colección puedes anexar elementos, iterar sobre ellos y generalmente tratan el objeto como lo harías con cualquier objeto de colección de Windows Runtime que es posible que has recibido de una API. Si necesitas una vista inmutable a través de la colección, a continuación, puedes llamar a [IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview), tal como se muestra. El patrón mostrado anteriormente&mdash;de crear y consumir una colección&mdash;es apropiado para escenarios sencillos donde quieres pasar datos a u obtener datos de una API.

### <a name="general-purpose-collection-primed-from-data"></a>Colección de propósito general, preparada de datos

También puede evitar la sobrecarga de las llamadas a **Append** que pueden aparecer en el ejemplo de código anterior. Es posible que tenga los datos de origen o es preferible rellenar antes de crear el objeto de colección de Windows Runtime. Aquí te mostramos cómo hacerlo.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Puedes pasar un objeto temporal que contiene los datos a **winrt::single_threaded_vector**, igual que con `coll1`, más arriba. O puedes mover un **std:: vector** (suponiendo que no se puede obtener acceso a él otra vez) en la función. En ambos casos, que estás pasando un *valor r* en la función. Esto permite al compilador que sea eficaz y para evitar copiar los datos. Si quieres obtener más información acerca de los *valores r*, consulta [las categorías de valor y las referencias a ellos](cpp-value-categories.md).

Si quieres enlazar un control de elementos XAML a la colección, a continuación, puedes hacerlo. Pero ten en cuenta que para configurar correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , debes establecer en un valor de tipo **IVector** de **IInspectable** (o de un tipo de interoperabilidad como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Este es un ejemplo de código que genera una colección de un tipo apropiado para el enlace y se agrega un elemento a ella.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

La colección anterior *puede* enlazarse a un control de elementos XAML; pero no de la colección observable.

### <a name="observable-collection"></a>Colección observable

Para recuperar un nuevo objeto de un tipo que implementa una colección *observable* , llama a la plantilla de función [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) con cualquier tipo de elemento. Pero para permitir una colección observable adecuado para enlazarlo a un control de elementos XAML, usa **IInspectable** como el tipo de elemento.

El objeto se devuelve como una [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)y que es la interfaz a través de la que tú (o el control al que está enlazado) llamar a las funciones y las propiedades del objeto devuelto.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obtener más información y ejemplos de código, sobre el enlace de usuario (UI) de la interfaz controles a una colección observable, consulta [controles de elementos XAML; enlazar a C++ / WinRT colección](binding-collection.md).

### <a name="associative-collection-map"></a>Colección asociativo (asignar)

Hay versiones de colección asociativos de las dos funciones que hemos visto.

- La plantilla de función [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) devuelve una colección asociativa de no observables como un [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- La plantilla de función [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) devuelve una colección observable asociativa como un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente, puede desbloquear estas colecciones con datos mediante la aprobación a la función de un *valor r* de tipo **std:: Map** o **std::unordered_map**.

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

### <a name="single-threaded"></a>Un solo subproceso

La "uniproceso" en los nombres de estas funciones indica que no proporcionan cualquier simultaneidad&mdash;en otras palabras, no son seguras para subprocesos. La mención de subprocesos no está relacionada con apartamentos, porque son ágiles de todos los objetos devueltos por estas funciones (consulta [objetos ágiles en C++ / WinRT](agile-objects.md)). Solo es que los objetos tienen un solo subproceso. Y eso es todo apropiado si quieres pasar datos de una forma u otra a través de la interfaz binaria de aplicaciones (ABI).

## <a name="base-classes-for-collections"></a>Clases base para las colecciones

Si, para una completa flexibilidad, que quieras implementar su propia colección personalizada, querrás evitar haciendo el disco duro. Por ejemplo, esto es cómo aparecería una vista personalizada de vector *sin la intervención de C++ / clases base de WinRT*.

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

En su lugar, es mucho más fácil derivar de la vista de vector personalizado de la plantilla de estructura [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) y simplemente implementar la función **get_container** para exponer el contenedor contiene los datos.

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

El contenedor devuelto por **get_container** debe proporcionar la interfaz **begin** y **end** ese **winrt::vector_view_base** espera. Como se muestra en el ejemplo anterior, **std:: vector** proporciona. Pero puedes devolver cualquier contenedor que cumpla el mismo contrato, incluido su propio contenedor personalizado.

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

Estos son la base de las clases que C++ / WinRT proporciona para ayudarte a implementar colecciones personalizadas.

### [<a name="winrtvectorviewbase"></a>winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

Vea los ejemplos de código anteriores.

### [<a name="winrtvectorbase"></a>winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)

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

### [<a name="winrtobservablevectorbase"></a>winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)

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

### [<a name="winrtmapviewbase"></a>winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)

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

### [<a name="winrtmapbase"></a>winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)

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

### [<a name="winrtobservablemapbase"></a>winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)

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
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Temas relacionados
* [Las categorías de valor y referencias a ellos](cpp-value-categories.md)
* [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md)