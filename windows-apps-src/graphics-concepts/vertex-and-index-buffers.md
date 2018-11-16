---
title: Búferes de vértices e índices
description: Los búferes de vértices son búferes de memoria que contienen datos de vértice. Los vértices de un búfer se procesan para transformar, iluminar y recortar.
ms.assetid: 8A39CD23-85FB-4424-9AC3-37919704CD68
keywords:
- Búferes de vértices e índices
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2327036eb53ac34c406aef53163be642468fbddc
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6972383"
---
# <a name="vertex-and-index-buffers"></a>Búferes de vértices e índices


Los *búferes de vértices* son búferes de memoria que contienen datos de vértice. Los vértices de un búfer se procesan para transformar, iluminar y recortar. Los *búferes de índices* son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.

Los búferes de vértices pueden contener cualquier tipo de vértice (transformado o no transformado, encendido o apagado) que se puedan representar. Puedes procesar los vértices en un búfer de vértices para realizar operaciones como transformación, iluminación o generación de marcas de recorte. Siempre se realiza la transformación.

La flexibilidad de los búferes de vértices los convierte en puntos de parada ideales para reutilizar geometría transformada. Podrías crear un único búfer de vértices, transformar, iluminar y recortar sus vértices, y re presentar el modelo en la escena tantas veces como sea necesario sin volver a transformarlo, incluso con cambios de estado de representación intercalados. Esto es útil cuando se representen los modelos que usan varias texturas: la geometría se transforma una sola vez y sus partes se pueden representar según sea necesario, intercaladas con los cambios necesarios de textura. Los cambios de estado de representación realizados después de procesar los vértices se aplicarán la próxima vez que se procesen los vértices.

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
<td align="left"><p><a href="introduction-to-buffers.md">Introducción a los búferes</a></p></td>
<td align="left"><p>Un recurso de búfer es una colección de datos completos, agrupados en elementos. Los búferes almacenan datos, como coordenadas de textura en un <em>búfer de vértices</em>, índices en una <em>búfer de índices</em>, datos de constantes del sombreador en un <em>búfer de constantes</em>, vectores de posición, vectores normales o el estado del dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="index-buffers.md">Búferes de índices</a></p></td>
<td align="left"><p>Los <em>búferes de índices</em> son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 




