---
description: En este tema se muestran dos funciones auxiliares que pueden usarse para realizar la conversión entre objetos de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) y [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).
title: Interoperabilidad entre C++/WinRT y C++/CX
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, port, migrate, interop, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: c786256efb5488fff65a8e2bdb4c5d2ca0fa181c
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180800"
---
# <a name="interop-between-cwinrt-and-ccx"></a>Interoperabilidad entre C++/WinRT y C++/CX

Antes de leer este tema, necesitará la información del tema [Migrar a C++/WinRT desde C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx). En este tema se presentan dos opciones de estrategia principales para portar un proyecto de [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) a [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

- Porte todo el proyecto en un solo paso. La opción más sencilla para un proyecto que no es demasiado grande. Si tiene un proyecto de componentes de Windows Runtime, esta estrategia es la única opción.
- Porte el proyecto gradualmente (el tamaño o la complejidad del código base pueden hacer que sea necesario). Sin embargo, esta estrategia le exige seguir un proceso de portabilidad en el que, durante un momento, existe código de C++/CX y C++/WinRT en paralelo en el mismo proyecto. Para un proyecto XAML, en un momento dado, los tipos de página XAML deben estar, *o bien* todos en C++/WinRT, *o bien* todos en C++/CX.

Este tema de interoperabilidad es pertinente para esa *segunda* estrategia&mdash;para los casos en los que es necesario portar el proyecto gradualmente. En este tema se muestran dos funciones auxiliares que pueden usarse para convertir un objeto de C++/CX y en un objeto de C++/WinRT (y viceversa) en el mismo proyecto.

Estas funciones auxiliares serán muy útiles a medida que porte el código gradualmente de C++/CX a C++/WinRT. También puede optar por usar las proyecciones de lenguaje C++/WinRT y C++/CX en el mismo proyecto, independientemente de si está portando o no, y usar estas funciones auxiliares para interoperar entre las dos.

Después de leer este tema, para obtener información y códigos de ejemplo que muestren cómo se admiten las tareas y corrutinas de PPL en paralelo en el mismo proyecto (por ejemplo, llamando a corrutinas desde cadenas de tareas), consulte el tema más avanzado [Asincronía e interoperabilidad entre C++/WinRT y C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async).

## <a name="the-from_cx-and-to_cx-functions"></a>Funciones **from_cx** y **to_cx**

A continuación se muestra una lista de código fuente de un archivo de encabezado denominado `interop_helpers.h`, que contiene dos funciones auxiliares de conversión. En las secciones siguientes se explican las funciones y cómo crear y usar el archivo de encabezado en el proyecto.

```cppwinrt
// interop_helpers.h
#pragma once

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
```

### <a name="the-from_cx-function"></a>Función **from_cx**

La función auxiliar **from_cx** convierte un objeto de C++/CX en un objeto de C++/WinRT equivalente. La función convierte un objeto C++/CX en su puntero a interfaz [**IUnknown**](/windows/win32/api/unknwn/nn-unknwn-iunknown) subyacente. Luego llama a [**QueryInterface**](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) en ese puntero para consultar la interfaz predeterminada del objeto de C++/WinRT. **QueryInterface** es la interfaz binaria de aplicaciones (ABI) de Windows Runtime equivalente a la extensión `safe_cast` de C++/CX. Y la función [**winrt::put_abi**](/uwp/cpp-ref-for-winrt/put-abi) recupera la dirección del puntero a interfaz **IUnknown** subyacente de un objeto de C++/WinRT para que pueda establecerse en otro valor.

### <a name="the-to_cx-function"></a>Función **to_cx**

La función auxiliar **to_cx** convierte un objeto de C++/WinRT en un objeto de C++/CX equivalente. La función [**winrt::get_abi**](/uwp/cpp-ref-for-winrt/get-abi) recupera un puntero **IUnknown** a la interfaz subyacente de un objeto de C++/WinRT. La función convierte ese puntero en un objeto de C++/CX antes de usar la extensión `safe_cast` de C++/CX para consultar el tipo de C++/CX solicitado.

### <a name="the-interop_helpersh-header-file"></a>Archivo de encabezado `interop_helpers.h`

Para usar las dos funciones auxiliares en el proyecto, siga estos pasos.

- Agregue un nuevo elemento de **archivo de encabezado (.h)** al proyecto y asígnele el nombre `interop_helpers.h`.
- Reemplace el contenido de `interop_helpers.h` por la lista de código anterior.
- Agregue estos include a `pch.h`.

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>Adopción de un proyecto de C++/CX y adición de compatibilidad con C++/WinRT

En esta sección se describe qué hacer si ha decidido tomar el proyecto de C++/CX existente, agregarle compatibilidad con C++/WinRT y llevar a cabo el trabajo de portabilidad allí. Consulta también [Compatibilidad de Visual Studio para C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Para combinar C++/CX y C++/WinRT en un proyecto de C++/CX&mdash;incluido el uso de las funciones auxiliares **from_cx** y **to_cx** en el proyecto&mdash;debe agregar manualmente la compatibilidad con C++/WinRT al proyecto.

En primer lugar, abra el proyecto de C++/CX en Visual Studio y confirme que la propiedad de proyecto **General** \> **Versión de la plataforma de destino** esté establecida en 10.0.17134.0 (Windows 10, versión 1803) o posterior.

### <a name="install-the-cwinrt-nuget-package"></a>Instalación del paquete NuGet de C++/WinRT

El [paquete NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) ofrece compatibilidad de compilación con C++/WinRT (propiedades y destinos de MSBuild). Para instalarlo, haga clic en el elemento de menú **Proyecto** \> **Administrar paquetes NuGet…** \> **Busca**, escribe o pega **Microsoft.Windows.CppWinRT** en el cuadro de búsqueda, selecciona el elemento en los resultados de la búsqueda y haz clic en **Instalar** para instalar el paquete de ese proyecto.

> [!IMPORTANT]
> La instalación del paquete NuGet de C++/WinRT hace que la compatibilidad con C++/CX se desactive en el proyecto. Si va a portar en un solo paso, es conveniente dejar desactivada la compatibilidad para que los mensajes de compilación le ayuden a encontrar (y a portar) todas las dependencias de C++/CX (lo que, en última instancia, convertirá un proyecto puramente de C++/CX en un proyecto puramente de C++/WinRT). Pero consulte la siguiente sección para obtener información sobre cómo volver a activarla.

### <a name="turn-ccx-support-back-on"></a>Reactivación de la compatibilidad con C++/CX

Si va a portar en un solo paso, no es necesario que lo haga. Pero si tiene que portar gradualmente, en este momento deberá volver a activar la compatibilidad con C++/CX en el proyecto. En las propiedades del proyecto, **C/C++** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)** .

Como alternativa (o, para un proyecto XAML, además), puede agregar compatibilidad con C++/CX mediante la página de propiedades del proyecto de C++/WinRT en Visual Studio. En propiedades del proyecto, **Propiedades comunes** \> **C++/WinRT** \> **Lenguaje del proyecto** \> **C++/CX**. Al hacerlo, se agregará la siguiente propiedad al archivo `.vcxproj`.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

> [!IMPORTANT]
> Cuando tenga que procesar el contenido de un **archivo MIDL (.idl)** en archivos de código auxiliar, tendrá que cambiar **Lenguaje del proyecto** nuevamente a **C++/WinRT**. Una vez que la compilación haya generado esos códigos auxiliares, vuelva a cambiar **Lenguaje del proyecto** a **C++/CX**.

Para obtener una lista de opciones de personalización similares (que ajustan el comportamiento de la herramienta `cppwinrt.exe`), consulta [readme](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) del paquete NuGet Microsoft.Windows.CppWinRT.

### <a name="include-cwinrt-header-files"></a>Inclusión de archivos de encabezado de C++/WinRT

Lo mínimo que debe hacer es, en el archivo de encabezado precompilado (normalmente `pch.h`), incluir `winrt/base.h` como se muestra a continuación.

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

Sin embargo, seguramente necesitará los tipos en el espacio de nombres **winrt::Windows::Foundation**. Y es posible que ya conozca otros espacios de nombres que necesitará. Por lo tanto, incluya los encabezado de API de Windows proyectados de C++/WinRT en esos espacios de nombres de este modo (no necesita incluir explícitamente `winrt/base.h` ahora, porque se incluirá automáticamente).

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

Consulte también el ejemplo de código en la sección siguiente (*Adopción de un proyecto de C++/WinRT y adición de compatibilidad con C++/CX*) para ver una técnica mediante los alias de espacio de nombres `namespace cx` y `namespace winrt`. Esta técnica le permite abordar las que, de lo contrario, serían posibles colisiones de espacios de nombres la proyección de C++/WinRT y la proyección de C++/CX.

### <a name="add-interop_helpersh-to-the-project"></a>Adición de `interop_helpers.h` al proyecto

Ahora podrá agregar las funciones **from_cx** y **to_cx** al proyecto de C++/CX. Para obtener instrucciones sobre cómo hacerlo, consulte la sección [Funciones **from_cx** y **to_cx**](#the-from_cx-and-to_cx-functions) anterior.

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>Adopción de un proyecto de C++/WinRT y adición de compatibilidad con C++/CX

En esta sección se describe qué hacer si ha decidido crear un nuevo proyecto de C++/WinRT y llevar a cabo el trabajo de portabilidad allí.

Para combinar C++/WinRT y C++/CX en un proyecto de C++/WinRT&mdash;incluido el uso de las funciones auxiliares **from_cx** y **to_cx** en el proyecto&mdash;debe agregar manualmente la compatibilidad con C++/CX al proyecto.

- Cree un nuevo proyecto de C++/WinRT en Visual Studio mediante una de las plantillas de proyecto de C++/WinRT (consulte [Compatibilidad de Visual Studio con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)).
- Active la compatibilidad del proyecto para C++/CX. En las propiedades del proyecto, **C/C++ ** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)**.

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>Ejemplo de proyecto de C++/WinRT que muestra las dos funciones auxiliares en uso

En esta sección, puede crear un proyecto de C++/WinRT de ejemplo que muestra cómo usar **from_cx** y **to_cx**. También muestra cómo puedes usar los alias de espacio de nombre en las diferentes islas de código para abordar las posibles colisiones de espacios de nombres entre la proyección de C++/WinRT y la proyección de C++/CX.

- Crea un proyecto de **Visual C++** \> **Universal de Windows** \> **Core App (C++/WinRT)** (Aplicación principal [C++/WinRT]).
- En las propiedades del proyecto, **C/C++ ** \> **General** \> **Usar extensión de Windows Runtime** \> **Sí (/ZW)**.
- Agregue `interop_helpers.h` al proyecto. Para obtener instrucciones sobre cómo hacerlo, consulte la sección [Funciones **from_cx** y **to_cx**](#the-from_cx-and-to_cx-functions) anterior.
- Reemplaza el contenido de `App.cpp` por el listado de código siguiente, compile y ejecute.

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
* [Interfaz IUnknown](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [Función QueryInterface](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [Función winrt::get_abi](/uwp/cpp-ref-for-winrt/get-abi)
* [Función winrt::put_abi function](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>Temas relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Migrar a C++/WinRT desde C++/CX](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
* [Asincronía e interoperabilidad entre C++/WinRT y C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async)
