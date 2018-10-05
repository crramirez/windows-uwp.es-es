---
author: stevewhims
description: Este tema muestra dos funciones auxiliares que pueden usarse para convertir entre objetos C++/CX y C++/WinRT.
title: Interoperabilidad entre C++/WinRT y C++/CX
ms.author: stwhi
ms.date: 05/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, interoperabilidad, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: b60b0d7c201f172261de1546fc250e40b8cd670f
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "4383205"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidad entre C++/WinRT y C++/CX
Este tema muestra dos funciones auxiliares que pueden usarse para convertir entre [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) y [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) objetos. Puede usarlos para interoperar entre el código que usa las dos proyecciones de lenguaje, o puedes usar las funciones de medida que muevas tu código desde C++ / CX a C++ / WinRT (consulta [mover a C++ / WinRT desde C++ / CX](move-to-winrt-from-cx.md)).

## <a name="fromcx-and-tocx-functions"></a>Funciones from_cx y to_cx
La siguiente función auxiliar convierte un objeto C++/CX en un objeto C++/WinRT equivalente. La función convierte un objeto C++/CX en su puntero de interfaz [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) subyacente. Luego llama a [**QueryInterface**](https://msdn.microsoft.com/library/windows/desktop/ms682521) en dicho puntero para consultar la interfaz predeterminada del objeto C++/WinRT. **QueryInterface** es la interfaz binaria (ABI) de la aplicación de Windows Runtime equivalente a la extensión safe_cast ++/CX. Y la función [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera la dirección del puntero de interfaz **IUnknown** subyacente a un objeto C++/WinRT para que pueda establecerse en otro valor.

```cppwinrt
template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}
```

La siguiente función auxiliar convierte un objeto C++/WinRT en un objeto C++/CX equivalente. La función [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) recupera un puntero a la interfaz **IUnknown** subyacente a un objeto C++/WinRT. La función convierte dicho puntero en un objeto C++/CX antes de usar la extensión safe_cast C++/CX para consultar el tipo C++/ CX solicitado.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="code-example"></a>Ejemplo de código
Aquí tienes un ejemplo de código (basado en la plantilla de proyecto **Blank App** C++/CX) que muestra las dos funciones auxiliares en uso. También muestra cómo puedes usar los alias de espacio de nombre para las diferentes islas para abordar las posibles colisiones de espacio de nombres entre la proyección de C++/WinRT y la proyección de C++/CX.

```cppwinrt
// MainPage.xaml.cpp

#include "pch.h"
#include "MainPage.xaml.h"
#include <winrt/Windows.Foundation.h>
#include <sstream>

using namespace InteropExample;

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows::Foundation;
}

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(),
            reinterpret_cast<void**>(winrt::put_abi(to))));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}

MainPage::MainPage()
{
    InitializeComponent();

    winrt::init_apartment(winrt::apartment_type::single_threaded);

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);
}
```

## <a name="important-apis"></a>API importantes
* [Interfaz IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Función QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
