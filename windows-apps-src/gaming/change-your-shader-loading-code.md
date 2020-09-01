---
title: Comparar la canalización de sombreador de OpenGL ES 2.0 con Direct3D
description: En términos conceptuales, la canalización de sombreador de Direct3D 11 es muy similar a la de OpenGL ES 2.0.
ms.assetid: 3678a264-e3f9-72d2-be91-f79cd6f7c4ca
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, OpenGL, Direct3D, canalización de sombreador
ms.localizationpriority: medium
ms.openlocfilehash: 3f7f512ce0eeb793f999ce133f8a2c32c35cd1cd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165379"
---
# <a name="compare-the-opengl-es-20-shader-pipeline-to-direct3d"></a>Comparar la canalización de sombreador de OpenGL ES 2.0 con Direct3D




**API importantes**

-   [Fase del ensamblador de entrada](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage)
-   [Fase del sombreador de vértices](/previous-versions/bb205146(v=vs.85))
-   [Fase del sombreador de píxeles](/previous-versions/bb205146(v=vs.85))

En términos conceptuales, la canalización de sombreador de Direct3D 11 es muy similar a la de OpenGL ES 2.0. En términos de diseño de API, sin embargo, los principales componentes para crear y administrar las fases de sombreador forman parte de dos interfaces importantes: [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1) y [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). En este tema intentamos asignar patrones comunes de API para la canalización de sombreador de OpenGL ES 2.0 a sus equivalentes en Direct3D 11 en estas interfaces.

## <a name="reviewing-the-direct3d-11-shader-pipeline"></a>Revisar la canalización de sombreador en Direct3D 11


Los objetos de sombreador se crean en la interfaz [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)con métodos, como [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) y [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader).

La canalización de gráficos en Direct3D 11 se administra mediante instancias de la interfaz [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) y tiene estas fases:

-   [Fase de ensamblado de entrada](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-input-assembler-stage). Esta fase suministra datos (triángulos, líneas y puntos) a la canalización. Los métodos[**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que admiten esta fase llevan el prefijo "IA".
-   [Fase del sombreador de vértices](/previous-versions/bb205146(v=vs.85)). Esta fase procesa vértices, normalmente realizando ciertas operaciones, como transformar, enmascarar e iluminar. Un sombreador de vértices siempre toma un solo vértice de entrada y produce un solo vértice de salida. Los métodos[**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que admiten esta fase llevan el prefijo "VS".
-   [Fase de salida en secuencias](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-stream-stage). Esta fase transmite datos primitivos de la canalización a la memoria para llegar al rasterizador. Los datos pueden transmitirse o pasarse al rasterizador. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse en la CPU. Los métodos [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que admiten esta fase llevan el prefijo "SO".
-   [Fase del rasterizador](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage). El rasterizador recorta primitivos, los prepara para el sombreador de píxeles y determina cómo invocar sombreadores de píxeles. Puede deshabilitar la rasterización indicando a la canalización que no hay ningún sombreador de píxeles (establezca la fase del sombreador de píxeles en NULL con [**ID3D11DeviceContext::P ssetshader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader)) y deshabilite la prueba de profundidad y estarcido (establezca DepthEnable y STENCILENABLE en false en [**D3D11 \_ Depth \_ estarcido \_ DESC**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_depth_stencil_desc)). Mientras estén deshabilitadas, los contadores de la canalización relacionados con la rasterización no se actualizarán.
-   [Fase del sombreador de píxeles](/previous-versions/bb205146(v=vs.85)): La fase del sombreador de píxeles recibe datos interpolados para un primitivo y genera datos por píxel, como el color. Los métodos[**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que admiten esta fase llevan el prefijo "PS".
-   [Fase de fusión de salida](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). Esta fase combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad y galería de símbolos para generar el resultado final de canalización. Los métodos [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) que admiten esta fase llevan el prefijo "OM.

(También hay fases para los sombreadores de geometría, sombreadores de casco, teseladores y sombreadores de dominio, pero, como estos no tienen análogos en OpenGL ES 2.0, no los trataremos en este tema). Para obtener una lista completa de los métodos para estas fases, consulta las páginas de referencia [**ID3D11DeviceContext**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) y [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). **ID3D11DeviceContext1** extiende **ID3D11DeviceContext** para Direct3D 11.

## <a name="creating-a-shader"></a>Crear un sombreador


En Direct3D, los recursos de sombreador no se crean antes de su compilación y carga, en cambio, un recurso se crea tras la carga de HLSL. Por lo tanto, no hay ninguna función directamente análoga a glCreateShader, que crea un recurso de sombreador inicializado de un tipo específico ( \_ como \_ sombreador de vértices de GL o \_ sombreador de fragmentos de contabilidad \_ ). En cambio, los sombreadores se crean después de haberse cargado HLSL con funciones específicas como [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) y [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader), que toman el tipo y el HLSL compilado como parámetros.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                                             |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCreateShader | Llama a [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) y [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) después de cargar correctamente el objeto de sombreador compilado, pasándoles el CSO como búfer. |

 

## <a name="compiling-a-shader"></a>Compilar un sombreador


Los sombreadores de Direct3D deben precompilarse como archivos de objetos de sombreador compilados (. CSO) en aplicaciones Plataforma universal de Windows (UWP) y cargarse mediante una de las API de archivo Windows Runtime. (Las aplicaciones de escritorio pueden compilar los sombreadores en cadenas o archivos de texto en tiempo de ejecución). Los archivos CSO se crean a partir de archivos .hlsl que forman parte del proyecto de Microsoft Visual Studio y conservan los mismos nombres, solo con una extensión de archivo .cso. Asegúrate de que estén incluidos en el paquete cuando hagas el traslado.

| OpenGL ES 2.0                          | Direct3D 11                                                                                                                                                                   |
|----------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glCompileShader                        | N/D Compila los sombreadores en archivos .cso en Visual Studio e inclúyelos en tu paquete.                                                                                     |
| Usar glGetShaderiv para el estado de compilación | N/D Consulta el resultado de la compilación en la herramienta compiladora FX de Visual Studio (FXC) si hay errores en la compilación. Si la compilación es correcta, se crea un archivo CSO correspondiente. |

 

## <a name="loading-a-shader"></a>Cargar un sombreador


Como se explicó en la sección de creación de un sombreador, Direct3D 11 crea el sombreador cuando el archivo CSO correspondiente se carga en un búfer y se pasa a uno de los métodos de la siguiente tabla.

| OpenGL ES 2.0 | Direct3D 11                                                                                                                                                                                                                           |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ShaderSource  | Llama a [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader) y [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) después de cargar correctamente el objeto de sombreador compilado. |

 

## <a name="setting-up-the-pipeline"></a>Configuración de la canalización


OpenGL ES 2.0 tiene el objeto del"programa sombreador" que contiene varios sombreadores para la ejecución. Los sombreadores individuales se adjuntan al objeto de programa sombreador. Si embargo, en Direct3D 11, trabajas directamente con el contexto de representación ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) y creas los sombreadores allí.

| OpenGL ES 2.0   | Direct3D 11                                                                                   |
|-----------------|-----------------------------------------------------------------------------------------------|
| glCreateProgram | N/D Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glLinkProgram   | N/D Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glUseProgram    | N/D Direct3D 11 no usa la abstracción de objeto de programa sombreador.                          |
| glGetProgramiv  | Usa la referencia que creaste para [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). |

 

Crea una instancia de [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1) y [**ID3D11Device1**](/windows/desktop/api/d3d11_2/nn-d3d11_2-id3d11device2) con el método [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) estático.

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


Establecer una ventanilla en Direct3D 11 es muy similar a como lo haces en OpenGL ES 2.0. En Direct3D 11, llame a [**ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) con una [** \_ ventanilla de CD3D11**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85))configurada.

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
| glViewport    | [**CD3D11 \_ Ventanilla**](/previous-versions/windows/desktop/legacy/jj151722(v=vs.85)), [ **ID3D11DeviceContext:: RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) |

 

## <a name="configuring-the-vertex-shaders"></a>Configurar los sombreadores de vértices


La configuración de un sombreador de vértices en Direct3D 11 se realiza cuando el sombreador está cargado. Los uniformes se pasan como búferes de constantes con [**ID3D11DeviceContext1::VSSetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vssetconstantbuffers1).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreateVertexShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader)                       |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::VSGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vsgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::VSGetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-vsgetconstantbuffers1). |

 

## <a name="configuring-the-pixel-shaders"></a>Configurar los sombreadores de píxeles


La configuración de un sombreador de píxeles en Direct3D 11 se realiza cuando el sombreador está cargado. Los uniformes se pasan como búferes de constantes con [**ID3D11DeviceContext1::PSSetConstantBuffers1.**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-pssetconstantbuffers1).

| OpenGL ES 2.0                    | Direct3D 11                                                                                               |
|----------------------------------|-----------------------------------------------------------------------------------------------------------|
| glAttachShader                   | [**ID3D11Device1::CreatePixelShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader)                         |
| glGetShaderiv, glGetShaderSource | [**ID3D11DeviceContext1::PSGetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-psgetshader)                       |
| glGetUniformfv, glGetUniformiv   | [**ID3D11DeviceContext1::PSGetConstantBuffers1**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-psgetconstantbuffers1). |

 

## <a name="generating-the-final-results"></a>Generar los resultados finales


Cuando la canalización se completa, dibujas los resultados de las fases de sombreador en el búfer de reserva. En Direct3D 11, al igual que en Open GL ES 2.0, esto implica llamar a un comando de dibujo para producir la salida de los resultados como mapa de colores en el búfer de reserva, y luego enviar ese búfer de reserva a la pantalla.

| OpenGL ES 2.0  | Direct3D 11                                                                                                                                                                                                                                         |
|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glDrawElements | [**Id3d11devicecontext1::D RAW**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw), [**Id3d11devicecontext1::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed) (u otros \* métodos de dibujo en [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext)). |
| eglSwapBuffers | [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1)                                                                                                                                                                              |

 

## <a name="porting-glsl-to-hlsl"></a>Migrar GLSL a HLSL


GLSL y HLSL no son muy distintos más allá de la compatibilidad con tipos complejos y la sintaxis general. Muchos desarrolladores consideran que la migración es más fácil estableciendo un alias de las instrucciones comunes de OpenGL ES 2.0 y las definiciones de sus equivalentes en HLSL. Ten en cuenta que Direct3D usa la versión del modelo de sombreador para expresar el conjunto de características del HLSL que una interfaz de gráficos admite. OpenGL tiene una especificación de versión distinta para HLSL. La siguiente tabla intenta darte una idea aproximada de los conjuntos de características del lenguaje de sombreador que se definen para Direct3D 11 y OpenGL ES 2.0 en términos de versión.

| Lenguaje de sombreador           | Versión de característica GLSL                                                                                                                                                                                                       | Modelo de sombreador Direct3D |
|---------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------|
| HLSL en Direct3D 11          | ~4.30.                                                                                                                                                                                                                    | SM 5.0                |
| GLSL ES para OpenGL ES 2.0 | 1.40. Las implementaciones anteriores de GLSL ES para OpenGL ES 2.0 pueden usar de 1.10 a 1.30. Compruebe el código original con glGetString (GL \_ shading \_ Language \_ version) o glGetString ( \_ versión de lenguaje de sombreado \_ ) para determinarlo. | ~SM 2.0               |

 

Para obtener más detalles sobre las diferencias entre los dos lenguajes de sombreador, así como las asignaciones comunes de sintaxis, lee la [referencia de GLSL a HLSL](glsl-to-hlsl-reference.md).

## <a name="porting-the-opengl-intrinsics-to-hlsl-semantics"></a>Migrar los intrínsecos de OpenGL a la semántica de HLSL


La semántica de HLSL de Direct3D 11consiste en cadenas que, como un uniforme o nombre de atributo, se usan para identificar un valor pasado entre la aplicación y un programa sombreador. Si bien hay una gran variedad de cadenas, el mejor procedimiento es usar una cadena como POSITION o COLOR que indique el uso. Asignas la semántica cuando construyes un búfer de constantes o un diseño de entrada de búfer. Puedes anexar un número entre 0 y 7 a la semántica para usar registros distintos para valores similares. Por ejemplo: COLOR0, COLOR1, COLOR2...

La semántica que tiene el prefijo "SV \_ " es la semántica del valor del sistema que escribe el programa del sombreador; la propia aplicación (que se ejecuta en la CPU) no puede modificarla. Normalmente, contiene los valores de entradas y salidas de otra fase del sombreador en la canalización de gráficos o se generan totalmente por la GPU).

Además, \_ la semántica de SV tiene comportamientos diferentes cuando se usan para especificar la entrada o salida de una fase de sombreador. Por ejemplo, \_ la posición de la VP (salida) contiene los datos de vértice transformados durante la fase del sombreador de vértices, y \_ la posición de la VP (entrada) contiene los valores de posición de píxeles interpolados durante la rasterización.

Estas son algunas asignaciones para intrínsecos de sombreador comunes de OpenGL ES 2.0:

| Valor del sistema de OpenGL | Usa esta semántica de HLSL                                                                                                                                                   |
|---------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| posición de contabilidad \_        | POSITION(n) para los datos del búfer de vértices. \_La posición de la VP proporciona una posición en píxeles al sombreador de píxeles y no se puede escribir en la aplicación.                                        |
| libro de contabilidad \_ normal          | NORMAL(n) para los datos normales proporcionados por el búfer de vértices.                                                                                                                 |
| GL \_ TexCoord \[ n\]   | TEXCOORD(n) para los datos de coordenadas de la textura UV (ST en algunos documentos de OpenGL) suministrados a un sombreador.                                                                       |
| GL \_ FragColor       | COLOR(n) para los datos RGBA suministrados a un sombreador. Ten en cuenta que el trato es idéntico para los datos de coordenadas, la semántica simplemente te ayuda a identificar que son datos de color. |
| GL \_ FragData \[ n\]   | \_Destino de la VP \[ n \] para escribir desde un sombreador de píxeles en una textura de destino u otro búfer de píxeles.                                                                               |

 

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

En este caso, el \_ destino de la VP es la ubicación del destino de representación en la que se escribe el color de píxel (definido como vector con cuatro valores float) cuando el sombreador finaliza la ejecución.

Para obtener más detalles sobre el uso de semántica con Direct3D, lee [Semántica de HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics).

 

 