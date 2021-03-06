---
title: Series de líneas
description: Una serie de líneas es un tipo de primitivo que se compone de segmentos de líneas conectados. La aplicación puede usar las series de líneas para crear polígonos que no están cerrados. Un polígono cerrado es un polígono cuyo último vértice está conectado a su primer vértice con un segmento de línea.
ms.assetid: 6E8C58E1-B463-44FD-A69F-81CCBF25D856
keywords:
- Series de líneas
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d17a79c14e981ab0c2c0414074aff17c90a0b478
ms.sourcegitcommit: 82edc63a5b3623abce1d5e70d8e200a58dec673c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58291633"
---
# <a name="line-strips"></a>Series de líneas

Una serie de líneas es un tipo de primitivo que se compone de segmentos de líneas conectados. La aplicación puede usar las series de líneas para crear polígonos que no están cerrados. Un polígono cerrado es un polígono cuyo último vértice está conectado a su primer vértice con un segmento de línea. Si la aplicación realiza polígonos basados en series de líneas, no se garantiza que los vértices estén en el mismo plano.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


La siguiente ilustración muestra una serie de líneas representada.

![ilustración de una serie de líneas](images/linstrip.gif)

El siguiente código muestra cómo crear vértices para esta serie de líneas.

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

El siguiente ejemplo de código muestra cómo representar una serie de líneas en Direct3D.

```cpp
//
// It is assumed that d3dDevice is a valid
// pointer to an IDirect3DDevice interface.
//
d3dDevice->DrawPrimitive( D3DPT_LINESTRIP, 0, 5 );
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Tipos primitivos](primitives.md)

 

 




