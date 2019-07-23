---
description: En este tema se usa un ejemplo de código completo de Direct2D para mostrar cómo utilizar C++/WinRT para consumir clases e interfaces COM.
title: Consumir componentes COM con C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, COM, component, class, interface
ms.localizationpriority: medium
ms.openlocfilehash: bb28ec7afa22f81033bfce2aff530119e53a4b91
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660157"
---
# <a name="consume-com-components-with-cwinrt"></a>Consumir componentes COM con C++/WinRT

Puedes utilizar las funciones de la biblioteca [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) para consumir los componentes COM, como los gráficos 2D y 3D de alto rendimiento de las API de DirectX. C++/WinRT es la manera más sencilla de usar DirectX sin comprometer el rendimiento. En este tema se usa un ejemplo de código completo de Direct2D para mostrar cómo utilizar C++/WinRT para consumir clases e interfaces COM. Por supuesto, puedes combinar la programación en COM y Windows Runtime dentro del mismo proyecto de C++/WinRT.

Al final de este tema, encontrarás una lista del código fuente completo de una aplicación de Direct2D mínima. Extraeremos partes de ese código y las usaremos para ilustrar cómo consumir componentes COM mediante C++/WinRT con varias facilidades de la biblioteca C++/WinRT.

## <a name="com-smart-pointers-winrtcomptruwpcpp-ref-for-winrtcom-ptr"></a>Punteros inteligentes de COM ([**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr))

Cuando se programa con COM, se trabaja directamente con interfaces en lugar de con objetos (esto también se puede aplicar para las API de Windows Runtime, que son una evolución de COM). Para llamar a una función en una clase COM, por ejemplo, tienes que activar la clase, volver a obtener una interfaz y, después, llamar a las funciones en esa interfaz. Para acceder al estado de un objeto, no se accede directamente a sus miembros de datos, sino que se llaman a las funciones de acceso y mutador en una interfaz.

Para ser más específico, hablamos de la interacción con los *punteros* de la interfaz. Y para ello, nos beneficiamos de la existencia del tipo de puntero inteligente de COM en C++/WinRT: el tipo [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr).

```cppwinrt
#include <d2d1_1.h>
...
winrt::com_ptr<ID2D1Factory1> factory;
```

En el código anterior se muestra cómo declarar un puntero inteligente no inicializado a una interfaz COM [**ID2D1Factory1**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1factory1). El puntero inteligente no está inicializado, por lo que aún no apunta a una interfaz **ID2D1D1Factory1** que pertenezca a un objeto real (no apunta a ninguna interfaz). Pero tienes la posibilidad de hacerlo; y, al ser un puntero inteligente, tienes la capacidad mediante un recuento de referencias COM de administrar el tiempo de vida del objeto propietario de la interfaz a la que apunta, y de ser el medio por el cual llamas a las funciones en esa interfaz.

## <a name="com-functions-that-return-an-interface-pointer-as-void"></a>Funciones COM que devuelven un puntero de interfaz como **void**

Puede llamar a la función [**com_ptr::put_void** ](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) para escribir en un puntero inteligente sin inicializar subyacente del puntero básico.

```cppwinrt
D2D1_FACTORY_OPTIONS options{ D2D1_DEBUG_LEVEL_NONE };
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    &options,
    factory.put_void()
);
```

El código anterior llama a la función [**D2D1CreateFactory**](/windows/desktop/api/d2d1/nf-d2d1-d2d1createfactory), que devuelve un puntero de interfaz **ID2D1Factory1** mediante su último parámetro, que tiene el tipo **void\*\*** . Muchas funciones COM devuelven un tipo **void\*\*** . Para esas funciones, utiliza [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function) tal como se muestra.

## <a name="com-functions-that-return-a-specific-interface-pointer"></a>Funciones COM que devuelven un puntero de interfaz específico

La función [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) devuelve un puntero de interfaz [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) mediante su tercer parámetro desde el último, que tiene el tipo **ID3D11Device\*\*** . Para las funciones que devuelven un puntero de interfaz específico como este, utiliza [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID3D11Device> device;
D3D11CreateDevice(
    ...
    device.put(),
    ...);
```

En el ejemplo de código de la sección anterior, se muestra cómo llamar a la función **D2D1CreateFactory** básica. Pero de hecho, cuando el ejemplo de código para este tema llama a **D2D1CreateFactory**, utiliza una plantilla de función auxiliar que envuelve la API básica, de modo que el ejemplo de código realmente utiliza [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function).

```cppwinrt
winrt::com_ptr<ID2D1Factory1> factory;
D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    options,
    factory.put());
```

## <a name="com-functions-that-return-an-interface-pointer-as-iunknown"></a>Funciones COM que devuelven un puntero de interfaz como **IUnknown**

La función [**DWriteCreateFactory**](/windows/desktop/api/dwrite/nf-dwrite-dwritecreatefactory) devuelve un puntero de interfaz del generador de DirectWrite mediante su último parámetro, que tiene el tipo [**IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Para usar dicha función, utiliza [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function), pero reinterpreta su conversión en **IUnknown**.

```cppwinrt
DWriteCreateFactory(
    DWRITE_FACTORY_TYPE_SHARED,
    __uuidof(dwriteFactory2),
    reinterpret_cast<IUnknown**>(dwriteFactory2.put()));
```

## <a name="re-seat-a-winrtcomptr"></a>Volver a sentar un **winrt::com_ptr**

> [!IMPORTANT]
> Si tienes una función [**winrt::com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr) que ya se ha sentado (el puntero básico interno ya tiene un destino) y quieres volver a sentarla para que señale a un objeto distinto, en primer lugar deberás asignar `nullptr` en esta, tal como se muestra en el ejemplo de código siguiente. Si no lo haces, una función **com_ptr** ya sentada llamará la atención sobre el problema (cuando llames a [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptr_put-function) o [**com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrput_void-function)) mediante la declaración de que el puntero interno no es null.

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

## <a name="handle-hresult-error-codes"></a>Control de los códigos de error HRESULT

Para comprobar el valor de un HRESULT devuelto desde una función COM e iniciar una excepción en el caso de que represente un código de error, llama a [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult).

```cppwinrt
winrt::check_hresult(D2D1CreateFactory(
    D2D1_FACTORY_TYPE_SINGLE_THREADED,
    __uuidof(factory),
    options,
    factory.put_void()));
```

## <a name="com-functions-that-take-a-specific-interface-pointer"></a>Funciones COM que toman un puntero de interfaz específico

Puedes llamar a la función [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrget-function) para pasar **com_ptr** a una función que toma un puntero de interfaz específico del mismo tipo.

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

## <a name="com-functions-that-take-an-iunknown-interface-pointer"></a>Funciones COM que toman un puntero de interfaz **IUnknown**

Puedes llamar a la función [**winrt::get_unknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#get_unknown-function) para pasar **com_ptr** a una función que toma un puntero de interfaz **IUnknown**.

```cppwinrt
winrt::check_hresult(factory->CreateSwapChainForCoreWindow(
    ...
    winrt::get_unknown(CoreWindow::GetForCurrentThread()),
    ...));
```

## <a name="passing-and-returning-com-smart-pointers"></a>Paso y devolución de punteros inteligentes COM

Una función que tome un puntero inteligente COM en forma de **winrt::com_ptr** debería hacerlo por referencia constante, o por referencia.

```cppwinrt
... GetDxgiFactory(winrt::com_ptr<ID3D11Device> const& device) ...

... CreateDevice(..., winrt::com_ptr<ID3D11Device>& device) ...
```

Una función que devuelve **winrt::com_ptr** debería hacerlo por valor.

```cppwinrt
winrt::com_ptr<ID2D1Factory1> CreateFactory() ...
```

## <a name="query-a-com-smart-pointer-for-a-different-interface"></a>Consulta de un puntero inteligente COM para otra interfaz

Puede usar la función [**com_ptr::as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptras-function) para consultar un puntero inteligente COM para otra interfaz. La función inicia una excepción si la consulta no se realiza correctamente.

```cppwinrt
void ExampleFunction(winrt::com_ptr<ID3D11Device> const& device)
{
    ...
    winrt::com_ptr<IDXGIDevice> const dxdevice{ device.as<IDXGIDevice>() };
    ...
}
```

Además, utiliza [**com_ptr::try_as**](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrtry_as-function), que devuelve un valor que puedes comparar con `nullptr` para ver si la consulta se ha realizado correctamente.

## <a name="full-source-code-listing-of-a-minimal-direct2d-application"></a>Lista del código fuente completo de una aplicación de Direct2D mínima

Si quieres compilar y ejecutar este ejemplo de código fuente, en primer lugar, en Visual Studio, crea una nueva **aplicación principal (C++/WinRT)** . `Direct2D` es un nombre razonable para el proyecto, pero puedes darle el nombre que quieras. Abre `App.cpp`, elimina todo su contenido y pega en la lista siguiente.

El código siguiente usa la [función winrt::com_ptr::capture](/uwp/cpp-ref-for-winrt/com-ptr#com_ptrcapture-function) siempre que sea posible. `WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

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
        factory.capture(adapter, &IDXGIAdapter::GetParent);
        return factory;
    }

    void CreateDeviceSwapChainBitmap(
        winrt::com_ptr<IDXGISwapChain1> const& swapchain,
        winrt::com_ptr<ID2D1DeviceContext> const& target)
    {
        WINRT_ASSERT(swapchain);
        WINRT_ASSERT(target);

        winrt::com_ptr<IDXGISurface> surface;
        surface.capture(swapchain, &IDXGISwapChain1::GetBuffer, 0);

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
    CoreApplication::Run(winrt::make<App>());
}
```

## <a name="working-with-com-types-such-as-bstr-and-variant"></a>Trabajo con tipos COM, como BSTR y VARIANT

Como puede ver, C++/WinRT proporciona compatibilidad tanto para implementar como para llamar interfaces COM. Para utilizar tipos COM, como BSTR y VARIANT, recomendamos que utilices los contenedores proporcionados por las [Bibliotecas de implementación de Windows (WIL)](https://github.com/Microsoft/wil), como **wil::unique_bstr** y **wil::unique_variant** (que administran la vigencia de los recursos).

[WIL](https://github.com/Microsoft/wil) reemplaza a plataformas como Active Template Library (ATL) y la compatibilidad de COM con el compilador de Visual C++. Y recomendamos sobrescribir sus propios contenedores, o el uso de tipos COM como BSTR y VARIANT en su forma básica (junto con las API apropiadas).

## <a name="avoiding-namespace-collisions"></a>Evitar conflictos de espacio de nombres

Es una práctica común en C++/WinRT &mdash;como se muestra en el listado de código de este tema&mdash; utilizar las directivas "using" libremente. Sin embargo, en algunos casos eso puede provocar el problema de importación de nombres en conflicto en el espacio de nombres global. A continuación te mostramos un ejemplo.

C++/WinRT contiene un tipo denominado [**winrt::Windows::Foundation::IUnknown**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown); mientras que COM define un tipo denominado [ **::IUnknown**](/windows/desktop/api/unknwn/nn-unknwn-iunknown). Por tanto, ten en cuenta el código siguiente, en un proyecto de C++/WinRT que consume encabezados de COM.

```cppwinrt
using namespace winrt::Windows::Foundation;
...
void MyFunction(IUnknown*); // error C2872:  'IUnknown': ambiguous symbol
```

El nombre no completo *IUnknown* entra en conflicto en el espacio de nombres global, de ahí el error de compilador *ambiguous symbol*. En su lugar, puedes aislar la versión de C++/WinRT del nombre en el espacio de nombres **winrt**, como se muestra a continuación.

```cppwinrt
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

O bien, si prefieres la comodidad de `using namespace winrt`, puedes usarlo. Basta calificar la versión global de *IUnknown*, como en este ejemplo.

```cppwinrt
using namespace winrt;
namespace winrt
{
    using namespace Windows::Foundation;
}
...
void MyFunctionA(::IUnknown*); // Ok.
void MyFunctionB(winrt::IUnknown const&); // Ok.
```

Naturalmente, esto funciona con cualquier espacio de nombres de C++/WinRT.

```cppwinrt
namespace winrt
{
    using namespace Windows::Storage;
    using namespace Windows::System;
}
```

A continuación, puedes hacer referencia a **winrt::Windows::Storage::StorageFile**, por ejemplo, simplemente como **winrt::StorageFile**.

## <a name="important-apis"></a>API importantes
* [Función winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Plantilla de estructura winrt::com_ptr](/uwp/cpp-ref-for-winrt/com-ptr)
* [Estructura winrt::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)
