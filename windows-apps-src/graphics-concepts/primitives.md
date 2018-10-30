---
title: Primitivos
description: Un primitivo 3D es una colección de vértices que forma una sola entidad 3D.
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- Primitivos
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 349d4aa4fbf35bd7dccbd48b0251f5bb9e90a779
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5767866"
---
# <a name="primitives"></a>Primitivos


Un *primitivo* 3D es una colección de vértices que forma una sola entidad 3D.

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
<td align="left"><p><a href="point-lists.md">Listas de puntos</a></p></td>
<td align="left"><p>Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">Listas de líneas</a></p></td>
<td align="left"><p>Una lista de líneas es una lista de segmentos de línea recta aislados. Las listas de líneas son útiles para tareas como agregar aguanieve o lluvia torrencial a una escena 3D. Para crear una lista de líneas, las aplicaciones rellenan una matriz de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">Series de líneas</a></p></td>
<td align="left"><p>Una serie de líneas es un primitivo que se compone de segmentos de línea conectados. La aplicación puede usar las series de líneas para crear polígonos que no están cerrados. Un polígono cerrado es un polígono cuyo último vértice está conectada al primero por un segmento de línea.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">Listas de triángulos</a></p></td>
<td align="left"><p>Una lista de triángulos es una lista de triángulos aislados. Los triángulos aislados podrían o no estar próximos entre sí. Una lista de triángulos debe tener al menos tres vértices, y el número total de vértices debe ser divisible por tres.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">Series de triángulos</a></p></td>
<td align="left"><p>Una serie de triángulos es una serie de triángulos conectados. Dado que los triángulos están conectados, la aplicación no tiene que especificar repetidamente los tres vértices de cada triángulo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




