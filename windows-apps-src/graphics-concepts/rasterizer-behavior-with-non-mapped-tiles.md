---
title: Comportamiento de rasterizador con iconos sin asignar
description: "En esta sección se describe un comportamiento de rasterizador con iconos sin asignar."
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords: Comportamiento de rasterizador con iconos sin asignar
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: b4f85689af4d1f4d69c7f50f094fc8487e4b59e6
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Comportamiento de rasterizador con iconos sin asignar


En esta sección se describe un comportamiento de rasterizador con iconos sin asignar.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


El comportamiento de las lecturas y escrituras de las vistas de galería de símbolos de profundidad (DSV) depende del nivel de compatibilidad del hardware. Para ver un desglose de los requisitos, consulta el comportamiento de lectura y escritura general de los [niveles de características de recursos de streaming](streaming-resources-features-tiers.md).

Este es el comportamiento ideal:

Si no se ha asignado ningún icono en DepthStencilView, el valor devuelto de profundidad de lectura es 0, que posteriormente se incorpora en todas las operaciones que se configuren para el valor de lectura de profundidad. La escrituras en el icono de profundidad ausente se descartan. Esta definición ideal para el control de la escritura no se requiere para el [nivel 2](tier-2.md); las escrituras en iconos sin asignar pueden acabar en una caché que lecturas posteriores podrían seleccionar.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


El comportamiento de las lecturas y escrituras de las vistas de destino de representación (RTV) depende del nivel de compatibilidad del hardware. Para ver un desglose de los requisitos, consulta el comportamiento de lectura y escritura general de los [niveles de características de recursos de streaming](streaming-resources-features-tiers.md).

En todas las implementaciones, diferentes vistas RTV (y DSV) enlazadas simultáneamente pueden tener distintas áreas asignadas en contraposición a áreas sin asignar y pueden tener formatos de superficie de tamaño diferente (lo que implica formas de icono diferentes).

Este es el comportamiento ideal:

Las lecturas de vistas RTV devuelven 0 en los iconos ausentes y las escrituras se descartan. Esta definición ideal para el control de la escritura no se requiere para el [nivel 2](tier-2.md); las escrituras en iconos sin asignar pueden acabar en una caché que lecturas posteriores podrían seleccionar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Acceso de canalización a recursos de streaming](pipeline-access-to-streaming-resources.md)

 

 



