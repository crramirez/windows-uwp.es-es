---
title: Fase de fusión de salida (OM)
description: La fase de fusión de salida (OM) combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad o galería de símbolos para generar el resultado final de la canalización.
ms.assetid: ED2DC4A0-2B92-47AF-884A-BFC2183C78B8
keywords:
- Fase de fusión de salida (OM)
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a4e2bbac12ddd3b60b2e4dd78f37b8934a8afea8
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5562606"
---
# <a name="output-merger-om-stage"></a>Fase de fusión de salida (OM)


La fase de fusión de salida (OM) combina varios tipos de datos de salida (valores de sombreador de píxeles, información de profundidad y galería de símbolos) con los contenidos del destino de representación y los búferes de profundidad o galería de símbolos para generar el resultado final de la canalización.

## <a name="span-idpurpose-and-usesspanspan-idpurpose-and-usesspanspan-idpurpose-and-usesspanpurpose-and-uses"></a><span id="Purpose-and-uses"></span><span id="purpose-and-uses"></span><span id="PURPOSE-AND-USES"></span>Finalidad y usos


La fase de fusión de salida (OM) es el último paso para determinar qué píxeles son visibles (con las pruebas de galería de símbolos de profundidad) y combinar los colores de píxeles finales.

La fase OM genera el color de píxel representado final mediante una combinación de los siguientes elementos:

-   Estado de la canalización
-   Datos de píxel generados por los sombreadores de píxeles
-   Contenido de los destinos de representación
-   Contenido de los búferes de profundidad o galería de símbolos

### <a name="span-idblending-overviewspanspan-idblending-overviewspanspan-idblending-overviewspanblending-overview"></a><span id="Blending-overview"></span><span id="blending-overview"></span><span id="BLENDING-OVERVIEW"></span>Introducción a la fusión

La fusión combina uno o más valores de píxel para crear un color de píxel final. El siguiente diagrama muestra el proceso de fusión de datos de píxel.

![diagrama de funcionamiento de la fusión de datos](images/d3d10-blend-state.png)

Conceptualmente, puedes visualizar este diagrama de flujo implementado dos veces en la fase de fusión de salida: el primero combina los datos RGB, mientras que, en paralelo, el segundo combina los datos alfa. Para ver cómo usar la API para crear y establecer el estado de fusión, consulta [Configuring Blending Functionality](https://msdn.microsoft.com/library/windows/desktop/bb205072) (Configurar la funcionalidad de fusión).

La fusión de la función fija se puede habilitar de manera independiente para cada destino de representación. No obstante, existe un único conjunto de controles de fusión, por lo que la misma fusión se aplica a todos los objetos RenderTargets con la fusión habilitada. Los valores de fusión (incluido BlendFactor) siempre se fijan en el rango del formato de destino de representación antes de la fusión. La fijación se realiza por destino de representación y respeta el tipo de destino de representación. La única excepción es para los formatos float16, float11 o float10, que no se fijan, de modo que las operaciones de fusión en estos formatos pueden realizarse, como mínimo, con la misma precisión o el mismo rango que el formato de salida. Los valores NaN y ceros con firma se propagan en todos los casos (incluidos los pesos de fusión de 0,0).

Cuando usas destinos de representación sRGB, el tiempo de ejecución convierte el color de destino de representación en espacio lineal antes de realizar la fusión. El tiempo de ejecución vuelve a convertir el valor fusionado final al espacio sRGB antes de volver a guardarlo en el destino de representación.

### <a name="span-iddual-source-color-blendingspanspan-iddual-source-color-blendingspanspan-iddual-source-color-blendingspandual-source-color-blending"></a><span id="Dual-source-color-blending"></span><span id="dual-source-color-blending"></span><span id="DUAL-SOURCE-COLOR-BLENDING"></span>Fusión de color de origen doble

Esta característica permite a la fase de fusión de salida usar simultáneamente ambas salidas de sombreador de píxeles (o0 y o1) como entradas para una operación de fusión con un único destino de representación en la ranura 0. Algunas operaciones de fusión válidas son: add, subtract y revsubtract. La ecuación de fusión y la máscara de escritura de salida especifican los componentes que genera el sombreador de píxeles. Los componentes adicionales se omiten.

La escritura en otras salidas del sombreador de píxeles (o2, o3, etc.) no está definida; es posible que no puedas escribir en un destino de representación si no está enlazado a la ranura 0. La escritura de oDepth es válida durante la fusión de color de origen doble.

### <a name="span-iddepth-stencil-testspanspan-iddepth-stencil-testspanspan-iddepth-stencil-testspandepth-stencil-testing-overview"></a><span id="Depth-Stencil-Test"></span><span id="depth-stencil-test"></span><span id="DEPTH-STENCIL-TEST"></span>Introducción a las pruebas de galería de símbolos de profundidad

Un búfer de galería de símbolos de profundidad, que se crea como un recurso de textura, puede contener los datos de profundidad y los datos de la galería de símbolos. Los datos de profundidad se utilizan para determinar qué píxeles están más cercanos a la cámara y los datos de la galería de símbolos se emplean para enmascarar los píxeles que se pueden actualizar. En última instancia, tanto los datos de valores de profundidad como de la galería de símbolos se utilizan en la fase de fusión de salida para determinar si se debe dibujar un píxel o no. El siguiente diagrama muestra conceptualmente cómo se realizan las pruebas de galería de símbolos de profundidad.

![Diagrama de funcionamiento de las pruebas de galería de símbolos de profundidad](images/d3d10-depth-stencil-test.png)

Para configurar las pruebas de profundidad y galería de símbolos, consulta [Configurar la funcionalidad de la galería de símbolos de profundidad](configuring-depth-stencil-functionality.md). Un objeto de la galería de símbolos de profundidad encapsula el estado de la galería de símbolos de profundidad. Una aplicación puede especificar el estado de la galería de símbolos de profundidad. De lo contrario, la fase OM utilizará los valores predeterminados. Las operaciones de fusión se realizan por píxel si el muestreo múltiple está deshabilitado. Si el muestreo múltiple está habilitado, la fusión se realiza por muestra múltiple.

El proceso de usar el búfer de profundidad para determinar qué píxel se debe dibujar se denomina "almacenamiento en búfer de profundidad" y, en ocasiones, también se denomina "almacenamiento en búfer Z".

Una vez que los valores de profundidad alcanzan la fase de fusión de salida (ya sea procedentes de la interpolación o de un sombreador de píxeles), siempre se unen: z = min(Viewport.MaxDepth,max(Viewport.MinDepth,z)) según el formato o la precisión del búfer de profundidad, con las reglas de punto flotante. Después de la fijación, se compara el valor de profundidad (con DepthFunc) con el valor existente del búfer de profundidad. Si no hay ningún búfer de profundidad enlazado, siempre se pasa la prueba de profundidad.

Si no hay ningún componente de galería de símbolos en el formato de búfer de profundidad o no existe ningún búfer de profundidad enlazado, siempre se pasa la prueba de la galería de símbolos.

Solo puede haber un búfer de profundidad o galería de símbolos activo a la vez; cualquier vista de recursos enlazada debe coincidir (en tamaño y dimensiones) con la vista de profundidad o galería de símbolos. Esto no significa que el tamaño del recurso deba coincidir, solo se aplica al tamaño de la vista.

### <a name="span-idsample-maskspanspan-idsample-maskspanspan-idsample-maskspansample-mask-overview"></a><span id="Sample-Mask"></span><span id="sample-mask"></span><span id="SAMPLE-MASK"></span>Introducción a la máscara de muestra

Una máscara de muestra es una máscara de cobertura multimuestra de 32 bits que determina qué muestras se actualizan en los destinos de representación activos. Solo se permite una máscara de muestra. La asignación de bits en una máscara de muestra a las muestras de un recurso está definida por un usuario. Para la representación de la muestra n, se usan los primeros n bits (desde el LSB) de la máscara de muestra (32 bits es el número máximo de bits).

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>Entrada


La fase de fusión de salida (OM) genera el color de píxel representado final mediante una combinación de los siguientes elementos:

-   Estado de la canalización
-   Datos de píxel generados por los sombreadores de píxeles
-   Contenido de los destinos de representación
-   Contenido de los búferes de profundidad o galería de símbolos

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>Salida


### <a name="span-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanspan-idoutput-write-mask-overviewspanoutput-write-mask-overview"></a><span id="Output-write-mask-overview"></span><span id="output-write-mask-overview"></span><span id="OUTPUT-WRITE-MASK-OVERVIEW"></span>Introducción a la máscara de escritura de salida

Utiliza una máscara de escritura de salida para controlar (por componente) los datos que se pueden escribir en un destino de representación.

### <a name="span-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanspan-idmultiple-render-targets-overviewspanmultiple-render-targets-overview"></a><span id="Multiple-render-targets-overview"></span><span id="multiple-render-targets-overview"></span><span id="MULTIPLE-RENDER-TARGETS-OVERVIEW"></span>Introducción a los destinos de representación múltiples

Un sombreador de píxeles puede usarse para representarse en 8 destinos de representación diferentes como mínimo, que deben ser todos del mismo tipo (buffer, Texture1D, Texture1DArray, etc.). Además, todos los destinos de representación deben tener el mismo tamaño en todas las dimensiones (ancho, alto, profundidad, tamaño de la matriz, recuentos de muestras). Cada destino de representación puede tener un formato de datos diferente.

Puedes usar cualquier combinación de ranuras de destinos de representación (hasta 8). Sin embargo, una vista de recursos no se puede enlazar a varias ranuras de destinos de representación simultáneamente. Una vista se puede reutilizar siempre que los recursos no se usen simultáneamente.

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
<td align="left"><p>En esta sección se describen los pasos para configurar el búfer de galería de símbolos de profundidad y el estado de la galería de símbolos de profundidad para la fase de fusión de salida.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

 

 




