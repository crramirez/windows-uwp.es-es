---
description: En este tema se muestra cómo realizar la conversión entre la interfaz binaria de aplicaciones (ABI) y objetos de C++/WinRT.
title: Interoperabilidad entre C++/WinRT y la ABI
ms.date: 11/30/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrate, interop, ABI
ms.localizationpriority: medium
ms.openlocfilehash: 91602c75cdaddc325407529ab4d231db46ecca39
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209150"
---
# <a name="interop-between-cwinrt-and-the-abi"></a>Interoperabilidad entre C++/WinRT y la ABI

En este tema se muestra cómo convertir entre la interfaz binaria de aplicaciones del SDK (ABI) y objetos [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Puedes usar estas técnicas de interoperabilidad entre el código que use estas dos formas de programación con Windows Runtime o puedes usarlas a medida que muevas tu código de la ABI a C++/WinRT.

En general, C++/WinRT expone los tipos ABI como **void\*** , por lo que no necesitas incluir archivos de encabezado de plataforma.

## <a name="what-is-the-windows-runtime-abi-and-what-are-abi-types"></a>¿Qué es la ABI de Windows Runtime y cuáles son los tipos de ABI?
Una clase de Windows Runtime (clase en tiempo de ejecución) realmente es una abstracción. Esta abstracción define una interfaz binaria (la interfaz binaria de aplicaciones o ABI) que permite que varios lenguajes de programación interactúen con un objeto. Independientemente del lenguaje de programación, la interacción de código de cliente con un objeto de Windows Runtime se produce en el nivel más bajo, con construcciones de lenguaje de cliente traducidas a llamadas en la ABI del objeto.

Los encabezados de Windows SDK en la carpeta "%WindowsSdkDir%Include\10.0.17134.0\winrt" (ajusta el número de versión SDK en tu caso, si es necesario) son los archivos de encabezado de la ABI de Windows Runtime. Los ha producido el compilador MIDL. He aquí un ejemplo con uno de estos encabezados incluidos.

```cpp
#include <windows.foundation.h>
```

Y aquí tienes un ejemplo simplificado de uno de los tipos ABI que encontrarás en dicho encabezado de SDK concreto. Ten en cuenta que el espacio de nombres de **ABI**, **Windows::Foundation** y todos los demás espacios de nombres de Windows se declaran por los encabezados de SDK dentro del espacio de nombres de la **ABI**.

```cpp
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

**IUriRuntimeClass** es una interfaz COM. Pero más que eso&mdash;dado que su base es **IInspectable**&mdash;**IUriRuntimeClass**, es una interfaz de Windows Runtime. Ten en cuenta el tipo devuelto **HRESULT** en lugar de generar excepciones. Y el uso de los artefactos como el controlador **HSTRING** (se recomienda devolver dicho controlador a `nullptr` cuando hayas terminado con él). Esto da una idea del aspecto de Windows Runtime en el nivel binario de la aplicación; en otras palabras, en el nivel de programación COM.

Windows Runtime se basa en las API de modelo de objetos componentes (COM). Puedes acceder a Windows Runtime de esta forma, o bien a través de *proyecciones de lenguaje*. Una proyección oculta los detalles del COM y proporciona una experiencia de programación más natural para un lenguaje determinado.

Por ejemplo, si buscas en la carpeta "%WindowsSdkDir%Include\10.0.17134.0\cppwinrt\winrt" (de nuevo, ajusta el número de versión SDK en tu caso, si es necesario), encontrarás los encabezados de proyección del lenguaje C++/WinRT. Hay un encabezado para cada espacio de nombres de Windows, al igual que hay un encabezado ABI por espacio de nombres de Windows. He aquí un ejemplo con uno de los encabezados C++/WinRT incluidos.

```cppwinrt
#include <winrt/Windows.Foundation.h>
```

Y, a partir de dicho encabezado, aquí tienes de forma simplificada el equivalente C++/WinRT de este tipo ABI que acabamos de ver.

```cppwinrt
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

## <a name="converting-to-and-from-abi-types-in-code"></a>Convertir a tipos ABI en código y desde ellos
Por seguridad y sencillez, para las conversiones en ambas direcciones puedes limitarte a usar [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr), [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) y [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function). He aquí un ejemplo de código (basado en la plantilla del proyecto **Console App**), que también muestra cómo puedes usar los alias de espacio de nombre para las diferentes islas para abordar las posibles colisiones de espacio de nombres entre la proyección de C++/WinRT y la ABI.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"

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
    winrt::com_ptr<abi::IStringable> ptr{ uri.as<abi::IStringable>() };

    // Convert from an ABI type.
    uri = ptr.as<winrt::Uri>();
    winrt::IStringable uriAsIStringable{ ptr.as<winrt::IStringable>() };
}
```

Las implementaciones de las funciones **as** llaman a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Si quieres que las conversiones de nivel más bajo solo llamen a [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref), puedes usar las funciones auxiliares [**winrt::copy_to_abi**](/uwp/cpp-ref-for-winrt/copy-to-abi) y [**winrt::copy_from_abi**](/uwp/cpp-ref-for-winrt/copy-from-abi). El siguiente ejemplo de código agrega estas conversiones de nivel más bajo al ejemplo de código anterior.

```cppwinrt
int main()
{
    // The code in main() already shown above remains here.

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
    // The code in main() already shown above remains here.

    // Copy to an owning raw ABI pointer with copy_to_abi.
    abi::IStringable* owning{ nullptr };
    winrt::copy_to_abi(uri, *reinterpret_cast<void**>(&owning));

    // Copy from a raw ABI pointer.
    uri = nullptr;
    winrt::copy_from_abi(uri, owning);
    owning->Release();
```

Para las conversiones de nivel más bajo, que solo copian direcciones, puedes usar las funciones auxiliares [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi), [**winrt::detach_abi**](/uwp/cpp-ref-for-winrt/detach-abi) y [**winrt::attach_abi**](/uwp/cpp-ref-for-winrt/attach-abi).

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

```cppwinrt
    // The code in main() already shown above remains here.

    // Lowest-level conversions that only copy addresses

    // Convert to a non-owning ABI object with get_abi.
    abi::IStringable* non_owning{ static_cast<abi::IStringable*>(winrt::get_abi(uri)) };
    WINRT_ASSERT(non_owning);

    // Avoid interlocks this way.
    owning = static_cast<abi::IStringable*>(winrt::detach_abi(uri));
    WINRT_ASSERT(!uri);
    winrt::attach_abi(uri, owning);
    WINRT_ASSERT(uri);
```

## <a name="convert_from_abi-function"></a>Función convert_from_abi
Esta función auxiliar convierte un puntero de interfaz ABI sin procesar en un objeto equivalente C++/WinRT con una sobrecarga mínima.

```cppwinrt
template <typename T>
T convert_from_abi(::IUnknown* from)
{
    T to{ nullptr }; // `T` is a projected type.

    winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
        winrt::put_abi(to)));

    return to;
}
```

La función simplemente llama a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) para consultar la interfaz predeterminada del tipo C++/WinRT solicitado.

Como hemos visto, no es necesaria la función auxiliar para convertir de un objeto C++/WinRT al puntero de interfaz ABI equivalente. Solo tienes que usar la función miembro [**winrt::Windows::Foundation::IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) (o [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)) para consultar la interfaz solicitada. Las funciones **as** y **try_as** devuelven un objeto [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que encapsula el tipo ABI solicitado.

## <a name="code-example-using-convert_from_abi"></a>Ejemplo de código con convert_from_abi
Aquí tienes un ejemplo de código que muestra esta función auxiliar en práctica.

```cppwinrt
// pch.h
#pragma once
#include <windows.foundation.h>
#include <unknwn.h>
#include "winrt/Windows.Foundation.h"

// main.cpp
#include "pch.h"
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

namespace sample
{
    template <typename T>
    T convert_from_abi(::IUnknown* from)
    {
        T to{ nullptr }; // `T` is a projected type.

        winrt::check_hresult(from->QueryInterface(winrt::guid_of<T>(),
            winrt::put_abi(to)));

        return to;
    }
    inline auto put_abi(winrt::hstring& object) noexcept
    {
        return reinterpret_cast<HSTRING*>(winrt::put_abi(object));
    }
}

int main()
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wcout << "C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert to an ABI type.
    winrt::com_ptr<abi::IUriRuntimeClass> ptr = uri.as<abi::IUriRuntimeClass>();
    winrt::hstring domain;
    winrt::check_hresult(ptr->get_Domain(sample::put_abi(domain)));
    std::wcout << "ABI: " << domain.c_str() << std::endl;

    // Convert from an ABI type.
    winrt::Uri uri_from_abi = sample::convert_from_abi<winrt::Uri>(ptr.get());

    WINRT_ASSERT(uri.Domain() == uri_from_abi.Domain());
    WINRT_ASSERT(uri == uri_from_abi);
}
```

## <a name="interoperating-with-abi-com-interface-pointers"></a>Interoperaciones con los punteros de la interfaz ABI COM

La plantilla de la función auxiliar que tienes a continuación ilustra cómo copiar un puntero de interfaz COM ABI de un tipo determinado a tu tipo de puntero inteligente y proyectado C++/WinRT equivalente.

```cppwinrt
template<typename To, typename From>
To to_winrt(From* ptr)
{
    To result{ nullptr };
    winrt::check_hresult(ptr->QueryInterface(winrt::guid_of<To>(), winrt::put_abi(result)));
    return result;
}
...
ID2D1Factory1* com_ptr{ ... };
auto cppwinrt_ptr {to_winrt<winrt::com_ptr<ID2D1Factory1>>(com_ptr)};
```

La siguiente plantilla de función auxiliar es equivalente, excepto que se copia desde el tipo de puntero inteligente que se encuentra en [Bibliotecas de implementación de Windows (WIL)](https://github.com/Microsoft/wil).

```cppwinrt
template<typename To, typename From, typename ErrorPolicy>
To to_winrt(wil::com_ptr_t<From, ErrorPolicy> const& ptr)
{
    To result{ nullptr };
    if constexpr (std::is_same_v<typename ErrorPolicy::result, void>)
    {
        ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result));
    }
    else
    {
        winrt::check_result(ptr.query_to(winrt::guid_of<To>(), winrt::put_abi(result)));
    }
    return result;
}
```

También, consulta [Consumir componentes COM con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-com).

### <a name="unsafe-interop-with-abi-com-interface-pointers"></a>Interoperabilidad insegura con punteros de interfaz ABI COM

En la siguiente tabla se muestra (además de otras operaciones) las conversiones inseguras entre un puntero de interfaz ABI COM de un tipo determinado y su tipo de puntero inteligente y proyectado C++/WinRT equivalente. Para el código en la tabla, se presentan estas declaraciones.

```cppwinrt
winrt::Sample s;
ISample* p;

void GetSample(_Out_ ISample** pp);
```

Supongamos, además, que **ISample** es la interfaz predeterminada de **Sample**.

Puedes establecer el tiempo de compilación con este código.

```cppwinrt
static_assert(std::is_same_v<winrt::default_interface<winrt::Sample>, winrt::ISample>);
```

| Operación | Cómo hacerlo | Notas |
|-|-|-|
| Extraer **ISample\*** de **winrt::Sample** | `p = reinterpret_cast<ISample*>(get_abi(s));` | *s* todavía posee el objeto. |
| Desasociar **ISample\*** de **winrt::Sample** | `p = reinterpret_cast<ISample*>(detach_abi(s));` | *s* ya no posee el objeto. |
| Transferir **ISample\*** al nuevo **winrt::Sample** | `winrt::Sample s{ p, winrt::take_ownership_from_abi };` | *s* toma posesión del objeto. |
| Establecer **ISample\*** al nuevo **winrt::Sample** | `*put_abi(s) = p;` | *s* toma posesión del objeto. Cualquier objeto que anteriormente era propiedad de *s* se filtra (se confirmará en la depuración). |
| Establecer **ISample\*** en el nuevo **winrt::Sample** | `GetSample(reinterpret_cast<ISample**>(put_abi(s)));` | *s* toma posesión del objeto. Cualquier objeto que anteriormente era propiedad de *s* se filtra (se confirmará en la depuración). |
| Establecer **ISample\*** en **winrt::Sample** | `attach_abi(s, p);` | *s* toma posesión del objeto. El objeto que anteriormente pertenecía a *s* se libera. |
| Copiar **ISample\*** en **winrt::Sample** | `copy_from_abi(s, p);` | *s* hace referencia al objeto. El objeto que anteriormente pertenecía a *s* se libera. |
| Copiar **winrt::Sample** a **ISample\*** | `copy_to_abi(s, reinterpret_cast<void*&>(p));` | *p* recibe una copia del objeto. Cualquier objeto que anteriormente pertenecía a *p* se libera. |

## <a name="interoperating-with-the-abis-guid-struct"></a>Interoperaciones con la estructura GUID de ABI

[**GUID**](/previous-versions/aa373931(v%3Dvs.80)) se proyecta como **winrt::guid**. Para las API que implementes, debes usar **winrt::guid** para los parámetros GUID. En caso contrario, hay conversiones automáticas entre **winrt::guid** y **GUID** siempre y cuando incluyas `unknwn.h` (incluido de forma implícita mediante < windows.h > y muchos otros archivos de encabezado) antes de incluir cualquier encabezado de C++/WinRT.

Si no lo haces, entonces puedes forzar el valor `reinterpret_cast` entre ellos. Para la tabla siguiente, se presentan estas declaraciones.

```cppwinrt
winrt::guid winrtguid;
GUID abiguid;
```

| Conversión | Con `#include <unknwn.h>` | Sin `#include <unknwn.h>` |
|-|-|-|
| Desde **winrt::guid** a **GUID** | `abiguid = winrtguid;` | `abiguid = reinterpret_cast<GUID&>(winrtguid);` |
| Desde **GUID** a **winrt::guid** | `winrtguid = abiguid;` | `winrtguid = reinterpret_cast<winrt::guid&>(abiguid);` |

## <a name="interoperating-with-the-abis-hstring"></a>Interoperaciones con el valor HSTRING de ABI

La tabla siguiente muestra las conversiones entre **winrt::hstring**, [**HSTRING**](/windows/win32/winrt/hstring) y otras operaciones. Para el código en la tabla, se presentan estas declaraciones.

```cppwinrt
winrt::hstring s;
HSTRING h;

void GetString(_Out_ HSTRING* value);
```

| Operación | Cómo hacerlo | Notas |
|-|-|-|
| Extraer **HSTRING** desde **hstring** | `h = static_cast<HSTRING>(get_abi(s));` | *s* todavía posee la cadena. |
| Desasociar **HSTRING** desde **hstring** | `h = reinterpret_cast<HSTRING>(detach_abi(s));` | *s* ya no posee la cadena. |
| Establecer **HSTRING** desde **hstring** | `*put_abi(s) = h;` | *s* toma posesión de la cadena. Cualquier cadena que anteriormente era propiedad de *s* se filtra (se confirmará en la depuración). |
| Recibir **HSTRING** en **hstring** | `GetString(reinterpret_cast<HSTRING*>(put_abi(s)));` | *s* toma posesión de la cadena. Cualquier cadena que anteriormente era propiedad de *s* se filtra (se confirmará en la depuración). |
| Reemplazar **HSTRING** en **hstring** | `attach_abi(s, h);` | *s* toma posesión de la cadena. La cadena que anteriormente pertenecía a *s* se libera. |
| Copiar **HSTRING** en **hstring** | `copy_from_abi(s, h);` | *s* realiza una copia privada de la cadena. La cadena que anteriormente pertenecía a *s* se libera. |
| Copiar **hstring** en **HSTRING** | `copy_to_abi(s, reinterpret_cast<void*&>(h));` | *h* recibe una copia de la cadena. Cualquier cadena que anteriormente pertenecía a *h* se libera. |

## <a name="important-apis"></a>API importantes
* [Función AddRef](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)
* [Función QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Función winrt::attach_abi](/uwp/cpp-ref-for-winrt/attach-abi)
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Función winrt::copy_from_abi](/uwp/cpp-ref-for-winrt/copy-from-abi)
* [Función winrt::copy_to_abi](/uwp/cpp-ref-for-winrt/copy-to-abi)
* [Función winrt::detach_abi](/uwp/cpp-ref-for-winrt/detach-abi)
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función miembro winrt::Windows::Foundation::IUnknown::as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)
* [Función miembro winrt::Windows::Foundation::IUnknown::try_as](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)
