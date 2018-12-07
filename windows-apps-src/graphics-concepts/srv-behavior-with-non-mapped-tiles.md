---
title: Comportamiento de la SRV con iconos sin asignar
description: El comportamiento de las lecturas de la vista de recurso de sombreador (SRV) que implican iconos sin asignar depende del nivel de compatibilidad del hardware.
ms.assetid: 0E1D64BE-EB09-4F9D-9800-BD23A3B374EE
keywords:
- Comportamiento de la SRV con iconos sin asignar
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d62a1d3158e13187f89277a1ba009bd56fc2b39a
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8785864"
---
# <a name="span-iddirect3dconceptssrvbehaviorwithnon-mappedtilesspansrv-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.srv_behavior_with_non-mapped_tiles"></span>Comportamiento de la SRV con iconos sin asignar


El comportamiento de las lecturas de la vista de recurso de sombreador (SRV) que implican iconos sin asignar depende del nivel de compatibilidad del hardware. Para ver un desglose de los requisitos, consulta el comportamiento de la lectura para los [Niveles de características de los recursos de streaming](streaming-resources-features-tiers.md). En esta sección se resume el comportamiento ideal, que requiere el [nivel 2](tier-2.md).

Pensemos en una operación de filtro de textura que lea un conjunto de elementos de textura en una SRV. Los elementos de textura que se encuentran en iconos sin asignar tienen una contribución de 0 en todos los componentes que no falten del formato (y el valor predeterminado de los componentes que faltan) en la operación de filtro general junto con las contribuciones de los elementos de textura asignados. Todos los elementos de textura se ponderan y combinan juntos, con independencia de si los datos procedían de iconos asignados o sin asignar.

Algún hardware de [nivel 2](tier-2.md) de primera generación no cumple este requisito específico y devuelve el valor 0 con los valores predeterminados descritos antes como el resultado del filtro general si cualquier elemento de textura (con una ponderación distinta de 0) se encuentra en iconos sin asignar. No se permitirá a ningún otro hardware que pase por alto el requisito de incluir todos los elementos de textura (con ponderación distinta de cero) en el filtro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




