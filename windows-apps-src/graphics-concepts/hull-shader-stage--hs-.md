---
title: Fase del sombreador de casco (HS)
description: La fase del sombreador de casco (HS) es una de las fases de teselación, que divide de forma eficaz una sola superficie de un modelo en muchos triángulos.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Fase del sombreador de casco (HS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d2aed18d476f966e644fa095aa6a5a518ebbe959
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5839366"
---
# <a name="hull-shader-hs-stage"></a>Fase del sombreador de casco (HS)


La fase del sombreador de casco (HS) es una de las fases de teselación, que divide de forma eficaz una sola superficie de un modelo en muchos triángulos. La fase del sombreador de casco (HS) produce una revisión de geometría (y constantes de revisión) que corresponden a cada revisión de entrada (cuadrado, triángulo o línea). Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en puntos de control que conforman una revisión. También hace algunos cálculos por revisión para proporcionar datos de la [fase del teselador (TS)](tessellator-stage--ts-.md) y la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md).

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


![diagrama de la fase del sombreador de casco](images/d3d11-hull-shader.png)

Las tres fases de teselación funcionan juntas para convertir las superficies de orden superior (que mantienen el modelo sencillo y eficaz) en muchos triángulos para la representación detallada dentro de la canalización de gráficos. Las fases de teselación incluyen la fase del sombreador de casco (HS), la [fase del teselador (TS)](tessellator-stage--ts-.md), y la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md).

La fase del sombreador de casco (HS) es una fase del sombreador programable. Un sombreador de casco se implementa con una función HLSL.

Un sombreador de casco funciona en dos fases: una fase de punto de control y una fase de constante de revisión, que el hardware ejecuta en paralelo. El compilador HLSL extrae el paralelismo en un sombreador de casco y lo codifica en código de bytes que controla el hardware.

-   La fase de punto de control funciona una vez para cada punto de control, ya que lee los puntos de control para obtener una revisión y genera uno punto de control de salida (identificado por un **ControlPointID**).
-   La fase de constante de revisión funciona una vez por revisión para generar factores de teselación de borde y otras constantes por revisión. Internamente, pueden ejecutarse varias fases de constante de revisión al mismo tiempo. La fase de constante de revisión tiene acceso de solo lectura a todos los puntos de control de entrada y salida.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Entre 1 y 32 puntos de control de entrada, que definen una superficie de orden inferior.

-   El sombreador de casco declara el estado requerido por la [fase del teselador (TS)](tessellator-stage--ts-.md). Esto incluye información, como el número de puntos de control, el tipo de cara de revisión y el tipo de partición que se debe usar durante la teselación. Esta información normalmente se muestra en forma de declaraciones en la parte frontal del código del sombreador.
-   Los factores de teselación determinarán en cuántas partes debe subdividirse cada revisión.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


Entre 1 y 32 puntos de control de salida, que juntos forman una revisión.

-   La salida del sombreador está entre 1 y 32 puntos de control, independientemente del número de factores teselación. La salida de los puntos de control de un sombreador de casco puede consumirse en la fase del sombreador de dominios. El sombreador de dominios puede consumir los datos de la constante de revisión. Los factores de teselación pueden consumirse en la [fase del teselador (TS)](tessellator-stage--ts-.md) y la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md).
-   Si el sombreador de casco establece cualquier factor de teselación de borde inferior o igual a 0 o NaN, se seleccionará la revisión (se omite). Como resultado, la fase del teselador puede ejecutarse o no, no se ejecutará el sombreador de dominios y no se producirá ningún resultado visible para esa revisión.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Por ejemplo:


```
[patchsize(12)]
[patchconstantfunc(MyPatchConstantFunc)]
MyOutPoint main(uint Id : SV_ControlPointID,
     InputPatch<MyInPoint, 12> InPts)
{
     MyOutPoint result;
     
     ...
     
     result = TransformControlPoint( InPts[Id] );

     return result;
}
```

Consulta [Crear un sombreador de casco](https://msdn.microsoft.com/library/windows/desktop/ff476338).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




