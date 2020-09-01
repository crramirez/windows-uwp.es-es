---
title: Topologías primitivas
description: Direct3D admite varias topologías primitivas, que definen cómo se interpretan y representan los vértices mediante la canalización, como las listas de puntos, las listas de líneas y las franjas de triángulo.
ms.assetid: 7AA5A4A2-0B7C-431D-B597-684D58C02BA5
keywords:
- Topologías primitivas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 45abb0c356b4ee6923bf6edd0b462f568749de5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156339"
---
# <a name="primitive-topologies"></a>Topologías primitivas


Direct3D admite varias topologías primitivas, que definen cómo se interpretan y representan los vértices mediante la canalización, como las listas de puntos, las listas de líneas y las franjas de triángulo.

## <a name="span-idprimitive_typesspanspan-idprimitive_typesspanspan-idprimitive_typesspanbasic-primitive-topologies"></a><span id="Primitive_Types"></span><span id="primitive_types"></span><span id="PRIMITIVE_TYPES"></span>Topologías primitivas básicas


Se admiten las siguientes topologías primitivas básicas (o tipos primitivos):

-   [Listas de puntos](point-lists.md)
-   [Listas de líneas](line-lists.md)
-   [Series de líneas](line-strips.md)
-   [Listas de triángulos](triangle-lists.md)
-   [Series de triángulos](triangle-strips.md)

Para ver una visualización de cada tipo primitivo, vea el diagrama que aparece más adelante en este tema en [dirección de bobinado y posiciones de vértice iniciales](#winding-direction-and-leading-vertex-positions).

La [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) Lee los datos de los búferes de vértices y de índices, ensambla los datos en estos primitivos y, a continuación, envía los datos a las fases de canalización restantes.

## <a name="span-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanspan-idprimitive_adjacencyspanprimitive-adjacency"></a><span id="Primitive_Adjacency"></span><span id="primitive_adjacency"></span><span id="PRIMITIVE_ADJACENCY"></span>Adyacencia primitiva


Todos los tipos primitivos de Direct3D (excepto la lista de puntos) están disponibles en dos versiones: un tipo primitivo con adyacencias y un tipo primitivo sin adyacencias. Los primitivos con adyacencias contienen algunos de los vértices circundantes, mientras que los primitivos sin adyacencias contienen solo los vértices de la primitiva de destino. Por ejemplo, el primitivo de la lista de líneas tiene un primitivo de lista de líneas correspondiente que incluye la adyacencia.

Los primitivos adyacentes están diseñados para proporcionar más información sobre su geometría y solo son visibles a través de un sombreador de geometría. La adyacencia es útil para los sombreadores de geometría que utilizan la detección de silueta, la extrusión de volumen de sombra, etc.

Por ejemplo, supongamos que desea dibujar una lista de triángulos con adyacencias. Una lista de triángulos que contiene 36 vértices (con adyacencias) producirá 6 primitivas completadas. Las primitivas con adyacencias (excepto las bandas de líneas) contienen exactamente el doble de vértices que el primitivo equivalente sin adyacencias, donde cada vértice adicional es un vértice adyacente.

## <a name="span-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding_direction_and_leading_vertex_positionsspanspan-idwinding-direction-and-leading-vertex-positionsspanwinding-direction-and-leading-vertex-positions"></a><span id="Winding_Direction_and_Leading_Vertex_Positions"></span><span id="winding_direction_and_leading_vertex_positions"></span><span id="WINDING_DIRECTION_AND_LEADING_VERTEX_POSITIONS"></span><span id="winding-direction-and-leading-vertex-positions"></span>Dirección de bobinado y posiciones de vértice iniciales


Como se muestra en la siguiente ilustración, un vértice inicial es el primer vértice no adyacente de un primitivo. Un tipo primitivo puede tener varios vértices iniciales definidos, siempre y cuando cada uno se use para un primitivo diferente.

-   En el caso de una franja de triángulo con adyacencias, los vértices iniciales son 0, 2, 4, 6, etc.
-   En el caso de una franja de líneas con adyacencias, los vértices iniciales son 1, 2, 3, etc.
-   Por otro lado, un primitivo adyacente no tiene ningún vértice inicial.

En la ilustración siguiente se muestra el orden de los vértices de todos los tipos primitivos que puede generar el ensamblador de entrada.

![diagrama de ordenación de vértices para tipos primitivos](images/d3d10-primitive-topologies.png)

Los símbolos de la ilustración anterior se describen en la tabla siguiente.

| Símbolo                                                                                   | NOMBRE              | Descripción                                                                         |
|------------------------------------------------------------------------------------------|-------------------|-------------------------------------------------------------------------------------|
| ![símbolo de un vértice](images/d3d10-primitive-topologies-vertex.png)                     | Vértice            | Punto en el espacio 3D.                                                                |
| ![símbolo de dirección de bobinado](images/d3d10-primitive-topologies-winding-direction.png) | Dirección de bobinado | El orden de los vértices al ensamblar un primitivo. Puede ser en el sentido contrario a las agujas del reloj. |
| ![símbolo para el vértice inicial](images/d3d10-primitive-topologies-leading-vertex.png)       | Vértice inicial    | Primer vértice no adyacente de un primitivo que contiene datos por constante.       |

 

## <a name="span-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspanspan-idgenerating_multiple_stripsspangenerating-multiple-strips"></a><span id="Generating_Multiple_Strips"></span><span id="generating_multiple_strips"></span><span id="GENERATING_MULTIPLE_STRIPS"></span>Generación de varias bandas


Puede generar varias bandas a través de la reducción de la franja. Puede realizar un corte de franja llamando explícitamente a la función HLSL [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) o insertando un valor de índice especial en el búfer de índice. Este valor es-1, que es 0xFFFFFFFF para índices de 32 bits o 0xFFFF para índices de 16 bits.

Un índice de – 1 indica un "cortar" explícito o un "reinicio" de la franja actual. El índice anterior completa el primitivo o la franja anterior y el índice siguiente inicia una nueva banda o primitiva.

Para obtener más información sobre la generación de varias bandas, consulte la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md)

[Canalización de gráficos](graphics-pipeline.md)

 

 