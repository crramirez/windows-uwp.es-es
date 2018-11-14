---
title: Nivel 2
description: La compatibilidad de nivel 2 para los recursos de streaming agrega funcionalidades a las del nivel 1, como garantizar mapas MIP de texturas sin empaquetar cuando el tamaño es al menos una forma de mosaico estándar; instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; además, la lectura en mosaicos asignados NULL trata el valor de muestra como cero.
ms.assetid: 111A28EA-661A-4D29-921A-F2E376A46DC5
keywords:
- Nivel 2
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fac8780995231ce56d1264ea8a1a5cb52fd9a3d0
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6258073"
---
# <a name="tier-2"></a>Nivel 2


La compatibilidad de nivel 2 para los recursos de streaming agrega funcionalidades a las del nivel 1, como garantizar mapas MIP de texturas sin empaquetar cuando el tamaño es al menos una forma de mosaico estándar; instrucciones del sombreador para la compresión de nivel de detalle (LOD) y para obtener el estado de la operación del sombreador; además, la lectura en mosaicos asignados NULL tratan el valor de muestra como cero.

## <a name="span-idtier2generalsupportspanspan-idtier2generalsupportspanspan-idtier2generalsupportspantier-2-general-support"></a><span id="Tier_2_general_support"></span><span id="tier_2_general_support"></span><span id="TIER_2_GENERAL_SUPPORT"></span>Compatibilidad general del nivel 2


La compatibilidad del nivel 2 incluye lo siguiente.

-   Hardware en el nivel de características 11.1 como mínimo.
-   Todas las características del nivel anterior (sin las limitaciones específicas del [nivel 1](tier-1.md)), con el añadido en los siguientes elementos:
-   Están disponibles instrucciones de sombreador para la compresión del LOD y los comentarios sobre el estado asignado. Consulta [Exposición de recursos de streaming de HLSL](hlsl-streaming-resources-exposure.md).

Además, hay algunos problemas de compatibilidad específicos que se explican a continuación.

## <a name="span-idnon-mappedtilesspanspan-idnon-mappedtilesspanspan-idnon-mappedtilesspannon-mapped-tiles"></a><span id="Non-mapped_tiles"></span><span id="non-mapped_tiles"></span><span id="NON-MAPPED_TILES"></span>Mosaicos sin asignar


Las lecturas desde mosaicos sin asignar devuelven 0 en todos los componentes del formato que no faltan y el valor predeterminado para los componentes que faltan.

Las escrituras en mosaicos sin asignar no van a la memoria, pero es posible que acaben en memorias caché que lecturas posteriores en la misma dirección podrían detectar o no.

## <a name="span-idtexturefilteringspanspan-idtexturefilteringspanspan-idtexturefilteringspantexture-filtering"></a><span id="Texture_filtering"></span><span id="texture_filtering"></span><span id="TEXTURE_FILTERING"></span>Filtrado de texturas


El filtrado de texturas con una superficie que incluya mosaicos **NULL** y no **NULL** contribuye 0 (con valores predeterminados para los componentes de formato que faltan) para los elementos de textura en mosaicos **NULL** en la operación general de filtro. Algún hardware anterior no cumple este requisito y devuelve 0 (con valores predeterminados para los componentes de formato que faltan) para el resultado del filtro completo si cualquier elemento de textura (con un ancho distinto de cero) se ubica en un mosaico **NULL**. No se permitirá a ningún otro hardware que pase por alto el requisito de incluir todos los elementos de textura (con ponderación distinta de cero) en la operación de filtro.

Los accesos a elementos de textura **NULL** hacen que la operación [**CheckAccessFullyMapped**](https://msdn.microsoft.com/library/windows/desktop/dn292083) para los comentarios de estado respecto a la lectura de una textura devuelva false. Esto sucede independientemente del modo en que el resultado del acceso a la textura puede escribirse enmascarado en el sombreador y de cuántos componentes haya en el formato de la textura (la combinación de los cuales pueden hacer que parezca que no es necesario acceder a la textura).

## <a name="span-idalignmentconstraintsspanspan-idalignmentconstraintsspanspan-idalignmentconstraintsspanalignment-constraints"></a><span id="Alignment_constraints"></span><span id="alignment_constraints"></span><span id="ALIGNMENT_CONSTRAINTS"></span>Restricciones de alineación


Restricciones de alineación para las formas de mosaico estándar: Los mapas MIP que llenen al menos un mosaico estándar en todas las dimensiones tienen garantizado usar la disposición en mosaico estándar, y el resto se considerarán empaquetados como una **unidad** en N mosaicos (N se notifica a la aplicación). La aplicación puede asignar los N mosaicos en ubicaciones arbitrariamente desligadas de un grupo de mosaicos, pero debes asignar todos o ninguno de los mosaicos empaquetados. El empaquetado MIP es un conjunto exclusivo de mosaicos empaquetado por segmento de matriz.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrado de reducción mínimo/máximo


Se admite el filtrado de reducción mínimo/máximo. Consulta [Características de muestreo de texturas de recursos de streaming](streaming-resources-texture-sampling-features.md).

## <a name="span-idlimitationsspanspan-idlimitationsspanspan-idlimitationsspanlimitations"></a><span id="Limitations"></span><span id="limitations"></span><span id="LIMITATIONS"></span>Limitaciones


Los recursos de streaming con mapas MIP menores que el tamaño de mosaico estándar en cualquier dimensión no pueden tener un tamaño de matriz mayor que 1.

Las limitaciones respecto a cómo se puede acceder a los mosaicos cuando hay asignaciones duplicadas se siguen aplicando. Consulta [Limitaciones de acceso a los iconos con asignaciones duplicadas](tile-access-limitations-with-duplicate-mappings.md).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Niveles de características de los recursos de streaming](streaming-resources-features-tiers.md)

 

 




