---
title: Atenuación y factor de foco de luz
description: Los componentes de iluminación difusa y especular de la ecuación global de iluminación contienen términos que describen la atenuación de la luz y el cono del foco de luz.
ms.assetid: F61D4ACB-09AB-4087-9E2D-224E472D6196
keywords:
- Atenuación y factor de foco de luz
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 65b9f6700ddd11c41193820a5247a90c2382c98b
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5815670"
---
# <a name="attenuation-and-spotlight-factor"></a>Atenuación y factor de foco de luz


Los componentes de iluminación difusa y especular de la ecuación global de iluminación contienen términos que describen la atenuación de la luz y el cono del foco de luz. A continuación se describen estos términos.

## <a name="span-idattenuationspanspan-idattenuationspanspan-idattenuationspanattenuation"></a><span id="Attenuation"></span><span id="attenuation"></span><span id="ATTENUATION"></span>Atenuación


La atenuación de una luz depende del tipo de luz y de la distancia entre la luz y la posición del vértice. Para calcular la atenuación, usa una de las ecuaciones siguientes.

Atten = 1/( att0<sub>i</sub> + att1<sub>i</sub> \* d + att2<sub>i</sub> \* d²)

Donde:

| Parámetro        | Valor predeterminado | Tipo           | Descripción                                     | Intervalo          |
|------------------|---------------|----------------|-------------------------------------------------|----------------|
| att0<sub>i</sub> | 0.0           | Punto flotante | Factor de atenuación constante                     | De 0 a +infinito |
| att1<sub>i</sub> | 0.0           | Punto flotante | Factor de atenuación lineal                       | De 0 a +infinito |
| att2<sub>i</sub> | 0.0           | Punto flotante | Factor de atenuación cuadrática                    | De 0 a +infinito |
| d                | N/C           | Punto flotante | Distancia desde la posición del vértice hasta la posición de la luz | N/C            |

 

-   Atten = 1, si la luz es una luz direccional.
-   Atten = 0, si la distancia entre la luz y el vértice supera el intervalo de la luz.

La distancia entre la luz y la posición del vértice siempre es positiva.

Pad = | L<sub>dir</sub> |

Donde:

| Parámetro       | Valor predeterminado | Tipo                                             | Descripción                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dir</sub> | N/C           | Vector 3D con valores de punto flotante x, y, z | Vector de dirección desde la posición del vértice hasta la posición de la luz |

 

Si d es mayor que el intervalo de la luz, Direct3D no hace ningún otro cálculo de atenuación y no aplica ningún efecto desde la luz al vértice.

Las constantes de atenuación actúan como coeficientes en la fórmula: puedes generar una variedad de curvas atenuación mediante ajustes sencillos. Puedes establecer Attenuation1 en 1,0 para crear una luz que no se atenúe, pero que aún esté limitada por el intervalo, o puedes experimentar con diferentes valores para lograr distintos efectos de atenuación.

La atenuación en el intervalo máximo de la luz no es 0.0. Para evitar que aparezcan luces repentinamente cuando estén en el intervalo de la luz, una aplicación puede aumentar el intervalo de la luz. O bien, la aplicación puede configurar constantes de atenuación para que el factor de atenuación sea cercano a 0,0 en el intervalo de la luz. El valor de la atenuación se multiplica por los componentes rojo, verde y azul del color de la luz para escalar la intensidad de la luz como factor de la distancia que recorre la luz hasta un vértice.

## <a name="span-idspotlight-factorspanspan-idspotlight-factorspanspan-idspotlight-factorspanspotlight-factor"></a><span id="Spotlight-Factor"></span><span id="spotlight-factor"></span><span id="SPOTLIGHT-FACTOR"></span>Factor de foco de luz


La siguiente ecuación especifica el factor de foco de luz.

![ecuación del factor de foco de luz](images/dx8light9.png)

| Parámetro         | Valor predeterminado | Tipo           | Descripción                              | Intervalo                    |
|-------------------|---------------|----------------|------------------------------------------|--------------------------|
| rho<sub>i</sub>   | N/C           | Punto flotante | coseno(ángulo) para el foco de luz i            | N/C                      |
| phi<sub>i</sub>   | 0.0           | Punto flotante | Ángulo de penumbra del foco de luz i en radianes | \[theta<sub>i</sub>, pi) |
| theta<sub>i</sub> | 0.0           | Punto flotante | Ángulo de umbra del foco de luz i en radianes    | \[0, pi)                 |
| falloff           | 0.0           | Punto flotante | Factor de disminución                           | (-infinito, +infinito)   |

 

Donde:

rho = norm(L<sub>dcs</sub>)<sup>.</sup>norm(L<sub>dir</sub>)

y:

| Parámetro       | Valor predeterminado | Tipo                                             | Descripción                                                 |
|-----------------|---------------|--------------------------------------------------|-------------------------------------------------------------|
| L<sub>dcs</sub> | N/C           | Vector 3D con valores de punto flotante x, y, z | El valor negativo de la dirección de la luz en el espacio de la cámara         |
| L<sub>dir</sub> | N/C           | Vector 3D con valores de punto flotante x, y, z | Vector de dirección desde la posición del vértice hasta la posición de la luz |

 

Tras calcular la atenuación de la luz, para calcular los componentes de difusión y especulares para el vértice, Direct3D también tiene en cuenta los efectos del foco de luz, si procede, el ángulo en el que se refleja la luz desde una superficie y la reflectividad del material actual. En [Tipos de luz](light-types.md), consulta "Foco de luz".

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




