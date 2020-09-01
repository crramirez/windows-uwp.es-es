---
title: Intercambiar escalado y superposiciones de cadenas
description: Aprende a crear cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas, y a usar cadenas de intercambio de superposición (si las hay) para mejorar la calidad visual.
ms.assetid: 3e4d2d19-cac3-eebc-52dd-daa7a7bc30d1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, escalado de cadenas de intercambio, superposiciones, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: ade5812999a3fe085a7c2091363857d7eefa1870
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165169"
---
# <a name="swap-chain-scaling-and-overlays"></a>Intercambiar escalado y superposiciones de cadenas



Aprende a crear cadenas de intercambio con escala para que las representaciones en los dispositivos móviles sean más rápidas, y a usar cadenas de intercambio de superposición (si las hay) para mejorar la calidad visual.

## <a name="swap-chains-in-directx-112"></a>Cadenas de intercambio en DirectX 11.2


En Direct3D 11.2 se pueden crear aplicaciones para la Plataforma universal de Windows (UWP) con cadenas de intercambio con escala desde resoluciones no nativas (reducidas), por lo que la velocidad de relleno es más rápida. Direct3D 11.2 también incluye API para poder realizar representaciones con superposiciones de hardware, lo que permite que puedas mostrar UI en otra cadena de intercambio en resolución nativa. Gracias a ello, tu juego puede dibujar una interfaz de usuario en una resolución nativa completa y a una gran velocidad de fotogramas, con lo cual estarás sacando el máximo partido a los dispositivos móviles y a las pantallas con valores altos de PPP (como 3840 x 2160). En este artículo te enseñamos a usar la superposición de cadenas de intercambio.

Direct3D 11.2 incluye también una característica nueva para reducir la latencia en las cadenas de intercambio de modelo de volteo. Consulta [Reducir la latencia con cadenas de intercambio DXGI 1.3](reduce-latency-with-dxgi-1-3-swap-chains.md).

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

    En la muestra de cadenas de intercambio en primer plano de DX, este tamaño reducido se calcula según un porcentaje:

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


Cuando se usa el escalado de cadenas de intercambio, existe un inconveniente característico de este aspecto, que consiste en que la UI también se escala, lo que puede hacer que aparezca borrosa y que sea más difícil de usar. En dispositivos con hardware que sea compatible con cadenas de intercambio de superposición, este problema desaparece por completo, ya que la interfaz de usuario se representa en una resolución nativa en una cadena de intercambio que se encuentra aparte del contenido del juego en tiempo real. Recuerda que esta técnica funciona únicamente en cadenas de intercambio [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow). No se puede usar con la interoperabilidad de XAML.

Realiza los siguientes pasos para crear una cadena de intercambio en primer plano que use la funcionalidad de superposición de hardware. Estos pasos son los que se efectúan después de crear por primera vez una cadena de intercambio para contenido de juego en tiempo real, como se ha descrito anteriormente.

1.  En primer lugar, comprueba si el adaptador de DXGI admite las superposiciones. Para ello, obtén el adaptador de salida de DXGI de la cadena de intercambio:

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

    El adaptador DXGI admitirá las superposiciones si el adaptador de salida devuelve "True" en el método [**SupportsOverlays**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays).

    ```cpp
    m_overlaySupportExists = dxgiOutput2->SupportsOverlays() ? true : false;
    ```
    
    > **Nota:**    Si el adaptador de DXGI admite superposiciones, continúe con el paso siguiente. Si no es así, la representación con varias cadenas de intercambio no tendrá un resultado eficaz. En lugar de ello, representa la interfaz de usuario en una resolución reducida en la misma cadena de intercambio que el contenido del juego en tiempo real.

     

2.  Crea la cadena de intercambio en primer plano con [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow). Las siguientes opciones se deben establecer en la [**cadena de intercambio de DXGI \_ \_ \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) suministrada al parámetro *pDesc* :

    -   Especifique la marca de la cadena de intercambio de la cadena de intercambio de DXGI para indicar una cadena de intercambio en primer plano. [** \_ \_ \_ \_ \_ **](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)
    -   Use la marca modo alfa [** \_ \_ \_ premultiplicado**](/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_alpha_mode) de modo alfa. Las cadenas de intercambio en primer plano siempre se multiplican de antemano.
    -   Establezca la marca de [** \_ \_ escalado de DXGI**](/windows/desktop/api/dxgi1_2/ne-dxgi1_2-dxgi_scaling) . Las cadenas de intercambio en primer plano siempre se ejecutan en una resolución nativa.

    ```cpp
     foregroundSwapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FOREGROUND_LAYER;
     foregroundSwapChainDesc.Scaling = DXGI_SCALING_NONE;
     foregroundSwapChainDesc.AlphaMode = DXGI_ALPHA_MODE_PREMULTIPLIED; // Foreground swap chain alpha values must be premultiplied.
    ```

    > **Nota:**    Vuelva a establecer el [** \_ \_ \_ \_ \_ nivel de primer plano**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag) de la cadena de intercambio de DXGI cada vez que se cambie el tamaño de la cadena de intercambio.

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

    El paso de la premultiplicación alfa puede realizarse en la fase de combinación de salida mediante la aplicación de un estado de Blend (consulte [**ID3D11BlendState**](/windows/desktop/api/d3d11/nn-d3d11-id3d11blendstate)) con el campo **SrcBlend** de [**D3D11 \_ Render \_ target \_ Blend \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_render_target_blend_desc) de la estructura DESC establecido en **D3D11 \_ src \_ Alpha**. También se pueden usar activos con valores de alfa multiplicados de antemano.

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

 

 