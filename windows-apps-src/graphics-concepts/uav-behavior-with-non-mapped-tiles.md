---
title: Comportamiento UAV con iconos sin asignar
description: El comportamiento de las lecturas y escrituras de las vistas de acceso sin ordenar (UAV) dependen del nivel de compatibilidad del hardware.
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords: Comportamiento UAV con iconos sin asignar
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: c5e3b3be467a49839d530fd976c7f756421de218
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>Comportamiento UAV con iconos sin asignar


El comportamiento de las lecturas y escrituras de las vistas de acceso sin ordenar (UAV) dependen del nivel de compatibilidad del hardware. Para ver un desglose de los requisitos, consulta el comportamiento de lectura y escritura general de los [niveles de características de recursos de streaming](streaming-resources-features-tiers.md). Esta sección resume el comportamiento ideal.

Las operaciones de sombreador que se leen en un icono no asignado en un UAV devuelven 0 en todos los componentes que no faltan del formato y el valor predeterminado de los componentes que faltan.

Las operaciones de sombreador que intentan escribir en un icono no asignado provocan que no se escriba nada en el área no asignada (mientras sí se puede escribir en un área asignada). Esta definición ideal para el control de la escritura no se requiere para el [nivel 2](tier-2.md); las escrituras en iconos sin asignar pueden acabar en una caché que lecturas posteriores podrían seleccionar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 



