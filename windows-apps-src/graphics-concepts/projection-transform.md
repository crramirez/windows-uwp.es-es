---
title: Transformación de proyección
description: Una transformación de proyección controla los aspectos internos de la cámara, como la elección de un objetivo para una cámara. Este es el más complicado de los tres tipos de transformación.
ms.assetid: 378F205D-3800-4477-9820-5EBE6528B14A
keywords:
- Transformación de proyección
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: bbaa0a6e02529bad55e2d90c90510a2ec1b40dd5
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044354"
---
# <a name="projection-transform"></a>Transformación de proyección


Una *transformación de proyección* controla los aspectos internos de la cámara, como la elección de un objetivo para una cámara. Este es el más complicado de los tres tipos de transformación.

La matriz de proyección suele ser una proyección de escala y perspectiva. La transformación de proyección convierte el frustum de visualización en una forma de cubo. Dado que el extremo más próximo del frustum de visualización es menor que el más alejado, tiene el efecto de expandir objetos que están cerca de la cámara; así es cómo se aplica la perspectiva a la escena.

En [el frustum de visualización](viewports-and-clipping.md), la distancia entre la cámara y el origen del espacio de transformación de visualización se define de manera arbitraria como D, de modo que la matriz de proyección se parece a la de la siguiente ilustración.

![Ilustración de la matriz de proyección](images/projmat1.png)

La matriz de visualización traslada la cámara al origen mediante la traslación en la dirección z a - D. La matriz de traslación es como la de la siguiente ilustración.

![Ilustración de la matriz de traslación](images/projmat2.png)

La multiplicación de la matriz de traslación por la matriz de proyección (T\*P) da como resultado la matriz de proyección compuesta, como se muestra en la siguiente ilustración.

![Ilustración de la matriz de proyección compuesta](images/projmat3.png)

La transformación de perspectiva convierte un frustum de visualización en un nuevo espacio de coordenadas. Observe que el frustum se convierte en un cubo y también que el origen se mueve de la esquina superior derecha de la escena al centro, como se muestra en el siguiente diagrama.

![Diagrama que muestra cómo la transformación de perspectiva convierte el frustum de visualización en un nuevo espacio de coordenadas](images/cuboid.png)

En la transformación de perspectiva, los límites de las direcciones x e y son -1 y 1. Los límites de la dirección z son 0 para el plano frontal y 1 para el plano posterior.

Esta matriz traslada y escala los objetos en función de una distancia especificada desde la cámara hasta el plano de recorte cercano, pero no considera el campo de visión (fov) y los valores z que genera para los objetos en la distancia pueden ser casi idénticos, lo que dificulta las comparaciones de profundidad. La siguiente matriz soluciona estos problemas y ajusta los vértices para tener en cuenta la relación de aspecto de la ventanilla, por lo que resulta una buena opción para la proyección de perspectiva.

![Ilustración de una matriz de proyección de perspectiva](images/prjmatx1.png)

En esta matriz, Zₙ es el valor z del plano de recorte cercano. Las variables w, h y Q tienen los siguientes significados. Ten en cuenta que fov<sub>w</sub> y fovₖ representan los campos de visión horizontal y vertical de la ventanilla, en radianes.

![Significados de las ecuaciones de las variables](images/prjmatx2.png)

En la aplicación, es posible que el uso de los ángulos del campo de visión para definir los coeficientes de escala x e y no resulte tan práctico como utilizar las dimensiones horizontal y vertical de la ventanilla (en el espacio de la cámara). Dado que la expresión matemática funciona, las siguientes dos ecuaciones para w y h utilizan las dimensiones de la ventanilla y son equivalentes a las ecuaciones anteriores.

![Significados de las ecuaciones de las variables w y h](images/prjmatx3.png)

En estas fórmulas, Zₙ representa la posición del plano de recorte cercano y las variables V<sub>w</sub> y Vₕ representan el ancho y el alto de la ventanilla, en el espacio de la cámara.

Independientemente de la fórmula que decidas utilizar, asegúrate de establecer Zₙ en el valor más alto posible, ya que los valores z muy cercanos a la cámara no varían mucho. Esto dificulta un poco las comparaciones de profundidad con búferes Z de 16 bits.

## <a name="span-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspanspan-idawfriendlyprojectionmatrixspana-w-friendly-projection-matrix"></a><span id="A_W_Friendly_Projection_Matrix"></span><span id="a_w_friendly_projection_matrix"></span><span id="A_W_FRIENDLY_PROJECTION_MATRIX"></span>Una matriz de proyección para w


Direct3D puede usar el componente w de un vértice transformado por las matrices de mundo, vista y proyección para realizar cálculos basados en profundidad en el búfer de profundidad o efectos de niebla. Los cálculos de este tipo requieren que la matriz de proyección normalice w para que sea equivalente a z del espacio del mundo. En resumen, si la matriz de proyección incluye un coeficiente (3,4) distinto de 1, debes escalar todos los coeficientes por la inversa del coeficiente (3,4) para crear una matriz adecuada. Si no proporcionas una matriz compatible, el almacenamiento en búfer de profundidad y los efectos de niebla no se aplicarán correctamente.

La siguiente ilustración muestra una matriz de proyección no conforme y la misma matriz con escala para que se habilite la niebla relativa al ojo.

![Ilustraciones de una matriz de proyección no conforme y una matriz con niebla relativa al ojo](images/eyerlmx.png)

En las matrices anteriores, se supone que todas las variables son distintas de cero. Para obtener información sobre el almacenamiento en búfer de profundidad basado en w, consulta [Búferes de profundidad](depth-buffers.md).

Direct3D utiliza la matriz de proyección establecida actualmente en sus cálculos de profundidad basados en w. Como resultado, las aplicaciones deben establecer una matriz de proyección compatible para recibir las características basadas en w deseadas, aunque no usen las transformaciones de Direct3D.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Transformaciones](transforms.md)

 

 




