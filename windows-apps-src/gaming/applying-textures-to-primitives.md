---
title: Aplicar texturas en primitivos
description: Aquí te mostramos cómo cargar datos de texturas sin procesar y cómo aplicar esos datos a un primitivo 3D, mediante el cubo que creamos en Usar profundidad y efectos en primitivos.
ms.assetid: aeed09e3-c47a-4dd9-d0e8-d1b8bdd7e9b4
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, texturas, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: 349dcc4e8cc65a6265579e84c6810dcac79938a1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156489"
---
# <a name="apply-textures-to-primitives"></a>Aplicar texturas en primitivos

Aquí te mostramos cómo cargar datos de texturas sin procesar y cómo aplicar esos datos a un primitivo 3D, mediante el cubo que creamos en [Usar profundidad y efectos en primitivos](using-depth-and-effects-on-primitives.md). También te mostramos cómo introducir un modelo de iluminación simple con producto escalar, donde las superficies del cubo son más claras o más oscuras a una distancia y ángulos relativos a una fuente de luz.

**Objetivo:** aplicar texturas en primitivos.

## <a name="prerequisites"></a>Requisitos previos

Para sacar el máximo partido de este tema, deberá estar familiarizado con C++. También necesitará experiencia básica con los conceptos de programación de gráficos. Y, idealmente, tendrá que seguir con [los pasos de inicio rápido: configurar recursos de DirectX y mostrar una imagen](setting-up-directx-resources.md), [crear sombreadores y dibujar primitivos](creating-shaders-and-drawing-primitives.md), y usar la [profundidad y los efectos en los primitivos](using-depth-and-effects-on-primitives.md).

**Tiempo en completarse:** 20 minutos.

<a name="instructions"></a>Instructions
------------
### <a name="1-defining-variables-for-a-textured-cube"></a>1. Definir variables para un cubo con textura

Primero necesitamos definir las estructuras **BasicVertex** y **ConstantBuffer** para el cubo con textura. Estas estructuras especifican las posiciones de los vértices, las orientaciones y texturas para el cubo y de qué manera se visualizará el cubo. De lo contrario, se declaran variables de forma similar al tutorial anterior, [con la profundidad y los efectos en los primitivos](using-depth-and-effects-on-primitives.md).

```cppcx
struct BasicVertex
{
    DirectX::XMFLOAT3 pos;  // Position
    DirectX::XMFLOAT3 norm; // Surface normal vector
    DirectX::XMFLOAT2 tex;  // Texture coordinate
};

struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};

// This class defines the application as a whole.
ref class Direct3DTutorialFrameworkView : public IFrameworkView
{
private:
    Platform::Agile<CoreWindow> m_window;
    ComPtr<IDXGISwapChain1> m_swapChain;
    ComPtr<ID3D11Device1> m_d3dDevice;
    ComPtr<ID3D11DeviceContext1> m_d3dDeviceContext;
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    ComPtr<ID3D11DepthStencilView> m_depthStencilView;
    ComPtr<ID3D11Buffer> m_constantBuffer;
    ConstantBuffer m_constantBufferData;
```

### <a name="2-creating-vertex-and-pixel-shaders-with-surface-and-texture-elements"></a>2. Crear sombreadores de vértices y píxeles con elementos de textura y superficie

Aquí creamos sombreadores de vértices y píxeles más complejos con respecto al tutorial anterior, [Usar profundidad y efectos en primitivos](using-depth-and-effects-on-primitives.md). El sombreador de vértices de la aplicación transforma cada posición de vértice en un espacio de proyección y pasa la coordenada de textura del vértice por el sombreador de píxeles.

La matriz de la aplicación de las estructuras [** \_ DESC del \_ elemento \_ de entrada D3D11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_input_element_desc) que describen el diseño del código del sombreador de vértices tiene tres elementos de diseño: un elemento define la posición del vértice, otro elemento define el vector normal de la superficie (la dirección a la que se enfrenta normalmente la superficie) y el tercer elemento define las coordenadas de textura.

Creamos búferes de vértices, índices y constantes que definen un cubo con textura en órbita.

**Para definir un cubo con textura en órbita**

1.  Primero definimos el cubo. A cada vértice se le asigna una posición, un vector normal de superficie y coordenadas de textura. Usamos varios vértices para cada esquina con el fin de permitir que se definan distintos vectores y coordenadas de textura.
2.  A continuación, se describen los búferes de vértices y de índices ( [** \_ \_ datos de los Subrecursos**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data)de[** \_ búfer D3D11 \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc) y D3D11) con la definición de cubo. Llamamos a [**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) una vez para cada búfer.
3.  A continuación, creamos un búfer de constantes ([** \_ \_ DESC de búfer de D3D11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_buffer_desc)) para pasar matrices de modelo, vista y proyección al sombreador de vértices. Luego podemos usar el búfer de constantes para girar el cubo y aplicarle una proyección de perspectiva. Llamamos a [**ID3D11Device::CreateBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) para crear el búfer de constantes.
4.  Por último, especificamos la transformación de vista que corresponde a una posición de la cámara de X = 0, Y = 1, Z = 2.

```cppcx
auto loadVSTask = DX::ReadDataAsync(L"SimpleVertexShader.cso");
auto loadPSTask = DX::ReadDataAsync(L"SimplePixelShader.cso");

auto createVSTask = loadVSTask.then([this](const std::vector<byte>& vertexShaderBytecode)
{
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
    // These correspond to the elements of the BasicVertex struct defined above.
    const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
    {
        { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
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
auto createPSTask = loadPSTask.then([this](const std::vector<byte>& pixelShaderBytecode)
{
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

// Create vertex and index buffers that define a simple unit cube.
auto createCubeTask = (createPSTask && createVSTask).then([this]()
{
    // In the array below, which will be used to initialize the cube vertex buffers,
    // multiple vertices are used for each corner to allow different normal vectors and
    // texture coordinates to be defined for each face.
    BasicVertex cubeVertices[] =
    {
        { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Y (top face)
        { DirectX::XMFLOAT3(0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Y (bottom face)
        { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, -1.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +X (right face)
        { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -X (left face)
        { DirectX::XMFLOAT3(-0.5f,  0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(-1.0f, 0.0f, 0.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(-0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // +Z (front face)
        { DirectX::XMFLOAT3(0.5f,  0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, 0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },

        { DirectX::XMFLOAT3(0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 0.0f) }, // -Z (back face)
        { DirectX::XMFLOAT3(-0.5f,  0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 0.0f) },
        { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(1.0f, 1.0f) },
        { DirectX::XMFLOAT3(0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, -1.0f), DirectX::XMFLOAT2(0.0f, 1.0f) },
    };

    unsigned short cubeIndices[] =
    {
        0, 1, 2,
        0, 2, 3,

        4, 5, 6,
        4, 6, 7,

        8, 9, 10,
        8, 10, 11,

        12, 13, 14,
        12, 14, 15,

        16, 17, 18,
        16, 18, 19,

        20, 21, 22,
        20, 22, 23
    };

    D3D11_BUFFER_DESC vertexBufferDesc = { 0 };
    vertexBufferDesc.ByteWidth = sizeof(BasicVertex) * ARRAYSIZE(cubeVertices);
    vertexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    vertexBufferDesc.BindFlags = D3D11_BIND_VERTEX_BUFFER;
    vertexBufferDesc.CPUAccessFlags = 0;
    vertexBufferDesc.MiscFlags = 0;
    vertexBufferDesc.StructureByteStride = 0;

    D3D11_SUBRESOURCE_DATA vertexBufferData;
    vertexBufferData.pSysMem = cubeVertices;
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
    indexBufferDesc.ByteWidth = sizeof(unsigned short) * ARRAYSIZE(cubeIndices);
    indexBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    indexBufferDesc.BindFlags = D3D11_BIND_INDEX_BUFFER;
    indexBufferDesc.CPUAccessFlags = 0;
    indexBufferDesc.MiscFlags = 0;
    indexBufferDesc.StructureByteStride = 0;

    D3D11_SUBRESOURCE_DATA indexBufferData;
    indexBufferData.pSysMem = cubeIndices;
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

    // Create a constant buffer for passing model, view, and projection matrices
    // to the vertex shader.  This will allow us to rotate the cube and apply
    // a perspective projection to it.

    D3D11_BUFFER_DESC constantBufferDesc = { 0 };
    constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
    constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
    constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
    constantBufferDesc.CPUAccessFlags = 0;
    constantBufferDesc.MiscFlags = 0;
    constantBufferDesc.StructureByteStride = 0;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateBuffer(
            &constantBufferDesc,
            nullptr,
            &m_constantBuffer
        )
    );

    // Specify the view transform corresponding to a camera position of
    // X = 0, Y = 1, Z = 2.  For a generalized camera class, see Lesson 5.

    m_constantBufferData.view = DirectX::XMFLOAT4X4(
        -1.00000000f, 0.00000000f, 0.00000000f, 0.00000000f,
        0.00000000f, 0.89442718f, 0.44721359f, 0.00000000f,
        0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
        0.00000000f, 0.00000000f, 0.00000000f, 1.00000000f
    );
});
```

### <a name="3-creating-textures-and-samplers"></a>3. Crear texturas y muestrarios

En este caso, se aplican datos de textura a un cubo en lugar de aplicar colores como en el tutorial anterior, [con la profundidad y los efectos de los primitivos](using-depth-and-effects-on-primitives.md).

Usamos datos de texturas sin procesar para crear texturas.

**Para crear texturas y muestrarios**

1.  En primer lugar, leemos datos de texturas sin procesar en el archivo texturedata.bin del disco.
2.  A continuación, creamos una estructura de [** \_ \_ datos de Subrecursos D3D11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) que haga referencia a esos datos de textura sin procesar.
3.  A continuación, rellenamos una estructura [** \_ \_ DESC de D3D11 TEXTURE2D**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_texture2d_desc) para describir la textura. A continuación, pasamos [**los \_ \_ datos de subrecurso D3D11**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_subresource_data) y las estructuras **D3D11 \_ TEXTURE2D \_ DESC** en una llamada a [**ID3D11Device:: CreateTexture2D**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createtexture2d) para crear la textura.
4.  A continuación, creamos una vista de los recursos de sombreador para la textura, de modo que los sombreadores la puedan usar. Para crear la vista de recursos del sombreador, se rellena una vista de recursos del [** \_ sombreador de D3D11 \_ \_ \_ **](/windows/desktop/api/d3d11/ns-d3d11-d3d11_shader_resource_view_desc) para describir la vista de recursos del sombreador y se pasa la descripción de la vista de recursos del sombreador y la textura a [**ID3D11Device:: CreateShaderResourceView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createshaderresourceview). Por lo general, se hace coincidir la descripción de la vista con la descripción de la textura.
5.  A continuación, creamos un estado de muestrario para la textura. Este estado de muestrario usa datos de texturas relevantes para definir de qué manera se determina el color de una coordenada de textura determinada. Rellenamos una estructura [** \_ \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_sampler_desc) de la muestra de D3D11 para describir el estado de la muestra. A continuación, pasamos la estructura ** \_ \_ DESC** de la muestra de D3D11 en una llamada a [**ID3D11Device:: CreateSamplerState**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createsamplerstate) para crear el estado de la muestra.
6.  Por último, declaramos una variable *degree* que vamos a usar para animar al cubo haciéndolo girar en cada fotograma.

```cppcx
// Load the raw texture data from disk and construct a subresource description that references it.
auto loadTDTask = DX::ReadDataAsync(L"texturedata.bin");

auto constructSubresourceTask = loadTDTask.then([this](const std::vector<byte>& textureData)
{
    D3D11_SUBRESOURCE_DATA textureSubresourceData = { 0 };
    textureSubresourceData.pSysMem = textureData.data();

    // Specify the size of a row in bytes, known as a priori about the texture data.
    textureSubresourceData.SysMemPitch = 1024;

    // As this is not a texture array or 3D texture, this parameter is ignored.
    textureSubresourceData.SysMemSlicePitch = 0;

    // Create a texture description from information known as a priori about the data.
    // Generalized texture loading code can be found in the Resource Loading sample.
    D3D11_TEXTURE2D_DESC textureDesc = { 0 };
    textureDesc.Width = 256;
    textureDesc.Height = 256;
    textureDesc.Format = DXGI_FORMAT_R8G8B8A8_UNORM;
    textureDesc.Usage = D3D11_USAGE_DEFAULT;
    textureDesc.CPUAccessFlags = 0;
    textureDesc.MiscFlags = 0;

    // Most textures contain more than one MIP level.  For simplicity, this sample uses only one.
    textureDesc.MipLevels = 1;

    // As this will not be a texture array, this parameter is ignored.
    textureDesc.ArraySize = 1;

    // Don't use multi-sampling.
    textureDesc.SampleDesc.Count = 1;
    textureDesc.SampleDesc.Quality = 0;

    // Allow the texture to be bound as a shader resource.
    textureDesc.BindFlags = D3D11_BIND_SHADER_RESOURCE;

    ComPtr<ID3D11Texture2D> texture;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &textureDesc,
            &textureSubresourceData,
            &texture
        )
    );

    // Once the texture is created, we must create a shader resource view of it
    // so that shaders may use it.  In general, the view description will match
    // the texture description.
    D3D11_SHADER_RESOURCE_VIEW_DESC textureViewDesc;
    ZeroMemory(&textureViewDesc, sizeof(textureViewDesc));
    textureViewDesc.Format = textureDesc.Format;
    textureViewDesc.ViewDimension = D3D11_SRV_DIMENSION_TEXTURE2D;
    textureViewDesc.Texture2D.MipLevels = textureDesc.MipLevels;
    textureViewDesc.Texture2D.MostDetailedMip = 0;

    ComPtr<ID3D11ShaderResourceView> textureView;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateShaderResourceView(
            texture.Get(),
            &textureViewDesc,
            &textureView
        )
    );

    // Once the texture view is created, create a sampler.  This defines how the color
    // for a particular texture coordinate is determined using the relevant texture data.
    D3D11_SAMPLER_DESC samplerDesc;
    ZeroMemory(&samplerDesc, sizeof(samplerDesc));

    samplerDesc.Filter = D3D11_FILTER_MIN_MAG_MIP_LINEAR;

    // The sampler does not use anisotropic filtering, so this parameter is ignored.
    samplerDesc.MaxAnisotropy = 0;

    // Specify how texture coordinates outside of the range 0..1 are resolved.
    samplerDesc.AddressU = D3D11_TEXTURE_ADDRESS_WRAP;
    samplerDesc.AddressV = D3D11_TEXTURE_ADDRESS_WRAP;
    samplerDesc.AddressW = D3D11_TEXTURE_ADDRESS_WRAP;

    // Use no special MIP clamping or bias.
    samplerDesc.MipLODBias = 0.0f;
    samplerDesc.MinLOD = 0;
    samplerDesc.MaxLOD = D3D11_FLOAT32_MAX;

    // Don't use a comparison function.
    samplerDesc.ComparisonFunc = D3D11_COMPARISON_NEVER;

    // Border address mode is not used, so this parameter is ignored.
    samplerDesc.BorderColor[0] = 0.0f;
    samplerDesc.BorderColor[1] = 0.0f;
    samplerDesc.BorderColor[2] = 0.0f;
    samplerDesc.BorderColor[3] = 0.0f;

    ComPtr<ID3D11SamplerState> sampler;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateSamplerState(
            &samplerDesc,
            &sampler
        )
    );
});

// This value will be used to animate the cube by rotating it every frame;
float degree = 0.0f;
```

### <a name="4-rotating-and-drawing-the-textured-cube-and-presenting-the-rendered-image"></a>4. Girar y dibujar el cubo con textura y mostrar la imagen representada

Al igual que en los tutoriales anteriores, introducimos un bucle sin fin para representar y mostrar continuamente la escena. Llamamos a la función insertada **rotationY** (BasicMath.h) con una cantidad de rotación para establecer los valores que girarán la matriz de modelo del cubo en torno al eje Y. A continuación, llamamos a [**ID3D11DeviceContext::UpdateSubresource**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-updatesubresource) para actualizar el búfer de constantes y girar el modelo del cubo. Llamamos a [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) para especificar el destino de representación y la vista de la galería de símbolos y profundidad. Llamamos a [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) para borrar el destino de representación de color azul y llamamos a [**ID3D11DeviceContext::ClearDepthStencilView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-cleardepthstencilview) para borrar el búfer de profundidad.

En el bucle sin fin, también dibujamos el cubo con textura en la superficie azul.

**Para dibujar el cubo con textura**

1.  En primer lugar, llamamos a [**ID3D11DeviceContext::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) para describir cómo se emitirán los datos del búfer de vértices a la etapa del ensamblador.
2.  A continuación, llamamos a [**ID3D11DeviceContext::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) y [**ID3D11DeviceContext::IASetIndexBuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer) para enlazar los búferes de vértices e índices a la etapa del ensamblador de entrada.
3.  A continuación, se llama a [**ID3D11DeviceContext:: IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) con el valor TRIANGLESTRIP de la [** \_ \_ topología \_ primitiva D3D11**](/previous-versions/windows/desktop/legacy/ff476189(v=vs.85)) para especificar que la etapa del ensamblador de entrada interprete los datos de vértice como una franja de triángulo.
4.  A continuación, llamamos a [**ID3D11DeviceContext::VSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader) para inicializar la etapa del sombreador de vértices con el código del sombreador de vértices e [**ID3D11DeviceContext::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader) para inicializar la etapa del sombreador de píxeles con el código del sombreador de píxeles.
5.  A continuación, llamamos a [**ID3D11DeviceContext::VSSetConstantBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetconstantbuffers) para establecer el búfer de constantes que se usa en la fase de canalización del sombreador de vértices.
6.  Luego llamamos a [**PSSetShaderResources**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshaderresources) para enlazar la vista de los recursos de sombreador de la textura con la fase de canalización del sombreador de píxeles.
7.  A continuación, llamamos a [**PSSetSamplers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetsamplers) para establecer el estado de muestrario en la fase de canalización del sombreador de píxeles.
8.  Por último, llamamos a [**ID3D11DeviceContext::DrawIndexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) para dibujar el cubo y enviarlo a la canalización de representación.

Al igual que en los tutoriales anteriores, llamamos a [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) para mostrar la imagen representada en la ventana.

```cppcx
// Update the constant buffer to rotate the cube model.
m_constantBufferData.model = DirectX::XMMatrixRotationY(-degree);
degree += 1.0f;

m_d3dDeviceContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    nullptr,
    &m_constantBufferData,
    0,
    0
);

// Specify the render target and depth stencil we created as the output target.
m_d3dDeviceContext->OMSetRenderTargets(
    1,
    m_renderTargetView.GetAddressOf(),
    m_depthStencilView.Get()
);

// Clear the render target to a solid color, and reset the depth stencil.
const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
m_d3dDeviceContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    clearColor
);

m_d3dDeviceContext->ClearDepthStencilView(
    m_depthStencilView.Get(),
    D3D11_CLEAR_DEPTH,
    1.0f,
    0
);

m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());

// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(BasicVertex);
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

m_d3dDeviceContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf()
);

m_d3dDeviceContext->PSSetShader(
    pixelShader.Get(),
    nullptr,
    0
);

m_d3dDeviceContext->PSSetShaderResources(
    0,
    1,
    textureView.GetAddressOf()
);

m_d3dDeviceContext->PSSetSamplers(
    0,
    1,
    sampler.GetAddressOf()
);

// Draw the cube.
m_d3dDeviceContext->DrawIndexed(
    ARRAYSIZE(cubeIndices),
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

## <a name="summary"></a>Resumen

En este tema, se cargaron datos de textura sin procesar y se aplicaron esos datos a una primitiva 3D.