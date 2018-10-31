---
title: La necesidad de los recursos de streaming
description: Los recursos de streaming son necesarios para que no se malgaste memoria de la GPU al almacenar regiones de superficies a las que no se accederá, así como para indicar al hardware cómo filtrar entre mosaicos adyacentes.
ms.assetid: A88BE65B-104F-4176-9809-C12580A3684C
keywords:
- La necesidad de los recursos de streaming
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 25d5e1d88ec631c3d9105d0291710ca6d0389f13
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "5822639"
---
# <a name="the-need-for-streaming-resources"></a>La necesidad de los recursos de streaming


Los recursos de streaming son necesarios para no que se malgaste memoria de la GPU al almacenar regiones de superficies a las que no se accederá, así como para indicar al hardware cómo filtrar entre mosaicos adyacentes.

## <a name="span-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanspan-idstreamingresourcesorsparsetexturesspanstreaming-resources-or-sparse-textures"></a><span id="Streaming_resources_or_sparse_textures"></span><span id="streaming_resources_or_sparse_textures"></span><span id="STREAMING_RESOURCES_OR_SPARSE_TEXTURES"></span>Recursos de streaming o texturas dispersas


Los *recursos de streaming* (llamados *recursos en mosaico* en Direct3D 11), son recursos lógicos de gran amaño que usan cantidades pequeñas de memoria física.

Otro nombre para los recursos de streaming es *texturas dispersas*. "Dispersas" transmite tanto el hecho de que los recursos están en mosaico como, quizá, el motivo principal para disponerlos en mosaico: que no se espera que se asignen todos a la vez. De hecho, es muy posible que una aplicación pudiera crear un recurso de streaming en el que no se creara ningún dato para todas las regiones + MIP del recurso, y de forma intencionada. Por lo tanto, el propio contenido podría ser disperso y la asignación del contenido en la memoria de la unidad de procesamiento gráfico (GPU) en un momento dado sería un subconjunto de ese (aún más disperso).

## <a name="span-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanspan-idwithouttilingmemoryallocationsaremanagedatsubresourcegranularityspanwithout-tiling-memory-allocations-are-managed-at-subresource-granularity"></a><span id="Without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="without_tiling__memory_allocations_are_managed_at_subresource_granularity"></span><span id="WITHOUT_TILING__MEMORY_ALLOCATIONS_ARE_MANAGED_AT_SUBRESOURCE_GRANULARITY"></span>Sin la disposición en mosaico, las asignaciones de memoria se administran en la granularidad de los subrecursos


En un sistema gráfico (es decir, el sistema operativo, el controlador de pantalla y el hardware gráfico) sin compatibilidad para los recursos de streaming, el sistema gráfico administra todas las asignaciones de memoria de Direct3D en la granularidad de los subrecursos.

En el caso de [búfer](introduction-to-buffers.md), dicho búfer al completo es el subrecurso.

En el caso de una [textura](textures.md), (por ejemplo, [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525)), cada nivel de MIP es un subrecurso; en el de una matriz de texturas, (por ejemplo, [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526)) cada nivel de MIP de un segmento de la matriz determinado es un subrecurso. El sistema gráfico solo expone la capacidad para administrar la asignación de asignaciones en esta granularidad de los subrecursos. En el contexto de los recursos de streaming, "asignación" se refiere a hacer que los datos sean visibles para la GPU.

## <a name="span-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanspan-idwithouttilingcantaccessonlyasmallportionofmipmapchainspanwithout-tiling-cant-access-only-a-small-portion-of-mipmap-chain"></a><span id="Without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="without_tiling__can_t_access_only_a_small_portion_of_mipmap_chain"></span><span id="WITHOUT_TILING__CAN_T_ACCESS_ONLY_A_SMALL_PORTION_OF_MIPMAP_CHAIN"></span>Sin la disposición en mosaico, no puede acceder solo a una pequeña parte de la cadena de mapas MIP


Supongamos que una aplicación sabe que una operación de representación determinada solo necesita tener acceso a una pequeña parte de una cadena de mapas MIP de imagen (quizás ni siquiera al área completa de un determinado mapa MIP). En teoría, la aplicación podría informar al sistema gráfico sobre esta necesidad. Luego el sistema gráfico solo se molestaría en asegurarse de que en la GPU se asignara la memoria necesaria sin paginar demasiada memoria.

En realidad, sin la compatibilidad con los recursos de streaming, el sistema gráfico solo recibiría información acerca de la memoria que se debe asignar en la GPU en la granularidad de los subrecursos (por ejemplo, un intervalo de niveles de mapa MIP completos a los que se podría acceder). Además, el sistema gráfico ya recibe muchas demandas, por lo que potencialmente podría usarse una gran cantidad de memoria de la GPU para asignar los subrecursos completos antes de ejecutar un comando de representación que haga referencia a cualquier parte de la memoria. Este es solo un problema que dificulta el uso de grandes asignaciones de memoria en Direct3D sin la compatibilidad con los recursos de streaming.

## <a name="span-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspanspan-idsoftwarepagingtobreakthesurfaceintosmallertilesspansoftware-paging-to-break-the-surface-into-smaller-tiles"></a><span id="Software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="software_paging_to_break_the_surface_into_smaller_tiles"></span><span id="SOFTWARE_PAGING_TO_BREAK_THE_SURFACE_INTO_SMALLER_TILES"></span>Paginación de software para separar la superficie en mosaicos más pequeños


Puede usarse la paginación de software para dividir la superficie en mosaicos que sean lo suficientemente pequeños para que el hardware los administre.

Direct3D admite superficies de [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) con hasta 16384 píxeles en un lugar determinado. Una imagen de 16384 de ancho por 16384 de alto y con 4 bytes por píxel consumirá 1GB de memoria de vídeo (y si se suman los mapas MIP, podría doblar esa cantidad). En la práctica, rara vez será necesario hacer referencia a la totalidad de los 1GB en una sola operación de representación.

Algunos desarrolladores de juegos modelan superficies de terreno de hasta 128KB por 128KB. La forma en que consiguen que esto funcione en las GPU existente es separar la superficie en mosaicos que sean lo suficientemente pequeños como para que el hardware los pueda administrar. La aplicación debe saber qué mosaicos pueden ser necesarios y cargarlos en una memoria caché de texturas en la GPU: un sistema de paginación de software.

Un inconveniente importante de este método se debe a que el hardware no sabe nada sobre la paginación que se produce: cuando es necesario mostrar en la pantalla una parte de una imagen que incluya mosaicos, el hardware desconoce cómo llevar a cabo un filtrado de función fija (es decir, eficaz) en los distintos mosaicos. Esto significa que la aplicación que administra su propia disposición en mosaicos de software debe recurrir al filtrado manual de las texturas en el código del sombreador (lo que resulta muy costoso si se desea un filtro anisotrópico de buena calidad ) o a malgastar medianiles de creación de memoria alrededor de los mosaicos que contienen datos de los mosaicos vecinos, de forma que el filtrado de hardware de función fija pueda seguir prestando cierta ayuda.

## <a name="span-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanspan-idmakingtiledrepresentationofsurfaceallocationsafirst-classfeaturespanmaking-tiled-representation-of-surface-allocations-a-first-class-feature"></a><span id="Making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="making_tiled_representation_of_surface_allocations_a_first-class_feature"></span><span id="MAKING_TILED_REPRESENTATION_OF_SURFACE_ALLOCATIONS_A_FIRST-CLASS_FEATURE"></span>Hacer que la representación en mosaico de las asignaciones de superficie sean una característica de primera clase


Si una representación en mosaico de las asignaciones de superficie es una característica de primera clase en el sistema gráfico, la aplicación podría indicar al hardware qué mosaicos deben estar disponibles. De esta forma, se malgasta menos memoria de la GPU al almacenar regiones de superficies a las que la aplicación sabe que no se accederá, y el hardware puede comprender cómo filtrar entre los mosaicos adyacentes, lo que reduce parte de las molestia que experimentan los desarrolladores que realizan la disposición en mosaicos de software por sí mismos.

Sin embargo, para proporcionar una solución completa, debe hacerse algo para solucionar el hecho de que, independientemente de si se admite la disposición en mosaico dentro de una superficie, la dimensión de la superficie máxima actualmente es de 16384, muy lejos de los más de 128K que las aplicaciones ya desean. Simplemente requerir que el hardware admita tamaños de textura más grandes es un método, pero esta ruta supone gastos importantes o ventajas e inconvenientes.

La ruta del filtro de textura y la ruta de representación de Direct3D ya están saturadas en términos de precisión al admitir texturas de 16K con los demás requisitos, como la compatibilidad con extensiones de ventanilla que se salgan de la superficie durante la representación o la compatibilidad del ajuste de texturas fuera del borde de la superficie durante el filtrado. Una posibilidad es definir un equilibrio, de manera que a medida que el tamaño de la textura aumente por encima de 16K, se sacrifiquen la funcionalidad o la precisión de alguna manera. Sin embargo, incluso con esta concesión, podrían requerirse costos de hardware adicionales en cuanto a la capacidad de direccionamiento en todo el sistema de hardware para ir a tamaños de textura más grandes.

## <a name="span-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanspan-idissuewithlargetexturesprecisionforlocationsonsurfacespanissue-with-large-textures-precision-for-locations-on-surface"></a><span id="Issue_with_large_textures__precision_for_locations_on_surface"></span><span id="issue_with_large_textures__precision_for_locations_on_surface"></span><span id="ISSUE_WITH_LARGE_TEXTURES__PRECISION_FOR_LOCATIONS_ON_SURFACE"></span>Problema con las texturas de gran tamaño: precisión para las ubicaciones en la superficie


Un problema que entra en juego cuando las texturas se hacen muy grandes es que las coordenadas de textura de punto de flotación de precisión único (y los interpoladores asociados para admitir la rasterización) se quedan sin precisión para especificar ubicaciones en la superficie de manera precisa. Se produciría un filtrado de textura impreciso. Una opción costosa sería requiere una compatibilidad con interpoladores de precisión dobles, aunque esto podría ser excesivo si se tiene una alternativa razonable.

## <a name="span-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanspan-idenablingmultipleresourcesofdifferentdimensionstosharememoryspanenabling-multiple-resources-of-different-dimensions-to-share-memory"></a><span id="Enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="enabling_multiple_resources_of_different_dimensions_to_share_memory"></span><span id="ENABLING_MULTIPLE_RESOURCES_OF_DIFFERENT_DIMENSIONS_TO_SHARE_MEMORY"></span>Permitir que varios recursos de diferentes dimensiones compartan la memoria


Otro escenario que en el que los recursos de streaming podrían resultar útiles es permitir que varios recursos de diferentes dimensiones o formatos compartan la misma memoria. A veces, las aplicaciones tienen conjuntos exclusivos de recursos que se sabe que no se usan al mismo tiempo, o recursos que se crean solo para un uso muy breve y, a continuación, se destruyen, para luego crear otros recursos. Algo general que puede quedar fuera de los "recursos de streaming" es que es posible permitir al usuario seleccionar varios recursos diferentes en la misma memoria (superpuesta). En otras palabras, la creación y la destrucción de "recursos" (que definen un formato o una dimensión, etc.) se pueden desacoplar de la administración de la memoria subyacente a los recursos desde el punto de vista de la aplicación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de streaming](streaming-resources.md)

 

 




