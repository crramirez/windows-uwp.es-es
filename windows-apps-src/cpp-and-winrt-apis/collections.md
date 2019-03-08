---
description: C++ / c++ / WinRT proporciona funciones y clases base que le ahorrarán mucho tiempo y esfuerzo cuando desea implementar o pasar colecciones.
title: Colecciones con C++ / WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, colección
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a50ab5f70faa0c8f8b73eada38444bcafd444d8b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635440"
---
# <a name="collections-with-cwinrt"></a>Colecciones con C++ / WinRT

Internamente, una colección en tiempo de ejecución de Windows tiene muchas partes móviles complicadas. Pero cuando desee pasar un objeto de colección a una función en tiempo de ejecución de Windows, o para implementar sus propias propiedades de la colección y los tipos de colección, existen funciones y clases base en [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) brindarle soporte técnico. Estas características tomar la complejidad de las manos y guardar una gran sobrecarga en tiempo y esfuerzo.

[**IVector** ](/uwp/api/windows.foundation.collections.ivector_t_) es la interfaz de Windows en tiempo de ejecución implementada por cualquier colección de acceso aleatorio de elementos. Si tuviera que implementar **IVector** usted mismo, también deberá implementar [ **IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [ **IVectorView** ](/uwp/api/windows.foundation.collections.ivectorview_t_), y [ **IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Incluso si se *necesita* un tipo de colección personalizada, que es una gran cantidad de trabajo. Sin embargo, si tiene datos en un **std:: vector** (o un **std:: Map**, o un **std:: unordered_map**) y todo lo que desea hacer es que pasar a la API de Windows en tiempo de ejecución y, después, tendrá que evitar hacer ese nivel de trabajo, si es posible. Y evitar *es* posible, dado que C++ / c++ / WinRT le ayuda a crear colecciones de forma eficaz y con poco esfuerzo.

Consulte también [controles de elementos de XAML; enlazar a C++ / c++ / WinRT colección](binding-collection.md).

> [!NOTE]
> Si no ha instalado el SDK de Windows versión 10.0.17763.0 (Windows 10, versión 1809) o versiones posteriores, a continuación, no tendrá acceso a las funciones y clases base que se documentan en este tema. En su lugar, consulte [si tiene una versión anterior del SDK de Windows](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector#if-you-have-an-older-version-of-the-windows-sdk) para obtener una lista de una plantilla de vector observable que puede usar en su lugar.

## <a name="helper-functions-for-collections"></a>Funciones auxiliares para las colecciones

### <a name="general-purpose-collection-empty"></a>Colección de uso general, vacía

Esta sección trata el escenario donde desea crear una colección que inicialmente está vacía; y, a continuación, rellenarla *después* creación.

Para recuperar un nuevo objeto de un tipo que implementa una colección de uso general, puede llamar a la [ **winrt::single_threaded_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-vector) plantilla de función. El objeto se devuelve como un [ **IVector**](/uwp/api/windows.foundation.collections.ivector_t_), y que es la interfaz a través de la que se llama a funciones y propiedades del objeto devuelto.

```cppwinrt
...
#include <winrt/Windows.Foundation.Collections.h>
#include <iostream>
using namespace winrt;
...
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

Como puede ver en el ejemplo de código anterior, después de crear la colección puede anexar elementos, una iteración con ellas y generalmente tratan el objeto como lo haría con cualquier objeto de colección en tiempo de ejecución de Windows que haya recibido desde una API. Si necesita una vista inmutable a través de la colección, entonces puede llamar a [ **IVector::GetView**](/uwp/api/windows.foundation.collections.ivector-1.getview), como se muestra. El patrón que se muestra arriba&mdash;de creación y consumo de una colección&mdash;es adecuado para escenarios sencillos donde desea pasar datos u obtener datos fuera de una API. Puede pasar un **IVector**, o un **IVectorView**y en cualquier lugar un [ **IIterable** ](/uwp/api/windows.foundation.collections.iiterable_t_) se espera.

En el ejemplo de código anterior, la llamada a **winrt::init_apartment** inicializa COM; de forma predeterminada, en un apartamento multiproceso.

### <a name="general-purpose-collection-primed-from-data"></a>Colección de uso general, preparada a partir de datos

Esta sección trata el escenario donde desea crear una colección y rellenarla al mismo tiempo.

Puede evitar la sobrecarga de las llamadas a **Append** en el ejemplo de código anterior. Es posible que ya tiene los datos de origen, o quizás prefiera rellenar los datos de origen antes de crear el objeto de colección en tiempo de ejecución de Windows. Aquí le mostramos cómo hacerlo.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Puede pasar un objeto temporal que contiene los datos a **winrt::single_threaded_vector**, igual que con `coll1`, más arriba. O puede mover un **std:: vector** (suponiendo que no se puede tener acceso a él nuevo) en la función. En ambos casos, pasamos un *rvalue* a la función. Que permite que el compilador sea eficaz y para evitar copiar los datos. Si desea saber más sobre *rvalues*, consulte [categorías de valor y las referencias a ellos](cpp-value-categories.md).

Si desea enlazar un control de elementos XAML a la colección, a continuación, puede. Pero tenga en cuenta que establecer correctamente la [ **ItemsControl.ItemsSource** ](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) propiedad, deberá establecerlo en un valor de tipo **IVector** de **IInspectable** (o de un tipo de interoperabilidad como [ **IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Este es un ejemplo de código que genera una colección de un tipo adecuado para el enlace y le anexa un elemento.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

Puede crear una colección en tiempo de ejecución de Windows a partir de datos y obtener una vista en ella listo para pasar a una API, todo ello sin necesidad de copiar nada.

```cppwinrt
std::vector<float> values{ 0.1f, 0.2f, 0.3f };
IVectorView<float> view{ winrt::single_threaded_vector(std::move(values)).GetView() };
```

En los ejemplos anteriores, la colección se creará *puede* enlazarse al control; de elementos de un XAML pero no de la colección observable.

### <a name="observable-collection"></a>Colección observable

Para recuperar un nuevo objeto de un tipo que implementa un *observable* colección, llame a la [ **winrt::single_threaded_observable_vector** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) plantilla de función con cualquier tipo de elemento. Pero para hacer una colección observable adecuado para enlazarlo a un control de elementos XAML, utilice **IInspectable** como el tipo de elemento.

El objeto se devuelve como un [ **IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), y que es la interfaz mediante el cual usted (o el control al que está enlazada) llame a funciones y propiedades del objeto devuelto.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obtener más detalles y ejemplos de código, sobre cómo enlazar el usuario (UI) controles de interfaz a una colección observable, consulte [controles de elementos de XAML; enlazar a C++ / c++ / WinRT colección](binding-collection.md).

### <a name="associative-collection-map"></a>Colección asociativa (map)

Hay versiones de una colección asociativa de las dos funciones que hemos analizado.

- El [ **winrt::single_threaded_map** ](/uwp/cpp-ref-for-winrt/single-threaded-map) plantilla de función devuelve una colección asociativa no observable como un [ **IMap**](/uwp/api/windows.foundation.collections.imap_k_v_).
- El [ **winrt::single_threaded_observable_map** ](/uwp/cpp-ref-for-winrt/single-threaded-observable-map) plantilla de función devuelve una colección asociativa observable como un [ **IObservableMap** ](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente, puede preparar estas colecciones con los datos al pasar a la función un *rvalue* typu **std:: Map** o **std:: unordered_map**.

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

El "uniproceso" en los nombres de estas funciones indica que no proporcionan ninguna simultaneidad&mdash;en otras palabras, no son seguros para subprocesos. La mención de subprocesos está relacionado con apartamentos, porque los objetos devueltos de estas funciones son ágiles todo (consulte [objetos ágiles en C++ / c++ / WinRT](agile-objects.md)). Simplemente es que los objetos tienen un solo subproceso. Y eso es completamente adecuado si desea pasar datos de una manera u otra a través de la interfaz binaria de aplicación (ABI).

## <a name="base-classes-for-collections"></a>Clases base para las colecciones

Si desea implementar su propia colección personalizada para una flexibilidad completa, deseará evitar hacer las malas. Por ejemplo, esto es lo que tendría el aspecto de una vista personalizada del vector *sin ayuda de C++ / c++ / clases base de WinRT*.

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

En su lugar, resulta mucho más fácil derivar la vista personalizada del vector desde el [ **winrt::vector_view_base** ](/uwp/cpp-ref-for-winrt/vector-view-base) struct de plantilla y sólo necesita implementar la **get_container** función exponer el contenedor que contiene los datos.

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

El contenedor devuelto por **get_container** debe proporcionar el **comenzar** y **final** interfaz que **winrt::vector_view_base** espera. Como se muestra en el ejemplo anterior, **std:: vector** que proporciona. Pero puede devolver cualquier contenedor que cumple el mismo contrato, incluido su propio contenedor personalizado.

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

Se trata de la base de las clases que C++ / c++ / WinRT proporciona para ayudarle a implementar colecciones personalizadas.

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
* [Interfaz de IVector](/uwp/api/windows.foundation.collections.ivector_t_)
* [winrt::map_base struct template](/uwp/cpp-ref-for-winrt/map-base)
* [winrt::map_view_base struct template](/uwp/cpp-ref-for-winrt/map-view-base)
* [winrt::observable_map_base struct template](/uwp/cpp-ref-for-winrt/observable-map-base)
* [winrt::observable_vector_base struct template](/uwp/cpp-ref-for-winrt/observable-vector-base)
* [plantilla de función winrt::single_threaded_observable_map](/uwp/cpp-ref-for-winrt/single-threaded-observable-map)
* [plantilla de función winrt::single_threaded_map](/uwp/cpp-ref-for-winrt/single-threaded-map)
* [plantilla de función winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [plantilla de función winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector)
* [winrt::vector_base struct template](/uwp/cpp-ref-for-winrt/vector-base)
* [winrt::vector_view_base struct template](/uwp/cpp-ref-for-winrt/vector-view-base)

## <a name="related-topics"></a>Temas relacionados
* [Categorías de valor y las referencias a ellos](cpp-value-categories.md)
* [Elementos de XAML de los controles; enlazar a C++ / c++ / WinRT colección](binding-collection.md)
