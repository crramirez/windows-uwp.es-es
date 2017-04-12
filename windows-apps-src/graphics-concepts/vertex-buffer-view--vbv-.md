---
title: "Vista de búfer de vértices (VBV) y de búfer de índices (IBV)"
description: "Un búfer de vértices contiene los datos de una lista de vértices."
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords: "Vista de búfer de vértices (VBV)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 502b0e4816e31ebace93d3250f7da335d2540272
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Vista de búfer de vértices (VBV) y de búfer de índices (IBV)


Un búfer de vértices contiene los datos de una lista de vértices. Los datos para cada vértice pueden incluir posición, color, vector normal, coordenadas de textura, etc. Un búfer de índices contiene índices de enteros (desplazamientos) en un búfer de vértices y se usa para definir y representar un objeto que se compone de un subconjunto de la lista completa de vértices.

A menudo le corresponde a la aplicación indicar la definición de un solo vértice a menudo, como por ejemplo:

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

La definición de CUSTOMVERTEX, pasaría al controlador de gráficos al crear búferes de vértices.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




