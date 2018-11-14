---
title: Acceso de canalización a recursos de streaming
description: Los recursos de streaming se pueden utilizar en vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (DSV) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se utilizan vistas, como los enlaces de búfer de vértices.
ms.assetid: 18DA5D61-930D-4466-8EFE-0CED566EA4A6
keywords:
- Acceso de canalización a recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f6f777669959721405fc5c77ef134e3726291b9c
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6254982"
---
# <a name="pipeline-access-to-streaming-resources"></a>Acceso de canalización a recursos de streaming


Los recursos de streaming se pueden utilizar en vistas de recurso de sombreador (SRV), vistas de destino de representación (RTV), vistas de galería de símbolos de profundidad (DSV) y vistas de acceso sin ordenar (UAV), así como en algunos puntos de enlace donde no se utilizan vistas, como los enlaces de búfer de vértices. Para obtener la lista de los enlaces compatibles, consulta [Parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md). Las distintas operaciones de copia de D3D también funcionan con los recursos de streaming.

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
<td align="left"><p><a href="srv-behavior-with-non-mapped-tiles.md">Comportamiento SRV con iconos sin asignar</a></p></td>
<td align="left"><p>El comportamiento de las lecturas de vistas de recurso de sombreador (SRV) que implican iconos no asignados depende del nivel de compatibilidad del hardware.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="uav-behavior-with-non-mapped-tiles.md">Comportamiento UAV con iconos sin asignar</a></p></td>
<td align="left"><p>El comportamiento de las lecturas y escrituras de las vistas de acceso sin ordenar (UAV) dependen del nivel de compatibilidad del hardware.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="rasterizer-behavior-with-non-mapped-tiles.md">Comportamiento de rasterizador con iconos sin asignar</a></p></td>
<td align="left"><p>En esta sección se describe un comportamiento de rasterizador con iconos sin asignar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tile-access-limitations-with-duplicate-mappings.md">Limitaciones de acceso de iconos con asignaciones duplicadas</a></p></td>
<td align="left"><p>Existen limitaciones sobre el acceso de iconos con asignaciones duplicadas, como al copiar recursos de streaming con el origen y el destino superpuestos, o al realizar representaciones en iconos compartidos dentro del área de representación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources-texture-sampling-features.md">Características de muestreo de texturas de recursos de streaming</a></p></td>
<td align="left"><p>Las características de muestreo de texturas de recursos de streaming incluyen obtener comentarios de estado del sombreador sobre las áreas asignadas, comprobar si todos los datos a los que se accede estaban asignados al recurso, realizar la compresión para ayudar a los sombreadores a evitar áreas en recursos de streaming con mapas MIP de los que se sabe que no están asignados, y detectar cuál será el LOD mínimo asignado completamente a toda una superficie de filtro de textura.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="hlsl-streaming-resources-exposure.md">Exposición de recursos de streaming de HLSL</a></p></td>
<td align="left"><p>Se necesita una sintaxis específica del lenguaje High Level Shader Language (HLSL) de Microsoft para admitir los recursos de streaming en <a href="https://msdn.microsoft.com/library/windows/desktop/ff471356">Shader Model 5</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




