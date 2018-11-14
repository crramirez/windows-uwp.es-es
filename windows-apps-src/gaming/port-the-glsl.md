---
author: mtoepke
title: Migrar GLSL
description: Una vez que traslades el código que crea y configura los búferes y objetos de sombreador, tienes que portar el código de esos sombreadores, del lenguaje GL Shader Language (GLSL) de OpenGL ES 2.0 al lenguaje High-level Shader Language (HLSL) de Direct3D 11.
ms.assetid: 0de06c51-8a34-dc68-6768-ea9f75dc57ee
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, games, juegos, glsl, port, portar
ms.localizationpriority: medium
ms.openlocfilehash: 47fa601a7e0ff307108713a0a6fcd7a5468b0468
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6465844"
---
# <a name="port-the-glsl"></a>Portar GLSL




**API importantes**

-   [Semántica de HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574)
-   [Constantes de sombreador (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509581)

Una vez que traslades el código que crea y configura los búferes y objetos de sombreador, tienes que portar el código de esos sombreadores, del lenguaje GL Shader Language (GLSL) de OpenGL ES 2.0 al lenguaje High-level Shader Language (HLSL) de Direct3D 11.

En OpenGL ES 2.0, los sombreadores devuelven datos después de la ejecución usando elementos intrínsecos como **gl\_Position**, **gl\_FragColor** o **gl\_FragData\[n\]** (donde n corresponde al índice de un destino de representación específico). En Direct3D, no hay elementos intrínsecos específicos y los sombreadores devuelven datos como el tipo devuelto de sus respectivas funciones main().

Los datos que quieres interpolar entre las fases de sombreador, como la posición de vértice o la normal, se controlan mediante la declaración **varying**. Sin embargo, Direct3D no tiene esta declaración. Cualquier dato que quieras pasar entre las fases de sombreador debe marcarse con una [semántica de HLSL](https://msdn.microsoft.com/library/windows/desktop/bb205574). La semántica que se elige indica el propósito de los datos. Por ejemplo, declararías datos de vértice que quieres interpolados en el sombreador de fragmentos, de esta manera:

`float4 vertPos : POSITION;`

o

`float4 vertColor : COLOR;`

Donde POSITION es la semántica usada para indicar los datos de posición de vértice. POSITION también es un caso especial, dado que después de la interpolación, el sombreador de píxeles no puede acceder a esta semántica. Por lo tanto, debes especificar la entrada al sombreador de píxeles mediante SV\_POSITION y los datos de vértice interpolados se colocarán en esa variable.

`float4 position : SV_POSITION;`

La semántica puede declararse en los métodos principales (main) de los sombreadores. En cuanto a los sombreadores de píxeles, SV\_TARGET\[\n], que indica un destino de representación, debe estar en el método principal. (Un elemento SV\_TARGET sin un sufijo numérico representa de forma predeterminada al índice 0 del destino).

Observa también que los sombreadores de vértices son necesarios para producir la salida de la semántica del valor del sistema SV\_POSITION. Esta semántica resuelve los datos de posición de vértice para coordinar valores donde: el valor de X está entre -1 y 1; el de Y está entre -1 y 1; el de Z se divide por el valor W de la coordenada homogénea original (Z/W) y el de W es 1 dividido por el valor W original (1/W). Los sombreadores de píxeles usan la semántica del valor del sistema SV\_POSITION para recuperar la ubicación de píxeles en la pantalla, donde X está entre 0 y el ancho del destino de representación, e Y está entre 0 y el alto del destino de representación (con un desplazamiento de 0,5 cada uno). Los sombreadores de píxeles del nivel de característica 9\_x no se pueden leer en el valor SV\_POSITION.

Los búferes de constantes deben declararse mediante **cbuffer** y asociarse con un registro de inicio específico para la búsqueda.

Direct3D 11: una declaración de búfer de constantes en HLSL

``` syntax
cbuffer ModelViewProjectionConstantBuffer : register(b0)
{
  matrix mvp;
};
```

En este ejemplo, el búfer de constantes usa el registro b0 para mantener el búfer empaquetado. Para hacer referencia a todos los registros, se usa b\#. Para obtener más información sobre la implementación de búferes de constantes, registros y empaquetado de datos en HLSL, lee [Shader Constants (HLSL) (Constantes de sombreador [HLSL])](https://msdn.microsoft.com/library/windows/desktop/bb509581).

<a name="instructions"></a>Instrucciones
------------

### <a name="step-1-port-the-vertex-shader"></a>Paso 1: portar el sombreador de vértices

En nuestro ejemplo simple de OpenGL ES 2.0, el sombreador de vértices tiene tres entradas: una matriz 4x4 de proyección de vista de modelo constante y dos vectores de 4 coordenadas. Estos dos vectores contienen la posición de vértice y su color. El sombreador transforma el vector de posición en coordenadas de perspectiva y lo asigna al elemento intrínseco gl\_Position para su rasterización. Asimismo, el color de vértice se copia en una variable de la interpolación durante la rasterización.

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

El sombreador de fragmentos de nuestro ejemplo en GLSL es extremadamente simple: proporciona el elemento intrínseco gl\_FragColor con el valor del color interpolado. OpenGL ES 2.0 lo escribirá en el destino de representación predeterminado.

OpenGL ES 2.0: sombreador de fragmentos para el objeto de cubo (GLSL)

``` syntax
varying vec4 destColor;

void main()
{
  gl_FragColor = destColor;
} 
```

Direct3D es casi igual de simple. La única diferencia significativa es que la función principal del sombreador de píxeles debe devolver un valor. Dado que el color es un valor flotante de 4 coordenadas (RGBA), debes indicar float4 como el tipo devuelto y, a continuación, especificar el destino de representación predeterminado como la semántica del valor del sistema SV\_TARGET.

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

Comprender la semántica de HLSL y el empaquetado de los búferes de constantes puede ahorrarte un buen dolor de cabeza por culpa de la depuración, además de ofrecerte oportunidades de optimización. Si tienes la opción de hacerlo, lee [Sintaxis de variables (HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509706), [Introducción a los búferes en Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476898) y [Cómo: crear un búfer de constantes](https://msdn.microsoft.com/library/windows/desktop/ff476896). De lo contrario, estas son algunas sugerencias iniciales para que tengas en cuenta sobre semántica y búferes de constantes.

-   Siempre comprueba dos veces el código de configuración Direct3D del representador para asegurarte de que las estructuras de tus búferes de constantes coincidan con las declaraciones de struct cbuffer en HLSL, y de que los tipos escalares de componentes coincidan en ambas declaraciones.
-   En el código C++ del representador, usa tipos de [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833) en tus declaraciones de búferes de constantes para garantizar el empaquetado de datos apropiado.
-   La mejor forma de usar búferes de constantes con eficiencia es organizar las variables de sombreador en búferes de constantes según su frecuencia de actualización. Por ejemplo, si tienes algunos datos de uniforme que se actualizan una vez por trama, y otros datos de uniforme que se actualizan solo cuando la cámara se mueve, considera separar esos datos en búferes de constantes independientes.
-   La semántica que olvidaste aplicar o que aplicaste de manera incorrecta será tu primera fuente de error de compilación de sombreador (FXC). ¡Revisa dos veces! Los documentos pueden ser algo confusos, ya que varias páginas y muestras anteriores se refieren a distintas versiones de la semántica de HLSL anteriores a Direct3D 11.
-   Asegúrate de saber qué nivel de características de Direct3D eliges como destino para cada sombreador. La semántica del nivel de característica 9\_\* difiere aquella que se encuentra en el nivel 11\_1.
-   La semántica SV\_POSITION resuelve los datos de posición asociados después de la interpolación, para coordinar valores donde: X está entre 0 y el ancho del destino de representación; Y está entre 0 y el alto del destino de representación; Z se divide por el valor W de la coordenada homogénea original (Z/W) y W es 1 dividido por el valor W original (1/W).

## <a name="related-topics"></a>Temas relacionados


[Portar un representador simple de OpenGL ES 2.0 a Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)

[Portar objetos de sombreador](port-the-shader-config.md)

[Migrar datos y búferes de vértices](port-the-vertex-buffers-and-data-config.md)

[Dibujar en la pantalla](draw-to-the-screen.md)

 

 




