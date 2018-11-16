---
author: stevewhims
description: Con C++/WinRT, puedes llamar a las API de Windows Runtime usando tipos de cadena de caracteres anchos de C++ estándar, o puedes usar el tipo winrt::hstring.
title: Control de cadenas en C++/WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c ++ cpp, winrt, proyección, cadena
ms.localizationpriority: medium
ms.openlocfilehash: 72032c3c522a8434d266842a83c443889e8efc19
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6859289"
---
# <a name="string-handling-in-cwinrt"></a>Control de cadenas en C++/WinRT

Con [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), puedes llamar a Windows en tiempo de ejecución APIs con tipos de cadena de caracteres anchos de la biblioteca estándar de C++ como **std:: wstring** (Nota: no con tipos de cadena de caracteres estrechos como **std:: String**). C++/WinRT tiene un tipo de cadena personalizada denominado [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) (definido en la biblioteca de base de C++/WinRT, que es `%WindowsSdkDir%Include\<WindowsTargetPlatformVersion>\cppwinrt\winrt\base.h`). Y este es el tipo de cadena que los constructores, funciones y propiedades de Windows Runtime toman y devuelven realmente. Pero, en muchos casos&mdash;gracias a los constructores de conversión y operadores de conversión de**hstring**&mdash;puedes tener en cuenta **hstring** o no en tu código cliente. Si vas a *crear* API, es probable que necesites saber sobre **hstring**.

Hay muchos tipos de cadena en C++. Las variantes existen en muchas bibliotecas además de **std::basic_string** de la biblioteca estándar de C++. C++17 tiene utilidades de conversión de cadena y **std::basic_string_view** para enlazar las separaciones entre todos los tipos de cadena.  [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) proporciona convertibilidad con **std::wstring_view** para proporcionar la interoperabilidad para la que se ha diseñado **std::basic_string_view**.

## <a name="using-stdwstring-and-optionally-winrthstring-with-uri"></a>Con **std:: wstring** (y opcionalmente **winrt::hstring**) con **Uri**
[**Windows::Foundation::URI**](/uwp/api/windows.foundation.uri) se ha construido a partir de un [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

```cppwinrt
public:
    Uri(winrt::hstring uri) const;
```

Pero **hstring** tiene [constructores de conversión](/uwp/api/windows.foundation.uri#hstringhstring-constructor) que te permiten trabajar con él sin necesidad de tenerlo en cuenta. Este es un ejemplo de código que muestra cómo realizar un **Uri** desde un literal de cadena de caracteres anchos y desde un **std:: wstring**.

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

El descriptor de acceso [**Uri::Domain**](https://docs.microsoft.com/uwp/api/windows.foundation.uri.Domain) es de tipo **hstring**.

```cppwinrt
public:
    winrt::hstring Domain();
```

Pero, nuevamente, tener en cuenta este detalle es opcional gracias al [operador de conversión a**std::wstring_view**](/uwp/api/hstring#hstringoperator-stdwstringview) de **hstring**.

```cppwinrt
// Access a property of type hstring, via a conversion operator to a standard type.
std::wstring domainWstring{ contosoUri.Domain() }; // L"contoso.com"
domainWstring = awUri.Domain(); // L"adventure-works.com"

// Or, you can choose to keep the hstring unconverted.
hstring domainHstring{ contosoUri.Domain() }; // L"contoso.com"
domainHstring = awUri.Domain(); // L"adventure-works.com"
```

Del mismo modo, [**IStringable::ToString**](https://msdn.microsoft.com/library/windows/desktop/dn302136) devuelve hstring.

```cppwinrt
public:
    hstring ToString() const;
```

**URI** implementa la interfaz [**IStringable**](https://msdn.microsoft.com/library/windows/desktop/dn302135).

```cppwinrt
// Access hstring's IStringable::ToString, via a conversion operator to a standard type.
std::wstring tostringWstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringWstring = awUri.ToString(); // L"http://www.adventure-works.com/"

// Or you can choose to keep the hstring unconverted.
hstring tostringHstring{ contosoUri.ToString() }; // L"http://www.contoso.com/"
tostringHstring = awUri.ToString(); // L"http://www.adventure-works.com/"
```

Puedes usar la función [**hstring::c_str function**](/uwp/api/windows.foundation.uri#hstringcstr-function) para obtener una cadena estándar de caracteres anchos de un **hstring** (del mismo modo que desde un **std::wstring**).

```cppwinrt
#include <iostream>
std::wcout << tostringHstring.c_str() << std::endl;
```
Si tienes un **hstring**, puedes hacer un **Uri** a partir de él.

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

**hstring** tiene un operador de conversión **std::wstring_view** miembro y la conversión se logra sin ningún gasto.

```cppwinrt
void legacy_print(std::wstring_view view);

void Print(winrt::hstring const& hstring)
{
    legacy_print(hstring);
}
```

## <a name="winrthstring-functions-and-operators"></a>Funciones y operadores de **winrt::hstring**
Se ha implementado una gran cantidad de constructores, operadores, funciones e iteradores para [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

Un **hstring** es un intervalo, así que puedes usarlo con `for` basado en intervalo o con `std::for_each`. También proporciona los operadores de comparación para compararlos de forma natural y eficaz con sus homólogos en la biblioteca estándar de C++. E incluye todo lo que necesitas para usar **hstring** como clave para contenedores asociativos.

Reconocemos que muchas de las bibliotecas de C++ usan **std::string**y funcionan exclusivamente con texto UTF-8. Para mayor comodidad, proporcionamos aplicaciones auxiliares, como [**winrt::to_string**](/uwp/cpp-ref-for-winrt/to-string) y [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring), para convertir adelante y atrás.

```cppwinrt
winrt::hstring w{ L"Hello, World!" };

std::string c = winrt::to_string(w);
WINRT_ASSERT(c == "Hello, World!");

w = winrt::to_hstring(c);
WINRT_ASSERT(w == L"Hello, World!");
```

Para obtener más ejemplos e información sobre las funciones y los operadores **hstring**, consulta el tema de referencia de API [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring).

## <a name="the-rationale-for-winrthstring-and-winrtparamhstring"></a>La lógica de **winrt::hstring** y **winrt::param::hstring**
Windows Runtime se implementa en términos de caracteres **wchar_t**, pero la interfaz binaria de aplicaciones (ABI) de Windows Runtime no es un subconjunto que **std:: wstring** o **std::wstring_view** proporcionen. Usarlos provocaría ineficiencias significativas. En su lugar, C++/WinRT proporciona **winrt::hstring**, que representa una cadena inmutable coherente con el subyacente [HSTRING](https://msdn.microsoft.com/library/windows/desktop/br205775)y se implementa detrás de una interfaz similar a la de **std:: wstring**. 

Es posible que observes que los parámetros de entrada de C++/WinRT, que deben aceptar lógicamente **winrt::hstring**, en realidad esperan **winrt::param::hstring**. El espacio de nombres **param** contiene un conjunto de tipos que se usa exclusivamente para optimizar los parámetros de entrada para enlazar naturalmente a tipos de la biblioteca estándar de C++ y evitar copias y otras ineficiencias. No deberías usar estos tipos directamente. Si quieres usar una optimización para tus propias funciones, usa **std::wstring_view**.

El resultado es que puedes omitir en gran medida las características específicas de gestión de cadenas de Windows Runtime y trabajar así eficazmente con aquello que conoces. Y esto es importante, teniendo en cuenta la gran cantidad de cadenas que se utilizan en Windows Runtime.

# <a name="formatting-strings"></a>Cadenas de formato
Una opción de formato de cadenas es **std::wstringstream**. Este es un ejemplo que da formato y muestra un mensaje de seguimiento de depuración simple.

```cppwinrt
#include <sstream>
...
void OnPointerPressed(IInspectable const&, PointerEventArgs const& args)
{
    float2 const point = args.CurrentPoint().Position();
    std::wstringstream wstringstream;
    wstringstream << L"Pointer pressed at (" << point.x << L"," << point.y << L")" << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());
}
```

## <a name="important-apis"></a>API importantes
* [Estructura winrt::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [función winrt:: to_hstring](/uwp/cpp-ref-for-winrt/to-hstring)
* [función winrt:: to_string](/uwp/cpp-ref-for-winrt/to-string)
