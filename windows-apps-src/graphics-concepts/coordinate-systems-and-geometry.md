---
title: "Sistemas de coordenadas y geometría"
description: "Es necesario estar familiarizado con el trabajo con principios geométricos 3D para programar aplicaciones de Direct3D. En esta sección se presentan los conceptos geométricos más importantes para la creación de escenas 3D."
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- "Sistemas de coordenadas y geometría"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 655e8587d103843bf2e040519b60f82160bc7b5d
ms.lasthandoff: 02/07/2017

---

# <a name="coordinate-systems-and-geometry"></a>Sistemas de coordenadas y geometría


Es necesario estar familiarizado con el trabajo con principios geométricos 3D para programar aplicaciones de Direct3D. En esta sección se presentan los conceptos geométricos más importantes para la creación de escenas 3D.

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
<td align="left"><p>[Sistemas de coordenadas](coordinate-systems.md)</p></td>
<td align="left"><p>Por lo general, las aplicaciones de gráficos 3D usan uno de dos tipos de sistemas de coordenadas cartesianas: diestro o zurdo. En ambos sistemas de coordenadas, el eje x positivo apunta a la derecha y el eje y positivo apunta arriba.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Primitivos](primitives.md)</p></td>
<td align="left"><p>Un <em>primitivo</em> 3D es una colección de vértices que forma una sola entidad 3D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Vectores normales de caras y vértices](face-and-vertex-normal-vectors.md)</p></td>
<td align="left"><p>Cada cara de una malla tiene un vector normal a la unidad. La dirección del vector está determinada por el orden en que se definen los vértices y por si el sistema de coordenadas es diestro o zurdo.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Rectángulos](rectangles.md)</p></td>
<td align="left"><p>En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores. Los lados de un rectángulo delimitador siempre son paralelos a los lados de la pantalla, por lo que se puede describir el rectángulo por dos puntos, la esquina superior izquierda y la esquina inferior derecha.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interpolación de triángulos](triangle-interpolation.md)</p></td>
<td align="left"><p>Durante la representación, la canalización interpola los datos de vértices en cada triángulo. Los datos de vértice pueden ser una amplia variedad de datos y pueden incluir, entre otros, color, color especular, alfa difuso (opacidad del triángulo), alfa especular y un factor de niebla.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Vectores, vértices y cuaterniones](vectors--vertices--and-quaternions.md)</p></td>
<td align="left"><p>En Direct3D, los vértices describen la posición y la orientación. Cada vértice de un primitivo se describe mediante un vector que proporciona la posición, el color, las coordenadas de textura y un vector normal que da su orientación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Transformaciones](transforms.md)</p></td>
<td align="left"><p>La parte de Direct3D que inserta geometría a través de la canalización de geometría de función fija es el motor de la transformación. Localiza el modelo y el visor en el mundo, proyecta vértices para mostrar en la pantalla y sujeta vértices a la ventanilla. El motor de transformación también realiza cálculos de iluminación para determinar los componentes de difusión y resaltados especulares en cada vértice.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Ventanillas y recortes](viewports-and-clipping.md)</p></td>
<td align="left"><p>Una <em>ventanilla</em> es un rectángulo bidimensional (2D) en el que se proyecta una escena 3D. En Direct3D, el rectángulo existe como coordenadas dentro de una superficie de Direct3D que el sistema usa como destino de representación. La transformación de proyección convierte los vértices en el sistema de coordenadas usado para la ventanilla. Una ventanilla también se usa para especificar el intervalo de valores de profundidad en una superficie de destino de representación en la que se representará una escena (normalmente 0,0 a 1,0).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 





