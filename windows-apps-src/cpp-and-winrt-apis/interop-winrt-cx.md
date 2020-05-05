---
description: En este tema se muestran dos funciones auxiliares que pueden usarse para realizar la conversión entre objetos de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) y [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Interoperabilidad entre C++/WinRT y C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrate, interop, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 0e54937391d3317f1b37415036aabc88a6cfaa41
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "80290027"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidad entre C++/WinRT y C++/CX

Las estrategias para migrar gradualmente el código del proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) se tratan en [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md).

En este tema se muestran dos funciones auxiliares que pueden usarse para realizar la conversión entre objetos de C++/CX y C++/WinRT en el mismo proyecto. Puedes usarlos para interoperar entre el código que usa las dos proyecciones de lenguaje, o puedes usar las funciones a medida que migras el código de C++/CX a C++/WinRT.

## <a name="from_cx-and-to_cx-functions"></a>Funciones from_cx y to_cx
La siguiente función auxiliar convierte un objeto C++/CX en un objeto C++/WinRT equivalente. La función convierte un objeto C++/CX en su puntero a interfaz [**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown) subyacente. Luego llama a [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) en ese puntero para consultar la interfaz predeterminada del objeto de C++/WinRT. **QueryInterface** es la interfaz binaria (ABI) de la aplicación de Windows Runtime equivalente a la extensión safe_cast de C++/CX. Y la función [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera la dirección del puntero a interfaz **IUnknown** subyacente de un objeto de C++/WinRT para que pueda establecerse en otro valor.

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

La siguiente función auxiliar convierte un objeto de C++/WinRT en un objeto de C++/CX equivalente. La función [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) recupera un puntero **IUnknown** a la interfaz subyacente de un objeto de C++/WinRT. La función convierte ese puntero en un objeto de C++/CX antes de usar la extensión safe_cast de C++/CX para consultar el tipo de C++/CX solicitado.

```cppwinrt
template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Proyecto de ejemplo que muestra las dos funciones auxiliares en curso

Para reproducir de manera sencilla el escenario de migración gradual del código de un proyecto de C++/CX a C++/WinRT, puedes empezar por crear un proyecto en Visual Studio mediante una de las plantillas de C++/WinRT (consulta el artículo de [soporte de Visual Studio para C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).

Este proyecto de ejemplo también muestra cómo puedes usar los alias de espacio de nombre en las diferentes islas de código para abordar las posibles colisiones de espacios de nombres entre la proyección de C++/WinRT y la proyección de C++/CX.

- Crea un proyecto de **Visual C++** \> **Universal de Windows** > **Core App (C++/WinRT)** (Aplicación principal [C++/WinRT]).
- En las propiedades del proyecto, **C/C++**  \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)** . Esto activa el soporte del proyecto para C++/CX.
- Reemplaza el contenido de `App.cpp` por el código siguiente.

`WINRT_ASSERT` es una definición de macro y se expande a [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros).

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
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función winrt::put_abi function](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
