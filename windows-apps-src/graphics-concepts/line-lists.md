---
title: Listas de líneas
description: Una lista de líneas es una lista de segmentos de línea recta aislados. Las listas de líneas son útiles para tareas como agregar aguanieve o lluvia torrencial a una escena 3D. Las aplicaciones crean una lista de líneas rellenando una matriz de vértices.
ms.assetid: 42BF32A1-3535-42A3-82C5-3945CB309F2C
keywords:
- Listas de líneas
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5a514fb072dd95b9de2c29e097e8387c0f1068a4
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5710710"
---
# <a name="line-lists"></a>Listas de líneas


Una lista de líneas es una lista de segmentos de línea recta aislados. Las listas de líneas son útiles para tareas como agregar aguanieve o lluvia torrencial a una escena 3D. Las aplicaciones crean una lista de líneas rellenando una matriz de vértices. Ten en cuenta que el número de vértices de una lista de líneas debe ser un número par superior o igual a dos.

-   [Ejemplo](#example)
-   [Temas relacionados](#related-topics)

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


La siguiente ilustración muestra una lista de líneas representada.

![ilustración de una lista de líneas](images/linelst.png)

Puedes aplicar texturas y materiales a una lista de líneas. Los colores de la textura o el material aparecen solo a lo largo de las líneas dibujadas, no en cualquier punto entre las líneas.

El siguiente código muestra cómo crear vértices para esta lista de líneas.

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

El siguiente ejemplo de código muestra cómo representar una lista de líneas en Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINELIST, 0, 3 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Primitivos](primitives.md)

 

 




