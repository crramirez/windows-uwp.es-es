---
title: Listas de puntos
description: "Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono."
ms.assetid: 332954AE-019F-4A91-B773-E3A7C92F3297
keywords: Listas de puntos
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: d5f2ee1c9a42a0d2f380f4f9a98c204088b1379a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="point-lists"></a>Listas de puntos


Una lista de puntos es una colección de vértices que se representan como puntos aislados. La aplicación puede utilizar listas de puntos en escenas 3D para campos de asteriscos, o bien líneas de puntos en la superficie de un polígono.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


La ilustración siguiente muestra una lista de puntos representados.

![Ilustración de una lista de puntos](images/pointlst.png)

La aplicación puede aplicar materiales y texturas a una lista de puntos. Los colores del material o la textura aparecen solo en los puntos dibujados y no en otros lugares entre los puntos.

El siguiente código muestra cómo crear vértices para esta lista de puntos.

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

El siguiente ejemplo de código muestra cómo representar esta lista de puntos en Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_POINTLIST, 0, 6 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Primitivos](primitives.md)

 

 




