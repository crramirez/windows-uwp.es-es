---
title: Luz difusa
description: La iluminación difusa depende de la dirección de la luz y la superficie normal del objeto.
ms.assetid: 8AF78742-76B1-4BBB-86E3-94AE6F48B847
keywords:
- Luz difusa
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5846edda167823b7ae161d332fbde450ccf20d72
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5874437"
---
# <a name="diffuse-lighting"></a>Luz difusa


*La iluminación difusa* depende de la dirección de la luz y la superficie normal del objeto. La iluminación difusa varía por la superficie de un objeto como resultado de los cambios de la dirección de la luz y el vector numeral de la superficie. Lleva más tiempo calcular la iluminación difusa porque cambia para cada vértice del objeto. Sin embargo, la ventaja de usarla es que sombrea objetos y les da profundidad tridimensional (3D).

Después de ajustar la intensidad de la luz para los efectos de atenuación, el motor de iluminación calcula la cantidad de luz restante que se refleja de un vértice, dado el ángulo de la normal del vértice y la dirección de la luz incidente. El motor de iluminación omite este paso para las luces direccionales porque no se atenúan a distancia. El sistema considera dos tipos de reflexión, difusa y especular, y usa una fórmula diferente para determinar la cantidad de luz que se refleja para cada uno.

Después de calcular la cantidad de luz reflejada, Direct3D aplica estos nuevos valores a las propiedades de reflectancia difusa y especular del material actual. Los valores de color resultantes son los componentes difusos y especulares que usa el rasterizador para producir el sombreado Gouraud y resaltado especular.

La iluminación difusa se describe en la siguiente ecuación.

Iluminación difusa = sum\[C<sub>d</sub>\*L<sub>d</sub>\*(N<sup>.</sup>L<sub>dir</sub>)\*Atten\*Spot\]

| Parámetro       | Valor predeterminado | Tipo          | Descripción                                                                                      |
|-----------------|---------------|---------------|--------------------------------------------------------------------------------------------------|
| sum             | N/A           | N/A           | Suma de cada componente difuso de luz.                                                     |
| C<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Color difuso.                                                                                   |
| L<sub>d</sub>   | (0,0,0,0)     | D3DCOLORVALUE | Color difuso claro.                                                                             |
| N               | N/A           | D3DVECTOR     | Normal del vértice                                                                                    |
| L<sub>dir</sub> | N/A           | D3DVECTOR     | Vector de dirección del vértice del objeto a la luz.                                                |
| Atten           | N/A           | FLOAT         | Atenuación de la luz. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md). |
| Spot            | N/A           | FLOAT         | Factor de foco de luz Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).  |

 

Para calcular la atenuación (Atten) o las características de foco de luz (Spot), consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).

Los componentes difusos se restringen para estar entre 0 y 255, después de que todas las luces se procesen e interpolen por separado. El valor de iluminación difuso resultante es una combinación de los valores de luz ambiental, difusa y de emisor.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


En este ejemplo, el objeto se colorea con el color difuso de la luz y un color difuso del material.

Según la ecuación, el color resultante para los vértices del objeto es una combinación del color del material y del color de la luz.

Las dos ilustraciones siguientes muestran el color del material, que es gris, y el color de la luz, que es rojo brillante.

![ilustración de una esfera gris](images/amb1.jpg)![ilustración de una esfera roja](images/lightred.jpg)

En la siguiente ilustración se muestra la escena resultante. El único objeto en la escena es una esfera. El cálculo de la iluminación difusa toma el material y el color difuso de luz y los modifica por el ángulo entre la dirección de la luz y la normal del vértice con el producto escalar. Como resultado, el reverso de la esfera oscurece a medida que la superficie de la esfera se aleja de la luz al curvarse.

![ilustración de una esfera con luz difusa](images/lightd.jpg)

La combinación de la luz difusa con la ambiente del ejemplo anterior sombrea toda la superficie del objeto. La luz ambiente sombrea toda la superficie y la luz difusa ayuda a revelar la forma 3D del objeto, como se muestra en la siguiente ilustración.

![ilustración de una esfera con iluminación difusa y ambiente](images/lightad.jpg)

La iluminación difusa cuesta más de calcular que la iluminación ambiental. Dado que depende de la normales del vértice y de la dirección de la luz, puedes ver la geometría de los objetos en el espacio 3D, lo que produce una iluminación más realista que la luz ambiental. Puedes usar resaltados especulares para lograr una apariencia más realista.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




