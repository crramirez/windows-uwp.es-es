---
title: Luz especular
description: La luz especular identifica la iluminación especular que se produce cuando la luz alcanza la superficie de un objeto y vuelve a reflejarse hacia la cámara.
ms.assetid: 71F87137-B00F-48CE-8E6A-F98A139A742A
keywords:
- Luz especular
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 283ea63d118f9a61fe745dd3eb60b68594c32279
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7129005"
---
# <a name="specular-lighting"></a>Luz especular


*La iluminación especular* identifica los resaltados especulares brillantes que se producen cuando la luz alcanza la superficie de un objeto y se refleja hacia la cámara. La iluminación especular es más intensa que la luz difusa y sale más rápidamente por la superficie del objeto. Se tarda más en calcular la luz especular que la luz difusa; sin embargo, la ventaja de usarla es que agrega un nivel importante de detalle a una superficie.

El modelado del reflejo especular requiere que el sistema sepa en qué dirección se desplaza la luz y la dirección respecto a los ojos del espectador. El sistema usa una versión simplificada del modelo de reflejo especular Phong, que emplea un vector medio para aproximar la intensidad de dicho reflejo especular.

El estado predeterminado de la luz no calcula la iluminación especular.

## <a name="span-idspecularlightingequationspanspan-idspecularlightingequationspanspan-idspecularlightingequationspanspecular-lighting-equation"></a><span id="Specular_Lighting_Equation"></span><span id="specular_lighting_equation"></span><span id="SPECULAR_LIGHTING_EQUATION"></span>Ecuación de la luz especular


La luz especular se describe mediante la siguiente ecuación.

|                                                                             |
|-----------------------------------------------------------------------------|
| Luz especular = Cₛ \ * sum\ [Lₛ \ * (N · (H)<sup>P</sup> \ * Atten \ * Spot\] |

 

Las variables, sus tipos y sus intervalos son los siguientes:

| Parámetro    | Valor predeterminado | Tipo                                                             | Descripción                                                                                            |
|--------------|---------------|------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------|
| Cₛ           | (0,0,0,0)     | Transparencia del rojo, el verde, el azul y el alfa (valores de punto flotante) | Color especular.                                                                                        |
| sum          | N/A           | N/A                                                              | La suma del componente especular de cada luz.                                                          |
| N            | N/A           | Vector 3D (valores de punto flotante de x, y y z)                    | Vértice normal.                                                                                         |
| H            | N/A           | Vector 3D (valores de punto flotante de x, y y z)                    | Vector medio. Consulta la sección sobre el vector medio.                                                |
| <sup>P</sup> | 0.0           | Punto flotante                                                   | La potencia del reflejo especular. El intervalo es de 0 a +infinity.                                                     |
| Lₛ           | (0,0,0,0)     | Transparencia del rojo, el verde, el azul y el alfa (valores de punto flotante) | El color especular de la luz.                                                                                  |
| Atten        | N/A           | Punto flotante                                                   | El valor de atenuación de la luz. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md). |
| Spot         | N/A           | Punto flotante                                                   | El factor de foco de luz. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).        |

 

El valor de Cₛ es uno de los siguientes:

-   El color del vértice 1, si el origen del material especular es el color del vértice difuso y el primer color del vértice se suministra en la declaración de vértices.
-   El color del vértice 2, si el origen del material especular es el color del vértice especular y el segundo color del vértice se suministra en la declaración de vértices.
-   El color del material especular.

**Nota**  si se usa cualquier opción de origen del material especular y no se proporciona el color del vértice, se usa el color del material especular.

 

Los componentes especulares se comprimen para ser de 0 a 255 después de que todas las luces se procesen y se interpolen por separado.

## <a name="span-idthehalfwayvectorspanspan-idthehalfwayvectorspanspan-idthehalfwayvectorspanthe-halfway-vector"></a><span id="The_Halfway_Vector"></span><span id="the_halfway_vector"></span><span id="THE_HALFWAY_VECTOR"></span>El vector medio


El vector medio (H) se encuentra a mitad de camino de dos vectores: el vector del vértice de un objeto respecto a la fuente de luz y el vector del vértice de un objeto respecto a la posición de cámara. Direct3D proporciona dos formas de calcular el vector medio. Cuando se habilita la iluminación especular relativa a la cámara (en lugar de la iluminación especular ortogonal), el sistema calcula el vector medio usando la posición de la cámara y la posición del vértice, junto con el vector de dirección de la luz. En la fórmula siguiente se ilustra esto.

|                                           |
|-------------------------------------------|
| H = norm(norm(Cₚ - Vₚ) + L<sub>dir</sub>) |

 

| Parámetro       | Valor predeterminado | Tipo                                          | Descripción                                                  |
|-----------------|---------------|-----------------------------------------------|--------------------------------------------------------------|
| Cₚ              | N/A           | Vector 3D (valores de punto flotante de x, y y z) | La posición de la cámara.                                             |
| Vₚ              | N/A           | Vector 3D (valores de punto flotante de x, y y z) | La posición del vértice.                                             |
| L<sub>dir</sub> | N/A           | Vector 3D (valores de punto flotante de x, y y z) | El vector de dirección de la posición del vértice respecto a la posición de la luz. |

 

Determinar el vector medio de esta manera puede suponer un alto número de cálculos. Como alternativa, el uso de la iluminación especular ortogonal (en lugar de la iluminación especular relativa a la cámara) le indica al sistema que se comporte como si el punto de vista esté infinitamente distante en el eje z. Esto se refleja en la siguiente fórmula.

|                                     |
|-------------------------------------|
| H = norm((0,0,1) + L<sub>dir</sub>) |

 

Esta configuración supone un menor número de cálculos, pero resulta mucho menos precisa, por lo que su uso recomendado es en aplicaciones que utilicen la proyección ortogonal.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo:


En este ejemplo, se aplica color al objeto mediante el color de la luz especular de la escena y un color especular del material.

Según la ecuación, el color resultante para los vértices del objeto es una combinación del color del material y el color de la luz.

La dos ilustraciones siguientes muestran el color del material especular, que es gris, y el color de la luz especular, que es blanco.

![Ilustración de una esfera gris](images/amb1.jpg)![Ilustración de una esfera blanca](images/lightwhite.jpg)

El reflejo especular resultante se muestra en la ilustración siguiente.

![Ilustración del reflejo especular](images/lights.jpg)

La combinación de la iluminación especular con la luz ambiental y la luz difusa produce la siguiente ilustración. Cuando se aplican los tres tipos de luz, se parece mucho más a un objeto realista.

![Ilustración de la combinación de la iluminación especular, la luz ambiental y la luz difusa](images/lightads.jpg)

La luz difusa supone más cálculos que la luz especular. Normalmente se usa para proporcionar pistas visuales sobre el material de la superficie. La luz especular varía en tamaño y color según el material de la superficie.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




