---
title: Limitaciones de acceso a los mosaicos con asignaciones duplicadas
description: "Existen limitaciones en el acceso a los mosaicos con asignaciones duplicadas, como al copiar recursos de streaming con el origen y el destino superpuestos, o cuando se representa en mosaicos compartidos dentro del área de representación."
ms.assetid: 6E40B1DC-BCF1-4B09-82A8-7B2D9B209A61
keywords: Limitaciones de acceso a los mosaicos con asignaciones duplicadas
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 13895acb5bb14cb1f0e5b91a2060e4eed9265a1c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="tile-access-limitations-with-duplicate-mappings"></a>Limitaciones de acceso a los mosaicos con asignaciones duplicadas


Existen limitaciones en el acceso a los mosaicos con asignaciones duplicadas, como al copiar recursos de streaming con el origen y el destino superpuestos, o cuando se representa en mosaicos compartidos dentro del área de representación.

## <a name="span-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspanspan-idcopyingstreamingresourceswithoverlappingsourceanddestinationspancopying-streaming-resources-with-overlapping-source-and-destination"></a><span id="Copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="copying_streaming_resources_with_overlapping_source_and_destination"></span><span id="COPYING_STREAMING_RESOURCES_WITH_OVERLAPPING_SOURCE_AND_DESTINATION"></span>Copia de recursos de streaming con el origen y el destino superpuestos


Si los mosaicos en el área de origen y destino de una operación Copy\ * cuentan con asignaciones duplicadas en el área de copia que se habrían superpuesto incluso si ambos recursos no fueran recursos de streaming y, además, la operación Copy\ * admite copias superpuestas, la operación Copy\ * se comportará correctamente (como si el origen se copiara en una ubicación temporal antes de ir al destino). Sin embargo, si la superposición no es evidente (por ejemplo, si los recursos de origen y de destino son diferentes pero compartan asignaciones, o si las asignaciones están duplicadas en una superficie determinada), los resultados de la operación de copia en los mosaicos compartidos son indefinidos.

## <a name="span-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspanspan-idcopyingtostreamingresourcewithduplicatedtilesindestinationareaspancopying-to-streaming-resource-with-duplicated-tiles-in-destination-area"></a><span id="Copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="copying_to_streaming_resource_with_duplicated_tiles_in_destination_area"></span><span id="COPYING_TO_STREAMING_RESOURCE_WITH_DUPLICATED_TILES_IN_DESTINATION_AREA"></span>Copia a un recurso de streaming con mosaicos duplicados en el área de destino


La copia en un recurso de streaming con mosaicos duplicados en el área de destino produce resultados indefinidos en estos mosaicos, a menos que los datos en sí sean idénticos; unos mosaicos diferentes podrían escribir los mosaicos en un orden diferente.

## <a name="span-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanspan-iduavaccessestoduplicatetilesmappingsspanuav-accesses-to-duplicate-tiles-mappings"></a><span id="UAV_accesses_to_duplicate_tiles_mappings"></span><span id="uav_accesses_to_duplicate_tiles_mappings"></span><span id="UAV_ACCESSES_TO_DUPLICATE_TILES_MAPPINGS"></span>Accesos a una UAV para asignaciones de mosaicos duplicados


Supongamos que una vista de acceso sin ordenar (UAV) en un recurso de streaming tiene asignaciones de mosaicos duplicados en su área o con otros recursos enlazados a la canalización. La ordenación de los accesos a estos mosaicos duplicados es indefinida si la realizan subprocesos diferentes, igual que cualquier ordenación del acceso de memoria a las UAV en general está desordenada.

## <a name="span-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanspan-idrenderingaftertilemappingchangesorcontentupdatesfromoutsidemappingsspanrendering-after-tile-mapping-changes-or-content-updates-from-outside-mappings"></a><span id="Rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="rendering_after_tile_mapping_changes_or_content_updates_from_outside_mappings"></span><span id="RENDERING_AFTER_TILE_MAPPING_CHANGES_OR_CONTENT_UPDATES_FROM_OUTSIDE_MAPPINGS"></span>Representación después del cambio de la asignación de mosaicos o las actualizaciones de contenido desde asignaciones externas


Si las asignaciones de mosaicos de un recurso de streaming han cambiado, o si el contenido de los mosaicos asignados de un grupo en mosaico ha cambiado mediante las asignaciones de otro recurso de streaming, y dicho recurso de streaming se va a representar a través de la vista de destino de representación o la vista de la galería de símbolos y profundidad, la aplicación debe borrar (con las API Clear de función fija) o copiar completamente encima con las API Copy\*/Update\* los mosaicos que han cambiado dentro del área que se representa (asignada o no).

Si una aplicación no borra o copia en estos casos, las estructuras de optimización de hardware de la vista de destino de representación determinada o la vista de galería de símbolos y profundidad se atascan, lo que producirá resultados de la representación muy malos en cierto hardware e incoherencia entre diferentes dispositivos de hardware. Estas estructuras de datos de optimización ocultas usadas por el hardware pueden ser locales para asignaciones individuales y no ser visibles para otras asignaciones en la misma memoria.

Al borrar una vista de recursos (estableciendo todos los elementos de una vista de recursos en un único valor), se pueden borrar las vistas de destinos de representación con rectángulos. En el caso de un hardware que admita recursos de streaming, borrar una vista de recursos también debe admitir el borrado de las vistas de galería de símbolos y profundidad con rectángulos, para las superficies de solo de profundidad (sin galería de símbolos). Esta operación permite que las aplicaciones borren solo el área necesaria de una superficie.

Si una aplicación necesita conservar el contenido de la memoria existente de áreas de un recurso de streaming en las que las asignaciones han cambiado, dicha aplicación debe solucionar el requisito de borrado. La aplicación puede solucionarlo si primero guarda el contenido en el que han cambiado las asignaciones de mosaicos (copiándolo en una superficie temporal), luego emite el comando de borrar necesario y, a continuación, vuelve a copiar el contenido. Aunque esto conseguiría conservar el contenido de la superficie para una representación incremental, la desventaja es que el rendimiento de la representación posterior en la superficie podría verse afectado, ya que es posible que se pierdan las optimizaciones de la representación.

Cuando un mosaico está asignado a varios recursos de streaming a la vez y el contenido de los iconos se manipula por cualquier medio (representación, copia, etc.) a través de uno de estos recursos de streaming, si el mismo mosaico debe representarse mediante cualquier otro recurso de streaming, antes se debe borrar dicho mosaico como se ha descrito anteriormente.

## <a name="span-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanspan-idrenderingtotilessharedoutsiderenderareaspanrendering-to-tiles-shared-outside-render-area"></a><span id="Rendering_to_tiles_shared_outside_render_area"></span><span id="rendering_to_tiles_shared_outside_render_area"></span><span id="RENDERING_TO_TILES_SHARED_OUTSIDE_RENDER_AREA"></span>Representación en mosaicos compartidos fuera del área de representación


Imaginemos que se va a representar un área de un recurso de streaming y los mosaicos del grupo de mosaicos a los que hace referencia el área de representación también se asignan desde fuera del área de representación (incluido mediante otros recursos de streaming, al mismo tiempo o no). No se garantiza que los datos representados en estos mosaicos aparezcan correctamente al verse a través de otras asignaciones, incluso aunque el diseño de memoria subyacente sea compatible. Esto se produce por las estructuras de datos de optimización que usan algunos dispositivos de hardware, que pueden ser locales para las asignaciones individuales correspondientes a las superficies representables y no ser visibles para otras asignaciones de la misma ubicación de memoria.

Puedes evitar esta restricción si copias desde la asignación representada a todas las demás asignaciones para la misma memoria a las que se pueda acceder (o si borras esa memoria o copias otros datos en ella cuando el contenido antiguo ya no sea necesarios). Aunque esta solución parece redundante, hace que todas las demás asignaciones para la misma memoria entiendan correctamente cómo acceder a su contenido, y al menos se conserva el ahorro de memoria por tener un único respaldo de memoria física.

Además, cuando conmutas entre el uso de distintos recursos de streaming que comparten asignaciones (excepto las de solo lectura), debes especificar una restricción de ordenación de acceso a datos (una barrera) entre varios recursos en mosaico, entre las conmutaciones.

## <a name="span-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanspan-idrenderingtotilessharedwithinrenderareaspanrendering-to-tiles-shared-within-render-area"></a><span id="Rendering_to_tiles_shared_within_render_area"></span><span id="rendering_to_tiles_shared_within_render_area"></span><span id="RENDERING_TO_TILES_SHARED_WITHIN_RENDER_AREA"></span>Representación en mosaicos compartidos dentro del área de representación


Si se está representando un área en un recurso de streaming para el área de representación (y dentro de ella) en la que están asignados varios mosaicos a la misma ubicación del grupo de mosaicos, los resultados de la representación no están definidos en dichos mosaicos.

## <a name="span-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspanspan-iddatacompatibilityacrossstreamingresourcessharingtilesspandata-compatibility-across-streaming-resources-sharing-tiles"></a><span id="Data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="data_compatibility_across_streaming_resources_sharing_tiles"></span><span id="DATA_COMPATIBILITY_ACROSS_STREAMING_RESOURCES_SHARING_TILES"></span>Compatibilidad de los datos en distintos recursos de streaming que comparten mosaicos


Supongamos que existen varios recursos de streaming que tienen asignaciones en las mismas ubicaciones de grupos de mosaicos, y que cada recurso se usa para acceder a los mismos datos. Este escenario solo es válido si se evitan las demás reglas sobre evitar problemas con las estructuras de optimización de hardware, se especifican barreras adecuadas (especificando una restricción del orden de acceso a datos entre varios recursos en mosaico) y los recursos de streaming son compatibles entre sí.

Esto último se describe aquí en términos de lo que significa no ser compatibles para los mosaicos que comparten recursos de streaming. Las condiciones de incompatibilidad del acceso a los mismos datos en distintas asignaciones de mosaicos duplicados son el uso de diferentes dimensiones de superficie o formato, o las diferencias en la presencia de marcas de enlace con el destino de representación o la galería de símbolos y profundidad en los recursos. La escritura en la memoria con un tipo de asignación produce resultados indefinido si posteriormente se lee o se representa mediante una asignación procedente de un recurso incompatible.

Si las otras asociaciones que comparten recursos se inicializan primero con datos nuevos (reciclando la memoria para un propósito diferente), las operaciones posteriores de lectura o representación serán correctas, ya que los datos no se extraerán de interpretaciones incompatibles. Sin embargo, al cambiar entre el acceso a asignaciones incompatibles como esta, debes especificar barreras (especificando una restricción en el orden de acceso a datos entre varios recursos en mosaico).

Si la marca de enlace con el destino de representación o la galería de símbolos y profundidad no está establecida en cualquiera de los recursos que comparten asignaciones entre sí, hay muchas menos restricciones. Siempre que los tipos de formato y de superficie (por ejemplo, Texture2D) sean los mismos, los mosaicos pueden compartirse. Que distintos formatos sean compatibles se produce en casos como superficies BC\* y el formato de tamaño equivalente sin comprimir de 32 bits o 16 bits por componente, como BC6H y R32G32B32A32. Muchos formatos de 32 bits por elemento pueden funcionar con R32\_\* también (R10G10B10A2\_\*, R8G8B8A8\_\*, B8G8R8A8\_\*,B8G8R8X8\_\*,R16G16\_\*); esta operación siempre se ha permitido para los recursos que no son de streaming.

El uso compartido entre mosaicos empaquetados y sin empaquetar está permitido si los formatos son compatibles y los mosaicos están rellenos con un color sólido.

Por último, si los recursos que comparten asignaciones de mosaicos no tienen nada en común salvo que ninguno tiene marcas de enlace del destino de representación o la galería de símbolos y profundidad, únicamente se puede compartir de forma segura la memoria rellena con 0; la asignación aparecerá como lo que se decodifique de 0 para la definición del formato de recurso determinado (normalmente, simplemente 0).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




