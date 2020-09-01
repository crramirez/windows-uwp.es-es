---
title: Fase del sombreador de casco (HS)
description: La fase del sombreador de casco (HS) es una de las fases de teselación, que dividen de forma eficaz una sola superficie de un modelo en muchos triángulos.
ms.assetid: C62F6F15-CAD7-4C72-9735-00762E346C4C
keywords:
- Fase del sombreador de casco (HS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 11c9f0a52c4a320306cf5dfd5ce90fd5bbe2c407
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165019"
---
# <a name="hull-shader-hs-stage"></a>Fase del sombreador de casco (HS)

La fase del sombreador de casco (HS) es una de las fases de teselación, que dividen de forma eficaz una sola superficie de un modelo en muchos triángulos. La fase del sombreador de casco (HS) genera una revisión de geometría (y constantes de revisión) que corresponden a cada revisión de entrada (cuádruple, triángulo o línea). Un sombreador de casco se invoca una vez por revisión y transforma los puntos de control de entrada que definen una superficie de orden inferior en los puntos de control que componen una revisión. También realiza algunos cálculos por revisión para proporcionar datos para la [etapa del teselador (TS)](tessellator-stage--ts-.md) y la fase del [sombreador de dominios (DS)](domain-shader-stage--ds-.md).

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Propósito y usos


![diagrama de la fase del sombreador de casco](images/d3d11-hull-shader.png)

Las tres fases de teselación funcionan juntas para convertir superficies de orden superior (que mantienen el modelo de forma sencilla y eficaz) en muchos triángulos para la representación detallada dentro de la canalización de gráficos. Las fases de teselación incluyen la fase del sombreador de casco (HS), la fase de [del teselador (TS)](tessellator-stage--ts-.md)y la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md).

La fase del sombreador de casco (HS) es una fase de sombreador programable. Un sombreador de casco se implementa con una función HLSL.

Un sombreador de casco funciona en dos fases: una fase de punto de control y una fase de revisión constante, que se ejecutan en paralelo por el hardware. El compilador de HLSL extrae el paralelismo en un sombreador de casco y lo codifica en código de bytes que controla el hardware.

-   La fase de punto de control funciona una vez para cada punto de control, Lee los puntos de control de una revisión y genera un punto de control de salida (identificado por un **ControlPointID**).
-   La fase patch-Constant funciona una vez por revisión para generar factores de teselación perimetral y otras constantes por revisión. Internamente, muchas fases constantes de revisión se pueden ejecutar al mismo tiempo. La fase patch-Constant tiene acceso de solo lectura a todos los puntos de control de entrada y salida.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


Entre 1 y 32 puntos de control de entrada, que juntos definen una superficie de orden inferior.

-   El sombreador de casco declara el Estado requerido por la [etapa del teselador (TS)](tessellator-stage--ts-.md). Esto incluye información como el número de puntos de control, el tipo de revisión y el tipo de partición que se va a usar cuando se teselar. Esta información aparece como declaraciones normalmente al principio del código del sombreador.
-   Los factores de teselación determinan la cantidad de subdividir cada revisión.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


Entre 1 y 32 puntos de control de salida, que juntos forman una revisión.

-   La salida del sombreador se encuentra entre 1 y 32 puntos de control, independientemente del número de factores de teselación. La fase del sombreador de dominio puede consumir la salida de los puntos de control de un sombreador de casco. Un sombreador de dominios puede consumir los datos constantes de revisión. Los factores de teselación pueden ser consumidos por la [fase del teselador (TS)](tessellator-stage--ts-.md) y la [fase del sombreador de dominios (DS)](domain-shader-stage--ds-.md).
-   Si el sombreador de casco establece cualquier factor de teselación perimetral en ≤ 0 o NaN, la revisión se seleccionará (se omitirá). Como resultado, la fase del teselador se puede ejecutar o no, el sombreador de dominios no se ejecutará y no se producirá ningún resultado visible para dicha revisión.

## <a name="span-idexamplespanspan-idexamplespanspan-idexamplespanexample"></a><span id="Example"></span><span id="example"></span><span id="EXAMPLE"></span>Ejemplo


```hlsl
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

Consulte [Cómo: crear un sombreador de casco](/windows/desktop/direct3d11/direct3d-11-advanced-stages-hull-shader-create).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 