---
title: Fase de fusión de salida (OM)
description: La fase de fusión de salida (OM) combina varios tipos de datos de salida (valores del sombreador de píxeles, información de profundidad y estarcido) con el contenido de los búferes de destino de representación y de profundidad/estarcido para generar el resultado final de la canalización.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Fase de fusión de salida (OM)
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 6478eaa2dadac74c22e5959623f03547f18babfc
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156319"
---
# <a name="output-merger-om-stage"></a>Fase de fusión de salida (OM)


La fase de fusión de salida (OM) combina varios tipos de datos de salida (valores del sombreador de píxeles, información de profundidad y estarcido) con el contenido de los búferes de destino de representación y de profundidad/estarcido para generar el resultado final de la canalización.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Propósito y usos


La fase de fusión de salida (OM) es el último paso para determinar qué píxeles son visibles (con pruebas de estarcido de profundidad) y mezclar los colores finales de los píxeles.

La fase OM genera el color de píxel representado final mediante una combinación de lo siguiente:

-   Estado de canalización
-   Los datos de píxeles generados por los sombreadores de píxeles
-   El contenido de los destinos de representación
-   El contenido de los búferes de profundidad/estarcido.

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Información general de blending

La mezcla combina uno o más valores de píxeles para crear un color de píxel final. En el diagrama siguiente se muestra el proceso implicado en la combinación de datos de píxeles.

![diagrama de cómo funciona la fusión de datos](images/d3d10-blend-state.png)

Conceptualmente, puede visualizar este diagrama de flujo implementado dos veces en la fase de fusión de salida: el primero combina los datos RGB, mientras que en paralelo, un segundo combina los datos alfa. Para ver cómo usar la API para crear y establecer el estado de Blend, consulte Configuración de la [funcionalidad de fusión](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-blend-state).

La mezcla de funciones fijas se puede habilitar de forma independiente para cada destino de representación. Sin embargo, solo hay un conjunto de controles de Blend, de modo que la misma mezcla se aplica a todos los RenderTargets con la fusión mediante combinación habilitada. Los valores de Blend (incluido BlendFactor) siempre se fijan en el intervalo del formato de destino de representación antes de la fusión. La compresión se realiza por destino de representación, respetando el tipo de destino de representación. La única excepción son los formatos float16, float11 o float10 que no están fijados, por lo que las operaciones de mezcla en estos formatos se pueden realizar con al menos la misma precisión/intervalo que el formato de salida. Los NaN y los ceros con signo se propagan para todos los casos (incluidos los pesos de 0,0 Blend).

Cuando se utilizan destinos de representación sRGB, el tiempo de ejecución convierte el color del destino de representación en un espacio lineal antes de realizar la fusión. El tiempo de ejecución convierte de nuevo el valor de fusión final en el espacio sRGB antes de guardar el valor en el destino de representación.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Fusión de colores de origen dual

Esta característica permite a la fase de fusión de salida usar simultáneamente ambas salidas de sombreador de píxeles (O0 y O1) como entradas de una operación de mezcla con el destino de representación único en la ranura 0. Entre las operaciones de mezcla válidas se incluyen: Add, Retract y revsubtract. La ecuación de Blend y la máscara de escritura de salida especifican qué componentes está generando el sombreador de píxeles. Se omiten los componentes adicionales.

La escritura en otras salidas del sombreador de píxeles (O2, O3 etc.) no está definida. no se puede escribir en un destino de representación si no está enlazado a la ranura 0. La escritura de oDepth es válida durante la combinación de colores de origen dual.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Información general sobre pruebas de estarcido de profundidad

Un búfer de estarcido de profundidad, que se crea como un recurso de textura, puede contener datos de profundidad y datos de estarcido. Los datos de profundidad se usan para determinar qué píxeles se encuentran más cerca de la cámara, y los datos de la galería de símbolos se usan para enmascarar los píxeles que se pueden actualizar. En última instancia, la fase de combinación de resultados utiliza los datos de los valores Depth y stencil para determinar si se debe dibujar un píxel. En el diagrama siguiente se muestra conceptualmente cómo se realizan las pruebas de profundidad de la galería de símbolos.

![diagrama de cómo funciona la prueba de profundidad-estarcido](images/d3d10-depth-stencil-test.png)

Para configurar las pruebas de profundidad y estarcido, consulte [configuración de la funcionalidad de la galería de símbolos](configuring-depth-stencil-functionality.md)de profundidad. Un objeto de galería de símbolos de profundidad encapsula el estado de la galería de símbolos de profundidad. Una aplicación puede especificar el estado de la galería de símbolos de profundidad o la fase OM usará los valores predeterminados. Las operaciones de combinación se realizan por píxel si el muestreo múltiple está deshabilitado. Si el muestreo múltiple está habilitado, la fusión se produce en cada muestreo.

El proceso de usar el búfer de profundidad para determinar qué píxel se debe dibujar se denomina almacenamiento en búfer de profundidad, también denominado almacenamiento en búfer z.

Una vez que los valores de profundidad llegan a la fase de fusión de salida (ya sea desde la interpolación o desde un sombreador de píxeles), siempre están fijados: z = min (viewport. MaxDepth, Max (viewport. MinDepth, z)) según el formato y la precisión del búfer de profundidad, mediante reglas de punto flotante. Después de la compresión, se compara el valor de profundidad (mediante DepthFunc) con el valor de búfer de profundidad existente. Si no se enlaza ningún búfer de profundidad, la prueba de profundidad siempre se supera.

Si no hay ningún componente de estarcido en el formato de búfer de profundidad o no hay ningún límite de búfer de profundidad, la prueba de estarcido siempre pasa.

Solo un búfer de profundidad/estarcido puede estar activo a la vez; cualquier vista de recursos enlazada debe coincidir con la vista de profundidad/estarcido (el mismo tamaño y dimensiones). Esto no significa que el tamaño del recurso debe coincidir, solo que el tamaño de la vista debe coincidir.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Información general de la máscara de ejemplo

Una máscara de ejemplo es una máscara de cobertura Multimuestra de 32 bits que determina qué muestras se actualizan en los destinos de representación activos. Solo se permite una máscara de ejemplo. El usuario define la asignación de bits en una máscara de ejemplo a los ejemplos de un recurso. En la representación de n-Sample, se usan los primeros n bits (del LSB) de la máscara de ejemplo (32 bits es el número máximo de bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entradas


La fase de fusión de salida (OM) genera el color de píxel representado final mediante una combinación de lo siguiente:

-   Estado de canalización
-   Los datos de píxeles generados por los sombreadores de píxeles
-   El contenido de los destinos de representación
-   El contenido de los búferes de profundidad/estarcido.

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Genere


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Salida: Introducción a la máscara de escritura

Use una máscara de salida-escritura para controlar (por componente) Qué datos se pueden escribir en un destino de representación.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Información general de varios destinos de representación

Un sombreador de píxeles se puede usar para representar al menos 8 destinos de representación independientes, todos ellos deben ser del mismo tipo (buffer, Texture1D, Texture1DArray, etc.). Además, todos los destinos de representación deben tener el mismo tamaño en todas las dimensiones (ancho, alto, profundidad, tamaño de la matriz, recuentos de muestras). Cada destino de representación puede tener un formato de datos diferente.

Puede usar cualquier combinación de ranuras de destinos de representación (hasta 8). Sin embargo, una vista de recursos no se puede enlazar a varias ranuras de destino de representación simultáneamente. Una vista se puede volver A usar siempre que los recursos no se usen simultáneamente.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>En esta sección


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tema</th>
<th align="left">Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="configuring-depth-stencil-functionality.md">Configurar la funcionalidad de la galería de símbolos de profundidad</a></p></td>
<td align="left"><p>En esta sección se explican los pasos para configurar el búfer de estarcido de profundidad y el estado de la galería de símbolos de profundidad para la fase de combinación de salida.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 