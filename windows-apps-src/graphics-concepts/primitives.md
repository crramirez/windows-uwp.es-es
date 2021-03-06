---
title: Primitivos
description: Un primitivo 3D es una colección de vértices que forma una sola entidad 3D.
ms.assetid: A1FE6F49-B0D4-4665-90E1-40AD98E668B1
keywords:
- Primitivos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6c2bf3aa2f421efa7061f3003e6cab8c9f7b8c58
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648670"
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
<td align="left"><p><a href="point-lists.md">Seleccione las listas</a></p></td>
<td align="left"><p>Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="line-lists.md">Listas de línea</a></p></td>
<td align="left"><p>Una lista de líneas es una lista de segmentos aislados de líneas rectas. Las listas de líneas son útiles para tareas como agregar aguanieve o lluvia torrencial a una escena 3D. Las aplicaciones crean una lista de líneas rellenando una matriz de vértices.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="line-strips.md">Bandas de línea</a></p></td>
<td align="left"><p>Una serie de líneas es un tipo de primitivo que se compone de segmentos de líneas conectados. La aplicación puede usar las series de líneas para crear polígonos que no están cerrados. Un polígono cerrado es un polígono cuyo último vértice está conectado a su primer vértice con un segmento de línea.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="triangle-lists.md">Listas de triángulo</a></p></td>
<td align="left"><p>Una lista de triángulos es una lista de triángulos aislados. Los triángulos aislados podrían o no estar próximos entre sí. Una lista de triángulos debe tener al menos tres vértices, y el número total de vértices debe ser divisible por tres.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-strips.md">Triángulo bandas</a></p></td>
<td align="left"><p>Una franja de triángulos es una serie de triángulos conectados. Dado que los triángulos están conectados, la aplicación no tiene que especificar repetidamente los tres vértices de cada triángulo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




