---
title: Configuración de recursos de DirectX y visualización de una imagen
description: Aquí te mostramos cómo crear un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y cómo mostrar la imagen representada en la pantalla.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, recursos, imágenes
ms.localizationpriority: medium
ms.openlocfilehash: cced3b5cb6ad9c3e1ffe077887c5f23ce95745bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159199"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Configuración de recursos de DirectX y visualización de una imagen



Aquí te mostramos cómo crear un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y cómo mostrar la imagen representada en la pantalla.

**Objetivo:** configurar recursos de DirectX en una aplicación para la Plataforma universal de Windows (UWP) con C++ y mostrar un color sólido.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de elementos gráficos.

**Tiempo en completarse:** 20 minutos.

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Declarar variables de interfaz de Direct3D con ComPtr

Declaramos variables de interfaz de Direct3D con la [plantilla de puntero inteligente](/cpp/cpp/smart-pointers-modern-cpp) ComPtr de la Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL), para poder administrar la vigencia de esas variables de manera segura. Podemos usar esas variables para acceder a la [**clase ComPtr**](/cpp/windows/comptr-class) y a sus miembros. Por ejemplo:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Si declara [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) con ComPtr, puede usar el método **getaddressof (** de ComPtr para obtener la dirección del puntero a **ID3D11RenderTargetView** ( \* \* ID3D11RenderTargetView) para pasar a [**ID3D11DeviceContext:: OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets). **OMSetRenderTargets** enlaza el destino de representación con la [fase de combinación de salida](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage) para especificar el destino de representación como el destino de salida.

La aplicación de muestra primero se inicia, luego se inicializa y se carga, y luego está lista para ejecutarse.

### <a name="2-creating-the-direct3d-device"></a>2. Crear el dispositivo Direct3D

Para usar la API de Direct3D para representar una escena, primero debemos crear un dispositivo Direct3D que represente el adaptador de pantalla. Para crear el dispositivo Direct3D, llamamos a la función [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Se especifican los niveles de 9,1 a 11,1 en la matriz de valores de [** \_ \_ nivel de característica D3D**](/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) . Direct3D recorre la matriz en orden y devuelve el nivel de características más alto compatible. Por lo tanto, para obtener el nivel de característica más alto disponible, se enumeran las entradas de matriz de ** \_ \_ nivel de característica D3D** de mayor a menor. Pasamos la marca [**D3D11 \_ Create \_ Device \_ BGRA \_ support**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) al parámetro *Flags* para hacer que los recursos de Direct3D interoperen con Direct2D. Si usamos la compilación de depuración, también pasamos el indicador de [** \_ \_ \_ depuración crear dispositivo de D3D11**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) . Para obtener más información sobre la depuración de aplicaciones, consulte [uso del nivel de depuración para](/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps)depurar aplicaciones.

Obtenemos el dispositivo Direct3D 11,1 ([**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) y el contexto de dispositivo ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) consultando el dispositivo Direct3D 11 y el contexto de dispositivo que se devuelve desde [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. Crear la cadena de intercambio

A continuación, creamos la cadena de intercambio que el dispositivo usa para representar y mostrar elementos en pantalla. Declaramos e inicializamos una estructura DESC1 de la cadena de intercambio de DXGI para describir la cadena de intercambio. [** \_ \_ \_ **](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) Después, configuramos la cadena de intercambio como Flip-Model (es decir, una cadena de intercambio que tiene el [**efecto de intercambio de DXGI, \_ \_ \_ Flip \_ Sequential**](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) set en el miembro **SwapEffect** ) y establece el valor del miembro **Format** en el [** \_ formato dxgi \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format). Establecemos el **miembro Count** de la [**estructura \_ \_ DESC de ejemplo de dxgi**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc) que el miembro **SampleDesc** especifica en 1 y el miembro **Quality** de **dxgi \_ Sample \_ DESC** en cero porque Flip-Model no es compatible con el suavizado de contorno de muestras múltiples (MSAA). Establecemos el miembro **BufferCount** en 2, de modo tal que la cadena de intercambio pueda usar un búfer frontal para presentar el dispositivo de pantalla y un búfer de reserva como el destino de representación.

Obtenemos el dispositivo DXGI subyacente consultando el dispositivo Direct3D 11.1. Para reducir el consumo de energía; lo cual es muy importante en el caso de dispositivos a batería, como equipos portátiles y tabletas, llamamos al método [**IDXGIDevice1::SetMaximumFrameLatency**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) con 1 como el número máximo de fotogramas del búfer de reserva que DXGI puede consultar. Esto asegura que la aplicación se represente solo después del espacio en blanco vertical.

Para crear finalmente la cadena de intercambio, necesitamos obtener la fábrica primaria del dispositivo DXGI. Llamamos a [**IDXGIDevice::GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) para obtener el adaptador del dispositivo y, a continuación, llamamos a [**IDXGIObject::GetParent**](/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) en el adaptador para obtener la fábrica primaria ([**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2)). Para crear la cadena de intercambio, llamamos a [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) con el descriptor de cadena de intercambio y la ventana principal de la aplicación.

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. Crear la vista del destino de representación

Para representar gráficos en la ventana, necesitamos crear una vista del destino de representación. Llamamos a [**IDXGISwapChain::GetBuffer**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) para obtener el búfer de reserva de la cadena de intercambio, al cual necesitaremos usar cuando creemos la vista del destino de representación. Especificamos el búfer de reserva como una textura 2D ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)). Para crear la vista del destino de representación, llamamos a [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) con el búfer de reserva de la cadena de intercambio. Debemos especificar que se dibuje en la ventana principal completa especificando el puerto de vista ([**D3D11 \_ VIEWPORT**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport)) como el tamaño completo del búfer de reserva de la cadena de intercambio. Usamos la ventanilla en una llamada a [**ID3D11DeviceContext::RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) para enlazar la ventanilla a la [fase de rasterización](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage) de la canalización. La fase de rasterización convierte la información de vector en una imagen rasterizada. En este caso, no necesitamos la conversión porque solo estamos mostrando color sólido.

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. Mostrar la imagen representada

Entramos en un bucle sin fin para representar y mostrar continuamente la escena.

En este bucle, llamamos a:

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para especificar el destino de representación como el destino de salida.
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) para borrar el destino representado en un color sólido.
3.  [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) para mostrar la imagen representada en la ventana.

Como antes habíamos establecido la latencia de fotogramas en 1, Windows, por lo general, disminuye la velocidad del bucle de representación a la velocidad de actualización de pantalla, que es normalmente alrededor de 60 Hz. Windows disminuye el bucle sin fin poniendo a la aplicación en modo de suspensión cuando llama a [**Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present). Windows pone a la aplicación en modo de suspensión hasta que se actualiza la pantalla.

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Cambiar el tamaño de la ventana de la aplicación y el búfer de la cadena de intercambio

Si el tamaño de la ventana de la aplicación cambia, la aplicación debe cambiar el tamaño de los búferes de la cadena de intercambio, recrear la vista del destino de representación y luego mostrar la imagen representada en el nuevo tamaño. Para cambiar el tamaño de los búferes de la cadena de intercambio, llamamos a [**IDXGISwapChain::ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers). En esta llamada, dejamos el número de búferes y el formato de los búferes sin modificar (el parámetro *BufferCount* en Two y el parámetro *NewFormat* en el [** \_ formato DXGI \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)). Hacemos que el tamaño del búfer de reserva de la cadena de intercambio sea el mismo que el de la ventana modificada. Después de cambiar el tamaño de los búferes de la cadena de intercambio, creamos el nuevo destino de representación y mostramos la nueva imagen representada de forma similar a cuando inicializamos la aplicación.

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes


Hemos creado un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y mostramos la imagen representada en la pantalla.

A continuación, también dibujaremos un triángulo en la pantalla.

[Crear sombreadores y dibujar primitivos](creating-shaders-and-drawing-primitives.md)

 

 