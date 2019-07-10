---
description: En este tema se muestra cómo portar código WRL a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde WRL
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migrar, WRL
ms.localizationpriority: medium
ms.openlocfilehash: 2b987da5fb556d49953e462a5780290909734041
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63795709"
---
# <a name="move-to-cwinrt-from-wrl"></a>Migrar a C++/WinRT desde WRL
En este tema se muestra cómo portar código de la [Biblioteca de plantillas C++ de Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) a su equivalente en [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

El primer paso en la migración a C++/WinRT es agregar manualmente soporte C++/WinRT a tu proyecto (consulta [Soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para ello, instala el [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Abre el proyecto en Visual Studio, haz clic en **Proyecto** \> **Administrar paquetes NuGet...** \> **Busca**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto. Un efecto de ese cambio es que el soporte para [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) está desactivado en el proyecto. Si usas C++/CX en el proyecto, puedes dejar el soporte desactivado y actualizar tu código de C++/CX a C++/WinRT también (consulta [Mover a C++/ WinRT desde C++/CX](move-to-winrt-from-cx.md)). O bien puedes volver a activar el soporte (en las propiedades del proyecto, **C/C++** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)** ) y centrarte primero en portar tu código WRL. El código de C++/CX y C++/WinRT puede coexistir en el mismo proyecto, a excepción de la compatibilidad con el compilador XAML y los componentes de Windows Runtime (consulta [Mover a C++/ WinRT desde C++/CX](move-to-winrt-from-cx.md)).

Establece la propiedad de proyecto **General** \> **Versión de la plataforma de destino** en 10.0.17134.0 (Windows 10, versión 1803) o superior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` de este modo, porque se incluirá automáticamente para ti.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Portar punteros inteligentes COM WRL ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Porta cualquier código que use **Microsoft::WRL::ComPtr\<T\>** para que use [**winrt::com_ptr\<T\>** ](/uwp/cpp-ref-for-winrt/com-ptr). Aquí se muestra un ejemplo de código de "antes" y "después". En la versión de *después*, la función miembro [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) recupera el puntero sin procesar subyacente para que se pueda establecer.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Si tienes una función [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ya se ha asentado (es decir, el puntero interno sin procesar ya tiene un destino) y quieres volver a asentarla para que señale a un objeto distinto, en primer lugar deberás asignarle `nullptr`, tal como se muestra en el ejemplo de código siguiente. Si no lo haces, una función **com_ptr** ya asentada atraerá tu atención hacia el problema (cuando llames a [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput-function) o [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) mediante la declaración de que el puntero interno no es null.

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

En el ejemplo siguiente (en la versión de *después*), la función miembro [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) recupera el puntero sin procesar subyacente como un puntero a un puntero a void.

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

Reemplaza **ComPtr::Get** por [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Cuando quieras pasar el puntero sin procesar subyacente a una función que espera un puntero a **IUnknown**, usa la función gratuita [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function), tal como se muestra en el siguiente ejemplo.

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

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Portabilidad a un módulo WRL (Microsoft::WRL::Module)
Puedes agregar gradualmente código de C++/WinRT a un proyecto existente que usa WRL para implementar un componente y tus clases WRL existentes seguirán siendo compatibles. En esta sección se muestra cómo hacerlo.

Si creas un nuevo proyecto de **Componente de Windows Runtime (C++/WinRT)** en Visual Studio y lo compilas, se generará el archivo `Generated Files\module.g.cpp`. Este archivo contiene las definiciones de dos funciones útiles de C++/WinRT (que se enumeran debajo) que puedes copiar y agregar al proyecto. Esas funciones son **WINRT_CanUnloadNow** y **WINRT_GetActivationFactory** y, como puedes ver, llaman de forma condicional a WRL para darte soporte en cualquier fase de la portabilidad en la que te encuentres.

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

Una vez que tengas estas funciones en tu proyecto, en lugar de llamar a [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method) directamente, llama a **WINRT_GetActivationFactory** (que llama a la función WRL internamente). Aquí se muestra un ejemplo de código de "antes" y "después".

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

En lugar de llamar a [**Module::Terminate**](/cpp/windows/module-terminate-method) directamente, llama a **WINRT_CanUnloadNow** (que llama a la función WRL internamente). Aquí se muestra un ejemplo de código de "antes" y "después".

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
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Temas relacionados
* [Introducción a C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
* [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
