---
author: stevewhims
description: Este tema usa un ejemplo de código completo de Direct2D para mostrar cómo usar C++ / WinRT para consumir COM clases e interfaces.
title: Consumir los componentes de COM con C++ / WinRT
ms.author: stwhi
ms.date: 07/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, estándar, c ++, cpp, winrt, COM, componente, clase, interfaz
ms.localizationpriority: medium
ms.openlocfilehash: 8af5a8149faab3bece62e4da5d41138aaede16e7
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "4310709"
---
# <a name="consume-com-components-with-cwinrt"></a>Consumir los componentes de COM con C++ / WinRT

Puedes usar las instalaciones de la [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) biblioteca consumir los componentes de COM, como los gráficos 2D y 3D de alto rendimiento de las APIs de DirectX. C++ / WinRT es la forma más sencilla de usar DirectX sin comprometer el rendimiento. Este tema usa un ejemplo de código de Direct2D para mostrar cómo usar C++ / WinRT para consumir COM clases e interfaces. Por supuesto, puede, mezclar la programación COM y en tiempo de ejecución de Windows dentro de la misma C++ / WinRT proyecto.

Al final de este tema, encontrarás una lista de código fuente completo de una aplicación mínima de Direct2D. Te enviaremos levantar extractos de ese código y usarlos para ilustrar cómo usar los componentes de COM con C++ / WinRT con varias instalaciones de C++ / WinRT biblioteca.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Punteros inteligentes COM ([**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Cuando programa con COM, trabajas directamente con interfaces en lugar de con objetos (que también de true en segundo plano para Windows en tiempo de ejecución APIs, que son una evolución del COM). Para llamar a una función en una clase de COM, por ejemplo, activar la clase, una interfaz atrás y, a continuación, llamamos funciones en esa interfaz. Para obtener acceso al estado de un objeto, no tengan acceso a sus miembros de datos directamente; en su lugar, se llama a las funciones de descriptor de acceso y mutación en una interfaz.

Para ser más específico, estamos hablando de interactuar con *punteros*de interfaz. Y para ello, se benefician de la existencia del tipo de puntero inteligente COM en C++ / WinRT&mdash;el tipo de [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) .

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
```

El código anterior muestra cómo declarar un puntero inteligente inicializado a una interfaz COM de [**ID2D1Factory1**](https://msdn.microsoft.com/library/Hh404596) . El puntero inteligente no está inicializado, por lo que todavía no señala a una interfaz **ID2D1Factory1** que pertenecen a cualquier objeto real (no señala a una interfaz en absoluto). Pero tiene el potencial de hacerlo; y (que se va a un puntero inteligente) tiene la capacidad de a través de COM recuento de referencias para administrar la duración del objeto de la interfaz que señala al propietario y para que sea el medio por el que llamar a funciones en esa interfaz.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Funciones de COM que devuelven un puntero de interfaz como **void**

Puedes llamar a la función [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) para escribir en el puntero sin procesar subyacente a un puntero de tarjeta inteligente sin inicializar.

```cppwinrt
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

El código anterior llama a la función [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory) , que devuelve un puntero de interfaz **ID2D1Factory1** a través de su último parámetro, que tiene **void\ * \ *** tipo. Muchas de las funciones de COM devuelven un **void\ * \ ***. Para estas funciones, usa [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) tal como se muestra.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funciones de COM que devuelven un puntero de interfaz específica

La función [**D3D11CreateDevice**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) devuelve un puntero de interfaz [**ID3D11Device**](https://msdn.microsoft.com/library/Hh404596) a través de su parámetro antepenultimate, que tiene **ID3D11Device\ * \ *** tipo. Para las funciones que devuelven un puntero de interfaz específica como ese, usa [**com_ptr:: Put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

El ejemplo de código en la sección antes de este proyecto muestra cómo llamar a la función **D2D1CreateFactory** sin procesar. Pero, de hecho, cuando el ejemplo de código de este tema, llama **D2D1CreateFactory**, usa una plantilla de función auxiliar que encapsula la API sin procesar y por lo tanto, el ejemplo de código usa realmente [**com_ptr:: Put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Funciones de COM que devuelven un puntero de interfaz **IUnknown**

La función [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) devuelve un puntero de interfaz de fábrica DirectWrite a través de su último parámetro, que tiene un tipo [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509) . Para esta función, uso [**com_ptr:: Put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function), pero reinterpretar convertirse **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Número de puestos volver a una **winrt:: com_ptr**

> [!IMPORTANT]
> Si tienes un [**winrt:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ya está instalada (su puntero sin procesar interno ya tiene un destino) y quieres volver a número de puestos para que apunte a un objeto diferente, a continuación, primero debes asignar `nullptr` a ella&mdash;tal como se muestra en el siguiente ejemplo de código. Si no lo haces, a continuación, un sentado ya **com_ptr** se dibujará el problema a tu atención (cuando se llame a [**com_ptr:: Put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) o [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)) mediante la aserción que su puntero interno no es null.

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

## <a name="handle-hresult-error-codes"></a>Controlar códigos de error HRESULT

Para comprobar que el valor de un valor de HRESULT devuelto desde una función de COM y generar una excepción en caso de que representa un código de error, llamar a [**winrt:: check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funciones de COM que tienen un puntero de interfaz específica

Puedes llamar a la función [**com_ptr:: Get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function) para pasar la **com_ptr** a una función que toma un puntero de interfaz específica del mismo tipo.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Funciones de COM que toman un puntero de interfaz **IUnknown**

Puedes llamar a la función libre de [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) para pasar la **com_ptr** a una función que toma un puntero de interfaz **IUnknown** .

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Pasar y devolver COM punteros inteligentes

Una función que toma un puntero inteligente de COM en forma de un **winrt:: com_ptr** debe hacerlo por referencia constante o referencia.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Una función que devuelva una **winrt:: com_ptr** debe hacerlo por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Consultar un puntero inteligente de COM para una interfaz diferente

Puedes usar la función [**com_ptr:: As**](/uwp/cpp-ref-for-winrt/com-ptr#comptras-function) para consultar un puntero inteligente de COM para una interfaz diferente. La función produce una excepción si la consulta no se realiza correctamente.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Como alternativa, usar [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#comptrtryas-function), que devuelve un valor que puede comprobar contra `nullptr` para ver si la consulta se realizó correctamente.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Listado de código fuente completo de una aplicación mínima de Direct2D

Si quieres compilar y ejecutar este ejemplo de código de origen, a continuación, en primer lugar, en Visual Studio, crea un nuevo **Core App (C++ / WinRT)**. `Direct2D` es un nombre razonable para el proyecto, pero puede asignar un nombre cualquier cosa que quieras. Abre `App.cpp`, eliminar todo su contenido y pegarlo en la siguiente lista.

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

        WINRT_TRACE("Draw %.2f x %.2f @ %.2f\n", size.width, size.height, m_dpi);
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

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Trabajar con tipos de COM, como BSTR y VARIANT

Como puedes ver, C++ / WinRT proporciona soporte técnico para implementar y llamar a las interfaces COM. Para el uso de tipos de COM, como BSTR y VARIANT, siempre hay la opción para usarlos en su forma sin procesar (junto con las API adecuadas). Como alternativa, puedes usar contenedores proporcionados por un marco de trabajo, como la [Biblioteca de plantilla Active (ATL)](/cpp/atl/active-template-library-atl-concepts), por el compilador de Visual C++ [Compatibilidad con COM](/cpp/cpp/compiler-com-support)o incluso por tus propios contenedores.

## <a name="important-apis"></a>API importantes
* [Función winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
