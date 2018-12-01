---
title: Franjas de triángulos
description: Una serie de triángulos es una serie de triángulos conectados. Dado que los triángulos están conectados, la aplicación no tiene que especificar repetidamente los tres vértices de cada triángulo.
ms.assetid: BACC74C5-14E5-4ECC-9139-C2FD1808DB82
keywords:
- Franjas de triángulos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9a420ed5ed8f498eb9c900cbacb1b766c4a01214
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8347498"
---
# <a name="triangle-strips"></a>Franjas de triángulos


Una serie de triángulos es una serie de triángulos conectados. Dado que los triángulos están conectados, la aplicación no tiene que especificar repetidamente los tres vértices de cada triángulo. Por ejemplo, solo necesitas siete vértices para definir la franja de triángulos siguiente.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


![Ilustración de una franja de triángulos con siete vértices](images/tristrip.png)

El sistema usa vértices v1, v2 y v3 para dibujar el primer triángulo; v2, v4 y v3 para dibujar el segundo triángulo; V3, v4 y v5 para dibujar el tercero; V4, v6 y v5 para dibujar el cuarto; y así sucesivamente. Ten en cuenta que los vértices de los triángulos segundo y cuarto no están en orden; esto es necesario para asegurarse de que todos los triángulos se dibujan en la orientación de las agujas del reloj.

La mayoría de los objetos en escenas 3D se componen de franjas de triángulos. El motivo es que las franjas de triángulos pueden usarse para especificar los objetos complejos de manera que haga un uso eficaz del tiempo de procesamiento y memoria.

La ilustración siguiente muestra una franja de triángulos representados.

![Ilustración de una franja de triángulos representada](images/tstrip2.png)

El siguiente código muestra cómo crear vértices para esta franja de triángulos.

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

El siguiente ejemplo de código muestra cómo representar esta franja de triángulos en Direct3D.

```
//
// It is assumed that d3dDevice is a valid
// pointer to a device interface.
//
d3dDevice->DrawPrimitive( D3DPT_TRIANGLESTRIP, 0, 4);
```

## <a name="span-idpolygonsspanspan-idpolygonsspanspan-idpolygonsspanpolygons"></a><span id="Polygons"></span><span id="polygons"></span><span id="POLYGONS"></span>Polígonos


A menudo, las franjas de triángulos se usan para crear polígonos. Un polígono es una figura 3D cerrada delimitada por al menos tres vértices. El polígono más simple es un triángulo. Microsoft Direct3D usa triángulos para componer la mayor parte de sus polígonos porque los tres vértices en un triángulo están garantizados a ser coplanares. Representar vértices no planos es ineficaz. Puedes combinar triángulos para formar mallas y polígonos grandes y complejos.

En la siguiente ilustración se muestra un cubo. Dos triángulos forman cada lado del cubo. Todo el conjunto de triángulos constituye un primitivo cúbico. Puedes aplicar texturas a las superficies de los primitivos para hacer que aparezcan como una sola forma sólida. Para obtener más información, consulta [Texturas](textures.md).

![ilustración de un cubo con dos triángulos en cada lado](images/cube3d.png)

También puedes usar los triángulos para crear primitivos cuyas superficies parecen ser curvas suaves. La siguiente ilustración muestra cómo se puede simular una esfera con triángulos. Después de aplicar un material, se puede hacer que la esfera parezca curva cuando se procesa.

![ilustración de una esfera simulada mediante el uso de triángulos](images/sphere3d.png)

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Primitivos](primitives.md)

 

 




