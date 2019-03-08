---
description: En este tema se muestra cómo migrar código WRL a su equivalente en C++/WinRT.
title: Migrar a C++/WinRT desde WRL
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e81f82fe823ee0fdf81741c89576adf268940d91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630750"
---
# <a name="move-to-cwinrt-from-wrl"></a>Migrar a C++/WinRT desde WRL
Este tema muestra cómo trasladar [biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) código a su equivalente de [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

El primer paso para migrar a C++ / c++ / WinRT es agregar manualmente C++ / c++ / WinRT soporte a su proyecto (vea [compatibilidad con Visual Studio C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para ello, instale el [paquete Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) en el proyecto. Haga clic en el proyecto en Visual Studio, abra **proyecto** \> **administrar paquetes NuGet...** \> **Examinar**, escriba o pegue **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, seleccione el elemento en los resultados de búsqueda y, a continuación, haga clic en **instalar** para instalar el paquete para ese proyecto. Un efecto de este cambio es que la compatibilidad de [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) está desactivada en el proyecto. Si usas C++/CX en el proyecto, puedes dejar el soporte desactivado y actualizar tu código de C++/CX a C++/WinRT también (consulta [Mover a C++/ WinRT desde C++/CX](move-to-winrt-from-cx.md)). O se puede volver a activar soporte técnico (en las propiedades del proyecto, **C o C++** \> **General** \> **usar extensión de Windows en tiempo de ejecución** \> **Sí (/ZW)**) y el primer enfoque para portar el código WRL. C++ / c++ / CX y c++ / WinRT código puede coexistir en el mismo proyecto, a excepción de compatibilidad con el compilador XAML y componentes de Windows en tiempo de ejecución (consulte [mover a C++ / c++ / WinRT en C++ / c++ / CX](move-to-winrt-from-cx.md)).

Establezca la propiedad del proyecto **General** \> **versión de la plataforma de destino** a 10.0.17134.0 (Windows 10, versión 1803) o superior.

En el archivo de encabezado precompilado (normalmente `pch.h`), incluye `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Si incluyes cualquier encabezado de API de Windows proyectado de C++/ WinRT (por ejemplo, `winrt/Windows.Foundation.h`), no necesitas incluir explícitamente `winrt/base.h` así porque se incluirá automáticamente para ti.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Migrar punteros inteligentes COM WRL ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Cualquier código que usa el puerto **Microsoft::WRL::ComPtr\<T\>**  usar [ **winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr). A continuación se muestra un ejemplo de código de "antes" y "después". En la versión de *después*, la función miembro [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) recupera el puntero sin procesar subyacente para que se puede establecer.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Si tiene un [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) que ya está asentada (su puntero sin formato interno ya tiene un destino) y desea volver a puestos para que señale a un objeto diferente, a continuación, en primer lugar deberá asignar `nullptr` en él&mdash;tal como se muestra en el ejemplo de código siguiente. Si no lo hace, a continuación, un ya-asentada **com_ptr** dibujará el problema a su atención (cuando se llama a [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) o [ **com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) mediante la declaración de que el puntero interno no es null.

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

Cuando desee pasar el puntero sin formato subyacente a una función que espera un puntero a **IUnknown**, utilice el [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) libre de función, como se muestra en la siguiente ejemplo.

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

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Migración de un módulo WRL (Microsoft::WRL::Module)
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
* [winrt::com_ptr struct template](/uwp/cpp-ref-for-winrt/com-ptr)
* [struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Temas relacionados
* [Introducción a C++ / c++ / WinRT](intro-to-using-cpp-with-winrt.md)
* [Mover a C++ / c++ / WinRT en C++ / c++ / CX](move-to-winrt-from-cx.md)
* [Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
