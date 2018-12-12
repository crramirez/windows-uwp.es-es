---
title: Sistemas de coordenadas y geometría
description: Es necesario estar familiarizado con el trabajo con principios geométricos 3D para programar aplicaciones de Direct3D. En esta sección se presentan los conceptos geométricos más importantes para la creación de escenas 3D.
ms.assetid: E82EB0A9-0678-496B-96B3-8993BA580099
keywords:
- Sistemas de coordenadas y geometría
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 962002d635c3e6edbf1f9581a4cbc57fbd5b1d96
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8931684"
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
<td align="left"><p><a href="coordinate-systems.md">Sistemas de coordenadas</a></p></td>
<td align="left"><p>Por lo general, las aplicaciones de gráficos 3D usan uno de dos tipos de sistemas de coordenadas cartesianas: diestro o zurdo. En ambos sistemas de coordenadas, el eje x positivo apunta a la derecha y el eje y positivo apunta arriba.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="primitives.md">Primitivos</a></p></td>
<td align="left"><p>Un <em>primitivo</em> 3D es una colección de vértices que forma una sola entidad 3D.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="face-and-vertex-normal-vectors.md">Vectores normales de caras y vértices</a></p></td>
<td align="left"><p>Cada cara de una malla tiene un vector normal a la unidad. La dirección del vector está determinada por el orden en que se definen los vértices y por si el sistema de coordenadas es diestro o zurdo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="rectangles.md">Rectángulos</a></p></td>
<td align="left"><p>En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores. Los lados de un rectángulo delimitador siempre son paralelos a los lados de la pantalla, por lo que se puede describir el rectángulo por dos puntos, la esquina superior izquierda y la esquina inferior derecha.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="triangle-interpolation.md">Interpolación de triángulos</a></p></td>
<td align="left"><p>Durante la representación, la canalización interpola los datos de vértices en cada triángulo. Los datos de vértice pueden ser una amplia variedad de datos y pueden incluir, entre otros, color, color especular, alfa difuso (opacidad del triángulo), alfa especular y un factor de niebla.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="vectors--vertices--and-quaternions.md">Vectores, vértices y cuaterniones</a></p></td>
<td align="left"><p>En Direct3D, los vértices describen la posición y la orientación. Cada vértice de un primitivo se describe mediante un vector que proporciona la posición, el color, las coordenadas de textura y un vector normal que da su orientación.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="transforms.md">Transformaciones</a></p></td>
<td align="left"><p>La parte de Direct3D que inserta geometría a través de la canalización de geometría de función fija es el motor de la transformación. Localiza el modelo y el visor en el mundo, proyecta vértices para mostrar en la pantalla y sujeta vértices a la ventanilla. El motor de transformación también realiza cálculos de iluminación para determinar los componentes de difusión y resaltados especulares en cada vértice.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="viewports-and-clipping.md">Ventanillas y recortes</a></p></td>
<td align="left"><p>Una <em>ventanilla</em> es un rectángulo bidimensional (2D) en el que se proyecta una escena 3D. En Direct3D, el rectángulo existe como coordenadas dentro de una superficie de Direct3D que el sistema usa como destino de representación. La transformación de proyección convierte los vértices en el sistema de coordenadas usado para la ventanilla. Una ventanilla también se usa para especificar el intervalo de valores de profundidad en una superficie de destino de representación en la que se representará una escena (normalmente 0,0 a 1,0).</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Guía de aprendizaje de gráficos de Direct3D](index.md)

 

 




