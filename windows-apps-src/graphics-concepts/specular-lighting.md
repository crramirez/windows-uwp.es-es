---
title: Luz especular
description: La iluminación especular identifica los reflejos especulares brillantes que se producen cuando la luz alcanza una superficie de objeto y se refleja de nuevo hacia la cámara.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Luz especular
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f60bd4019f330058d4396a5b0d75d00f90ecff09
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750191"
---
# <a name="specular-lighting"></a>Luz especular


La *iluminación especular* identifica los reflejos especulares brillantes que se producen cuando la luz alcanza una superficie de objeto y se refleja de nuevo hacia la cámara. La iluminación especular es más intensa que la luz difusa y se cae más rápidamente en la superficie del objeto. Se tarda más tiempo en calcular la iluminación especular que la iluminación difusa; sin embargo, la ventaja de utilizarla es que agrega un detalle significativo a una superficie.

El modelado de reflexión especular requiere que el sistema sepa en qué dirección se desplaza la luz y la dirección del ojo del espectador. El sistema utiliza una versión simplificada del modelo de reflexión especular de Phong, que emplea un vector a la mitad para aproximar la intensidad de reflexión especular.

El estado de iluminación predeterminado no calcula los reflejos especulares.

## <a name="span-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspan-idspecular_lighting_equationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Ecuación de iluminación especular


La iluminación especular se describe mediante la siguiente ecuación.

> Iluminación especular = CS \* SUM \[ LS \* (N · H)<sup>P</sup> \* ATTEN \*\]

Las variables, sus tipos y sus intervalos son los siguientes:

| Parámetro    | Valor predeterminado | Tipo                                                             | Descripción                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Estrategia           | (0, 0, 0, 0)     | Transparencia roja, verde, azul y alfa (valores de punto flotante) | Color especular.                                                                                        |
| Sum          | N/D           | N/D                                                              | Suma del componente especular de cada luz.                                                          |
| N            | N/D           | Vector 3D (valores de punto flotante x, y y z)                    | El vértice es normal.                                                                                         |
| H            | N/D           | Vector 3D (valores de punto flotante x, y y z)                    | Vector medio. Vea la sección sobre el vector a la mitad.                                                |
| <sup>P</sup> | 0,0           | Punto flotante                                                   | Potencia de reflexión especular. El intervalo es de 0 a + infinito                                                     |
| LS           | (0, 0, 0, 0)     | Transparencia roja, verde, azul y alfa (valores de punto flotante) | Color especular claro.                                                                                  |
| Atten        | N/D           | Punto flotante                                                   | Valor de atenuación de luz. Vea [atenuación y factor destacado](attenuation-and-spotlight-factor.md). |
| Zona         | N/D           | Punto flotante                                                   | Factor destacado. Vea [atenuación y factor destacado](attenuation-and-spotlight-factor.md).        |

 

El valor de CS es:

-   color 1 de vértice, si el origen de material especular es el color de vértice difuso, y el primer color de vértice se proporciona en la declaración de vértices.
-   color de vértice 2, si el origen de material especular es el color de vértice especular y el segundo color de vértice se proporciona en la declaración de vértices.
-   color especular de material

**Nota:**    Si se usa la opción de origen de material especular y no se proporciona el color de vértice, se usa el color especular de material.

 

Los componentes especulares están fijados de 0 a 255, después de que todas las luces se procesan y se interpolan por separado.

## <a name="span-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanspan-idthe_halfway_vectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>Vector en la mitad


El vector medio (H) existe entre dos vectores: el vector de un vértice de objeto a la fuente de luz y el vector de un vértice de objeto a la posición de la cámara. Direct3D proporciona dos maneras de calcular el vector a la mitad. Cuando se habilita el resaltado especular relativo a la cámara (en lugar de los reflejos especulares ortogonales), el sistema calcula el vector en la mitad con la posición de la cámara y la posición del vértice, junto con el vector de dirección de la luz. La siguiente fórmula ilustra esto.

> H = norma (Norm (CP-VP) + L<sub>dir</sub>)

 

| Parámetro       | Valor predeterminado | Tipo                                          | Descripción                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| CP              | N/D           | Vector 3D (valores de punto flotante x, y y z) | Posición de la cámara.                                             |
| Expertos              | N/D           | Vector 3D (valores de punto flotante x, y y z) | Posición del vértice.                                             |
| <sub>Directorio</sub> L | N/D           | Vector 3D (valores de punto flotante x, y y z) | Vector de dirección desde la posición del vértice hasta la posición de la luz. |

 

Determinar el vector a la mitad de esta manera puede consumir un uso intensivo de los cálculos. Como alternativa, el uso de los reflejos especulares ortogonales (en lugar de los reflejos especulares relativos a la cámara) indica al sistema que actúe como si el punto de vista se distan infinitamente en el eje z. Esto se refleja en la siguiente fórmula.

> H = normal ((0, 0, 1) + L<sub>dir</sub>)

Este valor es menos computacionalmente intensivo, pero es mucho menos preciso, por lo que es mejor para las aplicaciones que usan la proyección ortogonal.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


En este ejemplo, el objeto se colorea utilizando el color de luz especular de la escena y un color especular de material.

Según la ecuación, el color resultante para los vértices del objeto es una combinación del color del material y el color claro.

En las dos ilustraciones siguientes se muestra el color del material especular, que está atenuado, y el color de luz especular, que es blanco.

![Ilustración de una esfera gris](images/amb1.jpg)![Ilustración de una esfera blanca](images/lightwhite.jpg)

El resaltado especular resultante se muestra en la siguiente ilustración.

![Ilustración del resaltado especular](images/lights.jpg)

La combinación del resaltado especular con el ambiente y la iluminación difusa produce la siguiente ilustración. Con los tres tipos de iluminación aplicados, esto se parece más claramente a un objeto realista.

![Ilustración de la combinación del resaltado especular, la iluminación ambiente y la iluminación difusa](images/lightads.jpg)

La iluminación especular es más intensiva para calcular que la luz difusa. Normalmente se usa para proporcionar pistas visuales sobre el material de la superficie. El resaltado especular varía en tamaño y color con el material de la superficie.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




