---
title: Fase del ensamblador de entrada (IA)
description: La fase del ensamblador de entrada (IA) proporciona datos de primitivos y de adyacencia a la canalización, como triángulos, líneas y puntos, que incluyen los id. de semántica para reducir el procesamiento de primitivos que todavía no se procesaron con el fin de que los sombreadores sean más eficientes.
ms.assetid: AF1DC611-C872-47F1-BF1A-92C68C8903E6
keywords:
- Fase del ensamblador de entrada (IA)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8c5e1c294da2b4ef24ff8f62b686890cb8c69c06
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8196177"
---
# <a name="input-assembler-ia-stage"></a>Fase del ensamblador de entrada (IA)


La fase del ensamblador de entrada (IA) proporciona datos de primitivos y de adyacencia a la canalización, como triángulos, líneas y puntos, que incluyen los id. de semántica para reducir el procesamiento de primitivos que todavía no se procesaron con el fin de que los sombreadores sean más eficientes.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Finalidad y usos


La finalidad de la fase del ensamblador de entrada (IA) es leer datos de primitivos (puntos, líneas y triángulos) desde los búferes rellenados por el usuario, agrupar los datos en primitivos que usarán las otras fases de la canalización y adjuntar [valores generados por el sistema](https://msdn.microsoft.com/library/windows/desktop/bb509647) para ayudar a que los sombreadores sean más eficientes. Los valores generados por el sistema son cadenas de texto que también se denominan semántica. Se construyen las fases del sombreador programable desde un núcleo de sombreador común, que usa valores generados por el sistema (por ejemplo, un id. de primitivo, instancia o vértice), para que la fase del sombreador pueda reducir el procesamiento solo a los primitivos, las instancias o los vértices que todavía no se procesaron.

La fase de IA puede integrar vértices en varios [tipos primitivos](primitive-topologies.md) distintos (por ejemplo, listas de líneas, series de triángulos o primitivos con adyacencia). Los tipos de primitivos como una lista de triángulos con adyacencia y una lista de línea con adyacencia admiten la [fase de sombreador de geometría (GS)](geometry-shader-stage--gs-.md).

La información de adyacencia solo es visible para una aplicación en un sombreador de geometría. Si se invoca un sombreador de geometría con un triángulo con adyacencia, por ejemplo, los datos de entrada contendrían 3 vértices para cada triángulo y 3 vértices para datos de adyacencia por triángulo.

Cuando se solicita la salida de datos de adyacencia a la fase de IA, los datos de entrada deben incluir datos de adyacencia. Es posible que debas proporcionar un vértice ficticio (formar un triángulo incorrecto) o, quizás, marcar uno de los atributos de vértice para comprobar si el vértice existe o no. También sería necesario que un sombreador de geometría lo detectara y lo controlara, aunque la selección de geometría incorrecta sucederá en la fase de rasterización.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


La fase de IA lee datos de la memoria: datos de primitivos (puntos, líneas o triángulos), de los búferes rellenados por el usuario.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


La fase de IA ensambla los datos en primitivos, adjunta valores generados por el sistema y los genera como primitivos que usará la [fase del sombreador de vértices (VS)](vertex-shader-stage--vs-.md) y, a continuación, otras fases de la canalización.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="primitive-topologies.md">Topologías de primitivos</a></p></td>
<td align="left"><p>Direct3D admite varias topologías de primitivos, que definen cómo interpreta y representa la canalización los vértices, como listas de puntos, listas de líneas y series de triángulos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="using-system-generated-values.md">Uso de valores generados por el sistema</a></p></td>
<td align="left"><p>La fase del ensamblador de entrada (IA) genera valores generados por el sistema (basándose en la <a href="https://msdn.microsoft.com/library/windows/desktop/bb509647">semántica</a> de entrada proporcionada por el usuario) para admitir determinadas eficiencias en las operaciones del sombreador. Al adjuntar datos, como un id. de instancia (visible para la <a href="vertex-shader-stage--vs-.md">fase del sombreador de vértices (VS)</a>), un id. de vértice (visible para VS) o un id. de primitivo (visible para la <a href="geometry-shader-stage--gs-.md">fase del sombreador de geometría (GS)</a>/<a href="pixel-shader-stage--ps-.md">fase del sombreador de píxeles (PS)</a>), una fase del sombreador posterior puede ver estos valores del sistema para optimizar el procesamiento en esa fase.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




