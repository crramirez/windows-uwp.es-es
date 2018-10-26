---
title: Configurar la funcionalidad de la galería de símbolos de profundidad
description: En esta sección se describen los pasos para configurar el búfer de la galería de símbolos de profundidad y el estado de la galería de símbolos de profundidad para la fase de fusión de salida.
ms.assetid: B3F6CDAA-93ED-4DC1-8E69-972C557C7920
keywords:
- Configurar la funcionalidad de la galería de símbolos de profundidad
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c15ff6aa43540b61b410525e6bb20a0de3da821
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "5570795"
---
# <a name="span-iddirect3dconceptsconfiguringdepth-stencilfunctionalityspanconfiguring-depth-stencil-functionality"></a><span id="direct3dconcepts.configuring_depth-stencil_functionality"></span>Configurar la funcionalidad de la galería de símbolos de profundidad


En esta sección se describen los pasos para configurar el búfer de la galería de símbolos de profundidad y el estado de la galería de símbolos de profundidad para la fase de fusión de salida.

Una vez que sepas cómo usar el búfer de la galería de símbolos de profundidad y el estado correspondiente de la galería de símbolos de profundidad, consulta las [técnicas avanzadas de la galería de símbolos](#advanced-stencil-techniques).

## <a name="span-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespanspan-idcreatedepthstencilstatespancreate-depth-stencil-state"></a><span id="Create_Depth_Stencil_State"></span><span id="create_depth_stencil_state"></span><span id="CREATE_DEPTH_STENCIL_STATE"></span>Crear el estado de la galería de símbolos de profundidad


El estado de la galería de símbolos de profundidad indica a la fase de fusión de salida cómo llevar a cabo la [prueba de la galería de símbolos de profundidad](https://msdn.microsoft.com/library/windows/desktop/bb205120). La prueba de la galería de símbolos de profundidad determina si se debe dibujar un píxel determinado o no.

## <a name="span-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanspan-idbinddepthstenciltotheomstagespanbind-depth-stencil-data-to-the-om-stage"></a><span id="Bind_Depth_Stencil_to_the_OM_Stage"></span><span id="bind_depth_stencil_to_the_om_stage"></span><span id="BIND_DEPTH_STENCIL_TO_THE_OM_STAGE"></span>Enlazar los datos de la galería de símbolos de profundidad a la fase de fusión de salida


Enlaza el estado de la galería de símbolos de profundidad.

Enlaza el recurso de la galería de símbolos de profundidad mediante una vista.

Los destinos de representación deben ser todos del mismo tipo de recurso. Si se usa el suavizado de contorno multimuestra, todos los destinos de representación y búferes de profundidad enlazados deben tener los mismos recuentos de muestras.

Cuando se usa un búfer como destino de representación, no se admiten las pruebas de la galería de símbolos de profundidad y varios destinos de representación .

-   Hasta 8 destinos de representación se pueden enlazar al mismo tiempo.
-   Todos los destinos de representación deben tener el mismo tamaño en todas las dimensiones (ancho, alto y profundidad 3D o el tamaño de la matriz para tipos \*Array).
-   Cada destino de representación puede tener un formato de datos diferente.
-   Las máscaras de escritura controlan qué datos se escriben en un destino de representación. Las máscaras de escritura de salida controlan, para cada destino de representación y para cada nivel de componente, qué datos se escriben en los destinos de representación.

## <a name="span-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvancedstenciltechniquesspanspan-idadvanced-stencil-techniquesspanadvanced-stencil-techniques"></a><span id="Advanced_Stencil_Techniques"></span><span id="advanced_stencil_techniques"></span><span id="ADVANCED_STENCIL_TECHNIQUES"></span><span id="advanced-stencil-techniques"></span>Técnicas avanzadas de la galería de símbolos


La parte de la galería de símbolos del búfer de la galería de símbolos de profundidad puede usarse para crear efectos de representación, como composición, calco y contornos.

-   [Composición](#compositing)
-   [Calco](#decaling)
-   [Contornos y siluetas](#outlines-and-silhouettes)
-   [Galería de símbolos a doble cara](#two-sided-stencil)
-   [Leer el búfer de la galería de símbolos de profundidad como textura](#reading-the-depth-stencil-buffer-as-a-texture)

### <a name="span-idcompositingspanspan-idcompositingspanspan-idcompositingspancompositing"></a><span id="Compositing"></span><span id="compositing"></span><span id="COMPOSITING"></span>Composición

La aplicación puede usar el búfer de la galería de símbolos para componer imágenes 2D o 3D en una escena 3D. Una máscara en el búfer de la galería de símbolos se usa para tapar un área de la superficie de destino de representación. La información 2D almacenada, como texto o mapas de bits, luego puede escribirse en el área que queda oculta. Como alternativa, la aplicación puede representar primitivos 3D adicionales en la región enmascarada por la galería de símbolos de la superficie de destino de representación. Incluso puede representar una escena completa.

A menudo, los juegos componen varias escenas 3D juntas. Por ejemplo, los juegos de coches suelen mostrar un espejo retrovisor. El espejo contiene la vista de la escena 3D detrás del conductor. En esencia, es una segunda escena 3D compuesta con la vista hacia delante del conductor.

### <a name="span-iddecalingspanspan-iddecalingspanspan-iddecalingspandecaling"></a><span id="Decaling"></span><span id="decaling"></span><span id="DECALING"></span>Calco

Las aplicaciones Direct3D usan el calco para controlar qué píxeles de una imagen primitiva determinada se dibujan en la superficie de destino de representación. Las aplicaciones aplican los calcos a las imágenes de primitivos para permitir que los polígonos coplanares se representen correctamente.

Por ejemplo, al aplicar las marcas de rueda y líneas amarillas a una carretera, las marcas deben aparecer directamente en la parte superior de la carretera. Sin embargo, los valores z de las marcas y de la carretera son los mismos. Por lo tanto, el búfer de profundidad podría no producir una separación clara entre las dos. Algunos píxeles del primitivo trasero pueden representarse sobre el primitivo frontal y viceversa. La imagen resultante parece relucir de un fotograma a otro. Este efecto se denomina rivalidad z o destello.

Para resolver este problema, usa una galería de símbolos para enmascarar la sección del primitivo trasero donde va a aparecer el calco. Desactiva el búfer z y representa la imagen del primitivo frontal en el área con máscara de la superficie de destino de representación.

La combinación de varias texturas puede usarse para solucionar este problema.

### <a name="span-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlinesandsilhouettesspanspan-idoutlines-and-silhouettesoutlines-and-silhouettes"></a><span id="Outlines_and_Silhouettes"></span><span id="outlines_and_silhouettes"></span><span id="OUTLINES_AND_SILHOUETTES"></span><span id="outlines-and-silhouettes">Contornos y siluetas

Puedes usar el búfer de la galería de símbolos para efectos más abstractos, como para crear contornos y siluetas.

Si tu aplicación realiza dos pasadas de representación —una para generar la máscara de la galería de símbolos y la segundo para aplicar la máscara de la galería de símbolos a la imagen, pero con primitivos ligeramente más pequeños en la segunda pasada—, la imagen resultante contendrá solo el contorno del primitivo. La aplicación luego puede llenar el área con máscara de la galería de símbolos de la imagen con un color sólido, lo que da al primitivo una apariencia en relieve.

Si la máscara de la galería de símbolos tiene el mismo tamaño y forma que el primitivo que estás representando, la imagen resultante contiene un hueco donde debería estar el primitivo. La aplicación luego puede rellenar el agujero con negro para producir una silueta del primitivo.

### <a name="span-idtwosidedstencilspanspan-idtwosidedstencilspanspan-idtwosidedstencilspantwo-sided-stencil"></a><span id="Two_Sided_Stencil"></span><span id="two_sided_stencil"></span><span id="TWO_SIDED_STENCIL"></span>Galería de símbolos a doble cara

Los volúmenes de sombra se usan para dibujar sombras con el búfer de la galería de símbolos. La aplicación calcula los volúmenes de sombra que genera la geometría que tapa la luz; para ello, calcula los bordes de las siluetas y los extruye desde la luz hasta un conjunto de volúmenes 3D. Luego estos volúmenes se representan dos veces en el búfer de la galería de símbolos.

La primera representación dibuja polígonos orientados hacia el frente y aumenta los valores del búfer de la galería de símbolos. La segunda representación dibuja los polígonos orientados hacia atrás del volumen de sombra y reduce los valores del búfer de la galería de símbolos.

Normalmente, todos los valores incrementados y reduce aumentados. Sin embargo, la escena ya estaba representada con la geometría normal, por lo que algunos píxeles no supera la prueba de búfer z a medida que se representa el volumen de sombra. Los valores que quedan en el búfer de la galería de símbolos corresponden a los píxeles que se encuentran en la sombra. Estos contenidos del búfer de la galería de símbolos que quedan se usan como máscara, para la combinación alfa de un cuadrado negro grande, que abarca todo en la escena. Con el búfer de la galería de símbolos como máscara, el resultado es oscurecer los píxeles que se encuentran en las sombras.

Esto significa que la geometría de las sombras se dibuja dos veces por fuente de luz, lo que ejerce presión en el rendimiento de vértices de la GPU. La característica de galería de símbolos a doble cara se ha diseñado para mitigar esta situación. En este enfoque, existen dos conjuntos de estado de la galería de símbolos (nombrados a continuación), un conjunto cada uno para los triángulos orientados al frente y otro para los triángulos orientados hacia atrás. De esta forma, solo se dibuja una única pasada por volumen de sombra, por luz.

### <a name="span-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreadingthedepth-stencilbufferasatexturespanspan-idreading-the-depth-stencil-buffer-as-a-texturespanreading-the-depth-stencil-buffer-as-a-texture"></a><span id="Reading_the_Depth-Stencil_Buffer_as_a_Texture"></span><span id="reading_the_depth-stencil_buffer_as_a_texture"></span><span id="READING_THE_DEPTH-STENCIL_BUFFER_AS_A_TEXTURE"></span><span id="reading-the-depth-stencil-buffer-as-a-texture"></span>Leer el búfer de la galería de símbolos de profundidad como textura

Un sombreador puede leer un búfer inactivo de la galería de símbolos de profundidad como textura. Una aplicación que lee un búfer de la galería de símbolos de profundidad como textura se representa en dos pasadas, la primera pasada escribe en el búfer de la galería de símbolos de profundidad y la segunda pasada lee del búfer. Esto permite que un sombreador compare los valores de profundidad o de la galería de símbolos escritos anteriormente en el búfer con el valor para el píxel que se está representando actualmente. El resultado de la comparación puede usarse para crear efectos, como el mapa de sombras o partículas suaves en un sistema de partículas.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Fase de fusión de salida (OM)](output-merger-stage--om-.md)

[Canalización de gráficos](graphics-pipeline.md)

[Fase de fusión de salida](https://msdn.microsoft.com/library/windows/desktop/bb205120)
