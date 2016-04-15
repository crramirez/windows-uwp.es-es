---
title: Crear y mostrar una malla básica
description: En los juegos 3D de la Plataforma universal de Windows (UWP), normalmente se usan polígonos para representar objetos y superficies.
ms.assetid: bfe0ed5b-63d8-935b-a25b-378b36982b7d
---

# Crear y mostrar una malla básica


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En los juegos 3D de la Plataforma universal de Windows (UWP), normalmente se usan polígonos para representar objetos y superficies. Las listas de vértices que comprimen la estructura de estos objetos poligonales y superficies se denominan mallas. A continuación crearemos una malla básica para un objeto de cubo y haremos que se represente y muestre a través de la canalización de sombreador.

> **Importante**   El código de ejemplo incluido aquí usa tipos (como DirectX::XMFLOAT3 y DirectX::XMFLOAT4X4) y métodos insertados declarados en DirectXMath.h. Si vas a cortar y pegar este código, incluye \#include &lt;DirectXMath.h&gt; en el proyecto.

 

## Lo que debes saber


### Tecnologías

-   [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh769064)

### Requisitos previos

-   Conocimiento básico sobre álgebra lineal y sistemas de coordenadas 3D
-   Plantilla Direct3D de Visual Studio 2015

## Instrucciones

### Paso 1: Construir la malla para el modelo

En la mayoría de los juegos, la malla para un objeto del juego se carga desde un archivo que contiene los datos de vértices específicos. El orden de estos vértices depende de la aplicación, pero, por lo general, están serializados en franjas o ventiladores. Los datos de vértices pueden proceder de cualquier origen de software o pueden crearse manualmente. Depende de tu juego interpretar los datos de una forma que el sombreador de vértices pueda procesarlos de manera efectiva.

En nuestro ejemplo, usamos una malla simple para un cubo. El cubo, como cualquier otra malla para objetos en esta etapa de la canalización, se representa con su propio sistema de coordenadas. El sombreador de vértices toma sus coordenadas y, aplicando las matrices de transformación que le proporciones, devuelve la proyección de vista 2D final en un sistema de coordenadas homogéneo.

Define la malla para un cubo. (O cárgala de un archivo. ¡Tú decides!).

```cpp
SimpleCubeVertex cubeVertices[] =
{
    { DirectX::XMFLOAT3(-0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 0.0f) }, // +Y (top face)
    { DirectX::XMFLOAT3( 0.5f, 0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 0.0f) },
    { DirectX::XMFLOAT3( 0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 1.0f, 1.0f) },
    { DirectX::XMFLOAT3(-0.5f, 0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 1.0f, 1.0f) },

    { DirectX::XMFLOAT3(-0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 1.0f) }, // -Y (bottom face)
    { DirectX::XMFLOAT3( 0.5f, -0.5f,  0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 1.0f) },
    { DirectX::XMFLOAT3( 0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(1.0f, 0.0f, 0.0f) },
    { DirectX::XMFLOAT3(-0.5f, -0.5f, -0.5f), DirectX::XMFLOAT3(0.0f, 0.0f, 0.0f) },
};
```

En el sistema de coordenadas del cubo, el centro del cubo se ubica en el origen, el eje x se extiende de arriba a abajo y el sistema de coordenadas se extiende a la izquierda. Los valores de las coordenadas se expresan como valores flotantes de 32 bits entre -1 y 1.

En cada emparejamiento entre corchetes, el segundo grupo de valores DirectX::XMFLOAT3 especifica el color asociado con el vértice como un valor RGB. Por ejemplo, el primer vértice en (-0.5, 0.5, -0.5) tiene el color verde (el valor G se establece en 1.0 y los valores "R" y "B" en 0).

Entonces tienes 8 vértices, cada uno con un color específico. Cada emparejamiento de vértice y color son los datos completos de un vértice en nuestro ejemplo. Cuando especificas nuestro búfer de vértices, debes tener en mente este diseño específico. Proporcionamos este diseño de entrada al sombreador de vértices para que pueda comprender los datos de vértices.

### Paso 2: Configurar el diseño de entrada

Ahora tienes los vértices en la memoria. Pero el dispositivo de gráficos tiene su propia memoria a la que accedes con Direct3D. Cuando introduces los datos de vértices en el dispositivo de gráficos para que se procesen, necesitas facilitar las cosas: debes declarar cómo aparecen estos datos para el que dispositivo los interprete cuando los obtenga desde el juego. Para ello, usas [**ID3D11InputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476575).

Declara y establece el diseño de entrada para el búfer de vértices.

```cpp
const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
{
    { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
    { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
};

ComPtr<ID3D11InputLayout> inputLayout;
m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                vertexShaderBytecode->Data,
                vertexShaderBytecode->Length,
                &inputLayout)
);
```

En este código, especificas un diseño para los vértices, o sea qué datos contiene cada elemento de la lista de vértices. Aquí, en **basicVertexLayoutDesc**, especificas dos componentes de datos:

-   **POSICIÓN**: semántica HLSL para los datos de posición proporcionados a un sombreador. En este código, es un valor DirectX::XMFLOAT3 o, más específicamente, una estructura con 3 valores de punto flotante de 32 bits que corresponden a una coordenada 3D (x, y, z). También podrías usar un valor float4, si estás dando la coordenada homogénea "w". En ese caso, especificas DXGI\_FORMAT\_R32G32B32A32\_FLOAT. Usar un valor DirectX::XMFLOAT3 o un valor float4 depende de las necesidades específicas del juego. Tan solo asegúrate de que los datos de vértices para la malla sean los que corresponden al formato que usas.

    Cada valor de coordenada se expresa como un valor de punto flotante entre -1 y 1, en el espacio de coordenadas del objeto. Cuando el sombreador de vértices se completa, el vértice transformado se encuentra en el espacio de proyección de vista homogénea (perspectiva corregida).

    "Pero el valor de enumeración indica RGB, no XYZ", observas inteligentemente. ¡Buen ojo! Tanto en datos de color como en datos de coordenadas, normalmente usas 3 o 4 valores de componentes, entonces, ¿por qué no usar el mismo formato para ambos?. La semántica HLSL, no el nombre del formato, indica la manera en que el sombreador trata los datos.

-   **COLOR**: semántica HLSL para datos de color. Como **POSICIÓN**, consiste en 3 valores de punto flotante de 32 bits (DirectX::XMFLOAT3). Cada valor contiene un componente de color: rojo (r), azul (b) o verde (g), expresados como un número flotante entre 0 y 1.

    Los valores de **COLOR**, por lo general, se devuelven como un valor RGBA de 4 componentes al final de la canalización de sombreador. En este ejemplo, establecerás el valor alfa "A" en 1.0 (opacidad máxima) en la canalización de sombreador para todos los píxeles.

Si quieres obtener la lista completa de formatos, consulta [**DXGI\_FORMAT**](https://msdn.microsoft.com/library/windows/desktop/bb173059). Si quieres obtener la lista completa de la semántica HLSL, consulta [Semántica](https://msdn.microsoft.com/library/windows/desktop/bb509647).

Llama a [**ID3D11Device::CreateInputLayout**](https://msdn.microsoft.com/library/windows/desktop/ff476512) y crea el diseño de entrada en el dispositivo Direct3D. Ahora necesitas crear un búfer que pueda realmente mantener los datos.

### Paso 3: Rellenar los búferes de vértices

Los búferes de vértices contienen la lista de vértices para cada triángulo de la malla. Cada vértice debe ser único en esta lista. En nuestro ejemplo, tienes 8 vértices para el cubo. El sombreador de vértices se ejecuta en el dispositivo de gráficos y lee el búfer de vértices. Interpreta los datos según el diseño de entrada que especificaste en el paso anterior.

En el siguiente ejemplo, proporcionas una descripción y un subrecurso para el búfer que le dan a Direct3D una serie de indicaciones sobre la asignación física de los datos de vértices y cómo tratarlos en la memoria del dispositivo de gráficos. Esto es necesario porque usas un [**ID3D11Buffer**](https://msdn.microsoft.com/library/windows/desktop/ff476351) genérico, que podría contener de todo. Las estructuras [**D3D11\_BUFFER\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476092) y [**D3D11\_SUBRESOURCE\_DATA**](https://msdn.microsoft.com/library/windows/desktop/ff476220) sirven para asegurar que Direct3D comprenda el diseño de la memoria física del búfer, incluido el tamaño de cada elemento del vértice, así como el tamaño máximo de la lista de vértices. Aquí también puedes controlar el acceso a la memoria del búfer y su recorrido, pero eso ya correspondería a otro tutorial.

Después de configurar el búfer, llama a [**ID3D11Device::CreateBuffer**](https://msdn.microsoft.com/library/windows/desktop/ff476501) para crearlo realmente. Obviamente, si tienes más de un objeto, crea búferes para cada uno de los modelos.

Declara y crea el búfer de vértices.

```cpp
D3D11_BUFFER_DESC vertexBufferDesc = {0};
vertexBufferDesc.ByteWidth = sizeof(SimpleCubeVertex) * ARRAYSIZE(cubeVertices);
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
m_d3dDevice->CreateBuffer(
                &vertexBufferDesc,
                &vertexBufferData,
                &vertexBuffer);
```

Vértices cargados. ¿Pero cuál es el orden de procesamiento de estos vértices? Esto se controla cuando proporcionas una lista de índices a los vértices (el orden de estos índices es el orden en que el sombreador de vértices los procesa).

### Paso 4: Rellenar los búferes de índices

Ahora proporciona una lista de índices para cada uno de los vértices. Estos índices corresponden a la posición del vértice en el búfer de vértices, empezando por 0. Para ayudarte a visualizar esto, considera que cada vértice de la malla tiene asignado un número único, como un Id. Este Id. es la posición de entero del vértice en el búfer de vértices.

![un cubo con ocho vértices numerados](images/cube-mesh-1.png)

En nuestro cubo de ejemplo, tienes 8 vértices que crean 6 cuadrantes para los lados. Divides los cuadrantes en triángulos, por un total de 12 triángulos que usan los 8 vértices. Con 3 vértices por triángulo, tienes 36 entradas en el búfer de índices. En nuestro ejemplo, este patrón de índices se conoce como una lista de triángulos y la indicas a Direct3D como **D3D11\_PRIMITIVE\_TOPOLOGY\_TRIANGLELIST**, cuando estableces la topología primitiva.

Esta es probablemente la forma más ineficiente de hacer una lista de índices, ya que hay muchas redundancias cuando los triángulos comparten puntos y lados. Por ejemplo, cuando un triángulo comparte un lado en forma de rombo, haces una lista de 6 índices para los cuatro vértices, como la siguiente:

![orden de índices cuando construyes un rombo](images/rhombus-surface-1.png)

-   Triángulo 1: \[0, 1, 2\]
-   Triángulo 2: \[0, 2, 3\]

En una topología de abanico o franja, ordenas los vértices de manera que se eliminan varios lados redundantes durante el recorrido (como el lado del índice 0 al índice 2 de la imagen). Para mallas grandes, esto reduce mucho la cantidad de veces que se ejecuta el sombreador de vértices y mejora significativamente el rendimiento. Pero ahora lo hacemos de manera simple y nos adherimos a la lista de triángulos.

Declara los índices para el búfer de vértices como una simple topología de lista de triángulos.

```cpp
unsigned short cubeIndices[] =
{   0, 1, 2,
    0, 2, 3,

    4, 5, 6,
    4, 6, 7,

    3, 2, 5,
    3, 5, 4,

    2, 1, 6,
    2, 6, 5,

    1, 7, 6,
    1, 0, 7,

    0, 3, 4,
    0, 4, 7 };
```

Treinta y seis elementos de índice en el búfer es un número muy redundante para tan solo 8 vértices. Si eliges eliminar algunas de las redundancias y usar otro tipo de lista de vértices, como una franja o abanico, debes especificar el tipo cuando proporcionas un valor [**D3D11\_PRIMITIVE\_TOPOLOGY**](https://msdn.microsoft.com/library/windows/desktop/ff476189) específico para el método [**ID3D11DeviceContext::IASetPrimitiveTopology**](https://msdn.microsoft.com/library/windows/desktop/ff476455).

Para obtener más información sobre las distintas técnicas de listas de índices, consulta [Topologías primitivas](https://msdn.microsoft.com/library/windows/desktop/bb205124).

### Paso 5: Crear un búfer de constantes para las matrices de transformación

Antes de empezar a procesar los vértices, necesitas proporcionar las matrices de transformación que se aplicarán (multiplicarán) para cada vértice durante su ejecución. Hay tres matrices que se usan en la mayoría de los juegos 3D:

-   La matriz 4x4 que transforma del sistema de coordenadas del objeto (modelo) al sistema de coordenadas globales.
-   La matriz 4x4 que transforma del sistema de coordenadas globales al sistema de coordenadas de la cámara (vista).
-   La matriz 4x4 que transforma del sistema de coordenadas de la cámara al sistema de coordenadas de proyección de vista 2D.

Estas matrices se pasan al sombreador en un *búfer de constantes*. Un búfer de constantes es una región de la memoria que permanece constante durante la ejecución de la siguiente etapa de la canalización de sombreador y a la que los sombreadores pueden acceder directamente desde el código HLSL. Defines cada búfer de constantes dos veces: primero en el código C++ del juego y (al menos) una vez en la sintaxis HLSL como C para el código de sombreador. Las dos declaraciones deben coincidir directamente en tipos y alineación de datos. No cuesta nada introducir errores difíciles de detectar cuando el sombreador usa la declaración HLSL para interpretar datos declarados en C++ y cuando los tipos no coinciden o la alineación de datos no se produce.

HLSL no cambia los búferes de constantes. Puedes cambiarlos cuando el juego actualiza datos específicos. A menudo, los dispositivos de juegos crean 4 clases de búferes de constantes: un tipo para actualizaciones por fotograma, un tipo para actualizaciones por modelo u objeto, un tipo para actualizaciones por actualización de estado del juego y un tipo para datos que nunca cambian mientras dura el juego.

En este ejemplo, lo único que no cambia son los datos DirectX::XMFLOAT4X4 para las tres matrices.

> **Nota**   El código de ejemplo que presentamos aquí usa matrices principales de columna. Puedes usar matrices principales de fila en lugar de usar la palabra clave **row\_major** en HLSL y asegurarte de que coincida con los datos de la matriz de origen. DirectXMath usa matrices principales de fila y puede usarse directamente con matrices HLSL definidas con la palabra clave **row\_major**.

 

Declara y crea un búfer de constantes para las tres matrices que usas para transformar cada vértice.

```cpp
struct ConstantBuffer
{
    DirectX::XMFLOAT4X4 model;
    DirectX::XMFLOAT4X4 view;
    DirectX::XMFLOAT4X4 projection;
};
ComPtr<ID3D11Buffer> m_constantBuffer;
ConstantBuffer m_constantBufferData;

// ...

// Create a constant buffer for passing model, view, and projection matrices
// to the vertex shader.  This allows us to rotate the cube and apply
// a perspective projection to it.

D3D11_BUFFER_DESC constantBufferDesc = {0};
constantBufferDesc.ByteWidth = sizeof(m_constantBufferData);
constantBufferDesc.Usage = D3D11_USAGE_DEFAULT;
constantBufferDesc.BindFlags = D3D11_BIND_CONSTANT_BUFFER;
constantBufferDesc.CPUAccessFlags = 0;
constantBufferDesc.MiscFlags = 0;
constantBufferDesc.StructureByteStride = 0;
m_d3dDevice->CreateBuffer(
                &constantBufferDesc,
                nullptr,
                &m_constantBuffer
             );

m_constantBufferData.model = DirectX::XMFLOAT4X4( // Identity matrix, since you are not animating the object
            1.0f, 0.0f, 0.0f, 0.0f,
            0.0f, 1.0f, 0.0f, 0.0f,
            0.0f, 0.0f, 1.0f, 0.0f,
            0.0f, 0.0f, 0.0f, 1.0f);

);
// Specify the view (camera) transform corresponding to a camera position of
// X = 0, Y = 1, Z = 2.  

m_constantBufferData.view = DirectX::XMFLOAT4X4(
            -1.00000000f, 0.00000000f,  0.00000000f,  0.00000000f,
             0.00000000f, 0.89442718f,  0.44721359f,  0.00000000f,
             0.00000000f, 0.44721359f, -0.89442718f, -2.23606800f,
             0.00000000f, 0.00000000f,  0.00000000f,  1.00000000f);
```

> **Nota**  Normalmente declaras la matriz de proyección cuando configuras recursos específicos del dispositivo, porque los resultados de la multiplicación deben coincidir con los parámetros de tamaño de la ventanilla 2D (que a menudo corresponde con el alto y ancho del píxel en pantalla). Si cambian, debes aumentar los valores de la coordenada x y la coordenada y, en consecuencia.

 

```cpp
// Finally, update the constant buffer perspective projection parameters
// to account for the size of the application window.  In this sample,
// the parameters are fixed to a 70-degree field of view, with a depth
// range of 0.01 to 100.  

float xScale = 1.42814801f;
float yScale = 1.42814801f;
if (backBufferDesc.Width > backBufferDesc.Height)
{
    xScale = yScale *
                static_cast<float>(backBufferDesc.Height) /
                static_cast<float>(backBufferDesc.Width);
}
else
{
    yScale = xScale *
                static_cast<float>(backBufferDesc.Width) /
                static_cast<float>(backBufferDesc.Height);
}
m_constantBufferData.projection = DirectX::XMFLOAT4X4(
            xScale, 0.0f,    0.0f,  0.0f,
            0.0f,   yScale,  0.0f,  0.0f,
            0.0f,   0.0f,   -1.0f, -0.01f,
            0.0f,   0.0f,   -1.0f,  0.0f
            );
```

Mientras estás aquí, establece los búferes de índices y vértices en [ID3D11DeviceContext](https://msdn.microsoft.com/library/windows/desktop/ff476149), además de la topología que usas.

```cpp
// Set the vertex and index buffers, and specify the way they define geometry.
UINT stride = sizeof(SimpleCubeVertex);
UINT offset = 0;
m_d3dDeviceContext->IASetVertexBuffers(
                0,
                1,
                vertexBuffer.GetAddressOf(),
                &stride,
                &offset);

m_d3dDeviceContext->IASetIndexBuffer(
                indexBuffer.Get(),
                DXGI_FORMAT_R16_UINT,
                0);

 m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
```

¡Listo! Ensamblado de entrada completo. Todo está en su lugar para la representación. Pongamos en funcionamiento el sombreador de vértices.

### Paso 6: Procesar la malla con el sombreador de vértices

Ahora que tienes un búfer de vértices, con los vértices que definen la malla, y el búfer de índices, que define el orden en que se procesarán los vértices, los envías al sombreador de vértices. El código del sombreador de vértices, expresado en lenguaje de sombreado de alto nivel compilado, se ejecuta una vez para cada vértice en el búfer de vértices. Esto te permite realizar transformaciones por vértice. El resultado final normalmente es una proyección 2D.

(¿Cargaste el sombreador de vértices? Si no lo has hecho, revisa [Cómo cargar recursos en tu juego DirectX](load-a-game-asset.md)).

Aquí creas el sombreador de vértices...

``` syntax
// Set the vertex and pixel shader stage state.
m_d3dDeviceContext->VSSetShader(
                vertexShader.Get(),
                nullptr,
                0);
```

...y estableces los búferes de constantes.

``` syntax
m_d3dDeviceContext->VSSetConstantBuffers(
                0,
                1,
                m_constantBuffer.GetAddressOf());
```

Este es el código de sombreador de vértices que controla la transformación de las coordenadas del objeto a las coordenadas globales y después al sistema de coordenadas de proyección de vista 2D. También aplicas algo de iluminación simple por vértice para que todo se vea más atractivo. Esto se incluye en el archivo HLSL del sombreador de vértices (SimplerVertexShader.hlsl, en este ejemplo).

``` syntax
cbuffer simpleConstantBuffer : register( b0 )
{
    matrix model;
    matrix view;
    matrix projection;
};

struct VertexShaderInput
{
    DirectX::XMFLOAT3 pos : POSITION;
    DirectX::XMFLOAT3 color : COLOR;
};

struct PixelShaderInput
{
    float4 pos : SV_POSITION;
    float4 color : COLOR;
};

PixelShaderInput SimpleVertexShader(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;
    float4 pos = float4(input.pos, 1.0f);

    // Transform the vertex position into projection space.
    pos = mul(pos, model);
    pos = mul(pos, view);
    pos = mul(pos, projection);
    vertexShaderOutput.pos = pos;

    // Pass the vertex color through to the pixel shader.
    vertexShaderOutput.color = float4(input.color, 1.0f);

    return vertexShaderOutput;
}
```

¿Ves **cbuffer** en la parte superior? Ese es el búfer HLSL análogo al mismo búfer de constantes que declaramos anteriormente en el código C++. ¿Y **VertexShaderInputstruct**? Se parece a la declaración de datos de vértices y diseño de entrada. Es importante que las declaraciones de datos de vértices y búferes de constantes del código C++ coincidan con las declaraciones del código HLSL (y eso incluye signos, tipos y alineación de datos).

**PixelShaderInput** especifica el diseño de los datos que devuelve la función principal del sombreador de vértices. Cuando terminas de procesar un vértice, devolverás una posición de vértices en el espacio de proyección 2D y un color para la iluminación por vértice. La tarjeta gráfica usa la salida de datos del sombreador para calcular los "fragmentos" (posibles píxeles) que deben colorearse cuando el sombreador de píxeles se ejecute en la siguiente etapa de la canalización.

### Paso 7: Pasar la malla por el sombreador de píxeles

Normalmente, en esta etapa de la canalización de gráficos, haces operaciones por píxel en las superficies visibles proyectadas de los objetos. (A la gente le gusta las texturas). Pero, para la muestra, simplemente pasas por esta etapa.

Primero creamos una instancia del sombreador de píxeles. El sombreador de píxeles se ejecuta para cada píxel en la proyección 2D de la escena, asignando un color a cada uno de esos píxeles. En este caso, pasamos directamente a través del color que el sombreador de vértices devolvió para el píxel.

Establece el sombreador de píxeles.

``` syntax
m_d3dDeviceContext->PSSetShader( pixelShader.Get(), nullptr, 0 );
```

Define un sombreador de paso de píxeles en HLSL.

``` syntax
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

Coloca este código en un archivo HLSL independiente del HLSL del sombreador de vértices (como SimplePixelShader.hlsl). Este código se ejecuta una vez para cada píxel visible en la ventanilla (una representación en memoria de la parte de la pantalla en que dibujas) que, en este caso, corresponde a toda la pantalla. Ahora la canalización de gráficos está completamente definida.

### Paso 8: Rasterizar y mostrar la malla

Ejecutemos la canalización. Esto es fácil: llama a [**ID3D11DeviceContext::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/bb173565).

¡Dibuja el cubo!

```cpp
// Draw the cube.
m_d3dDeviceContext->DrawIndexed( ARRAYSIZE(cubeIndices), 0, 0 );
            
```

En la tarjeta gráfica, cada vértice se procesa en el orden especificado en el búfer de índices. Después de que el código ejecute el sombreador de vértices y los fragmentos 2D se definan, se invoca al sombreador de píxeles y se colorean los triángulos.

Ahora coloca el cubo en la pantalla.

Presenta ese búfer de fotogramas en pantalla.

```cpp
// Present the rendered image to the window.  Because the maximum frame latency is set to 1,
// the render loop is generally  throttled to the screen refresh rate, typically around
// 60 Hz, by sleeping the app on Present until the screen is refreshed.

m_swapChain->Present(1, 0);
```

¡Y listo! Para una escena repleta de modelos, usa varios búferes de índices y vértices. Incluso podrías tener distintos sombreadores para distintos tipos de modelos. Recuerda que cada modelo tiene su propio sistema de coordenadas y necesitas transformarlos al sistema compartido de coordenadas globales, usando las matrices definidas en el búfer de constantes.

## Comentarios

En este tema se explica cómo crear y mostrar la geometría simple que tú mismo has elaborado. Para obtener más información sobre cómo cargar geometría compleja de un archivo y convertirla al formato del objeto de búfer de vértices (.vbo) específico de la muestra, consulta [Cómo cargar recursos en tu juego DirectX](load-a-game-asset.md).

> **Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados


* [Cómo cargar recursos en tu juego DirectX](load-a-game-asset.md)

 

 






<!--HONumber=Mar16_HO1-->


