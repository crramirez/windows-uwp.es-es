---
title: Transformación de vista
description: Una Transformación de vista localiza el visor en el espacio de mundo, transformando los vértices en espacio de cámara.
ms.assetid: DA4C2051-4C28-4ABF-8C06-241C8CB87F2F
keywords:
- Transformación de vista
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 40f9312b4b85e9f6e54a8bf02c6d07df35b8b626
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "6466192"
---
# <a name="view-transform"></a>Transformación de vista


Una *Transformación de vista* localiza el visor en el espacio de mundo, transformando los vértices en espacio de cámara. En espacio de cámara, la cámara o el visor, están en el origen, mirando hacia la dirección positiva z. La matriz de vista reubica los objetos en el mundo alrededor de la posición de la cámara (el origen del espacio de cámara) y la orientación. Direct3D usa un sistema de coordenadas de la izquierda, por lo que z es positivo en una escena.

Existen muchas formas de crear una matriz de vista. En todos los casos, la cámara tiene alguna posición y orientación lógicas en el espacio del mundo que se usa como punto de partida para crear una matriz de vista que se aplicará a los modelos de una escena. La matriz de vista se traslada y gira los objetos para que aparezcan en el espacio de la cámara, donde la cámara se encuentra en el origen. Una forma de crear una matriz de vista es combinar una matriz de traslación con matrices de rotación para cada eje. En este enfoque, se aplica la siguiente ecuación de matriz general.

![ecuación de la transformación de vista](images/viewtran.png)

En esta fórmula, V es la matriz de vista que se crea, T es una matriz de traslación que cambia la posición de objetos en el mundo y de Rₓ a R<sub>z</sub> son matrices de rotación que giran objetos a lo largo de los ejes x, y y z. Las matrices de traslación y rotación se basan en la posición y orientación lógicas de la cámara en el espacio del mundo. Por lo tanto, si la posición lógica de la cámara en el mundo es &lt;10,20,100&gt;, el objetivo de la matriz de traslación es mover objetos -10 unidades a lo largo del eje x, -20 unidades a lo largo del eje y y -100 unidades a lo largo del eje z. Las matrices de rotación en la fórmula se basan en la orientación de la cámara, según cuánto se giren los ejes del espacio de la cámara fuera de la alineación con el espacio del mundo. Por ejemplo, si la cámara mencionada apunta hacia abajo, su eje z está 90 grados (pi/2 radianes) fuera de la alineación con respecto al eje z del espacio de mundo, como se muestra en la siguiente ilustración.

![ilustración del espacio de vista de la cámara en comparación con el espacio del mundo](images/camtop.png)

Las matrices de rotación aplican rotaciones de igual magnitud, pero opuestas a los modelos de la escena. La matriz de vista para esta cámara incluye una rotación de -90 grados alrededor del eje x. La matriz de rotación está combinada con la matriz de traslación para crear una matriz de vista que ajusta la posición y la orientación de los objetos de una escena, de forma que la partes superior esté delante de la cámara, dando la impresión que la cámara está encima del modelo.

## <a name="span-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspanspan-idsettingupaviewmatrixspansetting-up-a-view-matrix"></a><span id="Setting_Up_a_View_Matrix"></span><span id="setting_up_a_view_matrix"></span><span id="SETTING_UP_A_VIEW_MATRIX"></span>Configuración de una matriz de vista


Direct3D usa las matrices de mundo y vista para configurar varias estructuras de datos internas. Cada vez que defines una nueva matriz de mundo o vista, el sistema vuelve a calcular las estructuras internas asociadas. Configurar estas matrices con frecuencia lleva mucho tiempo desde el punto de vista computacional. Puedes minimizar el número de cálculos necesarios concatenando las matrices de vista y mundo en una matriz de vista-mundo que se establece como la matriz de mundo y estableciendo después la matriz de vista a la identidad. Mantén copias en caché de matrices de vista y mundo individuales para que se puede modificar, concatenar y restablecer la matriz de mundo, según sea necesario.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Transformaciones](transforms.md)

 

 




