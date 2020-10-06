---
title: Fase de sombreador de geometría (GS)
description: La fase del sombreador de geometría (GS) procesa triángulos, líneas y puntos primitivos enteros, junto con sus vértices adyacentes.
ms.assetid: 8A1350DD-B006-488F-9DAF-14CD2483BA4E
keywords:
- Fase de sombreador de geometría (GS)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e4573547ebffa0a58b4e2347162799035d8f898
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750011"
---
# <a name="geometry-shader-gs-stage"></a>Fase de sombreador de geometría (GS)


La fase del sombreador de geometría (GS) procesa los primitivos completos: triángulos, líneas y puntos, junto con sus vértices adyacentes. Resulta útil para los algoritmos, como la expansión de sprites de punto, los sistemas de partículas dinámicas y la generación de volúmenes de instantáneas. Admite amplificación geométrica y desamplificación.

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>Propósito y usos


La fase del sombreador de geometría procesa primitivos enteros: triángulos (3 vértices con hasta 3 vértices adyacentes), líneas (2 vértices con hasta 2 vértices adyacentes) y puntos (1 vértice).

![Ilustración de un triángulo y una línea con vértices adyacentes](images/d3d10-gs.png)

El sombreador de geometría también admite amplificación y desamplificación de geometría limitada. Dado un primitivo de entrada, el sombreador de geometría puede descartar la primitiva o emitir una o más primitivas nuevas.

La fase del sombreador de geometría (GS) es una fase de sombreador programable; se muestra como un bloque redondeado en el diagrama de [canalización de gráficos](graphics-pipeline.md) . Esta fase del sombreador expone su propia funcionalidad única, basada en los modelos de sombreador (vea [Common-Shader Core](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-common-core)).

La fase del sombreador de geometría es adecuada para los algoritmos, incluidos:

-   Expansión de Sprite de punto
-   Sistemas de partículas dinámicas
-   Generación de peletería/fin
-   Generación de volúmenes de instantáneas
-   Representación de un solo paso a mapa
-   Intercambio de material por primitivo
-   Configuración de material por primitivo: esta funcionalidad incluye la generación de coordenadas Barycentric como datos primitivos para que un sombreador de píxeles pueda realizar la interpolación de atributos personalizados.

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


La fase del sombreador de geometría ejecuta código de sombreador especificado por la aplicación con primitivos completos como entrada y la capacidad de generar vértices en la salida. A diferencia de los sombreadores de vértices, que operan en un solo vértice, las entradas del sombreador de geometría son los vértices de un primitivo completo (tres vértices para triángulos, dos vértices para las líneas o un solo vértice para el punto). Los sombreadores de geometría también pueden traer los datos de vértices de los primitivos adyacentes perimetrales como entrada (tres más para un triángulo, dos vértices adicionales para una línea).

La fase del sombreador de geometría puede consumir el valor de **VP \_ PrimitiveID** generado por el sistema que genera automáticamente la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md). Esto permite capturar o calcular los datos por primitivos si se desea.

Cuando un sombreador de geometría está activo, se invoca una vez para cada primitiva que se pasa o se genera anteriormente en la canalización. Cada invocación del sombreador de geometría ve como entrada los datos para la invocación de primitivos, ya sea un único punto, una sola línea o un solo triángulo. Una franja de triángulos anterior en la canalización produciría una invocación del sombreador de geometría para cada triángulo individual de la franja (como si la franja estuviera expandida en una lista de triángulos). Todos los datos de entrada para cada vértice de la primitiva individual están disponibles (es decir, 3 vértices para un triángulo), además de los datos de vértice adyacentes, si procede y están disponibles.

Abreviaturas de vértice comunes:

| Abreviatura | Término |
| ------------ | ---- |
| TV  | Vértice de triángulo |
| LV  | Vértice de línea     |
| AV  | Vértice adyacente |

 

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


La fase del sombreador de geometría (GS) es capaz de generar varios vértices que formen una sola topología seleccionada. Las topologías de salida del sombreador de geometría disponibles son **trifranja**, **linestrip**y **PointList**. El número de primitivas emitidas puede variar libremente dentro de cualquier invocación del sombreador de geometría, aunque el número máximo de vértices que podrían emitirse se debe declarar de forma estática. Las longitudes de franjas emitidas desde una invocación del sombreador de geometría pueden ser arbitrarias y se pueden crear nuevas bandas a través de la función HLSL [RestartStrip](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-so-restartstrip) .

La ejecución de una instancia del sombreador de geometría es atómica desde otras invocaciones, salvo que los datos que se agregan a los flujos son serie. Las salidas de una invocación determinada de un sombreador de geometría son independientes de otras invocaciones (aunque se respeta la ordenación). Un sombreador de geometría que genera bandas triángulos iniciará una nueva franja en cada invocación.

La salida del sombreador de geometría se puede alimentar a la fase de rasterizador o a un búfer de vértice en la memoria a través de la fase de salida de flujo. La salida introducida en la memoria se expande a listas de puntos, líneas y triángulos individuales (exactamente como se pasarían al rasterizador).

Un sombreador de geometría genera datos de un vértice a la vez anexando vértices a un objeto de flujo de salida. La topología de las secuencias se determina mediante una declaración fija, eligiendo un **TriangleStream**, **LineStream** y **PointStream** como salida para la fase GS.

Hay tres tipos de objetos de secuencia disponibles: **TriangleStream**, **LineStream** y **PointStream**, que son todos los objetos con plantilla. La topología de la salida viene determinada por su tipo de objeto respectivo, mientras que el formato de los vértices anexados a la secuencia viene determinado por el tipo de plantilla.

Cuando una salida del sombreador de geometría se identifica como un valor interpretado por el sistema (por ejemplo, la ** \_ posición** **VP \_ RenderTargetArrayIndex** o VP), el hardware examina estos datos y realiza algún comportamiento dependiente del valor, además de poder pasar los datos en la siguiente fase del sombreador para la entrada. Cuando dicha salida de datos del sombreador de geometría tiene significado para el hardware en función de la primitiva (como la **VP \_ RenderTargetArrayIndex** o la **VP \_ ViewportArrayIndex**), en lugar de un carácter por vértice (como la **VP \_ ClipDistance \[ n \] ** o **la \_ posición SV**), los datos por primitivos se toman del vértice inicial emitido para el primitivo.

El sombreador de geometría puede generar primitivos parcialmente completados si el sombreador de geometría finaliza y el primitivo está incompleto. Los primitivos incompletos se descartan de forma silenciosa. Esto es similar a la forma en que el IA trata los primitivos parcialmente completados.

El sombreador de geometría puede realizar operaciones de muestreo de la carga y la textura en las que no se necesitan los derivados del espacio de pantalla (**samplelevel**, **samplecmplevelzero**, **samplegrad**).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 