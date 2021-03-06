---
title: Migrar el bucle del juego
description: Aprende a implementar una ventana para un juego de la Plataforma universal de Windows (UWP) y a traer el bucle de la repetición, incluso cómo crear una interfaz IFrameworkView para controlar una clase CoreWindow de pantalla completa.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, portabilidad, bucle de juego, Direct3D 9, DirectX 11
ms.localizationpriority: medium
ms.openlocfilehash: e62b7e6576ff1b39cbeba2c201f929952abd4105
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159099"
---
# <a name="port-the-game-loop"></a>Migrar el bucle del juego



**Resumen**

-   [Parte 1: Inicializar Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Parte 2: Convertir el marco de representación](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Parte 3: Migrar el bucle del juego


Aprende a implementar una ventana para un juego de la Plataforma universal de Windows (UWP) y a traer el bucle de la repetición, incluso cómo crear una [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) para controlar una [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) de pantalla completa. Parte 3 del tutorial [Migrar una aplicación de Direct3D 9 sencilla a DirectX 11 y UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Crear una ventana


Para configurar una ventana de escritorio con una ventanilla de Direct3D 9, tuvimos que implementar el marco tradicional basado en ventanas para aplicaciones de escritorio. Creamos un HWND, establecimos el tamaño de la ventana y proporcionamos la devolución de llamada para su procesamiento, la hicimos visible, etc.

El entorno de UWP ofrece un sistema mucho más simple. En lugar de configurar una ventana tradicional, un juego de Microsoft Store con DirectX implementa [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Esta interfaz existe para que los juegos y las aplicaciones de DirectX se ejecuten directamente en una clase [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) en el interior del contenedor de la aplicación.

> **Nota:**    Windows proporciona punteros administrados a recursos como el objeto de aplicación de origen y el [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow). Vea [**identificador del operador de objeto (^)**](/cpp/extensions/handle-to-object-operator-hat-cpp-component-extensions).

 

Tu clase principal "main" necesita heredar de [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) e implementar los cinco métodos de **IFrameworkView**: [**Inicializar**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [**SetWindow**](/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [**Cargar**](/uwp/api/windows.applicationmodel.core.iframeworkview.load), [**Ejecutar**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) y [**Anular inicialización**](/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). Además de crear **IFrameworkView**, que es (esencialmente) donde reside el juego, es necesario que implementes una clase de fábrica para que cree una instancia de tu **IFrameworkView**. El juego aún tiene un ejecutable con un método llamado **main()**, pero todo lo que puede hacer la función main es usar la fábrica para crear la instancia de **IFrameworkView**.

Función main

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Fábrica de IFrameworkView

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Migrar el bucle del juego


Veamos el bucle del juego en nuestra implementación de Direct3D 9. Este código existe en la función main de la aplicación. Cada iteración de este bucle procesa un mensaje de ventana o representa un marco.

Bucle de un juego de escritorio de Direct3D 9

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

El bucle del juego es similar, pero más simple, en la versión de nuestro juego de UWP:

La repetición del juego está en el método [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) (en lugar de **main()**) porque nuestro juego funciona dentro de la clase [**IFrameworkView**](/uwp/api/Windows.ApplicationModel.Core.IFrameworkView).

En vez de implementar un mensaje controlando un marco o llamando a [**PeekMessage**](/windows/desktop/api/winuser/nf-winuser-peekmessagea), podemos llamar al método [**ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents) creado en [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) de la ventana de nuestra aplicación. La repetición del juego no necesita bifurcarse ni administrar mensajes, tan solo tiene que llamar a **ProcessEvents** y continuar.

Bucle de juego en Direct3D 11 Microsoft Store juego

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Ahora tenemos una aplicación para UWP que configura la misma infraestructura básica de elementos gráficos y representa el mismo cubo de color, como nuestro ejemplo de DirectX 9.

## <a name="where-do-i-go-from-here"></a>¿Y ahora qué?


Usa un marcador para las [preguntas más frecuentes sobre la migración a DirectX 11](directx-porting-faq.md).

Las plantillas de DirectX de UWP incluyen una sólida infraestructura de dispositivo de Direct3D lista para usar en tu juego. Si quieres obtener directrices para elegir la plantilla correcta, consulta [Crear un proyecto de juego DirectX con una plantilla](user-interface.md).

Visite los siguientes artículos detallados sobre desarrollo de juegos de Microsoft Store:

-   [Tutorial: un juego simple para UWP con DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Audio para juegos](working-with-audio-in-your-directx-game.md)
-   [Controles de movimiento y vista para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 