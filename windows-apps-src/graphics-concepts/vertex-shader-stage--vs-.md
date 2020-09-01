---
title: Fase del sombreador de vértices (VS)
description: La fase del sombreador de vértices (VS) procesa los vértices, normalmente realizando operaciones como transformaciones, máscaras e iluminación. Un sombreador de vértices toma un solo vértice de entrada y genera un solo vértice de salida.
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords:
- Fase del sombreador de vértices (VS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 88a990470d0ff8aea5c6415584bd63b2b4b65981
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156059"
---
# <a name="vertex-shader-vs-stage"></a>Fase del sombreador de vértices (VS)


La fase del sombreador de vértices (VS) procesa los vértices, normalmente realizando operaciones como transformaciones, máscaras e iluminación. Un sombreador de vértices toma un solo vértice de entrada y genera un solo vértice de salida.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Propósito y usos


La fase del sombreador de vértices (VS) se usa para el procesamiento individual por vértices, como:

-   Transformaciones
-   Máscaras
-   Transformar
-   Iluminación por vértice

La fase del sombreador de vértices es una fase de sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md) . Esta fase del sombreador usa el modelo de sombreador 4,0 [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core).

La fase del sombreador de vértices (VS) procesa los vértices del ensamblador de entrada. Los sombreadores de vértices siempre operan en un solo vértice de entrada y producen un único vértice de salida. La fase del sombreador de vértices siempre debe estar activa para que se ejecute la canalización. Si no se requiere ninguna modificación o transformación de vértices, debe crearse un sombreador de vértices de paso a través y establecerse en la canalización.

Cada vértice de entrada del sombreador de vértices puede constar de hasta 16 32 vectores de bits (hasta 4 componentes cada uno). Cada vértice de salida puede constar de un máximo de 16 32 vectores de componentes de 4 bits. Todos los sombreadores de vértices deben tener un mínimo de una entrada y una salida, que puede ser tan pequeña como un valor escalar.

La fase del sombreador de vértices puede consumir dos valores generados por el sistema del ensamblador de entrada: VertexID y InstanceID (consulte valores y semántica del sistema). Dado que VertexID y InstanceID son significativos en el nivel de vértice, y los identificadores generados por el hardware solo se pueden introducir en la primera fase que los comprenda, estos valores de identificador solo se pueden introducir en la fase del sombreador de vértices.

Los sombreadores de vértices siempre se ejecutan en todos los vértices, incluidos los vértices adyacentes en topologías primitivas de entrada con adyacencias. El número de veces que se ha ejecutado el sombreador de vértices se puede consultar desde la CPU mediante la estadística de canalización VSInvocations.

Un sombreador de vértices puede realizar operaciones de muestreo de la carga y de la textura en las que no se necesitan los derivados del espacio de pantalla (mediante las funciones intrínsecas de HLSL: [ejemplo (objeto de textura de HLSL de DirectX)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-sample), [SampleCmpLevelZero (objeto de textura de DirectX HLSL)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplecmplevelzero)y [SampleGrad (objeto de textura de HLSL de DirectX)](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-samplegrad)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


Un solo vértice, con los valores generados por el sistema VertexID y InstanceID. Cada vértice de entrada del sombreador de vértices puede constar de hasta 16 32 vectores de bits (hasta 4 componentes cada uno).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


Un solo vértice. Cada vértice de salida puede constar de un máximo de 16 32 vectores de componentes de 4 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 