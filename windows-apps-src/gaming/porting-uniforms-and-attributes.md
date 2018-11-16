---
author: mtoepke
title: Portar búferes, uniformes y vértices de OpenGL ES 2.0 a Direct3D
description: Durante el proceso de migración de OpenGL ES 2.0 a Direct3D 11, debes cambiar la sintaxis y el comportamiento de API para pasar datos entre la aplicación y los programas sombreadores.
ms.assetid: 9b215874-6549-80c5-cc70-c97b571c74fe
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, opengl, direct3d, buffers, búferes, uniforms, uniformes, vertex attributes, atributos de vértice
ms.localizationpriority: medium
ms.openlocfilehash: bc0192eb4b89ef91bc895a96e46cd39524f24c44
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977655"
---
# <a name="compare-opengl-es-20-buffers-uniforms-and-vertex-attributes-to-direct3d"></a>Comparar búferes, uniformes y atributos de vértice de OpenGL ES 2.0 con Direct3D




**API importantes**

-   [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575)
-   [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512)
-   [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454)

Durante el proceso de migración de OpenGL ES 2.0 a Direct3D 11, debes cambiar la sintaxis y el comportamiento de API para pasar datos entre la aplicación y los programas sombreadores.

En OpenGL ES 2.0, los datos se mueven desde y hacia los programas sombreadores de tres formas: como uniformes para datos de constantes, como atributos para datos de vértice y como objetos de búfer para otros datos de recursos (como texturas). A grandes rasgos, esto se asigna a los búferes de constantes, búferes de vértices y subrecursos de Direct3D 11. Pero a pesar de ser aparentemente similares, en el uso difieren bastante.

Esta es la asignación básica.

| OpenGL ES 2.0             | Direct3D 11                                                                                                                                                                         |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| uniforme                   | campo de búfer de constantes (**cbuffer**).                                                                                                                                                |
| atributo                 | campo de elementos de búfer de vértices, designado por un diseño de entrada y marcado con una semántica HLSL específica.                                                                                |
| objeto de búfer             | búfer; consulta [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) y [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) para ver definiciones de búfer de uso general. |
| objeto de búfer de cuadros (FBO) | destinos de representación; consulta [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) con [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635).                                       |
| búfer de reserva               | cadena de intercambio con superficie "búfer de reserva"; consulta [**IDXGISwapChain1**](https://msdn.microsoft.com/library/windows/desktop/hh404631) con la interfaz [**IDXGISurface1**](https://msdn.microsoft.com/library/windows/desktop/ff471343) adjunta.                       |

 

## <a name="port-buffers"></a>Búferes de puertos


En OpenGL ES 2.0, el proceso de crear y enlazar cualquier tipo de búfer, por lo general, sigue este patrón:

-   Llama a glGenBuffers para generar uno o más búferes y devolverles sus identificadores.
-   Llama a glBindBuffer para definir el diseño de un búfer, como GL\_ELEMENT\_ARRAY\_BUFFER.
-   Llama a glBufferData para rellenar el búfer con datos específicos (como estructuras de vértice, datos de índice o datos de color) en un diseño específico.

El tipo de búfer más común es el búfer de vértices, que como mínimo contiene las posiciones de los vértices en algún sistema de coordenadas. Normalmente, un vértice se representa con una estructura que contiene las coordenadas de posición, un vector normal en la posición del vértice, un vector tangente en la posición del vértice y coordenadas de búsqueda de texturas (uv). El búfer tiene una lista contigua de estos vértices en algún orden (como una lista de triángulos, franja o ventilador) y que, de manera colectiva, representan los polígonos visibles de la escena. (En Direct3D 11, al igual que en OpenGL ES 2.0, es ineficiente tener varios búferes de vértices por llamada Draw).

Este es un ejemplo de un búfer de vértices y un búfer de índices creados con OpenGL ES 2.0:

OpenGL ES 2.0: crear y rellenar un búfer de vértices y un búfer de índices

``` syntax
glGenBuffers(1, &renderer->vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(Vertex) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);

glGenBuffers(1, &renderer->indexBuffer);
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(int) * CUBE_INDICES, renderer->vertexIndices, GL_STATIC_DRAW);
```

Otros búferes incluyen búferes de píxeles y mapas, como texturas. La canalización de sombreador puede representar en búferes de texturas (mapas de píxeles) o representar objetos de búfer y usar esos búferes en futuros pases de sombreador. En el más simple de los casos, este es el flujo de llamadas:

-   Llama a glGenFramebuffers para generar un objeto de búfer de trama.
-   Llama a glBindFramebuffer para enlazar el objeto de búfer de trama para la escritura.
-   Llama a glFramebufferTexture2D para dibujar en un mapa de texturas específico.

En Direct3D 11, los elementos de datos del búfer se consideran "subrecursos" y pueden variar entre elementos de datos de vértice individuales y texturas de mapas de MIP.

-   Rellena una estructura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) con la configuración de un elemento de datos de búfer.
-   Rellena una estructura [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) con el tamaño de los elementos individuales del búfer, así como el tipo de búfer.
-   Llama a [**ID3D11Device1::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/hh404575) con estas dos estructuras.

Direct3D 11: crear y rellenar un búfer de vértices y un búfer de índices

``` syntax
D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
vertexBufferData.pSysMem = cubeVertices;
vertexBufferData.SysMemPitch = 0;
vertexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC vertexBufferDesc(sizeof(cubeVertices), D3D11_BIND_VERTEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &vertexBufferDesc,
  &vertexBufferData,
  &m_vertexBuffer);

m_indexCount = ARRAYSIZE(cubeIndices);

D3D11_SUBRESOURCE_DATA indexBufferData = {0};
indexBufferData.pSysMem = cubeIndices;
indexBufferData.SysMemPitch = 0;
indexBufferData.SysMemSlicePitch = 0;
CD3D11_BUFFER_DESC indexBufferDesc(sizeof(cubeIndices), D3D11_BIND_INDEX_BUFFER);

m_d3dDevice->CreateBuffer(
  &indexBufferDesc,
  &indexBufferData,
  &m_indexBuffer);
    
```

Los mapas o búferes de píxeles grabables, como un búfer de cuadros, pueden crearse como objetos [**ID3D11Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff476635). Estos pueden enlazarse como recursos a [**ID3D11RenderTargetView**](https://msdn.microsoft.com/library/windows/desktop/ff476582) o a [**ID3D11ShaderResourceView**](https://msdn.microsoft.com/library/windows/desktop/ff476628) y, una vez enlazados, pueden mostrarse con la cadena de intercambio asociada o pasar a un sombreador, respectivamente.

Direct3D 11: crear un objeto de búfer de trama

``` syntax
ComPtr<ID3D11RenderTargetView> m_d3dRenderTargetViewWin;
// ...
ComPtr<ID3D11Texture2D> frameBuffer;

m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(&frameBuffer));
m_d3dDevice->CreateRenderTargetView(
  frameBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

## <a name="change-uniforms-and-uniform-buffer-objects-to-direct3d-constant-buffers"></a>Cambiar uniformes y objetos de búfer de uniformes a búferes de constantes en Direct3D


En Open GL ES 2.0, los uniformes son los mecanismos para suministrar datos constantes a programas sombreadores individuales. Los sombreadores no pueden alterar estos datos.

Configurar un elemento uniforme, por lo general, implica proporcionar uno de los métodos glUniform\* con la ubicación de carga en la GPU, junto con un puntero en los datos de la memoria de la aplicación. Una vez que el método glUniform\* se ejecuta, los datos de uniformes están en la memoria GPU y son accesibles para los sombreadores que declararon esos elementos uniformes. Debes asegurarte de que los datos estén empaquetados de modo tal que el sombreador pueda interpretarlos según su declaración de uniforme (usando tipos compatibles).

OpenGL ES 2.0: crear un uniforme y cargarle datos

``` syntax
renderer->mvpLoc = glGetUniformLocation(renderer->programObject, "u_mvpMatrix");

// ...

glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);
```

En el GLSL de un sombreador, la correspondiente declaración de uniforme sería similar a la siguiente:

Open GL ES 2.0: declaración de uniforme en GLSL

``` syntax
uniform mat4 u_mvpMatrix;
```

Direct3D designa datos de uniforme como "búferes de constantes", que, al igual que los uniformes, contienen datos de constantes proporcionados a sombreadores individuales. Al igual que con los búferes de uniformes, es importante empaquetar los datos del búfer de constantes en la memoria de forma que el sombreador pueda interpretarlos. Usar tipos de DirectXMath (como [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608)) en lugar de tipos de plataformas (como **float\*** o **float\[4\]**) garantiza una alineación apropiada de los elementos de datos.

Los búferes de constantes deben tener un registro de GPU asociado que se use para hacer referencia a los datos en la GPU. Los datos se empaquetan en la ubicación del registro, como se indica en el diseño del búfer.

Direct3D 11: crear un búfer de constantes y cargarle datos

``` syntax
struct ModelViewProjectionConstantBuffer
{
     DirectX::XMFLOAT4X4 mvp;
};

// ...

ModelViewProjectionConstantBuffer   m_constantBufferData;

// ...

XMStoreFloat4x4(&m_constantBufferData.mvp, mvpMatrix);

CD3D11_BUFFER_DESC constantBufferDesc(sizeof(ModelViewProjectionConstantBuffer), D3D11_BIND_CONSTANT_BUFFER);
m_d3dDevice->CreateBuffer(
  &constantBufferDesc,
  nullptr,
  &m_constantBuffer);
```

En el HSLS de un sombreador, la correspondiente declaración de un búfer de constantes sería similar a la siguiente:

Direct3D 11: declaración de búfer de constantes en HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

Observa que se debe declarar un registro para cada búfer de constantes. Los distintos niveles de característica de Direct3D tienen distintos máximos de registros disponibles. Por lo tanto, no superes la cantidad máxima para el mínimo nivel de característica que estás eligiendo como destino.

## <a name="port-vertex-attributes-to-a-direct3d-input-layouts-and-hlsl-semantics"></a>Migrar atributos de vértice a diseños de entrada de Direct3D y semántica de HLSL


Dado que la canalización de sombreador puede modificar los datos de vértice, OpenGL ES 2.0 requiere que los especifiques como "atributos" en lugar de "uniformes". (Esto cambió en las versiones más recientes de OpenGL y GLSL.) Los datos específicos de vértices con valores de posición, normales, tangente y color se proporcionan en los sombreadores como valores de atributo. Estos valores de atributo corresponden a desplazamientos específicos para cada elemento en los datos de vértice; por ejemplo, el primer atributo podría apuntar al componente de posición de un vértice individual y el segundo, a la normal, y así sucesivamente.

El proceso básico de trasladar los datos del búfer de vértices de la memoria principal a la GPU es similar al siguiente:

-   Carga los datos de vértice con glBindBuffer.
-   Obtén la ubicación de los atributos en la GPU gracias a glGetAttribLocation. Haz una llamada para cada atributo del elemento de datos de vértice.
-   Llama a glVertexAttribPointer para proporcionar el tamaño de atributo correcto y el desplazamiento dentro de un elemento de datos de vértice individual. Haz esto para cada atributo.
-   Habilita la información de diseño de datos de vértice con glEnableVertexAttribArray.

OpenGL ES 2.0: cargar datos del búfer de vértices al atributo de sombreador

``` syntax
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->vertexBuffer);
loc = glGetAttribLocation(renderer->programObject, "a_position");

glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), 0);
loc = glGetAttribLocation(renderer->programObject, "a_color");
glEnableVertexAttribArray(loc);

glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
  sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
glEnableVertexAttribArray(loc);
```

Ahora, en el sombreador de vértices, declaras atributos con los mismos nombres que definiste en la llamada a glGetAttribLocation.

OpenGL ES 2.0: declarar un atributo en GLSL

``` syntax
attribute vec4 a_position;
attribute vec4 a_color;                     
```

De alguna manera, el mismo proceso se mantiene para Direct3D. En lugar de atributos, los datos de vértice se proporcionan en búferes de entrada, que incluyen búferes de vértices y los correspondientes búferes de índices. No obstante, dado que Direct3D no tiene la declaración de "atributo", debes especificar un diseño de entrada que declara el componente individual de los elementos de datos en el búfer de vértices, así como la semántica de HLSL que indica dónde y cómo el sombreador de vértices va a interpretar estos componentes. La semántica de HLSL requiere que definas el uso de cada componente con una cadena específica que informa al motor sombreador sobre su propósito. Por ejemplo, los datos de posición de vértice se marcan como POSITION, los datos normales se marcan como NORMAL y los datos de color de vértice se marcan como COLOR. (Otras fases del sombreador necesitan tener una semántica específica que debe tener diferentes interpretaciones según la fase del sombreador proporcionada). Para obtener más información acerca de la semántica HLSL, te recomendamos que leas [Portar la canalización del sombreador](change-your-shader-loading-code.md) y [Semántica de HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574).

De manera colectiva, el proceso de establecer los búferes de vértices e índices, así como el diseño de entrada, se denomina fase de "Ensamblado de entrada" (IA) de la canalización de gráficos de Direct3D.

Direct3D 11: configurar la fase de ensamblado de entrada

``` syntax
// Set up the IA stage corresponding to the current draw operation.
UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
        0,
        1,
        m_vertexBuffer.GetAddressOf(),
        &stride,
        &offset);

m_d3dContext->IASetIndexBuffer(
        m_indexBuffer.Get(),
        DXGI_FORMAT_R16_UINT,
        0);

m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Un diseño de entrada se declara y asocia con un sombreador de vértices al declarar el formato del elemento de datos de vértice y la semántica usada para cada componente. El diseño de datos de elemento de vértice descrito en D3D11\_INPUT\_ELEMENT\_DESC debe coincidir con el diseño de la estructura correspondiente. En este ejemplo, creas un diseño para los datos de vértice con dos componentes:

-   Una coordenada de posición de vértice, representada en la memoria principal como XMFLOAT3, que es una matriz alineada de 3 valores de puntos flotantes de 32 bits para las coordenadas (x, y, z).
-   Un valor de color de vértice, representado como XMFLOAT4, que es una matriz alineada de 4 valores de puntos flotantes de 32 bits para el color (RGBA).

Debes asignar una semántica y un tipo de formato a cada uno. A continuación, debes pasar la descripción a [**ID3D11Device1::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512). El diseño de entrada se usará cuando llamemos a [**ID3D11DeviceContext1::IASetInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476454) en el momento en que configures el ensamblado de entrada durante el método de representación.

Direct3D 11: describir un diseño de entrada con semántica específica

``` syntax
ComPtr<ID3D11InputLayout> m_inputLayout;

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileData->Length,
  &m_inputLayout);

// ...
// When we start the drawing process...

m_d3dContext->IASetInputLayout(m_inputLayout.Get());
```

Por último, asegúrate de que el sombreador pueda comprender los datos de entrada declarando la entrada. La semántica que asignaste en el diseño se usa para seleccionar las ubicaciones correctas en la memoria de GPU.

Direct3D 11: declarar datos de entrada de sombreador con semántica de HLSL

``` syntax
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};
```

 

 




