---
title: ¿Cómo se organiza en mosaico el área de un recurso de streaming?
description: Cuando se crea un recurso de streaming, las dimensiones, el tamaño de los elementos de formato y el número de mapas MIP y/o segmentos de matriz (si procede) determinan el número de mosaicos necesarios para respaldar todo el área expuesta.
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- ¿Cómo se organiza en mosaico el área de un recurso de streaming?
- área de recursos, en mosaico
- tamaño de tablas, recursos, en mosaico
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fae56f673aa3d952b7e85490ec79676c2ac6a48a
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220348"
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>¿Cómo se organiza en mosaico el área de un recurso de streaming?


Cuando se crea un recurso de streaming, las dimensiones, el tamaño de los elementos de formato y el número de mapas MIP y/o segmentos de matriz (si procede) determinan el número de mosaicos necesarios para respaldar todo el área expuesta. El diseño de píxeles/bytes dentro de los mosaicos viene determinado por la implementación de. El número de píxeles que caben en un mosaico, dependiendo del tamaño del elemento de formato, es fijo y idéntico si se usa un swizzle estándar o no.

El número de mosaicos que va a usar un tamaño de superficie y un ancho de elemento de formato determinados está bien definido y predecible en función de las tablas de las secciones siguientes. En el caso de los recursos que contienen mapas MIP o casos en los que las dimensiones de la superficie no rellenan un icono, existen algunas restricciones; consulte [empaquetado de mipmap](mipmap-packing.md).

Distintos recursos de streaming pueden apuntar a la memoria idéntica con formatos diferentes, siempre y cuando las aplicaciones no se basen en los resultados de escribir en la memoria con un formato y leer con otro. Sin embargo, las aplicaciones pueden basarse en los resultados de escribir en la memoria con un formato y leer con otro si los formatos están en la misma familia de formato (es decir, tienen el mismo formato primario sin tipo). Por ejemplo, el formato de DXGI \_ \_ R8G8B8A8 \_ UNORM y el formato de dxgi \_ \_ R8G8B8A8 \_ uint son compatibles entre sí, pero no con el formato de dxgi \_ \_ R16G16 \_ UNORM.

Una excepción es donde se definen bien los datos de sangrados de un formato de alias a otro: Si un mosaico contiene completamente 0 para todos sus bits, ese mosaico se puede usar con cualquier formato que interprete el contenido de la memoria como 0 (independientemente del diseño de memoria). Por lo tanto, un icono se puede borrar a 0x00 con el formato DXGI \_ \_ \_ UNORM y después usarse con un formato como dxgi \_ format \_ R32G32 \_ float y parecería que el contenido sigue siendo (0.0 f, 0.0 f).

El diseño de los datos dentro de un mosaico no depende del lugar en el que se asigne el icono en un recurso global. Por lo tanto, por ejemplo, un icono se puede reutilizar en distintas ubicaciones de una superficie a la vez con un comportamiento coherente en todas las ubicaciones.

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
<td align="left"><p><a href="texture2d-and-texture2darray-subresource-tiling.md">Mosaico de subrecurso Texture2D y Texture2DArray</a></p></td>
<td align="left"><p>En estas tablas se muestra cómo se muestran los Subrecursos <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2d"><strong>Texture2D</strong></a> y <a href="/windows/desktop/direct3dhlsl/sm5-object-texture2darray"><strong>Texture2DArray</strong></a> en mosaico.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="texture3d-subresource-tiling.md">Mosaico de subrecurso Texture3D</a></p></td>
<td align="left"><p>En esta tabla se muestra cómo se muestran los Subrecursos de <a href="/windows/desktop/direct3dhlsl/sm5-object-texture3d"><strong>Texture3D</strong></a> en mosaico.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="buffer-tiling.md">Mosaico de búfer</a></p></td>
<td align="left"><p>Un recurso de <a href="introduction-to-buffers.md">búfer</a> se divide en mosaicos de 64 KB, con un espacio vacío en el último mosaico si el tamaño no es múltiplo de 64 KB.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="mipmap-packing.md">Empaquetado de mapas MIP</a></p></td>
<td align="left"><p>Un cierto número de MIPS (por segmento de matriz) se puede empaquetar en un número de mosaicos, en función de las dimensiones, el formato, el número de mapas de secuencias y los segmentos de la matriz de un recurso de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 