---
title: La necesidad de recursos de streaming
description: Los recursos de streaming son necesarios para que la memoria de la GPU no se desperdicie en el almacenamiento de regiones de superficies a las que no se tiene acceso y para indicar al hardware cómo filtrar por los mosaicos adyacentes.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- La necesidad de recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b2e95427be7cdae450e60317d13765bb9372e84b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173029"
---
# <a name="the-need-for-streaming-resources"></a>La necesidad de recursos de streaming


Los recursos de streaming son necesarios para que la memoria de la GPU no se desperdicie en el almacenamiento de regiones de superficies a las que no se tiene acceso y para indicar al hardware cómo filtrar por los mosaicos adyacentes.

## <a name="span-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanspan-idstreaming_resources_or_sparse_texturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Recursos de streaming o texturas dispersas


*Los recursos de streaming* (denominados *recursos en mosaico* en Direct3D 11) son recursos lógicos de gran tamaño que utilizan pequeñas cantidades de memoria física.

Otro nombre para los recursos de streaming son las *texturas dispersas*. "Disperso" transmite tanto la naturaleza en mosaico de los recursos como la principal razón para su disposición en mosaico, de modo que no se espera que se asignen todos ellos a la vez. De hecho, una aplicación puede crear un recurso de streaming en el que no se crea ningún dato para todas las regiones y MIPS del recurso, de manera intencionada. Por lo tanto, el propio contenido podría ser disperso y la asignación del contenido en la memoria de la unidad de procesamiento de gráficos (GPU) en un momento dado sería un subconjunto de eso (incluso más disperso).

## <a name="span-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanspan-idwithout_tiling__memory_allocations_are_managed_at_subresource_granularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Sin mosaico, las asignaciones de memoria se administran en la granularidad de los Subrecursos


En un sistema de gráficos (es decir, el sistema operativo, el controlador de pantalla y el hardware de gráficos) sin compatibilidad con recursos de streaming, el sistema de gráficos administra todas las asignaciones de memoria de Direct3D en la granularidad de los recursos.

En el caso de un [búfer](introduction-to-buffers.md), todo el búfer es el subrecurso.

En el caso de una [textura](textures.md)(por ejemplo, [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d)), cada nivel de MIP es un subrecurso; en el caso de una matriz de texturas (por ejemplo, [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray)), cada nivel de MIP en un segmento de matriz determinado es un subrecurso. El sistema de gráficos solo expone la capacidad de administrar la asignación de asignaciones en esta granularidad de Subrecursos. En el contexto de los recursos de streaming, la "asignación" hace referencia a hacer visibles los datos en la GPU.

## <a name="span-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanspan-idwithout_tiling__can_t_access_only_a_small_portion_of_mipmap_chainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Sin mosaico, no puede tener acceso solo a una pequeña parte de la cadena de mipmap


Supongamos que una aplicación sabe que una operación de representación determinada solo necesita tener acceso a una pequeña parte de una cadena de mipmap de imagen (quizás no incluso al área completa de un mipmap determinado). Idealmente, la aplicación podría informar al sistema de gráficos sobre esta necesidad. El sistema de gráficos solo se molestaría para asegurarse de que la memoria necesaria está asignada en la GPU sin paginación en demasiada memoria.

En realidad, sin compatibilidad con recursos de streaming, solo se puede informar al sistema de gráficos sobre la memoria que debe asignarse en la GPU en la granularidad del subrecurso (por ejemplo, un intervalo de niveles de mipmap completo a los que se puede tener acceso). No hay ningún error de demanda en el sistema de gráficos, por lo que es posible que se use una gran cantidad de memoria de GPU para hacer que se asignen Subrecursos completos antes de que se ejecute un comando de representación que haga referencia a cualquier parte de la memoria. Esto es solo un problema que dificulta el uso de grandes asignaciones de memoria en Direct3D sin compatibilidad con recursos de streaming.

## <a name="span-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspanspan-idsoftware_paging_to_break_the_surface_into_smaller_tilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Paginación de software para dividir la superficie en mosaicos más pequeños


La paginación de software se puede usar para dividir la superficie en mosaicos que son lo suficientemente pequeños como para controlar el hardware.

Direct3D admite superficies [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) con hasta 16384 píxeles en un lado determinado. Una imagen de 16384 de ancho a 16384 de alto y 4 bytes por píxel consumirá 1 GB de memoria de vídeo (y la adición de mapas de bits duplicaría esa cantidad). En la práctica, no es necesario hacer referencia a todos los 1 GB en una sola operación de representación.

Algunos desarrolladores de juegos muestran superficies del terreno tan grandes como 128 KB. La forma en que lo hacen para trabajar en GPU existentes es dividir la superficie en mosaicos que son lo suficientemente pequeños como para controlar el hardware. La aplicación debe averiguar qué mosaicos podrían ser necesarios y cargarlos en una caché de texturas en el sistema de paginación de software GPU-a.

Un inconveniente importante para ese enfoque proviene del hardware de no saber nada sobre la paginación que se está llevando a cabo: cuando es necesario mostrar una parte de una imagen en la pantalla que ocupa los mosaicos, el hardware no sabe cómo realizar un filtrado de función fijo (es decir, eficaz) en los mosaicos. Esto significa que la aplicación que administra su propia segmentación de software debe recurrir al filtrado manual de texturas en el código del sombreador (lo que resulta muy caro si se desea un filtro anisotrópico de buena calidad) o desperdiciar los márgenes de la creación de memoria alrededor de los mosaicos que contienen datos de mosaicos contiguos, de modo que el filtrado de hardware de función fija pueda seguir proporcionando

## <a name="span-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanspan-idmaking_tiled_representation_of_surface_allocations_a_first-class_featurespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Crear una representación en mosaico de las asignaciones de superficie una característica de primera clase


Si una representación en mosaico de las asignaciones de superficie es una característica de primera clase en el sistema de gráficos, la aplicación podría indicar al hardware qué mosaicos debe poner a disposición. De esta manera, se desperdicia menos memoria de GPU al almacenar regiones de superficies que la aplicación sabe que no se tendrá acceso a ella, y el hardware puede entender cómo filtrar por los mosaicos adyacentes, lo que evitará que se detecten algunos de los problemas de los desarrolladores que realizan el mosaico de software por su cuenta.

Pero para proporcionar una solución completa, se debe hacer algo para tratar con el hecho de que, independientemente de si se admite la disposición en mosaico dentro de una superficie, la dimensión de superficie máxima es actualmente de 16384-no está cerca de las 128K + que las aplicaciones ya quieren. Simplemente requerir el hardware para admitir tamaños de textura mayores es un enfoque, sin embargo, hay costos y/o contrapartidas significativos para pasar a esta ruta.

La ruta de acceso del filtro de textura de Direct3D's y la ruta de representación ya están saturadas en términos de precisión en la compatibilidad con texturas de 16 k con los demás requisitos, como la compatibilidad con extensiones de ventanilla que caen fuera de la superficie durante la representación o la compatibilidad con el ajuste de textura en el borde de la superficie durante el filtrado. Una posibilidad es definir un equilibrio de forma que, a medida que el tamaño de la textura aumente más allá de 16 k, la funcionalidad/precisión se proporcione de alguna manera. Sin embargo, incluso con esta concesión, es posible que se requieran costos de hardware adicionales en cuanto a la capacidad de direccionamiento en todo el sistema de hardware para ir a tamaños de textura más grandes.

## <a name="span-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanspan-idissue_with_large_textures__precision_for_locations_on_surfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problema con texturas grandes: precisión para ubicaciones en superficie


Un problema que se reproduce como texturas es muy grande es que las coordenadas de textura de punto flotante de precisión sencilla (y los interpoladores asociados para admitir la rasterización) se agotan sin precisión para especificar las ubicaciones en la superficie de forma precisa. El filtrado de textura de vibración surgiría. Una opción costosa sería requerir compatibilidad con el interpolador de precisión doble, aunque esto podría ser exageración dada una alternativa razonable.

## <a name="span-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanspan-idenabling_multiple_resources_of_different_dimensions_to_share_memoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Habilitar varios recursos de dimensiones diferentes para compartir memoria


Otro escenario que podrían atender los recursos de streaming es habilitar varios recursos de diferentes dimensiones y formatos para compartir la misma memoria. A veces, las aplicaciones tienen conjuntos exclusivos de recursos que se sabe que no se usan al mismo tiempo, o recursos que se crean solo para un uso muy breve y, a continuación, se destruyen, seguido de la creación de otros recursos. Una forma de generalización que puede salir de "recursos de streaming" es que es posible permitir que el usuario señale a varios recursos distintos en la misma memoria (superpuestas). En otras palabras, la creación y destrucción de "recursos" (que definen una dimensión o formato, etc.) se pueden desacoplar de la administración de la memoria subyacente a los recursos desde el punto de vista de la aplicación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 