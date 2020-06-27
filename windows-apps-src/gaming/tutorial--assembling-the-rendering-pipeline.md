---
title: Introducción a la representación
description: Obtenga información sobre cómo desarrollar la canalización de representación para mostrar los gráficos. Introducción a la representación.
ms.assetid: 1da3670b-2067-576f-da50-5eba2f88b3e6
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, juegos y representación
ms.localizationpriority: medium
ms.openlocfilehash: d1abb324c5e9e16babbbf8d3650adc39cb995137
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409644"
---
# <a name="rendering-framework-i-intro-to-rendering"></a>Marco de representación I: Introducción a la representación

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

Hasta ahora hemos explicado cómo estructurar un juego Plataforma universal de Windows (UWP) y cómo definir una máquina de Estados para controlar el flujo del juego. Ahora es el momento de aprender a desarrollar el marco de representación. Echemos un vistazo a cómo el juego de ejemplo representa la escena del juego con Direct3D 11.

Direct3D 11 contiene un conjunto de API que proporcionan acceso a las características avanzadas de hardware gráfico de alto rendimiento que se pueden usar para crear gráficos 3D para aplicaciones con un uso intensivo de gráficos, como juegos.

La representación de gráficos en pantalla significa básicamente representar una secuencia de fotogramas en pantalla. En cada fotograma, tiene que representar objetos visibles en la escena, basados en la vista.

Para representar un fotograma, tiene que pasar la información de la escena necesaria al hardware para que se pueda mostrar en la pantalla. Si desea que aparezcan elementos en la pantalla, debe iniciar la representación en cuanto se inicie la ejecución del juego.

## <a name="objectives"></a>Objetivos

Para configurar un marco de representación básico para mostrar la salida de gráficos para un juego DirectX de UWP. Puede desglosar esto de forma flexible en estos tres pasos.

1. Establezca una conexión a la interfaz de gráficos.
2. Cree los recursos necesarios para dibujar los gráficos.
3. Muestre los gráficos mediante la representación del marco.

En este tema se explica cómo se representan los gráficos, que abarcan los pasos 1 y 3.

[Rendering Framework II: la representación de juegos](tutorial-game-rendering.md) cubre el paso 2 &mdash; Cómo configurar el marco de representación y cómo se preparan los datos antes de que pueda producirse la representación.

## <a name="get-started"></a>Introducción

Se recomienda familiarizarse con los conceptos básicos de gráficos y representación. Si no está familiarizado con Direct3D y la representación, vea [términos y conceptos](#terms-and-concepts) para obtener una breve descripción de los términos de representación y gráficos que se usan en este tema.

Para este juego, la clase **GameRenderer** representa el representador para este juego de ejemplo. Es responsable de crear y mantener todos los objetos de Direct3D 11 y Direct2D que se usan para generar los objetos visuales de juego. También mantiene una referencia al objeto **Simple3DGame** que se usa para recuperar la lista de objetos que se van a representar, así como el estado del juego para la pantalla de visualización frontal (HUD).

En esta parte del tutorial, nos centraremos en la representación de objetos 3D en el juego.

## <a name="establish-a-connection-to-the-graphics-interface"></a>Establecer una conexión a la interfaz de gráficos

Para obtener información sobre el acceso al hardware para la representación, consulte el tema [definir el marco de trabajo de la aplicación para UWP del juego](tutorial--building-the-games-uwp-app-framework.md#the-appinitialize-method) .

### <a name="the-appinitialize-method"></a>El método App:: Initialize

La función **STD:: make_shared** , como se muestra a continuación, se usa para crear un **shared_ptr** a **DX::D eviceresources**, que también proporciona acceso al dispositivo.

En Direct3D 11, se usa un [dispositivo](#device) para asignar y destruir objetos, representar primitivas y comunicarse con la tarjeta gráfica a través del controlador de gráficos.

```cppwinrt
void Initialize(CoreApplicationView const& applicationView)
{
    ...

    // At this point we have access to the device. 
    // We can create the device-dependent resources.
    m_deviceResources = std::make_shared<DX::DeviceResources>();
}
```

## <a name="display-the-graphics-by-rendering-the-frame"></a>Mostrar los gráficos representando el marco

La escena del juego debe representarse cuando se inicia el juego. Las instrucciones para la representación comienzan en el método [**GameMain:: Run**](#gamemainrun-method) , como se muestra a continuación.

El flujo sencillo es esto.

1. **Update**
2. **Render**
3. **Presente**

### <a name="gamemainrun-method"></a>GameMain:: Run (método)

```cppwinrt
void GameMain::Run()
{
    while (!m_windowClosed)
    {
        if (m_visible) // if the window is visible
        {
            switch (m_updateState)
            {
            ...
            default:
                CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);
                Update();
                m_renderer->Render();
                m_deviceResources->Present();
                m_renderNeeded = false;
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread().Dispatcher().ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
    m_game->OnSuspending();  // Exiting due to window close, so save state.
}
```

### <a name="update"></a>Actualizar

Para obtener más información sobre cómo se actualizan los Estados de los juegos en el método [**GameMain:: Update**](tutorial-game-flow-management.md#the-gamemainupdate-method) , vea el tema [Administración de flujo de juegos](tutorial-game-flow-management.md) .

### <a name="render"></a>Representación

La representación se implementa llamando al método [**GameRenderer:: Render**](#gamerendererrender-method) desde **GameMain:: Run**.

Si la [representación de estéreo](#stereo-rendering) está habilitada, hay dos fases de representación &mdash; una para el ojo izquierdo y otra para la derecha. En cada paso de representación, enlazamos el destino de representación y la vista de la galería de símbolos de profundidad al dispositivo. También borramos la vista de la galería de símbolos de profundidad.

> [!NOTE]
> La representación de estéreo se puede lograr mediante otros métodos, como el estéreo de paso único que usa la creación de instancias de vértices o sombreadores de geometría. El método de dos fases de representación es una forma más lenta pero más cómoda de lograr una representación de estéreo.

Una vez que se ejecuta el juego y se cargan los recursos, se actualiza la [matriz de proyección](#projection-transform-matrix), una vez por cada paso de representación. Los objetos son ligeramente diferentes de cada vista. A continuación, se configura la [canalización de representación de gráficos](#rendering-pipeline). 

> [!NOTE]
> Vea [crear y cargar recursos gráficos de DirectX](tutorial-game-rendering.md#create-and-load-directx-graphic-resources) para obtener más información sobre cómo se cargan los recursos.

En este juego de ejemplo, el representador está diseñado para usar un diseño de vértices estándar en todos los objetos. Esto simplifica el diseño del sombreador y permite cambios sencillos entre los sombreadores, independientemente de la geometría de los objetos.

#### <a name="gamerendererrender-method"></a>GameRenderer:: Render (método)

Establecemos el contexto de Direct3D para usar un diseño de vértices de entrada. Los objetos de diseño de entrada describen cómo se transmiten los datos de búfer de vértices en la [canalización de representación](#rendering-pipeline). 

A continuación, se establece el contexto de Direct3D para usar los búferes de constantes definidos anteriormente, que se usan en la fase de canalización del [sombreador de vértices](#vertex-shaders-and-pixel-shaders) y la fase de canalización del [sombreador de píxeles](#vertex-shaders-and-pixel-shaders) . 

> [!NOTE]
> Consulte el [marco de representación II: representación de juegos](tutorial-game-rendering.md) para obtener más información sobre la definición de los búferes de constantes.

Dado que el mismo diseño de entrada y el conjunto de búferes de constantes se usan para todos los sombreadores que se encuentran en la canalización, se configuran una vez por fotograma.

```cppwinrt
void GameRenderer::Render()
{
    bool stereoEnabled{ m_deviceResources->GetStereoState() };

    auto d3dContext{ m_deviceResources->GetD3DDeviceContext() };
    auto d2dContext{ m_deviceResources->GetD2DDeviceContext() };

    int renderingPasses = 1;
    if (stereoEnabled)
    {
        renderingPasses = 2;
    }

    for (int i = 0; i < renderingPasses; i++)
    {
        // Iterate through the number of rendering passes to be completed.
        // 2 rendering passes if stereo is enabled.
        if (i > 0)
        {
            // Doing the Right Eye View.
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetViewRight() };

            // Resets render targets to the screen.
            // OMSetRenderTargets binds 2 things to the device.
            // 1. Binds one render target atomically to the device.
            // 2. Binds the depth-stencil view, as returned by the GetDepthStencilView method, to the device.
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets

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

            // Direct2D -- discussed later
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmapRight());
        }
        else
        {
            // Doing the Mono or Left Eye View.
            // As compared to the right eye:
            // m_deviceResources->GetBackBufferRenderTargetView instead of GetBackBufferRenderTargetViewRight
            ID3D11RenderTargetView* const targets[1] = { m_deviceResources->GetBackBufferRenderTargetView() };

            // Same as the Right Eye View.
            d3dContext->OMSetRenderTargets(1, targets, m_deviceResources->GetDepthStencilView());
            d3dContext->ClearDepthStencilView(m_deviceResources->GetDepthStencilView(), D3D11_CLEAR_DEPTH, 1.0f, 0);

            // d2d -- Discussed later under Adding UI
            d2dContext->SetTarget(m_deviceResources->GetD2DTargetBitmap());
        }

        const float clearColor[4] = { 0.5f, 0.5f, 0.8f, 1.0f };

        // Only need to clear the background when not rendering the full 3D scene since
        // the 3D world is a fully enclosed box and the dynamics prevents the camera from
        // moving outside this space.
        if (i > 0)
        {
            // Doing the Right Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetViewRight(), clearColor);
        }
        else
        {
            // Doing the Mono or Left Eye View.
            d3dContext->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), clearColor);
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
                            m_game->GameCamera().LeftEyeProjection() :
                            m_game->GameCamera().RightEyeProjection()
                            ),
                        XMMatrixTranspose(XMLoadFloat4x4(&orientation))
                        )
                    );

                d3dContext->UpdateSubresource(
                    m_constantBufferChangeOnResize.get(),
                    0,
                    nullptr,
                    &changesOnResize,
                    0,
                    0
                    );
            }

            // Update variables that change once per frame.
            ConstantBufferChangesEveryFrame constantBufferChangesEveryFrameValue;
            XMStoreFloat4x4(
                &constantBufferChangesEveryFrameValue.view,
                XMMatrixTranspose(m_game->GameCamera().View())
                );
            d3dContext->UpdateSubresource(
                m_constantBufferChangesEveryFrame.get(),
                0,
                nullptr,
                &constantBufferChangesEveryFrameValue,
                0,
                0
                );

            // Set up the graphics pipeline. This sample uses the same InputLayout and set of
            // constant buffers for all shaders, so they only need to be set once per frame.
            // For more info about the graphics or rendering pipeline, see
            // https://docs.microsoft.com/windows/win32/direct3d11/overviews-direct3d-11-graphics-pipeline

            // IASetInputLayout binds an input-layout object to the input-assembler (IA) stage. 
            // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
            // Set up the Direct3D context to use this vertex layout. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout
            d3dContext->IASetInputLayout(m_vertexLayout.get());

            // VSSetConstantBuffers sets the constant buffers used by the vertex shader pipeline stage.
            // Set up the Direct3D context to use these constant buffers. For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers

            ID3D11Buffer* constantBufferNeverChanges{ m_constantBufferNeverChanges.get() };
            d3dContext->VSSetConstantBuffers(0, 1, &constantBufferNeverChanges);
            ID3D11Buffer* constantBufferChangeOnResize{ m_constantBufferChangeOnResize.get() };
            d3dContext->VSSetConstantBuffers(1, 1, &constantBufferChangeOnResize);
            ID3D11Buffer* constantBufferChangesEveryFrame{ m_constantBufferChangesEveryFrame.get() };
            d3dContext->VSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            ID3D11Buffer* constantBufferChangesEveryPrim{ m_constantBufferChangesEveryPrim.get() };
            d3dContext->VSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);

            // Sets the constant buffers used by the pixel shader pipeline stage. 
            // For more info, see
            // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-pssetconstantbuffers

            d3dContext->PSSetConstantBuffers(2, 1, &constantBufferChangesEveryFrame);
            d3dContext->PSSetConstantBuffers(3, 1, &constantBufferChangesEveryPrim);
            ID3D11SamplerState* samplerLinear{ m_samplerLinear.get() };
            d3dContext->PSSetSamplers(0, 1, &samplerLinear);

            for (auto&& object : m_game->RenderObjects())
            {
                // The 3D object render method handles the rendering.
                // For more info, see Primitive rendering below.
                object->Render(d3dContext, m_constantBufferChangesEveryPrim.get());
            }
        }

        // Start of 2D rendering
        ...
    }
}
```

### <a name="primitive-rendering"></a>Representación primitiva

Al representar la escena, se recorren todos los objetos que se deben representar. Los pasos siguientes se repiten para cada objeto (primitivo).

- Actualice el búfer de constantes (**m_constantBufferChangesEveryPrim**) con la [matriz de transformación universal](#world-transform-matrix) del modelo y la información del material.
- El **m_constantBufferChangesEveryPrim** contiene parámetros para cada objeto. Incluye la matriz de transformación de objeto a mundo, así como propiedades de material como el exponente especular y de color para los cálculos de iluminación.
- Establezca el contexto de Direct3D para usar el diseño de vértices de entrada para que los datos del objeto de malla se transmitan a la fase del ensamblador de entrada (IA) de la [canalización de representación](#rendering-pipeline).
- Establezca el contexto de Direct3D para que use un [búfer de índice](#index-buffer) en la fase del IA. Proporcione la información primitiva: tipo, orden de los datos.
- Envíe una llamada a Draw para dibujar el primitivo indizado sin instancia. El método **GameObject:: Render** actualiza el [búfer de constantes](#constant-buffer-or-shader-constant-buffer) primitivas con los datos específicos de un primitivo determinado. Esto da como resultado una llamada a **DrawIndexed** en el contexto para dibujar la geometría de cada primitiva. En concreto, esta llamada a Draw pone en cola comandos y datos en la unidad de procesamiento de gráficos (GPU), como parametrizados por los datos de búfer de constantes. Cada llamada a Draw ejecuta el sombreador de vértices una vez por vértice y, después, el [sombreador de píxeles](#vertex-shaders-and-pixel-shaders) una vez por cada píxel de cada triángulo de la primitiva. Las texturas forman parte del estado que el sombreador de píxeles usa para llevar a cabo la representación.

Estos son los motivos para usar varios búferes de constantes.

- El juego usa varios búferes de constantes, pero solo necesita actualizar estos búferes una vez por cada primitivo. Como se mencionó anteriormente, los búferes de constantes son como las entradas a los sombreadores que se ejecutan para cada primitiva. Algunos datos son estáticos (**m_constantBufferNeverChanges**); algunos datos son constantes sobre el marco (**m_constantBufferChangesEveryFrame**), como la posición de la cámara; y algunos datos son específicos de la primitiva, como el color y las texturas (**m_constantBufferChangesEveryPrim**).
- El representador del juego separa estas entradas en distintos búferes de constantes para optimizar el ancho de banda de la memoria que usan la CPU y la GPU. Este enfoque también ayuda a minimizar la cantidad de datos de los que es necesario realizar un seguimiento de la GPU. La GPU tiene una gran cola de comandos y cada vez que el juego llama a **Draw**, ese comando se pone en cola junto con los datos asociados a él. Cuando el juego actualiza el búfer de constantes de primitivos y envía el siguiente comando **Draw**, el controlador de gráficos agrega este comando siguiente y los datos asociados a la cola. Si el juego dibuja 100 primitivos, podría haber potencialmente 100 copias de los datos del búfer de constantes en la cola. Para minimizar la cantidad de datos que el juego envía a la GPU, el juego usa un búfer de constante primitiva independiente que solo contiene las actualizaciones para cada primitiva.

#### <a name="gameobjectrender-method"></a>GameObject:: Render (método)

```cppwinrt
void GameObject::Render(
    _In_ ID3D11DeviceContext* context,
    _In_ ID3D11Buffer* primitiveConstantBuffer
    )
{
    if (!m_active || (m_mesh == nullptr) || (m_normalMaterial == nullptr))
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
```

#### <a name="meshobjectrender-method"></a>MeshObject:: Render (método)

```cppwinrt
void MeshObject::Render(_In_ ID3D11DeviceContext* context)
{
    // PNTVertex is a struct. stride provides us the size required for all the mesh data
    // struct PNTVertex
    //{
    //  DirectX::XMFLOAT3 position;
    //  DirectX::XMFLOAT3 normal;
    //  DirectX::XMFLOAT2 textureCoordinate;
    //};
    uint32_t stride{ sizeof(PNTVertex) };
    uint32_t offset{ 0 };

    // Similar to the main render loop.
    // Input-layout objects describe how vertex buffer data is streamed into the IA pipeline stage.
    ID3D11Buffer* vertexBuffer{ m_vertexBuffer.get() };
    context->IASetVertexBuffers(0, 1, &vertexBuffer, &stride, &offset);

    // IASetIndexBuffer binds an index buffer to the input-assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer.
    context->IASetIndexBuffer(m_indexBuffer.get(), DXGI_FORMAT_R16_UINT, 0);

    // Binds information about the primitive type, and data order that describes input data for the input assembler stage.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology.
    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

    // Draw indexed, non-instanced primitives. A draw API submits work to the rendering pipeline.
    // For more info, see
    // https://docs.microsoft.com/windows/win32/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed.
    context->DrawIndexed(m_indexCount, 0, 0);
}
```

### <a name="deviceresourcespresent-method"></a>DeviceResources::P método reenviado

Llamamos al método **DeviceResources::P reenviar** para mostrar el contenido que hemos colocado en los búferes.

Usamos el término cadena de intercambio para una colección de búferes que se usan para mostrar fotogramas al usuario. Cada vez que una aplicación presenta un nuevo fotograma para su presentación, el primer búfer de la cadena de intercambio ocupa el lugar del búfer mostrado. Este proceso se denomina intercambio o volteo. Para obtener más información, vea [cadenas de intercambio](../graphics-concepts/swap-chains.md).

- El método **present** de la interfaz **IDXGISwapChain1** indica a [DXGI](#dxgi) que se bloquee hasta que tenga lugar la sincronización vertical (VSYNC), de modo que la aplicación se suspenda hasta la siguiente VSYNC. Esto garantiza que no se pierda ningún ciclo que represente fotogramas que nunca se mostrará en la pantalla.
- El método **DiscardView** de la interfaz **ID3D11DeviceContext3** descarta el contenido del destino de [representación](#render-target). Se trata de una operación válida solo cuando el contenido existente se sobrescribirá por completo. Si se usan los rectángulos modificados o de desplazamiento, se debe quitar esta llamada.
* Con el mismo método **DiscardView** , descarte el contenido de la [Galería de símbolos de profundidad](#depth-stencil).
* El método **HandleDeviceLost** se usa para administrar el escenario del [dispositivo](#device) que se va a quitar. Si el dispositivo se quitó mediante una desconexión o una actualización de controlador, debe volver a crear todos los recursos de dispositivo. Para obtener más información, consulte [controlar escenarios quitados de dispositivos en Direct3D 11](handling-device-lost-scenarios.md).

> [!TIP]
> Para conseguir una velocidad de fotogramas suave, debe asegurarse de que la cantidad de trabajo para representar un fotograma quepa en el tiempo entre VSyncs.

```cppwinrt
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
    m_d3dContext->DiscardView(m_d3dRenderTargetView.get());

    // Discard the contents of the depth stencil.
    m_d3dContext->DiscardView(m_d3dDepthStencilView.get());

    // If the device was removed either by a disconnection or a driver upgrade, we 
    // must recreate all device resources.
    if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
    {
        HandleDeviceLost();
    }
    else
    {
        winrt::check_hresult(hr);
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

En este tema se explica cómo se representan los gráficos en la pantalla y se proporciona una breve descripción de algunos de los términos de representación utilizados (a continuación). Obtenga más información sobre la representación en el [marco de representación II:](tutorial-game-rendering.md) tema de representación de juegos y aprenda a preparar los datos necesarios antes de la representación.

## <a name="terms-and-concepts"></a>Términos y conceptos

### <a name="simple-game-scene"></a>Escena de juego simple

Una escena de juego simple se compone de varios objetos con varias fuentes de luz.

La forma de un objeto se define mediante un conjunto de coordenadas X, Y, Z en el espacio. La ubicación real de representación en el mundo de juego se puede determinar aplicando una matriz de transformación a las coordenadas X, Y, Z posicionales. También puede tener un conjunto de coordenadas de textura &mdash; que usted y V, &mdash; que especifican cómo se aplica un material al objeto. Esto define las propiedades de la superficie del objeto y le permite ver si un objeto tiene una superficie aproximada (por ejemplo, una bola de tenis) o una superficie brillante suave (como una bola de bolos).

El marco de representación usa la información de escena y objeto para volver a crear el fotograma de escena por fotograma, lo que hace que esté activo en el monitor de pantalla.

### <a name="rendering-pipeline"></a>Canalización de representación

La canalización de representación es el proceso por el que se traduce la información de escena 3D en una imagen que se muestra en la pantalla. En Direct3D 11, esta canalización es programable. Puede adaptar las fases para satisfacer sus necesidades de representación. Las fases que incluyen núcleos de sombreador comunes se pueden programar mediante el lenguaje de programación HLSL. También se conoce como la *canalización de representación de gráficos*o simplemente *canalización*.

Para ayudarle a crear esta canalización, debe estar familiarizado con estos detalles.

- [HLSL](#hlsl). Se recomienda el uso del modelo de sombreador de HLSL 5,1 y versiones posteriores para juegos DirectX de UWP.
- [Sombreadores](#shaders).
- [Sombreadores de vértices y sombreadores de píxeles](#vertex-shaders-and-pixel-shaders).
- [Fases del sombreador](#shader-stages).
- [Varios formatos de archivo de sombreador](#various-shader-file-formats).

Para obtener más información, consulte [Descripción de la](/windows/desktop/direct3dgetstarted/understand-the-directx-11-2-graphics-pipeline) canalización de representación de Direct3D 11 y la [canalización de gráficos](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="hlsl"></a>HLSL

HLSL es el lenguaje de sombreador de alto nivel para DirectX. Con HLSL, puede crear sombreadores programables similares a C para la canalización de Direct3D. Para obtener más información, vea [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl).

#### <a name="shaders"></a>Sombreadores

Un sombreador puede considerarse como un conjunto de instrucciones que determinan cómo aparece la superficie de un objeto cuando se representa. Los que se programan con HLSL se conocen como sombreadores HLSL. Los archivos de código fuente para los sombreadores [HLSL]) (#hlsl) tienen la `.hlsl` extensión de archivo. Estos sombreadores pueden compilarse en tiempo de compilación o en tiempo de ejecución y establecerse en el tiempo de ejecución en la fase de canalización adecuada. Un objeto de sombreador compilado tiene una `.cso` extensión de archivo.

Los sombreadores de Direct3D 9 se pueden diseñar con el modelo de sombreador 1, el modelo de sombreador 2 y el modelo de sombreador 3; Los sombreadores de Direct3D 10 solo se pueden diseñar en el modelo de sombreador 4. Los sombreadores de Direct3D 11 se pueden diseñar en el modelo de sombreador 5. Direct3D 11,3 y Direct3D 12 se pueden diseñar en el modelo de sombreador 5,1 y Direct3D 12 también se puede diseñar en el modelo de sombreador 6.

#### <a name="vertex-shaders-and-pixel-shaders"></a>Sombreadores de vértices y sombreadores de píxeles

Los datos entran en la canalización de gráficos como un flujo de primitivas y se procesan mediante diversos sombreadores, como los sombreadores de vértices y los sombreadores de píxeles. 

Los sombreadores de vértices procesan los vértices, normalmente realizando operaciones como transformaciones, máscaras e iluminación. Los sombreadores de píxeles permiten técnicas de sombreado enriquecidas, como la iluminación por píxel y el procesamiento posterior. Combina variables constantes, datos de textura, valores interpolados por vértice y otros datos para generar salidas por píxel. 

#### <a name="shader-stages"></a>Fases del sombreador

Una secuencia de estos diversos sombreadores definidos para procesar esta secuencia de primitivas se conoce como fases del sombreador en una canalización de representación. Las fases reales dependen de la versión de Direct3D, pero normalmente incluyen las fases de vértice, geometría y píxel. También hay otras fases, como el casco y los sombreadores de dominio para la teselación, y el sombreador de cálculo. Todas estas fases son completamente programables mediante [HLSL](#hlsl). Para obtener más información, vea [canalización de gráficos](/windows/desktop/direct3d11/overviews-direct3d-11-graphics-pipeline).

#### <a name="various-shader-file-formats"></a>Varios formatos de archivo de sombreador

Estas son las extensiones de archivo de código de sombreador.

- Un archivo con la `.hlsl` extensión contiene código fuente [HLSL]) (#hlsl).
- Un archivo con la `.cso` extensión contiene un objeto de sombreador compilado.
- Un archivo con la `.h` extensión es un archivo de encabezado, pero en un contexto de código del sombreador, este archivo de encabezado define una matriz de bytes que contiene los datos del sombreador.
- Un archivo con la `.hlsli` extensión contiene el formato de los búferes de constantes. En el juego de ejemplo, el archivo es **shaders**  >  **ConstantBuffers. hlsli**.

> [!NOTE]
> Puede incrustar un sombreador cargando un `.cso` archivo en tiempo de ejecución o agregando un `.h` archivo en el código ejecutable. Pero no usaría ambos para el mismo sombreador.

### <a name="deeper-understanding-of-directx"></a>Conocimientos más profundos de DirectX

Direct3D 11 es un conjunto de API que pueden ayudarnos a crear gráficos para aplicaciones con un uso intensivo de gráficos como juegos, donde queremos tener una buena tarjeta gráfica para procesar el cálculo intensivo. En esta sección se explican brevemente los conceptos de programación de gráficos de Direct3D 11: recursos, Subrecursos, dispositivos y contexto de dispositivo.

#### <a name="resource"></a>Recurso

Los recursos (también conocidos como recursos de dispositivo) se pueden considerar como información sobre cómo representar un objeto, como la textura, la posición o el color. Los recursos proporcionan datos a la canalización y definen lo que se representa durante la escena. Los recursos se pueden cargar desde el medio de juego o se pueden crear dinámicamente en tiempo de ejecución.

Un recurso es, de hecho, un área de memoria a la que se puede tener acceso mediante la [canalización](#rendering-pipeline)de Direct3D. Para que la canalización pueda acceder a la memoria de forma eficiente, los datos que se proporcionan a la canalización (por ejemplo, geometría de entrada, recursos del sombreador y texturas) deben almacenarse en un recurso. Hay dos tipos de recursos de los que derivan todos los recursos de Direct3D: un búfer o una textura. Se pueden activar hasta 128 recursos para cada fase de la canalización. Para obtener más información, consulte [Recursos](../graphics-concepts/resources.md).

#### <a name="subresource"></a>Subrecurso

El término subresource hace referencia a un subconjunto de un recurso. Direct3D puede hacer referencia a un recurso completo, o puede hacer referencia a subconjuntos de un recurso. Para obtener más información, vea [subresource](../graphics-concepts/resource-types.md#subresources).

#### <a name="depth-stencil"></a>Panel de estarcido de profundidad

Un recurso de estarcido de profundidad contiene el formato y el búfer para contener información de profundidad y de estarcido. Se crea mediante un recurso de textura. Para obtener más información sobre cómo crear un recurso de estarcido de profundidad, consulte [configuración de la funcionalidad de la galería de símbolos](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-depth-stencil)de profundidad. Se tiene acceso al recurso de estarcido de profundidad a través de la vista de galería de símbolos de profundidad implementada mediante la interfaz [ID3D11DepthStencilView](/windows/desktop/api/d3d11/nn-d3d11-id3d11depthstencilview) .

La información de profundidad nos indica qué áreas de polígonos están detrás de otras, para que podamos determinar cuáles están ocultas. La información de la galería de símbolos nos indica qué píxeles se enmascaran. Se puede usar para producir efectos especiales, ya que determina si se dibuja un píxel o no; establece el bit en 1 o 0. 

Para obtener más información, vea [la vista de galería de símbolos de profundidad, el](../graphics-concepts/depth-stencil-view--dsv-.md) [búfer de profundidad](../graphics-concepts/depth-buffers.md)y el [búfer de estarcido](../graphics-concepts/stencil-buffers.md).

#### <a name="render-target"></a>Destino de representación

Un destino de representación es un recurso en el que se puede escribir al final de una fase de representación. Normalmente se crea mediante el método [ID3D11Device:: CreateRenderTargetView](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview) mediante el búfer de retroceso de cadena de intercambio (que también es un recurso) como parámetro de entrada. 

Cada destino de representación también debe tener una vista de la galería de símbolos de profundidad correspondiente porque cuando usamos [OMSetRenderTargets](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para establecer el destino de representación antes de usarlo, también se requiere una vista de galería de símbolos de profundidad. Se tiene acceso al recurso de destino de representación a través de la vista de destino de representación implementada mediante la interfaz [ID3D11RenderTargetView](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) . 

#### <a name="device"></a>Dispositivo

Puede imaginar un dispositivo como una manera de asignar y destruir objetos, representar primitivas y comunicarse con la tarjeta gráfica a través del controlador de gráficos. 

Para obtener una explicación más precisa, un dispositivo Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, realiza transformaciones y operaciones de iluminación y rasteriza una imagen en una superficie. Para obtener más información, consulte [dispositivos](../graphics-concepts/devices.md) .

Un dispositivo se representa mediante la interfaz [**ID3D11Device**](/windows/desktop/api/d3d11/nn-d3d11-id3d11device) . En otras palabras, la interfaz **ID3D11Device** representa un adaptador de pantalla virtual y se usa para crear recursos que son propiedad de un dispositivo. 

Hay diferentes versiones de ID3D11Device. [**ID3D11Device5**](/windows/desktop/api/d3d11_4/nn-d3d11_4-id3d11device5) es la versión más reciente y agrega nuevos métodos a los de **ID3D11Device4**. Para obtener más información sobre cómo Direct3D se comunica con el hardware subyacente, consulte [arquitectura de Windows Device Driver Model (WDDM)](/windows-hardware/drivers/display/windows-vista-and-later-display-driver-model-architecture).

Cada aplicación debe tener al menos un dispositivo. la mayoría de las aplicaciones solo crean una. Cree un dispositivo para uno de los controladores de hardware instalados en el equipo llamando a **D3D11CreateDevice** o **D3D11CreateDeviceAndSwapChain** y especificando el tipo de controlador con la marca **D3D_DRIVER_TYPE** . Cada dispositivo puede usar uno o más contextos de dispositivo, en función de la funcionalidad deseada. Para obtener más información, vea [función D3D11CreateDevice](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice).

#### <a name="device-context"></a>Contexto del dispositivo

Un contexto de dispositivo se usa para establecer el estado de la [canalización](#rendering-pipeline) y generar comandos de representación mediante los [recursos](#resource) que pertenecen a un [dispositivo](#device). 

Direct3D 11 implementa dos tipos de contextos de dispositivo, uno para la representación inmediata y otro para la representación diferida; ambos contextos se representan con una interfaz [ID3D11DeviceContext](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) . 

Las interfaces **ID3D11DeviceContext** tienen versiones diferentes; **ID3D11DeviceContext4** agrega nuevos métodos a los de **ID3D11DeviceContext3**.

**ID3D11DeviceContext4** se introduce en Windows 10 Creators Update y es la versión más reciente de la interfaz **ID3D11DeviceContext** . Las aplicaciones que tienen como destino Windows 10 Creators Update y versiones posteriores deben usar esta interfaz en lugar de las versiones anteriores. Para obtener más información, vea [ID3D11DeviceContext4](/windows/desktop/api/d3d11_3/nn-d3d11_3-id3d11devicecontext4).

#### <a name="dxdeviceresources"></a>DX::D eviceResources

La clase **DX::D eviceresources** se encuentra en los archivos **DeviceResources. cpp** / **. h** y controla todos los recursos de dispositivo de DirectX.

### <a name="buffer"></a>Buffer

Un recurso de búfer es una colección de datos totalmente tipados agrupados en elementos. Puede usar los búferes para almacenar una gran variedad de datos, incluidos los vectores de posición, los vectores normales, las coordenadas de textura en un búfer de vértices, los índices en un búfer de índice o el estado del dispositivo. Los elementos de búfer pueden incluir valores de datos empaquetados (como valores de la superficie **R8G8B8A8** ), enteros de 8 bits únicos o valores de punto flotante de 4 32 bits.

Hay tres tipos de búferes disponibles: búfer de vértices, búfer de índice y búfer de constantes.

#### <a name="vertex-buffer"></a>Búfer de vértices

Contiene los datos de vértice utilizados para definir la geometría. Los datos de vértice incluyen coordenadas de posición, datos de color, datos de coordenadas de textura, datos normales, etc. 

#### <a name="index-buffer"></a>Búfer de índice

Contiene desplazamientos enteros en búferes de vértices y se usan para representar primitivas de forma más eficaz. Un búfer de índice contiene un conjunto secuencial de índices de 16 o 32 bits; cada índice se usa para identificar un vértice en un búfer de vértice.

#### <a name="constant-buffer-or-shader-constant-buffer"></a>Búfer de constantes o búfer de constantes de sombreador

Permite proporcionar de forma eficaz datos de sombreador a la canalización. Puede usar búferes de constantes como entradas para los sombreadores que se ejecutan para cada primitiva y almacenar los resultados de la fase de salida de flujo de la canalización de representación. Conceptualmente, un búfer de constantes tiene un aspecto similar al de un búfer de vértice de un solo elemento.

#### <a name="design-and-implementation-of-buffers"></a>Diseño e implementación de búferes

Puede diseñar búferes basándose en el tipo de datos, por ejemplo, como en el juego de ejemplo, se crea un búfer para los datos estáticos, otro para los datos constantes sobre el marco y otro para los datos específicos de un primitivo.

Todos los tipos de búfer están encapsulados por la interfaz **ID3D11Buffer** y se puede crear un recurso de búfer llamando a **ID3D11Device:: CreateBuffer**. Pero un búfer debe estar enlazado a la canalización antes de que se pueda tener acceso a él. Los búferes se pueden enlazar a varias fases de canalización simultáneamente para su lectura. También se puede enlazar un búfer a una sola fase de canalización para escribir en él. sin embargo, no se puede enlazar el mismo búfer para lectura y escritura simultáneamente.

Puede enlazar búferes de estas maneras.

- En la fase de ensamblador de entrada mediante una llamada a métodos de **ID3D11DeviceContext** como **ID3D11DeviceContext:: IASetVertexBuffers** y **ID3D11DeviceContext:: IASetIndexBuffer**.
- En la fase de salida de flujo llamando a **ID3D11DeviceContext:: SOSetTargets**.
- En la fase del sombreador mediante una llamada a métodos de sombreador, como **ID3D11DeviceContext:: VSSetConstantBuffers**.

Para obtener más información, vea [Introducción a los búferes en Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro).

### <a name="dxgi"></a>DXGI

Microsoft DirectX Graphics Infrastructure (DXGI) es un subsistema que encapsula algunas de las tareas de bajo nivel que requiere Direct3D. Se debe tener especial cuidado al usar DXGI en una aplicación multiproceso para garantizar que no se produzcan interbloqueos. Para obtener más información, vea [multithreading y DXGI](/windows/win32/direct3darticles/dxgi-best-practices#multithreading-and-dxgi) .

### <a name="feature-level"></a>Nivel de características

El nivel de características es un concepto incluido en Direct3D 11 para controlar la diversidad de tarjetas de vídeo en máquinas nuevas y existentes. Un nivel de característica es un conjunto bien definido de funciones de la unidad de procesamiento de gráficos (GPU). 

Cada tarjeta de vídeo implementa un cierto nivel de funcionalidad de DirectX según las GPU instaladas. En versiones anteriores de Microsoft Direct3D, podía averiguar la versión de Direct3D que la tarjeta de vídeo implementó y, después, programar la aplicación en consecuencia. 

Con el nivel de característica, cuando se crea un dispositivo, se puede intentar crear un dispositivo para el nivel de característica que se desea solicitar. Si la creación del dispositivo funciona, ese nivel de característica existe, de lo contrario, el hardware no admite ese nivel de características. Puede intentar volver a crear un dispositivo en un nivel de características inferior o puede salir de la aplicación. Por ejemplo, el nivel de característica 12_0 requiere Direct3D 11,3 o Direct3D 12 y el modelo de sombreador 5,1. Para obtener más información, vea [niveles de características de Direct3D: información general para cada nivel de característica](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

Con los niveles de características, puede desarrollar una aplicación para Direct3D 9, Microsoft Direct3D 10 o Direct3D 11 y, a continuación, ejecutarla en el hardware de 9, 10 u 11 (con algunas excepciones). Para obtener más información, vea [niveles de características de Direct3D](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro).

### <a name="stereo-rendering"></a>Representación de estéreo

La representación de estéreo se usa para mejorar la sensación de profundidad. Usa dos imágenes, una desde el ojo izquierdo y la otra desde el ojo derecho para mostrar una escena en la pantalla de presentación. 

Matemáticamente, se aplica una matriz de proyección estéreo, que es un ligero desplazamiento horizontal a la derecha y a la izquierda, de la matriz de proyección mono normal para lograrlo.

Hicimos dos pasos de representación para lograr la representación de estéreo en este juego de ejemplo.

- Enlazar al destino de representación de la derecha, aplicar la proyección derecha y, a continuación, dibujar el objeto primitivo.
- Enlazar al destino de representación izquierdo, aplicar la proyección izquierda y, a continuación, dibujar el objeto primitivo.

### <a name="camera-and-coordinate-space"></a>Cámara y espacio de coordenadas

El juego cuenta con el código para actualizar el mundo en su propio sistema de coordenadas (en ocasiones llamado espacio del mundo o espacio de la escena). Todos los objetos, incluida la cámara, se posicionan y se orientan en este espacio. Para obtener más información, vea [sistemas de coordenadas](../graphics-concepts/coordinate-systems.md).

Un sombreador de vértices realiza el pesado trabajo de conversión de las coordenadas del modelo a las coordenadas del dispositivo con el siguiente algoritmo (donde V es un vector y M es una matriz).

`V(device) = V(model) x M(model-to-world) x M(world-to-view) x M(view-to-device)`

- `M(model-to-world)`es una matriz de transformación para las coordenadas del modelo a coordenadas universales, también conocido como [matriz de transformación universal](#world-transform-matrix). Lo proporciona el primitivo.
- `M(world-to-view)`es una matriz de transformación para coordenadas universales de las coordenadas de vista, también conocida como [matriz de transformación de vista](#view-transform-matrix).
  - Lo proporciona la matriz de vista de la cámara. Se define mediante la posición de la cámara junto con los vectores de la apariencia *(el vector de búsqueda que* apunta directamente a la escena desde la cámara y el vector de *búsqueda* que se encuentra en su posición perpendicular).
  - En el juego de ejemplo, **m_viewMatrix** es la matriz de transformación de la vista y se calcula mediante **Camera:: SetViewParams**.
- `M(view-to-device)`es una matriz de transformación para las coordenadas de la vista a las coordenadas del dispositivo, también conocida como [matriz de transformación de proyección](#projection-transform-matrix).
  - Lo proporciona la proyección de la cámara. Proporciona información sobre la cantidad de ese espacio realmente visible en la escena final. El campo de vista (campo de visualización), la relación de aspecto y los planos de recorte definen la matriz de transformación de proyección.
  - En el juego de ejemplo, **m_projectionMatrix** define la transformación en las coordenadas de proyección, calculada mediante **Camera:: SetProjParams** (para la proyección estéreo, se usan dos matrices &mdash; de proyección una para cada vista de ojo).

El código del sombreador de `VertexShader.hlsl` se carga con estos vectores y matrices de los búferes de constantes y realiza esta transformación para cada vértice.

### <a name="coordinate-transformation"></a>Transformación de coordenadas

Direct3D usa tres transformaciones para cambiar las coordenadas del modelo 3D por coordenadas de píxeles (espacio de pantalla). Estas transformaciones son transformación universal, transformación de vista y transformación de proyección. Para obtener más información, vea [información general sobre](../graphics-concepts/transform-overview.md)transformaciones.

#### <a name="world-transform-matrix"></a>Matriz de transformación universal

Una transformación de mundo cambia las coordenadas del espacio del modelo, donde los vértices se definen en relación con el origen local de un modelo, en el espacio universal, donde se definen los vértices en relación con un origen común a todos los objetos de una escena. En esencia, la transformación mundial coloca un modelo en el mundo; por lo tanto, su nombre. Para obtener más información, vea [universal Transform](../graphics-concepts/world-transform.md).

#### <a name="view-transform-matrix"></a>Matriz de transformación de vista

La transformación de vista busca el visor en el espacio universal, transformando los vértices en el espacio de la cámara. En el espacio de la cámara, la cámara o el visor, está en el origen y busca en la dirección z positiva. Para obtener más información, vaya a [vista transformación](../graphics-concepts/view-transform.md).

####  <a name="projection-transform-matrix"></a>Matriz de transformación de proyección

La transformación de proyección convierte el frustum de visualización en una forma Cuboid. Un frustum de visualización es un volumen 3D en una escena colocada en relación con la cámara de la ventanilla. Una ventanilla es un rectángulo 2D en el que se proyecta una escena 3D. Para obtener más información, vea las [ventanillas y el recorte](../graphics-concepts/viewports-and-clipping.md)

Dado que el extremo cercano del frustum de visualización es más pequeño que el extremo, tiene el efecto de expandir los objetos que están cerca de la cámara. así es cómo se aplica la perspectiva a la escena. Por tanto, los objetos que están más cerca del jugador aparecen más grandes; los objetos que se encuentran más lejos aparecen más pequeños.

Matemáticamente, la transformación de proyección es una matriz que normalmente es una escala y una proyección en perspectiva. Funciona como la lente de una cámara. Para obtener más información, vea [transformación de proyección](../graphics-concepts/projection-transform.md).

### <a name="sampler-state"></a>Estado de muestra

Estado de muestra determina cómo se muestrean los datos de textura mediante modos de direccionamiento de texturas, filtrado y nivel de detalle. El muestreo se realiza cada vez que se lee un píxel de textura (o textura) de una textura.

Una textura contiene una matriz de textura. La posición de cada textura se denota mediante `(u,v)` , donde `u` es el ancho y `v` es el alto, y se asigna entre 0 y 1 según el ancho y el alto de la textura. Las coordenadas de textura resultantes se usan para direccionar un textura al hacer un muestreo de una textura.

Cuando las coordenadas de textura están por debajo de 0 o superior a 1, el modo de dirección de textura define cómo la coordenada de textura direcciona una ubicación textura. Por ejemplo, al usar **TextureAddressMode. Clamp**, cualquier coordenada fuera del intervalo 0-1 se fija en un valor máximo de 1 y el valor mínimo de 0 antes del muestreo.

Si la textura es demasiado grande o demasiado pequeña para el polígono, la textura se filtra para ajustarse al espacio. Un filtro de ampliación amplía una textura, un filtro de minificación reduce la textura para que quepa en un área más pequeña. La ampliación de textura repite el textura de ejemplo para una o varias direcciones, lo que produce una imagen de blurrier. La minificación de textura es más complicada porque requiere combinar más de un textura valores en un único valor. Esto puede producir un alias o bordes escalonados en función de los datos de textura. El enfoque más popular para minificación es usar un mipmap. Un mipmap es una textura de varios niveles. El tamaño de cada nivel es una potencia de 2 más pequeña que el nivel anterior a una textura 1x1. Cuando se usa minificación, un juego elige el nivel de mipmap más cercano al tamaño necesario en el momento de la representación. 

### <a name="the-basicloader-class"></a>La clase BasicLoader

**BasicLoader** es una clase de cargador simple que proporciona compatibilidad para cargar sombreadores, texturas y mallas de archivos en disco. Proporciona métodos sincrónicos y asincrónicos. En este juego de ejemplo, los `BasicLoader.h/.cpp` archivos se encuentran en la carpeta **Utilities** .

Para obtener más información, vea [cargador básico](complete-code-for-basicloader.md).