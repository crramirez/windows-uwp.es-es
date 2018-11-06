---
author: mtoepke
title: Representar el mapa de sombras en el búfer de profundidad
description: Representa desde el punto de vista de la luz para crear un mapa de profundidad de dos dimensiones representando el volumen de sombra.
ms.assetid: 7f3d0208-c379-8871-cc48-027047c6c2d0
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, rendering, representación, shadow map, mapa de sombras, depth buffer, búfer de profundidad, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: a73754fef6d87505751460ec134d853c6bca0530
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6047051"
---
# <a name="render-the-shadow-map-to-the-depth-buffer"></a>Representar el mapa de sombras en el búfer de profundidad




Representa desde el punto de vista de la luz para crear un mapa de profundidad de dos dimensiones representando el volumen de sombra. El mapa de profundidad enmascara el espacio que se va a representar en la sombra Parte 2 de [Tutorial: implementar volúmenes de sombra con búferes de profundidad en Direct3D 11](implementing-depth-buffers-for-shadow-mapping.md).

## <a name="clear-the-depth-buffer"></a>Borra el búfer de profundidad


Siempre borra el búfer de profundidad antes de representarlo.

```cpp
context->ClearRenderTargetView(m_deviceResources->GetBackBufferRenderTargetView(), DirectX::Colors::CornflowerBlue);
context->ClearDepthStencilView(m_shadowDepthView.Get(), D3D11_CLEAR_DEPTH | D3D11_CLEAR_STENCIL, 1.0f, 0);
```

## <a name="render-the-shadow-map-to-the-depth-buffer"></a>Representar el mapa de sombras en el búfer de profundidad


Para el pase (o transferencia) de representación de sombras, especifica un búfer de profundidad pero no especifiques un destino de representación.

Especifica la ventanilla de luz y un sombreador de vértices, y establece los búferes de constantes del espacio de luz. Usa la selección (culling) de la cara anterior para este pase a fin de optimizar los valores de profundidad en el búfer de sombras.

Observa que en la mayoría de dispositivos, puedes especificar nullptr para el sombreador de píxeles (o saltear completamente la especificación de un sombreador de píxeles). Sin embargo algunos controladores pueden generar una excepción cuando llamas a draw en el dispositivo de Direct3D con un sombreador de píxeles nulo establecido. Para evitar esta excepción, puedes establecer un sombreador de píxeles mínimo para el pase de representación de sombras. El resultado de este sombreador se descarta; puede llamar a [**discard**](https://msdn.microsoft.com/library/windows/desktop/bb943995) en cada píxel.

Representa los objetos que pueden arrojar sombras, pero no te preocupes en representar geometría que no pueda dar sombra (como el piso de una habitación u objetos quitados del pase de sombras por motivos de optimización).

```cpp
void ShadowSceneRenderer::RenderShadowMap()
{
    auto context = m_deviceResources->GetD3DDeviceContext();

    // Render all the objects in the scene that can cast shadows onto themselves or onto other objects.

    // Only bind the ID3D11DepthStencilView for output.
    context->OMSetRenderTargets(
        0,
        nullptr,
        m_shadowDepthView.Get()
        );

    // Note that starting with the second frame, the previous call will display
    // warnings in VS debug output about forcing an unbind of the pixel shader
    // resource. This warning can be safely ignored when using shadow buffers
    // as demonstrated in this sample.

    // Set rendering state.
    context->RSSetState(m_shadowRenderState.Get());
    context->RSSetViewports(1, &m_shadowViewport);

    // Each vertex is one instance of the VertexPositionTexNormColor struct.
    UINT stride = sizeof(VertexPositionTexNormColor);
    UINT offset = 0;
    context->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset
        );

    context->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT, // Each index is one 16-bit unsigned integer (short).
        0
        );

    context->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
    context->IASetInputLayout(m_inputLayout.Get());

    // Attach our vertex shader.
    context->VSSetShader(
        m_simpleVertexShader.Get(),
        nullptr,
        0
        );

    // Send the constant buffers to the Graphics device.
    context->VSSetConstantBuffers(
        0,
        1,
        m_lightViewProjectionBuffer.GetAddressOf()
        );

    context->VSSetConstantBuffers(
        1,
        1,
        m_rotatedModelBuffer.GetAddressOf()
        );

    // In some configurations, it's possible to avoid setting a pixel shader
    // (or set PS to nullptr). Not all drivers are tolerant of this, so to be
    // safe set a minimal shader here.
    //
    // Direct3D will discard output from this shader because the render target
    // view is unbound.
    context->PSSetShader(
        m_textureShader.Get(),
        nullptr,
        0
        );

    // Draw the objects.
    context->DrawIndexed(
        m_indexCountCube,
        0,
        0
        );
}
```

**Optimizar el frustum de vista:** asegúrate de que tu implementación calcule un frustum de vista ajustado para que obtengas la mayor precisión posible de tu búfer de profundidad. Consulta [Técnicas habituales para mejorar los mapas de profundidad de sombras](https://msdn.microsoft.com/library/windows/desktop/ee416324) para ver más sugerencias sobre la técnica de sombras.

## <a name="vertex-shader-for-shadow-pass"></a>Sombreador de vértices para el pase de sombras


Usa una versión simplificada de tu sombreador de vértices para representar solo la posición de vértice en el espacio de luz. No incluyas ninguna normal de iluminación ni transformaciones secundarias, etc.

```cpp
PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput output;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projected space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    output.pos = pos;

    return output;
}
```

En la siguiente parte de este tutorial, aprenderás a agregar sombras mediante la [representación con pruebas de profundidad](render-the-scene-with-depth-testing.md).

 

 




