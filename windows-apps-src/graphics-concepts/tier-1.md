---
title: Nivel 1
description: "Esta sección describe la compatibilidad del nivel 1."
ms.assetid: 153DA429-0C99-4691-AEB4-124FD9B17BE2
keywords: Nivel 1
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 0c463e2a6ea4f43ea1f3eccab69a94f9d6f13170
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="tier-1"></a>Nivel 1


Esta sección describe la compatibilidad del nivel 1.

## <a name="span-idtier1generallimitationsspanspan-idtier1generallimitationsspanspan-idtier1generallimitationsspantier-1-general-limitations"></a><span id="Tier_1_general_limitations"></span><span id="tier_1_general_limitations"></span><span id="TIER_1_GENERAL_LIMITATIONS"></span>Limitaciones generales del nivel 1


-   Hardware en el nivel de características 11.0 como mínimo.
-   No se admite el acolchado.
-   No se admite Texture1D ni Texture3D.
-   No se admite el suavizado de contorno de muestras múltiples (MSAA) 2, 8 ni 16. Solo es obligatorio 4x, excepto en formatos que no sean de 128bpp.
-   Ningún patrón de referencia estándar (el diseño en mosaicos de 64KB y el empaquetado MIP de cola depende del proveedor del hardware).
-   Limitaciones al modo de acceso a los mosaicos cuando hay asignaciones duplicadas. Consulta [Limitaciones de acceso a los iconos con asignaciones duplicadas](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspan-idspecificlimitationsaffectingtier1onlyspanspecific-limitations-affecting-tier-1-only"></a><span id="Specific_limitations_affecting_tier_1_only"></span><span id="specific_limitations_affecting_tier_1_only"></span><span id="SPECIFIC_LIMITATIONS_AFFECTING_TIER_1_ONLY"></span>Limitaciones específicas que solo afectan al nivel 1


### <a name="span-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanspan-idreadingwritingtostreamingresourcesthathavenullmappingsspanreadingwriting-to-streaming-resources-that-have-null-mappings"></a><span id="Reading_writing_to_streaming_resources_that_have_NULL_mappings"></span><span id="reading_writing_to_streaming_resources_that_have_null_mappings"></span><span id="READING_WRITING_TO_STREAMING_RESOURCES_THAT_HAVE_NULL_MAPPINGS"></span>Lectura/escritura en recursos de streaming que tengan asignaciones NULL

Los recursos de streaming puede tener asignaciones **NULL**, pero si se lee o escribe en ellos produce resultados definidos, incluyendo la eliminación del dispositivo. Para solucionar esto, las aplicaciones pueden asignar una sola página ficticia a todas las áreas vacías. Ten cuidado si escribes y representas en una página asignada a varias ubicaciones de destino de representación, porque el orden de las escrituras será indefinido.

### <a name="span-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanspan-idnoshaderinstructionsforclampinglodandmappedstatusfeedbackspanno-shader-instructions-for-clamping-lod-and-mapped-status-feedback"></a><span id="No_shader_instructions_for_clamping_LOD_and_mapped_status_feedback"></span><span id="no_shader_instructions_for_clamping_lod_and_mapped_status_feedback"></span><span id="NO_SHADER_INSTRUCTIONS_FOR_CLAMPING_LOD_AND_MAPPED_STATUS_FEEDBACK"></span>No hay instrucciones de sombreador para la compresión del LOD y los comentarios sobre el estado asignado

No están disponibles instrucciones de sombreador para la compresión del LOD y los comentarios sobre el estado asignado. Consulta [Exposición de recursos de streaming de HLSL](hlsl-streaming-resources-exposure.md).

### <a name="span-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanspan-idalignmentconstraintsforstandardtileshapesspanalignment-constraints-for-standard-tile-shapes"></a><span id="Alignment_constraints_for_standard_tile_shapes"></span><span id="alignment_constraints_for_standard_tile_shapes"></span><span id="ALIGNMENT_CONSTRAINTS_FOR_STANDARD_TILE_SHAPES"></span>Restricciones de alineación para las formas de mosaico estándar

Solo se garantiza que los MIP (a partir de los más finos) cuyas dimensiones sean todas múltiplos del tamaño de mosaico estándar admitan las formas de mosaico estándar y se les puedan asignar o desasignar mosaicos individuales de forma arbitraria. El primer mapa MIP de un recurso de streaming que tenga cualquier dimensión que no sea un múltiplo del tamaño de mosaico estándar, junto con todos los mapas MIP más gruesos, puede tener una forma de mosaico no estándar que encaje en mosaico de N 64KB para este conjunto de MIP a la vez (N se notifica a la aplicación). Estos N mosaicos se consideran empaquetados como una unidad, que la aplicación debe asignar o desasignar completamente en cualquier momento, aunque las asignaciones de cada uno de los N mosaicos pueden estar en ubicaciones arbitrariamente desligadas de un grupo de mosaicos.

### <a name="span-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanspan-idarrayofmipmapsthatarentamultipleofstandardtilesizespanarray-of-mipmaps-that-arent-a-multiple-of-standard-tile-size"></a><span id="Array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="array_of_mipmaps_that_aren_t_a_multiple_of_standard_tile_size"></span><span id="ARRAY_OF_MIPMAPS_THAT_AREN_T_A_MULTIPLE_OF_STANDARD_TILE_SIZE"></span>Matriz de mapas MIP que no es un múltiplo del tamaño de mosaico estándar

Los recursos de streaming con mapas MIP que no sean un múltiplo del tamaño de mosaico estándar en todas sus dimensiones no pueden tener un tamaño de matriz mayor de 1.

### <a name="span-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanspan-idswitchingbetweenreferencingtilesinatilepoolviaabufferandtextureresourcespanswitching-between-referencing-tiles-in-a-tile-pool-via-a-buffer-and-texture-resource"></a><span id="Switching_between_referencing_tiles_in_a_tile_pool_via_a_Buffer_and_Texture_resource"></span><span id="switching_between_referencing_tiles_in_a_tile_pool_via_a_buffer_and_texture_resource"></span><span id="SWITCHING_BETWEEN_REFERENCING_TILES_IN_A_TILE_POOL_VIA_A_BUFFER_AND_TEXTURE_RESOURCE"></span>Conmutación entre hacer referencia a los mosaicos de un grupo de mosaicos mediante un recurso de textura y mediante un recurso de búfer

Para conmutar entre hacer referencia a los mosaicos de un grupo de mosaicos a través de un recurso de [búfer](introduction-to-buffers.md) a hacer referencia a los mismos mosaicos a través de un recurso de [textura](introduction-to-textures.md), o viceversa, la actualización más reciente de las asignaciones de mosaicos o la copia de las asignaciones de mosaicos que definen las asignaciones para dichos mosaicos del grupo de mosaicos deben ser para la misma dimensión de recurso (búfer frente a textura\ *) que la dimensión de recurso que se usará para acceder a los mosaicos. De lo contrario, el comportamiento es indefinido, incluida la posibilidad de un restablecimiento del dispositivo.

Por lo tanto, por ejemplo, no es válido actualizar las asignaciones de mosaicos para definir las asignaciones de mosaicos de un búfer, luego actualizar las asignaciones de mosaicos a los mismos mosaicos en el grupo de mosaicos a través de un recurso [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y luego acceder a los mosaicos a través del búfer. Las soluciones son redefinir las asignaciones de mosaicos de un recurso al conmutar entre la textura y el búfer (o viceversa) compartiendo los mosaicos, o simplemente no compartir nunca los mosaicos de un grupo de mosaicos entre los recursos de búfer y los recursos de textura.

### <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mínimo/máximo

No se admite el filtrado de reducción mínimo/máximo. Consulta [Características de muestreo de texturas de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Niveles de características de los recursos de streaming](streaming-resources-features-tiers.md)

 

 



