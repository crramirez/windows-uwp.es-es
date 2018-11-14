---
title: Fase del sombreador de geometría (GS)
description: 'La fase del sombreador de geometría (GS) procesa primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes.'
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Fase del sombreador de geometría (GS)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c4659ee4200915a7cc82f46c90f0e53965f322d5
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6275642"
---
# <a name="geometry-shader-gs-stage"></a>Fase del sombreador de geometría (GS)


La fase del sombreador de geometría (GS) procesa primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Resulta útil para los algoritmos, como Point Sprite Expansion, Dynamic Particle Systems y Shadow Volume Generation. Admite la amplificación y la desamplificación geométrica.

## <a name="span-idpurposeandusesspanspan-idpurposeandusesspanspan-idpurposeandusesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Finalidad y usos


La fase del sombreador de geometría procesa primitivos completos: triángulos (3 vértices con un máximo de 3 vértices adyacentes), líneas (2 vértices con un máximo de 2 vértices adyacentes) y puntos (1 vértice).

![ilustración de un triángulo y una línea con vértices adyacentes](images/d3d10-gs.png)

El sombreador de geometría también admite la amplificación y la desamplificación geométrica limitada. Dado un primitivo de entrada, el sombreador de geometría puede descartar el primitivo o emitir uno o más primitivos nuevos.

La fase del sombreador de geometría (GS) es una fase de sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md). Esta fase del sombreador expone su propia funcionalidad única, integrada en modelos del sombreador (consulta [common-shader core](https://msdn.microsoft.com/library/windows/desktop/bb509580)).

La fase del sombreador de geometría es adecuada para algoritmos como los siguientes:

-   Point Sprite Expansion
-   Dynamic Particle Systems
-   Fur/Fin Generation
-   Shadow Volume Generation
-   Single Pass Render-to-Cubemap
-   Per-Primitive Material Swapping
-   Per-Primitive Material Setup: esta funcionalidad incluye la generación de coordenadas baricéntricas como datos primitivos para que un sombreador de píxeles pueda realizar la interpolación del atributo personalizado.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


La fase del sombreador de geometría ejecuta código de sombreador específico de la aplicación con primitivos completos como entrada y la capacidad de generar vértices de salida. A diferencia de los sombreadores de vértices, que trabajan en un solo vértice, las entradas del sombreador de geometría son los vértices de un primitivo completo (tres vértices para triángulos, dos vértices para líneas o un solo vértice para el punto). Los sombreadores de geometría también pueden incorporar los datos de vértice de los primitivos de bordes adyacentes como entrada (tres adicionales para un triángulo, dos vértices adicionales para una línea).

La fase del sombreador de geometría puede consumir el valor generado por el sistema **SV\_PrimitiveID** que se genera automáticamente en la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md). Esto permite capturar o calcular los datos de cada primitivo si se quiere.

Cuando un sombreador de geometría está activo, se invoca una vez para cada primitivo que se pasa por la canalización o se genera antes de ella. Cada invocación del sombreador de geometría considera como entrada los datos del primitivo que se invoca, ya sea un punto único, una sola línea o un solo triángulo. Una franja de triángulos anterior en la canalización produciría una invocación del sombreador de geometría para cada triángulo individual en la franja (como si la franja se expandiera en una lista de triángulos). Todos los datos de entrada de cada vértice del primitivo individual están disponibles (es decir, 3 vértices por triángulo), además de los datos del vértice adyacente, si corresponde y estos están disponibles.

Abreviaturas de vértices comunes:

|     |                 |
|-----|-----------------|
| TV  | Vértice de triángulo |
| LV  | Vértice de línea     |
| AV  | Vértice adyacente |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


La fase del sombreador de geometría (GS) es capaz de presentar varios vértices que forman una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son **tristrip**, **linestrip** y **pointlist**. El número de primitivos emitido puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podría emitirse debe declararse de forma estática. Las longitudes de franja emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias, y pueden crearse nuevas franjas a través de la función HLSL [RestartStrip](https://msdn.microsoft.com/library/windows/desktop/bb509660).

La ejecución de una instancia del sombreador de geometría es atómica de las otras invocaciones, excepto en que los datos agregados a las secuencias son de serie. Las salidas de una invocación determinada de un sombreador de geometría son independientes de otras invocaciones (aunque se respeta la ordenación). Un sombreador de geometría que genera franjas de triángulos iniciará una nueva franja en cada invocación.

La salida del sombreador de geometría puede alimentarse en la fase del rasterizador y/o en un búfer de vértice de la memoria a través de la fase de salida de flujo. La salida alimentada en la memoria se expande en listas de triángulos, líneas o puntos individuales (al igual que si se pasaran al rasterizador).

Un sombreador de geometría devuelve datos de un vértice cada vez anexando vértices a un objeto de secuencia de salida. La topología de las secuencias se determina mediante una declaración fija, al elegir **TriangleStream**, **LineStream** y **PointStream** como el resultado de la fase de GS.

Hay tres tipos de objetos de secuencia disponibles: **TriangleStream**, **LineStream** y **PointStream**, todos procedentes de plantillas. La topología de la salida viene determinada por su tipo de objeto correspondiente, mientras que el formato de los vértices que se anexa a la secuencia está determinado por el tipo de plantilla.

Cuando una salida del sombreador de geometría se identifica como un valor interpretado por el sistema (por ejemplo, **SV\_RenderTargetArrayIndex** o **SV\_Position**), el hardware examina estos datos y realiza algún tipo de comportamiento dependiente del valor y, además, es capaz de pasar los datos a la siguiente fase del sombreador para la entrada. Cuando esa salida de datos del sombreador de geometría tiene significado para el hardware en función del primitivo (como **SV\_RenderTargetArrayIndex** o **SV\_ViewportArrayIndex**), en lugar de en función del vértice (como **SV\_ClipDistance\ [n\]** o **SV\_Position**), los datos de cada primitivo se toman desde el vértice principal emitido para el primitivo.

El sombreador de geometría podría generar los primitivos completados parcialmente si el sombreador de geometría termina y el primitivo está incompleto. Los primitivos incompletos se descartan de forma automática. Esto es similar a la manera en que IA trata los primitivos parcialmente completados.

El sombreador de geometría puede realizar operaciones de muestreo de textura y carga, donde no se requieren derivados del espacio de pantalla (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




