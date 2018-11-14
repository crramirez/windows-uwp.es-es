---
title: Luz ambiente
description: La luz ambiente proporciona iluminación constante a una escena.
ms.assetid: C34FA65A-3634-4A4B-B183-4CDA89F4DC95
keywords:
- Luz ambiente
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 87b5c72ef99e3802a348ddfd28951bc2865891e5
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6663634"
---
# <a name="ambient-lighting"></a>Luz ambiente


La luz de ambiente proporciona iluminación constante a una escena. Ilumina todos los vértices de un objeto del mismo modo porque no depende de ningún otro factor de iluminación, como las normales de los vértices, la dirección de la luz, la posición de la luz, el alcance o la atenuación. La luz ambiente es constante en todas las direcciones y colorea todos los píxeles de un objeto de la misma manera. Se calcular con rapidez, pero deja objetos con una apariencia plana y poco realista.

La luz ambiente es el tipo de iluminación más rápido, pero obtienen los resultados menos realistas. Direct3D contiene una sola propiedad de luz ambiente global que puedes usar sin tener que crear ninguna luz. Como alternativa, puedes establecer cualquier objeto de luz para que proporcione luz ambiente.

La luz ambiente para una escena se describe mediante la siguiente ecuación.

Luz ambiente = Cₐ\*\[Gₐ + sum(Atten<sub>i</sub>\*Spot<sub>i</sub>\*L<sub>ai</sub>)\]

Donde:

| Parámetro         | Valor predeterminado | Tipo          | Descripción                                                                                                       |
|-------------------|---------------|---------------|-------------------------------------------------------------------------------------------------------------------|
| Cₐ                | (0,0,0,0)     | D3DCOLORVALUE | Color ambiente del material                                                                                            |
| Gₐ                | (0,0,0,0)     | D3DCOLORVALUE | Color ambiente global                                                                                              |
| Atten<sub>i</sub> | (0,0,0,0)     | D3DCOLORVALUE | Atenuación lumínica de la luz i. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md). |
| Spot<sub>i</sub>  | (0,0,0,0)     | D3DVECTOR     | Factor del foco de luz de la luz i. Consulta [Atenuación y factor de foco de luz](attenuation-and-spotlight-factor.md).  |
| sum               | N/A           | N/C           | Suma de la luz ambiente                                                                                          |
| L<sub>ai</sub>    | (0,0,0,0)     | D3DVECTOR     | Color de luz ambiente de la luz i.                                                                              |

 

El valor de Cₐ es:

-   vertex color1, si AMBIENTMATERIALSOURCE = D3DMCS\_COLOR1, y el primer color del vértice se suministra en la declaración del vértice.
-   vertex color2, si AMBIENTMATERIALSOURCE = D3DMCS\_COLOR2, y el segundo color del vértice se suministra en la declaración del vértice.
-   color ambiente del material.

**Nota**  si se usan cualquiera de estas opciones de AMBIENTMATERIALSOURCE y no se proporciona el color del vértice, se usa el color ambiente del material.

 

Para usar el color ambiente del material, usa SetMaterial como se muestra en el siguiente código de ejemplo.

Gₐ es el color ambiente global. Se establece mediante SetRenderState(D3DRS\_AMBIENT). Hay un color ambiente global en una escena de Direct3D. Este parámetro no está asociado con un objeto de luz de Direct3D.

L<sub>ai</sub> es el color ambiente de la luz i en la escena. Cada luz en Direct3D tiene un conjunto de propiedades, una de ellas es el color ambiente. El término, sum(L<sub>ai</sub>) es la suma de todos los colores ambiente de la escena.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


En este ejemplo, el objeto se colorea con la luz ambiente de la escena y un color ambiente del material.

```
#define GRAY_COLOR  0x00bfbfbf

Ambient.r = 0.75f;
Ambient.g = 0.0f;
Ambient.b = 0.0f;
Ambient.a = 0.0f;
```

Según la ecuación, el color resultante para los vértices del objeto es una combinación del color del material y del color de la luz.

Las dos ilustraciones siguientes muestran el color del material, que es gris, y el color de la luz, que es rojo brillante.

![ilustración de una esfera gris](images/amb1.jpg)![ilustración de una esfera roja](images/lightred.jpg)

En la siguiente ilustración se muestra la escena resultante. El único objeto en la escena es una esfera. La luz ambiente ilumina todos los vértices del objeto con el mismo color. No depende de la normal del vértice ni de la dirección de la luz. Como consecuencia, la esfera parece un círculo en 2D porque no existe ninguna diferencia en sombreado alrededor de la superficie del objeto.

![ilustración de una esfera con luz ambiente](images/lighta.jpg)

Para dar a los objetos un aspecto más realista, aplica iluminación especular o difusa, además de luz ambiente.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




