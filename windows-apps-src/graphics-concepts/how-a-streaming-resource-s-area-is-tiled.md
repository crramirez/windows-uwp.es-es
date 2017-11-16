---
title: "Cómo se organiza en iconos el área de un recurso de streaming"
description: "Cuando creas un recurso de streaming, las dimensiones, el tamaño del elemento de formato y el número de mapas MIP o los segmentos de matriz (si procede) determinan el número de iconos que son necesarios para cubrir toda la superficie."
ms.assetid: 3485FD8D-2A06-4B0A-8810-ECF37736F94B
keywords:
- "Cómo se organiza en iconos el área de un recurso de streaming"
- "área de recursos, en iconos"
- "tablas de tamaño, size tables, recursos, resources, iconos, tiled"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 0718557245d9072adb0db608a7692dc7674ceca8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="how-a-streaming-resources-area-is-tiled"></a>Cómo se organiza en iconos el área de un recurso de streaming


Cuando creas un recurso de streaming, las dimensiones, el tamaño del elemento de formato y el número de mapas MIP o los segmentos de matriz (si procede) determinan el número de iconos que son necesarios para cubrir toda la superficie. El diseño de bytes o píxeles en iconos viene determinado por la implementación. El número de píxeles que caben en un icono, según el tamaño del elemento de formato, es fijo e idéntico tanto si usas una referencia estándar o no.

El número de iconos que se usará en un ancho de elemento de formato y un tamaño de superficie determinados está bien definido y es predecible según las tablas de las siguientes secciones. Para los recursos que contienen casos o mapas MIP donde las dimensiones de la superficie no rellenan un icono, existen algunas restricciones; consulta [Mipmap packing (Empaquetado de mapas MIP](mipmap-packing.md).

Diferentes recursos de streaming pueden apuntar a una memoria idéntica con diferentes formatos siempre que las aplicaciones no se basen en los resultados de la escritura en la memoria con un formato y de la lectura con otro. Sin embargo, las aplicaciones pueden basarse en los resultados de escritura en la memoria con un formato y de lectura con otro, si los formatos tienen la misma familia de formato (es decir, si tienen el mismo formato principal sin tipo). Por ejemplo, DXGI\_FORMAT\_R8G8B8A8\_UNORM y DXGI\_FORMAT\_R8G8B8A8\_UINT son compatibles entre sí, pero no con DXGI\_FORMAT\_R16G16\_UNORM.

Existe una excepción cuando los datos de sangrado están bien definidos de un formato con efecto escalonado (aliasing) a otro: si un icono contiene completamente 0 para todos los bits, ese icono puede usarse con cualquier formato que interprete esos contenidos de la memoria como 0 (independientemente del diseño de la memoria). Por lo tanto, un icono podría borrarse en 0x00 con el formato DXGI\_FORMAT\_R8\_UNORM y, a continuación, usarse con un formato como DXGI\_FORMAT\_R32G32\_FLOAT y el contenido aún se mostraría como (0.0f, 0.0f).

El diseño de los datos dentro de un icono no depende de dónde se asigna el icono en un recurso global. Por lo tanto, por ejemplo, un icono se puede reutilizar en distintas ubicaciones de una superficie a la vez con un comportamiento coherente en todas las ubicaciones.

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
<td align="left"><p>[Icono de subrecurso Texture2D y Texture2DArray](texture2d-and-texture2darray-subresource-tiling.md)</p></td>
<td align="left"><p>Estas tablas muestran cómo los subrecursos [<strong>Texture2D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [<strong>Texture2DArray</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471526) se colocan en iconos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Icono de subrecurso Texture3D](texture3d-subresource-tiling.md)</p></td>
<td align="left"><p>En esta tabla se muestra cómo los subrecursos [<strong>Texture3D</strong>](https://msdn.microsoft.com/library/windows/desktop/ff471562) se colocan en iconos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Icono de búfer](buffer-tiling.md)</p></td>
<td align="left"><p>Un recurso de [búfer](introduction-to-buffers.md) se divide en iconos de 64KB, con algo de espacio vacío en el último icono si el tamaño no es un múltiplo de 64KB.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Empaquetado de mapas MIP](mipmap-packing.md)</p></td>
<td align="left"><p>Se pueden empaquetar algunos MIP (por segmento de matriz) en un determinado número de iconos, según las dimensiones, el formato, el número de mapas MIP y los segmentos de matriz de un recurso de streaming.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Creación de recursos de streaming](creating-streaming-resources.md)

 

 



