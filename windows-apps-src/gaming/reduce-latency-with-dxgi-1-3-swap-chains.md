---
title: Reducir la latencia con cadenas de intercambio de DXGI 1.3
description: Usa DXGI 1.3 para reducir la latencia de fotogramas eficaz esperando a que la cadena de intercambio señale el momento adecuado para empezar a representar un nuevo fotograma.
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, latency, latencia, dxgi, swap chains, cadenas de intercambio, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: dd414c3ea65d30253d54cd335ed0b85d151b6dff
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258438"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>Reducir la latencia con cadenas de intercambio de DXGI 1.3



Usa DXGI 1.3 para reducir la latencia de fotogramas eficaz esperando a que la cadena de intercambio señale el momento adecuado para empezar a representar un nuevo fotograma. Por lo general, los juegos necesitan proporcionar la menor latencia posible desde el momento en el que se recibe la entrada del jugador hasta que el juego responde a dicha entrada actualizando la pantalla. En este tema se describe una técnica (disponible a partir de Direct3D 11.2) que puedes usar para reducir la latencia de fotogramas efectiva de tu juego.

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>¿De qué forma reduce la latencia esperar al búfer de reserva?


Con la cadena de intercambio del modelo de volteo, los "giros" del búfer de reserva se ponen en cola cada vez que el juego llama a [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present). Cuando el bucle de representación llama a Present(), el sistema bloquea el subproceso hasta que se termine de mostrar un fotograma anterior, lo que permite crear espacio para el nuevo fotograma antes de que se muestre realmente. Esto hace que haya más latencia entre el momento en el que el juego dibuja un fotograma y el momento en el que el sistema le permite mostrar dicho fotograma. Muchas veces, el sistema logrará un equilibrio estable en el que el juego siempre esté esperando casi un fotograma extra completo entre el momento en el que representa y el momento en que muestra cada fotograma. Lo mejor es esperar a que el sistema esté listo para aceptar un nuevo fotograma y, luego, representar el fotograma según los datos actuales y poner el fotograma inmediatamente en cola.

Cree una cadena de intercambio que se debe esperar con la [**cadena de\_de intercambio de DXGI\_\_marca\_\_latencia**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)\_la marca de objeto\_espera. Las cadenas de intercambio que se crean de este modo pueden informar al bucle de representación de cuándo está el sistema listo para aceptar un nuevo fotograma, lo que hace posible que el juego represente según los datos actuales y, luego, ponga el resultado directamente en la cola actual.

## <a name="step-1-create-a-waitable-swap-chain"></a>Paso 1: Crear una cadena de intercambio que pueda esperar


Especifique la [**cadena de\_de intercambio de DXGI\_\_marca\_** ](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)\_de\_latencia\_espera que se puede esperar marcador de objeto al llamar a [**CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow).

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> **Nota**   a diferencia de algunas marcas, esta marca no se puede agregar ni quitar mediante [**ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). DXGI devuelve un código de error si esta marca se configura de otro modo distinto de cuando se creó la cadena de intercambio.

 

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>Paso 2: Establecer la latencia de fotogramas


Define la latencia de fotogramas con la API [**IDXGISwapChain2::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency), en vez de llamar a [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency).

La latencia de fotogramas de las cadenas de intercambio que pueden esperar se establece de forma predeterminada en 1, lo cual hace que la latencia se reduzca todo lo posible, pero también el paralelismo entre CPU y GPU. Establece la latencia de fotogramas en 2 en caso de que necesites un mayor paralelismo entre CPU y GPU para llegar a los 60 fotogramas/s (esto es, si tanto la CPU como la GPU consumen cada una menos de 16,7 ms por fotograma al procesar el trabajo de representación, pero juntas superan esos 16,7 ms). De este modo, la GPU puede procesar el trabajo que la CPU ha puesto en cola durante el fotograma anterior y, al mismo tiempo, la CPU puede enviar comandos de representación para el fotograma actual de manera independiente.

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>Paso 3: Obtener el objeto que puede esperar de la cadena de intercambio


Llama a [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) para obtener el identificador de espera, que es un elemento que señala al objeto que puede esperar. Guarda este identificador para que lo use el bucle de representación.

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>Paso 4: Esperar a que cada fotograma se represente


El bucle de representación debe esperar a que la cadena de intercambio indique (a través del objeto que puede esperar) que se puede representar cada fotograma. Esto incluye el primer fotograma representado con la cadena de intercambio. Usa [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex) (suministrando el identificador de espera obtenido en el paso 2) para indicar el inicio de cada fotograma.

En el siguiente ejemplo se muestra el bucle de representación de la muestra de DirectXLatency:

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

En el siguiente ejemplo se muestra la llamada a WaitForSingleObjectEx de la muestra de DirectXLatency:

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>¿Qué debe hacer el juego mientras espera a que la cadena de intercambio presente fotogramas?


En los juegos en los que no hay ninguna tarea que se bloquee en el bucle de representación, esperar a que la cadena de intercambio presente cada fotograma puede ser muy ventajoso porque se ahorra energía, algo que es particularmente importante en los dispositivos móviles. Si no es así, puedes recurrir al multithreading para acometer el trabajo mientras el juego espera a que la cadena de intercambio presente fotogramas. Estas son algunas de las tareas que el juego puede completar:

-   Procesar eventos de red
-   Actualizar la AI
-   Física basada en la CPU
-   Representación de contexto aplazado (en dispositivos compatibles)
-   Carga de activos

Consulta los siguientes temas relacionados para obtener más información sobre la programación con multithreading en Windows.

## <a name="related-topics"></a>Temas relacionados


* [Ejemplo de DirectXLatency](https://code.msdn.microsoft.com/windowsapps/DirectXLatency-sample-a2e2c9c3)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows. System. Threading**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)
* [Programación asincrónica enC++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [Procesos y subprocesos](https://docs.microsoft.com/windows/desktop/ProcThread/processes-and-threads)
* [Sincronización](https://docs.microsoft.com/windows/desktop/Sync/synchronization)
* [Usar objetos de eventos (Windows)](https://docs.microsoft.com/windows/desktop/Sync/using-event-objects)

 

 




