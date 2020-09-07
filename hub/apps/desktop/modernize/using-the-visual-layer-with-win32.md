---
title: Usar la capa visual con Win32
description: Obtenga información sobre cómo usar las API de Composition de Windows Runtime en la Plataforma universal de Windows (UWP), también conocida como la capa visual, para mejorar la interfaz de usuario de una aplicación de Win32 en C++.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, rendering, composition, win32
ms.author: jimwalk
author: jwmsft
ms.localizationpriority: medium
ms.openlocfilehash: 98811d890aa496e05c38009dedea6978386d5e4a
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043477"
---
# <a name="using-the-visual-layer-with-win32"></a>Usar la capa visual con Win32

Puedes usar las API de Composition de Windows Runtime (también llamada la [capa visual](/windows/uwp/composition/visual-layer)) en las aplicaciones de Win32 para crear experiencias modernas que atraigan a los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo HelloComposition de Win32](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Las aplicaciones de la Plataforma universal de Windows que necesitan un control preciso sobre su composición de la interfaz de usuario tienen acceso al espacio de nombres [Windows.UI.Composition](/uwp/api/windows.ui.composition) para ejercer un control preciso sobre cómo se compone y representa su interfaz de usuario. Sin embargo, esta API de Composition no se limita a las aplicaciones para UWP. Las aplicaciones de escritorio de Win32 pueden aprovechar las ventajas de los sistemas de composición modernos en UWP y Windows 10.

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de UWP tiene estos requisitos previos.

- Damos por hecho que estás familiarizado con el desarrollo de aplicaciones con Win32 y UWP. Para obtener más información, consulta:
  - [Introducción a Win32 y C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10, versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Cómo usar las API de Composition desde una aplicación de escritorio de Win32

En este tutorial, crearás una aplicación sencilla C++ de Win32 y le agregarás elementos de Composition de UWP. El objetivo es configurar correctamente el proyecto, crear el código de interoperabilidad y dibujar algo sencillo con las API de Composition de Windows. La aplicación finalizada tiene el siguiente aspecto.

![Interfaz de usuario de la aplicación en ejecución](images/visual-layer-interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Crear un proyecto de Win32 con C++ en Visual Studio

El primer paso es crear el proyecto de aplicación de Win32 en Visual Studio.

Para crear un nuevo proyecto de aplicación de Win32 en C++ llamado _HelloComposition_:

1. Abre Visual Studio y selecciona **Archivo** > **Nuevo** > **Proyecto**.

    Se abre el cuadro de diálogo **Nuevo proyecto**.
1. En la categoría **Instalados**, expande el nodo **Visual C++** y, después, selecciona **Escritorio de Windows**.
1. Selecciona la plantilla **Aplicación de escritorio de Windows**.
1. Escribe el nombre _HelloComposition_ y, a continuación, haz clic en **Aceptar**.

    Visual Studio crea el proyecto y abre el editor con el archivo de aplicación principal.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar las API de Windows Runtime

Para usar las API de Windows Runtime (WinRT) en la aplicación de C++ para Win32, usamos C++/WinRT. Tienes que configurar el proyecto de Visual Studio para agregar compatibilidad con C++/WinRT.

(Para más información, consulta [Introducción a C++a/WinRT: Modificar un proyecto de aplicación de escritorio de Windows para agregar compatibilidad con C++/WinRT](/windows/uwp/cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. En el menú **Proyecto**, abre las propiedades del proyecto (_Propiedades de HelloComposition_) y asegúrate de establecer las opciones siguientes en los valores especificados:

    - En **Configuración**, selecciona _Todas las configuraciones_. En **Plataforma**, selecciona _Todas las plataformas_.
    - **Propiedades de configuración** > **General** > **Windows SDK versión** = _10.0.17763.0_ o posterior

    ![Establecer la versión del SDK](images/visual-layer-interop/sdk-version.png)

    - **C/C++**  > **Lenguaje** > **Estándar de lenguaje C++**  = _ISO C++ 17 Standard (/stf:c++17)_

    ![Establecer el estándar del lenguaje](images/visual-layer-interop/language-standard.png)

    - En **Enlazador** > **Entrada** > **Dependencias adicionales** se debe incluir "_windowsapp.lib_". Si no está incluido en la lista, agrégalo.

    ![Agregar la dependencia del enlazador](images/visual-layer-interop/linker-dependencies.png)

1. Actualiza el encabezado precompilado.

    - Cambia el nombre de `stdafx.h` y `stdafx.cpp` a `pch.h` y `pch.cpp`, respectivamente.
    - Establece la propiedad del proyecto **C/C++**  > **Encabezados precompilados** > **Archivo de encabezado precompilado** en *pch.h*.
    - Busca y reemplaza `#include "stdafx.h"` por `#include "pch.h"` en todos los archivos.

        (**Editar** > **Buscar y reemplazar** > **Buscar en archivos**)

        ![Buscar y reemplazar stdafx h](images/visual-layer-interop/replace-stdafx.png)

    - En `pch.h`, incluye `winrt/base.h` y `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Es una buena idea compilar el proyecto en este momento para asegurarse de que no hay errores antes de continuar.

## <a name="create-a-class-to-host-composition-elements"></a>Crear una clase para hospedar elementos de Composition

Para hospedar el contenido que crea con la capa visual, crea una clase (_CompositionHost_) para administrar la interoperabilidad y crear elementos de Composition. Aquí es donde se realiza la mayoría de la configuración para hospedar las API de Composition, entre otra:

- Obtener [Compositor](/uwp/api/windows.ui.composition.compositor), que crea y administra los objetos del espacio de nombres [Windows.UI.Composition](/uwp/api/windows.ui.composition).
- Crear [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) para administrar las tareas de las API de WinRT.
- Crear [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) y un contenedor de Composition para mostrar los objetos de Composition.

Esta clase se convierte en singleton para evitar problemas de subprocesamiento. Por ejemplo, solo puedes crear una cola de distribuidor por subproceso, por lo que al crear instancias de una segunda instancia de CompositionHost en el mismo subproceso se produciría un error.

> [!TIP]
> Si es necesario, comprueba el código completo al final del tutorial para asegurarte de que todo el código está en el lugar correcto mientras trabajas en el tutorial.

1. Agrega un nuevo archivo de clase al proyecto.
    - En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto _HelloComposition_.
    - En el menú contextual, selecciona **Agregar** > **Clase...** .
    - En el cuadro de diálogo **Agregar clase**, asigna a la clase el nombre _CompositionHost.cs_ y, después, haz clic en **Agregar**.

1. Incluye encabezados y las declaraciones _using_ necesarias para la interoperabilidad de Composition.
    - En CompositionHost.h, agrega estas instrucciones _include_ en la parte superior del archivo.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - En CompositionHost.cpp, agrega estas instrucciones _using_ en la parte superior del archivo, después de cualquier instrucción _include_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Edita la clase para que use el patrón singleton.
    - En CompositionHost.h, haz que el constructor sea privado.
    - Declara un método _GetInstance_ público estático.

    ```cppwinrt
    class CompositionHost
    {
    public:
        ~CompositionHost();
        static CompositionHost* GetInstance();

    private:
        CompositionHost();
    };
    ```

    - En CompositionHost.cpp, agrega la definición del método _GetInstance_.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. En CompositionHost.h, declara las variables de miembro privado para Compositor, DispatcherQueueController y DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Agrega un método público para inicializar los objetos de interoperabilidad de Composition.
    > [!NOTE]
    > En _Initialize_, se llama a los métodos _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_ y _CreateCompositionRoot_. Estos métodos se crean en los pasos siguientes.

    - En CompositionHost.h, declara un método público llamado _Initialize_ que toma un HWND como argumento.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - En CompositionHost.cpp, agrega la definición del método _Initialize_.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Crea una cola de distribuidores en el subproceso que va a usar Windows Composition.

    Se debe crear un compositor en un subproceso que tenga una cola de distribución, por lo que se llama primero a este método durante la inicialización.

    - En CompositionHost.h, declara un método privado llamado _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - En CompositionHost.cpp, agrega la definición del método _EnsureDispatcherQueue_.

    ```cppwinrt
    void CompositionHost::EnsureDispatcherQueue()
    {
        namespace abi = ABI::Windows::System;

        if (m_dispatcherQueueController == nullptr)
        {
            DispatcherQueueOptions options
            {
                sizeof(DispatcherQueueOptions), /* dwSize */
                DQTYPE_THREAD_CURRENT,          /* threadType */
                DQTAT_COM_ASTA                  /* apartmentType */
            };

            Windows::System::DispatcherQueueController controller{ nullptr };
            check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
            m_dispatcherQueueController = controller;
        }
    }
    ```

1. Registra la ventana de la aplicación como un destino de Composition.
    - En CompositionHost.h, declara un método privado llamado _CreateDesktopWindowTarget_ que toma un HWND como argumento.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - En CompositionHost.cpp, agrega la definición del método _CreateDesktopWindowTarget_.

    ```cppwinrt
    void CompositionHost::CreateDesktopWindowTarget(HWND window)
    {
        namespace abi = ABI::Windows::UI::Composition::Desktop;

        auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
        DesktopWindowTarget target{ nullptr };
        check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
        m_target = target;
    }
    ```

1. Cree un contenedor visual raíz para incluir los objetos visuales.
    - En CompositionHost.h, declara un método privado llamado _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - En CompositionHost.cpp, agrega la definición del método _CreateCompositionRoot_.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 124, 12, 0 });
        m_target.Root(root);
    }
    ```

Compila el proyecto ahora para asegurarte de que no haya errores.

Estos métodos configuran los componentes necesarios para la interoperabilidad entre la capa visual de UWP y las API de Win32. Ahora puedes agregar contenido a la aplicación.

### <a name="add-composition-elements"></a>Agregar elementos de Composition

Una vez implementada la infraestructura, ahora puedes generar el contenido de Composition que quieres mostrar.

En este ejemplo, se agrega código que crea un cuadrado de color aleatorio [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual) con una animación que hace caiga después de un breve período de tiempo.

1. Agrega un elemento de Composition.
    - En CompositionHost.h, declara un método público llamado _AddElement_ que toma 3 valores **Float** como argumentos.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - En CompositionHost.cpp, agrega la definición del método _AddElement_.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            auto element = m_compositor.CreateSpriteVisual();
            uint8_t r = (double)(double)(rand() % 255);;
            uint8_t g = (double)(double)(rand() % 255);;
            uint8_t b = (double)(double)(rand() % 255);;

            element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
            element.Size({ size, size });
            element.Offset({ x, y, 0.0f, });

            auto animation = m_compositor.CreateVector3KeyFrameAnimation();
            auto bottom = (float)600 - element.Size().y;
            animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

            using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

            std::chrono::seconds duration(2);
            std::chrono::seconds delay(3);

            animation.Duration(timeSpan(duration));
            animation.DelayTime(timeSpan(delay));
            element.StartAnimation(L"Offset", animation);
            visuals.InsertAtTop(element);

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Crear y mostrar la ventana

Ahora, puedes agregar un botón y el contenido de Composition de UWP a la interfaz de usuario de Win32.

1. En HelloComposition.cpp, en la parte superior del archivo, incluye _CompositionHost.h_, define BTN_ADD y obtén una instancia de CompositionHost.

    ```cppwinrt
    #include "CompositionHost.h"

    // #define MAX_LOADSTRING 100 // This is already in the file.
    #define BTN_ADD 1000

    CompositionHost* compHost = CompositionHost::GetInstance();
    ```

1. En el método `InitInstance`, cambia el tamaño de la ventana que se crea. (En esta línea, cambia `CW_USEDEFAULT, 0` a `900, 672`).

    ```cppwinrt
    HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
        CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);
    ```

1. En la función WndProc, agrega `case WM_CREATE` al bloque switch _message_. En este caso, inicializas CompositionHost y creas el botón.

    ```cppwinrt
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
    ```

1. También en la función WndProc, controla el clic del botón para agregar un elemento de Composition a la interfaz de usuario. 

    Agrega `case BTN_ADD` al bloque switch _wmId_ dentro del bloque WM_COMMAND.

    ```cppwinrt
    case BTN_ADD: // addButton click
    {
        double size = (double)(rand() % 150 + 50);
        double x = (double)(rand() % 600);
        double y = (double)(rand() % 200);
        compHost->AddElement(size, x, y);
        break;
    }
    ```

Ahora puedes compilar y ejecutar la aplicación. Si es necesario, comprueba el código completo al final del tutorial para asegurarte de que todo el código está en el lugar correcto.

Al ejecutar la aplicación y hacer clic en el botón, verás que se agregan los cuadrados animados a la interfaz de usuario.

## <a name="additional-resources"></a>Recursos adicionales

- [Ejemplo HelloComposition de Win32 (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Introducción a Win32 y C++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Introducción a las aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejorar tu aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este es el código completo de la clase CompositionHost y el método InitInstance.

### <a name="compositionhosth"></a>CompositionHost.h

```cppwinrt
#pragma once
#include <winrt/Windows.UI.Composition.Desktop.h>
#include <windows.ui.composition.interop.h>
#include <DispatcherQueue.h>

class CompositionHost
{
public:
    ~CompositionHost();
    static CompositionHost* GetInstance();

    void Initialize(HWND hwnd);
    void AddElement(float size, float x, float y);

private:
    CompositionHost();

    void CreateDesktopWindowTarget(HWND window);
    void EnsureDispatcherQueue();
    void CreateCompositionRoot();

    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
};
```

### <a name="compositionhostcpp"></a>CompositionHost.cpp

```cppwinrt
#include "pch.h"
#include "CompositionHost.h"

using namespace winrt;
using namespace Windows::System;
using namespace Windows::UI;
using namespace Windows::UI::Composition;
using namespace Windows::UI::Composition::Desktop;
using namespace Windows::Foundation::Numerics;

CompositionHost::CompositionHost()
{
}

CompositionHost* CompositionHost::GetInstance()
{
    static CompositionHost instance;
    return &instance;
}

CompositionHost::~CompositionHost()
{
}

void CompositionHost::Initialize(HWND hwnd)
{
    EnsureDispatcherQueue();
    if (m_dispatcherQueueController) m_compositor = Compositor();

    if (m_compositor)
    {
        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
}

void CompositionHost::EnsureDispatcherQueue()
{
    namespace abi = ABI::Windows::System;

    if (m_dispatcherQueueController == nullptr)
    {
        DispatcherQueueOptions options
        {
            sizeof(DispatcherQueueOptions), /* dwSize */
            DQTYPE_THREAD_CURRENT,          /* threadType */
            DQTAT_COM_ASTA                  /* apartmentType */
        };

        Windows::System::DispatcherQueueController controller{ nullptr };
        check_hresult(CreateDispatcherQueueController(options, reinterpret_cast<abi::IDispatcherQueueController**>(put_abi(controller))));
        m_dispatcherQueueController = controller;
    }
}

void CompositionHost::CreateDesktopWindowTarget(HWND window)
{
    namespace abi = ABI::Windows::UI::Composition::Desktop;

    auto interop = m_compositor.as<abi::ICompositorDesktopInterop>();
    DesktopWindowTarget target{ nullptr };
    check_hresult(interop->CreateDesktopWindowTarget(window, false, reinterpret_cast<abi::IDesktopWindowTarget**>(put_abi(target))));
    m_target = target;
}

void CompositionHost::CreateCompositionRoot()
{
    auto root = m_compositor.CreateContainerVisual();
    root.RelativeSizeAdjustment({ 1.0f, 1.0f });
    root.Offset({ 124, 12, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        auto element = m_compositor.CreateSpriteVisual();
        uint8_t r = (double)(double)(rand() % 255);;
        uint8_t g = (double)(double)(rand() % 255);;
        uint8_t b = (double)(double)(rand() % 255);;

        element.Brush(m_compositor.CreateColorBrush({ 255, r, g, b }));
        element.Size({ size, size });
        element.Offset({ x, y, 0.0f, });

        auto animation = m_compositor.CreateVector3KeyFrameAnimation();
        auto bottom = (float)600 - element.Size().y;
        animation.InsertKeyFrame(1, { element.Offset().x, bottom, 0 });

        using timeSpan = std::chrono::duration<int, std::ratio<1, 1>>;

        std::chrono::seconds duration(2);
        std::chrono::seconds delay(3);

        animation.Duration(timeSpan(duration));
        animation.DelayTime(timeSpan(delay));
        element.StartAnimation(L"Offset", animation);
        visuals.InsertAtTop(element);

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp-partial"></a>HelloComposition.cpp (parcial)

```cppwinrt
#include "pch.h"
#include "HelloComposition.h"
#include "CompositionHost.h"

#define MAX_LOADSTRING 100
#define BTN_ADD 1000

CompositionHost* compHost = CompositionHost::GetInstance();

// Global Variables:

// ...
// ... code not shown ...
// ...

BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, 900, 672, nullptr, nullptr, hInstance, nullptr);

// ...
// ... code not shown ...
// ...
}

// ...

LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
{
    switch (message)
    {
// Add this...
    case WM_CREATE:
    {
        compHost->Initialize(hWnd);
        srand(time(nullptr));

        CreateWindow(TEXT("button"), TEXT("Add element"),
            WS_VISIBLE | WS_CHILD | BS_PUSHBUTTON,
            12, 12, 100, 50,
            hWnd, (HMENU)BTN_ADD, nullptr, nullptr);
    }
    break;
// ...
    case WM_COMMAND:
    {
        int wmId = LOWORD(wParam);
        // Parse the menu selections:
        switch (wmId)
        {
        case IDM_ABOUT:
            DialogBox(hInst, MAKEINTRESOURCE(IDD_ABOUTBOX), hWnd, About);
            break;
        case IDM_EXIT:
            DestroyWindow(hWnd);
            break;
// Add this...
        case BTN_ADD: // addButton click
        {
            double size = (double)(rand() % 150 + 50);
            double x = (double)(rand() % 600);
            double y = (double)(rand() % 200);
            compHost->AddElement(size, x, y);
            break;
        }
// ...
        default:
            return DefWindowProc(hWnd, message, wParam, lParam);
        }
    }
    break;
    case WM_PAINT:
    {
        PAINTSTRUCT ps;
        HDC hdc = BeginPaint(hWnd, &ps);
        // TODO: Add any drawing code that uses hdc here...
        EndPaint(hWnd, &ps);
    }
    break;
    case WM_DESTROY:
        PostQuitMessage(0);
        break;
    default:
        return DefWindowProc(hWnd, message, wParam, lParam);
    }
    return 0;
}

// ...
// ... code not shown ...
// ...
```