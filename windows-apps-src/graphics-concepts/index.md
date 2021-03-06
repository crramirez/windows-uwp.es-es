---
title: Glosario de gráficos Direct3D
description: Consulte un glosario en el que se definen a grandes rasgos los términos generales de gráficos 3D informatizados que se utilizan en el desarrollo de juegos y aplicaciones de Direct3D.
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- Glosario de gráficos Direct3D
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3359b81c6932bfc63c3939d2f857aaef9248e52f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173079"
---
# <a name="direct3d-graphics-glossary"></a>Glosario de gráficos Direct3D


Define los términos de gráficos de Microsoft Direct3D. En este glosario se definen a grandes rasgos los términos generales de gráficos 3D informatizados que se utilizan en el desarrollo de juegos y aplicaciones de Direct3D.

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
<td align="left"><p><a href="coordinate-systems-and-geometry.md">Sistemas de coordenadas y geometría</a></p></td>
<td align="left"><p>Es necesario estar familiarizado con el trabajo con principios geométricos 3D para programar aplicaciones de Direct3D. En esta sección se presentan los conceptos geométricos más importantes para la creación de escenas 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vertex-and-index-buffers.md">Búferes de vértices e índices</a></p></td>
<td align="left"><p>Los <em>búferes de vértices</em> son búferes de memoria que contienen datos de vértice; los vértices de un búfer se procesan para realizar transformaciones, iluminaciones y recortes. Los <em>búferes de índices</em> son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices, y se usan para representar primitivos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="devices.md">Dispositivos</a></p></td>
<td align="left"><p>Un dispositivo de Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, realiza transformaciones y operaciones de iluminación y rasteriza una imagen en una superficie.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="lights-and-materials.md">Iluminación</a></p></td>
<td align="left"><p>Las luces se usan para iluminar los objetos de una escena. El color del vértice de cada objeto se basa en el mapa de texturas actual, los colores del vértice y las fuentes de luz.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="depth-and-stencil-buffers.md">Búferes de profundidad y de galerías de símbolos</a></p></td>
<td align="left"><p>Un <em>búfer de profundidad</em> almacena la información de profundidad para controlar qué áreas de polígonos se representan y no se ocultan de la vista. Un <em>búfer de galería de símbolos</em> se usa para enmascarar píxeles de una imagen con la finalidad de producir efectos especiales, como composición, calcado, disolución, atenuación, barrido; contornos, siluetas y galería de símbolos a doble cara.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="textures.md">Texturas</a></p></td>
<td align="left"><p>Las texturas son una herramienta muy eficaz para dotar de realismo a las imágenes en 3D generadas por PC. Direct3D admite un amplio conjunto de características de texturas y ofrece a los desarrolladores acceso fácil a técnicas avanzadas de texturas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="graphics-pipeline.md">Canalización de gráficos</a></p></td>
<td align="left"><p>La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="views.md">Vistas</a></p></td>
<td align="left"><p>El término &quot;vista&quot; se usa para referirse a los &quot;datos en el formato necesario&quot;. Por ejemplo, una vista de búfer de constantes (CBV) serían los datos del búfer de constantes con el formato correcto. En esta sección se describen las vistas más comunes y útiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="compute-pipeline.md">Canalización del proceso</a></p></td>
<td align="left"><p>La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="resources.md">Recursos</a></p></td>
<td align="left"><p>Un recurso es un área en memoria a la que puede acceder la canalización de Direct3D. Para que la canalización pueda acceder a la memoria de forma eficiente, los datos que se proporcionan a la canalización (por ejemplo, geometría de entrada, recursos del sombreador y texturas) deben almacenarse en un recurso. Hay dos tipos de recursos de los que derivan todos los recursos de Direct3D: un búfer o una textura. Se pueden activar hasta 128 recursos para cada fase de la canalización.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="streaming-resources.md">Recursos de streaming</a></p></td>
<td align="left"><p>Los <em>recursos de streaming</em> son grandes recursos lógicos que usan pequeñas cantidades de memoria física. En lugar de pasar un recurso grande completo, se transmiten pequeñas partes del recurso según son necesarias. Los recursos de streaming se conocían anteriormente como <em>recursos en mosaico</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="appendix.md">Apéndices</a></p></td>
<td align="left"><p>Estas secciones proporcionan información técnica detallada.</p></td>
</tr>
</tbody>
</table>

 

 

 
