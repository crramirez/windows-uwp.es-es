---
title: Listas de triángulos
description: Una lista de triángulos es una lista de triángulos aislados. Los triángulos aislados podrían o no estar próximos entre sí. Una lista de triángulos debe tener al menos tres vértices, y el número total de vértices debe ser divisible por tres.
ms.assetid: BC50D532-9E9C-4AAE-B466-9E8C4AD1862A
keywords:
- Listas de triángulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: aae797db890c6bee141c3b4a79a6a85a55a6b512
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7842288"
---
# <a name="triangle-lists"></a>Listas de triángulos


Una lista de triángulos es una lista de triángulos aislados. Los triángulos aislados podrían o no estar próximos entre sí. Una lista de triángulos debe tener al menos tres vértices, y el número total de vértices debe ser divisible por tres.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


Usa listas de triángulos para crear un objeto que se compone de partes desligadas. Por ejemplo, una forma de crear una pared de fuerza de campo en un juego 3D es especificar una larga lista de triángulos pequeños, desconectados. A continuación, aplica un material y textura que parece emitir luz a la lista de triángulos. Cada triángulo de la pared parece brillante. La escena detrás de la pared puede verse parcialmente a través de los espacios entre los triángulos, como un jugador podría esperar cuando mira un campo de fuerza.

Las listas de triángulos también son útiles para crear primitivos que tienen bordes nítidos y sombreados con sombreado Gouraud. Ver [Vectores normales de caras y vértices](face-and-vertex-normal-vectors.md).

La ilustración siguiente muestra una lista de triángulos representados.

![Ilustración de una lista de triángulos representada](images/trilist.png)

El siguiente código muestra cómo crear vértices para esta lista de triángulos.

```
struct CUSTOMVERTEX
{
    float x,y,z;
};

CUSTOMVERTEX Vertices[] = 
{
    {-5.0, -5.0, 0.0},
    { 0.0,  5.0, 0.0},
    { 5.0, -5.0, 0.0},
    {10.0,  5.0, 0.0},
    {15.0, -5.0, 0.0},
    {20.0,  5.0, 0.0}

};
```

El siguiente ejemplo de código muestra cómo representar esta lista de triángulos en Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLELIST, 0, 2 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Primitivos](primitives.md)

 

 




