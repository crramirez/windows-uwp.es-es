---
title: Listas de puntos
description: Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono.
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords:
- Listas de puntos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3f59d86a03abdeb097ab60e1961d7869669875eb
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291593"
---
# <a name="point-lists"></a>Listas de puntos

Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


La ilustración siguiente muestra una lista de puntos representados.

![Ilustración de una lista de puntos](images/pointlst.png)

La aplicación puede aplicar materiales y texturas a una lista de puntos. Los colores del material o la textura aparecen solo en los puntos dibujados y no en otros lugares entre los puntos.

El siguiente código muestra cómo crear vértices para esta lista de puntos.

```cpp
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

El siguiente ejemplo de código muestra cómo representar esta lista de puntos en Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Tipos primitivos](primitives.md)

 

 




