---
title: Uso de la capa Visual con Win32
description: Utilice la capa Visual para mejorar la interfaz de usuario de la aplicación de escritorio de Win32.
template: detail.hbs
ms.date: 03/18/2019
ms.topic: article
keywords: UWP, representación, composición, win32
ms.localizationpriority: medium
ms.openlocfilehash: cfaa0d19b7a7361c5d604636c30beda0f416d063
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58174177"
---
# <a name="using-the-visual-layer-with-win32"></a>Uso de la capa Visual con Win32

Puede usar las API de composición de Windows en tiempo de ejecución (también denominado el [capa Visual](visual-layer.md)) en las aplicaciones de Win32 para crear experiencias modernas es clara para los usuarios de Windows 10.

El código completo de este tutorial está disponible en GitHub: [Ejemplo de Win32 HelloComposition](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition).

Aplicaciones universales de Windows que necesitan un control preciso sobre su composición de interfaz de usuario tiene acceso a la [Windows.UI.Composition](/uwp/api/windows.ui.composition) espacio de nombres para ejercer el control más preciso sobre cómo se compone y presenta su interfaz de usuario. Esta API de composición no se limita a las aplicaciones UWP, sin embargo. Las aplicaciones de escritorio de Win32 pueden aprovechar las ventajas de los sistemas modernos de composición en UWP y Windows 10.

## <a name="prerequisites"></a>Requisitos previos

La API de hospedaje de UWP tiene estos requisitos previos.

- Se supone que tiene cierta familiaridad con el desarrollo de aplicaciones con Win32 y UWP. Para obtener más información, consulta:
  - [Empezar a trabajar con Win32 yC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
  - [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/)
  - [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance)
- Windows 10, versión 1803 o posterior
- SDK de Windows 10 17134 o posterior

## <a name="how-to-use-composition-apis-from-a-win32-desktop-application"></a>Cómo usar las API de composición de una aplicación de escritorio de Win32

En este tutorial, creará un simple Win32 C++ aplicación y agregarle los elementos de la composición de UWP. El foco está en la configuración del proyecto, crear el código de interoperabilidad y dibujar algo sencillo mediante las API de composición de Windows correctamente. La aplicación finalizada es similar al siguiente.

![La interfaz de usuario de aplicación en ejecución](images/interop/win32-comp-interop-app-ui.png)

## <a name="create-a-c-win32-project-in-visual-studio"></a>Crear un C++ proyecto Win32 de Visual Studio

El primer paso es crear el proyecto de aplicación de Win32 en Visual Studio.

Para crear un nuevo proyecto de aplicación de Win32 en C++ denominado _HelloComposition_:

1. Abra Visual Studio y seleccione **archivo** > **New** > **proyecto**.

    El **nuevo proyecto** abre el cuadro de diálogo.
1. En el **instalado** categoría, expanda el **Visual C++**  nodo y, a continuación, seleccione **Windows Desktop**.
1. Seleccione el **aplicación de escritorio de Windows** plantilla.
1. Escriba el nombre _HelloComposition_, a continuación, haga clic en **Aceptar**.

    Visual Studio crea el proyecto y abre el editor para el archivo de aplicación principal.

## <a name="configure-the-project-to-use-windows-runtime-apis"></a>Configurar el proyecto para usar Windows Runtime APIs

Para usar Windows en tiempo de ejecución (WinRT) API en la aplicación de Win32, usamos C++/WinRT. Deberá configurar el proyecto de Visual Studio para agregar C++/WinRT soporte.

(Para obtener más información, consulte [Introducción C++/WinRT - modificar un proyecto de aplicación de escritorio de Windows para agregar C++soporte /WinRT](../cpp-and-winrt-apis/get-started.md#modify-a-windows-desktop-application-project-to-add-cwinrt-support)).

1. Desde el **proyecto** menú, abra las propiedades del proyecto (_HelloComposition propiedades_) y asegúrese de que los siguientes valores se establecen en los valores especificados:

    - Para **configuración**, seleccione _todas las configuraciones de_. Para **plataforma**, seleccione _todas las plataformas_.
    - **Propiedades de configuración** > **General** > **Windows SDK versión** = _10.0.17763.0_ o superior

    ![Establezca la versión SDK](images/interop/sdk-version.png)

    - **C /C++** > **lenguaje**  >   **C++ estándar de lenguaje** = _ISO C++ estándar 17 (/ stf:c ++ 17)_

    ![Conjunto estándar del lenguaje](images/interop/language-standard.png)

    - **Vinculador** > **entrada** > **dependencias adicionales** debe incluir "_windowsapp.lib_". Si no se incluye en la lista, agréguelo.

    ![Agregar una dependencia de vinculador](images/interop/linker-dependencies.png)

1. Actualizar el encabezado precompilado

    - Cambiar el nombre de `stdafx.h` y `stdafx.cpp` a `pch.h` y `pch.cpp`, respectivamente.
    - Establezca la propiedad del proyecto **C o C++** > **encabezados precompilados** > **el archivo de encabezado precompilado** a *pch.h*.
    - Buscar y reemplazar `#include "stdafx.h"` con `#include "pch.h"` en todos los archivos.

        (**Editar** > **buscar y reemplazar** > **buscar en archivos**)

        ![Buscar y reemplazar stdafx.h](images/interop/replace-stdafx.png)

    - En `pch.h`, incluir `winrt/base.h` y `unknwn.h`.

        ```cppwinrt
        // reference additional headers your program requires here
        #include <unknwn.h>
        #include <winrt/base.h>
        ```

Es una buena idea para compilar el proyecto en este momento para asegurarse de que no hay ningún error antes de continuar.

## <a name="create-a-class-to-host-composition-elements"></a>Crear una clase a los elementos de la composición de host

Para hospedar contenido cree con la capa visual, cree una clase (_CompositionHost_) para administrar la interoperabilidad y crear elementos de composición. Esto es donde realiza la mayoría de la configuración para el hospedaje de API de composición, incluidas:

- obtener un [Compositor](/uwp/api/windows.ui.composition.compositor), que crea y administra los objetos en el [Windows.UI.Composition](/uwp/api/windows.ui.composition) espacio de nombres.
- creación de un [DispatcherQueueController](/uwp/api/windows.system.dispatcherqueuecontroller)/[DispatcherQueue](/uwp/api/windows.system.dispatcherqueue) para administrar las tareas de las APIs WinRT.
- creación de un [DesktopWindowTarget](/uwp/api/windows.ui.composition.desktop.desktopwindowtarget) y contenedor de composición para mostrar los objetos de composición.

Esto es muy clase singleton para evitar problemas de subprocesamiento. Por ejemplo, solo puede crear una cola del distribuidor por subproceso, por lo que crear una segunda instancia de CompositionHost en el mismo subproceso, se produciría un error.

> [!TIP]
> Si necesita, compruebe el código completo al final del tutorial para asegurarse de que todo el código está en los lugares correctos cuando se trabaja con el tutorial.

1. Agrega un nuevo archivo de clase al proyecto.
    - En **el Explorador de soluciones**, haga clic en el _HelloComposition_ proyecto.
    - En el menú contextual, seleccione **agregar** > **clase...** .
    - En el **Agregar clase** cuadro de diálogo, nombre de la clase _CompositionHost.cs_, a continuación, haga clic en **agregar**.

1. Incluir encabezados y _instrucciones using_ necesarios para la interoperabilidad de composición.
    - En CompositionHost.h, agregue estas _incluye_ en la parte superior del archivo.

    ```cppwinrt
    #pragma once
    #include <winrt/Windows.UI.Composition.Desktop.h>
    #include <windows.ui.composition.interop.h>
    #include <DispatcherQueue.h>
    ```

    - En CompositionHost.cpp, agregue estas _instrucciones using_ en la parte superior del archivo, después de cualquier _incluye_.

    ```cppwinrt
    using namespace winrt;
    using namespace Windows::System;
    using namespace Windows::UI;
    using namespace Windows::UI::Composition;
    using namespace Windows::UI::Composition::Desktop;
    using namespace Windows::Foundation::Numerics;
    ```

1. Modifique la clase para usar el patrón singleton.
    - En CompositionHost.h, hacer privado el constructor.
    - Declarar una estática pública _GetInstance_ método.

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

    - En CompositionHost.cpp, agregue la definición de la _GetInstance_ método.

    ```cppwinrt
    CompositionHost* CompositionHost::GetInstance()
    {
        static CompositionHost instance;
        return &instance;
    }
    ```

1. En CompositionHost.h, declarar miembro privado variables para el Compositor, DispatcherQueueController y DesktopWindowTarget.

    ```cppwinrt
    winrt::Windows::UI::Composition::Compositor m_compositor{ nullptr };
    winrt::Windows::System::DispatcherQueueController m_dispatcherQueueController{ nullptr };
    winrt::Windows::UI::Composition::Desktop::DesktopWindowTarget m_target{ nullptr };
    ```

1. Agregue un método público para inicializar los objetos de interoperabilidad de composición.
    > [!NOTE]
    > En _inicializar_, se llama a la _EnsureDispatcherQueue_, _CreateDesktopWindowTarget_, y _CreateCompositionRoot_ métodos. Puede crear estos métodos en los pasos siguientes.

    - En CompositionHost.h, declare un método público denominado _inicializar_ que toma un HWND como argumento.

    ```cppwinrt
    void Initialize(HWND hwnd);
    ```

    - En CompositionHost.cpp, agregue la definición de la _inicializar_ método.

    ```cppwinrt
    void CompositionHost::Initialize(HWND hwnd)
    {
        EnsureDispatcherQueue();
        if (m_dispatcherQueueController) m_compositor = Compositor();

        CreateDesktopWindowTarget(hwnd);
        CreateCompositionRoot();
    }
    ```

1. Crear una cola del distribuidor en el subproceso que se va a utilizar la composición de Windows.

    En un subproceso que tiene una cola del distribuidor, por lo que primero se llama a este método durante la inicialización, se debe crear un Compositor.

    - En CompositionHost.h, declare un método privado denominado _EnsureDispatcherQueue_.

    ```cppwinrt
    void EnsureDispatcherQueue();
    ```

    - En CompositionHost.cpp, agregue la definición de la _EnsureDispatcherQueue_ método.

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

1. Registrar la ventana de la aplicación como un destino de composición.
    - En CompositionHost.h, declare un método privado denominado _CreateDesktopWindowTarget_ que toma un HWND como argumento.

    ```cppwinrt
    void CreateDesktopWindowTarget(HWND window);
    ```

    - En CompositionHost.cpp, agregue la definición de la _CreateDesktopWindowTarget_ método.

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

1. Crear un contenedor visual de raíz para almacenar objetos visuales.
    - En CompositionHost.h, declare un método privado denominado _CreateCompositionRoot_.

    ```cppwinrt
    void CreateCompositionRoot();
    ```

    - En CompositionHost.cpp, agregue la definición de la _CreateCompositionRoot_ método.

    ```cppwinrt
    void CompositionHost::CreateCompositionRoot()
    {
        auto root = m_compositor.CreateContainerVisual();
        root.RelativeSizeAdjustment({ 1.0f, 1.0f });
        root.Offset({ 24, 24, 0 });
        m_target.Root(root);
    }
    ```

Compile el proyecto ahora para asegurarse de que no hay ningún error.

Estos métodos se configuran los componentes necesarios para la interoperabilidad entre la capa visual de UWP y las API de Win32. Ahora puede agregar contenido a la aplicación.

### <a name="add-composition-elements"></a>Agregar elementos de composición

Con la infraestructura en su lugar, ahora puede generar el contenido de composición que desea mostrar.

En este ejemplo, agrega código que crea un cuadrado simple [SpriteVisual](/uwp/api/windows.ui.composition.spritevisual).

1. Agregue un elemento de composición.
    - En CompositionHost.h, declare un método público denominado _al agregar elemento_ que toma 3 **float** valores como argumentos.

    ```cppwinrt
    void AddElement(float size, float x, float y);
    ```

    - En CompositionHost.cpp, agregue la definición de la _al agregar elemento_ método.

    ```cppwinrt
    void CompositionHost::AddElement(float size, float x, float y)
    {
        if (m_target.Root())
        {
            auto visuals = m_target.Root().as<ContainerVisual>().Children();
            auto visual = m_compositor.CreateSpriteVisual();

            visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
            visual.Size({ size, size });
            visual.Offset({ x, y, 0.0f, });

            visuals.InsertAtTop(visual);
        }
    }
    ```

## <a name="create-and-show-the-window"></a>Crear y mostrar la ventana

Ahora, puede agregar el contenido de la composición de UWP a su interfaz de usuario de Win32. Usar la aplicación _InitInstance_ método para inicializar la composición de Windows y generar el contenido.

1. En HelloComposition.cpp, incluya _CompositionHost.h_ en la parte superior del archivo.

    ```cppwinrt
    #include "CompositionHost.h"
    ```

1. En el `InitInstance` método, agregue código para inicializar y usar la clase CompositionHost.

    Agregue este código después de crea el HWND y antes de llamar a `ShowWindow`.

    ```cppwinrt
    CompositionHost* compHost = CompositionHost::GetInstance();
    compHost->Initialize(hWnd);
    compHost->AddElement(150, 10, 10);
    ```

Ahora puede compilar y ejecutar la aplicación. Si necesita, compruebe el código completo al final del tutorial para asegurarse de que todo el código está en los lugares correctos.

Al ejecutar la aplicación, debería ver un cuadrado azul que se agregan a la interfaz de usuario.

## <a name="additional-resources"></a>Recursos adicionales

- [Ejemplo de Win32 HelloComposition (GitHub)](https://github.com/Microsoft/Windows.UI.Composition-Win32-Samples/tree/master/cpp/HelloComposition)
- [Empezar a trabajar con Win32 yC++](/windows/desktop/learnwin32/learn-to-program-for-windows)
- [Empezar a trabajar con aplicaciones de Windows 10](/windows/uwp/get-started/) (UWP)
- [Mejore su aplicación de escritorio para Windows 10](/windows/uwp/porting/desktop-to-uwp-enhance) (UWP)
- [Espacio de nombres Windows.UI.Composition](/uwp/api/windows.ui.composition) (UWP)

## <a name="complete-code"></a>Código completo

Este es el código completo de la clase CompositionHost y InitInstance (método).

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
    root.Offset({ 24, 24, 0 });
    m_target.Root(root);
}

void CompositionHost::AddElement(float size, float x, float y)
{
    if (m_target.Root())
    {
        auto visuals = m_target.Root().as<ContainerVisual>().Children();
        auto visual = m_compositor.CreateSpriteVisual();

        visual.Brush(m_compositor.CreateColorBrush({ 0xDC, 0x5B, 0x9B, 0xD5 }));
        visual.Size({ size, size });
        visual.Offset({ x, y, 0.0f, });

        visuals.InsertAtTop(visual);
    }
}
```

### <a name="hellocompositioncpp---initinstance"></a>HelloComposition.cpp - InitInstance

```cppwinrt
BOOL InitInstance(HINSTANCE hInstance, int nCmdShow)
{
   hInst = hInstance; // Store instance handle in our global variable

   HWND hWnd = CreateWindowW(szWindowClass, szTitle, WS_OVERLAPPEDWINDOW,
      CW_USEDEFAULT, 0, CW_USEDEFAULT, 0, nullptr, nullptr, hInstance, nullptr);

   if (!hWnd)
   {
      return FALSE;
   }

   CompositionHost* compHost = CompositionHost::GetInstance();
   compHost->Initialize(hWnd);
   compHost->AddElement(150, 10, 10);

   ShowWindow(hWnd, nCmdShow);
   UpdateWindow(hWnd);

   return TRUE;
}
```