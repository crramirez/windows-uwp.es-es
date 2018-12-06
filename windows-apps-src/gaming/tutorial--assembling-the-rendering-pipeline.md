---
title: Introducción a la representación
description: Aprende a ensamblar la canalización de representación para mostrar gráficos. Introducción a la representación.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, representar
ms.localizationpriority: medium
ms.openlocfilehash: 6724aedf898706dd4c5bf728616c918d64b2fb32
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8747713"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Marco de representación I: Introducción a la representación

En temas anteriores hemos visto cómo estructurar un juego de la Plataforma universal de Windows (UWP) y cómo definir una máquina de estados para gestionar el flujo del juego. Ahora vamos a ver cómo ensamblar el marco de representación. Echemos un vistazo a cómo el juego de muestra representa la escena de juego usando Direct3D11 (conocido comunmente como DirectX 11).

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

Direct3D 11 contiene un conjunto de API que proporcionan acceso a las características avanzadas de hardware de gráficos de alto rendimiento que pueden usarse para crear gráficos 3D para aplicaciones que usan muchos gráficos, como los juegos.

La representación de gráficos de juegos en pantalla es básicamente la representación de una secuencia de fotogramas en pantalla. En cada fotograma, tienes que representar objetos que están visibles en la escena, en función de la vista. 

Para representar un fotograma, tienes que pasar la información de la escena necesaria al hardware, de modo que se puede mostrar en la pantalla. Si quieres que aparezca cualquier cosa en la pantalla, deberás iniciar la representación tan pronto como el juego comience a ejecutarse.

## <a name="objective"></a>Objetivo

Para configurar un marco de representación básico a fin de mostrar la salida de gráficos para un juego DirectX de UWP, puedes agruparlos aproximadamente en estos tres pasos.

 1. Establecer una conexión con la interfaz gráfica
 2. Crear los recursos que necesitamos para dibujar los gráficos
 3. Mostrar los gráficos representando el fotograma

Este artículo explica cómo se representan gráficos (pasos 1 y 3).

El paso 2 se abarca en [Marco de representación II: representación de juego](tutorial-game-rendering.md); cómo configurar el marco de representación y cómo se preparan los datos antes de que puede producirse la representación.

## <a name="get-started"></a>Introducción

Antes de comenzar, debes familiarizarte con los conceptos básicos de gráficos y representación. Si no estás familiarizado con Direct3D y la representación, consulta [Términos y conceptos](#terms-and-concepts), donde encontrarás una breve descripción de los términos de gráficos y representación que se usan en este artículo.

En este juego, el objeto de clase __GameRenderer__ es el representador en este juego de muestra.  Es el responsable de crear y mantener todos los objetos Direct3D 11 y Direct2D usados para generar los elementos visuales del juego.  También mantiene una referencia al objeto __Simple3DGame__ usado para recuperar la lista de objetos a representar, así como el estado del juego para la pantalla de visualización frontal (HUD). 

En esta parte del tutorial, nos centraremos en la representación de objetos 3D en el juego.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Establecer una conexión con la interfaz gráfica

Para obtener acceso al hardware para la representación, consulta el artículo de marco UWP en [__App::Initialize__](tutorial--building-the-games-uwp-app-framework.md#appinitialize-method).

__make\_shared función__, como se muestra [abajo](#appinitialize-method), se usa para crear un __shared\_ptr__ a [__DX::DeviceResources__](#dxdeviceresources), que también proporciona acceso al dispositivo. 

En Direct3D 11, un [dispositivo](#device) se usa para asignar y destruir los objetos, representar primitivos y comunicarse con la tarjeta gráfica mediante el controlador de gráficos.

### <a name="appinitialize-method"></a>Método App::Initialize

```cpp
void App::Initialize(
    CoreApplicationView^ applicationView
    )
{
    //...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Mostrar los gráficos representando el fotograma

La escena de juego debe representarse cuando se inicia el juego. Las instrucciones para la representación empiezan en el método [__GameMain::Run__](#gameamainrun-method), tal y como se muestra a continuación.

El flujo simple es:
1. __Actualizar__
2. __Representar__
3. __Presentar__

### <a name="gamemainrun-method"></a>Método GameMain::Run

```cpp
// Comparing this to App::Run() in the DirectX 11 App template
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            // Added: Switching different game states since this game sample is more complex
            switch (m_updateState) 
            {

                // ...
                CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                // 1. Update
                // Also exist in DirectX 11 App template: Update loop to get the latest game changes
                Update();
                
                // 2. Render
                // Present also in DirectX 11 App template: Prepare to render
                m_renderer->Render();
                
                // 3. Present
                // Present also in DirectX 11 App template: Present the 
                // contents of the swap chain to the screen.
                m_deviceResources->Present(); 
                
                // Added: resetting a variable we've added to indicate that 
                // render has been done and no longer needed to render
                m_renderNeeded = false; 
            }
        }
        else
        {   
            // Present also in DirectX 11 App template
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending); 
        }
    }
    m_game->OnSuspending();  // exiting due to window close. Make sure to save state.
}
```

### <a name="update"></a>Actualizar

Consulta el artículo [Administración del flujo de los juegos](tutorial-game-flow-management.md) para obtener más información sobre la actualización de los estados de juego en los métodos [__App::Update__ y __GameMain::Update__](tutorial-game-flow-management.md#appupdate-method).

### <a name="render"></a>Representar

La representación se implementa mediante una llamada al método [__GameRenderer::Render__](#gamerendererrender-method) en __GameMain::Run__.

Si la [representación en estéreo](#stereo-rendering) está habilitada, hay dos fases de representación: una para el ojo derecho y otra para el ojo izquierdo. En cada pase de representación, enlazamos el destino de representación y la [vista de galería de símbolos de profundidad](#depth-stencil-view) al dispositivo. También borramos la vista de galería de símbolos de profundidad después.

> [!Note]
> La representación en estéreo se puede conseguir con otros métodos, como el estéreo de pase único mediante la creación de instancias de vértices o los sombreadores de geometría. El método de dos pases de representación es más lento, pero más conveniente para lograr la representación en estéreo.

Una vez que existe el juego y los recursos se cargan, actualiza la [matriz de proyección](#projection-transform-matrix), una vez en cada pase de representación. Los objetos son ligeramente diferentes desde cada vista. A continuación, configura la [canalización de representación de gráficos](#rendering-pipeline). 

> [!Note]
> Consulta [Crear y cargar recursos gráficos de DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) para obtener más información sobre la carga de recursos.

En esta muestra de juego, el representador está diseñado para usar un diseño de vértices estándar en todos los objetos. Esto simplifica el diseño del sombreador y permite cambios fácilmente entre los sombreadores, independientemente de la geometría de los objetos.

#### <a name="gamerendererrender-method"></a>Método GameRenderer::Render

Establece el contexto de Direct3D para usar un diseño de vértice de entrada. Los objetos del diseño de entrada describen cómo se transmiten los datos del búfer de vértices en la [canalización de representación](#rendering-pipeline). 

A continuación, establecemos el contexto de Direct3D para usar los [búferes de constantes](#constant-buffers) definidos anteriormente, usados por el estado de la canalización del [sombreador de vértices](#vertex-shaders-and-pixel-shaders) y del [sombreador de píxeles](#vertex-shaders-and-pixel-shaders). 

> [!Note]
> Consulta [Marco de representación II: representación de juego](tutorial-game-rendering.md) para obtener más información acerca de la definición de los búferes de constantes.

Dado que el mismo diseño de entrada y conjunto de búferes de constantes se usa para todos los sombreadores que se encuentran en la canalización, se configura una vez por fotograma.

```cpp
void GameRenderer::Render()
{
    bool stereoEnabled = m_deviceResources->GetStereoState();

    auto d3dContext = m_deviceResources->GetD3DDeviceContext();
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx

            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            
            // Clears the depth stencil view.
            // A depth stencil view contains the format and buffer to hold depth and stencil info.
            // For more info about depth stencil view, go to: 
            // https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-stencil-view--dsv-
            // A depth buffer is used to store depth information to control which areas of 
            // polygons are rendered rather than hidden from view. To learn more about a depth buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/depth-buffers
            // A stencil buffer is used to mask pixels in an image, to produce special effects. 
            // The mask determines whether a pixel is drawn or not,
            // by setting the bit to a 1 or 0. To learn more about a stencil buffer,
            // go to: https://docs.microsoft.com/windows/uwp/graphics-concepts/stencil-buffers

            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView *const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() }; 
            
            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);
            
            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap()); 
        }

        // Render the scene objects
        if (m_game != nullptr && m_gameResourcesLoaded && m_levelResourcesLoaded)
        {
            // This section is only used after the game state has been initialized and all device
            // resources needed for the game have been created and associated with the game objects.
            if (stereoEnabled)
            {
                // When doing stereo, it is necessary to update the projection matrix once per rendering pass.

                auto orientation = m_deviceResources->GetOrientationTransform3D();

                ConstantBufferChangeOnResize changesOnResize;

                // Apply either a left or right eye projection, which is an offset from the middle
                XMStoreFloat4x4(
                    &changesOnResize.projection,
                    XMMatrixMultiply(
                        XMMatrixTranspose(
                            i == 0 ?
                            m_game->GameCamera()->LeftEyeProjection() :
                            m_game->GameCamera()->RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.Get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrame;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrame.view,
                XMMatrixTranspose(m_game->GameCamera()->View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.Get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrame,
                0,
                0
                );

            // Setup the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, 
            // go to https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476454.aspx
            d3dContext->IASetInputLayout(m_vertexLayout.Get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers.
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476491.aspx

            d3dContext->VSSetConstantBuffers(0, 1, m_constantBufferNeverChanges.GetAddressOf());
            d3dContext->VSSetConstantBuffers(1, 1, m_constantBufferChangeOnResize.GetAddressOf());
            d3dContext->VSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->VSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476470.aspx

            d3dContext->PSSetConstantBuffers(2, 1, m_constantBufferChangesEveryFrame.GetAddressOf());
            d3dContext->PSSetConstantBuffers(3, 1, m_constantBufferChangesEveryPrim.GetAddressOf());
            d3dContext->PSSetSamplers(0, 1, m_samplerLinear.GetAddressOf());

            auto objects = m_game->RenderObjects();
            for (auto object = objects.begin(); object != objects.end(); object++)
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                (*object)->Render(d3dContext, m_constantBufferChangesEveryPrim.Get()); /
            }
        }
        else
        {
            const float ClearColor[4] = {0.5f, 0.5f, 0.8f, 1.0f};

            // Only need to clear the background when not rendering the full 3D scene since
            // the 3D world is a fully enclosed box and the dynamics prevents the camera from
            // moving outside this space.
            if (i > 0)
            {
                // Doing the Right Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), ClearColor);
            }
            else
            {
                // Doing the Mono or Left Eye View.
                d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), ClearColor);
            }
        }

        // Start of 2D rendering
        d3dContext->BeginEventInt(L"D2D BeginDraw", 1);
        d2dContext->BeginDraw();

        // ...
    }
}
```

### <a name="primitive-rendering"></a>Representación de los primitivos

Al representar la escena, recorres todos los objetos que se deben representar. Los siguientes pasos se repiten para cada objeto (primitivo).

* Actualizar el búfer de constantes (__m\_constantBufferChangesEveryPrim__) con la [matriz de transformación universal](#world-transform-matrix) del modelo y la información de materiales.
* __m\_constantBufferChangesEveryPrim__ contiene los parámetros para cada objeto.  Incluye la matriz de transformación objeto a mundo, así como propiedades de materiales, como el color y el exponente especular para calcular la iluminación.
* Definir el contexto de Direct3D para usar el diseño de vértice de entrada para que los datos de objeto de malla se transmitan en la fase de ensamblador-entrada (IA) de la [canalización de representación](#rendering-pipeline)
* Establecer el contexto de Direct3D para usar un [búfer de índices](#index-buffer) en la fase de IA. Proporciona la información del primitivo: tipo, orden de datos.
* Enviar una llamada Draw para dibujar al primitivo indexado sin instancia. El método __GameObject::Render__ actualiza el [búfer de constantes](#constant-buffer-or-shader-constant-buffer) primitivas con los datos específicos a un primitivo determinado. El resultado de todo esto es una llamada a __DrawIndexed__ en el contexto para dibujar la geometría de cada primitivo. En concreto, esta llamada a Draw coloca comandos y datos en la cola para la unidad de procesos de gráficos (GPU), parametrizados por los datos del búfer de constantes. Cada llamada a Draw ejecuta el [sombreador de vértices](#vertex-shaders-and-pixel-shaders) una vez por vértice y después el [sombreador de píxeles](#vertex-shaders-and-pixel-shaders) una vez por cada píxel de cada triángulo del primitivo. Las texturas forman parte del estado que el sombreador de píxeles usa para llevar a cabo la representación.

Razones para varios búferes de constantes:
    * El juego usa múltiples búferes de constantes, pero sólo necesita actualizar dichos búferes una vez por primitivo. Como se ha mencionado antes, los búferes de constantes son como entradas para los sombreadores que se ejecutan para cada primitivo. Algunos datos son estáticos (__m\_constantBufferNeverChanges__); algunos datos son constantes para todo el fotograma (__m\_constantBufferChangesEveryFrame)__, como la posición de la cámara; y algunos datos son específicos del primitivo, como su color y sus texturas (__m\_constantBufferChangesEveryPrim__)
    * El [representador](#renderer) del juego separa estas entradas en distintos búferes de constantes para optimizar el ancho de banda de la memoria que usan la CPU y la GPU. Esta medida ayuda a minimizar la cantidad de datos de los que la GPU debe realizar un seguimiento. La GPU tiene una gran cola de comandos y que, cada vez que el juego llama a __Draw__, ese comando se pone a la cola junto con sus datos asociados. Cuando el juego actualiza el búfer de constantes de primitivos y envía el siguiente comando __Draw__, el controlador de gráficos agrega este comando siguiente y los datos asociados a la cola. Si el juego dibuja 100 primitivos, podría haber potencialmente 100 copias de los datos del búfer de constantes en la cola. Para minimizar la cantidad de datos que el juego envía a la GPU, el juego use un búfer de constantes de primitivos independiente que solo contiene las actualizaciones de cada primitivo.

#### <a name="gameobjectrender-method"></a>Método GameObject::Render

```cpp
void GameObject::Render(
    _In_ ID3D11DeviceContext *context,
    _In_ ID3D11Buffer *primitiveConstantBuffer
    )
{
    if (!m\_active || (m\_mesh == nullptr) || (m_normalMaterial == nullptr))
    {
        return;
    }

    ConstantBufferChangesEveryPrim constantBuffer;

    // Put the model matrix info into a constant buffer, in world matrix.
    XMStoreFloat4x4(
        &constantBuffer.worldMatrix,
        XMMatrixTranspose(ModelMatrix())
        );

    // Check to see which material to use on the object.
    // If a collision (a hit) is detected, GameObject::Render checks the current context, which 
    // indicates whether the target has been hit by an ammo sphere. If the target has been hit, 
    // this method applies a hit material, which reverses the colors of the rings of the target to 
    // indicate a successful hit to the player. Otherwise, it applies the default material 
    // with the same method. In both cases, it sets the material by calling Material::RenderSetup, 
    // which sets the appropriate constants into the constant buffer. Then, it calls 
    // ID3D11DeviceContext::PSSetShaderResources to set the corresponding texture resource for the 
    // pixel shader, and ID3D11DeviceContext::VSSetShader and ID3D11DeviceContext::PSSetShader 
    // to set the vertex shader and pixel shader objects themselves, respectively.

    if (m_hit && m_hitMaterial != nullptr)
    {
        m_hitMaterial->RenderSetup(context, &constantBuffer);
    }
    else
    {
        m_normalMaterial->RenderSetup(context, &constantBuffer);
    }

    // Update the primitive constant buffer with the object model's info.
    context->UpdateSubresource(primitiveConstantBuffer, 0, nullptr, &constantBuffer, 0, 0);

    // Render the mesh.
    // See MeshObject::Render method below.
    m_mesh->Render(context);
}

#### MeshObject::Render method

void MeshObject::Render(\_In\_ ID3D11DeviceContext *context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32 stride = sizeof(PNTVertex);
    uint32 offset = 0;

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    context->IASetVertexBuffers(0, 1, m_vertexBuffer.GetAddressOf(), &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476453.aspx
    context->IASetIndexBuffer(m_indexBuffer.Get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476455.aspx
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, go to: https://msdn.microsoft.com/library/windows/desktop/ff476409.aspx
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="present"></a>Presentar

Llamamos al método __DX::DeviceResources::Present__ para colocar el contenido que hemos puesto en los búferes y mostrarlo.

Usamos el término cadena de intercambio para una colección de búferes que se usan para mostrar fotogramas al usuario. Cada vez que una aplicación presenta un nuevo marco para mostrar, el primer búfer de la cadena de intercambio toma el lugar del búfer que se muestra. Este proceso se denomina intercambio o inversión. Para obtener más información, consulta [Cadenas de intercambio](../graphics-concepts/swap-chains.md).

* El método actual de la interfaz __IDXGISwapChain1______ le indica a [DXGI](#dxgi) que bloquee hasta la sincronización vertical (VSync), con lo que la aplicación pasa a modo de suspensión hasta la próxima VSync. Esto garantiza que no pierdas ningún ciclo representando fotogramas que nunca se mostrarán en la pantalla.
* El método __DiscardView__ de la interfaz __ID3D11DeviceContext3__ descarta el contenido del [destino de representación](#render-target). Esta es una operación válida únicamente cuando el contenido existente se sobrescribirá por completo. Si se usan rectángulos de modificación o desplazamiento, esta llamada debe quitarse.
* Con el mismo método __DiscardView__, descarta el contenido de la [galería de símbolos de profundidad](#depth-stencil).
* El método __HandleDeviceLost__ se usa para administrar el escenario si el [dispositivo](#device) se quita. Si el dispositivo se quitó con una desconexión o una actualización del controlador, debes volver a crear todos los recursos del dispositivo. Para obtener más información, consulta [Controlar escenarios cuando se quitan dispositivos en Direct3D11](handling-device-lost-scenarios.md).

> [!Tip]
> Para lograr una velocidad de fotogramas suave, debes asegurarte de que la cantidad de trabajo para representar un fotograma encaje con el tiempo entre VSyncs.

```cpp
// Present the contents of the swap chain to the screen.
void DX::DeviceResources::Present()
{
    // The first argument instructs DXGI to block until VSync, putting the application
    // to sleep until the next VSync. This ensures we don't waste any cycles rendering
    // frames that will never be displayed to the screen.
    HRESULT hr = m_swapChain->Present(1, 0);

    // Discard the contents of the render target.
    // This is a valid operation only when the existing contents will be entirely
    // overwritten. If dirty or scroll rects are used, this call should be removed.
    m_d3dContext->DiscardView(m_d3dRenderTargetView.Get());

    // Discard the contents of the depth-stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.Get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    // For more info about how to handle a device lost scenario, go to:
    // https://docs.microsoft.com/windows/uwp/gaming/handling-device-lost-scenarios
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        DX::ThrowIfFailed(hr);
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Este artículo ha explicado cómo se representan los gráficos en la pantalla y ha proporcionado una breve descripción de algunos de los términos de representación usados. Puedes obtener más información sobre la representación en el artículo [Marco de representación II: Representación de juego](tutorial-game-rendering.md) y aprender a preparar los datos necesarios antes de la representación.

## <a name="terms-and-concepts"></a>Términos y conceptos

### <a name="simple-game-scene"></a>Escena sencilla de juego

Una sencilla escena de juego se compone de unos pocos objetos con varias fuentes de luz.

La forma de un objeto está definida por un conjunto de coordenadas X, Y, Z en el espacio. La ubicación de representación real en el mundo del juego puede determinarse al aplicar una matriz de transformación a coordinadas X, Y, Z de posición. También puede tener un conjunto de coordenadas de textura: U y V, que especifican cómo se aplica un material al objeto. Esto define las propiedades de la superficie del objeto y te da la capacidad de ver si un objeto tiene una superficie rugosa como la de una pelota de tenis o una superficie brillante y suave como la de una bola usada para jugar a los bolos.

La información de la escena y el objeto se usa por el marco de representación para volver a crear la escena fotograma a fotograma, haciendo que cobre vida en el monitor de la pantalla.

### <a name="rendering-pipeline"></a>Canalización de representación

La canalización de representación es el proceso en el que la información de la escena en 3D se convierte en una imagen que aparece en pantalla. En Direct3D 11, esta canalización es programable. Puedes adaptar las fases para que sean compatibles con tus necesidades de representación. Las fases que incluyen núcleos de sombreador comunes pueden programarse mediante el lenguaje de programación HLSL. Es también conocida como la canalización de la representación de gráficos o simplemente la canalización.

Para ayudarte a crear esta canalización, debes estar familiarizado con lo siguiente:
* [HLSL](#HLSL). Se recomienda el uso de HLSL Shader Model 5.1 y superior para juegos DirectX de UWP.
* [Sombreadores](#Shaders)
* [Sombreadores de vértices y píxeles](#vertext-shaders-pixel-shaders)
* [Fases de sombreador](#shader-stages)
* [Diversos formatos de archivo de sombreador](#various-shader-file-formats)

Para obtener más información, consulta [Comprender la canalización de representación de Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/dn643746.aspx) y [Canalización de gráficos](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="hlsl"></a>HLSL

HLSL es el lenguaje de sombreado de nivel alto para DirectX. Con HLSL puedes crear sombreadores programables como en C para la canalización de Direct3D. Para obtener más información, consulta [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561.aspx).

#### <a name="shaders"></a>Sombreadores

Los sombreadores se pueden considerar como un conjunto de instrucciones que determinan cómo aparece la superficie de un objeto cuando se representa. Los que se programan mediante HLSL se conocen como sombreadores HLSL. Los archivos de código de origen para sombreadores [HLSL])(#hlsl) tienen la extensión de archivo .hlsl. Estos sombreadores se pueden compilar en tiempo de ejecución o creación y se pueden establecer en tiempo de ejecución en la fase de canalización apropiada; un objeto de sombreador compilado tiene una extensión de archivo .cso.

Los sombreadores de Direct3D 9 se pueden diseñar con el modelo de sombreador 1, 2 y 3; los sombreadores de Direct3D 10 solo se pueden diseñar en el modelo de sombreador 4. Los sombreadores de Direct3D 11 se pueden diseñar en el modelo de sombreador 5. Direct3D 11.3 y Direct3D 12 se pueden diseñar en el modelo de sombreador 5.1, y Direct3D 12 también se puede diseñaren el modelo de sombreador 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Sombreadores de vértices y píxeles

Los datos entran en la canalización de gráficos como una secuencia de primitivos y se procesan mediante distintos sombreadores, como los sombreadores de vértices y de píxeles. 

Los sombreadores de vértices procesan vértices, normalmente realizando operaciones como transformar, enmascarar e iluminar.  Los sombreadores de píxeles permiten técnicas de sombreado enriquecidas, como el posprocesamiento y la iluminación por píxel. Combina variables de constantes, datos de textura, valores interpolados por vértices y otros datos para producir salidas por píxel. 

#### <a name="shader-stages"></a>Fases de sombreador

Una secuencia de estos diferentes sombreadores definidos para procesar esta secuencia de primitivos se conoce como fase de sombreador en una canalización de representación. Las fases reales dependen de la versión de Direct3D, pero normalmente incluyen las fases de vértices, geometría y píxeles. También hay otras fases, como los sombreadores de casco y dominio para teselación y el sombreador de cálculo. Todas estas fases son completamente programables mediante el [HLSL])(#hlsl). Para obtener más información, consulta [Canalización de gráficos](https://msdn.microsoft.com/library/windows/desktop/ff476882.aspx).

#### <a name="various-shader-file-formats"></a>Diversos formatos de archivo de sombreador

Extensiones de archivo de código de sombreador:
    * Un archivo con la extensión .hlsl contiene código fuente [HLSL])(#hlsl).
    * Un archivo con la extensión .cso contiene un objeto sombreador compilado.
    * Un archivo con la extensión. h es un archivo de encabezado, pero en un contexto de código de sombreador, este archivo de encabezado define una matriz de bytes que contiene datos del sombreador.
    * Un archivo .hlsli contiene el formato de los búferes de constantes. En la muestra de juego, el archivo es __Sombreadores__>__ConstantBuffers.hlsli__.

> [!Note]
> El sombreador se inserta cargando un archivo .cso en tiempo de ejecución o agregando el archivo. h en el código ejecutable. Pero no se deben usar ambos para el mismo sombreador.

### <a name="deeper-understanding-of-directx"></a>Conocimiento más exhaustivo de DirectX

Direct3D 11 es un conjunto de API que nos ayudan a crear gráficos para aplicaciones con gran cantidad de gráficos, como los juegos, donde necesitamos una buena tarjeta gráfica para procesar un cálculo intensivo. Esta sección explica brevemente los conceptos de programación de gráficos de Direct3D 11: recurso, subrecurso, dispositivo y contexto de dispositivo.

#### <a name="resource"></a>Recurso

Para aquellos que son nuevos, los recursos (también conocidos como recursos de dispositivo) son como información sobre la representación de un objeto como texturas, posición y color. Los recursos proporcionan datos a la canalización y definen lo que se representa en la escena. Los recursos se pueden cargar desde el medio de juegos o crearse de forma dinámica en tiempo de ejecución.

Un recurso es, de hecho, un área en la memoria a la que puede obtener acceso la [canalización](#rendering-pipeline) de Direct3D. Para que la canalización pueda obtener acceso a la memoria de forma eficiente, los datos que se proporcionan a la canalización (por ejemplo, geometría de entrada, recursos del sombreador y texturas) deben almacenarse en un recurso. Hay dos tipos de recursos de los que derivan todos los recursos de Direct3D: un búfer o una textura. Se pueden activar hasta 128 recursos para cada fase de la canalización. Para obtener más información, consulta [Recursos](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Subrecurso

El término subrecurso hace referencia al subconjunto de un recurso. Direct3D puede hacer referencia a un recurso completo o a los subconjuntos de un recurso. Puedes obtener más información en [Subrecurso](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Galería de símbolos de profundidad

Una recurso de galería de símbolos de profundidad contiene el formato y el búfer para contener la información de la galería de símbolos de profundidad. Se crea mediante un recurso de textura. Para obtener más información sobre cómo crear un recurso de galería de símbolos de profundidad, consulta [Configurar la funcionalidad de la galería de símbolos de profundidad](https://msdn.microsoft.com/library/windows/desktop/bb205074.aspx). Accedemos al recurso de galería de símbolos de profundidad a través de la vista de galería de símbolos de profundidad que se implementa mediante la interfaz [ID3D11DepthStencilView](https://msdn.microsoft.com/library/windows/desktop/ff476377.aspx).

La información de profundidad nos dice qué áreas de polígonos se representan en lugar de ocultarse de la vista. La información de la galería de símbolos nos indica qué píxeles se enmascaran. Se puede usar para producir efectos especiales, ya que determina si se dibuja un píxel o no; establece el bit a 1 o 0. 

Para obtener más información, consulta: [Vista de galería de símbolos de profundidad](../graphics-concepts/depth-stencil-view--dsv-.md), [búfer de profundidad](../graphics-concepts/depth-buffers.md), y [búfer de galería de símbolos](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Destino de representación

Un destino de representación es un recurso que podemos escribir al final de un pase de representación. Normalmente se crea mediante el método [ID3D11Device::CreateRenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476517.aspx), usando el búfer de reserva de la cadena de intercambio (que también es un recurso) como el parámetro de entrada. 

Cada destino de representación debe tener también una vista de galería de símbolos de profundidad correspondiente cuando usamos [OMSetRenderTargets](https://msdn.microsoft.com/library/windows/desktop/ff476464.aspx) para establecer el destino de representación antes de usarlo; también requiere una vista de galería de símbolos de profundidad. Accedemos el recurso de destino de representación a través de la vista de destino de representación implementada con la interfaz [ID3D11RenderTargetView](https://msdn.microsoft.com/library/windows/desktop/ff476582.aspx). 

#### <a name="device"></a>Dispositivo

Para los que no conocen Direct3D 11, un dispositivo es una manera de asignar y destruir objetos, representar primitivos y comunicarse con la tarjeta gráfica mediante el controlador de gráficos. 

Para una explicación más precisa, un dispositivo Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, transforma, ilumina y rasteriza una imagen en una superficie. Para obtener más información, consulta [Dispositivos](../graphics-concepts/devices.md).

Un dispositivo está representado mediante la interfaz [ID3D11Device](https://msdn.microsoft.com/library/windows/desktop/ff476379.aspx). En otras palabras, la interfaz ID3D11Device representa un adaptador de pantalla virtual y se usa para crear los recursos que pertenecen a un dispositivo. 

Ten en cuenta que hay diferentes versiones de ID3D11Device, [ID3D11Device5](https://msdn.microsoft.com/library/windows/desktop/mt492478.aspx) es la versión más reciente y agrega nuevos métodos a los de ID3D11Device4. Para obtener más información sobre cómo se comunica Direct3D con el hardware subyacente, consulta [Arquitectura del modelo de controlador de dispositivo de Windows (WDDM)](https://docs.microsoft.com/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Cada aplicación debe tener al menos un dispositivo, la mayoría de las aplicaciones solo crea un dispositivo. Crea un dispositivo para uno de los controladores de hardware instalados en el equipo mediante una llamada a __D3D11CreateDevice__ o __D3D11CreateDeviceAndSwapChain__ y especificando el tipo de controlador con la marca D3D\_DRIVER\_TYPE. Cada dispositivo puede usar uno o varios contextos de dispositivo, según las funciones deseadas. Para más información consulta [Función D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082.aspx).

#### <a name="device-context"></a>Contexto de dispositivo

Un contexto de dispositivo se usa para establecer el estado de la [canalización](#rendering-pipeline) y generar comandos de representación usando [recursos](#resource) propiedad de un [dispositivo](#device). 

Direct3D 11 implementa dos tipos de contextos de dispositivo, uno para la representación inmediata y otro para la representación diferida; ambos contextos se representan con una interfaz [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476385.aspx).  

Las interfaces __ID3D11DeviceContext__ tienen diferentes versiones; __ID3D11DeviceContext4__ agrega nuevos métodos a los de __ID3D11DeviceContext3__.

Nota: __ID3D11DeviceContext4__ se introduce en la Windows 10 Creators Update y es la versión más reciente de la interfaz __ID3D11DeviceContext__. Las aplicaciones orientadas a Windows 10 Creators Update deben usar esta interfaz en lugar de las versiones anteriores. Para obtener más información, consulta [ID3D11DeviceContext4](https://msdn.microsoft.com/library/windows/desktop/mt492481.aspx).

#### <a name="dxdeviceresources"></a>DX::DeviceResources

La clase __DX::DeviceResources__ está en los archivos .h de __DeviceResources.cpp__/____ y controla todos los recursos de dispositivo de DirectX. En el proyecto de juego de muestra y en el proyecto de plantilla de DirectX 11 App, estos archivos están en la carpeta __Commons__. Puedes conseguir la versión más reciente de estos archivos al crear un nuevo proyecto de plantilla DirectX 11 App en Visual Studio 2015 o posterior.

### <a name="buffer"></a>Búfer

Un recurso de búfer es una colección de datos completos agrupados en elementos. Puedes usar búferes para almacenar una gran cantidad de datos, como vectores de posiciones, vectores normales, coordenadas de textura en un búfer de vértices, índices en un búfer de índices o estados de dispositivo. Los elementos de búfer pueden incluir valores de datos agrupados (por ejemplo, valores de superficie R8G8B8A8), enteros de 8 bits únicos o cuatro valores de punto flotante de 32 bits.

Existen tres tipos de búferes disponibles: búfer de vértices, búfer de índices y búfer de constantes.

#### <a name="vertex-buffer"></a>Búfer de vértices

Contiene los datos de vértices que se usan para definir la geometría. Los datos de vértice incluyen coordenadas de posición, datos de color, datos de coordenadas de textura, datos normales, etc. 

#### <a name="index-buffer"></a>Búfer de índices

Contienen desplazamientos de enteros en búferes de vértices y se usan para representar primitivos de forma más eficiente. Un búfer de índices contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se utiliza para identificar un vértice en un búfer de vértices.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Búferes de constantes o búferes de constante del sombreador

Te permite suministrar de forma eficiente datos de sombreador a la canalización. Puedes usar búferes de constantes como entradas para los sombreadores que se ejecutan para cada primitivo y almacenan los resultados de la fase de salida en secuencias de la canalización de representación. Conceptualmente, un búfer de constantes es similar a un búfer de vértices de un solo elemento.

#### <a name="design-and-implementation-of-buffers"></a>Diseño e implementación de búferes

Puedes diseñar búferes en función del tipo de datos. Por ejemplo, en nuestro juego de muestra, se crea un búfer de datos estáticos, otro para los datos que son constantes para todo el fotograma y otro para los datos que son específicos para un primitivo.

Todos los tipos de búfer están encapsulados por la interfaz __ID3D11Buffer__ y puedes crear un recurso de búfer mediante una llamada a __ID3D11Device::CreateBuffer__. Sin embargo, un búfer debe estar enlazado a la canalización antes de que se puede acceder a él. Los búferes se pueden enlazar a varias fases de canalización simultáneamente para la lectura. Un búfer también se puede enlazar a una fase de canalización única para la escritura; sin embargo, no se puede enlazar el mismo búfer para lectura y escritura al mismo tiempo.

Enlaza búferes a:
    * La fase de ensamblador de entrada, llamando a métodos __ID3D11DeviceContext__ como __ID3D11DeviceContext::IASetVertexBuffers__ y __ID3D11DeviceContext::IASetIndexBuffer__
    * La fase de salida de secuencia llamando a __ID3D11DeviceContext::SOSetTargets__
    * La fase del sombreador, llamando a métodos de sombreado como __ID3D11DeviceContext::VSSetConstantBuffers__

Para obtener más información consulta [Introducción a los búferes en Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898.aspx).

### <a name="dxgi"></a>DXGI

Infraestructura de gráficos de Microsoft DirectX (DXGI) es un subsistema nuevo que se introdujo con Windows Vista que encapsula algunas de las tareas de bajo nivel que se necesitan Direct3D 10, 10.1, 11 y 11.1. Se debe tener especial cuidado al usar DXGI en una aplicación multiproceso, para garantizar que no se producen interbloqueos. Para obtener más información, consulta [Infraestructura de gráficos DirectX (DXGI): Procedimientos recomendados- Multiproceso](https://msdn.microsoft.com/library/windows/desktop/ee417025.aspx#multithreading_and_dxgi)

### <a name="feature-level"></a>Nivel de características

El nivel de características es un concepto que se introdujo en Direct3D 11 para controlar la diversidad de tarjetas de vídeo en equipos nuevos y existentes. Un nivel de características es un conjunto bien definido de funcionalidades en la unidad de procesamiento de gráficos (GPU). 

Cada tarjeta de vídeo implementa un cierto nivel de funcionalidad DirectX, en función de las GPU instaladas. En versiones anteriores de Microsoft Direct3D, podías averiguar la versión de Direct3D de la tarjeta de vídeo implementada y, a continuación, programar la aplicación de la manera correspondiente. 

Con el nivel de características, cuando creas un dispositivo, puedes intentar crear un dispositivo para el nivel de características que deseas solicitar. Si la creación de dispositivos funciona, ese nivel de característica existe, si no, el hardware no admite ese nivel de características. Puedes intentar volver a crear un dispositivo a un nivel de características inferior o puedes elegir salir de la aplicación. Por ejemplo, el nivel de característica 12\_0 requiere Direct3D 11.3 o Direct3D 12 y el modelo de sombreador 5.1. Para obtener más información, consulta [Niveles de característica de Direct3D: Introducción para cada nivel de características](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx#Overview).

Con los niveles de característica, puedes desarrollar una aplicación para Direct3D9, Microsoft Direct3D10 o Direct3D11 y, a continuación, se ejecuta en hardware 9, 10 u 11 (salvo algunas excepciones). Para más información, consulta [Niveles de características Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876.aspx).

### <a name="stereo-rendering"></a>Representación en estéreo

La representación en estéreo se utiliza para mejorar la sensación de profundidad. Usa dos imágenes, una desde el ojo izquierdo y la otra desde el ojo derecho, para mostrar una escena en la pantalla. 

Matemáticamente, aplicamos una matriz de proyección estéreo, que es un desplazamiento horizontal ligeramente hacia la derecha y a la izquierda de la matriz de proyección mono normal para lograr esto.

Hicimos dos pases de representación para lograr la representación en estéreo en este juego de muestra:
* Enlazar al destino de representación derecho, aplicar la proyección derecha y dibujar el objeto primitivo.
* Enlazar al destino de representación izquierdo, aplicar la proyección izquierda y dibujar el objeto primitivo.

### <a name="camera-and-coordinate-space"></a>Espacio de coordenadas y cámara

El juego cuenta con el código para actualizar el mundo en su propio sistema de coordenadas (en ocasiones llamado espacio del mundo o espacio de la escena). Todos los objetos, incluida la cámara, se posicionan y se orientan en este espacio. Para obtener más información, consulta [Sistemas de coordenadas](../graphics-concepts/coordinate-systems.md).

Un sombreador de vértices se encarga del pesado trabajo de convertir las coordenadas del modelo en coordenadas del dispositivo con el siguiente algoritmo (donde V es un vector y M es una matriz).

V (dispositivo) = V (modelo) x M (modelo a mundo) x M (mundo a vista) x M (vista a dispositivo).

donde: 
* M (modelo a mundo) es una matriz de transformación para coordenadas de modelo a coordenadas de mundo, también conocida como la [Matriz de transformación de mundo](#world-transform-matrix). Lo proporciona el primitivo.
* M (mundo a vista) es una matriz de transformación para coordenadas de mundo a coordenadas de vista, también conocida como la [Matriz de transformación de vista](#view-transform-matrix).
    * Lo proporciona la matriz de vista de la cámara. Se define por la posición de la cámara junto con los vectores de vista (el vector de vista que apunta directamente a la escena desde la cámara y el vector de vista hacia arriba situado hacia arriba y perpendicular a la primera vista).
    * En el juego de muestra, __m\_viewMatrix__ es la matriz de transformación de vista y se calcula mediante __Camera::SetViewParams__ 
* M (vista a dispositivo) es una matriz de transformación para coordenadas de vista a coordenadas de dispositivo, también conocida como la [Matriz de transformación de proyección](#projection-transform-matrix).
    * Lo proporciona la proyección de la cámara. Proporciona información sobre la cantidad de ese espacio que está realmente visible en la escena final. El campo de visión (FoV), relación de aspecto y los planos de recorte definen la matriz de transformación de proyección.
    * En el juego de muestra, __m\_projectionMatrix__ define la transformación a las coordenadas de proyección, que se calculan con __Camera::SetProjParams__ (para la proyección estéreo, usas dos matrices de proyección, una para la vista de cada ojo.) 

El código de sombreador en VertexShader.hlsl se carga con estos vectores y matrices desde los búferes de constantes y realiza esta transformación para cada vértice.

### <a name="coordinate-transformation"></a>Transformación de coordenadas

Direct3D utiliza tres transformaciones para cambiar las coordenadas del modelo 3D en coordenadas de píxeles (espacio de pantalla). Estas transformaciones son transformaciones de mundo, de vista y de proyección. Para obtener más información, consulta [Introducción a las transformaciones](../graphics-concepts/transform-overview.md).

#### <a name="world-transform-matrix"></a>Matriz de transformación de mundos

Una transformación de mundo cambia coordenadas del espacio modelo, donde se definen vértices con respecto al origen local de un modelo, al espacio del mundo, donde se definen los vértices en relación a un origen común a todos los objetos de una escena. En esencia, la transformación de mundos coloca un modelo en el mundo, de ahí su nombre. Para obtener más información, consulta [Transformación de mundos](../graphics-concepts/world-transform.md).

####  <a name="view-transform-matrix"></a>Matriz de transformación de vistas

La transformación de vistas localiza el visor en el espacio del mundo, transformando los vértices en espacio de cámara. En espacio de cámara, la cámara o el visor, están en el origen, mirando hacia la dirección positiva z. Para obtener más información, consulta [Transformación de vistas](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matriz de transformación de proyecciones

La transformación de proyecciones convierte el tronco de visualización en una forma de cubo. Un tronco de visualización es un volumen 3D en una escena colocada en relación con la cámara de la ventanilla. Una ventanilla es un rectángulo 2D en el que se proyecta una escena 3D. Para obtener más información, consulta [Ventanillas y recortes](../graphics-concepts/viewports-and-clipping.md).

Dado que el extremo más próximo del frustum de visualización es menor que el más alejado, tiene el efecto de expandir objetos que están cerca de la cámara; así es cómo se aplica la perspectiva a la escena. Por tanto, los objetos que están más próximos al jugador, aparecen más grandes; los objetos que están más lejos, aparecen más pequeños.

Matemáticamente, la transformación de proyecciones es una matriz que normalmente es una proyección de perspectiva y escala. Funciona como el objetivo de una cámara. Para obtener más información, consulta [Transformación de proyecciones](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Estado de muestra

El estado de muestra determina cómo se recogen muestras de los datos de textura usando modelos de direccionamiento de textura, filtrado y nivel de detalle. El muestreo se realiza cada vez que se lee un píxel de textura, o elemento de textura, de una textura.

Una textura contiene una matriz de elementos o píxeles de textura. La posición de cada elemento de textura se denota mediante (u,v), donde u es el ancho y v es el alto, y se asigna entre 0 y 1 en función de la altura y anchura de la textura. Las coordenadas de textura resultante se usan para direccionar un elemento de textura al realizar la muestra de una textura.

Cuando las coordenadas de textura están por debajo de 0 o por encima de 1, el modo de dirección de textura define cómo direcciona una ubicación de elemento de textura la coordenada de textura. Por ejemplo, al utilizar __TextureAddressMode.Clamp__, cualquier coordenada fuera del rango 0-1 queda restringido a un valor máximo de 1 y un valor mínimo de 0 antes de realizar las muestras.

Si la textura es demasiado grande o demasiado pequeña para el polígono, se filtra para ocupar el espacio. Un filtro de ampliación amplía una textura, un filtro de reducción la reduce para encajar en un área más pequeña. La ampliación de textura repite el elemento de textura de muestra para la dirección o direcciones que generan una imagen más desenfocada. La reducción de textura es más complicada porque requiere combinar más de un valor de elemento de textura en un valor único. Esto puede provocar un efecto escalonado (aliasing) o bordes irregulares, dependiendo de los datos de textura. El enfoque más popular para la reducción es usar un mapa MIP. Un mapa MIP es una textura multinivel. El tamaño de cada nivel es una potencia de dos menor que el nivel anterior, hasta una textura de 1x1. Cuando se usa la reducción, un juego elige el nivel de mapa MIP más cercano al tamaño que se necesita en el tiempo de representación. 

### <a name="basicloader"></a>BasicLoader

__BasicLoader__ es un cargador simple que proporciona compatibilidad para cargar sombreadores, texturas y mallas desde archivos en disco. Proporciona métodos sincrónicos y asincrónicos. En este juego de muestra, los archivos BasicLoader.h/.cpp están en la carpeta __Commons__.

Para obtener más información, consulta [Cargador básico](complete-code-for-basicloader.md).