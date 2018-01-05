---
title: "Muestreo de punto más cercano"
description: Las aplicaciones no tienen que usar el filtrado de textura.
ms.assetid: D7F88320-2C61-47E9-9B92-EC31D48DB079
keywords: "Muestreo de punto más cercano"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 048ff2755343e5e6230900ea9acb819e85ba654b
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="span-iddirect3dconceptsnearest-pointsamplingspannearest-point-sampling"></a><span id="direct3dconcepts.nearest-point_sampling"></span>Muestreo de punto más cercano


Las aplicaciones no tienen que usar el filtrado de textura. Direct3D puede establecerse para que calcule la dirección de elementos de textura, que a menudo no se evalúa como enteros, y para que copie el color del elemento de textura con la dirección de entero más próxima. Este proceso se denomina *muestreo de punto más cercano*. El muestreo de punto más cercano puede ser una forma rápida y eficiente de procesar texturas si el tamaño de la textura es similar al tamaño de la imagen del primitivo en la pantalla. De lo contrario, se debe ampliar o minimizar la textura. El resultado de no hacer coincidir los tamaños de textura con el tamaño de la imagen del primitivo puede ser una imagen cortada, que no coincide o borrosa.

Usa el muestreo de punto más cercano con cuidado: a veces, puede causar artefactos gráficos cuando se muestrea una textura que está en el límite entre dos elementos de textura. Este límite es la posición a lo largo de la textura (u o v) en la que el elemento de textura de muestra cambia de un elemento de textura al siguiente. Cuando se usa el muestreo de punto, el sistema elige un elemento de textura de muestra u otro y el resultado puede cambiar repentinamente de un elemento de textura al siguiente si se cruzan los límites. Este efecto puede aparecer como artefactos gráficos no deseados en la textura mostrada. Cuando se usa el filtrado lineal, el elemento de textura resultante se calcula a partir de ambos elementos de textura admitidos y los combina sin problemas porque el índice de texturas se mueve a través del límite.

Este efecto puede verse al asignar una textura muy pequeña a un polígono muy grande: una operación conocida normalmente como ampliación. Por ejemplo, al usar una textura que parece un tablero de ajedrez, el muestreo de punto más cercano hace que se vea un tablero de ajedrez más grande que muestra bordes diferentes. Por el contrario, el filtrado de textura lineal causa una imagen donde los colores del tablero de ajedrez varían sin problemas a través del polígono.

En la mayoría de los casos, las aplicaciones reciben los mejores resultados porque evitan el muestreo de punto más cercano siempre que sea posible. La mayor parte del hardware actual está optimizado para el filtrado lineal, de modo que la aplicación no debería tener disminución del rendimiento. Si el efecto que quieres necesita obligatoriamente el uso del muestreo de punto más cercano (por ejemplo, al usar texturas para mostrar los caracteres de texto legible), la aplicación debe ir con cuidado para evitar usar el muestreo en los límites de los elementos de textura, lo que podría ocasionar efectos no deseados. En la siguiente ilustración se muestra el aspecto que podrían tener los artefactos.

![ilustración de un cuadro de seis secciones con líneas horizontales discontinuas en los dos cuadrados superiores de la derecha](images/ptrtfct.png)

Los dos cuadrados de la esquina superior derecha del grupo aparecen diferentes que los que tienen al lado, con desplazamientos en diagonal. Para evitar estos artefactos gráficos, debes estar familiarizado con las reglas de muestreo de texturas de Direct3D para el filtrado de punto más cercano. Direct3D asigna una coordenada de textura de punto flotante que va desde \[0,0, 1,0\] (0,0 a 1,0, ambos incluidos) a un valor de espacio de elemento de textura entero que va desde \[- 0,5, n - 0,5\], donde n es el número de elementos de textura de una dimensión determinada en la textura. El índice de textura resultante se redondea al entero más próximo. Esta asignación puede introducir imprecisiones de muestreo en los límites de los elementos de textura.

Por ejemplo, simplemente, supongamos que tenemos una aplicación que representa polígonos con el modo de direccionamiento de texturas de ajuste. Con la asignación que usa Direct3D, el índice de texturas u se asigna como se muestra en el siguiente diagrama para una textura con un ancho de 4 elementos de textura.

![diagrama de coordenadas de textura 0,0 y 1,0 en el límite entre elementos de textura](images/ptsmpprb.png)

Las coordenadas de textura, 0,0 y 1,0 en esta ilustración, están exactamente en el límite entre elementos de textura. Con el método que usa Direct3D para asignar valores, las coordenadas de la textura están comprendidas entre \[- 0,5, 4 - 0,5\], donde 4 es el ancho de la textura. En este caso, el elemento de textura de muestreo es el elemento de textura 0 para un índice de texturas de 1,0. Sin embargo, si la coordenada de textura solo es ligeramente menor que 1,0, el elemento de textura de muestreo sería el elemento de textura n en lugar del 0.

La implicación que tiene esto es que ampliar una textura pequeña con coordenadas de textura de exactamente 0,0 y 1,0 con filtrado de punto más cercano en un triángulo alineado al espacio de pantalla causa píxeles para los que se hace un muestreo del mapa de texturas en el límite entre los elementos de textura. Cualquier imprecisión en el cálculo de coordenadas de texturas, aunque sea pequeño, causa artefactos a lo largo de las áreas de la imagen representada que corresponden a los bordes de los elementos de textura del mapa de texturas.

Realizar esta asignación de coordenadas de texturas de punto flotante en elementos de textura de enteros con una precisión perfecta es difícil, consume mucho tiempo de cálculo y, por lo general, no es necesario. La mayoría de las implementaciones de hardware usan un enfoque iterativo para calcular las coordenadas de texturas en cada ubicación de píxeles de un triángulo. Los enfoques iterativos tienden a ocultar estas imprecisiones porque los errores se acumulan uniformemente durante la iteración.

El rasterizador de referencia de Direct3D usa un enfoque de evaluación directa para calcular índices de texturas en cada ubicación de píxeles. La evaluación directa es distinta del enfoque iterativo porque cualquier imprecisión en la operación presenta una distribución de error más aleatoria. El resultado es que los errores de muestreo que se producen en los límites pueden ser más evidentes porque el rasterizador de referencia no realiza esta operación con una precisión perfecta.

El mejor enfoque es usar el filtrado de punto más cercano solo cuando sea necesario. Cuando debas usarlo, se recomienda que desplaces las coordenadas de textura ligeramente desde las posiciones de los límites para evitar artefactos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Filtrado de texturas](texture-filtering.md)

 

 




