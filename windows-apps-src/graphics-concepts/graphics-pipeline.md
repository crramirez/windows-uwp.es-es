---
title: "Canalización de gráficos"
description: "La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables."
ms.assetid: C9519AD0-5425-48BD-9FF4-AED8959CA4AD
keywords:
- "Canalización de gráficos"
- "Fases de canalización"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 72e4985481f464cd45c13fd390d27c4ba0e7c618
ms.lasthandoff: 02/07/2017

---

# <a name="graphics-pipeline"></a>Canalización de gráficos


La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.

Todas las fases pueden configurarse mediante la API de Direct3D. Las fases que ofrecen núcleos de sombreador comunes (bloques rectangulares redondeados) pueden programarse mediante el lenguaje de programación [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561). Esto hace que la canalización sea muy flexible y adaptable.

La más utilizadas son la fase del sombreador de vértices (VS) y la fase del sombreador de píxeles (PS). Si no proporcionas estas fases de sombreador, se usa el sombreador de píxeles y un vértice de paso a través no operativo predeterminado.

![diagrama del flujo de datos en la canalización programable de Direct3D 11](images/d3d11-pipeline-stages.jpg)

## <a name="input-assembler-stage"></a>Fase del ensamblador de entrada

 

| | |
|-|-|
|Propósito|La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) proporciona datos de primitivos y de adyacencia a la canalización, como triángulos, líneas y puntos, que incluyen los id. de semántica para reducir el procesamiento de primitivos que todavía no se procesaron con el fin de que los sombreadores sean más eficientes.|
|Entrada|Datos primitivos (triángulos, líneas y puntos) de los búferes rellenados por el usuario en la memoria. Y posiblemente datos de adyacencia. Un triángulo tendría 3 vértices para cada triángulo y posiblemente 3 vértices para los datos de adyacencia de cada triángulo.|
|Salida|Primitivos con valores adjuntos generados por el sistema (como un id. primitivo, un id. de instancia o un id. de vértice).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Input Assembler (IA) stage](input-assembler-stage--ia-.md) supplies primitive and adjacency data to the pipeline, such as triangles, lines and points, including semantics IDs to help make shaders more efficient by reducing processing to primitives that haven't already been processed.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> Primitive data (triangles, lines, and/or points), from user-filled buffers in memory. And possibly adjacency data. A triangle would be 3 vertices for each triangle and possibly 3 vertices for adjacency data per triangle.  </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Primitives with attached system-generated values (such as a primitive ID, an instance ID, or a vertex ID). </td>
</tr>
</tbody>
</table>
 -->



## <a name="vertex-shader-stage"></a>Fase del sombreador de vértices
 

| | |
|-|-|
|Propósito|La [fase del sombreador de vértices](vertex-shader-stage--vs-.md) procesa vértices, normalmente realizando ciertas operaciones, como transformar, enmascarar e iluminar. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida. Operaciones por vértice individuales, como transformaciones, enmascaramientos e iluminaciones por vértice.|
|Entrada|Un solo vértice, con los valores VertexID e InstanceID generados por el sistema. Cada vértice de entrada de sombreador de vértices puede comprender hasta 16 vectores de 32 bits (hasta 4 componentes cada uno).|
|Salida|Un solo vértice. Cada vértice de salida puede comprender hasta 16 vectores de 4 componentes de 32 bits.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Vertex Shader (VS) stage](vertex-shader-stage--vs-.md) processes vertices, typically performing operations such as transformations, skinning, and lighting. A vertex shader takes a single input vertex and produces a single output vertex. Individual per-vertex operations, such as:
<ul>
<li>Transformations</li>
<li>Skinning</li>
<li>Morphing</li>
<li>Per-vertex lighting</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">A single vertex, with VertexID and InstanceID system-generated values. Each vertex shader input vertex can be comprised of up to 16 32-bit vectors (up to 4 components each).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">A single vertex. Each output vertex can be comprised of as many as 16 32-bit 4-component vectors.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="hull-shader-stage"></a>Fase del sombreador de casco
 

| | |
|-|-|
|Propósito|La [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md) es una de las fases de teselación, que divide de forma eficaz una sola superficie de un modelo en muchos triángulos. Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en puntos de control que conforman una revisión. También hace algunos cálculos por revisión para proporcionar datos de la fase del teselador (TS) y la fase del sombreador de dominios (DS).|
|Entrada|Entre 1 y 32 puntos de control de entrada, que en conjunto definen una superficie de orden inferior.|
|Salida|Entre 1 y 32 puntos de control de salida, que en conjunto forman una revisión. El sombreador de casco declara el estado de la fase del teselador (TS), incluido el número de puntos de control, el tipo de la cara de revisión y el tipo de partición que se debe usar al realizar la teselación.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Hull Shader (HS) stage](hull-shader-stage--hs-.md) is one of the tessellation stages, which efficiently break up a single surface of a model into many triangles. A hull shader is invoked once per patch, and it transforms input control points that define a low-order surface into control points that make up a patch. It also does some per-patch calculations to provide data for the Tessellator (TS) stage and the Domain Shader (DS) stage.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Between 1 and 32 input control points, which together define a low-order surface. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">Between 1 and 32 output control points, which together make up a patch. The hull shader declares the state for the Tessellator (TS) stage, including the number of control points, the type of patch face, and the type of partitioning to use when tessellating. </td>
</tr>
</tbody>
</table>
-->

## <a name="tessellator-stage"></a>Fase del teselador
 

| | |
|-|-|
|Propósito|La [fase del teselador (TS)](tessellator-stage--ts-.md) crea un patrón de muestreo del dominio que representa la revisión de geometría y genera un conjunto de objetos de menor tamaño (triángulos, líneas o puntos) que conectan con estas muestras.|
|Entrada|El teselador opera una vez por revisión con los factores de teselación (que especifican con qué precisión se teselará el dominio) y el tipo de partición (que especifica el algoritmo usado para dividir una revisión) que se pasan desde la fase del sombreador de casco. |
|Salida|El teselador devuelve las coordenadas uv (y, opcionalmente, w) y la topología de superficie para la fase del sombreador de dominios.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Tessellator (TS) stage](tessellator-stage--ts-.md) creates a sampling pattern of the domain that represents the geometry patch and generates a set of smaller objects (triangles, points, or lines) that connect these samples.   </td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"> The tessellator operates once per patch using the tessellation factors (which specify how finely the domain will be tessellated) and the type of partitioning (which specifies the algorithm used to slice up a patch) that are passed in from the hull-shader stage. </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The tessellator outputs uv (and optionally w) coordinates and the surface topology to the domain-shader stage.     </td>
</tr>
</tbody>
</table>
 -->

## <a name="domain-shader-stage"></a>Fase del sombreador de dominios
 

| | |
|-|-|
|Propósito|La [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice correspondiente a cada muestra de dominio. Un sombreador de dominios se ejecuta una vez por punto de salida de la fase del teselador y tiene acceso de solo lectura a las constantes de revisión de salida y de revisión de salida del sombreador de casco, y a las coordenadas de UV de salida de la fase del teselador.|
|Entrada|Un sombreador de dominios consume puntos de control de salida de la [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md). Las salidas del sombreador de casco incluyen: puntos de control, datos constantes de revisión y factores de teselación (los factores de teselación pueden incluir los valores que usa el teselador de función fija, así como los valores sin procesar [antes del redondeo por teselación de entero], lo que facilita la geotransformación, por ejemplo). Un sombreador de dominios se invoca una vez por coordenada de salida desde la [fase de teselador (TS)](tessellator-stage--ts-.md).|
|Salida|La fase del sombreador de dominios (DS) devuelve la posición del vértice de un punto subdividido en la revisión de salida.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Domain Shader (DS) stage](domain-shader-stage--ds-.md) calculates the vertex position of a subdivided point in the output patch; it calculates the vertex position that corresponds to each domain sample. A domain shader is run once per tessellator stage output point and has read-only access to the hull shader output patch and output patch constants, and the tessellator stage output UV coordinates.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>A domain shader consumes output control points from the [Hull Shader (HS) stage](hull-shader-stage--hs-.md). The hull shader outputs include:
<ul>
<li>Control points.</li>
<li>Patch constant data.</li>
<li>Tessellation factors. The tessellation factors can include the values used by the fixed-function tessellator as well as the raw values (before rounding by integer tessellation, for example), which facilitates geomorphing, for example.</li>
</ul></li>
<li>A domain shader is invoked once per output coordinate from the [Tessellator (TS) stage](tessellator-stage--ts-.md).</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Domain Shader (DS) stage outputs the vertex position of a subdivided point in the output patch.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="geometry-shader-stage"></a>Fase de sombreador de geometría
 

| | |
|-|-|
|Propósito|La [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) procesa primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Admite la amplificación y la desamplificación geométrica. Es útil para algoritmos, como Point Sprite Expansion, Dynamic Particle Systems, Fur/Fin Generation, Shadow Volume Generation, Single Pass Render-to-Cubemap, Per-Primitive Material Swapping y Per-Primitive Material Setup, que incluye la generación de coordenadas baricéntricas como datos primitivos, de modo que un sombreador de píxeles pueda realizar la interpolación de atributos personalizada. |
|Entrada|A diferencia de los sombreadores de vértices, que trabajan en un solo vértice, las entradas del sombreador de geometría son los vértices para un primitivo completo (tres vértices para triángulos, dos vértices para líneas o un solo vértice para el punto).|
|Salida|La fase del sombreador de geometría (GS) es capaz de presentar varios vértices que forman una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son <strong>tristrip</strong>, <strong>linestrip</strong> y <strong>pointlist</strong>. El número de primitivos emitido puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podría emitirse debe declararse de forma estática. Las longitudes de franja emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias, y pueden crearse nuevas franjas a través de la función HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Geometry Shader (GS) stage](geometry-shader-stage--gs-.md) processes entire primitives: triangles, lines, and points, along with their adjacent vertices. It is useful for algorithms including Point Sprite Expansion, Dynamic Particle Systems, and Shadow Volume Generation. It supports geometry amplification and de-amplification.
<ul>
<li>Point Sprite Expansion</li>
<li>Dynamic Particle Systems</li>
<li>Fur/Fin Generation</li>
<li>Shadow Volume Generation</li>
<li>Single Pass Render-to-Cubemap</li>
<li>Per-Primitive Material Swapping</li>
<li>Per-Primitive Material Setup, including generation of barycentric coordinates as primitive data so that a pixel shader can perform custom attribute interpolation.</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Unlike vertex shaders, which operate on a single vertex, the geometry shader's inputs are the vertices for a full primitive (three vertices for triangles, two vertices for lines, or single vertex for point).</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Geometry Shader (GS) stage is capable of outputting multiple vertices forming a single selected topology. Available geometry shader output topologies are <strong>tristrip</strong>, <strong>linestrip</strong>, and <strong>pointlist</strong>. The number of primitives emitted can vary freely within any invocation of the geometry shader, though the maximum number of vertices that could be emitted must be declared statically. Strip lengths emitted from a geometry shader invocation can be arbitrary, and new strips can be created via the [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) HLSL function.</td>
</tr>
</tbody>
</table>
-->
 

## <a name="stream-output-stage"></a>Fase de salida de flujo
 

| | |
|-|-|
|Propósito|La [fase de salida de flujo (SO)](stream-output-stage--so-.md) devuelve (o transmite) continuamente datos de vértices de la fase activa anterior a uno o más búferes de la memoria. Los datos trasmitidos a la memoria pueden regresar a la canalización como datos de entrada o volver a leerse en la CPU.|
|Entrada|Datos de los vértices de una fase de canalización anterior.|
|Salida|La fase de salida de flujo (SO) devuelve (o transmite) continuamente datos de vértices de la fase activa anterior, como la fase del sombreador de geometría (GS), a uno o más búferes de la memoria. Si la fase del sombreador de geometría (GS) está inactiva y la fase de salida de flujo (SO) está activa, devuelve continuamente datos de vértices de la fase del sombreador de dominio (DS) a los búferes de la memoria (o si DS también está inactivo, de la fase del sombreador de vértices [VS]).|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Stream Output (SO) stage](stream-output-stage--so-.md) continuously outputs (or streams) vertex data from the previous active stage to one or more buffers in memory. Data streamed out to memory can be recirculated back into the pipeline as input data, or read-back from the CPU.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertex data from a previous pipeline stage.   </td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The Stream Output (SO) stage continuously outputs (or streams) vertex data from the previous active stage, such as the Geometry Shader (GS) stage, to one or more buffers in memory. If the Geometry Shader (GS) stage is inactive, and the Stream Output (SO) stage is active, it continuously outputs vertex data from the Domain Shader (DS) stage to buffers in memory (or if the DS is also inactive, from the Vertex Shader (VS) stage).</td>
</tr>
</tbody>
</table>
-->

## <a name="rasterizer-stage"></a>Fase del rasterizador
 

| | |
|-|-|
|Propósito|La [fase del rasterizador (RS)](rasterizer-stage--rs-.md) recorta primitivos que no están en la vista, los prepara para la fase del sombreador de píxeles (PS) y determina cómo invocar sombreadores de píxeles. Convierte la información de vector (que se compone de formas o primitivos) en una imagen de trama (que se compone de píxeles) con el fin de mostrar gráficos 3D en tiempo real.|
|Entrada|Se supone que los vértices (x,y,z,w) que se incorporan a la fase de rasterización están en el espacio de recorte homogéneo. En este espacio de coordenadas, el eje X apunta a la derecha, el eje Y apunta arriba y el eje Z, fuera de la cámara.|
|Salida|Píxeles reales que se deben representar. Incluye algunos atributos de vértice que debe usar el sombreador de píxeles en la interpolación.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Rasterizer (RS) stage](rasterizer-stage--rs-.md) clips primitives that aren't in view, prepares primitives for the Pixel Shader (PS) stage, and determines how to invoke pixel shaders. Converts vector information (composed of shapes or primitives) into a raster image (composed of pixels) for the purpose of displaying real-time 3D graphics.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left">Vertices (x,y,z,w) coming into the Rasterizer stage are assumed to be in homogeneous clip-space. In this coordinate space the X axis points right, Y points up and Z points away from camera.</td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The actual pixels that need to be rendered. Includes some vertex attributes for use in interpolation by the pixel Shader.</td>
</tr>
</tbody>
</table>
 -->

## <a name="pixel-shader-stage"></a>Fase del sombreador de píxeles
 

| | |
|-|-|
|Propósito|La [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md) recibe datos interpolados de un primitivo y genera datos por píxel, como el color.|
|Entrada|Cuando se configura la canalización sin un sombreador de geometría, un sombreador de píxeles se limita a las entradas de 4 componentes de 16 y 32 bits. De lo contrario, un sombreador de píxeles puede aceptar hasta 32 entradas de 4 componentes y 32 bits. Los datos de entrada del sombreador de píxeles incluyen atributos de vértice (que se pueden interpolar con o sin corrección de la perspectiva) o pueden tratarse como constantes por primitivo. Las entradas del sombreador de píxeles se interpolan desde los atributos de vértice del primitivo que se está rasterizando, según el modo de interpolación declarado. Si un primitivo se recorta antes de la rasterización, el modo de interpolación se respeta también durante el proceso de recorte. |
|Salida|Un sombreador de píxeles puede producir hasta 8 colores de 4 componentes y 32 bits, o ningún color si el píxel se descarta. Los componentes de registro de salida del sombreador de píxeles deben declararse para que se puedan utilizar; se permite una máscara de escritura de salida diferente a cada registro.|
| | |

<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Pixel Shader (PS) stage](pixel-shader-stage--ps-.md) receives interpolated data for a primitive and generates per-pixel data such as color.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><p>When the pipeline is configured without a geometry shader, a pixel shader is limited to 16, 32-bit, 4-component inputs. Otherwise, a pixel shader can take up to 32, 32-bit, 4-component inputs.</p>
<p>Pixel shader input data includes vertex attributes (that can be interpolated with or without perspective correction) or can be treated as per-primitive constants. Pixel shader inputs are interpolated from the vertex attributes of the primitive being rasterized, based on the interpolation mode declared. If a primitive gets clipped before rasterization, the interpolation mode is honored during the clipping process as well.</p></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left"><p>A pixel shader can output up to 8, 32-bit, 4-component colors, or no color if the pixel is discarded. Pixel shader output register components must be declared before they can be used; each register is allowed a distinct output-write mask.</p></td>
</tr>
</tbody>
</table>
-->
 

## <a name="output-merger-stage"></a>Fase de fusión de salida
 

| | |
|-|-|
|Propósito|La [fase de fusión de salida (OM)](output-merger-stage--om-.md) combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad o galería de símbolos para generar el resultado final de la canalización.|
|Entrada|Las entradas de fusión de salida son el estado de canalización, los datos de píxeles generados por los sombreadores de píxeles, el contenido de los destinos de representación y el contenido de los búferes de profundidad o galería de símbolos.|
|Salida|Color de píxel de representación final.|
| | |


<!---
<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Purpose</td>
<td align="left">The [Output Merger (OM) stage](output-merger-stage--om-.md) combines various types of output data (pixel shader values, depth and stencil information) with the contents of the render target and depth/stencil buffers to generate the final pipeline result.</td>
</tr>
<tr class="even">
<td align="left">Input</td>
<td align="left"><ul>
<li>Pipeline state</li>
<li>The pixel data generated by the pixel shaders</li>
<li>The contents of the render targets</li>
<li>The contents of the depth/stencil buffers.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Output</td>
<td align="left">The final rendered pixel color.</td>
</tr>
</tbody>
</table>


| | |
|-|-|
|Purpose|xxxx|
|Input|yyyy|
|Output|zzzz|
| | |
-->

## <a name="related-topics"></a>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

[Canalización del proceso](compute-pipeline.md)

 

 

