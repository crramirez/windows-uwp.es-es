---
title: "Vista de búfer de vértices (VBV) y de búfer de índices (IBV)"
description: "Un búfer de vértices contiene los datos de una lista de vértices."
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords: "Vista de búfer de vértices (VBV)"
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: da0b8e8841e7c9df88ea22ba637d7236090e7ee5
ms.sourcegitcommit: c80b9e6589a1ee29c5032a0b942e6a024c224ea7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
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

 

 




