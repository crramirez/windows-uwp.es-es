---
title: Tipos de luz
description: 'La propiedad de tipo de luz define qué tipo de fuente de luz estás usando. Hay tres tipos de luz en Direct3D: luces puntuales, focos de luz y luces direccionales.'
ms.assetid: 57748CAF-6F08-4D1D-9ED6-8FAA8C5FE314
keywords:
- Tipos de luz
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9898a050131813b7b2431f8fc11397eee7c7942c
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "6471690"
---
# <a name="light-types"></a>Tipos de luz


La propiedad de tipo de luz define qué tipo de fuente de luz estás usando. Hay tres tipos de luz en Direct3D: luces puntuales, focos de luz y luces direccionales. Cada tipo ilumina objetos de una escena de forma diferente, con distintos niveles de sobrecarga de cálculo.

## <a name="span-idpointlightspanspan-idpointlightspanspan-idpointlightspanpoint-light"></a><span id="Point_Light"></span><span id="point_light"></span><span id="POINT_LIGHT"></span>Luz puntual


Las luces puntuales tienen color y posición dentro de una escena, pero no tienen dirección. Proyectan luz por igual en todas las direcciones, tal como se muestra en la siguiente ilustración.

![Ilustración de luz puntual](images/ptlight.png)

Una bombilla es un buen ejemplo de una luz puntual. La atenuación y el intervalo afectan a las luces puntuales e iluminan una malla con una base de vértice por vértice. Durante la iluminación, Direct3D usa la posición de la luz puntual en el espacio global y las coordenadas del vértice que se ilumina para derivar un vector para la dirección de la luz y la distancia a la que se desplaza. Ambos se usan, junto con el vértice normal, para calcular la participación de la luz en la iluminación de la superficie.

## <a name="span-iddirectionallightspanspan-iddirectionallightspanspan-iddirectionallightspandirectional-light"></a><span id="Directional_Light"></span><span id="directional_light"></span><span id="DIRECTIONAL_LIGHT"></span>Luz direccional


Las luces direccionales tienen solo color y dirección, no posición. Emiten luz paralela. Esto significa que toda la luz que generan las luces direccionales viaja a través de una escena en la misma dirección. Imagina una luz direccional como una fuente de luz casi a distancia infinita, como el sol. La atenuación o el intervalo no afecta a las luces direccionales, por lo que la dirección y el color que especifiques son los únicos factores que se consideran cuando Direct3D calcula los colores de los vértices. Debido al pequeño número de factores de iluminación, estas luces son las menos intensivas a nivel de cálculo que se usan.

## <a name="span-idspotlightspanspan-idspotlightspanspan-idspotlightspanspotlight"></a><span id="SpotLight"></span><span id="spotlight"></span><span id="SPOTLIGHT"></span>Foco de luz


Los focos de luz tienen color, posición y dirección en la que emiten luz. La luz que emite un foco de luz se compone de un cono interno brillante y un cono externo más grande, con menos intensidad de luz entre los dos, como se muestra en la siguiente ilustración.

![ilustración de un foco de luz con un cono interno y un cono externo](images/spotlt.png)

La caída, la atenuación y el intervalo afectan a los focos de luz. Estos factores, así como la distancia que recorre luz hasta cada vértice, se indican al calcular los efectos de iluminación para los objetos de una escena. Calcular estos efectos para cada vértice hace que los focos de luz sea la luz que más consume, en términos de tiempo de cálculo, de Direct3D.

Solo los focos de luz usan los valores de caída, Theta y Phi. Estos valores controlan el tamaño de los conos interno y externo de un objeto del foco de luz y la reducción de luz entre ambos.

Theta es el ángulo de radián del cono interno del foco de luz y el valor Phi es el ángulo del cono exterior de luz. La caída controla la reducción de la intensidad de la luz entre el borde externo del cono interno y el borde interno del cono externo. La mayoría de las aplicaciones establecen la caída a 1,0 para crear una caída que se produzca uniformemente entre los dos conos, pero puedes definir otros valores según necesites.

En la siguiente ilustración se muestra la relación entre estos valores y cómo pueden afectar a los conos interno y externo de un foco de luz.

![ilustración de cómo se relacionan los valores phi y theta con los conos de los focos de luz](images/spotlt2.png)

Los focos de luz emiten un cono de luz que tiene dos partes: un cono interno brillante y un cono externo. La luz es más brillante en el cono interno y no está fuera del cono externo, con la intensidad de la luz que se atenúa entre las dos áreas. Este tipo de atenuación normalmente se conoce como caída.

La cantidad de luz que recibe un vértice se basa en la ubicación del vértice en el cono interno o externo. Direct3D calcula el producto escalar del vector de dirección del foco de luz (L) y el vector desde la luz hasta el vértice (D). Este valor es igual al coseno del ángulo entre los dos vectores y actúa como indicador de posición del vértice que se puede comparar con los ángulos del cono de la luz para determinar dónde puede estar el vértice en el cono interno o externo. En la siguiente ilustración se proporciona una representación gráfica de la asociación entre estos dos vectores.

![ilustración del vector de dirección del foco de luz y el vector desde el vértice hasta el foco de luz](images/spotalg1.png)

El sistema compara este valor con el coseno de los ángulos del cono interno y externo del foco de luz. Los valores Theta y Phi de la luz representan los ángulos totales del cono para el cono interno y externo. Dado que la atenuación se produce porque el vértice se aleja más del centro de iluminación (en lugar de en el ángulo total del cono), el tiempo de ejecución divide los ángulos del cono por la mitad antes de calcular sus cosenos.

Si el producto escalar de los vectores L y D es menor o igual al coseno del ángulo del cono externo, el vértice se encuentra fuera del cono externo y no recibe luz. Si el producto escalar de L y D es mayor que el coseno del ángulo del cono interno, el vértice está dentro del cono interno y recibe la cantidad máxima de luz, aunque se considere la atenuación por la distancia. Si el vértice está en algún lugar entre las dos regiones, se calcula la caída con la siguiente ecuación.

![fórmula para obtener la intensidad de la luz en el vértice, después de la caída](images/falloff.png)

donde:

-   I<sub>f</sub> es la intensidad de la luz después de la caída
-   Alpha es el ángulo entre los vectores L y D
-   Theta es el ángulo del cono interno
-   Phi es el ángulo del cono externo
-   p es la caída

Esta fórmula genera un valor entre 0,0 y 1,0 que escala la intensidad de la luz en el vértice para tener en cuenta la caída. También se aplica la atenuación como un factor de la distancia del vértice respecto a la luz. En el siguiente gráfico se muestra cómo pueden afectar diferentes valores de caída a la curva de caída.

![gráfico de intensidad de la luz en oposición a distancia del vértice respecto a la luz](images/fallgraf.png)

El efecto de distintos valores de caída en la iluminación real es sutil y se incurre en una pequeña penalización de rendimiento al definir la curva de caída con otros valores de caída que no sean 1,0. Por estas razones, este valor se establece normalmente en 1,0.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Luces y materiales](lights-and-materials.md)

 

 




