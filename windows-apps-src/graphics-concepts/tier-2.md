---
title: Nivel 2
description: La compatibilidad de nivel 2 con recursos de streaming agrega funcionalidades más allá del nivel 1, como garantizar el mipmap de textura no empaquetada cuando el tamaño es al menos una forma de mosaico estándar. instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; Además, la lectura de los mosaicos asignados con valores NULL trata el valor muestreado como cero.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Nivel 2
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 62fbbf3f21d614739918478b58394e51dbe577f9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175169"
---
# <a name="tier-2"></a>Nivel 2


La compatibilidad de nivel 2 con recursos de streaming agrega funcionalidades más allá del nivel 1, como garantizar el mipmap de textura no empaquetada cuando el tamaño es al menos una forma de mosaico estándar. instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; Además, la lectura de los mosaicos asignados con valores NULL trata el valor muestreado como cero.

## <a name="span-idtier_2_general_supportspanspan-idtier_2_general_supportspanspan-idtier_2_general_supportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Soporte general de nivel 2


La compatibilidad con el nivel 2 incluye lo siguiente.

-   Hardware en el nivel de características 11,1 mínimo.
-   Todas las características del nivel anterior (sin limitaciones específicas de [nivel 1](tier-1.md) ) además de las adiciones en los siguientes elementos:
-   Hay disponibles instrucciones del sombreador para la compresión de LOD y comentarios de estado asignados. Consulte [exposición de recursos de streaming de HLSL](hlsl-streaming-resources-exposure.md).

Además, hay algunos problemas de compatibilidad específicos que se indican a continuación.

## <a name="span-idnon-mapped_tilesspanspan-idnon-mapped_tilesspanspan-idnon-mapped_tilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Mosaicos no asignados


Las lecturas de mosaicos no asignados devuelven 0 en todos los componentes que no faltan del formato y el valor predeterminado para los componentes que faltan.

Las escrituras en mosaicos no asignados se detienen de ir a la memoria, pero pueden acabar en las memorias caché que las lecturas posteriores de la misma dirección podrían o no retomarse.

## <a name="span-idtexture_filteringspanspan-idtexture_filteringspanspan-idtexture_filteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Filtrado de textura


El filtrado de texturas con una superficie que abarca los mosaicos **nulos** y los que no son**null** aporta 0 (con valores predeterminados para los componentes de formato que faltan) para textura en los mosaicos **nulos** en la operación de filtro global. Algunos equipos de hardware temprano no cumplen este requisito y devuelven 0 (con valores predeterminados para los componentes de formato que faltan) para el resultado del filtro completo si cualquier textura (con un peso distinto de cero) se encuentra en un icono **nulo** . No se permitirá que ningún otro hardware pierda el requisito de incluir todos los textura (distinto de cero ponderados) en la operación de filtro.

Los accesos de textura **nulos** provocan que la operación [**CheckAccessFullyMapped**](/windows/desktop/direct3dhlsl/checkaccessfullymapped) en la información de estado de una textura leída devuelva false. Esto se debe a la manera en que el resultado de acceso de la textura podría aparecer enmascarado en el sombreador y el número de componentes que se encuentran en el formato de textura (la combinación de puede hacer que parezca que no es necesario tener acceso a la textura).

## <a name="span-idalignment_constraintsspanspan-idalignment_constraintsspanspan-idalignment_constraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Restricciones de alineación


Restricciones de alineación para las formas de mosaico estándar: los mapas MIP que rellenan al menos un icono estándar en todas las dimensiones tienen la garantía de que usan el mosaico estándar, donde el resto se considera empaquetado como una **unidad** en N mosaicos (n se ha comunicado a la aplicación). La aplicación puede asignar los iconos N en ubicaciones contiguas de forma arbitraria en un grupo de mosaicos, pero debe asignar todos los iconos empaquetados o ninguno de ellos. El empaquetado de MIP es un conjunto único de mosaicos empaquetados por segmento de matriz.

## <a name="span-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanspan-idmin_max_reduction_filteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mín./máx.


Se admite el filtrado de reducción mínima/máxima. Consulte [características de muestreo de textura de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Límite


No se permite que los recursos de streaming con cualquier mipmaps que no sea el tamaño de mosaico estándar en ninguna dimensión tengan un tamaño de matriz mayor que 1.

Limitaciones en el modo en que se puede obtener acceso a los mosaicos cuando se siguen aplicando asignaciones duplicadas. Consulte [limitaciones de acceso a iconos con asignaciones duplicadas](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Niveles de características de recursos de streaming](streaming-resources-features-tiers.md)

 

 