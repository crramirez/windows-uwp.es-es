---
author: stevewhims
description: Este tema muestra cómo convertir entre la interfaz binaria de aplicaciones (ABI) y objetos C++/WinRT.
title: Interoperabilidad entre C++/WinRT y la ABI
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, interoperabilidad, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 40d2c29dcab1a54046cb0def882cfa5f80b1f1f6
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6844035"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilidad entre C++/WinRT y la ABI

Este tema muestra cómo convertir entre la interfaz binaria de aplicaciones de SDK (ABI) y [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) objetos. Puedes usar estas técnicas de interoperabilidad entre el código que use estas dos formas de programación con Windows Runtime o puedes usarlas a medida que muevas tu código de la ABI a C++/WinRT.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>¿Qué es la ABI de Windows Runtime y cuáles son los tipos de ABI?
Una clase de Windows Runtime (clase en tiempo de ejecución) realmente es una abstracción. Esta abstracción define una interfaz binaria (la interfaz binaria de aplicaciones o ABI) que permite que varios lenguajes de programación interactúen con un objeto. Independientemente del lenguaje de programación, la interacción de código de cliente con un objeto de Windows Runtime se produce en el nivel más bajo, con construcciones de lenguaje de cliente traducidas a llamadas en la ABI del objeto.

Los encabezados de Windows SDK en la carpeta "%WindowsSdkDir%Include\10.0.17134.0\winrt" (ajusta el número de versión SDK en tu caso, si es necesario) son los archivos de encabezado de la ABI de Windows Runtime. Los ha producido el compilador MIDL. He aquí un ejemplo con uno de estos encabezados incluidos.

```
#include <windows.foundation.h>
```

Y aquí tienes un ejemplo simplificado de uno de los tipos ABI que encontrarás en dicho encabezado de SDK concreto. Ten en cuenta que el espacio de nombres de **ABI**, **Windows::Foundation** y todos los demás espacios de nombres de Windows se declaran por los encabezados de SDK dentro del espacio de nombres de la **ABI**.

```
namespace ABI::Windows::Foundation
{
    IUriRuntimeClass : public IInspectable
    {
    public:
        /* [propget] */ virtual HRESULT STDMETHODCALLTYPE get_AbsoluteUri(/* [retval, out] */__RPC__deref_out_opt HSTRING * value) = 0;
        ...
    }
}
```

**IUriRuntimeClass** es una interfaz COM. Pero más que eso&mdash;dado que su base es **IInspectable**&mdash;**IUriRuntimeClass**, es una interfaz de Windows Runtime. Ten en cuenta el tipo devuelto **HRESULT** en lugar de generar excepciones. Y el uso de los artefactos como el manipulador **HSTRING** (es recomendable devolver dicho manipulador a `nullptr` cuando hayas terminado con él). Esto da una idea del aspecto de Windows Runtime en el nivel binario de la aplicación; en otras palabras, en el nivel de programación COM.

Windows Runtime se basa en las API de modelo de objetos componentes (COM). Puedes acceder a Windows Runtime de esta forma, o puedes acceder a él a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un determinado lenguaje.

Por ejemplo, si buscas en la carpeta "%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt" (de nuevo, ajusta el número de versión SDK en tu caso, si es necesario), encontrarás los encabezados de proyección del lenguaje C++/WinRT. Hay un encabezado para cada espacio de nombres de Windows, al igual que hay un encabezado ABI por espacio de nombres de Windows. He aquí un ejemplo con uno de los encabezados C++/WinRT incluidos.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Y, a partir de dicho encabezado, aquí tienes de forma simplificada el equivalente C++/WinRT de este tipo ABI que acabamos de ver.

```
namespace winrt::Windows::Foundation
{
    struct Uri : IUriRuntimeClass, ...
    {
        winrt::hstring AbsoluteUri() const { ... }
        ...
    };
}
```

La interfaz es C++ moderno y estándar. Acaba con los **HRESULT** (C++/WinRT genera excepciones si es necesario). Y la función de descriptor de acceso devuelve un objeto de cadena simple, que se ha limpiado al final de su ámbito.

Este tema es para casos en los que quieres interoperar con código que funciona en la capa de la interfaz binaria de aplicaciones (ABI), o portarlo.

## <a name="converting-to-and-from-abi-types-in-code"></a>Convertir a y desde tipos ABI en código
Por seguridad y sencillez, para las conversiones en ambas direcciones puedes limitarte a usar [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) y [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). He aquí un ejemplo de código (basado en la plantilla del proyecto **Console App**), que también muestra cómo puedes usar los alias de espacio de nombre para las diferentes islas para abordar las posibles colisiones de espacio de nombres entre la proyección de C++/WinRT y la ABI.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");

    // Convert to an ABI type.
    winrt::com_ptr<abi::IStringable> ptr = uri.as<abi::IStringable>();

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable = ptr.as<winrt::IStringable>();
}
```

Las implementaciones de las funciones **as** llaman a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521). Si quieres que las conversiones de nivel más bajo solo llamen a [**AddRef**](https://msdn.microsoft.com/library/windows/desktop/ms691379), puedes usar las funciones auxiliares [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) y [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). El siguiente ejemplo de código agrega estas conversiones de nivel más bajo al ejemplo de código anterior.

```cppwinrt
int main()
{
    ...

    // Lower-level conversions that only call AddRef.

    // Convert to an ABI type.
    ptr = nullptr;
    winrt::copy_to_abi(uri, *ptr.put_void());

    // Convert from an ABI type.
    uri = nullptr;
    winrt::copy_from_abi(uri, ptr.get());
    ptr = nullptr;
}
```

Estas son otras técnicas de conversiones de bajo nivel de forma similar, pero usando punteros sin procesar a tipos de interfaz ABI (los definidos por los encabezados de Windows SDK) esta vez.

```cppwinrt
    ...

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning = nullptr;
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Para las conversiones de nivel más bajo, que solo copian direcciones, puedes usar las funciones auxiliares [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) y [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

```cppwinrt
    ...

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning = static_cast<abi::IStringable*>(winrt::get_abi(uri));
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convertfromabi-function"></a>Función convert_from_abi
Esta función auxiliar convierte un puntero de interfaz ABI sin procesar a un objeto equivalente C++/WinRT con una sobrecarga mínima.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

La función simplemente llama a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) para consultar la interfaz predeterminada del tipo C++/WinRT solicitado.

Como hemos visto, no es necesaria la función auxiliar para convertir de un objeto C++/WinRT al puntero de interfaz ABI equivalente. Solo tienes que usar la función miembro [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (o [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)) para consultar la interfaz solicitada. Las funciones **as** y **try_as** devuelven un objeto [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que encapsula el tipo ABI solicitado.

## <a name="code-example-using-convertfromabi"></a>Ejemplo de código con convert_from_abi
Aquí tienes un ejemplo de código que muestra esta función auxiliar en práctica.

```cppwinrt
// main.cpp
#include "pch.h"

#include <windows.foundation.h>
#include <winrt/Windows.Foundation.h>
#include <iostream>

using namespace winrt;
using namespace Windows::Foundation;

namespace winrt
{
    using namespace Windows::Foundation;
}

namespace abi
{
    using namespace ABI::Windows::Foundation;
};

template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr };

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="important-apis"></a>API importantes
* [Función AddRef](https://msdn.microsoft.com/library/windows/desktop/ms691379)
* [Función QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [función winrt:: attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [función winrt:: copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [función winrt:: copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [función winrt:: detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función miembro winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Función miembro winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntryas-function)
