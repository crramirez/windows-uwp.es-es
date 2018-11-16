---
title: Vista de búfer de vértices (VBV) y de búfer de índices (IBV)
description: Un búfer de vértices contiene los datos de una lista de vértices.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Vista de búfer de vértices (VBV)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7956c7a03256da04e98a5b8f9a33951d7e0216d3
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995200"
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

 

 




