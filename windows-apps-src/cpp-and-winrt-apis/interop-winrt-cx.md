---
description: Este tema muestra dos funciones auxiliares que pueden usarse para convertir entre objetos C++/CX y C++/WinRT.
title: Interoperabilidad entre C++/WinRT y C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, interoperabilidad, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 5394443b4832864e5b46bfbf917c04f0af6d8a19
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66360216"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidad entre C++/WinRT y C++/CX

Estrategias para migrar gradualmente el código en su [C++ / c++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) proyecto a [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) se tratan en [mover a C++ / c++ / WinRT en C++ / c++ / CX](move-to-winrt-from-cx.md).

Este tema muestra dos funciones auxiliares que se pueden utilizar para convertir entre C++ / c++ / CX y c++ / WinRT objetos dentro del mismo proyecto. Puede usar para la interoperabilidad entre el código que usa las proyecciones de lenguaje de dos, o puede utilizar las funciones como portar su código desde C++ / c++ / CX C / c++ / WinRT.

## <a name="fromcx-and-tocx-functions"></a>Funciones from_cx y to_cx
La siguiente función auxiliar convierte un objeto C++/CX en un objeto C++/WinRT equivalente. La función convierte un objeto C++/CX en su puntero de interfaz [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) subyacente. Luego llama a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) en dicho puntero para consultar la interfaz predeterminada del objeto C++/WinRT. **QueryInterface** es la interfaz binaria (ABI) de la aplicación de Windows Runtime equivalente a la extensión safe_cast ++/CX. Y la función [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera la dirección del puntero de interfaz **IUnknown** subyacente a un objeto C++/WinRT para que pueda establecerse en otro valor.

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

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Proyecto de ejemplo que muestra las dos funciones auxiliares en uso

Reproducir, de una manera sencilla, el escenario de migración gradualmente el código en C++ / c++ / CX proyecto C++ / c++ / WinRT, puede empezar creando un nuevo proyecto en Visual Studio mediante uno de C++ / c++ / WinRT plantillas de proyecto (vea [compatibilidad con Visual Studio C++ / c++ / WinRT ](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Este proyecto de ejemplo también muestra cómo puede usar alias de espacio de nombres para las diferentes islas de código, con el fin de tratar de lo contrario posibles conflictos de espacio de nombres entre C++ / c++ / WinRT de proyección y C++ / c++ / proyección de CX.

- Crear un **Visual C++** \> **Windows Universal** > **Core App (C++ / c++ / WinRT)** proyecto.
- En las propiedades del proyecto, **C o C++** \> **General** \> **usar extensión de Windows en tiempo de ejecución** \> **Sí (/ZW)** . Esto activa el proyecto compatibilidad con C++ / c++ / CX.
- Reemplace el contenido de `App.cpp` con la lista de código siguiente.

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
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

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

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

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>API importantes
* [Interfaz IUnknown](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)
* [Función QueryInterface](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi function](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi function](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
