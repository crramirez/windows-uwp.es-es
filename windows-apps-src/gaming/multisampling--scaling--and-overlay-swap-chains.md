---
author: mtoepke
title: Escalado y superposiciones de cadenas de intercambio
description: Aprende a crear cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas, y usa también cadenas de intercambio (si las hay) para mejorar la calidad visual.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, intercambiar escalado de cadenas, superposiciones, directx, games, swap chain scaling, overlays
ms.localizationpriority: medium
ms.openlocfilehash: 9d159a78412bea528c1a12428288daebe31d1fe1
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6196780"
---
# <a name="swap-chain-scaling-and-overlays"></a>Escalado y superposiciones de cadenas de intercambio



Aprende a crear cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas, y a usar cadenas de intercambio de superposición (si las hay) para mejorar la calidad visual.

## <a name="swap-chains-in-directx-112"></a>Cadenas de intercambio en DirectX11.2


En Direct3D11.2 se pueden crear aplicaciones para la Plataforma universal de Windows (UWP) con cadenas de intercambio con escala desde resoluciones no nativas (reducidas), por lo que la velocidad de relleno es más rápida. Direct3D11.2 también incluye API para poder realizar representaciones con superposiciones de hardware, lo que permite que puedas mostrar UI en otra cadena de intercambio en resolución nativa. Gracias a ello, tu juego puede dibujar una interfaz de usuario en una resolución nativa completa y a una gran velocidad de fotogramas, con lo cual estarás sacando el máximo partido a los dispositivos móviles y a las pantallas con valores altos de PPP (como 3840 x 2160). En este artículo te enseñamos a usar la superposición de cadenas de intercambio.

Direct3D11.2 incluye también una característica nueva para reducir la latencia en las cadenas de intercambio de modelo de volteo. Consulta [Reducir la latencia con cadenas de intercambio DXGI1.3](reduce-latency-with-dxgi-1-3-swap-chains.md).

## <a name="use-swap-chain-scaling"></a>Usar el escalado de cadenas de intercambio


Cuando tu juego se ejecuta en hardware de bajo nivel (o en hardware optimizado para el ahorro de energía), puede que convenga representar el contenido del juego en tiempo real en una resolución menor de la que la pantalla es capaz de forma nativa. Para ello, debes usar una cadena de intercambio, que se usa para representar el contenido del juego, inferior a la resolución nativa, o bien usar una subregión de dicha cadena.

1.  En primer lugar, crea una cadena de intercambio en una resolución nativa total.

    ```cpp
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

    swapChainDesc.Width = static_cast<UINT>(m_d3dRenderTargetSize.Width); // Match the size of the window.
    swapChainDesc.Height = static_cast<UINT>(m_d3dRenderTargetSize.Height);
    swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
    swapChainDesc.Stereo = false;
    swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
    swapChainDesc.SampleDesc.Quality = 0;
    swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
    swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
    swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
    swapChainDesc.Flags = 0;
    swapChainDesc.Scaling = DXGI_SCALING_STRETCH;

    // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
    ComPtr<IDXGIDevice3> dxgiDevice;
    DX::ThrowIfFailed(
        m_d3dDevice.As(&dxgiDevice)
        );

    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );

    ComPtr<IDXGIFactory2> dxgiFactory;
    DX::ThrowIfFailed(
        dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
        );

    ComPtr<IDXGISwapChain1> swapChain;
    DX::ThrowIfFailed(
        dxgiFactory->CreateSwapChainForCoreWindow(
            m_d3dDevice.Get(),
            reinterpret_cast<IUnknown*>(m_window.Get()),
            &swapChainDesc,
            nullptr,
            &swapChain
            )
        );

    DX::ThrowIfFailed(
        swapChain.As(&m_swapChain)
        );
    ```

2.  Después, elige una subregión de esa cadena para ajustar la escala, estableciendo para ello el tamaño del origen en una resolución reducida.

    En la muestra de cadenas de intercambio en primer plano deDX, este tamaño reducido se calcula según un porcentaje:

    ```cpp
    m_d3dRenderSizePercentage = percentage;

    UINT renderWidth = static_cast<UINT>(m_d3dRenderTargetSize.Width * percentage + 0.5f);
    UINT renderHeight = static_cast<UINT>(m_d3dRenderTargetSize.Height * percentage + 0.5f);

    // Change the region of the swap chain that will be presented to the screen.
    DX::ThrowIfFailed(
        m_swapChain->SetSourceSize(
            renderWidth,
            renderHeight
            )
        );
    ```

3.  Crea una ventanilla que coincida con la subregión de la cadena de intercambio.

    ```cpp
    // In Direct3D, change the Viewport to match the region of the swap
    // chain that will now be presented from.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        static_cast<float>(renderWidth),
        static_cast<float>(renderHeight)
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);
    ```

4.  Si se usa Direct2D, la transformación de giro debe ajustarse para compensar la región de origen.

## <a name="create-a-hardware-overlay-swap-chain-for-ui-elements"></a>Crear una cadena de intercambio de superposición de hardware para elementos de UI


Cuando se usa el escalado de cadenas de intercambio, existe un inconveniente característico de este aspecto, que consiste en que la UI también se escala, lo que puede hacer que aparezca borrosa y que sea más difícil de usar. En dispositivos con hardware que sea compatible con cadenas de intercambio de superposición, este problema desaparece por completo, ya que la interfaz de usuario se representa en una resolución nativa en una cadena de intercambio que se encuentra aparte del contenido del juego en tiempo real. Recuerda que esta técnica funciona únicamente en cadenas de intercambio [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). No se puede usar con la interoperabilidad de XAML.

Realiza los siguientes pasos para crear una cadena de intercambio en primer plano que use la funcionalidad de superposición de hardware. Estos pasos son los que se efectúan después de crear por primera vez una cadena de intercambio para contenido de juego en tiempo real, como se ha descrito anteriormente.

1.  En primer lugar, comprueba si el adaptador de DXGI admite las superposiciones. Para ello, obtén el adaptador de salida deDXGI de la cadena de intercambio:

    ```cpp
    ComPtr<IDXGIAdapter> outputDxgiAdapter;
    DX::ThrowIfFailed(
        dxgiFactory->EnumAdapters(0, &outputDxgiAdapter)
        );

    ComPtr<IDXGIOutput> dxgiOutput;
    DX::ThrowIfFailed(
        outputDxgiAdapter->EnumOutputs(0, &dxgiOutput)
        );

    ComPtr<IDXGIOutput2> dxgiOutput2;
    DX::ThrowIfFailed(
        dxgiOutput.As(&dxgiOutput2)
        );
    ```

    El adaptador DXGI admitirá las superposiciones si el adaptador de salida devuelve "True" en el método [**SupportsOverlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411).

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Nota**  si el adaptador DXGI admite las superposiciones, continúa al paso siguiente. Si no es así, la representación con varias cadenas de intercambio no tendrá un resultado eficaz. En lugar de ello, representa la interfaz de usuario en una resolución reducida en la misma cadena de intercambio que el contenido del juego en tiempo real.

     

2.  Crea la cadena de intercambio en primer plano con [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559). Las siguientes opciones deben estar establecidas en la estructura [**DXGI\_SWAP\_CHAIN\_DESC1**](https://msdn.microsoft.com/library/windows/desktop/hh404528) que suministraste al parámetro *pDesc*:

    -   Especifica la marca de cadena de intercambio [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) de forma que señale a una cadena de intercambio en primer plano.
    -   Usa la marca de modo alfa [**DXGI\_ALPHA\_MODE\_PREMULTIPLIED**](https://msdn.microsoft.com/library/windows/desktop/hh404496). Las cadenas de intercambio en primer plano siempre se multiplican de antemano.
    -   Establece la marca [**DXGI\_SCALING\_NONE**](https://msdn.microsoft.com/library/windows/desktop/hh404526). Las cadenas de intercambio en primer plano siempre se ejecutan en una resolución nativa.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Nota**  establecer el [**DXGI\_SWAP\_CHAIN\_FLAG\_FOREGROUND\_LAYER**](https://msdn.microsoft.com/library/windows/desktop/bb173076) de nuevo cada vez que se cambie el tamaño de la cadena de intercambio.

    ```cpp
    HRESULT hr = m_foregroundSwapChain->ResizeBuffers(
        2, // Double-buffered swap chain.
        static_cast<UINT>(m_d3dRenderTargetSize.Width),
        static_cast<UINT>(m_d3dRenderTargetSize.Height),
        DXGI_FORMAT_B8G8R8A8_UNORM,
        DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER // The FOREGROUND_LAYER flag cannot be removed with ResizeBuffers.
        );
    ```

3.  Si usas dos cadenas de intercambio, aumenta la latencia de fotogramas máxima a 2 para que el adaptador DXGI tenga tiempo para representar a las dos al mismo tiempo (en el mismo intervalo de SincV).

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

4.  Las cadenas de intercambio en primer plano siempre usan un alfa premultiplicado. Por lo tanto, se espera que los valores de color de cada píxel ya estén multiplicados por el valor de alfa antes de que el fotograma se muestre. Por ejemplo, un píxel BGRA 100% blanco en un alfa del 50% se establecerá en (0,5, 0,5, 0,5, 0,5).

    El paso para multiplicar de antemano el elemento alfa se puede realizar en la fase de fusión de salida, aplicando para ello un estado de mezcla de la aplicación (consulta [**ID3D11BlendState**](https://msdn.microsoft.com/library/windows/desktop/ff476349)) con el campo **SrcBlend** de la estructura [**D3D11\_RENDER\_TARGET\_BLEND\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476200) establecido como **D3D11\_SRC\_ALPHA**. También se pueden usar activos con valores de alfa multiplicados de antemano.

    Si el paso de premultiplicación del alpha no se lleva a cabo, los colores de la cadena de intercambio en primer plano serán más brillantes de lo previsto.

5.  Dependiendo de si se ha creado la cadena de intercambio en primer plano, es posible que la superficie de dibujo de Direct2D para los elementos de UI deba asociarse a la cadena de intercambio en primer plano.

    Para crear las vistas de destino de representación:

    ```cpp
    // Create a render target view of the foreground swap chain's back buffer.
    if (m_foregroundSwapChain)
    {
        ComPtr<ID3D11Texture2D> foregroundBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&foregroundBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                foregroundBackBuffer.Get(),
                nullptr,
                &m_d3dForegroundRenderTargetView
                )
            );
    }
    ```

    Para crear la superficie de dibujo de Direct2D:

    ```cpp
    if (m_foregroundSwapChain)
    {
        // Create a Direct2D target bitmap for the foreground swap chain.
        ComPtr<IDXGISurface2> dxgiForegroundSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_foregroundSwapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiForegroundSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiForegroundSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }
    else
    {
        // Create a Direct2D target bitmap for the swap chain.
        ComPtr<IDXGISurface2> dxgiSwapChainBackBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiSwapChainBackBuffer))
            );

        DX::ThrowIfFailed(
            m_d2dContext->CreateBitmapFromDxgiSurface(
                dxgiSwapChainBackBuffer.Get(),
                &bitmapProperties,
                &m_d2dTargetBitmap
                )
            );
    }

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());
    ```

    ```cpp
    // Create a render target view of the swap chain's back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );
    ```

6.  Muestra la cadena de intercambio en primer plano con la cadena de intercambio con escala utilizada para el contenido del juego en tiempo real. Como la latencia de fotogramas se estableció en 2 en ambas cadenas de intercambio, la infraestructura DXGI podrá mostrarlas en el mismo intervalo de SincV.

    ```cpp
    // Present the contents of the swap chain to the screen.
    void DX::DeviceResources::Present()
    {
        // The first argument instructs DXGI to block until VSync, putting the application
        // to sleep until the next VSync. This ensures that we don't waste any cycles rendering
        // frames that will never be displayed to the screen.
        HRESULT hr = m_swapChain->Present(1, 0);

        if (SUCCEEDED(hr) && m_foregroundSwapChain)
        {
            m_foregroundSwapChain->Present(1, 0);
        }

        // Discard the contents of the render targets.
        // This is a valid operation only when the existing contents will be entirely
        // overwritten. If dirty or scroll rects are used, this call should be removed.
        m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());
        if (m_foregroundSwapChain)
        {
            m_d3dContext->DiscardView(m_d3dForegroundRenderTargetView.Get());
        }

        // Discard the contents of the depth stencil.
        m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

        // If the device was removed either by a disconnection or a driver upgrade, we
        // must recreate all device resources.
        if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            HandleDeviceLost();
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    ```

 

 




