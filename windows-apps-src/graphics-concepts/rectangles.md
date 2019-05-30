---
title: Rectángulos
description: En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores.
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rectángulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9d6a3b1eb64c89f231d0334a3cbe8e58d11c58ae
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370953"
---
# <a name="rectangles"></a>Rectángulos

En la programación de Direct3D y Windows, se hace referencia a los objetos de la pantalla en términos de rectángulos delimitadores. Los lados de un rectángulo delimitador siempre son paralelos a los lados de la pantalla, por lo que se puede describir el rectángulo por dos puntos, la esquina superior izquierda y la esquina inferior derecha.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Rectángulos


La mayoría de las aplicaciones utilizan la estructura [**RECT**](https://docs.microsoft.com/previous-versions//dd162897(v=vs.85)) (o un alias typedef definido para esta) para transmitir información acerca de un rectángulo delimitador y usarla al realizar la transferencia de bloques de bits a la pantalla o al realizar la detección de coincidencias. En C++, la estructura **RECT** tiene la siguiente definición.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

En el ejemplo anterior, los miembros izquierdo y superior son las coordenadas x e y de la esquina superior izquierda de un rectángulo delimitador. De manera similar, los miembros derecho e inferior conforman las coordenadas de la esquina inferior derecha. La siguiente ilustración muestra cómo puedes visualizar estos valores.

![Ilustración del rectángulo delimitado por los valores izquierdo, superior, derecho e inferior](images/rect.png)

La columna de píxeles del borde derecho y la fila de píxeles del borde inferior no se incluyen en la estructura RECT. Por ejemplo, el bloqueo de una estructura RECT con miembros {10, 10, 138, 138} da como resultado un objeto de 128 píxeles de alto y ancho.

Por motivos de eficiencia, coherencia y facilidad de uso, todas las funciones de presentación de Direct3D funcionan con rectángulos.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Sistemas de coordenadas y geometría](coordinate-systems-and-geometry.md)

 

 




