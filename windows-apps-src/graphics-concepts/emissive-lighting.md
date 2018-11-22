---
title: Luz de emisión
description: La iluminación de emisión es la luz que emite un objeto; por ejemplo, un resplandor.
ms.assetid: 262EB9E2-DF96-401F-93D6-51A7BB60074B
keywords:
- Luz de emisión
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ce2082479261cd96fb1c5bafd5f2df06bf6f239
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7575511"
---
# <a name="emissive-lighting"></a>Luz de emisión


*La iluminación de emisión* es la luz que emite un objeto; por ejemplo, un resplandor. La emisión hace que un objeto representado parezca que sea luminoso por sí mismo. La emisión afecta al color de un objeto y, por ejemplo, puede hacer que un material oscuro sea más brillante y absorba parte del color emitido.

La iluminación de emisión se describe con un único término.

Luz de emisión = Cₑ

donde:

| Parámetro | Valor predeterminado | Tipo                                                                 | Descripción     |
|-----------|---------------|----------------------------------------------------------------------|-----------------|
| Cₑ        | (0,0,0,0)     | Transparencia roja, verde, azul y alfa (todos los valores de punto flotante) | Color de emisión. |

 

El valor de Cₑ es el color 1 o el 2. Si no se proporciona el color de vértice, se usa el color de emisión material.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


En este ejemplo, el objeto se colorea con la luz ambiente de la escena y un color ambiente del material.

Según la ecuación, el color resultante para los vértices del objeto es el color del material.

En la siguiente ilustración se muestra el color del material, que es el verde. La luz de emisión ilumina todos los vértices del objeto con el mismo color. No depende de la normal del vértice ni de la dirección de la luz. Como consecuencia, la esfera parece un círculo en 2D porque no existe ninguna diferencia en sombreado alrededor de la superficie del objeto.

![ilustración de una esfera verde](images/lighte.jpg)

En la siguiente ilustración se muestra cómo la luz de emisión se combina con los otros tres tipos de luces. En el lado derecho de la esfera, hay una mezcla de la luz de emisión verde y la luz ambiental roja. En el lado izquierdo de la esfera, la luz de emisión verde se combina con la luz roja ambiente y la luz difusa, lo que produce un degradado de color rojo. El resaltado especular es blanco en el centro y crea un anillo amarillo a medida que el valor de luz especular disminuye drásticamente dejando paso al ambiente, que se forma con valores de luz difusos y de emisión que se entremezclan para crear el color amarillo.

![Ilustración de una esfera con luz de emisión verde](images/lightadse.jpg)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cálculos de iluminación](mathematics-of-lighting.md)

 

 




