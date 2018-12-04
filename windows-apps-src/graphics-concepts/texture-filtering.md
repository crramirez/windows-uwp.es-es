---
title: Filtrado de texturas
description: El filtrado de texturas produce un color para cada píxel en la imagen representada en 2D del primitivo cuando un primitivo se representa mediante la asignación de un primitivo en 3D a una pantalla 2D.
ms.assetid: 1CCF4138-5D48-4B07-9490-996844F994D8
keywords:
- Filtrado de texturas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 449a31d92235efc50119bcd0db11b3532f523cd2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8474789"
---
# <a name="texture-filtering"></a>Filtrado de texturas


El filtrado de texturas produce un color para cada píxel en la imagen representada en 2D del primitivo cuando un primitivo se representa mediante la asignación de un primitivo en 3D a una pantalla 2D.

Cuando Direct3D representa un primitivo, asigna el primitivo 3D a una pantalla 2D. Si el primitivo tiene una textura, Direct3D debe usar esa textura para producir un color para cada píxel de la imagen representada en 2D del primitivo. Para cada píxel de la imagen en pantalla del primitivo, debe obtener un valor de color de la textura. Este proceso se denomina *filtrado de texturas*.

Cuando se realiza una operación de filtrado de texturas, la textura que se usa normalmente también se amplía o se reduce. En otras palabras, se asigna en una imagen de primitivo que es mayor o menor que sí misma. La ampliación de una textura puede provocar que se asignen muchos píxeles a un solo elemento de textura. El resultado puede ser un aspecto grueso. La reducción de una textura a menudo implica que un solo píxel se asigne a muchos elementos de textura. La imagen resultante puede ser borrosa o tener los bordes dentados. Para resolver estos problemas, se debe realizar alguna combinación de los colores de los elementos de textura para llegar a un color para el píxel.

Direct3D simplifica el proceso complejo de filtrado de texturas. Te ofrece tres tipos de filtrado de texturas: filtrado lineal, filtrado anisotrópico y filtrado de mapas MIP. Si no seleccionas ningún filtrado de texturas, Direct3D usa una técnica llamada "muestreo de punto más cercano".

Cada tipo de filtrado de texturas tiene ventajas y desventajas. Por ejemplo, el filtrado de texturas lineal puede producir bordes escalonados o un aspecto grueso en la imagen final. Sin embargo, es un método con baja sobrecarga de filtrado de texturas desde el punto de vista computacional. El filtrado con mapas MIP generalmente produce los mejores resultados, en especial si se combina con el filtrado anisotrópico. Sin embargo, es la técnica que requiere más memoria de las admitidas por Direct3D.

## <a name="span-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspanspan-idtypes-of-texture-filteringspantypes-of-texture-filtering"></a><span id="Types-of-texture-filtering"></span><span id="types-of-texture-filtering"></span><span id="TYPES-OF-TEXTURE-FILTERING"></span>Tipos de filtrado de texturas


Direct3D admite los siguientes métodos de filtrado de texturas.

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
<td align="left"><p><a href="nearest-point-sampling.md">Muestreo de punto más cercano</a></p></td>
<td align="left"><p>Las aplicaciones no tienen que usar el filtrado de texturas. Direct3D puede establecerse para que calcule la dirección de los elementos de textura, que a menudo no evalúa como enteros, y copia el color del elemento de textura con la dirección del número entero más cercano. Este proceso se denomina <em>muestreo de punto más cercano</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="bilinear-texture-filtering.md">Filtrado de texturas bilineal</a></p></td>
<td align="left"><p>El <em>filtrado bilineal</em> calcula el promedio ponderado de los 4 elementos de textura más cercanos al punto de muestreo. Este enfoque filtrado es más precisos y común que el filtrado por punto más cercano. Este método es eficaz, porque se implementa en el hardware gráfico actual.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="anisotropic-texture-filtering.md">Filtrado de texturas anisotrópico</a></p></td>
<td align="left"><p>La <em>anisotropía</em> es la distorsión visible en los elementos de textura de un objeto 3D cuya superficie está orientada en un ángulo con respecto al plano de la pantalla. Cuando un píxel de un primitivo anisotrópico se asigna a elementos de textura, su forma se distorsiona.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture-filtering-with-mipmaps.md">Filtrado de texturas con mapas MIP</a></p></td>
<td align="left"><p>Un <em>mapa MIP</em> es una secuencia de texturas, cada una de las cuales es una representación de la misma imagen con una resolución progresivamente inferior. El alto y ancho de cada imagen, o nivel, en el mapa MIP es una potencia de dos inferior al nivel anterior.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Texturas](textures.md)

 

 




