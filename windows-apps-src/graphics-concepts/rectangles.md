---
title: Rectángulos
description: En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rectángulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a30aa1a2901f109a4f13316024785981023975b8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156259"
---
# <a name="rectangles"></a>Rectángulos

En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores. Los lados de un rectángulo delimitador siempre son paralelos a los lados de la pantalla, por lo que el rectángulo puede describirse mediante dos puntos, la esquina superior izquierda y la esquina inferior derecha.

## <a name="span-idbounding_rectanglesspanspan-idbounding_rectanglesspanspan-idbounding_rectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Rectángulos delimitadores


La mayoría de las aplicaciones usan la estructura [**Rect**](/previous-versions/dd162897(v=vs.85)) (o un alias de definición de tipo) para llevar información sobre un rectángulo delimitador que se va a utilizar al blitting a la pantalla o al realizar la detección de aciertos. En C++, la estructura **Rect** tiene la siguiente definición.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

En el ejemplo anterior, los miembros izquierdo y superior son las coordenadas x e y de la esquina superior izquierda de un rectángulo delimitador. Del mismo modo, los miembros derecho e inferior forman las coordenadas de la esquina inferior derecha. En la ilustración siguiente se muestra cómo puede visualizar estos valores.

![Ilustración del rectángulo delimitado por los valores izquierdo, superior, derecho e inferior](images/rect.png)

La columna de píxeles en el borde derecho y la fila de píxeles en el borde inferior no se incluyen en el rectángulo. Por ejemplo, si se bloquea un RECT con miembros {10, 10, 138, 138}, se obtiene un objeto de 128 píxeles de ancho y alto.

En cuanto a la eficacia, la coherencia y la facilidad de uso, todas las funciones de presentación de Direct3D funcionan con los rectángulos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 