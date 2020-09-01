---
title: Migrar GLSL
description: 'Una vez que traslades el código que crea y configura los búferes y objetos de sombreador, tienes que portar el código a esos sombreadores: del lenguaje GL Shader Language (GLSL) de OpenGL ES 2.0 al lenguaje High-level Shader Language (HLSL) de Direct3D 11.'
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, GLSL, Puerto
ms.localizationpriority: medium
ms.openlocfilehash: b9f3d3a8bc6bfe9205ccba9d743409519bf8a572
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173129"
---
# <a name="port-the-glsl"></a>Migrar GLSL




**API importantes**

-   [Semántica de HLSL](/windows/desktop/direct3dhlsl/dcl-usage---ps)
-   [Constantes de sombreador (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants)

Una vez que traslades el código que crea y configura los búferes y objetos de sombreador, tienes que portar el código a esos sombreadores: del lenguaje GL Shader Language (GLSL) de OpenGL ES 2.0 al lenguaje High-level Shader Language (HLSL) de Direct3D 11.

En OpenGL ES 2,0, los sombreadores devuelven datos después de la ejecución mediante intrínsecos como **GL \_ Position**, **GL \_ FragColor**o **GL \_ FragData \[ n \] ** (donde n es el índice de un destino de representación específico). En Direct3D, no hay elementos intrínsecos específicos y los sombreadores devuelven datos como el tipo devuelto de sus respectivas funciones main().

Los datos que quieres interpolar entre las fases de sombreador, como la posición de vértice o la normal, se controlan mediante la declaración **varying**. Sin embargo, Direct3D no tiene esta declaración. Cualquier dato que quieras pasar entre las fases de sombreador debe marcarse con una [semántica de HLSL](/windows/desktop/direct3dhlsl/dcl-usage---ps). La semántica que se elige indica el propósito de los datos. Por ejemplo, declararías datos de vértice que quieres interpolados en el sombreador de fragmentos, de esta manera:

`float4 vertPos : POSITION;`

o

`float4 vertColor : COLOR;`

Donde POSITION es la semántica usada para indicar los datos de posición de vértice. POSITION también es un caso especial, dado que después de la interpolación, el sombreador de píxeles no puede acceder a esta semántica. Por lo tanto, debe especificar la entrada para el sombreador de píxeles con la \_ posición VP y los datos de vértice interpolados se colocarán en esa variable.

`float4 position : SV_POSITION;`

La semántica puede declararse en los métodos principales (main) de los sombreadores. Para los sombreadores de píxeles \_ , \[ el destino de SV n \] , que indica un destino de representación, es necesario en el método de cuerpo. (SV \_ El destino sin un sufijo numérico tiene como valor predeterminado el índice de destino 0).

Tenga en cuenta también que se requieren sombreadores de vértices para generar la \_ semántica del valor del sistema de posición de la VP. Esta semántica resuelve los datos de posición de vértice para coordinar valores donde: el valor de X está entre -1 y 1; el de Y está entre -1 y 1; el de Z se divide por el valor W de la coordenada homogénea original (Z/W) y el de W es 1 dividido por el valor W original (1/W). Los sombreadores de píxeles usan la \_ semántica del valor del sistema de posición VP para recuperar la ubicación de píxeles en la pantalla, donde x está entre 0 y el ancho del destino de representación e y está entre 0 y el alto del destino de representación (cada desplazamiento por 0,5). \_Los sombreadores de píxeles de nivel de característica 9 x no pueden leer desde el valor de posición de la VP \_ .

Los búferes de constantes deben declararse mediante **cbuffer** y asociarse con un registro de inicio específico para la búsqueda.

Direct3D 11: una declaración de búfer de constantes en HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

En este ejemplo, el búfer de constantes usa el registro b0 para mantener el búfer empaquetado. En el formulario b se hace referencia a todos los registros \# . Para obtener más información sobre la implementación de búferes de constantes, registros y empaquetado de datos en HLSL, lee [Shader Constants (HLSL) (Constantes de sombreador [HLSL])](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-constants).

<a name="instructions"></a>Instructions
------------
### <a name="step-1-port-the-vertex-shader"></a>Paso 1: portar el sombreador de vértices

En nuestro ejemplo simple de OpenGL ES 2.0, el sombreador de vértices tiene tres entradas: una matriz 4x4 de proyección de vista de modelo constante y dos vectores de 4 coordenadas. Estos dos vectores contienen la posición de vértice y su color. El sombreador transforma el vector de posición en coordenadas de perspectiva y lo asigna al \_ intrínseco de posición de libro de contabilidad para la rasterización. Asimismo, el color de vértice se copia en una variable de la interpolación durante la rasterización.

OpenGL ES 2.0: sombreador de vértices para el objeto de cubo (GLSL)

``` syntax
uniform mat4 u_mvpMatrix; 
attribute vec4 a_position;
attribute vec4 a_color;
varying vec4 destColor;

void main()
{           
  gl_Position = u_mvpMatrix * a_position;
  destColor = a_color;
}
```

Ahora en Direct3D, la matriz de proyección de vista de modelo constante está en un búfer de constantes empaquetado en el registro b0, y la posición y el color de vértice están específicamente marcados con la semántica de HSLS apropiada para cada uno: POSITION y COLOR. Dado que nuestro diseño de entrada indica una disposición específica para estos dos valores de vértice, creas un struct para conservarlos y los declaras como el tipo de parámetro de entrada en la función principal (main) del sombreador. (También podrías especificarlos como dos parámetros individuales, pero sería engorroso). Además, debes especificar un tipo de salida para esta fase, que contiene la posición y el color interpolados, y declararlo como el valor devuelto de la función principal del sombreador de vértices.

Direct3D 11: sombreador de vértices para el objeto de cubo (HLSL)

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};

// Per-vertex data used as input to the vertex shader.
struct VertexShaderInput
{
  float3 pos : POSITION;
  float3 color : COLOR;
};

// Per-vertex color data passed through the pixel shader.
struct PixelShaderInput
{
  float3 pos : SV_POSITION;
  float3 color : COLOR;
};

PixelShaderInput main(VertexShaderInput input)
{
  PixelShaderInput output;
  float4 pos = float4(input.pos, 1.0f); // add the w-coordinate

  pos = mul(mvp, projection);
  output.pos = pos;

  output.color = input.color;

  return output;
}
```

El tipo de datos de salida, PixelShaderInput, se rellena durante la rasterización y se proporciona al sombreador de fragmentos (píxeles).

### <a name="step-2-port-the-fragment-shader"></a>Paso 2: portar el sombreador de fragmentos

Nuestro sombreador de fragmentos de ejemplo en GLSL es muy simple: proporcione el \_ valor intrínseco de GL FragColor con el valor de color interpolado. OpenGL ES 2.0 lo escribirá en el destino de representación predeterminado.

OpenGL ES 2.0: sombreador de fragmentos para el objeto de cubo (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D es casi igual de simple. La única diferencia significativa es que la función principal del sombreador de píxeles debe devolver un valor. Puesto que el color es un valor de punto flotante de 4 coordenadas (RGBA), se indica FLOAT4 como el tipo de valor devuelto y, a continuación, se especifica el destino de representación predeterminado como la \_ semántica del valor del sistema de destino VP.

Direct3D 11: sombreador de píxeles para el objeto de cubo (HLSL)

``` syntax
struct PixelShaderInput
{
  float4 pos : SV_POSITION;
  float3 color : COLOR;
};


float4 main(PixelShaderInput input) : SV_TARGET
{
  return float4(input.color, 1.0f);
}
```

El color para el píxel en posición se escribe en el destino de representación. Ahora veamos cómo mostrar el contenido de ese destino de representación en [Dibujar en la pantalla](draw-to-the-screen.md).

## <a name="previous-step"></a>Paso anterior


[Portar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md) Paso siguiente
---------
[Dibujar en la pantalla](draw-to-the-screen.md) Observaciones
-------
Comprender la semántica de HLSL y el empaquetado de los búferes de constantes puede ahorrarte un buen dolor de cabeza por culpa de la depuración, además de ofrecerte oportunidades de optimización. Si tienes la opción de hacerlo, lee [Sintaxis de variables (HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-variable-syntax), [Introducción a los búferes en Direct3D 11](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro) y [Cómo: crear un búfer de constantes](/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-constant-how-to). De lo contrario, estas son algunas sugerencias iniciales para que tengas en cuenta sobre semántica y búferes de constantes.

-   Siempre comprueba dos veces el código de configuración Direct3D del representador para asegurarte de que las estructuras de tus búferes de constantes coincidan con las declaraciones de struct cbuffer en HLSL, y de que los tipos escalares de componentes coincidan en ambas declaraciones.
-   En el código C++ del representador, usa tipos de [DirectXMath](/windows/desktop/dxmath/directxmath-portal) en tus declaraciones de búferes de constantes para garantizar el empaquetado de datos apropiado.
-   La mejor forma de usar búferes de constantes con eficiencia es organizar las variables de sombreador en búferes de constantes según su frecuencia de actualización. Por ejemplo, si tienes algunos datos de uniforme que se actualizan una vez por trama, y otros datos de uniforme que se actualizan solo cuando la cámara se mueve, considera separar esos datos en búferes de constantes independientes.
-   La semántica que olvidaste aplicar o que aplicaste de manera incorrecta será tu primera fuente de error de compilación de sombreador (FXC). ¡Revisa dos veces! Los documentos pueden ser algo confusos, ya que varias páginas y muestras anteriores se refieren a distintas versiones de la semántica de HLSL anteriores a Direct3D 11.
-   Asegúrate de saber qué nivel de características de Direct3D eliges como destino para cada sombreador. La semántica del nivel de característica 9 \_ \* es diferente de la de 11 \_ 1.
-   La semántica de la posición de la VP \_ resuelve los datos asociados de la posición posterior a la interpolación en los valores de coordenadas donde x está entre 0 y el ancho del destino de representación, y se encuentra entre 0 y el alto del destino de representación, z se divide por el valor de la coordenada homogénea original (z/e).

## <a name="related-topics"></a>Temas relacionados


[Cómo: trasladar un representador simple de OpenGL ES 2,0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Migrar objetos de sombreador](port-the-shader-config.md)

[Migrar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)

[Dibujar en la pantalla](draw-to-the-screen.md)

 

 