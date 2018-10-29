---
title: Topologías primitivas
description: Direct3D admite varias topologías primitivas, que definen cómo se interpretan los vértices y cómo los representa la canalización, tales como listas de puntos, listas de líneas y series de triángulos.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Topologías primitivas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d7456cd773196520e066062c664f5e3073941dfe
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "5747823"
---
# <a name="primitive-topologies"></a>Topologías primitivas


Direct3D admite varias topologías primitivas, que definen cómo se interpretan los vértices y cómo los representa la canalización, tales como listas de puntos, listas de líneas y series de triángulos.

## <a name="span-idprimitivetypesspanspan-idprimitivetypesspanspan-idprimitivetypesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Topologías primitivas básicas


Se admiten las siguientes topologías primitivas (o tipos primitivos):

-   [Listas de puntos](point-lists.md)
-   [Listas de líneas](line-lists.md)
-   [Series de líneas](line-strips.md)
-   [Listas de triángulos](triangle-lists.md)
-   [Series de triángulos](triangle-strips.md)

Para una visualización de cada tipo primitivo, consulta el diagrama más adelante en este tema en [Dirección de generación y posiciones de los vértices iniciales](#winding-direction-and-leading-vertex-positions).

La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) lee los datos de los búferes de vértices e índices, ensambla los datos en estos primitivos y, a continuación, envía los datos a las demás fases de la canalización.

## <a name="span-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanspan-idprimitiveadjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Adyacencia de los primitivos


Todos los tipos primitivos de Direct3D (excepto la lista de puntos) están disponibles en dos versiones: un tipo primitivo con adyacencia y otro sin. Los primitivos con adyacencia contienen algunos de los vértices circundantes, mientras que los primitivos sin adyacencia solo contienen los vértices del primitivo de destino. Por ejemplo, el primitivo de lista de líneas tiene un primitivo de lista de líneas correspondiente que incluye adyacencia.

Los primitivos adyacentes están destinadas a proporcionar más información sobre la geometría y solo son visibles a través de un sombreador de geometría. La adyacencia resulta útil para los sombreadores de geometría que usan la detección de silueta, la extrusión de volumen de sombra, etc.

Por ejemplo, supongamos que quieres dibujar una lista de triángulos con adyacencia. Una lista de triángulos que contiene 36 vértices (con adyacencia) dará como resultado 6 primitivos completados. Los primitivos con adyacencia (excepto las series de líneas) contienen exactamente el doble de vértices que el primitivo equivalente sin adyacencia, donde cada vértice adicional es un vértice adyacente.

## <a name="span-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwindingdirectionandleadingvertexpositionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Dirección de generación y posiciones de los vértices iniciales


Como se muestra en la siguiente ilustración, un vértice inicial es el primer vértice no adyacente de un primitivo. Un tipo primitivo puede tener varios vértices iniciales definidos, siempre que cada uno de ellos se utilice para un primitivo diferente.

-   Para una serie de triángulos con adyacencia, los vértices iniciales son 0, 2, 4, 6, etc.
-   Para una serie de líneas con adyacencia, los vértices iniciales son 1, 2, 3, etc.
-   Un primitivo adyacente, por otro lado, no tiene ningún vértice inicial.

La siguiente ilustración muestra el orden de los vértices de todos los tipos primitivos que el ensamblador de entrada puede producir.

![Diagrama del orden de los vértices de los tipos primitivos](images/d3d10-primitive-topologies.png)

En la siguiente tabla, se describen los símbolos de la ilustración anterior.

| Símbolo                                                                                   | Nombre              | Descripción                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![Símbolo de un vértice](images/d3d10-primitive-topologies-vertex.png)                     | Vértice            | Un punto en el espacio 3D.                                                                |
| ![Símbolo de la dirección de generación](images/d3d10-primitive-topologies-winding-direction.png) | Dirección de generación | Orden de los vértices al ensamblar un primitivo. Puede ser en el sentido de las agujas del reloj o en sentido opuesto. |
| ![Símbolo de vértice inicial](images/d3d10-primitive-topologies-leading-vertex.png)       | Vértice inicial    | Primer vértice no adyacente de un primitivo que contiene datos por constante.       |

 

## <a name="span-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspanspan-idgeneratingmultiplestripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Generación de varias series


Puedes generar varias series a través del corte en tiras. Para realizar un corte en tiras, puedes llamar explícitamente a la función HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660) o insertar un valor de índice especial en el búfer de índices. Este valor es -1, que es 0xffffffff para índices de 32 bits o 0xffff para índices de 16 bits.

Un índice de -1 indica un "corte" o un "reinicio" explícito de la serie actual. El índice anterior completa la serie o el primitivo anterior y el índice siguiente inicia un nuevo primitivo o serie.

Para obtener más información acerca de cómo generar varias series, consulta [Fase de sombreador de geometría (GS)](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)

[Canalización de gráficos](graphics-pipeline.md)

 

 




