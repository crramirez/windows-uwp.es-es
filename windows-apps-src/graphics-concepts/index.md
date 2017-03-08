---
title: "Guía de aprendizaje de gráficos de Direct3D"
description: "Describe los conceptos de gráficos en los que se basa Microsoft Direct3D."
ms.assetid: c3850a92-4d05-4f72-bf0f-6a0c79e841eb
keywords:
- "Guía de aprendizaje de gráficos de Direct3D"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e62f9cfde35580dd384ef69fe6e5658d927ce3d8
ms.lasthandoff: 02/07/2017

---

# <a name="direct3d-graphics-learning-guide"></a>Guía de aprendizaje de gráficos de Direct3D


Describe los conceptos de gráficos en los que se basa Microsoft Direct3D. Este conjunto de documentación no depende en gran parte de ninguna versión de Direct3D y está destinado al desarrollador de gráficos que necesita más información general de la que se proporciona en la documentación de API específica de la versión.

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
<td align="left"><p>[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)</p></td>
<td align="left"><p>Es necesario estar familiarizado con el trabajo con principios geométricos 3D para programar aplicaciones de Direct3D. En esta sección se presentan los conceptos geométricos más importantes para la creación de escenas 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Búferes de vértices e índices](vertex-and-index-buffers.md)</p></td>
<td align="left"><p><em>Los búferes de vértices</em> son búferes de memoria que contienen datos de vértice. Los vértices de un búfer se procesan para transformar, iluminar y recortar. <em>Los búferes de índices</em> son búferes de memoria que contienen datos de índice, que son desplazamientos de enteros en búferes de vértices y se usan para representar primitivos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Dispositivos](devices.md)</p></td>
<td align="left"><p>Un dispositivo de Direct3D es el componente de representación de Direct3D. Un dispositivo encapsula y almacena el estado de representación, transforma, ilumina y rasteriza una imagen en una superficie.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Iluminación](lights-and-materials.md)</p></td>
<td align="left"><p>Las luces se usan para iluminar objetos de una escena. El color del vértice de cada objeto se basa en el mapa de texturas actual, los colores del vértice y las fuentes de luz.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Búferes de profundidad y de galerías de símbolos](depth-and-stencil-buffers.md)</p></td>
<td align="left"><p>Un <em>búfer de profundidad</em> almacena la información de profundidad para controlar qué áreas de polígonos se representan y no se ocultan de la vista. Un <em>búfer de la galería de símbolos</em> se usa para enmascarar píxeles en una imagen para producir efectos especiales, como la composición, el calcado, la disolución, la atenuación, el barrido; contornos, siluetas o la galería de símbolos a doble cara.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Texturas](textures.md)</p></td>
<td align="left"><p>Las texturas son una herramienta eficaz para crear realismo en imágenes 3D generadas por el equipo. Direct3D admite un amplio conjunto de características de texturas y ofrece a los desarrolladores acceso fácil a técnicas avanzadas de texturas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Canalización de gráficos](graphics-pipeline.md)</p></td>
<td align="left"><p>La canalización de gráficos de Direct3D está diseñada para generar gráficos para aplicaciones de juegos en tiempo real. Los datos fluyen de la entrada a la salida por cada una de las fases configurables o programables.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vistas](views.md)</p></td>
<td align="left"><p>El término &quot;vista&quot; se usa para referirse a los &quot;datos en el formato necesario&quot;. Por ejemplo, una vista de búfer de constantes (CBV) serían los datos de búfer de constantes con el formato correcto. En esta sección se describen las vistas más comunes y útiles.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Canalización del proceso](compute-pipeline.md)</p></td>
<td align="left"><p>La canalización del proceso de Direct3D está diseñada para controlar los cálculos que se pueden llevar a cabo principalmente en paralelo con la canalización de gráficos.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Recursos](resources.md)</p></td>
<td align="left"><p>Un recurso es un área en la memoria a la que puede obtener acceso la canalización de Direct3D. Para que la canalización pueda obtener acceso a la memoria de forma eficiente, los datos que se proporcionan a la canalización (por ejemplo, geometría de entrada, recursos del sombreador y texturas) deben almacenarse en un recurso. Hay dos tipos de recursos de los que derivan todos los recursos de Direct3D: un búfer o una textura. Se pueden activar hasta 128 recursos para cada fase de la canalización.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Recursos de streaming](streaming-resources.md)</p></td>
<td align="left"><p><em>Los recursos de streaming</em> son grandes recursos lógicos que usan pequeñas cantidades de memoria física. En lugar de pasar un recurso completo grande, se envían pequeñas partes del recurso según sea necesario. Los recursos de streaming se conocían anteriormente como <em>recursos en mosaico</em>.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Anexos](appendix.md)</p></td>
<td align="left"><p>Estas secciones proporcionan información técnica detallada.</p></td>
</tr>
</tbody>
</table>

 

 

 

