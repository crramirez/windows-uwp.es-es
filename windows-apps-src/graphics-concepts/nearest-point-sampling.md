---
title: Muestreo de punto más cercano
description: Obtenga información sobre cómo usar el muestreo de punto más cercano en Direct3D como alternativa al filtrado de texturas para procesar texturas en una aplicación.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords:
- Muestreo de punto más cercano
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 090c148e05e664ffe0b027fe9af7eb69c11b22a9
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043497"
---
# <a name="span-iddirect3dconceptsnearest-point_samplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Muestreo de punto más cercano


No es necesario que las aplicaciones usen el filtrado de textura. Direct3D se puede establecer para que calcule la dirección textura, que a menudo no se evalúa como enteros, y copia el color de textura con la dirección de entero más cercana. Este proceso se denomina *muestreo de punto más cercano*. El muestreo de punto más cercano puede ser como si el tamaño de la textura es similar al tamaño de la imagen del primitivo en la pantalla. Si no es así, la textura debe ser ampliada o reducida. El resultado de no coincidir los tamaños de textura con el tamaño de la imagen primitiva puede ser una imagen fragmentada, con alias o borrosa.

Use detenidamente el muestreo del punto más cercano, ya que a veces puede producir artefactos gráficos cuando se muestrea la textura en el límite entre dos textura. Este límite es la posición a lo largo de la textura (u o v) en la que el textura muestreado realiza la transición de un textura al siguiente. Cuando se usa el muestreo de puntos, el sistema elige un textura de ejemplo u otro, y el resultado puede cambiar repentinamente de un textura al siguiente textura a medida que se cruza el límite. Este efecto puede aparecer como artefactos gráficos no deseados en la textura mostrada. Cuando se usa el filtrado lineal, el textura resultante se calcula a partir de los textura adyacentes y de mezcla fluida entre ellos a medida que el índice de textura se desplaza por el límite.

Este efecto puede verse cuando se asigna una textura muy pequeña a un polígono muy grande: una operación a menudo denominada ampliación. Por ejemplo, cuando se usa una textura que se parece a un tablero de ajedrez, el muestreo de punto más cercano produce un tablero de ajedrez mayor que muestra bordes distintos. Por el contrario, el filtrado de textura lineal da como resultado una imagen en la que los colores del tablero de ajedrez varían suavemente en el polígono.

En la mayoría de los casos, las aplicaciones reciben los mejores resultados evitando el ejemplo de punto más cercano siempre que sea posible. La mayoría del hardware de hoy en día está optimizado para el filtrado lineal, por lo que la aplicación no debe sufrir un rendimiento degradado. Si el efecto que desea realmente requiere el uso del muestreo de punto más cercano (por ejemplo, al usar texturas para mostrar caracteres de texto legibles), la aplicación debe ser extremadamente cuidadosa para evitar el muestreo en los límites de textura, lo que podría dar lugar a efectos no deseados. En la ilustración siguiente se muestra el aspecto de estos artefactos.

![Ilustración de un cuadro de seis secciones con líneas horizontales no continuas en los dos cuadrados de la esquina superior derecha](images/ptrtfct.png)

Los dos cuadrados situados en la esquina superior derecha del grupo aparecen distintos de sus vecinos, con desplazamientos diagonales que se ejecutan a través de ellos. Para evitar artefactos gráficos como estos, debe estar familiarizado con las reglas de muestreo de texturas de Direct3D para el filtrado de punto más cercano. Direct3D asigna una coordenada de textura de punto flotante que va de \[ 0,0, 1,0 \] (0,0 a 1,0, inclusive) a un valor de espacio textura entero que va de \[ -0,5, n-0,5 \] , donde n es el número de textura de una dimensión determinada en la textura. El índice de textura resultante se redondea al entero más próximo. Esta asignación puede introducir imprecisiones de muestreo en los límites de textura.

Para un ejemplo sencillo, imagine una aplicación que representa polígonos con el modo de direccionamiento de textura de ajuste. Con la asignación empleada por Direct3D, el índice de textura u se asigna como se muestra en el diagrama siguiente para una textura con un ancho de 4 textura.

![diagrama de coordenadas de textura 0,0 y 1,0 en el límite entre textura](images/ptsmpprb.png)

Las coordenadas de textura, 0,0 y 1,0 para esta ilustración, se encuentran exactamente en el límite entre textura. Mediante el método por el que Direct3D asigna valores, las coordenadas de textura van de \[ -0,5, 4-0,5 \] , donde 4 es el ancho de la textura. En este caso, el valor de textura muestreado es 0 textura para un índice de textura de 1,0. Sin embargo, si la coordenada de textura solo era ligeramente inferior a 1,0, el textura muestreado sería el n textura en lugar de 0 textura.

La implicación de esto es que la ampliación de una pequeña textura con coordenadas de textura de exactamente 0,0 y 1,0 con filtrado de punto más cercano en un triángulo alineado con el espacio de pantalla da como resultado píxeles en los que se muestrea el mapa de textura en el límite entre textura. Las imprecisiones en el cálculo de coordenadas de textura, sin embargo pequeñas, producen artefactos a lo largo de las áreas de la imagen representada que corresponden a los bordes textura del mapa de textura.

La realización de esta asignación de coordenadas de textura de punto flotante al número entero textura con una precisión perfecta es difícil, computacionalmente lenta y, por lo general, no es necesario. La mayoría de las implementaciones de hardware usan un enfoque iterativo para calcular las coordenadas de textura en cada ubicación de píxel dentro de un triángulo. Los enfoques iterativos suelen ocultar estas imprecisiones, ya que los errores se acumulan uniformemente durante la iteración.

El rasterizador de referencia de Direct3D usa un enfoque de evaluación directa para calcular los índices de textura en cada ubicación de píxel. La evaluación directa difiere del enfoque iterativo en que cualquier inexactitud en la operación exhibe una distribución de errores más aleatoria. El resultado de esto es que los errores de muestreo que se producen en los límites pueden ser más evidentes, ya que el rasterizador de referencia no realiza esta operación con una precisión perfecta.

El mejor enfoque es usar el filtrado de punto más cercano solo cuando sea necesario. Cuando se debe utilizar, se recomienda desplazar ligeramente las coordenadas de textura de las posiciones de límite para evitar los artefactos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Filtrado de texturas](texture-filtering.md)

 

 




