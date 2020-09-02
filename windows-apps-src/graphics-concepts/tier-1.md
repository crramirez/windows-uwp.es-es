---
title: Nivel 1
description: Obtenga información sobre las limitaciones generales y específicas que afectan a la compatibilidad de nivel 1 con las características de recursos de streaming en Direct3D.
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords:
- Nivel 1
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2ff90c75e3f4aefa28ddad5528d39d2f2d541bb0
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304647"
---
# <a name="tier-1"></a>Nivel 1


En esta sección se describe la compatibilidad de nivel 1.

## <a name="span-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspanspan-idtier_1_general_limitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Limitaciones generales de nivel 1


-   Hardware en el nivel de características 11,0 mínimo.
-   No se admiten tejidos.
-   No se admite Texture1D ni Texture3D.
-   No compatible con el suavizado de contorno de muestreo Multimuestra (MSAA) de 2, 8 o 16. Solo se requiere 4x, excepto los formatos 128 BPP.
-   Ningún patrón swizzle estándar (el diseño dentro de los mosaicos de 64 KB y el empaquetado de MIP de cola es el proveedor de hardware).
-   Limitaciones en el modo en que se puede obtener acceso a los mosaicos cuando hay asignaciones duplicadas. Consulte [limitaciones de acceso a iconos con asignaciones duplicadas](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspan-idspecific_limitations_affecting_tier_1_onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Limitaciones específicas que solo afectan al nivel 1


### <a name="span-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanspan-idreading_writing_to_streaming_resources_that_have_null_mappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Lectura/escritura de recursos de streaming que tienen asignaciones NULL

Los recursos de streaming pueden tener asignaciones **null** , pero leerlos o escribir en ellos produce resultados indefinidos, incluido el dispositivo quitado. Las aplicaciones pueden evitar esto asignando una sola página ficticia a todas las áreas vacías. Tenga cuidado si escribe y representa en una página que está asignada a varias ubicaciones de destino de representación, ya que el orden de las escrituras será undefined.

### <a name="span-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanspan-idno_shader_instructions_for_clamping_lod_and_mapped_status_feedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>No hay instrucciones de sombreador para la compresión de LOD y comentarios de estado asignados

Las instrucciones del sombreador para la compresión de LOD y los comentarios de estado asignados no están disponibles. Consulte [exposición de recursos de streaming de HLSL](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanspan-idalignment_constraints_for_standard_tile_shapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Restricciones de alineación para las formas de mosaico estándar

Solo se garantiza que MIPS (a partir del más fino) cuyas dimensiones son todos los múltiplos del tamaño de mosaico estándar admiten las formas de mosaico estándar y pueden tener mosaicos individuales asignados o desasignados de forma arbitraria. El primer mipmap de un recurso de streaming que tiene cualquier dimensión que no sea un múltiplo del tamaño de mosaico estándar, junto con todos los mapas de bits más generales, puede tener una forma de mosaico no estándar, ajustarse a N mosaicos de 64 KB para este conjunto de MIPS a la vez (N informada a la aplicación). Estos N mosaicos se consideran empaquetados como una unidad, que debe ser totalmente asignada o totalmente Desasignada por la aplicación en un momento dado, aunque las asignaciones de cada uno de los iconos N pueden estar en ubicaciones geojuntas arbitrariamente en un grupo de mosaicos.

### <a name="span-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanspan-idarray_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_sizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Matriz de mapas MIP que no son múltiplos del tamaño de mosaico estándar

Los recursos de streaming con cualquier MIP que no sea un múltiplo del tamaño de mosaico estándar en todas las dimensiones no pueden tener un tamaño de matriz mayor que 1.

### <a name="span-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanspan-idswitching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Cambiar entre los mosaicos de referencia de un grupo de mosaicos a través de un recurso de textura y búfer

Para cambiar entre los mosaicos de referencia de un grupo de mosaicos a través de un recurso de [búfer](introduction-to-buffers.md) para hacer referencia a los mismos mosaicos a través de un recurso de [textura](introduction-to-textures.md) , o viceversa, la actualización más reciente de las asignaciones de iconos o la copia de las asignaciones de mosaicos que define las asignaciones a esos mosaicos de grupos de mosaicos debe ser para la misma dimensión de recursos (búfer frente a textura \* ) que la dimensión de recursos que se usará para tener acceso a los mosaicos. De lo contrario, el comportamiento es indefinido, lo que incluye la posibilidad de restablecer el dispositivo.

Por lo tanto, por ejemplo, no es válido actualizar las asignaciones de iconos para definir asignaciones de mosaicos para un búfer y, a continuación, actualizar las asignaciones de iconos a los mismos mosaicos en el grupo de mosaicos a través de un recurso [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) y, a continuación, tener acceso a los iconos a través del búfer. Las operaciones de solución alternativa consisten en volver a definir asignaciones de mosaicos para un recurso al cambiar entre el búfer y la textura (o viceversa) para compartir iconos o simplemente no compartir iconos en un grupo de mosaicos entre recursos de búfer y recursos de textura.

### <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mín./máx.

No se admite el filtrado de reducción mín./máx. Consulte [características de muestreo de textura de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Niveles de características de recursos de streaming](streaming-resources-features-tiers.md)

 

 