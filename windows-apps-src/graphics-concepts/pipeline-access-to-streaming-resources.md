---
title: Acceso de canalización a recursos de streaming
description: Los recursos de streaming pueden usarse en las vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (origen de datos) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se usan vistas, como los enlaces de búfer de vértices.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Acceso de canalización a recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6d95ffc14e9ae6d4ea59a4b3bdc33fd215cb61be
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616380"
---
# <a name="pipeline-access-to-streaming-resources"></a>Acceso de canalización a recursos de streaming


Los recursos de streaming pueden usarse en las vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (origen de datos) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se usan vistas, como los enlaces de búfer de vértices. Para obtener la lista de los enlaces compatibles, consulta [Parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md). Las distintas operaciones de copia de D3D también funcionan con los recursos de streaming.

Si varias coordenadas del icono en una o varias vistas están enlazadas a la misma ubicación de memoria, las lecturas y escrituras desde diferentes rutas de acceso en la misma memoria se producirán en un orden no determinista y no reproducible de accesos de memoria.

Si todos los iconos detrás de una superficie de acceso de memoria de un sombreador se asignan a iconos únicos, el comportamiento es idéntico en todas las implementaciones a la superficie que tiene el mismo contenido de memoria que no está en mosaico.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportamiento SRV con iconos no asignados</a></p></td>
<td align="left"><p>El comportamiento de las lecturas de la vista de recurso de sombreador (SRV) que implican iconos sin asignar depende del nivel de compatibilidad del hardware.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportamiento UAV con iconos no asignados</a></p></td>
<td align="left"><p>El comportamiento de las lecturas y escrituras de las vistas de acceso sin ordenar (UAV) dependen del nivel de compatibilidad del hardware.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportamiento del rasterizador con iconos no asignados</a></p></td>
<td align="left"><p>En esta sección se describe un comportamiento de rasterizador con iconos sin asignar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Limitaciones de acceso de mosaico con asignaciones duplicadas</a></p></td>
<td align="left"><p>Existen limitaciones sobre el acceso de iconos con asignaciones duplicadas, como al copiar recursos de streaming con el origen y el destino superpuestos, o al realizar representaciones en iconos compartidos dentro del área de representación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Características de muestreo de textura de los recursos de streaming</a></p></td>
<td align="left"><p>Las características de muestreo de texturas de los recursos de streaming incluyen la obtención de los comentarios de estado del sombreador sobre las áreas asignadas, la comprobación de si todos los datos a los que se accede se han asignado en el recurso, la compresión para ayudar a los sombreadores a evitar áreas de recursos de streaming con mapas MIP que se sabe que no se han asignado y la detección de que el LOD mínimo se ha asignado completamente para la totalidad de una superficie de filtro de texturas.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposición de recursos de streaming de HLSL</a></p></td>
<td align="left"><p>Se necesita una sintaxis específica del lenguaje High Level Shader Language (HLSL) de Microsoft para admitir los recursos de streaming en <a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">Shader Model 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




