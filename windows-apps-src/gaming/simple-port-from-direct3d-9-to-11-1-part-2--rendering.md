---
author: mtoepke
title: "Convertir el marco de representación"
description: "Aprende a convertir un marco de representación simple de Direct3D 9 a Direct3D 11 y a realizar algunas acciones como portar búferes de geometría, compilar y cargar programas sombreadores HLSL e implementar la cadena de representación en Direct3D 11."
ms.assetid: f6ca1147-9bb8-719a-9a2c-b7ee3e34bd18
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c5cdddbf2bf75da761f4439ef2d890170c6681c5

---

# Convertir el marco de representación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**Resumen**

-   [Parte 1: Inicializar Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   Parte 2: Convertir el marco de representación
-   [Parte 3: Migrar el bucle del juego](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md)


Aprende a convertir un marco de representación simple de Direct3D 9 a Direct3D 11 y a realizar algunas acciones como migrar búferes de geometría, compilar y cargar programas sombreadores HLSL e implementar la cadena de representación en Direct3D 11. Parte 2 del tutorial [Migrar una aplicación simple de Direct3D9 a DirectX11 y la Plataforma universal de Windows (UWP)](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

## Convertir efectos a sombreadores HLSL


El siguiente ejemplo es una técnica simple de D3DX escrita para la API heredada de efectos, para los datos de color de paso y la transformación de vértices en el hardware.

Código de sombreador en Direct3D 9

```cpp
// Global variables
matrix g_mWorld;        // world matrix for object
matrix g_View;          // view matrix
matrix g_Projection;    // projection matrix

// Shader pipeline structures
struct VS_OUTPUT
{
    float4 Position   : POSITION;   // vertex position
    float4 Color      : COLOR0;     // vertex diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : COLOR0;  // Pixel color    
};

// Vertex shader
VS_OUTPUT RenderSceneVS(float3 vPos : POSITION, 
                        float3 vColor : COLOR0)
{
    VS_OUTPUT Output;
    
    float4 pos = float4(vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, g_mWorld);
    pos = mul(pos, g_View);
    pos = mul(pos, g_Projection);

    Output.Position = pos;
    
    // Just pass through the color data
    Output.Color = float4(vColor, 1.0f);
    
    return Output;
}

// Pixel shader
PS_OUTPUT RenderScenePS(VS_OUTPUT In) 
{ 
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}

// Technique
technique RenderSceneSimple
{
    pass P0
    {          
        VertexShader = compile vs_2_0 RenderSceneVS();
        PixelShader  = compile ps_2_0 RenderScenePS(); 
    }
}
```

En Direct3D 11 aún podemos usar nuestros sombreadores HLSL. Ponemos cada sombreador en su propio archivo HLSL para que Visual Studio los compile en distintos archivos. Luego los cargamos como recursos independientes de Direct3D. Establecemos el nivel de destino en [Sombreador modelo 4 nivel 9\_1 (/4\_0\_level\_9\_1)](https://msdn.microsoft.com/library/windows/desktop/ff476876) porque estos sombreadores se han escrito para las unidades de procesamiento gráfico (GPU) de DirectX 9.1.

Después de definir el diseño de entrada, nos aseguramos de que este represente la misma estructura de datos que usamos para almacenar datos de vértice en la memoria del sistema y en la memoria de GPU. De manera similar, la salida de un sombreador de vértices debe coincidir con la estructura que se usó como entrada en el sombreador de píxeles. Las reglas difieren de las de pasar datos de una función a otra en C++. Puedes omitir las variables no usadas al final de la estructura. No obstante, no se puede reorganizar el orden ni omitir contenido en el medio de la estructura de datos.

> **Nota**  
Las reglas que Direct3D 9 ofrecía para enlazar sombreadores de vértices con sombreadores de píxeles eran menos estrictas que las reglas de Direct3D 11. La organización en Direct3D 9 era flexible pero ineficiente.

 

Es posible que tus archivos HLSL usen la sintaxis anterior para la semántica de sombreador, por ejemplo, COLOR en lugar de SV\_TARGET. En ese caso, tendrás que habilitar el modo de compatibilidad HLSL (opción del compilador /Gec) o actualizar la [semántica](https://msdn.microsoft.com/library/windows/desktop/bb509647) del sombreador para la sintaxis actual. En este ejemplo, se actualizó el sombreador de vértices con la sintaxis actual.

Este es nuestro sombreador de vértices para la transformación de hardware, esta vez definido en su propio archivo.

> **Nota** Los sombreadores de vértices son necesarios para producir la salida de la semántica del valor del sistema SV\_POSITION. Esta semántica resuelve los datos de posición de vértice para coordinar valores donde: el valor de X está entre -1 y 1; el de Y está entre -1 y 1; el de Z se divide por el valor W de la coordenada homogénea original (Z/W) y el de W es 1 dividido por el valor W original (1/W).

 

Sombreador de vértices HLSL (nivel de característica 9.1)

```cpp
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
    matrix mWorld;       // world matrix for object
    matrix View;        // view matrix
    matrix Projection;  // projection matrix
};

struct VS_INPUT
{
    float3 vPos   : POSITION;
    float3 vColor : COLOR0;
};

struct VS_OUTPUT
{
    float4 Position : SV_POSITION; // Vertex shaders must output SV_POSITION
    float4 Color    : COLOR0;
};

VS_OUTPUT main(VS_INPUT input) // main is the default function name
{
    VS_OUTPUT Output;

    float4 pos = float4(input.vPos, 1.0f);

    // Transform the position from object space to homogeneous projection space
    pos = mul(pos, mWorld);
    pos = mul(pos, View);
    pos = mul(pos, Projection);
    Output.Position = pos;

    // Just pass through the color data
    Output.Color = float4(input.vColor, 1.0f);

    return Output;
}
```

Esto es todo lo que necesitamos para nuestro sombreador de píxeles de paso. Aunque lo llamamos de paso, en verdad obtiene datos de color interpolados en correcta perspectiva para cada píxel. Ten en cuenta que nuestro sombreador de píxeles aplica la semántica del valor del sistema SV\_TARGET a la salida del valor de color, como lo requiere la API.

> **Nota** Los sombreadores de píxeles del nivel 9\_x no se pueden leer en la semántica del valor del sistema SV\_POSITION. Los sombreadores de píxeles de modelo 4.0 (y posterior) pueden usar SV\_POSITION para recuperar la ubicación de píxeles en la pantalla, donde x está entre 0 y el ancho del destino de representación e y está entre 0 y el alto del destino de representación (con desplazamiento de 0,5 cada uno).

 

La mayoría de los sombreadores de píxeles son mucho más complejos que uno de paso. Recuerda que los niveles más altos de característica de Direct3D permiten hacer un mayor número de cálculos por programa sombreador.

Sombreador de píxeles HLSL (nivel de característica 9.1)

```cpp
struct PS_INPUT
{
    float4 Position : SV_POSITION;  // interpolated vertex position (system value)
    float4 Color    : COLOR0;       // interpolated diffuse color
};

struct PS_OUTPUT
{
    float4 RGBColor : SV_TARGET;  // pixel color (your PS computes this system value)
};

PS_OUTPUT main(PS_INPUT In)
{
    PS_OUTPUT Output;

    Output.RGBColor = In.Color;

    return Output;
}
```

## Compilar y cargar sombreadores


Los juegos de Direct3D 9 por lo general usaban la biblioteca de efectos como una forma conveniente de implementar canalizaciones programables. Los efectos pueden compilarse en tiempo de ejecución con el método [**función D3DXCreateEffectFromFile**](https://msdn.microsoft.com/library/windows/desktop/bb172768).

Cargar un efecto en Direct3D 9

```cpp
// Turn off preshader optimization to keep calculations on the GPU
DWORD dwShaderFlags = D3DXSHADER_NO_PRESHADER;

// Only enable debug info when compiling for a debug target
#if defined (DEBUG) || defined (_DEBUG)
dwShaderFlags |= D3DXSHADER_DEBUG;
#endif

D3DXCreateEffectFromFile(
    m_pd3dDevice,
    L"CubeShaders.fx",
    NULL,
    NULL,
    dwShaderFlags,
    NULL,
    &m_pEffect,
    NULL
    );
```

Direct3D 11 funciona con programas sombreadores como recursos binarios. Los sombreadores se compilan una vez creado el proyecto y luego se consideran como recursos. Por lo tanto, nuestro ejemplo cargará el código de bytes del sombreador en la memoria del sistema, usará la interfaz de dispositivos de Direct3D para crear un recurso de Direct3D para cada sombreador y elegirá recursos de sombreador Direct3D cuando configuremos cada marco.

Cargar un recurso de sombreador en Direct3D 11

```cpp
// BasicReaderWriter is a tested file loader used in SDK samples.
BasicReaderWriter^ readerWriter = ref new BasicReaderWriter();


// Load vertex shader:
Platform::Array<byte>^ vertexShaderData =
    readerWriter->ReadData("CubeVertexShader.cso");

// This call allocates a device resource, validates the vertex shader 
// with the device feature level, and stores the vertex shader bits in 
// graphics memory.
m_d3dDevice->CreateVertexShader(
    vertexShaderData->Data,
    vertexShaderData->Length,
    nullptr,
    &m_vertexShader
    );
```

Para incluir el código de bytes del sombreador en el paquete de la aplicación compilado, solo agrega el archivo HLSL al proyecto de Visual Studio. Visual Studio usará la [herramienta compiladora de efectos](https://msdn.microsoft.com/library/windows/desktop/bb232919) (FXC) para compilar archivos HLSL en objetos de sombreador compilado (archivos .CSO) e incluirlos en el paquete de la aplicación.

> **Nota** Asegúrate de establecer el nivel de característica de destino correcto para el compilador HLSL. Haz clic con el botón derecho en el archivo de origen HLSL en Visual Studio, selecciona Propiedades y cambia la configuración de **Modelo de sombreador** en **Compilador HLSL -&gt; General**. Direct3D compara esta propiedad con las capacidades de hardware cuando la aplicación crea el recurso de sombreador de Direct3D.

 

![Propiedades de sombreador HLSL](images/hlslshaderpropertiesmenu.png)![Tipo de sombreador HLSL](images/hlslshadertypeproperties.png)

Este es un buen lugar para crear el diseño de entrada, que corresponde a la declaración de la transmisión de vértices en Direct3D 9. La estructura de datos de vértice debe coincidir con lo que el sombreador de vértices usa. En Direct3D 11 tenemos un mayor control del diseño de entrada; podemos definir el tamaño de la matriz y la longitud en bits de los vectores de punto flotante, y también podemos especificar la semántica para el sombreador de vértices. Creamos una estructura [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180) y la usamos para informar a Direct3D el aspecto que tendrán los datos de vértice. Esperamos que termine la carga del sombreador de vértices para definir el diseño de entrada, porque la API lo valida con el recurso de sombreador de vértices. Si el diseño de entrada no es compatible, entonces Direct3D inicia una excepción.

Los datos de vértice se deben almacenar en tipos compatibles en la memoria del sistema. Los tipos de datos DirectXMath pueden ser útiles, por ejemplo, DXGI\_FORMAT\_R32G32B32\_FLOAT corresponde a [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475).

> **Nota** Los búferes de constantes usan un diseño de entrada fijo que se alinea a cuatro números de punto flotante a la vez. [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) (y sus derivados) se recomiendan para datos de búferes de constantes.

 

Establecer el diseño de entrada en Direct3D 11

```cpp
// Create input layout:
const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT,
        0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },

    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 
        0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};
```

## Crear recursos de geometría


En Direct3D 9, almacenamos recursos de geometría al crear búferes en el dispositivo Direct3D, bloquear la memoria y copiar datos de la memoria de CPU a la memoria de GPU.

Direct3D 9

```cpp
// Create vertex buffer:
VOID* pVertices;

// In Direct3D 9 we create the buffer, lock it, and copy the data from 
// system memory to graphics memory.
m_pd3dDevice->CreateVertexBuffer(
    sizeof(CubeVertices),
    0,
    D3DFVF_XYZ | D3DFVF_DIFFUSE,
    D3DPOOL_MANAGED,
    &pVertexBuffer,
    NULL);

pVertexBuffer->Lock(
    0,
    sizeof(CubeVertices),
    &pVertices,
    0);

memcpy(pVertices, CubeVertices, sizeof(CubeVertices));
pVertexBuffer->Unlock();
```

DirectX 11 sigue un proceso más simple. La API copia automáticamente los datos de la memoria del sistema a la GPU. Podemos usar punteros inteligentes COM para que la programación resulte más fácil.

DirectX 11

```cpp
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = CubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(
    sizeof(CubeVertices),
    D3D11_BIND_VERTEX_BUFFER);
  
// This call allocates a device resource for the vertex buffer and copies
// in the data.
m_d3dDevice->CreateBuffer(
    &vertexBufferDesc,
    &vertexBufferData,
    &m_vertexBuffer
    );
```

## Implementar la cadena de representación


Los juegos de Direct3D 9 a menudo usaban una cadena de representación basada en efectos. Este tipo de cadena de representación configura el objeto de efecto, le proporciona los recursos que necesita y le permite representar cada pase.

Cadena de representación de Direct3D 9

```cpp
// Clear the render target and the z-buffer.
m_pd3dDevice->Clear(
    0, NULL,
    D3DCLEAR_TARGET | D3DCLEAR_ZBUFFER,
    D3DCOLOR_ARGB(0, 45, 50, 170),
    1.0f, 0
    );

// Set the effect technique
m_pEffect->SetTechnique("RenderSceneSimple");

// Rotate the cube 1 degree per frame.
D3DXMATRIX world;
D3DXMatrixRotationY(&world, D3DXToRadian(m_frameCount++));


// Set the matrices up using traditional functions.
m_pEffect->SetMatrix("g_mWorld", &world);
m_pEffect->SetMatrix("g_View", &m_view);
m_pEffect->SetMatrix("g_Projection", &m_projection);

// Render the scene using the Effects library.
if(SUCCEEDED(m_pd3dDevice->BeginScene()))
{
    // Begin rendering effect passes.
    UINT passes = 0;
    m_pEffect->Begin(&passes, 0);
    
    for (UINT i = 0; i < passes; i++)
    {
        m_pEffect->BeginPass(i);
        
        // Send vertex data to the pipeline.
        m_pd3dDevice->SetFVF(D3DFVF_XYZ | D3DFVF_DIFFUSE);
        m_pd3dDevice->SetStreamSource(
            0, pVertexBuffer,
            0, sizeof(VertexPositionColor)
            );
        m_pd3dDevice->SetIndices(pIndexBuffer);
        
        // Draw the cube.
        m_pd3dDevice->DrawIndexedPrimitive(
            D3DPT_TRIANGLELIST,
            0, 0, 8, 0, 12
            );
        m_pEffect->EndPass();
    }
    m_pEffect->End();
    
    // End drawing.
    m_pd3dDevice->EndScene();
}

// Present frame:
// Show the frame on the primary surface.
m_pd3dDevice->Present(NULL, NULL, NULL, NULL);
```

La cadena de representación de DirectX 11 seguirá haciendo las mismas tareas, pero es necesario implementar las transferencias de representación de otra manera. En lugar de colocar las especificaciones en archivos FX y dejar que las técnicas de representación sean más o menos opacas para nuestro código C++, configuraremos toda la representación en C++.

Esta es la manera en que se verá la cadena de representación. Necesitamos suministrar el diseño de entrada que creamos después de cargar el sombreador de vértices, suministrar cada uno de los objetos de sombreador y especificar los búferes de constantes que cada sombreador usará. Este ejemplo no incluye varias transferencias de representación, pero si lo hiciera, haríamos una cadena similar para cada transferencia, cambiando la configuración según sea necesario.

Cadena de representación de Direct3D 11

```cpp
// Clear the back buffer.
const float midnightBlue[] = { 0.098f, 0.098f, 0.439f, 1.000f };
m_d3dContext->ClearRenderTargetView(
    m_renderTargetView.Get(),
    midnightBlue
    );

// Set the render target. This starts the drawing operation.
m_d3dContext->OMSetRenderTargets(
    1,  // number of render target views for this drawing operation.
    m_renderTargetView.GetAddressOf(),
    nullptr
    );


// Rotate the cube 1 degree per frame.
XMStoreFloat4x4(
    &m_constantBufferData.model, 
    XMMatrixTranspose(XMMatrixRotationY(m_frameCount++ * XM_PI / 180.f))
    );

// Copy the updated constant buffer from system memory to video memory.
m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,      // update the 0th subresource
    NULL,   // use the whole destination
    &m_constantBufferData,
    0,      // default pitch
    0       // default pitch
    );


// Send vertex data to the Input Assembler stage.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;

m_d3dContext->IASetVertexBuffers(
    0,  // start with the first vertex buffer
    1,  // one vertex buffer
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset
    );

m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0   // no offset
    );

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());


// Set the vertex shader.
m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0
    );

// Set the vertex shader constant buffer data.
m_d3dContext->VSSetConstantBuffers(
    0,  // register 0
    1,  // one constant buffer
    m_constantBuffer.GetAddressOf()
    );


// Set the pixel shader.
m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0
    );


// Draw the cube.
m_d3dContext->DrawIndexed(
    m_indexCount,
    0,  // start with index 0
    0   // start with vertex 0
    );
```

La cadena de intercambio es parte de la infraestructura de gráficos, por lo tanto, usamos nuestra cadena DXGI para presentar el marco completo. DXGI bloquea la llamada hasta la siguiente sincronización vertical (vsync), luego vuelve y nuestro bucle del juego puede continuar con la siguiente iteración.

Presentar un marco en pantalla con DirectX 11

```cpp
m_swapChain->Present(1, 0);
```

La cadena de representación que acabamos de crear recibirá una llamada proveniente de un bucle de juego implementado en el método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505). Esto se muestra en [Parte 3: Ventanilla y bucle del juego](simple-port-from-direct3d-9-to-11-1-part-3--viewport-and-game-loop.md).

 

 







<!--HONumber=Aug16_HO3-->


