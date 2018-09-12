---
author: stevewhims
description: C + + / WinRT proporciona funciones y clases base que se ahorrará mucho tiempo y esfuerzo cuando desee implementar o pasar colecciones.
title: Colecciones con C + + / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10 uwp, estándar, c ++, cpp, winrt, proyección, colección
ms.localizationpriority: medium
ms.openlocfilehash: 1ef6fbfab45197c868296186363c168a6c443247
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930791"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Colecciones con [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Internamente, una colección en tiempo de ejecución de Windows tiene muchas partes móviles complicadas. Pero si desea pasar un objeto de colección para una función de tiempo de ejecución de Windows, o para implementar sus propias propiedades de colección y los tipos de colección, funciones y clases base en C + + / WinRT proporcionar soporte técnico. Estas características la complejidad a las manos y guardar una gran sobrecarga en tiempo y esfuerzo.

> [!IMPORTANT]
> Las características descritas en este tema están disponibles si ha instalado el [SDK de vista previa generar 17661 de 10 de Windows](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)o posterior.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) es la interfaz de tiempo de ejecución de Windows implementada por cualquier colección de acceso aleatorio de elementos. Si tuviera que implementar **IVector** personalmente, necesitará implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)y [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Incluso si *necesita* una colección personalizada de tipo, es mucho trabajo. Pero si tiene datos en un **std:: vector** (un **std:: Map**o un **std::unordered_map**) y todo lo que desea hacer es pasarla a una API de tiempo de ejecución de Windows, a continuación, ¿desea evitar realizar ese nivel de trabajo, si es posible. Y evitar *es* posible, porque C + + / WinRT le ayuda a crear colecciones de forma eficaz y con poco esfuerzo.

## <a name="helper-functions-for-collections"></a>Funciones auxiliares para las colecciones

### <a name="general-purpose-collection-empty"></a>Colección de propósito general, vacía

Para recuperar un nuevo objeto de un tipo que implementa una colección general, puede llamar a la plantilla de función [**winrt::single_threaded_vector**](/uwp/cpp-ref-for-winrt/single-threaded-vector) . El objeto se devuelve como un [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)y que es la interfaz a través de la cual se llama a las funciones y las propiedades del objeto devuelto.

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

Como puede ver en el ejemplo de código anterior, después de crear la colección puede anexar elementos, recorrerlas en iteración y generalmente tratan el objeto como haría con cualquier objeto de la colección en tiempo de ejecución de Windows que ha recibido de una API. Si necesita una vista inmutable sobre la colección, a continuación, puede llamar a [**IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), como se muestra. El diseño descrito anteriormente&mdash;de creación y consumo de una colección&mdash;es adecuado para escenarios sencillos donde desea pasar datos a u obtener datos de una API. Se puede pasar un **IVector**o un **IVectorView**, en cualquier lugar en que se espera un [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_) .

### <a name="general-purpose-collection-primed-from-data"></a>Colección general, preparada a partir de los datos

También puede evitar la sobrecarga de las llamadas de **datos anexados** que puede ver en el ejemplo de código anterior. Es posible que tenga los datos de origen, o prefiere rellenarlo antes de crear el objeto de la colección en tiempo de ejecución de Windows. Explicamos cómo hacerlo.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Puede pasar un objeto temporal que contiene los datos a **winrt::single_threaded_vector**, como ocurre con `coll1`, más arriba. O se puede mover un **std:: vector** (suponiendo que no pueden tener acceso a él otra vez) en la función. En ambos casos, está pasando un *valor r* a la función. Que permite al compilador para ser eficaz y evitar copiar los datos. Si desea obtener más información acerca de los *valores r*, vea [categorías de valor y las referencias a ellos](cpp-value-categories.md).

Si desea enlazar un control de elementos XAML a la colección, puede. Pero tenga en cuenta que para establecer correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , debe establecerla en un valor de tipo **IVector** de **IInspectable** (o de un tipo de interoperabilidad como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Este es un ejemplo de código que produce una colección de un tipo adecuado para el enlace y agrega un elemento a ella.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Puede crear una colección en tiempo de ejecución de Windows a partir de datos y obtener una vista en él listo para pasar a una API, todo ello sin copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

En los ejemplos anteriores, la colección que creamos que *puede* estar enlazado a un control de elementos XAML; pero la colección no es perceptible.

### <a name="observable-collection"></a>Colección visible

Para recuperar un nuevo objeto de un tipo que implementa una colección *observables* , llame a la plantilla de función [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) con cualquier tipo de elemento. Pero para hacer una colección observable adecuado para enlazarlo a un control de elementos XAML, utilice **IInspectable** como el tipo de elemento.

El objeto se devuelve como un [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), y que es la interfaz a través de la que usted (o el control al que está enlazado) llame a las funciones y las propiedades del objeto devuelto.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obtener más detalles y ejemplos de código, enlace el usuario interfaz de controles a una colección observable, consulte [elementos de XAML controla; enlazar a C + + / WinRT colección](binding-collection.md).

### <a name="associative-collection-map"></a>Conjunto asociativo (map)

Existen versiones de colección asociativa de las dos funciones que hemos visto.

- La plantilla de la función [**winrt::single_threaded_map**](/uwp/cpp-ref-for-winrt/single-threaded-map) devuelve un conjunto asociativo no observables como un [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- La plantilla de la función [**winrt::single_threaded_observable_map**](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) devuelve una colección asociativa observable como un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente puede desbloquear estas colecciones con datos pasando a la función *rvalue* de tipo **std:: Map** o **std::unordered_map**.

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

### <a name="single-threaded"></a>Un único subproceso

El "subproceso único" en los nombres de estas funciones indica que no proporcionan ninguna simultaneidad&mdash;en otras palabras, no son seguros para subprocesos. La mención de los subprocesos está relacionado con apartamentos, porque los objetos devueltos por estas funciones son todo agiles (consulte [objetos ágiles en C + + / WinRT](agile-objects.md)). Simplemente es que los objetos tienen un solo subproceso. Y eso es todo apropiado si desea pasar datos de una manera u otra a través de la interfaz de aplicación binaria (ABI).

## <a name="base-classes-for-collections"></a>Clases base para las colecciones

Si, para una completa flexibilidad, desea implementar su propia colección personalizada, deseará evitar el camino difícil. Por ejemplo, esto es cuál sería el aspecto de una vista personalizada del vector *sin la Ayuda de C + + / clases base de WinRT*.

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

En su lugar, es mucho más fácil derivar su vista personalizada del vector de la plantilla de la estructura [**winrt::vector_view_base**](/uwp/cpp-ref-for-winrt/vector-view-base) y sólo necesita implementar la función **get_container** para exponer el contenedor que contiene los datos.

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

El contenedor devuelto por **get_container** debe proporcionar la interfaz **begin** y **end** que **winrt::vector_view_base** espera. Como se muestra en el ejemplo anterior, **std:: vector** proporciona. Pero puede devolver cualquier contenedor que cumpla el mismo contrato, incluido su propio contenedor personalizado.

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

Se trata de la base de las clases que C + + / WinRT se proporciona para ayudarle a implementar colecciones personalizadas.

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
* [Propiedad ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [Interfaz IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [Interfaz IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [plantilla de estructura winrt::map_base](/uwp/cpp-ref-for-winrt/map-base)
* [plantilla de estructura winrt::map_view_base](/uwp/cpp-ref-for-winrt/map-view-base)
* [plantilla de estructura winrt::observable_map_base](/uwp/cpp-ref-for-winrt/observable-map-base)
* [plantilla de estructura winrt::observable_vector_base](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [plantilla de función winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [plantilla de función winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [plantilla de función winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [plantilla de función winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [plantilla de estructura winrt::vector_base](/uwp/cpp-ref-for-winrt/vector-base)
* [plantilla de estructura winrt::vector_view_base](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Temas relacionados
* [Categorías de valor y las referencias a ellos](cpp-value-categories.md)
* [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md)
