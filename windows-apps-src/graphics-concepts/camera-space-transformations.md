---
title: "Transformaciones del espacio de cámara"
description: "Los vértices del espacio de cámara se calculan al transformar los vértices del objeto con la matriz de vista global."
ms.assetid: 86EDEB95-8348-4FAA-897F-25251B32B076
keywords: "Transformaciones del espacio de cámara"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 907df69fdd0c785294283de858a0fcebd0c63513
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="camera-space-transformations"></a>Transformaciones del espacio de cámara


Los vértices del espacio de cámara se calculan al transformar los vértices del objeto con la matriz de vista global.

V = V \* wvMatrix

Para calcular las normales de los vértices, en el espacio de cámara, se transforman las normales de los objetos con la traspuesta inversa de la matriz de vista global. La matriz de vista global puede ser ortogonal o no.

N = N \* (wvMatrix⁻¹)<sup>T</sup>

La inversión de la matriz y la traspuesta de la matriz funcionan en una matriz de 4×4. La multiplicación combina la normal con la porción de 3×3 de la matriz resultante de 4×4.

Si el estado de representación se establece para normalizar las normales, los vectores de las normales de los vértices se normalizan tras la transformación al espacio de cámara del modo siguiente:

N = norm(N)

Para calcular la posición de la luz en el espacio de cámara, se transforma la posición del origen de luz con la matriz de vista.

Lₚ = Lₚ \* vMatrix

La dirección a la luz en el espacio de cámara para una luz direccional se calcula multiplicando la dirección del origen de luz por la matriz de vista, normalizando y haciendo negativo el resultado.

L<sub>dir</sub> = -norm(L<sub>dir</sub> \* wvMatrix)

Para una luz puntual y un foco de luz, la dirección a la luz se calcula como sigue:

L<sub>dir</sub> = norm(V \* Lₚ), donde los parámetros se definen en la siguiente tabla.

| Parámetro       | Valor predeterminado | Tipo                                          | Descripción                                               |
|-----------------|---------------|-----------------------------------------------|-----------------------------------------------------------|
| L<sub>dir</sub> | N/C           | Vector 3D (valores de punto flotante x, y, z) | Vector de dirección de los vértices del objeto a la luz          |
| V               | N/C           | Vector 3D (valores de punto flotante x, y, z) | Posición del vértice en el espacio de cámara                           |
| wvMatrix        | Identidad      | Matriz de 4×4 de valores de punto flotante           | Matriz compuesto que contiene las transformaciones global y de vista |
| N               | N/C           | Vector 3D (valores de punto flotante x, y, z) | Normal de los vértices                                             |
| Lₚ              | N/C           | Vector 3D (valores de punto flotante x, y, z) | Posición de la luz en el espacio de cámara                            |
| vMatrix         | Identidad      | Matriz de 4×4 de valores de punto flotante           | Matriz que contiene la transformación de vista                      |

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 



