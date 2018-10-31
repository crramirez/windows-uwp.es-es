---
author: mtoepke
title: Comparar la canalización de sombreador de OpenGL ES 2.0 con Direct3D
description: En términos conceptuales, la canalización de sombreador de Direct3D 11 es muy similar a la de OpenGL ES 2.0.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, OpenGL, Direct3D, canalización de sombreador
ms.localizationpriority: medium
ms.openlocfilehash: f8e3671b5d3490cf565db34ec891c203ee1f7c7a
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5867895"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Comparar la canalización de sombreador de OpenGL ES 2.0 con Direct3D




**API importantes**

-   [Fase del ensamblador de entrada](https://msdn.microsoft.com/library/windows/desktop/bb205116)
-   [Fase del sombreador de vértices](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage)
-   [Fase del sombreador de píxeles](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage)

En términos conceptuales, la canalización de sombreador de Direct3D 11 es muy similar a la de OpenGL ES 2.0. En términos de diseño de API, sin embargo, los principales componentes para crear y administrar las fases de sombreador forman parte de dos interfaces importantes: [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575) y [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). En este tema intentamos asignar patrones comunes de API para la canalización de sombreador de OpenGL ES 2.0 a sus equivalentes en Direct3D 11 en estas interfaces.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Revisar la canalización de sombreador en Direct3D 11


Los objetos de sombreador se crean en la interfaz [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/hh404575)con métodos, como [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) y [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513).

La canalización de gráficos en Direct3D 11 se administra mediante instancias de la interfaz [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) y tiene estas fases:

-   [Fase del ensamblador de entrada](https://msdn.microsoft.com/library/windows/desktop/bb205116) Esta fase suministra datos (triángulos, líneas y puntos) a la canalización. Los métodos [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) que admiten esta fase llevan el prefijo "IA".
-   [Fase del sombreador de vértices](https://msdn.microsoft.com/library/windows/desktop/bb205146#Vertex_Shader_Stage): Esta fase procesa vértices, normalmente realizando ciertas operaciones como transformar, enmascarar e iluminar. Un sombreador de vértices siempre toma un solo vértice de entrada y produce un solo vértice de salida. Los métodos [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) que admiten esta fase llevan el prefijo "VS".
-   [Fase de salida en secuencias](https://msdn.microsoft.com/library/windows/desktop/bb205121): Esta fase transmite datos primitivos de desde la canalización a la memoria para llegar al rasterizador. Los datos pueden transmitirse o pasarse al rasterizador. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse en la CPU. Los métodos [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) que admiten esta fase llevan el prefijo "SO".
-   [Fase del rasterizador](https://msdn.microsoft.com/library/windows/desktop/bb205125): El rasterizador recorta primitivos, los prepara para el sombreador de píxeles y determina cómo invocar sombreadores de píxeles. Si quieres deshabilitar la rasterización, indica a la canalización que no hay sombreador de píxeles (establece la fase del sombreador de píxeles en NULL con [**ID3D11DeviceContext::PSSetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476472)) y deshabilita la prueba de profundidad y la galería de símbolos (establece DepthEnable y StencilEnable en FALSE en [**D3D11\_DEPTH\_STENCIL\_DESC**](https://msdn.microsoft.com/library/windows/desktop/ff476110)). Mientras estén deshabilitadas, los contadores de la canalización relacionados con la rasterización no se actualizarán.
-   [Fase del sombreador de píxeles](https://msdn.microsoft.com/library/windows/desktop/bb205146#Pixel_Shader_Stage): La fase del sombreador de píxeles recibe datos interpolados para un primitivo y genera datos por píxel, como el color. Los métodos [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) que admiten esta fase llevan el prefijo "PS".
-   [Fase de fusión de salida](https://msdn.microsoft.com/library/windows/desktop/bb205120): Esta fase combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad y galería de símbolos para generar el resultado final de la canalización. Los métodos [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) que admiten esta fase llevan el prefijo "OM".

(También hay fases para los sombreadores de geometría, sombreadores de casco, teseladores y sombreadores de dominio, pero, como estos no tienen análogos en OpenGL ES 2.0, no los trataremos en este tema). Para obtener una lista completa de los métodos para estas fases, consulta las páginas de referencia [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) y [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). **ID3D11DeviceContext1** extiende **ID3D11DeviceContext** para Direct3D 11.

## <a name="creating-a-shader"></a>Crear un sombreador


En Direct3D, los recursos de sombreador no se crean antes de su compilación y carga, en cambio, un recurso se crea tras la carga de HLSL. Por lo tanto, no hay una función análoga directa para glCreateShader, que crea un recurso de sombreador inicializado de un tipo específico (como GL\_VERTEX\_SHADER o GL\_FRAGMENT\_SHADER). En cambio, los sombreadores se crean después de haberse cargado HLSL con funciones específicas como [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) y [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513), que toman el tipo y el HLSL compilado como parámetros.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Llama a [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) y [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) después de cargar correctamente el objeto de sombreador compilado, pasándoles el CSO como búfer. |

 

## <a name="compiling-a-shader"></a>Compilar un sombreador


Los sombreadores de Direct3D deben precompilarse como archivos de objeto de sombreador compilado (.cso) en aplicaciones para la Plataforma universal de Windows (UWP) y cargarse mediante una de las API de los archivos de Windows Runtime. (Las aplicaciones de escritorio pueden compilar los sombreadores en cadenas o archivos de texto en tiempo de ejecución). Los archivos CSO se crean a partir de archivos .hlsl que forman parte del proyecto de Microsoft Visual Studio y conservan los mismos nombres, solo con una extensión de archivo .cso. Asegúrate de que estén incluidos en el paquete cuando hagas el traslado.

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | N/C. Compila los sombreadores en archivos .cso en Visual Studio e inclúyelos en tu paquete.                                                                                     |
| Usar glGetShaderiv para el estado de compilación | N/C. Consulta el resultado de la compilación en la herramienta compiladora FX de Visual Studio (FXC) si hay errores en la compilación. Si la compilación es correcta, se crea un archivo CSO correspondiente. |

 

## <a name="loading-a-shader"></a>Cargar un sombreador


Como se explicó en la sección de creación de un sombreador, Direct3D 11 crea el sombreador cuando el archivo CSO correspondiente se carga en un búfer y se pasa a uno de los métodos de la siguiente tabla.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Llama a [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524) y [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513) después de cargar correctamente el objeto de sombreador compilado. |

 

## <a name="setting-up-the-pipeline"></a>Configurar la canalización


OpenGL ES 2.0 tiene el objeto del"programa sombreador" que contiene varios sombreadores para la ejecución. Los sombreadores individuales se adjuntan al objeto de programa sombreador. Si embargo, en Direct3D 11, trabajas directamente con el contexto de representación ([**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598)) y creas los sombreadores allí.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | N/A. Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glLinkProgram   | N/A. Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glUseProgram    | N/A. Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glGetProgramiv  | Usa la referencia que creaste para [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598). |

 

Crea una instancia de [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/hh404598) y [**ID3D11Device1**](https://msdn.microsoft.com/library/windows/desktop/dn280493) con el método [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) estático.

``` syntax
Microsoft::WRL::ComPtr<ID3D11Device1>          m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>  m_d3dContext;

// ...

D3D11CreateDevice(
  nullptr, // Specify nullptr to use the default adapter.
  D3D_DRIVER_TYPE_HARDWARE,
  nullptr,
  creationFlags, // Set set debug and Direct2D compatibility flags.
  featureLevels, // List of feature levels this app can support.
  ARRAYSIZE(featureLevels),
  D3D11_SDK_VERSION, // Always set this to D3D11_SDK_VERSION for UWP apps.
  &device, // Returns the Direct3D device created.
  &m_featureLevel, // Returns feature level of device created.
  &m_d3dContext // Returns the device's immediate context.
);
```

## <a name="setting-the-viewports"></a>Establecer las ventanillas


Establecer una ventanilla en Direct3D 11 es muy similar a como lo haces en OpenGL ES 2.0. En Direct3D 11, llama a [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) con un [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722) configurado.

Direct3D 11: Establecer una ventanilla.

``` syntax
CD3D11_VIEWPORT viewport(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );
m_d3dContext->RSSetViewports(1, &viewport);
```

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                  |
|---------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| glViewport    | [**CD3D11\_VIEWPORT**](https://msdn.microsoft.com/library/windows/desktop/jj151722), [**ID3D11DeviceContext::RSSetViewports**](https://msdn.microsoft.com/library/windows/desktop/ff476480) |

 

## <a name="configuring-the-vertex-shaders"></a>Configurar los sombreadores de vértices


La configuración de un sombreador de vértices en Direct3D 11 se realiza cuando el sombreador está cargado. Los uniformes se pasan como búferes de constantes con [**ID3D11DeviceContext1::VSSetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446795).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](https://msdn.microsoft.com/library/windows/desktop/ff476524)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476489)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh446793). |

 

## <a name="configuring-the-pixel-shaders"></a>Configurar los sombreadores de píxeles


La configuración de un sombreador de píxeles en Direct3D 11 se realiza cuando el sombreador está cargado. Los uniformes se pasan como búferes de constantes con [**ID3D11DeviceContext1::PSSetConstantBuffers1.**](https://msdn.microsoft.com/library/windows/desktop/hh404649).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](https://msdn.microsoft.com/library/windows/desktop/ff476513)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](https://msdn.microsoft.com/library/windows/desktop/ff476468)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](https://msdn.microsoft.com/library/windows/desktop/hh404645). |

 

## <a name="generating-the-final-results"></a>Generar los resultados finales


Cuando la canalización se completa, dibujas los resultados de las fases de sombreador en el búfer de reserva. En Direct3D 11, al igual que en Open GL ES 2.0, esto implica llamar a un comando de dibujo para producir la salida de los resultados como mapa de colores en el búfer de reserva, y luego enviar ese búfer de reserva a la pantalla.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**ID3D11DeviceContext1::Draw**](https://msdn.microsoft.com/library/windows/desktop/ff476407), [**ID3D11DeviceContext1::DrawIndexed**](https://msdn.microsoft.com/library/windows/desktop/ff476409) (or other Draw\* methods on [**ID3D11DeviceContext1**](https://msdn.microsoft.com/library/windows/desktop/ff476385)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](https://msdn.microsoft.com/library/windows/desktop/hh446797)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Migrar GLSL a HLSL


GLSL y HLSL no son muy distintos más allá de la compatibilidad con tipos complejos y la sintaxis general. Muchos desarrolladores consideran que la migración es más fácil estableciendo un alias de las instrucciones comunes de OpenGL ES 2.0 y las definiciones de sus equivalentes en HLSL. Ten en cuenta que Direct3D usa la versión del modelo de sombreador para expresar el conjunto de características del HLSL que una interfaz de gráficos admite. OpenGL tiene una especificación de versión distinta para HLSL. La siguiente tabla intenta darte una idea aproximada de los conjuntos de características del lenguaje de sombreador que se definen para Direct3D 11 y OpenGL ES 2.0 en términos de versión.

| Lenguaje de sombreador           | Versión de característica GLSL                                                                                                                                                                                                       | Modelo de sombreador Direct3D |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| HLSL en Direct3D 11          | ~4.30.                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES para OpenGL ES 2.0 | 1.40. Las implementaciones anteriores de GLSL ES para OpenGL ES 2.0 pueden usar de 1.10 a 1.30. Comprueba el código original con glGetString(GL\_SHADING\_LANGUAGE\_VERSION) o glGetString(SHADING\_LANGUAGE\_VERSION) para determinarlo. | ~SM 2.0               |

 

Para obtener más detalles sobre las diferencias entre los dos lenguajes de sombreador, así como las asignaciones comunes de sintaxis, lee la [referencia de GLSL a HLSL](glsl-to-hlsl-reference.md).

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Migrar los intrínsecos de OpenGL a la semántica de HLSL


La semántica de HLSL de Direct3D 11consiste en cadenas que, como un uniforme o nombre de atributo, se usan para identificar un valor pasado entre la aplicación y un programa sombreador. Si bien hay una gran variedad de cadenas, el mejor procedimiento es usar una cadena como POSITION o COLOR que indique el uso. Asignas la semántica cuando construyes un búfer de constantes o un diseño de entrada de búfer. Puedes anexar un número entre 0 y 7 a la semántica para usar registros distintos para valores similares. Por ejemplo: COLOR0, COLOR1, COLOR2...

La semántica que tiene el prefijo "SV\_" es la semántica de valor del sistema que el programa sombreador escribe. La aplicación (que se ejecuta en la CPU) no puede modificarla. Normalmente, contiene los valores de entradas y salidas de otra fase del sombreador en la canalización de gráficos o se generan totalmente por la GPU).

Ademas, la semántica con el prefijo SV\_ tiene distintos comportamientos cuando se usa para especificar la entrada o la salida correspondientes a una fase del sombreador. Por ejemplo, SV\_POSITION (salida) tiene los datos de vértice transformados durante la fase del sombreador de vértices y SV\_POSITION (entrada) tiene los valores de posición de píxel interpolados durante la rasterización.

Estas son algunas asignaciones para intrínsecos de sombreador comunes de OpenGL ES 2.0:

| Valor del sistema de OpenGL | Usa esta semántica de HLSL                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| gl\_Position        | POSITION(n) para los datos del búfer de vértices. SV\_POSITION proporciona una posición de píxel al sombreador de píxeles que la aplicación no puede escribir.                                        |
| gl\_Normal          | NORMAL(n) para los datos normales proporcionados por el búfer de vértices.                                                                                                                 |
| gl\_TexCoord\[n\]   | TEXCOORD(n) para los datos de coordenadas de la textura UV (ST en algunos documentos de OpenGL) suministrados a un sombreador.                                                                       |
| gl\_FragColor       | COLOR(n) para los datos RGBA suministrados a un sombreador. Ten en cuenta que el trato es idéntico para los datos de coordenadas, la semántica simplemente te ayuda a identificar que son datos de color. |
| gl\_FragData\[n\]   | SV\_Target\[n\] para escribir de un sombreador de píxeles a una textura de destino u otro búfer de píxeles.                                                                               |

 

Es el método por el cual el código para la semántica no equivale a usar intrínsecos en OpenGL ES 2.0. En OpenGL puedes acceder a varios de los intrínsecos directamente sin ninguna configuración o declaración; en Direct3D debes declarar un campo en un búfer de constantes específico para usar una semántica determinada o declararlo como el valor devuelto para un método **main()** del sombreador.

Este es un ejemplo de semántica usada en una definición de búfer de constantes:

```cpp
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR0;
};

// The position is interpolated to the pixel value by the system. The per-vertex color data is also interpolated and passed through the pixel shader. 
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR0;
};
```

Este código define un par de búferes de constantes simples.

Y este es un ejemplo de semántica usada para definir el valor devuelto por un sombreador de fragmentos:

```cpp
// A pass-through for the (interpolated) color data.
float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color,1.0f);
}
```

En este caso, SV\_TARGET es la ubicación del destino de representación que el color de píxel (definido como un vector con cuatro valores flotantes) se escribe cuando el sombreador finaliza la ejecución.

Para obtener más detalles sobre el uso de semántica con Direct3D, lee [Semántica de HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509647).

 

 




