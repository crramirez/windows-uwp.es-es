---
title: Comportamiento de rasterizador con iconos sin asignar
description: Obtenga información sobre los comportamientos del rasterizador DepthStencilView (DSV) y RenderTargetView (RTV) con mosaicos no asignados.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- Comportamiento de rasterizador con iconos sin asignar
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f8085d8d29a86c0c5da82f6cb98c57c037b81beb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171889"
---
# <a name="span-iddirect3dconceptsrasterizer_behavior_with_non-mapped_tilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Comportamiento de rasterizador con iconos sin asignar


En esta sección se describe el comportamiento de rasterizador con mosaicos no asignados.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


El comportamiento de las lecturas y escrituras de la vista de galería de símbolos de profundidad (DSV) depende del nivel de compatibilidad de hardware. Para obtener un desglose de los requisitos, consulte el comportamiento general de lectura y escritura para [los niveles de características de recursos de streaming](streaming-resources-features-tiers.md).

Este es el comportamiento ideal:

Si un mosaico no se asigna en DepthStencilView, el valor devuelto de la profundidad de lectura es 0, que se introduce en las operaciones configuradas para el valor de lectura de profundidad. Las escrituras en el icono de profundidad que falta se quitan. Esta definición ideal para el control de escritura no es necesaria en el [nivel 2](tier-2.md); las escrituras en mosaicos no asignados pueden acabar en una memoria caché que las lecturas posteriores podrían recogerse.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


El comportamiento de las lecturas y escrituras de la vista de destino de representación (RTV) depende del nivel de compatibilidad de hardware. Para obtener un desglose de los requisitos, consulte el comportamiento general de lectura y escritura para [los niveles de características de recursos de streaming](streaming-resources-features-tiers.md).

En todas las implementaciones, los distintos RTVs (y DSV) enlazados simultáneamente pueden tener distintas áreas asignadas frente a no asignadas y pueden tener formatos de superficie de tamaño diferentes (lo que significa diferentes formas de mosaico).

Este es el comportamiento ideal:

Las lecturas de RTVs devuelven 0 en los mosaicos que faltan y las escrituras se quitan. Esta definición ideal para el control de escritura no es necesaria en el [nivel 2](tier-2.md); las escrituras en mosaicos no asignados pueden acabar en una memoria caché que las lecturas posteriores podrían recogerse.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 




