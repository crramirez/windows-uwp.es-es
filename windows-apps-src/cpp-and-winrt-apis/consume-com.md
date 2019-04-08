---
description: En este tema se usa un ejemplo de código completo de Direct2D para mostrar cómo utilizar C++/WinRT para consumir clases e interfaces COM.
title: Consumir componentes COM con C++ / WinRT
ms.date: 07/23/2018
ms.topic: article
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, COM, componente, clase, interfaz
ms.localizationpriority: medium
ms.openlocfilehash: 129477689e12de2634b422a0fc4487b283e3bf03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644810"
---
# <a name="consume-com-components-with-cwinrt"></a>Consumir componentes COM con C++ / WinRT

Puede usar las funciones de la [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) biblioteca para consumir componentes COM, como los gráficos 2D y 3D de alto rendimiento de las APIs de DirectX. C++ / c++ / WinRT es la manera más sencilla de usar DirectX sin comprometer el rendimiento. En este tema se usa un ejemplo de código de Direct2D para mostrar cómo usar C++ / c++ / WinRT para consumir las clases COM e interfaces. Por supuesto, puede mezclar la programación COM y en tiempo de ejecución de Windows en el mismo C++ / c++ / WinRT proyecto.

Al final de este tema, encontrará una lista de código fuente completo de una aplicación de Direct2D mínima. Se le elevación extractos de ese código y usarlos para ilustrar cómo consumir los componentes COM con C++ / c++ / WinRT con diversas funciones de C++ / c++ / WinRT biblioteca.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Punteros inteligentes de COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Al programar con COM, se trabaja directamente con las interfaces en lugar de con objetos (que también es true en segundo plano de Windows en tiempo de ejecución APIs, que es una evolución de COM). Para llamar a una función en una clase COM, por ejemplo, activar la clase, obtener una interfaz de nuevo y, a continuación, llamar a funciones en esa interfaz. Para obtener acceso al estado de un objeto, no acceso a sus miembros de datos directamente. en su lugar, llame a funciones de descriptor de acceso y mutador en una interfaz.

Para ser más específico, hablamos sobre la interacción con la interfaz *punteros*. Y para ello, se benefician de la existencia del tipo de puntero inteligente COM en C++ / c++ / WinRT&mdash;el [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) tipo.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

El código anterior muestra cómo declarar un puntero inteligente no inicializado a un [ **ID2D1Factory1** ](https://msdn.microsoft.com/library/Hh404596) interfaz COM. El puntero inteligente se ha inicializado, por lo que aún no está señalando a una **ID2D1Factory1** interfaz que pertenezca a cualquier objeto real (no está señalando a una interfaz en absoluto). Pero tiene la posibilidad de hacerlo; y (que se va a un puntero inteligente) tiene la capacidad a través de la referencia COM para administrar la vigencia del objeto propietario de la interfaz que apunta y que el medio por el que se llama a funciones en esa interfaz de recuento.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Las funciones de COM que devuelven un puntero de interfaz como **void**

Puede llamar a la [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) función para escribir en un puntero inteligente sin inicializar subyacente del puntero sin formato.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

El código anterior llama a la [ **D2D1CreateFactory** ](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) función, que devuelve un **ID2D1Factory1** puntero de interfaz a través de su último parámetro, que tiene **void \* \***  tipo. Muchas funciones COM devuelven un **void\*\***. Para esas funciones, use [ **com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) tal como se muestra.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funciones de COM que devuelven un puntero de interfaz específica

El [ **D3D11CreateDevice** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) función devuelve un [ **ID3D11Device** ](https://msdn.microsoft.com/library/Hh404596) puntero de interfaz a través de su parámetro antepenultimate, que tiene **ID3D11Device\* \***  tipo. Para las funciones que devuelven un puntero de interfaz específica como esa, use [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

El ejemplo de código en la sección antes de que éste muestra cómo llamar a sin formato **D2D1CreateFactory** función. Pero de hecho, cuando se llama el ejemplo de código de este tema **D2D1CreateFactory**, usa una plantilla de función auxiliar que encapsula la API sin formato y, por lo que realmente usa el ejemplo de código [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Las funciones de COM que devuelven un puntero de interfaz como **IUnknown**

El [ **DWriteCreateFactory** ](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) función devuelve un puntero de interfaz de fábrica de DirectWrite a través de su último parámetro, que tiene [ **IUnknown** ](https://msdn.microsoft.com/library/windows/desktop/ms680509)tipo. Para usar una función [ **com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function), pero reinterpretar que se va a convertir **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Volver a puestos un **winrt::com_ptr**

> [!IMPORTANT]
> Si tiene un [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) que ya está asentada (su puntero sin formato interno ya tiene un destino) y desea volver a puestos para que señale a un objeto diferente, a continuación, en primer lugar deberá asignar `nullptr` en él&mdash;tal como se muestra en el ejemplo de código siguiente. Si no lo hace, a continuación, un ya-asentada **com_ptr** dibujará el problema a su atención (cuando se llama a [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) o [ **com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) mediante la declaración de que el puntero interno no es null.

```cppwinrt
winrt::com_ptr<ID2D1SolidColorBrush> brush;
...
    brush.put()
...
brush = nullptr; // Important because we're about to re-seat
target->CreateSolidColorBrush(
    color_orange,
    D2D1::BrushProperties(0.8f),
    brush.put()));
```

## <a name="handle-hresult-error-codes"></a>Controlar los códigos de error HRESULT

Para comprobar el valor de un valor HRESULT devuelto desde una función COM y producir una excepción en caso de que representa un código de error, llame a [ **winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funciones de COM que tienen un puntero de interfaz específica

Puede llamar a la [ **com_ptr::get** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) función para pasar su **com_ptr** a una función que toma un puntero de interfaz específica del mismo tipo.

```cppwinrt
... ExampleFunction(
    winrt::com_ptr<ID2D1Factory1> const& factory,
    winrt::com_ptr<IDXGIDevice> const& dxdevice)
{
    ...
    winrt::check_hresult(factory->CreateDevice(dxdevice.get(), ...));
    ...
}
```

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>COM, las funciones que utilizan un **IUnknown** puntero de interfaz

Puede llamar a la [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) libre de la función para pasar su **com_ptr** a una función que toma un **IUnknown** interfaz puntero.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Pasar y devolver COM inteligente punteros

Una función que toma un puntero inteligente COM en forma de un **winrt::com_ptr** debe hacerlo por referencia constante, o por referencia.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Una función que devuelve un **winrt::com_ptr** debería hacerlo por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Consultar un puntero inteligente de COM para una interfaz diferente

Puede usar el [ **com_ptr::as** ](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) función para consultar un puntero inteligente de COM para una interfaz diferente. La función produce una excepción si la consulta no se realiza correctamente.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

También puede usar [ **com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function), que devuelve un valor que se puede comprobar con `nullptr` para ver si la consulta se realizó correctamente.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Lista de código fuente completo de una aplicación de Direct2D mínima

Si desea compilar y ejecutar este ejemplo de código fuente, a continuación, en primer lugar, en Visual Studio, cree un nuevo **Core App (C++ / c++ / WinRT)**. `Direct2D` es un nombre para el proyecto razonable, pero puede asignarle el nombre que desee. Abra `App.cpp`, elimine todo su contenido y pegue en la lista siguiente.

```cppwinrt
#include "pch.h"
#include <d2d1_1.h>
#include <d3d11.h>
#include <dxgi1_2.h>
#include <winrt/Windows.Graphics.Display.h>

using namespace winrt;

using namespace Windows;
using namespace Windows::ApplicationModel::Core;
using namespace Windows::UI;
using namespace Windows::UI::Core;
using namespace Windows::Graphics::Display;

namespace
{
    winrt::com_ptr<ID2D1Factory1> CreateFactory()
    {
        D2D1_FACTORY_OPTIONS options{};

#ifdef _DEBUG
        options.debugLevel = D2D1_DEBUG_LEVEL_INFORMATION;
#endif

        winrt::com_ptr<ID2D1Factory1> factory;

        winrt::check_hresult(D2D1CreateFactory(
            D2D1_FACTORY_TYPE_SINGLE_THREADED,
            options,
            factory.put()));

        return factory;
    }

    HRESULT CreateDevice(D3D_DRIVER_TYPE const type, winrt::com_ptr<ID3D11Device>& device)
    {
        WINRT_ASSERT(!device);

        return D3D11CreateDevice(
            nullptr,
            type,
            nullptr,
            D3D11_CREATE_DEVICE_BGRA_SUPPORT,
            nullptr, 0,
            D3D11_SDK_VERSION,
            device.put(),
            nullptr,
            nullptr);
    }

    winrt::com_ptr<ID3D11Device> CreateDevice()
    {
        winrt::com_ptr<ID3D11Device> device;
        HRESULT hr{ CreateDevice(D3D_DRIVER_TYPE_HARDWARE, device) };

        if (DXGI_ERROR_UNSUPPORTED == hr)
        {
            hr = CreateDevice(D3D_DRIVER_TYPE_WARP, device);
        }

        winrt::check_hresult(hr);
        return device;
    }

    winrt::com_ptr<ID2D1DeviceContext> CreateRenderTarget(
        winrt::com_ptr<ID2D1Factory1> const& factory,
        winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(factory);
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<ID2D1Device> d2device;
        winrt::check_hresult(factory->CreateDevice(dxdevice.get(), d2device.put()));

        winrt::com_ptr<ID2D1DeviceContext> target;
        winrt::check_hresult(d2device->CreateDeviceContext(D2D1_DEVICE_CONTEXT_OPTIONS_NONE, target.put()));
        return target;
    }

    winrt::com_ptr<IDXGIFactory2> GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };

        winrt::com_ptr<IDXGIAdapter> adapter;
        winrt::check_hresult(dxdevice->GetAdapter(adapter.put()));

        winrt::com_ptr<IDXGIFactory2> factory;
        winrt::check_hresult(adapter->GetParent(__uuidof(factory), factory.put_void()));
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        winrt::check_hresult(swapchain->GetBuffer(0, __uuidof(surface), surface.put_void()));

        D2D1_BITMAP_PROPERTIES1 const props{ D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_IGNORE)) };

        winrt::com_ptr<ID2D1Bitmap1> bitmap;

        winrt::check_hresult(target->CreateBitmapFromDxgiSurface(surface.get(),
            props,
            bitmap.put()));

        target->SetTarget(bitmap.get());
    }

    winrt::com_ptr<IDXGISwapChain1> CreateSwapChainForCoreWindow(winrt::com_ptr<ID3D11Device> const& device)
    {
        WINRT_ASSERT(device);

        winrt::com_ptr<IDXGIFactory2> const factory{ GetDxgiFactory(device) };

        DXGI_SWAP_CHAIN_DESC1 props{};
        props.Format = DXGI_FORMAT_B8G8R8A8_UNORM;
        props.SampleDesc.Count = 1;
        props.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        props.BufferCount = 2;
        props.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;

        winrt::com_ptr<IDXGISwapChain1> swapChain;

        winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
            device.get(),
            winrt::get_unknown(CoreWindow::GetForCurrentThread()),
            &props,
            nullptr, // all or nothing
            swapChain.put()));

        return swapChain;
    }

    constexpr D2D1_COLOR_F color_white{ 1.0f,  1.0f,  1.0f,  1.0f };
    constexpr D2D1_COLOR_F color_orange{ 0.92f,  0.38f,  0.208f,  1.0f };
}

struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::com_ptr<ID2D1Factory1> m_factory;
    winrt::com_ptr<ID2D1DeviceContext> m_target;
    winrt::com_ptr<IDXGISwapChain1> m_swapChain;
    winrt::com_ptr<ID2D1SolidColorBrush> m_brush;
    float m_dpi{};

    IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(CoreApplicationView const&)
    {
    }

    void Load(hstring const&)
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };

        window.SizeChanged([&](auto&&...)
        {
            if (m_target)
            {
                ResizeSwapChainBitmap();
                Render();
            }
        });

        DisplayInformation const display{ DisplayInformation::GetForCurrentView() };
        m_dpi = display.LogicalDpi();

        display.DpiChanged([&](DisplayInformation const& display, IInspectable const&)
        {
            if (m_target)
            {
                m_dpi = display.LogicalDpi();
                m_target->SetDpi(m_dpi, m_dpi);
                CreateDeviceSizeResources();
                Render();
            }
        });

        m_factory = CreateFactory();
        CreateDeviceIndependentResources();
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        CoreWindow const window{ CoreWindow::GetForCurrentThread() };
        window.Activate();

        Render();
        CoreDispatcher const dispatcher{ window.Dispatcher() };
        dispatcher.ProcessEvents(CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(CoreWindow const&) {}

    void Draw()
    {
        m_target->Clear(color_white);

        D2D1_SIZE_F const size{ m_target->GetSize() };
        D2D1_RECT_F const rect{ 100.0f, 100.0f, size.width - 100.0f, size.height - 100.0f };
        m_target->DrawRectangle(rect, m_brush.get(), 100.0f);

        char buffer[1024];
        (void)snprintf(buffer, sizeof(buffer), "Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
        ::OutputDebugStringA(buffer);
    }

    void Render()
    {
        if (!m_target)
        {
            winrt::com_ptr<ID3D11Device> const device{ CreateDevice() };
            m_target = CreateRenderTarget(m_factory, device);
            m_swapChain = CreateSwapChainForCoreWindow(device);

            CreateDeviceSwapChainBitmap(m_swapChain, m_target);

            m_target->SetDpi(m_dpi, m_dpi);

            CreateDeviceResources();
            CreateDeviceSizeResources();
        }

        m_target->BeginDraw();
        Draw();
        m_target->EndDraw();

        HRESULT const hr{ m_swapChain->Present(1, 0) };

        if (S_OK != hr && DXGI_STATUS_OCCLUDED != hr)
        {
            ReleaseDevice();
        }
    }

    void ReleaseDevice()
    {
        m_target = nullptr;
        m_swapChain = nullptr;

        ReleaseDeviceResources();
    }

    void ResizeSwapChainBitmap()
    {
        WINRT_ASSERT(m_target);
        WINRT_ASSERT(m_swapChain);

        m_target->SetTarget(nullptr);

        if (S_OK == m_swapChain->ResizeBuffers(0, // all buffers
            0, 0, // client area
            DXGI_FORMAT_UNKNOWN, // preserve format
            0)) // flags
        {
            CreateDeviceSwapChainBitmap(m_swapChain, m_target);
            CreateDeviceSizeResources();
        }
        else
        {
            ReleaseDevice();
        }
    }

    void CreateDeviceIndependentResources()
    {
    }

    void CreateDeviceResources()
    {
        winrt::check_hresult(m_target->CreateSolidColorBrush(
            color_orange,
            D2D1::BrushProperties(0.8f),
            m_brush.put()));
    }

    void CreateDeviceSizeResources()
    {
    }

    void ReleaseDeviceResources()
    {
        m_brush = nullptr;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    CoreApplication::Run(App());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Trabajar con tipos COM, como BSTR y VARIANT

Como puede ver, C++ / c++ / WinRT proporciona compatibilidad para implementar y llamar a las interfaces COM. Para utilizar tipos COM, como BSTR y VARIANT, siempre hay la opción Use en su formato sin procesar (junto con las API adecuadas). Como alternativa, puede usar los contenedores que proporcionan un marco de trabajo, como el [Active Template Library (ATL)](/cpp/atl/active-template-library-atl-concepts), o por el compilador de Visual C++ [compatibilidad con COM](/cpp/cpp/compiler-com-support), o incluso por sus propios contenedores.

## <a name="important-apis"></a>API importantes
* [función winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [struct winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
