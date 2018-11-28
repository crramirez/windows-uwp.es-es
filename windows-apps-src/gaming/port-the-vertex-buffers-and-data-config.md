---
title: Migrar datos y búferes de vértices
description: En este paso, definirás los búferes de vértices que contendrán las mallas, y los búferes de índices que permitirán a los sombreadores recorrer los vértices en un orden especificado.
ms.assetid: 9a8138a5-0797-8532-6c00-58b907197a25
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, port, portar, vertex buffers, búferes de vértices, data, datos, direct3d
ms.localizationpriority: medium
ms.openlocfilehash: 4c961a8852fb1e03e4e86209f62bda821b980f8c
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7964745"
---
# <a name="port-the-vertex-buffers-and-data"></a>Portar datos y búferes de vértices




**API importantes**

-   [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501)
-   [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456)
-   [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588)

En este paso, definirás los búferes de vértices que contendrán las mallas, y los búferes de índices que permitirán a los sombreadores recorrer los vértices en un orden especificado.

Examinemos ahora el modelo rígido de la malla del cubo que estamos usando. Ambas representaciones tienen vértices organizados en una lista de triángulos (de manera opuesta a una franja o a otro diseño de triángulos más eficiente). Todos los vértices en ambas representaciones también tienen valores de color e índices asociados. La mayor parte del código Direct3D de este tema hace referencia a variables y objetos definidos en el proyecto de Direct3D.

Este es el cubo para que OpenGL ES 2.0 procese. En la implementación de la muestra, cada vértice consiste en 7 valores flotantes: 3 coordenadas de posición seguidas de 4 valores de color RGBA.

```cpp
#define CUBE_INDICES 36
#define CUBE_VERTICES 8

GLfloat cubeVertsAndColors[] = 
{
  -0.5f, -0.5f,  0.5f, 0.0f, 0.0f, 1.0f, 1.0f,
  -0.5f, -0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f,
  -0.5f,  0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 1.0f,
  -0.5f,  0.5f, -0.5f, 0.0f, 1.0f, 0.0f, 1.0f,
  0.5f, -0.5f,  0.5f, 1.0f, 0.0f, 1.0f, 1.0f,
  0.5f, -0.5f, -0.5f, 1.0f, 0.0f, 0.0f, 1.0f,  
  0.5f,  0.5f,  0.5f, 1.0f, 1.0f, 1.0f, 1.0f,
  0.5f,  0.5f, -0.5f, 1.0f, 1.0f, 0.0f, 1.0f
};

GLuint cubeIndices[] = 
{
  0, 1, 2, // -x
  1, 3, 2,

  4, 6, 5, // +x
  6, 7, 5,

  0, 5, 1, // -y
  5, 6, 1,

  2, 6, 3, // +y
  6, 7, 3,

  0, 4, 2, // +z
  4, 6, 2,

  1, 7, 3, // -z
  5, 7, 1
};
```

Y este es el mismo cubo para procesar con Direct3D 11.

```cpp
VertexPositionColor cubeVerticesAndColors[] = 
// struct format is position, color
{
  {XMFLOAT3(-0.5f, -0.5f, -0.5f), XMFLOAT3(0.0f, 0.0f, 0.0f)},
  {XMFLOAT3(-0.5f, -0.5f,  0.5f), XMFLOAT3(0.0f, 0.0f, 1.0f)},
  {XMFLOAT3(-0.5f,  0.5f, -0.5f), XMFLOAT3(0.0f, 1.0f, 0.0f)},
  {XMFLOAT3(-0.5f,  0.5f,  0.5f), XMFLOAT3(0.0f, 1.0f, 1.0f)},
  {XMFLOAT3( 0.5f, -0.5f, -0.5f), XMFLOAT3(1.0f, 0.0f, 0.0f)},
  {XMFLOAT3( 0.5f, -0.5f,  0.5f), XMFLOAT3(1.0f, 0.0f, 1.0f)},
  {XMFLOAT3( 0.5f,  0.5f, -0.5f), XMFLOAT3(1.0f, 1.0f, 0.0f)},
  {XMFLOAT3( 0.5f,  0.5f,  0.5f), XMFLOAT3(1.0f, 1.0f, 1.0f)},
};
        
unsigned short cubeIndices[] = 
{
  0, 2, 1, // -x
  1, 2, 3,

  4, 5, 6, // +x
  5, 7, 6,

  0, 1, 5, // -y
  0, 5, 4,

  2, 6, 7, // +y
  2, 7, 3,

  0, 4, 6, // -z
  0, 6, 2,

  1, 3, 7, // +z
  1, 7, 5
};
```

Si revisas este código, notas que el cubo en OpenGL ES 2.0 se representa en un sistema de coordenadas a la derecha, mientras que el cubo en el código específico de Direct3D se representa en un sistema de coordenadas a la izquierda. Cuando importas tu propia malla de datos, debes invertir las coordenadas del eje z del modelo y cambiar los índices de cada malla, para recorrer los triángulos de acuerdo con el cambio del sistema de coordenadas.

Suponiendo que trasladaste correctamente la malla de cubo del sistema de coordenadas que se encuentra a la derecha de OpenGL ES 2.0 al sistema de coordenadas que se encuentra a la izquierda de Direct3D, ahora veremos cómo cargar los datos del cubo para procesarlos en ambos modelos.

## <a name="instructions"></a>Instrucciones

### <a name="step-1-create-an-input-layout"></a>Paso 1: crear un diseño de entrada

En OpenGL ES 2.0, los datos de vértice se suministran como atributos y estos atributos se proporcionarán a los objetos de sombreador para que los lea. Normalmente, le proporcionas una cadena que contiene el nombre del atributo que figura en el GLSL del sombreador al objeto del programa sombreador y obtienes una ubicación de memoria que puedes suministrar al sombreador. En este ejemplo, un objeto de búfer de vértices contiene una lista de estructuras personalizadas de vértice a las que se define y se da formato de esta manera: 

OpenGL ES 2.0: configurar los atributos que contiene la información por vértice

``` syntax
typedef struct 
{
  GLfloat pos[3];        
  GLfloat rgba[4];
} Vertex;
```

En OpenGL ES 2.0, los diseños de entrada son implícitos. Tomas un elemento GL\_ELEMENT\_ARRAY\_BUFFER de propósito general y suministras el intervalo y el desplazamiento para que el sombreador de vértices pueda interpretar los datos después de la carga. Debes informar al sombreador antes de representar los atributos que se asignarán a determinadas porciones de cada bloque de datos de vértice mediante **glVertexAttribPointer**.

Cuando se trata de Direct3D, debes proporcionar un diseño de entrada para describir la estructura de los datos de vértice en el búfer de vértices cuando creas el búfer, en lugar de hacerlo antes de dibujar la geometría. Para ello, puedes usar un diseño de entrada que se corresponda con el diseño de los datos de nuestros vértices individuales en la memoria. Es muy importante que especifiques esto de la manera correcta.

Una vez hecho esto, debes crear una descripción de entrada como una matriz de estructuras [**D3D11\_INPUT\_ELEMENT\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476180).

Direct3D: definir una descripción de diseño de entrada

``` syntax
struct VertexPositionColor
{
  DirectX::XMFLOAT3 pos;
  DirectX::XMFLOAT3 color;
};

// ...

const D3D11_INPUT_ELEMENT_DESC vertexDesc[] = 
{
  { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
  { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

```

Esta descripción de entrada define un vértice como un par de vectores de 3 coordenadas: un vector 3D para almacenar la posición del vértice en las coordenadas del modelo y otro vector 3D para almacenar el valor de color RGB asociado con el vértice. En este caso, usas un formato de punto flotante de 3x32 bits, cuyos elementos se representan como `XMFLOAT3(X.Xf, X.Xf, X.Xf)` en nuestro código. Recuerda que debes usar los tipos de la biblioteca [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415574) siempre que estés manipulando datos que posteriormente usará un sombreador, dado que esto garantiza un correcto empaquetado y alineamiento de esos datos. (Por ejemplo, usa [**XMFLOAT3**](https://msdn.microsoft.com/library/windows/desktop/ee419475) o [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) para los datos de vector y [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) para las matrices).

Para obtener una lista de todos los tipos de formato posibles, consulta [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059).

Mediante el diseño de entrada por vértice definido, crearás el objeto de diseño. En el siguiente código, escribes en **m\_inputLayout** una variable de tipo **ComPtr** (que apunta a un objeto de tipo [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575)). **fileData** contiene el objeto de sombreador de vértices compilado del paso anterior, [Portar los sombreadores](port-the-shader-config.md).

Direct3D: crear el diseño de entrada utilizado por el búfer de vértices

``` syntax
Microsoft::WRL::ComPtr<ID3D11InputLayout>      m_inputLayout;

// ...

m_d3dDevice->CreateInputLayout(
  vertexDesc,
  ARRAYSIZE(vertexDesc),
  fileData->Data,
  fileShaderData->Length,
  &m_inputLayout
);
```

Ya definimos el diseño de entrada. A continuación, crearemos un búfer que use este diseño y lo cargue con los datos de la malla del cubo.

### <a name="step-2-create-and-load-the-vertex-buffers"></a>Paso 2: crear y cargar los búferes de vértices

En OpenGL ES 2.0, debes crear un par de búferes: uno para los datos de posición y otro para los datos de color. (También puedes crear una estructura que pueda contener a ambos o a un solo búfer). A continuación, enlazas cada búfer y escribes en ellos los datos de posición y color. Luego, durante la función de representación, vuelves a enlazar los búferes y le proporcionas al sombreador el formato de los datos del búfer para que pueda interpretarlos correctamente.

OpenGL ES 2.0: enlazar búferes de vértices

``` syntax
// upload the data for the vertex position buffer
glGenBuffers(1, &renderer->vertexBuffer);    
glBindBuffer(GL_ARRAY_BUFFER, renderer->vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(VERTEX) * CUBE_VERTICES, renderer->vertices, GL_STATIC_DRAW);   
```

En cuanto a Direct3D, los búferes accesibles de los sombreadores se representan como estructuras [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220). Para enlazar la ubicación de este búfer al objeto de sombreador, necesitas crear una estructura CD3D11\_BUFFER\_DESC para cada búfer mediante [**ID3DDevice::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501); a continuación debes establecer el contexto de dispositivo de Direct3D llamando a un método set específico del tipo de búfer, como [**ID3DDeviceContext::IASetVertexBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff476456).

Una vez hayas establecido el búfer, debes indicar tanto el intervalo (correspondiente al tamaño del elemento de datos de un vértice individual) como el desplazamiento (donde empieza realmente la matriz de datos de vértice).

Ten en cuenta que asignaremos el puntero a la matriz **vertexIndices** del campo **pSysMem** perteneciente a la estructura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220). Si estos datos no son correctos, querrá decir que la malla está dañada o vacía.

Direct3D: crear y establecer el búfer de vértices

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
        
// ...

UINT stride = sizeof(VertexPositionColor);
UINT offset = 0;
m_d3dContext->IASetVertexBuffers(
  0,
  1,
  m_vertexBuffer.GetAddressOf(),
  &stride,
  &offset);
```

### <a name="step-3-create-and-load-the-index-buffer"></a>Paso 3: crear y cargar los búferes de índices

Los búferes de índices son una manera eficiente de permitir que el sombreador de vértices busque vértices individuales. Aunque no sean obligatorios, los usamos en este representador de muestra. Al igual que con los búferes de vértices en OpenGL ES 2.0, un búfer de índices se crea y enlaza como un búfer de propósito general y allí se copian los índices de vértices que creaste antes.

Cuando estés listo para comenzar a dibujar, debes volver a enlazar ambos búferes (el de vértices y el de índices) y llamar a **glDrawElements**.

OpenGL ES 2.0: enviar el orden de índices a la llamada Draw

``` syntax
GLuint indexBuffer;

// ...

glGenBuffers(1, &renderer->indexBuffer);    
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);   
glBufferData(GL_ELEMENT_ARRAY_BUFFER, 
  sizeof(GLuint) * CUBE_INDICES, 
  renderer->vertexIndices, 
  GL_STATIC_DRAW);

// ...
// Drawing function

// Bind the index buffer
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);
glDrawElements (GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);
```

Con Direct3D, es un proceso bastante similar, aunque un poco más didáctico. Suministra el búfer de índices como un subrecurso de Direct3D a la interfaz [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) que creaste cuando configuraste Direct3D. Para ello, llama a [**ID3D11DeviceContext::IASetIndexBuffer**](https://msdn.microsoft.com/library/windows/desktop/bb173588) mediante el subrecurso configurado de la matriz de índices, de esta manera: (Recuerda tener en cuenta que asignarás el puntero a la matriz **cubeIndices** del campo **pSysMem** perteneciente a la estructura [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220)).

Direct3D: crear el búfer de índices

``` syntax
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

// ...

m_d3dContext->IASetIndexBuffer(
  m_indexBuffer.Get(),
  DXGI_FORMAT_R16_UINT,
  0);
```

A continuación, dibujarás los triángulos mediante una llamada a [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (o al método [**ID3D11DeviceContext::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407) de los vértices no indizados), de esta manera: (Para obtener más información, consulta la sección [Dibujar en la pantalla](draw-to-the-screen.md)).

Direct3D: dibujar los vértices indizados

``` syntax
m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
m_d3dContext->IASetInputLayout(m_inputLayout.Get());

// ...

m_d3dContext->DrawIndexed(
  m_indexCount,
  0,
  0);
```

## <a name="previous-step"></a>Paso anterior


[Portar objetos de sombreador](port-the-shader-config.md)

## <a name="next-step"></a>Paso siguiente

[Portar GLSL](port-the-glsl.md)

## <a name="remarks"></a>Observaciones

Cuando estructures tu contenido de Direct3D, separa el código que llama a los métodos de [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379) y reúnelo en un método que recibirá llamadas siempre que necesites volver a crear los recursos del dispositivo. En la plantilla del proyecto de Direct3D, este código está en los métodos **CreateDeviceResource** del objeto del representador). Asimismo, el código que actualiza el contexto de dispositivo ([**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)), se coloca, por el contrario, en el método **Render** debido a que aquí es donde realmente se construyen las fases del sombreador y donde se enlazan los datos.

## <a name="related-topics"></a>Temas relacionados


* [Portar un representador simple de OpenGL ES 2.0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Portar objetos de sombreador](port-the-shader-config.md)
* [Migrar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)
* [Migrar GLSL](port-the-glsl.md)

 

 




