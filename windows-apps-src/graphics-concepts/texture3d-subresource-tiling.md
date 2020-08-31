---
title: Mosaico de subrecurso Texture3D
description: Vea una tabla que muestra cómo se muestran los Subrecursos Texture3D en mosaico, en función de los bits por píxel del mosaico Texture2D.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Mosaico de subrecurso Texture3D
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ee4ff5c87022f9fd303b1331665a2551704cb93
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167949"
---
# <a name="texture3d-subresource-tiling"></a>Mosaico de subrecurso Texture3D


En esta tabla se muestra cómo se muestran los Subrecursos de [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) en mosaico. Los valores de esta tabla no cuentan el empaquetado de la cola MIP.

En esta tabla se toma el mosaico [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) y se dividen las dimensiones x/y en 4 cada una de ellas y se agregan 16 niveles de profundidad. Todos los mosaicos para el primer plano (plano 2D de mosaicos que definen las 16 primeras capas de profundidad) aparecen antes que los planos posteriores.

**Tenga en cuenta**que la compatibilidad con  [**Texture3D**](/windows/desktop/direct3dhlsl/sm5-object-texture3d) en los recursos de streaming no se expone en la implementación inicial de los recursos de streaming, pero las formas de iconos deseadas se enumeran aquí para una posible compatibilidad en una versión futura.

 

| Bits/píxel (1 ejemplo/píxel) | Dimensiones del mosaico (píxeles, a la x) |
|-----------------------------|---------------------------------|
| 8                           | 64x32x32                        |
| 16                          | 32x32x32                        |
| 32                          | 32x32x16                        |
| 64                          | 32x16x16                        |
| 128                         | 16x16x16                        |
| BC1, 4                       | 128x64x16                       |
| BC2, 3, 5, 6, 7                 | 64x64x16                        |

 

Los recuentos de bits de formato no admitidos en los recursos de streaming son formatos de 96 BPP, formatos de vídeo, formato de DXGI \_ \_ R1 \_ UNORM, formato de dxgi \_ \_ R8G8 \_ B8G8 \_ UNORM y formato de dxgi \_ \_ R8R8 \_ \_ G8B8 UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[¿Cómo se organiza en mosaico el área de un recurso de streaming?](how-a-streaming-resource-s-area-is-tiled.md)

 

 