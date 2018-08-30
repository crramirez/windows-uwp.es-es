---
author: stevewhims
description: En este tema se muestra cómo migrar código WRL a su equivalente en C++/WinRT.
title: Mover a C++/WinRT desde WRL
ms.author: stwhi
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 3b9afd50b2c360c11b103f676b91d512881eaa71
ms.sourcegitcommit: 7efffcc715a4be26f0cf7f7e249653d8c356319b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "3117492"
---
# <a name="move-to-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt-from-wrl"></a>Mover a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) desde WRL
En este tema se muestra cómo migrar código de la [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) a su equivalente en C++/WinRT.

El primer paso en la migración a C++/WinRT es agregar manualmente soporte C++/WinRT a tu proyecto (consulta [Soporte de Visual Studio para C++/WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)). Para ello, edita tu archivo `.vcxproj`, encuentra `<PropertyGroup Label="Globals">` y, dentro de ese grupo de propiedades, establece la propiedad `<CppWinRTEnabled>true</CppWinRTEnabled>`. Un efecto de ese cambio es que el soporte para [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) está desactivado en el proyecto. Si usas C++/CX en el proyecto, puedes dejar el soporte desactivado y actualizar tu código de C++/CX a C++/WinRT también (consulta [Mover a C++/ WinRT desde C++/CX](move-to-winrt-from-cx.md)). O bien, puedes volver a activar el soporte (en las propiedades del proyecto, **C/C++** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)**) y centrarte primero en migrar tu código WRL. C++ / CX y C++ / WinRT código puede coexistir en el mismo proyecto, a excepción de soporte técnico de compilador XAML y componentes de Windows Runtime (consulta [mover a C++ / WinRT desde C++ / CX](move-to-winrt-from-cx.md)).

Establece la propiedad de proyecto **General** \> **Versión de la plataforma de destino** en 10.0.17134.0 (Windows 10, versión 1803) o superior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así porque se incluirá automáticamente para ti.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Migrar punteros inteligentes COM WRL ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Migra cualquier código que use **Microsoft::WRL::ComPtr\<T\>** para que use [**winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr). A continuación se muestra un ejemplo de código de "antes" y "después". En la versión de *después*, la función miembro [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) recupera el puntero sin procesar subyacente para que se puede establecer.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Si tienes un [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ya está instalada (su puntero sin procesar interno ya tiene un objetivo) y quieres volver a número de puestos para que señale a un objeto diferente, a continuación, primero debes asignar `nullptr` a ella&mdash;tal como se muestra en el siguiente ejemplo de código. Si no lo haces, a continuación, un sentado ya **com_ptr** dibujarán el problema a tu atención (cuando se llame a [**com_ptr:: Put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) o [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) por una aserción que su puntero interno no es "null".

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

En el ejemplo siguiente (en la versión de *después*), la función miembro [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) recupera el puntero sin procesar subyacente como un puntero a un puntero a void.

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

Reemplaza **ComPtr::Get** por [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Cuando quieres pasar el puntero sin procesar subyacente a una función que espera un puntero a **IUnknown**, usa la función libre [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) , como se muestra en el siguiente ejemplo.

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Migrar a un módulo WRL ([Microsoft:: WRL::Module]())
Puedes agregar gradualmente código C++/WinRT a un proyecto existente que usa WRL para implementar un componente y tus clases WRL existentes seguirán siendo compatibles. En esta sección se muestra cómo hacerlo.

Si creas un nuevo proyecto de **Componente de Windows Runtime (C++/WinRT)** en Visual Studio, y compilación, se generará el archivo `Generated Files\module.g.cpp`. Este archivo contiene las definiciones de dos funciones útiles de C++/WinRT (que se enumeran a continuación), que puedes copiar y agregar al proyecto. Esas funciones son **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** y, como puedes ver, llaman de forma condicional a WRL para darte soporte en cualquier fase de la migración en la que te encuentres.

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

Una vez que tengas estas funciones en tu proyecto, en lugar de llamar a [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method) directamente, llama a **WINRT_GetActivationFactory** (que llama a la función WRL internamente). A continuación se muestra un ejemplo de código de "antes" y "después".

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

En lugar de llamar a [**Module::Terminate**](/cpp/windows/module-terminate-method) directamente, llama a **WINRT_CanUnloadNow** (que llama a la función WRL internamente). A continuación se muestra un ejemplo de código de "antes" y "después".

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>API importantes
* [winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Temas relacionados
* [Introducción a C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
* [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
