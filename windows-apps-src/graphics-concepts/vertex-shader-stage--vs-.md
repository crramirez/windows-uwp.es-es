---
title: "Fase del sombreador de vértices (VS)"
description: "La fase del sombreador de vértices procesa vértices, normalmente realizando ciertas operaciones como transformar, enmascarar e iluminar. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida."
ms.assetid: 5133C4BB-B4E6-4697-9276-F718AD44869C
keywords: "Fase del sombreador de vértices (VS)"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 761296a9897ad99ec57527f073a1ad1c2d792966
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="vertex-shader-vs-stage"></a>Fase del sombreador de vértices (VS)


La fase del sombreador de vértices procesa vértices, normalmente realizando ciertas operaciones como transformar, enmascarar e iluminar. Un sombreador de vértices toma un solo vértice de entrada y produce un solo vértice de salida.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


La fase del sombreador de vértices (VS) se usa para el procesamiento individual por vértice, como por ejemplo:

-   Transformaciones
-   Aplicación de máscaras
-   Transformación
-   Iluminación por vértice

La fase del sombreador de vértices es una fase de sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md). Esta fase del sombreador usa el modelo de sombreador 4.0 [sombreador común básico](https://msdn.microsoft.com/library/windows/desktop/bb509580).

La fase del sombreador de vértices (VS) procesa los vértices de ensamblador de entrada. Los sombreadores de vértices trabajan con un solo vértice de entrada y producen un solo vértice de salida. La fase del sombreador de vértices siempre debe estar activa para que se ejecute la canalización. Si no se requiere ninguna modificación o transformación de vértice, debe crearse un sombreador de vértices transparente y establecerlo en la canalización.

Cada vértice de entrada de sombreador de vértices puede comprender hasta 16 vectores de 32 bits (hasta 4 componentes cada uno). Cada vértice de salida puede comprender hasta 16 vectores de 4 componentes de 32 bits. Todos los sombreadores de vértices deben tener como mínimo una entrada y una salida, que puede ser tan solo un valor escalar.

La fase del sombreador de vértices puede consumir dos valores generados por el sistema del ensamblador de entrada: VertexID y InstanceID (consulta Valores del sistema y semántica). Dado que VertexID y InstanceID son significativos en un nivel de vértices, y solo se pueden enviar identificadores generados por hardware en la primera fase que los entiende, estos valores de identificador solo se pueden enviar a la fase del sombreador de vértices.

Los sombreadores de vértices siempre se ejecutan en todos los vértices, incluidos los vértices adyacentes en topologías primitivas de entrada con proximidad. Se puede consultar el número de veces que se ha ejecutado el sombreador de vértices de la CPU con las estadísticas de canalización VSInvocations.

Un sombreador de vértices puede realizar operaciones de muestreo de textura y carga, donde no se requieren derivados del espacio de pantalla (con funciones intrínsecas HLSL: [Muestra (objeto de textura de DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509695), [SampleCmpLevelZero (objeto de textura de DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509697), y [SampleGrad (objeto de textura de DirectX HLSL)](https://msdn.microsoft.com/library/windows/desktop/bb509698)).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


Un solo vértice, con valores generados por el sistema VertexID e InstanceID. Cada vértice de entrada de sombreador de vértices puede comprender hasta 16 vectores de 32 bits (hasta 4 componentes cada uno).

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


Un solo vértice. Cada vértice de salida puede comprender hasta 16 vectores de 4 componentes de 32 bits.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 



