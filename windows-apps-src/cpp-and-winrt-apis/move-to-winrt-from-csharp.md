---
description: En este tema se muestra cómo migrar código de C# a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde C#
ms.date: 07/15/2019
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migrar, C#
ms.localizationpriority: medium
ms.openlocfilehash: a63d38db613ebe6425a05ed20563405242ffd441
ms.sourcegitcommit: ba4a046793be85fe9b80901c9ce30df30fc541f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68328860"
---
# <a name="move-to-cwinrt-from-c"></a>Migrar a C++/WinRT desde C#

En este tema se muestra cómo trasladar el código de un proyecto de [C#](/visualstudio/get-started/csharp) a su equivalente en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

## <a name="register-an-event-handler"></a>Registrar un controlador de eventos

Puedes registrar un controlador de eventos en el marcado XAML.

```xaml
<Button x:Name="OpenButton" Click="OpenButton_Click" />
```

En C#, el método **OpenButton_Click** puede ser privado, y XAML aún podrá conectarlo al evento [**ButtonBase.Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) generado por *OpenButton*.

En C++/WinRT, el método **OpenButton_Click** debe ser público en el [tipo de implementación](/windows/uwp/cpp-and-winrt-apis/author-apis).

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

Como alternativa, puedes convertir la clase de registro en amigo de la clase de implementación, y **OpenButton_Click** en privado.

```cppwinrt
namespace winrt::MyProject::implementation
{
    struct MyPage : MyPageT<MyPage>
    {
    private:
        friend MyPageT;
        void OpenButton_Click(
            winrt::Windows:Foundation::IInspectable const& sender,
            winrt::Windows::UI::Xaml::RoutedEventArgs const& args);
    }
};
```

## <a name="making-a-class-available-to-the-binding-markup-extension"></a>Poner una clase a disposición de la extensión de marcado {Binding}

Si piensas usar la extensión de marcado {Binding} para enlazar datos al tipo de datos, consulta [Objeto de enlace que se declara usando {Binding}](/windows/uwp/data-binding/data-binding-in-depth#binding-object-declared-using-binding).

## <a name="making-a-data-source-available-to-xaml-markup"></a>Poner un origen de datos a disposición del marcado XAML

En C++/WinRT versión 2.0.190530.8 y posterior, [**winrt::single_threaded_observable_vector**](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector) crea un vector observable que admite tanto **[IObservableVector](/uwp/api/windows.foundation.collections.iobservablevector_t_)\<T\>** como **IObservableVector\<IInspectable\>** .

Puedes crear el **archivo Midl (.idl)** de este modo (consulta también [Factorizar clases en tiempo de ejecución en archivos (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

```idl
namespace Bookstore
{
    runtimeclass BookSku { ... }

    runtimeclass BookstoreViewModel
    {
        Windows.Foundation.Collections.IObservableVector<BookSku> BookSkus{ get; };
    }

    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();
        BookstoreViewModel MainViewModel{ get; };
    }
}
```

E implementarlo de este modo.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Bookstore::BookSku>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Bookstore::BookSku> m_bookSkus;
};
...
```

Para más información, consulta [Controles de elementos XAML; enlazar a una colección C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection).

## <a name="making-a-data-source-available-to-xaml-markup-prior-to-cwinrt-201905308"></a>Poner un origen de datos a disposición del marcado XAML (antes de C++/WinRT 2.0.190530.8)

El enlace de datos XAML requiere que un origen de los elementos implemente **[IIterable](/uwp/api/windows.foundation.collections.iiterable_t_)\<IInspectable\>** , así como una de las siguientes combinaciones de interfaces.

- **IObservableVector\<IInspectable\>**
- **IBindableVector** e **INotifyCollectionChanged**
- **IBindableVector** e **IBindableObservableVector**
- **IBindableVector** por sí mismo (no responderá a los cambios)
- **IVector\<IInspectable\>**
- **IBindableIterable** (se iterará y guardará los elementos en una colección privada)

No se puede detectar una interfaz genérica como **IVector\<T\>** en tiempo de ejecución. Cada **IVector\<T\>** tiene un identificador de interfaz diferente (IID), que es una función de **T**. Cualquier desarrollador puede expandir el conjunto de **T** de manera arbitraria, por lo que queda claro que el código de enlace XAML nunca puede conocer el conjunto completo al que va a consultar. Esa restricción no supone un problema para C#, ya que cada objeto CLR que implementa **IEnumerable\<T\>** implementa **IEnumerable** de manera automática. En el nivel de ABI, eso significa que cada objeto que implementa **IObservableVector\<T\>** implementa **IObservableVector\<IInspectable\>** de manera automática.

C++/WinRT no ofrece esa garantía. Si una clase de C++/WinRT en tiempo de ejecución implementa **IObservableVector\<T\>** , no podemos suponer que de, algún modo, también se proporciona una implementación de **IObservableVector\<IInspectable\>** .

Por lo tanto, así es como deberá verse el ejemplo anterior.

```idl
...
runtimeclass BookstoreViewModel
{
    // This is really an observable vector of BookSku.
    Windows.Foundation.Collections.IObservableVector<Object> BookSkus{ get; };
}
```

Y la implementación.

```cppwinrt
// BookstoreViewModel.h
...
struct BookstoreViewModel : BookstoreViewModelT<BookstoreViewModel>
{
    BookstoreViewModel()
    {
        m_bookSkus = winrt::single_threaded_observable_vector<Windows::Foundation::IInspectable>();
        m_bookSkus.Append(winrt::make<Bookstore::implementation::BookSku>(L"To Kill A Mockingbird"));
    }
    
    // This is really an observable vector of BookSku.
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> BookSkus();
    {
        return m_bookSkus;
    }

private:
    Windows::Foundation::Collections::IObservableVector<Windows::Foundation::IInspectable> m_bookSkus;
};
...
```

Si tienes que acceder a objetos en *m_bookSkus*, tendrás que volver a aplicarles QueryInterface en **Bookstore::BookSku**.

```cppwinrt
Widget MyPage::BookstoreViewModel(winrt::hstring title)
{
    for (auto&& obj : m_bookSkus)
    {
        auto bookSku = obj.as<Bookstore::BookSku>();
        if (bookSku.Title() == title) return bookSku;
    }
    return nullptr;
}
```

## <a name="tostring"></a>ToString()

Los tipos de C# proporcionan el método [Object.ToString](/dotnet/api/system.object.tostring).

```csharp
int i = 2;
var s = i.ToString(); // s is a System.String with value "2".
```

C++/ WinRT no ofrece directamente esta función, pero puedes elegir alternativas.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT también admite [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) para un número limitado de tipos. Tendrás que agregar sobrecargas para los tipos adicionales a los que quieras aplicar stringify.

| Idioma | Stringify int | Stringify enum |
| - | - | - |
| C# | `string result = "hello, " + intValue.ToString();`<br>`string result = $"hello, {intValue}";` | `string result = "status: " + status.ToString();`<br>`string result = $"status: {status}";` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

En caso de aplicar stringify en una enumeración, debes proporcionar la implementación de **winrt::to_hstring**.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

El enlace de datos suele consumir implícitamente estas aplicaciones de stringify.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Estos enlaces ejecutarán **winrt::to_hstring** de la propiedad enlazada. En el caso del segundo ejemplo (**StatusEnum**), debes proporcionar tu propia sobrecarga de **winrt::to_hstring**; de lo contrario, recibirás un error del compilador.

## <a name="string-building"></a>Creación de cadenas

En el caso de la creación de cadenas, C# tiene un tipo [**StringBuilder**](/dotnet/api/system.text.stringbuilder) integrado.

| | C# | C++/WinRT |
|-|-|-|
| Clase de generador | `StringBuilder builder;` | `std::wstringstream stream;` |
| Anexar cadena, conservando valores null | `builder.Append(s);` | `stream << std::wstring_view{ s };` |
| Extraer resultado | `s = builder.ToString();` | `ws = stream.str();` |

## <a name="boxing-and-unboxing"></a>Conversiones boxing y unboxing

C# aplica automáticamente boxing a los valores escalares para convertirlos en objetos. C++/WinRT requiere que llames a la función [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) de manera explícita. Ambos lenguajes requieren que apliques la conversión unboxing de manera explícita. Consulta [Conversión boxing y unboxing con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

En las tablas siguientes, usaremos estas definiciones.

| C# | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `string s;` | `winrt::hstring s;` |
| `object o;` | `IInspectable o;`|

| Operación | C# | C++/WinRT|
|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (string)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX y C# producen excepciones si intentas aplicar unboxing para convertir un puntero nulo en a un tipo de valor. C++/WinRT considera que se trata de un error de programación y se bloquea. En C++/WinRT, usa la función [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or) si quieres controlar el caso en el que el objeto no sea del tipo que pensabas que era.

| Escenario | C# | C++/WinRT|
|-|-|-|
| Conversión unboxing de un entero conocido |`i = (int)o;` | `i = unbox_value<int>(o);` |
| Si o es null | `System.NullReferenceException` | Bloqueo |
| Si o no es un entero con conversión boxing | `System.InvalidCastException` | Bloqueo |
| Unboxing de entero, usar reserva si es null; bloquear en cualquier otro caso | `i = o != null ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Unboxing de entero, si es posible; usar reversión en cualquier otro caso | `var box = o as int?;`<br>`i = box != null ? box.Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Conversiones boxing y unboxing en una cadena

En cierto modo, una cadena es un tipo de valor y, de otro modo, un tipo de referencia. C# y C++/WinRT tratan las cadenas de manera diferente.

El tipo de ABI [**HSTRING**](/windows/win32/winrt/hstring) es un puntero a una cadena de recuento de referencias. Pero no se obtiene de [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable), por lo que, técnicamente, no es un *objeto*. Además, un **HSTRING** nulo representa la cadena vacía. La conversión boxing aplicada a cosas que no se derivan de **IInspectable** se realiza encapsulándolas dentro de un [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), y Windows Runtime proporciona una implementación estándar en forma de objeto [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue) (los tipos personalizados se notifican como [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype)).

C# representa una cadena de Windows Runtime como tipo de referencia; mientras que C++/WinRT proyecta una cadena como tipo de valor. Es decir, una cadena nula a la que se ha aplicado la conversión boxing puede tener distintas representaciones en función de cómo llegue allí.

| Operación | C# | C++/WinRT|
|-|-|-|
| Categoría de tipo de cadena | Tipo de referencia | Tipo de valor |
| **HSTRING** null se proyecta como | `""` | `hstring{ nullptr }` |
| ¿Son null y `""` idénticos? | No | Sí |
| Validez de null | `s = null;`<br>`s.Length` genera **NullReferenceException** | `s = nullptr;`<br>`s.size() == 0` (válido) |
| Aplicar boxing a una cadena | `o = s;` | `o = box_value(s);` |
| Si `s` es `null` | `o = (string)null;`<br>`o == null` | `o = box_value(hstring{nullptr});`<br>`o != nullptr` |
| Si `s` es `""` | `o = "";`<br>`o != null;` | `o = box_value(hstring{L""});`<br>`o != nullptr;` |
| Aplicar boxing a una cadena, conservando null | `o = s;` | `o = s.empty() ? nullptr : box_value(s);` |
| Forzar boxing a una cadena | `o = PropertyValue.CreateString(s);` | `o = box_value(s);` |
| Unboxing a una cadena conocida | `s = (string)o;` | `s = unbox_value<hstring>(o);` |
| Si `o` es null | `s == null; // not equivalent to ""` | Bloqueo |
| Si `o` no es una cadena con conversión boxing | `System.InvalidCastException` | Bloqueo |
| Unboxing de cadena, usar reserva si es null; bloquear en cualquier otro caso | `s = o != null ? (string)o : fallback;` | `s = o ? unbox_value<hstring>(o) : fallback;` |
| Unboxing de cadena, si es posible; usar reversión en cualquier otro caso | `var s = o as string ?? fallback;` | `s = unbox_value_or<hstring>(o, fallback);` |

En los dos casos de *conversión unboxing con reserva* anteriores, es posible que se fuerce la conversión boxing a una cadena null, en cuyo caso no se usará la reserva. El valor resultante será una cadena vacía, ya que es lo que se encontraba tras la conversión boxing.

## <a name="derived-classes"></a>Clases derivadas

Para derivar a partir de una clase en tiempo de ejecución, la clase base debe *admitir la composición*. C# no requiere que sigas ningún paso en particular para hacer que estas clases admitan composición, pero C++/WinRT sí. Usa la [palabra clave unsealed](/uwp/midl-3/intro#base-classes) para indicar que quieres que la clase se pueda usar como clase base.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

En la clase de encabezado de tu implementación, tienes que incluir el archivo de encabezado de la clase base antes de incluir el encabezado generado automáticamente para la clase derivada. De lo contrario, obtendrás errores como "uso no válido de este tipo como expresión".

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="consuming-objects-from-xaml-markup"></a>Consumir objetos a partir del marcado XAML

En un proyecto de C#, puedes consumir miembros privados y elementos con nombre del marcado XAML. Pero en C++/WinRT, todas las entidades consumidas por la [**extensión de marcado {x:Bind}** ](/windows/uwp/xaml-platform/x-bind-markup-extension) de XAML deben exponerse públicamente en IDL.

Además, el enlace a un valor booleano muestra `true` o `false`en C#, pero muestra **Windows.Foundation.IReference`1\<valor booleano\>** en C++/WinRT.

Para más información y ejemplos de código, consulta [Consumir objetos a partir del marcado](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="important-apis"></a>API importantes
* [Plantilla de función winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)
* [Espacio de nombres de winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Temas relacionados
* [Tutoriales de C#](/visualstudio/get-started/csharp)
* [Crear API con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis)
* [Enlace de datos en profundidad](/windows/uwp/data-binding/data-binding-in-depth)
* [Controles de elementos de XAML; enlazar a una colección C++/WinRT](/windows/uwp/cpp-and-winrt-apis/binding-collection)