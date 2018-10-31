---
title: Fase del sombreador de dominios (DS)
description: La fase del sombreador de dominios (DS) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice correspondiente a cada muestra de dominio.
ms.assetid: 673CC04A-A74F-495F-AFB7-49157538749C
keywords:
- Fase del sombreador de dominios (DS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3bcad4a5e22249d4d7faed08fe9cc9af4c3fb338
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5840366"
---
# <a name="domain-shader-ds-stage"></a>Fase del sombreador de dominios (DS)


La fase del sombreador de dominios (DS) calcula la posición del vértice de un punto subdividido en la revisión de salida; calcula la posición del vértice correspondiente a cada muestra de dominio. Un sombreador de dominios se ejecuta una vez por punto de salida de la fase del teselador y tiene acceso de solo lectura a las constantes de revisión de salida y de revisión de salida del sombreador de casco, y a las coordenadas de UV de salida de la fase del teselador.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


La fase del sombreador de dominios (DS) devuelve la posición del vértice de un punto subdividido de la revisión de salida, en función de la entrada de la [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md) y la [fase del teselador (TS)](tessellator-stage--ts-.md).

![diagrama de la fase del sombreador de dominios](images/d3d11-domain-shader.png)

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


-   Un sombreador de dominios consume puntos de control de salida de la [fase del sombreador de casco (HS)](hull-shader-stage--hs-.md). Entre los resultados del sombreador de casco se incluyen:
    -   Puntos de control.
    -   Datos de constantes de revisión.
    -   Factores de teselación. Los factores de teselación pueden incluir los valores que usa el teselador de función fija, así como los valores sin procesar (antes del redondeo mediante la teselación de enteros, por ejemplo), lo que facilita la geotransformación, por ejemplo.
-   Un sombreador de dominios se invoca una vez por coordenada de salida de la [fase del teselador (TS)](tessellator-stage--ts-.md).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


-   La fase del sombreador de dominios (DS) devuelve la posición del vértice de un punto subdividido de la revisión de salida.

Cuando se completa el sombreador de dominios, la teselación finaliza y los datos de la canalización continúan a la siguiente fase de canalización, como la [fase del sombreador de geometría (GS)](geometry-shader-stage--gs-.md) y [fase del sombreador de píxeles (PS)](pixel-shader-stage--ps-.md). Un sombreador de geometría que espera primitivos con adyacencia (por ejemplo, 6 vértices por triángulo) no es válido cuando la teselación está activa (esto da como resultado un comportamiento definido, del que se queja la capa de depuración).

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


```
void main( out    MyDSOutput result, 
           float2 myInputUV : SV_DomainPoint, 
           MyDSInput DSInputs,
           OutputPatch<MyOutPoint, 12> ControlPts, 
           MyTessFactors tessFactors)
{
     ...

     result.Position = EvaluateSurfaceUV(ControlPoints, myInputUV);
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




