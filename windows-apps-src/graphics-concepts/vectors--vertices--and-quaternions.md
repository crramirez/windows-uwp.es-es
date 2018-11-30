---
title: Vectores, vértices y cuaterniones
description: En Direct3D, los vértices describen la posición y la orientación. Cada vértice de un primitivo se describe mediante un vector que proporciona la posición, el color, las coordenadas de textura y un vector normal que da su orientación.
ms.assetid: 94EC3D59-43FC-4509-A233-916E9FA8381E
keywords:
- Vectores, vértices y cuaterniones
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8942e53b7372e2e8b3cf4ed05f89b4187bdfc4be
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8217538"
---
# <a name="vectors-vertices-and-quaternions"></a>Vectores, vértices y cuaterniones


En Direct3D, los vértices describen la posición y la orientación. Cada vértice de un primitivo se describe mediante un vector que proporciona la posición, el color, las coordenadas de textura y un vector normal que da su orientación.

Los cuaterniones agregan un cuarto elemento a los valores \[x, y, z\] que definen un vector de tres componentes. Los cuaterniones son una alternativa a los métodos de matriz que se usan normalmente para rotaciones 3D. Un cuaternión representa un eje en el espacio 3D y una rotación alrededor de ese eje. Por ejemplo, un cuaternión puede representar un eje (1,1,2) y una rotación de 1 radián. Los cuaterniones llevan información valiosa, pero su verdadera energía proviene de las dos operaciones que se pueden realizar en ellos: composición e interpolación.

Realizar composición en cuaterniones es similar a combinarlos. La composición de dos valores de cuaternión se expresa como en la siguiente ilustración.

![ilustración de notación de cuaternión](images/quateq.png)

La composición de dos valores de cuaternión aplicada a una geometría significa "girar la geometría alrededor de axis₂ mediante rotation₂ y luego girar alrededor de axis₁ mediante rotation₁". En este caso, Q representa una rotación alrededor de un solo eje que es el resultado de la aplicación de q₂, y luego q₁ a la geometría.

Mediante la interpolación de cuaternión, una aplicación puede calcular una ruta razonable y suave de un eje y orientación a otro. Por lo tanto, la interpolación entre q₁ y q₂ ofrece una manera simple de animar de una orientación a otra.

Cuando usas composición e interpolación juntas, proporcionan una forma sencilla de manipular una geometría de manera que parezca compleja. Por ejemplo, imagina que tienes una geometría que quieres que gire a una orientación determinada. Sabes que tienes que hacerla girar r₂ grados alrededor de axis₂, después hacerla girar r₁ grados alrededor de axis₁, pero no sabes cuál es el cuaternión final. Mediante el uso de composición, podrías combinar los dos giros en la geometría para obtener un cuaternión único que es el resultado. Después podrías interpolar del cuaternión original al cuaternión compuesto para lograr una transición suave de uno a otro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




