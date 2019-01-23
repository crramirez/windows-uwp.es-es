---
description: Este tema muestra dos funciones auxiliares que pueden usarse para convertir entre objetos C++/CX y C++/WinRT.
title: Interoperabilidad entre C++/WinRT y C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, estándar, c++, cpp, winrt, proyección, puerto, migar, interoperabilidad, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: e1e4570320e9d48351ccb01052fc77d35ae03642
ms.sourcegitcommit: 4a359aecafb73d73b5a8e78f7907e565a2a43c41
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "9024574"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidad entre C++/WinRT y C++/CX

Estrategias para migrar gradualmente el código de tu [C++ / CX](/cpp/cppcx/visual-c-language-reference-c-cx) proyecto a [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) se describen en [mover a C++ / WinRT desde C++ / CX](move-to-winrt-from-cx.md).

Este tema muestra dos funciones auxiliares que puedes usar para convertir entre C++ / CX y C++ / WinRT objetos en el mismo proyecto. Puede usarlos para interoperar entre el código que usa las dos proyecciones de lenguaje, o puedes usar las funciones como migrar el código de C++ / CX a C++ / WinRT.

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

## <a name="example-project-showing-the-two-helper-functions-in-use"></a>Proyecto de ejemplo que muestra las dos funciones auxiliares en uso

Para reproducir, de una manera sencilla, el escenario de migración gradualmente el código de C++ / proyecto CX a C++ / WinRT, puede comenzar creando un nuevo proyecto en Visual Studio usando uno de C++ / WinRT plantillas de proyecto (consulta [soporte de Visual Studio para C++ / WinRT y VSIX](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-and-the-vsix)).

Este proyecto de ejemplo también muestra cómo puedes usar los alias de espacio de nombres para las diferentes islas de código, con el fin de abordar las posibles colisiones de espacio de nombres entre la C++ / WinRT de proyección y C++ / proyección CX.

- Crear un **Visual C++** \> **Windows Universal** > **Core App (C++ / WinRT)** proyecto.
- En las propiedades del proyecto, **C/c ++** \> **General** \> **Usar extensión de Windows en tiempo de ejecución** \> **Sí (/ZW)**. Esto activa de soporte de proyecto para C++ / CX.
- Reemplaza el contenido del `App.cpp` con la siguiente lista de códigos.

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
* [Interfaz IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)
* [Función QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521)
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función winrt::put_abi](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Artículos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Migrar a C++/WinRT desde C++/CX](move-to-winrt-from-cx.md)
