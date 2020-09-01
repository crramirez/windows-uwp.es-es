---
title: Configurar la funcionalidad de la galería de símbolos de profundidad
description: En esta sección se explican los pasos para configurar el búfer de estarcido de profundidad y el estado de la galería de símbolos de profundidad para la fase de combinación de salida.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Configurar la funcionalidad de la galería de símbolos de profundidad
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2d9c33e9625f36bbf183df46d5cd590f9e66d3c4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162809"
---
# <a name="span-iddirect3dconceptsconfiguring_depth-stencil_functionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Configurar la funcionalidad de la galería de símbolos de profundidad


En esta sección se explican los pasos para configurar el búfer de estarcido de profundidad y el estado de la galería de símbolos de profundidad para la fase de combinación de salida.

Una vez que sepa cómo usar el búfer de estarcido de profundidad y el estado de la galería de símbolos de profundidad correspondiente, consulte [técnicas avanzadas de estarcido](#advanced-stencil-techniques).

## <a name="span-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespanspan-idcreate_depth_stencil_statespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Crear estado de estarcido de profundidad


El estado de la galería de símbolos de profundidad indica a la fase de combinación de salida cómo realizar la [prueba de la galería de símbolos de profundidad](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage). La prueba de profundidad-estarcido determina si se debe dibujar un píxel determinado.

## <a name="span-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanspan-idbind_depth_stencil_to_the_om_stagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Enlazar datos de estarcido de profundidad a la fase de OM


Enlazar el estado de la galería de símbolos de profundidad.

Enlazar el recurso de la galería de símbolos de profundidad mediante una vista.

Todos los destinos de representación deben ser del mismo tipo de recurso. Si se usa el suavizado de contorno Multimuestra, todos los destinos de representación enlazados y los búferes de profundidad deben tener el mismo número de muestras.

Cuando se usa un búfer como destino de representación, no se admiten las pruebas de la galería de símbolos de profundidad y los diversos destinos de representación.

-   Hasta 8 destinos de representación se pueden enlazar simultáneamente.
-   Todos los destinos de representación deben tener el mismo tamaño en todas las dimensiones (ancho y alto, y profundidad para el tamaño 3D o de la matriz para los \* tipos de matriz).
-   Cada destino de representación puede tener un formato de datos diferente.
-   Las máscaras de escritura controlan los datos que se escriben en un destino de representación. El control de máscaras de escritura de salida controla el destino de cada representación y el nivel de cada componente de los datos que se escriben en los destinos de representación.

## <a name="span-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced_stencil_techniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Técnicas avanzadas de estarcido


La parte de la galería de símbolos del búfer de estarcido de profundidad se puede usar para crear efectos de representación como composición, alineación y esquematización.

-   [Composición](#compositing)
-   [Marcas](#decaling)
-   [Contornos y siluetas](#outlines-and-silhouettes)
-   [Galería de símbolos de dos caras](#two-sided-stencil)
-   [Lectura del búfer de estarcido de profundidad como textura](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composición

La aplicación puede usar el búfer de estarcido para componer imágenes 2D o 3D en una escena 3D. Se utiliza una máscara en el búfer de estarcido para tapabar un área de la superficie de destino de representación. La información de 2D almacenada, como texto o mapas de bits, se puede escribir en el área ocluidos. Como alternativa, la aplicación puede representar primitivos 3D adicionales en la región enmascarada de estarcido de la superficie de destino de representación. Incluso puede representar una escena completa.

A menudo, los juegos componen varias escenas 3D juntas. Por ejemplo, los juegos de conducción suelen mostrar un reflejo de la vista trasera. El reflejo contiene la vista de la escena 3D detrás del controlador. En esencia, es una segunda escena 3D compuesta con la vista hacia delante del controlador.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Marcas

Las aplicaciones de Direct3D usan las marcas para controlar qué píxeles de una imagen primitiva determinada se dibujan en la superficie de destino de representación. Las aplicaciones aplican marcas a las imágenes de primitivas para permitir que los polígonos coplanoles se representen correctamente.

Por ejemplo, al aplicar las marcas de neumático y las líneas amarillas a un Roadway, las marcas deben aparecer directamente encima de la carretera. Sin embargo, los valores z de las marcas y la carretera son los mismos. Por lo tanto, es posible que el búfer de profundidad no produzca una separación limpia entre ambos. Algunos píxeles de la primitiva inversa se pueden representar encima de la primitiva delantera y viceversa. La imagen resultante parece Shimmer de fotograma a fotograma. Este efecto se denomina lucha z o flimmering.

Para solucionar este problema, use una galería de símbolos para enmascarar la sección de la primitiva inversa en la que aparecerá la calcomanía. Desactive el almacenamiento en búfer z y represente la imagen de la primitiva delantera en el área enmascarada de la superficie de representación-destino.

Se puede usar la combinación de varias texturas para resolver este problema.

### <a name="span-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines_and_silhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Contornos y siluetas

Puede usar el búfer de estarcido para efectos más abstractos, como la esquematización y la silhouetting.

Si la aplicación realiza dos fases de representación: una para generar la máscara de la galería de símbolos y la segunda para aplicar la máscara de la galería de símbolos a la imagen, pero con los primitivos ligeramente más pequeños en el segundo paso: la imagen resultante contendrá solo el contorno de la primitiva. A continuación, la aplicación puede rellenar el área enmascarada de estarcido de la imagen con un color sólido, lo que proporciona al primitivo una apariencia en relieve.

Si la máscara de la galería de símbolos tiene el mismo tamaño y forma que la primitiva que se está representando, la imagen resultante contiene un hueco en el que el primitivo debe ser. A continuación, la aplicación puede rellenar el hueco con negro para producir una silueta de la primitiva.

### <a name="span-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo_sided_stencilspanspan-idtwo-sided-stenciltwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span><span id="two-sided-stencil">Galería de símbolos de dos caras

Los volúmenes de sombra se usan para dibujar sombras con el búfer de estarcido. La aplicación calcula los volúmenes de sombra convertidos por geometría occluding, mediante el cálculo de los bordes de la silueta y la extrusión de la luz en un conjunto de volúmenes 3D. A continuación, estos volúmenes se representan dos veces en el búfer de estarcido.

La primera representación dibuja polígonos hacia delante e incrementa los valores del búfer de estarcido. La segunda representación dibuja los polígonos hacia delante del volumen de instantáneas y disminuye los valores de búfer de la galería de símbolos.

Normalmente, todos los valores incrementados y disminuidos se cancelan entre sí. Sin embargo, la escena ya se representó con geometría normal, lo que provoca que algunos píxeles no superen la prueba de búfer z mientras se representa el volumen de la instantánea. Los valores que quedan en el búfer de estarcido se corresponden con los píxeles que están en la sombra. El resto del contenido del búfer de la galería de símbolos se usa como máscara, para que la combinación de alfa sea una gran cantidad de color negro que abarque a la escena. Con el búfer de estarcido que actúa como máscara, el resultado es oscurecer los píxeles que se encuentran en las sombras.

Esto significa que la geometría de la sombra se dibuja dos veces por origen de la luz, por lo que se pone presión sobre el rendimiento de los vértices de la GPU. La característica de galería de símbolos de dos caras se ha diseñado para mitigar esta situación. En este enfoque, hay dos conjuntos de estado de la galería de símbolos (denominados a continuación), uno para cada uno de los triángulos orientados delante y otro para los triángulos de cara a la inversa. De este modo, solo se dibuja un único paso por volumen de instantánea, por luz.

### <a name="span-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading_the_depth-stencil_buffer_as_a_texturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Lectura del búfer de estarcido de profundidad como textura

Un sombreador puede leer un búfer de estarcido de profundidad inactivo como textura. Una aplicación que lee un búfer de estarcido de profundidad como una textura se representa en dos fases, la primera pasada escribe en el búfer de la galería de símbolos de profundidad y el segundo paso Lee del búfer. Esto permite a un sombreador comparar los valores de profundidad o de estarcido previamente escritos en el búfer con el valor de la Currrently de píxeles que se está representando. El resultado de la comparación se puede usar para crear efectos como la asignación de sombras o las partículas flexibles en un sistema de partículas.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase de fusión de salida (OM)](output-merger-stage--om-.md)

[Canalización de gráficos](graphics-pipeline.md)

[Fase de combinación de salida](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)