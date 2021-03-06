---
title: Crear sombreadores y dibujar primitivos
description: Aquí te mostramos cómo usar archivos de origen HLSL para compilar y crear sombreadores que luego podrás usar para dibujar primitivos en la pantalla.
ms.assetid: 91113bbe-96c9-4ef9-6482-39f1ff1a70f4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, sombreadores, primitivos, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: e900767b594b195ac5e1dd1d5777cfab16c2ece4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175369"
---
# <a name="create-shaders-and-drawing-primitives"></a>Crear sombreadores y dibujar primitivos



Aquí te mostramos cómo usar archivos de origen HLSL para compilar y crear sombreadores que luego podrás usar para dibujar primitivos en la pantalla.

Creamos y dibujamos un triángulo amarillo con sombreadores de vértices y píxeles. Después de crear el dispositivo Direct3D, la cadena de intercambio y la vista del destino de representación, leemos datos de los archivos de objetos binarios del sombreador.

**Objetivo:** crear sombreadores y dibujar primitivos.

## <a name="prerequisites"></a>Requisitos previos


Suponemos que estás familiarizado con C++. También necesitas tener experiencia básica en los conceptos de programación de elementos gráficos.

Suponemos además que has consultado [Inicio rápido: configurar recursos de DirectX y mostrar una imagen](setting-up-directx-resources.md).

**Tiempo en completarse:** 20 minutos.

## <a name="instructions"></a>Instructions

### <a name="1-compiling-hlsl-source-files"></a>1. Compilar archivos de origen HLSL

Microsoft Visual Studio usa el compilador de código HLSL [fxc.exe](/windows/desktop/direct3dtools/fxc) para compilar los archivos de origen .hlsl (SimpleVertexShader.hlsl y SimplePixelShader.hlsl) en archivos de objetos de sombreador binarios .cso (SimpleVertexShader.cso y SimplePixelShader.cso). Para obtener más información acerca del compilador de código HLSL, consulta el tema sobre el compilador de efectos. Para obtener más información acerca de cómo compilar código de sombreadores, consulta el tema sobre la [compilación de sombreadores](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-part1).

Este es el código en SimpleVertexShader.hlsl:

```hlsl
struct VertexShaderInput
{
    DirectX::XMFLOAT2 pos : POSITION;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // For this lesson, set the vertex depth value to 0.5, so it is guaranteed to be drawn.
    vertexShaderOutput.pos = float4(input.pos, 0.5f, 1.0f);

    return vertexShaderOutput;
}
```

Este es el código en SimplePixelShader.hlsl:

```hlsl
struct PixelShaderInput
{
    float4 pos : SV_POSITION;
};

float4 SimplePixelShader(PixelShaderInput input) : SV_TARGET
{
    // Draw the entire triangle yellow.
    return float4(1.0f, 1.0f, 0.0f, 1.0f);
}
```

### <a name="2-reading-data-from-disk"></a>2. Leer datos del disco

Usamos la función DX::ReadDataAsync de DirectXHelper.h en la plantilla Aplicación DirectX 11 (Universal Windows) para leer datos desde un archivo en el disco de forma asincrónica.

### <a name="3-creating-vertex-and-pixel-shaders"></a>3. Crear sombreadores de vértices y píxeles

Leemos los datos del archivo SimpleVertexShader.cso y asignamos esos datos a la matriz de bytes *vertexShaderBytecode*. Llamamos a [**ID3D11Device::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) con la matriz de bytes para crear el sombreador de vértices ([**ID3D11VertexShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11vertexshader)). Establecemos el valor de profundidad del vértice en 0,5 en el origen SimpleVertexShader.hlsl para garantizar que el triángulo se dibuja. Rellenamos una matriz de estructuras [** \_ DESC del \_ elemento \_ de entrada D3D11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) para describir el diseño del código del sombreador de vértices y, después, llama a [**ID3D11Device:: CreateInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createinputlayout) para crear el diseño. La matriz tiene un elemento de diseño que define la posición del vértice. Leemos los datos del archivo SimplePixelShader.cso y asignamos esos datos a la matriz de bytes *pixelShaderBytecode*. Llamamos a [**ID3D11Device::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) con la matriz de bytes para crear el sombreador de píxeles ([**ID3D11PixelShader**](/windows/desktop/api/d3d11/nn-d3d11-id3d11pixelshader)). Establecemos el valor del píxel en (1,1,1,1) en el origen SimplePixelShader.hlsl para que el triángulo sea amarillo. Puedes cambiar el color modificando este valor.

Creamos un búfer de índices y un búffer de vértices que definen un triángulo simple. Para ello, primero se define el triángulo, a continuación, se describen los búferes de vértices y de índices [**( \_ \_ datos de los Subrecursos**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)de[** \_ búfer D3D11 \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) y D3D11) mediante la definición del triángulo y, por último, se llama a [**ID3D11Device:: CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) una vez para cada búfer.

```cpp
        auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
        auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");
        
        // Load the raw vertex shader bytecode from disk and create a vertex shader with it.
        auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode) {


          ComPtr<ID3D11VertexShader> vertexShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateVertexShader(
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  nullptr,
                  &vertexShader
                  )
              );

          // Create an input layout that matches the layout defined in the vertex shader code.
          // For this lesson, this is simply a DirectX::XMFLOAT2 vector defining the vertex position.
          const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
          {
              { "POSITION", 0, DXGI_FORMAT_R32G32_FLOAT, 0, 0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
          };

          ComPtr<ID3D11InputLayout> inputLayout;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateInputLayout(
                  basicVertexLayoutDesc,
                  ARRAYSIZE(basicVertexLayoutDesc),
                  vertexShaderBytecode->Data,
                  vertexShaderBytecode->Length,
                  &inputLayout
                  )
              );
        });
        
        // Load the raw pixel shader bytecode from disk and create a pixel shader with it.
        auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode) {
          ComPtr<ID3D11PixelShader> pixelShader;
          DX::ThrowIfFailed(
              m_d3dDevice->CreatePixelShader(
                  pixelShaderBytecode->Data,
                  pixelShaderBytecode->Length,
                  nullptr,
                  &pixelShader
                  )
              );
        });

        // Create vertex and index buffers that define a simple triangle.
        auto createTriangleTask = (createPSTask && createVSTask).then([this] () {

          DirectX::XMFLOAT2 triangleVertices[] =
          {
              float2(-0.5f, -0.5f),
              float2( 0.0f,  0.5f),
              float2( 0.5f, -0.5f),
          };

          unsigned short triangleIndices[] =
          {
              0, 1, 2,
          };

          D3D11_BUFFER_DESC vertexBufferDesc = {0};
          vertexBufferDesc.ByteWidth = sizeof(float2) * ARRAYSIZE(triangleVertices);
          vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
          vertexBufferDesc.CPUAccessFlags = 0;
          vertexBufferDesc.MiscFlags = 0;
          vertexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA vertexBufferData;
          vertexBufferData.pSysMem = triangleVertices;
          vertexBufferData.SysMemPitch = 0;
          vertexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> vertexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &vertexBufferDesc,
                  &vertexBufferData,
                  &vertexBuffer
                  )
              );

          D3D11_BUFFER_DESC indexBufferDesc;
          indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(triangleIndices);
          indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
          indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
          indexBufferDesc.CPUAccessFlags = 0;
          indexBufferDesc.MiscFlags = 0;
          indexBufferDesc.StructureByteStride = 0;

          D3D11_SUBRESOURCE_DATA indexBufferData;
          indexBufferData.pSysMem = triangleIndices;
          indexBufferData.SysMemPitch = 0;
          indexBufferData.SysMemSlicePitch = 0;

          ComPtr<ID3D11Buffer> indexBuffer;
          DX::ThrowIfFailed(
              m_d3dDevice->CreateBuffer(
                  &indexBufferDesc,
                  &indexBufferData,
                  &indexBuffer
                  )
              );
        });
```

Hemos usado los sombreadores de vértices y píxeles, el diseño del sombreador de vértices, el búfer de índices y el búfer de vértices para dibujar un triángulo amarillo.

### <a name="4-drawing-the-triangle-and-presenting-the-rendered-image"></a>4. Dibujar el triángulo y mostrar la imagen representada

Entramos en un bucle sin fin para representar y mostrar continuamente la escena. Llamamos a [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para especificar el destino de representación como el destino de salida. Llamamos a [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) con {0.071f, 0.04f, 0.561f, 1.0f} para borrar el destino de representación y que muestre un color azul sólido.

En el bucle sin fin, dibujamos un triángulo amarillo en la superficie azul.

**Para dibujar un triángulo amarillo**

1.  En primer lugar, llamamos a [**ID3D11DeviceContext::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) para describir cómo se emitirán los datos del búfer de vértices a la etapa del ensamblador.
2.  A continuación, llamamos a [**ID3D11DeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) y [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) para enlazar los búferes de vértices e índices a la etapa del ensamblador de entrada.
3.  A continuación, se llama a [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) con el valor TRIANGLESTRIP de la [** \_ \_ topología \_ primitiva D3D11**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) para especificar que la etapa del ensamblador de entrada interprete los datos de vértice como una franja de triángulo.
4.  A continuación, llamamos a [**ID3D11DeviceContext::VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) para inicializar la etapa del sombreador de vértices con el código del sombreador de vértices e [**ID3D11DeviceContext::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) para inicializar la etapa del sombreador de píxeles con el código del sombreador de píxeles.
5.  Por último, llamamos a [**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) para dibujar el triángulo y enviarlo a la canalización de representación.

Llamamos a [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) para mostrar la imagen representada en la ventana.

```cpp
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

            m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

            // Set the vertex and index buffers, and specify the way they define geometry.
            UINT stride = sizeof(float2);
            UINT offset = 0;
            m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset
                );

            m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0
                );

            m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

            // Set the vertex and pixel shader stage state.
            m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0
                );

            m_d3dDeviceContext->PSSetShader(
                pixelShader.Get(),
                nullptr,
                0
                );

            // Draw the cube.
            m_d3dDeviceContext->DrawIndexed(
                ARRAYSIZE(triangleIndices),
                0,
                0
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
```

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes


Hemos creado y dibujado un triángulo amarillo con sombreadores de vértices y píxeles.

A continuación, crearemos un cubo 3D en órbita y le aplicaremos efectos de iluminación.

[Usar profundidad y efectos en primitivos](using-depth-and-effects-on-primitives.md)

 

 