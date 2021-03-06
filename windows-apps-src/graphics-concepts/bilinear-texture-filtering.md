---
title: Filtrado de texturas bilineales
description: El filtrado bilineal calcula el promedio ponderado de los 4 elementos de textura más cercanos al punto de muestreo.
ms.assetid: 0851AD28-8246-4547-A663-47884DDDFC3E
keywords:
- Filtrado de texturas bilineales
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1e76aeceafa3f75c78bd7078f57fa9a3b1edee2f
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291663"
---
# <a name="bilinear-texture-filtering"></a>Filtrado de texturas bilineales

El *filtrado bilineal* calcula el promedio ponderado de los 4 elementos de textura más cercanos al punto de muestreo. Este enfoque filtrado es más precisos y común que el filtrado por punto más cercano. Este enfoque es eficaz, porque está implementado en el hardware de gráficos moderno.


## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


Las texturas siempre se direccionan linealmente desde (0,0, 0,0) en la esquina superior izquierda hasta (1.0, 1.0) en la esquina inferior derecha. En la siguiente ilustración se muestra el direccionamiento lineal de una textura.

![ilustración de una textura de 4×4 con bloques sólido de colores](images/bilinear-fig7a.png)

Las texturas normalmente se representan como si se han formado de bloques sólidos del color, pero es realmente más correcto pensar texturas en la misma manera que se debe considerar la visualización de mapa de bits: Cada elemento de textura se define exactamente en el centro de una celda de cuadrícula, como se muestra en la siguiente ilustración.

![ilustración de textura de 4×4 con elementos de textura definidos en el centro de las celdas de la cuadrícula](images/bilinear-fig7b.png)

Si le preguntas al muestrario de textura por el color de esta textura en las coordenadas UV (0.375, 0.375), obtendrás rojo sólido (255, 0, 0). Esto tiene sentido, ya que el centro de la celda del elemento de textura rojo está en UV (0.375, 0.375). ¿Qué ocurre si le pregunta al muestrario por el color de la textura en UV (0.25, 0.25)? Esto no es tan sencillo, ya que el punto en UV (0.25, 0.25) reside en la esquina exacta de los 4 elementos de textura.

El esquema más sencillo es simplemente que el muestrario devuelva el color del elemento de textura más cercano; esto se denomina filtrado de puntos (consulta [Muestreo de punto más cercano](nearest-point-sampling.md)) y no suele ser conveniente, debido a los resultados granulados o en bloque. El muestreo de puntos en nuestra textura en UV (0.25, 0.25) muestra otro problema sutil con el filtrado por punto más cercano: hay cuatro elementos de textura equidistantes desde el punto de muestreo, por lo que hay un único elemento de textura más cercano. Uno de estos cuatro elementos de textura se elegirá como el color devuelto, pero la selección depende de cómo se redondea la coordenada, lo que puede producir artefactos de rasgado (consulta el artículo sobre el muestreo de punto más cercano en el SDK).

Un esquema de filtrado ligeramente más preciso y común es calcular el promedio ponderado de los 4 elementos de textura más cercano al punto de muestreo; esto se denomina *filtrado bilineal*. La carga de cálculos adicionales el filtrado bilineal suele ser insignificante, porque esta rutina está implementada en el hardware de gráficos moderno. Estos son los colores que obtenemos en algunos puntos de muestra diferentes con el filtrado bilineal:

```cpp
UV: (0.5, 0.5)
```

Este punto se encuentra en el borde exacto entre los elementos de textura rojo, verde, azul y blanco. El color que devuelve el muestrario es gris:

```cpp
  0.25 * (255, 0, 0)
  0.25 * (0, 255, 0) 
  0.25 * (0, 0, 255) 
## + 0.25 * (255, 255, 255) 
------------------------
= (128, 128, 128)
```

```cpp
UV: (0.5, 0.375)
```

Este punto se encuentra en el punto medio del borde entre los elementos de textura rojo y verde. El color que devuelve el muestrario es amarillo-gris (ten en cuenta que las contribuciones de los elementos de textura azul y blanco se escalan a 0):

```cpp
  0.5 * (255, 0, 0)
  0.5 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (128, 128, 0)
```

```cpp
UV: (0.375, 0.375)
```

Esta es la dirección del elemento de textura rojo, que es el color devuelto (todos los demás elementos de textura en el cálculo del filtrado se ponderan como 0):

```cpp
  1.0 * (255, 0, 0)
  0.0 * (0, 255, 0) 
  0.0 * (0, 0, 255) 
## + 0.0 * (255, 255, 255) 
------------------------
= (255, 0, 0)
```

Compara estos cálculos con la siguiente ilustración, que muestra lo que sucede si se realiza el cálculo de filtrado bilineal en cada dirección de textura a lo largo de la textura de 4×4.

![ilustración de textura de 4×4 con el filtro bilineal en todas las direcciones de textura](images/bilinear-fig7c.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Filtrado de textura](texture-filtering.md)

 

 




