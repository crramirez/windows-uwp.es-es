---
title: Cálculos de iluminación
description: El modelo de luz de Direct3D cubre la iluminación de ambiente, difusa, especular y de emisión. Es suficiente flexibilidad para resolver una gran variedad de situaciones de iluminación. La cantidad total de luz en una escena se conoce como iluminación global.
ms.assetid: D0521F56-050D-4EDF-9BD1-34748E94B873
keywords:
- Cálculos de iluminación
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 19734964c9b4ab087f7d5fd6ea749b57cccce26c
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6990806"
---
# <a name="mathematics-of-lighting"></a>Cálculos de iluminación


El modelo de luz de Direct3D cubre la iluminación de ambiente, difusa, especular y de emisión. Es suficiente flexibilidad para resolver una gran variedad de situaciones de iluminación. La cantidad total de luz en una escena se conoce como *iluminación global*.

La iluminación global se calcula del siguiente modo:

```
Global Illumination = Ambient Light + Diffuse Light + Specular Light + Emissive Light 
```

[La luz de ambiente](ambient-lighting.md) es iluminación constante. La luz ambiente es constante en todas las direcciones y colorea todos los píxeles de un objeto de la misma manera. Se calcula con rapidez, pero deja objetos con una apariencia plana y poco realista.

[La iluminación difusa](diffuse-lighting.md) depende de la dirección de la luz y la superficie normal del objeto. La iluminación difusa varía por la superficie de un objeto como resultado de los cambios de la dirección de la luz y el vector numeral de la superficie. Lleva más tiempo calcular la iluminación difusa porque cambia para cada vértice del objeto. Sin embargo, la ventaja de usarla es que sombrea objetos y les da profundidad tridimensional (3D).

[La iluminación especular](specular-lighting.md) identifica los resaltados especulares brillantes que se producen cuando la luz llega a una superficie del objeto y se refleja hacia la cámara. La iluminación especular es más intensa que la luz difusa y sale más rápidamente por la superficie del objeto. Lleva más tiempo calcular la iluminación especular que la iluminación difusa. Sin embargo, la ventaja de usarla es que agrega detalles importantes a una superficie.

[La iluminación de emisión](emissive-lighting.md) es la luz que emite un objeto; por ejemplo, un resplandor. La emisión hace que un objeto representado parezca que sea luminoso por sí mismo. La emisión afecta al color de un objeto y, por ejemplo, puede hacer que un material oscuro sea más brillante y absorba parte del color emitido.

La iluminación realista puede realizarse mediante la aplicación de cada uno de estos tipos de iluminación en una escena 3D. Los valores calculados para los componentes de ambiente, de emisión y difuso se generan como color de vértice difuso; el valor del componente de iluminación especular se genera como color de vértice especular. Un factor de foco de luz y de atenuación de una luz determinada pueden afectar a los valores de luz de ambiente, difusa y especular. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).

Para lograr un efecto de iluminación más realista, agrega más luces. Sin embargo, la escena tarda más en representarse. Para obtener todos los efectos que quiere un diseñador, algunos juegos usan más capacidad de la CPU de la que normalmente está disponible. En este caso, es habitual reducir el número de cálculos de iluminación al mínimo mediante el uso de mapas de luz y mapas de entorno para agregar iluminación a una escena mientras se usan mapas de texturas.

La iluminación se calcula en el espacio de cámara. Consulta [Transformaciones del espacio de cámara](camera-space-transformations.md). Se puede calcular la iluminación optimizada en el espacio de modelo, cuando se dan condiciones especiales: ya están normalizados vectores normales, no es necesaria la combinación de vértices y las matrices de transformación son ortogonales.

Todos los cálculos de iluminación se realizan en el espacio de modelo al transformar la posición y la dirección de la fuente de luz, junto con la posición de cámara, al espacio de modelo con la inversa de la matriz de mundo. Como resultado, si las matrices de vista o mundo presentan un ajuste de escala no uniforme, la iluminación resultante podría ser incorrecta.

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
<td align="left"><p><a href="ambient-lighting.md">Iluminación de ambiente</a></p></td>
<td align="left"><p>La luz de ambiente proporciona iluminación constante a una escena. Ilumina todos los vértices de un objeto del mismo modo porque no depende de ningún otro factor de iluminación, como las normales de los vértices, la dirección de la luz, la posición de la luz, el alcance o la atenuación. La luz ambiente es constante en todas las direcciones y colorea todos los píxeles de un objeto de la misma manera. Se calcula con rapidez, pero deja objetos con una apariencia plana y poco realista.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="diffuse-lighting.md">Luz difusa</a></p></td>
<td align="left"><p><em>La iluminación difusa</em> depende de la dirección de la luz y la superficie normal del objeto. La iluminación difusa varía por la superficie de un objeto como resultado de los cambios de la dirección de la luz y el vector numeral de la superficie. Lleva más tiempo calcular la iluminación difusa porque cambia para cada vértice del objeto. Sin embargo, la ventaja de usarla es que sombrea objetos y les da profundidad tridimensional (3D).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="specular-lighting.md">Luz especular</a></p></td>
<td align="left"><p><em>La iluminación especular</em> identifica los resaltados especulares brillantes que se producen cuando la luz alcanza la superficie de un objeto y se refleja hacia la cámara. La iluminación especular es más intensa que la luz difusa y sale más rápidamente por la superficie del objeto. Lleva más tiempo calcular la iluminación especular que la iluminación difusa. Sin embargo, la ventaja de usarla es que agrega detalles importantes a una superficie.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="emissive-lighting.md">Luz de emisión</a></p></td>
<td align="left"><p><em>La iluminación de emisión</em> es la luz que emite un objeto; por ejemplo, un resplandor. La emisión hace que un objeto representado parezca que sea luminoso por sí mismo. La emisión afecta al color de un objeto y, por ejemplo, puede hacer que un material oscuro sea más brillante y absorba parte del color emitido.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="camera-space-transformations.md">Transformaciones del espacio de cámara</a></p></td>
<td align="left"><p>Los vértices del espacio de cámara se calculan al transformar los vértices del objeto con la matriz de vista del mundo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="attenuation-and-spotlight-factor.md">Atenuación y factor de foco de luz</a></p></td>
<td align="left"><p>Los componentes de iluminación difusa y especular de la ecuación global de iluminación contienen términos que describen la atenuación de la luz y el cono del foco de luz.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Luces y materiales](lights-and-materials.md)

 

 




