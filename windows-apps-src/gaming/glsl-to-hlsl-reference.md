---
author: mtoepke
title: Referencia de GLSL a HLSL
description: "El código de lenguaje de sombreador de OpenGL (GLSL) se migra a código de lenguaje de sombreado de alto nivel de Microsoft (HLSL) cuando se migra la arquitectura de gráficos de OpenGL ES 2.0 a Direct3D11 para crear un juego para la Plataforma universal de Windows (UWP)."
ms.assetid: 979d19f6-ef0c-64e4-89c2-a31e1c7b7692
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, glsl, hlsl, opengl, directx, sombreados, shaders
ms.openlocfilehash: f2d5f5a363abf026e865ed07221ba9075a6a67e7
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: es-ES
---
# <a name="glsl-to-hlsl-reference"></a>Referencia de GLSL a HLSL


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

El código de lenguaje de sombreador de OpenGL (GLSL) se migra a código de lenguaje de sombreado de alto nivel de Microsoft (HLSL) cuando [se migra la arquitectura de gráficos de OpenGL ES 2.0 a Direct3D11](port-from-opengl-es-2-0-to-directx-11-1.md) para crear un juego para la Plataforma universal de Windows (UWP). El GLSL al que se hace referencia aquí es compatible con OpenGL ES 2.0. El HLSL es compatible con Direct3D11. Para obtener información acerca de las diferencias entre Direct3D 11 y las versiones anteriores de Direct3D, consulta el tema sobre la [asignación de características](feature-mapping.md).

-   [Comparación entre OpenGL ES 2.0 y Direct3D 11](#comparing-opengl-es-20-with-direct3d-11)
-   [Migrar variables de GLSL a HLSL](#porting-glsl-variables-to-hlsl)
-   [Migrar tipos de GLSL a HLSL](#porting-glsl-types-to-hlsl)
-   [Migración de variables globales predefinidas de GLSL a HLSL](#porting-glsl-pre-defined-global-variables-to-hlsl)
-   [Ejemplos de cómo migrar variables de GLSL a HLSL](#examples-of-porting-glsl-variables-to-hlsl)
    -   [Elementos uniform, attribute y varying en GLSL](#uniform-attribute-and-varying-in-glsl)
    -   [Búferes de constantes y transferencias de datos en HLSL](#constant-buffers-and-data-transfers-in-hlsl)
-   [Ejemplos de cómo migrar código de representación de OpenGL a Direct3D](#examples-of-porting-opengl-rendering-code-to-direct3d)
-   [Temas relacionados](#related-topics)

## <a name="comparing-opengl-es-20-with-direct3d-11"></a>Comparación entre OpenGL ES 2.0 y Direct3D 11


OpenGL ES 2.0 y Direct3D 11 tienen muchas similitudes. Ambos tienen características de gráficos y canalización de representación similares. No obstante, Direct3D 11 es una implementación de representación y API, no es una especificación. OpenGL ES 2.0 es una especificación de representación y API, no es una implementación. Direct3D 11 y OpenGL ES 2.0 generalmente difiere de estas maneras:

| OpenGL ES 2.0                                                                                         | Direct3D 11                                                                                                            |
|-------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------|
| Especificación válida para el sistema operativo y hardware con implementaciones proporcionadas por el proveedor             | Implementación de Microsoft de certificación y abstracción de hardware en plataformas de Windows                                |
| Abstraído para diversidad de hardware, el tiempo de ejecución administra la mayoría de los recursos                                     | Acceso directo a diseño de hardware; la aplicación puede administrar recursos y procesamiento                                              |
| Proporciona módulos de nivel más alto a través de bibliotecas de terceros (por ejemplo, Simple DirectMedia Layer (SDL)) | Se compilan módulos de nivel más alto, como Direct2D, a partir de módulos más bajos para simplificar el desarrollo para aplicaciones de Windows             |
| Los proveedores de hardware se diferencian a través de extensiones                                                         | Microsoft agrega características opcionales a la API de una forma genérica para que no sean específicas de ningún proveedor de hardware en particular |

 

GLSL y HLSL generalmente difieren de estas maneras:

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">GLSL</th>
<th align="left">HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Procedimientos, centrado en los pasos (Como en C)</td>
<td align="left">Orientado a los objetos, centrado en los datos (Como en C++)</td>
</tr>
<tr class="even">
<td align="left">Compilación de sombreador integrada en la API gráfica</td>
<td align="left">El compilador de HLSL [compila el sombreador](https://msdn.microsoft.com/library/windows/desktop/bb509633) en una representación binaria intermedia antes de que Direct3D la pase al controlador.
<div class="alert">
<strong>Nota</strong>  Esta representación binaria no depende del hardware. Generalmente se compila en el tiempo de compilación de la aplicación en lugar de en su tiempo de ejecución.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Modificadores de almacenamiento de [variable](#porting-glsl-variables-to-hlsl)</td>
<td align="left">Búfer de constantes y transferencias de datos a través de declaraciones de diseño de entrada</td>
</tr>
<tr class="even">
<td align="left"><p>[Tipos](#porting-glsl-types-to-hlsl)</p>
<p>Tipo de vector típico: vec2/3/4</p>
<p>lowp, mediump, highp</p></td>
<td align="left"><p>Tipo de vector típico: float2/3/4</p>
<p>min10float, min16float</p></td>
</tr>
<tr class="odd">
<td align="left">texture2D [Function]</td>
<td align="left">[texture.Sample](https://msdn.microsoft.com/library/windows/desktop/bb509695) [datatype.Function]</td>
</tr>
<tr class="even">
<td align="left">sampler2D [datatype]</td>
<td align="left">[Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) [datatype]</td>
</tr>
<tr class="odd">
<td align="left">Matrices principales de fila (predeterminado)</td>
<td align="left">Matrices principales de columna (predeterminado)
<div class="alert">
<strong>Nota:</strong>   Usa el modificador de tipo <strong>row_major</strong> para cambiar el diseño de una variable. Para obtener más información, consulta el tema sobre la [sintaxis de variable](https://msdn.microsoft.com/library/windows/desktop/bb509706). También puedes especificar una marca de compilador o una pragma para cambiar el valor predeterminado global.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Sombreador de fragmentos</td>
<td align="left">Sombreador de píxeles</td>
</tr>
</tbody>
</table>

 

> **Nota:** HLSL tiene las texturas y las muestras como dos objetos separados. En GLSL, como en Direct3D 9, el enlace de textura es parte del estado de la muestra.

 

En GLSL, se presenta la mayoría del estado de OpenGL como variables globales predefinidas. Por ejemplo, con GLSL, se usa la variable **gl\_Position** para especificar la posición del vértice y la variable **gl\_FragColor** para especificar el color del fragmento. En HLSL, se pasa el estado de Direct3D explícitamente desde el código de la aplicación al sombreador. Por ejemplo, con Direct3D y HLSL, la entrada al sombreador de vértices debe coincidir con el formato de datos en el búfer de vértices, y la estructura de un búfer de constantes en el código de la aplicación debe coincidir con la estructura de un búfer de constantes ([cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581)) en el código del sombreador.

## <a name="porting-glsl-variables-to-hlsl"></a>Migrar variables de GLSL a HLSL


En GLSL, aplicas modificadores (calificadores) a una declaración de variable de sombreador global para darle a esa variable un comportamiento específico en tus sombreadores. En HLSL, estos modificadores no son necesarios porque el flujo del sombreador se define con los argumentos que se pasan al sombreador y que se devuelven de él.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Comportamiento de la variable de GLSL</th>
<th align="left">Equivalente de HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>uniform</strong></p>
<p>Se pasa una variable uniform desde el código de la aplicación a los sombreadores de fragmentos o vértices. Debes establecer los valores de todos los uniformes antes de dibujar cualquier triángulo con esos sombreadores para que sus valores no cambien durante el proceso de dibujo de una malla de triángulo. Estos valores son uniformes. Algunos uniformes se establecen para todo el cuadro y otros exclusivamente para un par de sombreador de vértices y píxeles particular.</p>
<p>Las variables uniformes son variables por polígono.</p></td>
<td align="left"><p>Se usa el búfer de constantes.</p>
<p>Consulta el tema sobre los [procedimientos para crear un búfer de constante](https://msdn.microsoft.com/library/windows/desktop/ff476896) y [constantes de sombreador](https://msdn.microsoft.com/library/windows/desktop/bb509581).</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>varying</strong></p>
<p>Se inicializa una variable varying dentro del sombreador de vértices y se pasa a una variable varying con el mismo nombre en el sombreador de fragmentos. Dado que el sombreador de vértices solamente establece el valor de las variables cambiantes en cada vértice, el rasterizador interpola esos valores (de una forma correcta para la perspectiva) para generar valores por fragmento para pasarlos al sombreador de fragmentos. Estas variables varían en cada triángulo.</p></td>
<td align="left">Se usa la estructura que se devuelve del sombreador de vértices como la entrada al sombreador de píxeles. Asegúrate de que los valores semánticos coincidan.</td>
</tr>
<tr class="odd">
<td align="left"><p><strong>attribute</strong></p>
<p>Un elemento attribute es una parte de la descripción de un vértice que se pasa desde el código de la aplicación solo al sombreador de vértices. A diferencia de un uniforme, estableces el valor de cada atributo para cada vértice, el cual permite que cada vértice tenga un valor diferente. Las variables de atributo son variables por vértice.</p></td>
<td align="left"><p>Define un búfer de vértices en tu código de la aplicación de Direct3D y hazlo coincidir con la entrada del vértice definida en el sombreador de vértices. Tienes la opción de definir un búfer de índice. Consulta el tema sobre los [procedimientos para crear un búfer de vértices](https://msdn.microsoft.com/library/windows/desktop/ff476899) y los [procedimientos para crear un búfer de índice](https://msdn.microsoft.com/library/windows/desktop/ff476897).</p>
<p>Crea un diseño de entrada en el código de la aplicación de Direct3D y haz coincidir los valores semánticos con los que se encuentran en la entrada de vértice. Consulta el tema sobre [cómo crear el diseño de entrada](https://msdn.microsoft.com/library/windows/desktop/bb205117#Create_the_Input_Layout).</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>const</strong></p>
<p>Constantes que se compilan dentro del sombreador y nunca cambian.</p></td>
<td align="left">Usa <strong>static const</strong>. <strong>static</strong> significa que el valor no está expuesto a búferes de constantes, <strong>const</strong> implica que el sombreador no puede cambiar el valor. Por ello, el valor se conoce en el tiempo de compilación según su inicializador.</td>
</tr>
</tbody>
</table>

 

En GLSL, las variables sin modificadores son tan solo variables globales ordinarias que son privadas para cada sombreador.

Cuando pasas datos a las texturas ([Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525) en HLSL) y sus muestras asociadas ([SamplerState](https://msdn.microsoft.com/library/windows/desktop/bb509644) en HLSL), generalmente los declaras como variables globales en el sombreador de píxeles.

## <a name="porting-glsl-types-to-hlsl"></a>Migrar tipos de GLSL a HLSL


Usa esta tabla para migrar tus tipos de GLSL a HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tipo de GLSL</th>
<th align="left">Tipo de HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">tipos escalares: float, int, bool</td>
<td align="left"><p>tipos escalares: float, int, bool</p>
<p>también, uint, double</p>
<p>Para más información, consulta el tema sobre los [tipos escalares](https://msdn.microsoft.com/library/windows/desktop/bb509646)).</p></td>
</tr>
<tr class="even">
<td align="left"><p>tipo de vector</p>
<ul>
<li>vector de punto flotante: vec2, vec3, vec4</li>
<li>vector booleano: bvec2, bvec3, bvec4</li>
<li>vector de entero con signo: ivec2, ivec3, ivec4</li>
</ul></td>
<td align="left"><p>tipo de vector</p>
<ul>
<li>float2, float3, float4, and float1</li>
<li>bool2, bool3, bool4, and bool1</li>
<li>int2, int3, int4, and int1</li>
<li><p>Estos tipos también tienen expansiones de vector similares a float, bool y int:</p>
<ul>
<li>uint</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>Para obtener más información, consulta el tema sobre el [tipo de vector](https://msdn.microsoft.com/library/windows/desktop/bb509707) y las [palabras clave](https://msdn.microsoft.com/library/windows/desktop/bb509568).</p>
<p>El tipo de vector también está definido como float4 (typedef vector &lt;float, 4&gt; vector;). Para obtener más información, consulta el tema sobre el [tipo definido por el usuario](https://msdn.microsoft.com/library/windows/desktop/bb509702).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>tipo de matriz</p>
<ul>
<li>mat2: matriz float 2x2</li>
<li>mat3: matriz float 3x3</li>
<li>mat4: matriz float 4x4</li>
</ul></td>
<td align="left"><p>tipo de matriz</p>
<ul>
<li>float2x2</li>
<li>float3x3</li>
<li>float4x4</li>
<li>también, float1x1, float1x2, float1x3, float1x4, float2x1, float2x3, float2x4, float3x1, float3x2, float3x4, float4x1, float4x2, float4x3</li>
<li><p>Estos tipos también tienen expansiones de matriz similares a float:</p>
<ul>
<li>int, uint, bool</li>
<li>min10float, min16float</li>
<li>min12int, min16int</li>
<li>min16uint</li>
</ul></li>
</ul>
<p>También puedes usar el [tipo de matriz](https://msdn.microsoft.com/library/windows/desktop/bb509623) para definir una matriz.</p>
<p>Por ejemplo: matriz &lt;float, 2, 2&gt; fMatrix = {0.0f, 0.1, 2.1f, 2.2f};</p>
<p>el tipo de matriz también está definido como float4x4 (matriz typedef &lt;float, 4, 4&gt; matrix;). Para obtener más información, consulta el tema sobre el [tipo definido por el usuario](https://msdn.microsoft.com/library/windows/desktop/bb509702).</p></td>
</tr>
<tr class="even">
<td align="left"><p>calificadores de precisión para float, int, sampler</p>
<ul>
<li><p>highp</p>
<p>Este calificador proporciona los requisitos de precisión mínimos que son mayores que los provistos por min16float y menores que un float de 32 bits completo. El equivalente en HLSL es:</p>
<p>highp float -&gt; float</p>
<p>highp int -&gt; int</p></li>
<li><p>mediump</p>
<p>Este calificador aplicado a float e int es equivalente a min16float y min12int en HLSL. Mínimo de 10 bits de mantisa, no como min10float.</p></li>
<li><p>lowp</p>
<p>Este calificador aplicado a float proporciona un intervalo de puntos flotantes de -2 a 2. Es equivalente a min10float en HLSL.</p></li>
</ul></td>
<td align="left"><p>tipos de precisión</p>
<ul>
<li>min16float: valor de punto flotante de 16 bits como mínimo</li>
<li><p>min10float</p>
<p>Valor de 2,8 bits con signo de punto fijo mínimo (2 bits de número entero y 8 bits de componente fraccionario). El componente fraccionario de 8 bits puede incluir el 1 en lugar de excluirlo para darle un rango de inclusión completo de -2 a 2.</p></li>
<li>min16int:: entero con signo de 16 bits mínimo</li>
<li><p>min12int: entero con signo de 12 bits mínimo</p>
<p>Este tipo es para 10Level9 ([niveles de característica 9_x ](https://msdn.microsoft.com/library/windows/desktop/ff476876)) en el que los enteros están representados por números de punto flotante. Esta es la precisión que puedes obtener cuando emulas un entero con un número de punto flotante de 16 bits.</p></li>
<li>min16uint: entero sin signo de 16 bits mínimo</li>
</ul>
<p>Para más información, consulta el tema sobre los [tipos escalares](https://msdn.microsoft.com/library/windows/desktop/bb509646) y el [uso de la precisión mínima de HLSL](https://msdn.microsoft.com/library/windows/desktop/hh968108).</p></td>
</tr>
<tr class="odd">
<td align="left">sampler2D</td>
<td align="left">[Texture2D](https://msdn.microsoft.com/library/windows/desktop/ff471525)</td>
</tr>
<tr class="even">
<td align="left">samplerCube</td>
<td align="left">[TextureCube](https://msdn.microsoft.com/library/windows/desktop/bb509700)</td>
</tr>
</tbody>
</table>

 

## <a name="porting-glsl-pre-defined-global-variables-to-hlsl"></a>Migrar variables globales predefinidas de GLSL a HLSL


Usa esta tabla para migrar variables globales predefinidas de GLSL a HLSL.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Variable global predefinida de GLSL</th>
<th align="left">Semántica de HLSL</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><strong>gl_Position</strong></p>
<p>Esta variable es de tipo <strong>vec4</strong>.</p>
<p>Posición del vértice</p>
<p>por ejemplo: gl_Position = position;</p></td>
<td align="left"><p>SV_Position</p>
<p>POSITION en Direct3D 9</p>
<p>Esta semántica es de tipo <strong>float4</strong>.</p>
<p>Salida del sombreador de vértices</p>
<p>Posición del vértice</p>
<p>por ejemplo: float4 vPosition : SV_Position;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_PointSize</strong></p>
<p>Esta variable es de tipo <strong>float</strong>.</p>
<p>Tamaño del punto</p></td>
<td align="left"><p>PSIZE</p>
<p>Sin significado a menos que el destino sea Direct3D 9</p>
<p>Esta semántica es de tipo <strong>float</strong>.</p>
<p>Salida del sombreador de vértices</p>
<p>Tamaño del punto</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragColor</strong></p>
<p>Esta variable es de tipo <strong>vec4</strong>.</p>
<p>Color de fragmento</p>
<p>por ejemplo: gl_FragColor = vec4(colorVarying, 1.0);</p></td>
<td align="left"><p>SV_Target</p>
<p>COLOR en Direct3D 9</p>
<p>Esta semántica es de tipo <strong>float4</strong>.</p>
<p>Salida del sombreador de píxeles</p>
<p>Color de píxel</p>
<p>por ejemplo: float4 Color[4] : SV_Target;</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragData[n]</strong></p>
<p>Esta variable es de tipo <strong>vec4</strong>.</p>
<p>Color de fragmento para adjunto de color n</p></td>
<td align="left"><p>SV_Target[n]</p>
<p>Esta semántica es de tipo <strong>float4</strong>.</p>
<p>Valor de salida del sombreador de píxeles que está almacenado en el destino de representación n, donde 0 &lt;= n &lt;= 7.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_FragCoord</strong></p>
<p>Esta variable es de tipo <strong>vec4</strong>.</p>
<p>Posición del fragmento dentro del búfer de cuadros</p></td>
<td align="left"><p>SV_Position</p>
<p>No disponible en Direct3D 9</p>
<p>Esta semántica es de tipo <strong>float4</strong>.</p>
<p>Entrada del sombreador de píxeles</p>
<p>Coordenadas de espacio de pantalla</p>
<p>por ejemplo: float4 screenSpace : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FrontFacing</strong></p>
<p>Esta variable es de tipo <strong>bool</strong>.</p>
<p>Determina si el fragmento pertenece a un primitivo frontal.</p></td>
<td align="left"><p>SV_IsFrontFace</p>
<p>VFACE en Direct3D 9</p>
<p>SV_IsFrontFace es de tipo <strong>bool</strong>.</p>
<p>VFACE es de tipo <strong>float</strong>.</p>
<p>Entrada del sombreador de píxeles</p>
<p>Orientación de primitiva</p></td>
</tr>
<tr class="odd">
<td align="left"><p><strong>gl_PointCoord</strong></p>
<p>Esta variable es de tipo <strong>vec2</strong>.</p>
<p>Posición de fragmento dentro de un punto (rasterización de punto solamente)</p></td>
<td align="left"><p>SV_Position</p>
<p>VPOS en Direct3D 9</p>
<p>SV_Position es de tipo <strong>float4</strong>.</p>
<p>VPOS es de tipo <strong>float2</strong>.</p>
<p>Entrada del sombreador de píxeles</p>
<p>La posición de la muestra o el píxel en el espacio de pantalla</p>
<p>por ejemplo: float4 pos : SV_Position</p></td>
</tr>
<tr class="even">
<td align="left"><p><strong>gl_FragDepth</strong></p>
<p>Esta variable es de tipo <strong>float</strong>.</p>
<p>Datos de búfer de profundidad</p></td>
<td align="left"><p>SV_Depth</p>
<p>DEPTH en Direct3D 9</p>
<p>SV_Depth es de tipo <strong>float</strong>.</p>
<p>Salida del sombreador de píxeles</p>
<p>Datos de búfer de profundidad</p></td>
</tr>
</tbody>
</table>

 

Se usa la semántica para especificar la posición, el color, etc. de la entrada del sombreador de vértices y la entrada del sombreador de píxeles. Debes hacer coincidir los valores de la semántica en el diseño de entrada con la entrada del sombreador de vértices. Para ver ejemplos, consulta [Ejemplos de cómo migrar variables de GLSL a HLS](#examples-of-porting-glsl-variables-to-hlsl). Para obtener más información acerca de la semántica de HLSL, consulta el tema sobre [semántica](https://msdn.microsoft.com/library/windows/desktop/bb509647).

## <a name="examples-of-porting-glsl-variables-to-hlsl"></a>Ejemplos de cómo migrar variables de GLSL a HLSL


Aquí te mostramos ejemplos del uso de variables de GLSL en código de OpenGL/GLSL y luego el ejemplo equivalente en código de Direct3D/HLSL.

### <a name="uniform-attribute-and-varying-in-glsl"></a>Elementos uniform, attribute y varying en GLSL

Código de aplicación de OpenGL

``` syntax
// Uniform values can be set in app code and then processed in the shader code.
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

// Incoming position of vertex
attribute vec4 position;
 
// Incoming color for the vertex
attribute vec3 color;
 
// The varying variable tells the shader pipeline to pass it  
// on to the fragment shader.
varying vec3 colorVarying;
```

Código de sombreador de vértices de GLSL

``` syntax
//The shader entry point is the main method.
void main()
{
colorVarying = color; //Use the varying variable to pass the color to the fragment shader
gl_Position = position; //Copy the position to the gl_Position pre-defined global variable
}
```

Código de sombreador de fragmentos de GLSL

``` syntax
void main()
{
//Pad the colorVarying vec3 with a 1.0 for alpha to create a vec4 color
//and assign that color to the gl_FragColor pre-defined global variable
//This color then becomes the fragment's color.
gl_FragColor = vec4(colorVarying, 1.0);
}
```

### <a name="constant-buffers-and-data-transfers-in-hlsl"></a>Búferes de constantes y transferencias de datos en HLSL

He aquí un ejemplo de cómo pasas datos al sombreador de vértices de HLSL que después fluyen a través del sombreador de píxeles. En tu código de aplicación, define un vértice y un búfer de constantes. Después, en el código del sombreador de vértices, define el búfer de constantes como un elemento [cbuffer](https://msdn.microsoft.com/library/windows/desktop/bb509581) y almacena los datos por vértice y los datos de entrada del sombreador de píxeles. Aquí se usan estructuras denominadas **VertexShaderInput** y **PixelShaderInput**.

Código de aplicación de Direct3D

```cpp
struct ConstantBuffer
{
    XMFLOAT4X4 model;
    XMFLOAT4X4 view;
    XMFLOAT4X4 projection;
};
struct SimpleCubeVertex
{
    XMFLOAT3 pos;   // position
    XMFLOAT3 color; // color
};

 // Create an input layout that matches the layout defined in the vertex shader code.
 const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
 {
     { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0,  0, D3D11_INPUT_PER_VERTEX_DATA, 0 },
     { "COLOR",    0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
 };

// Create vertex and index buffers that define a geometry.
```

Código del sombreador de vértices de HLSL

``` syntax
cbuffer ModelViewProjectionCB : register( b0 )
{
    matrix model; 
    matrix view;
    matrix projection;
};
// The POSITION and COLOR semantics must match the semantics in the input layout Direct3D app code.
struct VertexShaderInput
{
    float3 pos : POSITION; // Incoming position of vertex 
    float3 color : COLOR; // Incoming color for the vertex
};

struct PixelShaderInput
{
    float4 pos : SV_Position; // Copy the vertex position.
    float4 color : COLOR; // Pass the color to the pixel shader.
};

PixelShaderInput main(VertexShaderInput input)
{
    PixelShaderInput vertexShaderOutput;

    // shader source code

    return vertexShaderOutput;
}
```

Código del sombreador de píxeles de HLSL

``` syntax
// Collect input from the vertex shader. 
// The COLOR semantic must match the semantic in the vertex shader code.
struct PixelShaderInput
{
    float4 pos : SV_Position;
    float4 color : COLOR; // Color for the pixel
};

// Set the pixel color value for the renter target. 
float4 main(PixelShaderInput input) : SV_Target
{
    return input.color;
}
```

## <a name="examples-of-porting-opengl-rendering-code-to-direct3d"></a>Ejemplos de cómo migrar código de representación de OpenGL a Direct3D


Aquí se muestra un ejemplo de representación en código de OpenGL ES 2.0 y después el ejemplo equivalente en código de Direct3D 11.

Código de representación de OpenGL

``` syntax
// Bind shaders to the pipeline. 
// Both vertex shader and fragment shader are in a program.
glUseProgram(m_shader->getProgram());
 
// Input asssembly 
// Get the position and color attributes of the vertex.

m_positionLocation = glGetAttribLocation(m_shader->getProgram(), "position");
glEnableVertexAttribArray(m_positionLocation);

m_colorLocation = glGetAttribColor(m_shader->getProgram(), "color");
glEnableVertexAttribArray(m_colorLocation);
 
// Bind the vertex buffer object to the input assembler.
glBindBuffer(GL_ARRAY_BUFFER, m_geometryBuffer);
glVertexAttribPointer(m_positionLocation, 4, GL_FLOAT, GL_FALSE, 0, NULL);
glBindBuffer(GL_ARRAY_BUFFER, m_colorBuffer);
glVertexAttribPointer(m_colorLocation, 3, GL_FLOAT, GL_FALSE, 0, NULL);
 
// Draw a triangle with 3 vertices.
glDrawArray(GL_TRIANGLES, 0, 3);
```

Código de representación de Direct3D

```cpp
// Bind the vertex shader and pixel shader to the pipeline.
m_d3dDeviceContext->VSSetShader(vertexShader.Get(),nullptr,0);
m_d3dDeviceContext->PSSetShader(pixelShader.Get(),nullptr,0);
 
// Declare the inputs that the shaders expect.
m_d3dDeviceContext->IASetInputLayout(inputLayout.Get());
m_d3dDeviceContext->IASetVertexBuffers(0, 1, vertexBuffer.GetAddressOf(), &stride, &offset);

// Set the primitive's topology.
m_d3dDeviceContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);

// Draw a triangle with 3 vertices. triangleVertices is an array of 3 vertices.
m_d3dDeviceContext->Draw(ARRAYSIZE(triangleVertices),0);
```

## <a name="related-topics"></a>Temas relacionados


* [Migrar de OpenGL ES 2.0 a Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)

 

 




