---
title: Objetos de estado
description: El estado del dispositivo se agrupa en objetos de estado, que reducen en gran medida el coste de los cambios de estado. Existen varios objetos de estado, y cada uno de ellos está diseñado para inicializar un conjunto de estados para una fase concreta de la canalización . Los objetos de estado varían según la versión de Direct3D.
ms.assetid: D998745C-2B75-4E59-9923-AD1A17A92E05
keywords:
- Objetos de estado
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3b0333d77e635961d51d3d5bb45cdf2def646fb8
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7428365"
---
# <a name="state-objects"></a>Objetos de estado


El estado del dispositivo se agrupa en objetos de estado, que reducen en gran medida el coste de los cambios de estado. Existen varios objetos de estado, y cada uno de ellos está diseñado para inicializar un conjunto de estados para una fase concreta de la canalización . Los objetos de estado varían según la versión de Direct3D.

## <a name="span-idinputlayoutspanspan-idinputlayoutspanspan-idinputlayoutspaninput-layout-state"></a><span id="Input_Layout"></span><span id="input_layout"></span><span id="INPUT_LAYOUT"></span>Estado del diseño de entrada


Este grupo de estado determina la forma en la que la [fase del ensamblador de entrada (IA)](input-assembler-stage--ia-.md) lee datos de los búferes de entrada y los ensambla para que los use el sombreador de vértices. Esto incluye el estado, como el número de elementos en el búfer de entrada y la firma de los datos de entrada. La fase del ensamblador de entrada (IA) transmite primitivos de la memoria a la canalización.

## <a name="span-idrasterizerspanspan-idrasterizerspanspan-idrasterizerspanrasterizer-state"></a><span id="Rasterizer"></span><span id="rasterizer"></span><span id="RASTERIZER"></span>Estado del rasterizador


Este grupo de estado inicializa la [fase del rasterizador (RS)](rasterizer-stage--rs-.md). Este objeto incluye el estado, como los modos de relleno o de eliminación, la habilitación de un rectángulo de tijera para el recorte y la configuración de los parámetros de muestreo múltiple. Esta fase rasteriza primitivos en píxeles, realizando operaciones como el recorte y la asignación de primitivos a la ventanilla.

## <a name="span-iddepthstencilspanspan-iddepthstencilspanspan-iddepthstencilspandepth-stencil-state"></a><span id="DepthStencil"></span><span id="depthstencil"></span><span id="DEPTHSTENCIL"></span>Estado de la profundidad y la galería de símbolos


Este grupo de estado inicializa la parte de la profundidad y la galería de símbolos de la [fase de fusión de salida (OM)](output-merger-stage--om-.md). Más concretamente, este objeto inicializa las pruebas de profundidad y de la galería de símbolos.

## <a name="span-idblendspanspan-idblendspanspan-idblendspanblend-state"></a><span id="Blend"></span><span id="blend"></span><span id="BLEND"></span>Estado de fusión


Este grupo de estado inicializa la parte de la fusión de la [fase de fusión de salida (OM)](output-merger-stage--om-.md).

## <a name="span-idsamplerspanspan-idsamplerspanspan-idsamplerspansampler-state"></a><span id="Sampler"></span><span id="sampler"></span><span id="SAMPLER"></span>Estado de muestra


Este grupo de estado Inicializa un objeto de muestra. Un objeto de muestra lo usan las fases del sombreador para filtrar las texturas en la memoria.

En Direct3D, el objeto de muestra no está enlazado a una textura específica, simplemente describe cómo hacer el filtrado en función de cualquier recurso adjunto.

## <a name="span-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanspan-idperformanceconsiderationsspanperformance-considerations"></a><span id="Performance_Considerations"></span><span id="performance_considerations"></span><span id="PERFORMANCE_CONSIDERATIONS"></span>Consideraciones de rendimiento


Diseñar la API para usar objetos de estado produce varias ventajas en el rendimiento. Estas incluyen validar el estado en el momento de creación del objeto, lo que permite el almacenamiento en caché de los objetos de estado en el hardware y reduce considerablemente la cantidad de estado que se pasa durante una llamada de la API de configuración de estado (al pasar un identificador al objeto de estado en lugar del estado).

Para lograr estas mejoras del rendimiento, debes crear los objetos de estado cuando la aplicación se inicie, bueno, mucho antes del bucle de representación. Los objetos de estado son inmutables; es decir, una vez que se crean, no se pueden cambiar. En su lugar, debes destruirlos y volver a crearlos.

Podrías crear varios objetos de muestra con diversas combinaciones de estados de muestra. Luego el cambio del estado de muestra se logra mediante una llamada a la API adecuada "Set", que pasa un identificador al objeto (en lugar del estado de muestra). Esto reduce significativamente la cantidad de carga durante cada marco de representación para cambiar el estado, ya que el número de llamadas y la cantidad de datos se disminuyen notablemente.

Como alternativa, puedes usar el sistema de efectos, que administra automáticamente la creación y la destrucción eficaz de objetos de estado de la aplicación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Canalización de gráficos](graphics-pipeline.md)

[Vistas](views.md)

 

 




