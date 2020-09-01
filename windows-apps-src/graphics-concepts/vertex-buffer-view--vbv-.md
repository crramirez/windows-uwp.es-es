---
title: Vista de búfer de vértices (VBV) y de búfer de índices (IBV)
description: Obtenga información sobre la vista del búfer de vértices (VBV) y la vista del búfer de índice (IBV), que contienen los datos y los índices enteros para los vértices en la representación de Direct3D.
ms.assetid: 695115D2-9DA0-41F2-9416-33BFAB698129
keywords:
- Vista de búfer de vértices (VBV)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a616f2bad8f478b2d20e96b183ba944950fef8a8
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156099"
---
# <a name="vertex-buffer-view-vbv-and-index-buffer-view-ibv"></a>Vista de búfer de vértices (VBV) y de búfer de índices (IBV)


Un búfer de vértices contiene datos para una lista de vértices. Los datos de cada vértice pueden incluir la posición, el color, el vector normal, las coordenadas de textura, etc. Un búfer de índice contiene índices enteros (desplazamientos) en un búfer de vértices y se usa para definir y representar un objeto compuesto por un subconjunto de la lista completa de vértices.

La definición de un solo vértice suele ser la aplicación que se va a definir, por ejemplo:

``` syntax
struct CUSTOMVERTEX { 
        FLOAT x, y, z;      // The position
        FLOAT nx, ny, nz;   // The normal
        DWORD color;        // RGBA color
        FLOAT tu, tv;       // The texture coordinates. 
}; 
```

La definición de CUSTOMVERTEX se pasaría al controlador de gráficos al crear búferes de vértices.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




