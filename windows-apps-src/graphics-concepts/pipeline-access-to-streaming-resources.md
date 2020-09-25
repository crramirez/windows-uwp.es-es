---
title: Acceso de canalización a recursos de streaming
description: Los recursos de streaming se pueden usar en las vistas de recursos del sombreador (SRV), las vistas de destino de representación (RTV), las vistas de estarcido de profundidad (DSV) y las vistas de acceso desordenado (UAV), así como algunos puntos de enlace en los que no se usan vistas, como los enlaces de búfer de vértices.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Acceso de canalización a recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 15b37e471e45a1c2ca604c1a5bf28ace69e35ad3
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220328"
---
# <a name="pipeline-access-to-streaming-resources"></a>Acceso de canalización a recursos de streaming


Los recursos de streaming se pueden usar en las vistas de recursos del sombreador (SRV), las vistas de destino de representación (RTV), las vistas de estarcido de profundidad (DSV) y las vistas de acceso desordenado (UAV), así como algunos puntos de enlace en los que no se usan vistas, como los enlaces de búfer de vértices. Para obtener la lista de enlaces admitidos, consulte [parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md). Las diversas operaciones de copia D3D también funcionan en recursos de streaming.

Si varias coordenadas del mosaico de una o varias vistas están enlazadas a la misma ubicación de memoria, las lecturas y escrituras de diferentes rutas de acceso a la misma memoria se producirán en un orden no determinista y no repetible de accesos a la memoria.

Si todos los mosaicos detrás de una superficie de acceso a memoria desde un sombreador se asignan a mosaicos únicos, el comportamiento es idéntico en todas las implementaciones a la superficie que tiene el mismo contenido de memoria en un modo no mosaico.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportamiento SRV con iconos sin asignar</a></p></td>
<td align="left"><p>El comportamiento de las lecturas de vista de recursos del sombreador (SRV) que impliquen mosaicos no asignados depende del nivel de compatibilidad de hardware.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportamiento UAV con iconos sin asignar</a></p></td>
<td align="left"><p>El comportamiento de las lecturas y escrituras de la vista de acceso desordenado (UAV) depende del nivel de compatibilidad de hardware.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportamiento de rasterizador con iconos sin asignar</a></p></td>
<td align="left"><p>En esta sección se describe el comportamiento de rasterizador con mosaicos no asignados.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Limitaciones de acceso de iconos con asignaciones duplicadas</a></p></td>
<td align="left"><p>Existen limitaciones en el acceso a los iconos con las asignaciones duplicadas, como cuando se copian recursos de streaming con el origen y el destino superpuestos, o cuando se representan en mosaicos compartidos en el área de representación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Características de muestreo de texturas de recursos de streaming</a></p></td>
<td align="left"><p>Recursos de streaming las características de muestreo de textura incluyen la obtención de comentarios sobre el estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se tiene acceso se han asignado en el recurso, la fijación a los sombreadores de ayuda evita áreas en los recursos de streaming de mipmapped que se sabe que no están asignados y la detección de lo que el</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposición de recursos de streaming de HLSL</a></p></td>
<td align="left"><p>Se requiere una sintaxis específica de lenguaje de sombreado de alto nivel (HLSL) de Microsoft para admitir recursos de streaming en el <a href="/windows/desktop/direct3dhlsl/d3d11-graphics-reference-sm5">modelo de sombreador 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 