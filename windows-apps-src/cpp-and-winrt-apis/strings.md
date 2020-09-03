---
description: Con C++/WinRT puedes llamar a las API de Windows Runtime mediante tipos de cadena de caracteres anchos de C++ estándar, o bien, puedes usar el tipo winrt::hstring.
title: Control de cadenas en C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, string
ms.localizationpriority: medium
ms.openlocfilehash: 56b75710c2d259e59dac476bcb860a5e4c6938d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154399"
---
# <a name="string-handling-in-cwinrt"></a>Control de cadenas en C++/WinRT

Con [C++/WinRT](./intro-to-using-cpp-with-winrt.md) puedes llamar a las API de Windows Runtime mediante tipos de cadenas de caracteres anchos de la biblioteca estándar de C++ como **std::wstring** (nota: no con tipos de cadenas de caracteres estrechos **std::string**). C++/WinRT tiene un tipo de cadena personalizada denominado [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (definido en la biblioteca de base de C++/WinRT, que es `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Y este es el tipo de cadena que los constructores, las funciones y las propiedades de Windows Runtime toman y devuelven realmente. Pero, en muchos casos (gracias a los constructores de conversión y los operadores de conversión de**hstring**) puedes elegir tener en cuenta **hstring** o no en tu código cliente. Si vas a *crear* API, es probable que necesites información sobre **hstring**.

Hay muchos tipos de cadena en C++. Las variantes existen en muchas bibliotecas además de **std::basic_string** en la biblioteca estándar de C++. C++17 tiene utilidades de conversión de cadena y **std::basic_string_view**, para enlazar las separaciones entre todos los tipos de cadena.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) proporciona convertibilidad con **std::wstring_view** para proporcionar la interoperabilidad para la que se ha diseñado **std::basic_string_view**.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Mediante **std::wstring** (y opcionalmente **winrt::hstring**) con **Uri**
[**Windows::Foundation::URI**](/uwp/api/windows.foundation.uri) se ha construido a partir de [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Pero **hstring** tiene [constructores de conversión](/uwp/cpp-ref-for-winrt/hstring#hstringhstring-constructor) que te permiten trabajar con él sin necesidad de tenerlo en cuenta. Este es un ejemplo de código que muestra cómo crear un **Uri** desde un literal de cadena de caracteres anchos, desde una vista de cadena de caracteres anchos y desde un **std::wstring**.

```cppwinrt
#include <winrt/Windows.Foundation.h>
#include <string_view>

using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    using namespace std::literals;

    winrt::init_apartment();

    // You can make a Uri from a wide string literal.
    Uri contosoUri{ L"http://www.contoso.com" };

    // Or from a wide string view.
    Uri contosoSVUri{ L"http://www.contoso.com"sv };

    // Or from a std::wstring.
    std::wstring wideString{ L"http://www.adventure-works.com" };
    Uri awUri{ wideString };
}
```

El descriptor de acceso a la propiedad [**Uri::Domain**](/uwp/api/windows.foundation.uri.Domain) es de tipo **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Pero, nuevamente, tener en cuenta este detalle es opcional gracias al [operador de conversión a **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view) de **hstring**.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Del mismo modo, [**IStringable::ToString**](/windows/desktop/api/windows.foundation/nf-windows-foundation-istringable-tostring) devuelve hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**URI** implementa la interfaz [**IStringable**](/windows/desktop/api/windows.foundation/nn-windows-foundation-istringable).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Puedes usar la función [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function) para obtener una cadena estándar de caracteres anchos de un **hstring** (del mismo modo que desde un **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Si tienes un **hstring**, puedes crear un **Uri** a partir de él.

```cppwinrt
Uri awUriFromHstring{ tostringHstring };
```

Ten en cuenta un método que tome un **hstring**.

```cppwinrt
public:
    Uri CombineUri(winrt::hstring relativeUri) const;
```

Todas las opciones que acabas de ver también se aplican en estos casos.

```cppwinrt
std::wstring contact{ L"contact" };
contosoUri = contosoUri.CombineUri(contact);
    
std::wcout << contosoUri.ToString().c_str() << std::endl;
```

**hstring** tiene un operador de conversión **std::wstring_view** miembro y la conversión se logra sin costos.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Funciones y operadores de **winrt::hstring**
Se ha implementado una gran cantidad de constructores, operadores, funciones e iteradores para [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

**hstring** es un intervalo, así que puedes usarlo con `for` basado en intervalo o con `std::for_each`. También proporciona los operadores de comparación para compararlos de forma natural y eficaz con sus homólogos en la biblioteca estándar de C++. E incluye todo lo que necesitas para usar **hstring** como clave para contenedores asociativos.

Reconocemos que muchas de las bibliotecas de C++ usan **std::string** y funcionan exclusivamente con texto UTF-8. Para mayor comodidad, proporcionamos asistentes, como [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) y [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), para convertir y revertir.

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Para más ejemplos e información sobre las funciones y los operadores de **hstring**, consulta el tema de referencia de API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>Lógica de **winrt::hstring** y **winrt::param::hstring**
Windows Runtime se implementa en términos de caracteres **wchar_t**, pero la interfaz binaria de aplicaciones (ABI) de Windows Runtime no es un subconjunto que proporcionen **std::wstring** o **std::wstring_view**. Usarlos provocaría ineficiencias significativas. En su lugar, C++/WinRT proporciona **winrt::hstring**, que representa una cadena inmutable coherente con el subyacente [HSTRING](/windows/desktop/WinRT/hstring) y se implementa detrás de una interfaz similar a la de **std::wstring**. 

Es posible que observes que los parámetros de entrada de C++/WinRT, que deben aceptar lógicamente **winrt::hstring**, en realidad esperan **winrt::param::hstring**. El espacio de nombres **param** contiene un conjunto de tipos que se usa exclusivamente para optimizar los parámetros de entrada para el enlace natural a tipos de la biblioteca estándar de C++ y para evitar la copia y otras ineficiencias. No deberías usar estos tipos directamente. Si quieres usar una optimización para tus propias funciones, usa **std::wstring_view**. Consulta también [Pasar parámetros a los límites de la ABI](./pass-parms-to-abi.md).

El resultado es que puedes omitir en gran medida las características específicas de gestión de cadenas de Windows Runtime y trabajar así eficazmente con aquello que conoces. Y esto es importante, teniendo en cuenta la gran cantidad de cadenas que se utilizan en Windows Runtime.

## <a name="formatting-strings"></a>Formato de las cadenas
Una opción para dar formato a las cadenas es **std::wostringstream**. Este es un ejemplo que da formato y muestra un mensaje de seguimiento de depuración simple.

```cppwinrt
#include <sstream>
#include <winrt/Windows.UI.Input.h>
#include <winrt/Windows.UI.Xaml.Input.h>
...
void MainPage::OnPointerPressed(winrt::Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    winrt::Windows::Foundation::Point const point{ e.GetCurrentPoint(nullptr).Position() };
    std::wostringstream wostringstream;
    wostringstream << L"Pointer pressed at (" << point.X << L"," << point.Y << L")" << std::endl;
    ::OutputDebugString(wostringstream.str().c_str());
}
```

## <a name="the-correct-way-to-set-a-property"></a>La forma correcta para establecer una propiedad

Establece una propiedad pasando un valor a una función de establecimiento. A continuación se muestra un ejemplo.

```cppwinrt
// The right way to set the Text property.
myTextBlock.Text(L"Hello!");
```

El código siguiente no es correcto. Se compila, pero todo lo que hace es modificar el elemento **winrt::hstring** temporal que devuelve la función de acceso **Text()** , para luego descartar el resultado.

```cppwinrt
// *Not* the right way to set the Text property.
myTextBlock.Text() = L"Hello!";
```

## <a name="important-apis"></a>API importantes
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [Función winrt::to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [Función winrt::to_string](/uwp/cpp-ref-for-winrt/to-string)