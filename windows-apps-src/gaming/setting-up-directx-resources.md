---
title: Configuración de recursos de DirectX y visualización de una imagen
description: Aquí te mostramos cómo crear un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y cómo mostrar la imagen representada en la pantalla.
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, juegos, directx, recursos, imágenes
ms.localizationpriority: medium
ms.openlocfilehash: b650f77627e02427b2861a2e6d0df7d1fc86831a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628370"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>Configuración de recursos de DirectX y visualización de una imagen



Aquí te mostramos cómo crear un dispositivo Direct3D, una cadena de intercambio y una vista de destino de representación, y cómo mostrar la imagen representada en la pantalla.

**Objetivo:** Para configurar los recursos de DirectX en una aplicación de C++ Universal Windows Platform (UWP) y mostrar un color sólido.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de elementos gráficos.

**Tiempo en completarse:** 20 minutos.

## <a name="instructions"></a>Instrucciones

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. Declaración de variables de la interfaz de Direct3D con ComPtr

Declaramos variables de interfaz de Direct3D con la [plantilla de puntero inteligente](https://msdn.microsoft.com/library/windows/apps/hh279674.aspx) ComPtr de la Biblioteca de plantillas C++ de Windows en tiempo de ejecución (WRL), para poder administrar la vigencia de esas variables de manera segura. Podemos usar esas variables para acceder a la [**clase ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) y a sus miembros. Por ejemplo:

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

Si declara [ **ID3D11RenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476582) con ComPtr, a continuación, puede usar del ComPtr **GetAddressOf** método para obtener la dirección del puntero para  **ID3D11RenderTargetView** (\*\*ID3D11RenderTargetView) para pasar a [ **ID3D11DeviceContext::OMSetRenderTargets**](https://msdn.microsoft.com/library/windows/desktop/ff476464). **OMSetRenderTargets** enlaza el destino de representación con la [fase de combinación de salida](https://msdn.microsoft.com/library/windows/desktop/bb205120) para especificar el destino de representación como el destino de salida.

La aplicación de muestra primero se inicia, luego se inicializa y se carga, y luego está lista para ejecutarse.

### <a name="2-creating-the-direct3d-device"></a>2. Crear el dispositivo Direct3D

Para usar la API de Direct3D para representar una escena, primero debemos crear un dispositivo Direct3D que represente el adaptador de pantalla. Para crear el dispositivo Direct3D, llamamos a la función [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082). Se especifican niveles 9.1 a través de 11.1 de la matriz de [ **D3D\_característica\_nivel** ](https://msdn.microsoft.com/library/windows/desktop/ff476329) valores. Direct3D recorre la matriz en orden y devuelve el nivel de características más alto compatible. Por lo tanto, para obtener el máximo nivel de función disponible, se enumeran los **D3D\_característica\_nivel** matriz entradas de mayor a menor. Pasamos el [ **D3D11\_crear\_dispositivo\_BGRA\_soporte** ](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_BGRA_SUPPORT) marca a la *marcas* parámetro para hacer Recursos de Direct3D interoperan con Direct2D. Si usamos la compilación de depuración, también pasamos el [ **D3D11\_crear\_dispositivo\_depurar** ](https://msdn.microsoft.com/library/windows/desktop/ff476107#D3D11_CREATE_DEVICE_DEBUG) marca. Para obtener más información acerca de la depuración de aplicaciones, consulta el tema sobre cómo [usar la capa de depuración para depurar aplicaciones](https://msdn.microsoft.com/library/windows/desktop/jj200584).

Obtenemos el dispositivo Direct3D 11.1 ([**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)) y el contexto de dispositivo ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) consultando el dispositivo Direct3D 11 y su contexto devueltos en [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082).

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

### <a name="3-creating-the-swap-chain"></a>3. Creación de la cadena de intercambio

A continuación, creamos la cadena de intercambio que el dispositivo usa para representar y mostrar elementos en pantalla. Nos declarar e inicializar un [ **DXGI\_intercambio\_cadena\_DESC1** ](https://msdn.microsoft.com/library/windows/desktop/hh404528) estructura para describir la cadena de intercambio. A continuación, configuramos la cadena de intercambio flip-modelo de as (es decir, una cadena de intercambio que tiene el [ **DXGI\_intercambio\_efecto\_VOLTEAR\_SEQUENTIAL** ](https://msdn.microsoft.com/library/windows/desktop/bb173077#DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL) valor establecido el **SwapEffect** miembro) y establezca el **formato** miembro [ **DXGI\_formato\_B8G8R8A8\_UNORM** ](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM). Establecemos el **recuento** miembro de la [ **DXGI\_ejemplo\_DESC** ](https://msdn.microsoft.com/library/windows/desktop/bb173072) estructura que el **SampleDesc** miembro especifica en 1 y el **calidad** miembro de **DXGI\_ejemplo\_DESC** en cero porque el modelo flip no es compatible con suavizado de contorno de ejemplo múltiples (MSAA). Establecemos el miembro **BufferCount** en 2, de modo tal que la cadena de intercambio pueda usar un búfer frontal para presentar el dispositivo de pantalla y un búfer de reserva como el destino de representación.

Obtenemos el dispositivo DXGI subyacente consultando el dispositivo Direct3D 11.1. Para reducir el consumo de energía; lo cual es muy importante en el caso de dispositivos a batería, como equipos portátiles y tabletas, llamamos al método [**IDXGIDevice1::SetMaximumFrameLatency**](https://msdn.microsoft.com/library/windows/desktop/ff471334) con 1 como el número máximo de fotogramas del búfer de reserva que DXGI puede consultar. Esto asegura que la aplicación se represente solo después del espacio en blanco vertical.

Para crear finalmente la cadena de intercambio, necesitamos obtener la fábrica primaria del dispositivo DXGI. Llamamos a [**IDXGIDevice::GetAdapter**](https://msdn.microsoft.com/library/windows/desktop/bb174531) para obtener el adaptador del dispositivo y, a continuación, llamamos a [**IDXGIObject::GetParent**](https://msdn.microsoft.com/library/windows/desktop/bb174542) en el adaptador para obtener la fábrica primaria ([**IDXGIFactory2**](https://msdn.microsoft.com/library/windows/desktop/hh404556)). Para crear la cadena de intercambio, llamamos a [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://msdn.microsoft.com/library/windows/desktop/hh404559) con el descriptor de cadena de intercambio y la ventana principal de la aplicación.

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

### <a name="4-creating-the-render-target-view"></a>4. Creación de la vista de destino de representación

Para representar gráficos en la ventana, necesitamos crear una vista del destino de representación. Llamamos a [**IDXGISwapChain::GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb174570) para obtener el búfer de reserva de la cadena de intercambio, al cual necesitaremos usar cuando creemos la vista del destino de representación. Especificamos el búfer de reserva como una textura 2D ([**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635)). Para crear la vista del destino de representación, llamamos a [**ID3D11Device::CreateRenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476517) con el búfer de reserva de la cadena de intercambio. Debemos especificar para dibujar en la ventana principal todo mediante la especificación de la ventanilla ([**D3D11\_VENTANILLA**](https://msdn.microsoft.com/library/windows/desktop/ff476260)) como el tamaño de búfer de reserva de la cadena de intercambio completo. Usamos la ventanilla en una llamada a [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) para enlazar la ventanilla a la [fase de rasterización](https://msdn.microsoft.com/library/windows/desktop/bb205125) de la canalización. La fase de rasterización convierte la información de vector en una imagen rasterizada. En este caso, no necesitamos la conversión porque solo estamos mostrando color sólido.

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

### <a name="5-presenting-the-rendered-image"></a>5. Presentar la imagen representada

Entramos en un bucle sin fin para representar y mostrar continuamente la escena.

En este bucle, llamamos a:

1.  [**ID3D11DeviceContext::OMSetRenderTargets** ](https://msdn.microsoft.com/library/windows/desktop/ff476464) para especificar el destino de representación como el destino de salida.
2.  [**ID3D11DeviceContext::ClearRenderTargetView** ](https://msdn.microsoft.com/library/windows/desktop/ff476388) para borrar el destino de representación para un color sólido.
3.  [**IDXGISwapChain::Present** ](https://msdn.microsoft.com/library/windows/desktop/bb174576) para presentar la imagen representada en la ventana.

Como antes habíamos establecido la latencia de fotogramas en 1, Windows, por lo general, disminuye la velocidad del bucle de representación a la velocidad de actualización de pantalla, que es normalmente alrededor de 60 Hz. Windows disminuye el bucle sin fin poniendo a la aplicación en modo de suspensión cuando llama a [**Present**](https://msdn.microsoft.com/library/windows/desktop/bb174576). Windows pone a la aplicación en modo de suspensión hasta que se actualiza la pantalla.

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

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. Cambiar el tamaño de búfer de la cadena de intercambio y la ventana de aplicación

Si el tamaño de la ventana de la aplicación cambia, la aplicación debe cambiar el tamaño de los búferes de la cadena de intercambio, recrear la vista del destino de representación y luego mostrar la imagen representada en el nuevo tamaño. Para cambiar el tamaño de los búferes de la cadena de intercambio, llamamos a [**IDXGISwapChain::ResizeBuffers**](https://msdn.microsoft.com/library/windows/desktop/bb174577). En esta llamada, se deja el número de búferes y el formato de los búferes sin cambios (la *BufferCount* parámetro a dos y *NewFormat* parámetro [ **DXGI\_ Dar formato a\_B8G8R8A8\_UNORM**](https://msdn.microsoft.com/library/windows/desktop/bb173059#DXGI_FORMAT_B8G8R8A8_UNORM)). Hacemos que el tamaño del búfer de reserva de la cadena de intercambio sea el mismo que el de la ventana modificada. Después de cambiar el tamaño de los búferes de la cadena de intercambio, creamos el nuevo destino de representación y mostramos la nueva imagen representada de forma similar a cuando inicializamos la aplicación.

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

[Creación de sombreadores y primitivos de dibujo](creating-shaders-and-drawing-primitives.md)

 

 




