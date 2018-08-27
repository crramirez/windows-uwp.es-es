---
author: stevewhims
description: C + + / WinRT proporciona funciones y clases base que se guarde una gran cantidad de tiempo y esfuerzo cuando desee implementar o pasar colecciones.
title: Las colecciones con C + + / WinRT
ms.author: stwhi
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, proyección, colección
ms.localizationpriority: medium
ms.openlocfilehash: 54f949c41af885ec379eaa9e5b12764710532b50
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "2867953"
---
# <a name="collections-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>Las colecciones con [C + + / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)

> [!NOTE]
> **Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.**

Internamente, una colección de tiempo de ejecución de Windows tiene una gran cantidad de elementos móviles complicados. Pero cuando desea pasar un objeto de colección a una función de tiempo de ejecución de Windows, o para implementar sus propios tipos de colección y propiedades de la colección, hay funciones y clases base en C + + / WinRT proporcionar soporte técnico. Estas características tomar la complejidad de las manos y guardar una gran cantidad de sobrecarga en el tiempo y esfuerzo.

> [!IMPORTANT]
> Las características descritas en este tema están disponibles si ha instalado el [SDK de vista previa generar 17661 de 10 de Windows](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK)o posterior.

[**IVector**](/uwp/api/windows.foundation.collections.ivector_t_) es la interfaz de tiempo de ejecución de Windows implementada por cualquier colección de acceso aleatorio de elementos. Si desea implementar **IVector** usted mismo, también necesitará implementar [**IIterable**](/uwp/api/windows.foundation.collections.iiterable_t_), [**IVectorView**](/uwp/api/windows.foundation.collections.ivectorview_t_)y [**IIterator**](/uwp/api/windows.foundation.collections.iiterator_t_). Incluso si *necesita* una colección personalizada escriba, es una gran cantidad de trabajo. Pero si dispone de datos en un **std:: vector** (o un **std:: Map**o una **std::unordered_map**) y todo lo que desea hacer es pasar a una API de tiempo de ejecución de Windows, a continuación, ¿desea evitar realizar ese nivel de trabajo, si es posible. Y evitar *es* posible, porque C + + / WinRT le ayuda a crear colecciones de forma eficaz y con muy poco esfuerzo.

## <a name="helper-functions-for-collections"></a>Funciones auxiliares para las colecciones

### <a name="general-purpose-collection-empty"></a>Colección de propósito general, vacía

Para recuperar un nuevo objeto de un tipo que implementa una colección de propósito general, se puede llamar a la plantilla de función **winrt::single_threaded_vector** . El objeto se devuelve como una [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)y que es la interfaz a través de la que se llama a funciones y las propiedades del objeto devuelto.

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

Como puede ver en el ejemplo de código anterior, después de crear la colección puede anexar elementos, realice una iteración a través de ellos y generalmente tratan el objeto tal y como lo haría con cualquier objeto de colección de tiempo de ejecución de Windows que es posible que haya recibido desde una API. Si necesita una vista a través de la colección, a continuación, puede llamar a [IVector::GetView](/uwp/api/windows.foundation.collections.ivector-1.getview), tal como se muestra. El patrón que se muestra encima de&mdash;de creación y consumo de una colección de&mdash;es adecuado para escenarios sencillos donde desea pasar datos en u obtener datos fuera de una API.

### <a name="general-purpose-collection-primed-from-data"></a>Colección de propósito general, preparada a partir de los datos

También puede evitar la sobrecarga de las llamadas a **Append** que pueden ver en el ejemplo de código anterior. Es posible que tenga el origen de datos, o puede que prefiera rellenar antes de crear el objeto de colección de tiempo de ejecución de Windows. Aquí es cómo hacerlo.

```cppwinrt
auto coll1{ winrt::single_threaded_vector<int>({ 1,2,3 }) };

std::vector<int> values{ 1,2,3 };
auto coll2{ winrt::single_threaded_vector<int>(std::move(values)) };

for (auto const& el : coll2)
{
    std::cout << el << std::endl;
}
```

Puede pasar un objeto temporal que contiene los datos a **winrt::single_threaded_vector**, al igual que con `coll1`, más arriba. O puede mover un **std:: vector** (suponiendo que no se puede obtener acceso a ella nuevamente) en la función. En ambos casos, está pasando un *valor r* a la función. Que permite que el compilador para ser eficaz y evitar copiar los datos. Si desea obtener más información acerca de los *valores r*, vea [categorías de valor y las referencias a ellos](cpp-value-categories.md).

Si desea enlazar un control de elementos XAML a la colección, a continuación, se puede. Pero tenga en cuenta que para establecer correctamente la propiedad [**ItemsControl.ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource) , debe establecer en un valor de tipo **IVector** de **IInspectable** (o de un tipo de interoperabilidad como [**IBindableObservableVector**](/uwp/api/windows.ui.xaml.interop.ibindableobservablevector)). Este es un ejemplo de código que genera una colección de un tipo adecuado para el enlace y agrega un elemento a ella.

```cppwinrt
auto bookSkus{ winrt::single_threaded_vector<Windows::Foundation::IInspectable>() };
bookSkus.Append(make<Bookstore::implementation::BookSku>(L"Moby Dick"));
```

La colección anterior *puede* estar enlazada a un control de elementos XAML; pero la colección no es perceptible.

### <a name="observable-collection"></a>Colección visible

Para recuperar un nuevo objeto de un tipo que implementa una colección *perceptible* , llame a la plantilla de función **winrt::single_threaded_observable_vector** con cualquier tipo de elemento. Pero, para realizar una colección perceptible adecuadas para el enlace a un control de elementos XAML, use **IInspectable** como el tipo de elemento.

El objeto se devuelve como un [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_), y que es la interfaz a través de la que usted (o el control al que está enlazado) llame a las funciones y las propiedades del objeto devuelto.

```cppwinrt
auto bookSkus{ winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>() };
```

Para obtener más detalles y ejemplos de código, acerca de enlace de su usuario (UI) de la interfaz controles a una colección perceptible, vea [controles de los elementos XAML; enlazar a un C + + / WinRT colección](binding-collection.md).

### <a name="associative-container-map"></a>Contenedor asociativa (map)

Existen versiones de contenedor asociativa de las dos funciones que hemos observado.

- La plantilla de la función **single_threaded_map** devuelve un contenedor asociativo como una [**IMap**](/uwp/api/windows.foundation.collections.imap_k_v_). El mapa no es perceptible.
- La plantilla de la función **single_threaded_observable_map** devuelve un contenedor asociativo perceptible como un [**IObservableMap**](/uwp/api/windows.foundation.collections.iobservablemap_k_v_).

Opcionalmente, puede desbloquear estos contenedores con datos pasando a la función un *valor r* de tipo **std:: Map** o **std::unordered_map**.

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

La "subproceso único" en los nombres de estas funciones indica que no proporcionan ninguna simultaneidad&mdash;en otras palabras, no son seguros para subprocesos. La mención de subprocesos no está relacionado con apartamentos, debido a que los objetos devueltos por estas funciones son todo ágiles (vea [objetos ágiles en C + + / WinRT](agile-objects.md)). Sólo es que los objetos son un único subproceso. Y que es totalmente adecuado si desea pasar datos de una forma o la otra a través de la interfaz binaria de aplicaciones (ABI).

## <a name="base-classes-for-collections"></a>Clases base para las colecciones

Si, para una flexibilidad total, que desea implementar su propia colección personalizado, seguramente deseará evitar realizar la manera de disco duro. Por ejemplo, esto es cuál sería el aspecto de una vista personalizada de vector *sin la asistencia de C + + / clases base del WinRT*.

```cppwinrt
...
using namespace Windows::Foundation::Collections;

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

En su lugar, resulta mucho más sencillo derivar su vista vector personalizado de la plantilla de struct **winrt::vector_view_base** y sólo necesita implementar la función **get_container** para exponer el contenedor mantiene los datos.

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

El contenedor devuelto por **get_container** debe proporcionar la interfaz **begin** y **end** que **winrt::vector_view_base** espera. Tal como se muestra en el ejemplo anterior, **std:: vector** proporciona. Pero puede devolver cualquier contenedor que cumpla el mismo contrato, incluidos sus propios contenedores personalizados.

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

Se trata de la base de las clases que C + + / WinRT proporciona para ayudar a implementar colecciones personalizadas.

- **winrt::vector_view_base**
- **winrt::vector_base**
- **winrt::observable_vector_base**
- **winrt::map_view_base**
- **winrt::map_base**
- **winrt::observable_map_base**

## <a name="important-apis"></a>API importantes
* [ItemsControl.ItemsSource](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)
* [IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)
* [IVector](/uwp/api/windows.foundation.collections.ivector_t_)

## <a name="related-topics"></a>Temas relacionados
* [Categorías de valor y las referencias a ellos](cpp-value-categories.md)
* [Controles de elementos XAML; enlazar a una colección C++/WinRT](binding-collection.md)
