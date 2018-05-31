---
author: mtoepke
title: Migrar el bucle del juego
description: Aprende a implementar una ventana para un juego de la Plataforma universal de Windows (UWP) y a traer el bucle de la repetición, incluso cómo crear una interfaz IFrameworkView para controlar una clase CoreWindow de pantalla completa.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, juegos, migración, bucle del juego, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: baf230559ebeb285d5faa3e2de8e38b355638070
ms.sourcegitcommit: 842ddba19fa3c028ea43e7922011515dbeb34e9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2018
ms.locfileid: "1488849"
---
# <a name="port-the-game-loop"></a>Migrar el bucle del juego



**Resumen**

-   [Parte 1: Inicializar Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Parte 2: Convertir el marco de representación](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Parte 3: Migrar el bucle del juego


Aprende a implementar una ventana para un juego de la Plataforma universal de Windows (UWP) y a traer el bucle de la repetición, incluso cómo crear una [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) para controlar una [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) de pantalla completa. Parte 3 del tutorial [Migrar una aplicación de Direct3D 9 sencilla a DirectX 11 y UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## <a name="create-a-window"></a>Crear una ventana


Para configurar una ventana de escritorio con una ventanilla de Direct3D 9, tuvimos que implementar el marco tradicional basado en ventanas para aplicaciones de escritorio. Creamos un HWND, establecimos el tamaño de la ventana y proporcionamos la devolución de llamada para su procesamiento, la hicimos visible, etc.

El entorno de UWP ofrece un sistema mucho más simple. En lugar de configurar una ventana tradicional, un juego de Microsoft Store que usa DirectX implementa [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Esta interfaz existe para que los juegos y las aplicaciones de DirectX se ejecuten directamente en una clase [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) en el interior del contenedor de la aplicación.

> **Nota** Windows suministra punteros administrados a recursos, como el objeto de la aplicación de origen y la clase [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Consulta [**Identificador de objeto operador (^)**]https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx.

 

Tu clase principal "main" necesita heredar de [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) e implementar los cinco métodos de **IFrameworkView**: [**Inicializar**](https://msdn.microsoft.com/library/windows/apps/hh700495), [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509), [**Cargar**](https://msdn.microsoft.com/library/windows/apps/hh700501), [**Ejecutar**](https://msdn.microsoft.com/library/windows/apps/hh700505) y [**Anular inicialización**](https://msdn.microsoft.com/library/windows/apps/hh700523). Además de crear **IFrameworkView**, que es (esencialmente) donde reside el juego, es necesario que implementes una clase de fábrica para que cree una instancia de tu **IFrameworkView**. El juego aún tiene un ejecutable con un método llamado **main()**, pero todo lo que puede hacer la función main es usar la fábrica para crear la instancia de **IFrameworkView**.

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


Veamos el bucle del juego en nuestra implementación de Direct3D9. Este código existe en la función main de la aplicación. Cada iteración de este bucle procesa un mensaje de ventana o representa un marco.

Bucle de un juego de escritorio de Direct3D9

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

La repetición del juego está en el método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) (en lugar de **main()**) porque nuestro juego funciona dentro de la clase [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478).

En vez de implementar un mensaje controlando un marco o llamando a [**PeekMessage**](https://msdn.microsoft.com/library/windows/desktop/ms644943), podemos llamar al método [**ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) creado en [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) de la ventana de nuestra aplicación. La repetición del juego no necesita bifurcarse ni administrar mensajes, tan solo tiene que llamar a **ProcessEvents** y continuar.

Bucle de un juego de Direct3D 11 en Microsoft Store.

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

## <a name="where-do-i-go-from-here"></a>Dónde ir desde aquí


Usa un marcador para las [preguntas más frecuentes sobre la migración a DirectX11](directx-porting-faq.md).

Las plantillas de DirectX de UWP incluyen una sólida infraestructura de dispositivo de Direct3D lista para usar en tu juego. Si quieres obtener directrices para elegir la plantilla correcta, consulta [Crear un proyecto de juego DirectX con una plantilla](user-interface.md).

Para obtener información detallada sobre el desarrollo de juegos de Microsoft Store, consulta estos artículos:

-   [Tutorial: un juego simple para UWP con DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Audio para juegos](working-with-audio-in-your-directx-game.md)
-   [Controles de movimiento y vista para juegos](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




