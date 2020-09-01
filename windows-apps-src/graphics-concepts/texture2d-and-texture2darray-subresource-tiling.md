---
title: Mosaico de subrecurso Texture2D y Texture2DArray
description: En estas tablas se muestra cómo se muestran los Subrecursos Texture2D y Texture2DArray en mosaico.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Mosaico de subrecurso Texture2D y Texture2DArray
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 28152f39983f4831a9efa981efcb85fb65fa0204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156179"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Mosaico de subrecurso Texture2D y Texture2DArray


En estas tablas se muestra cómo se muestran los Subrecursos [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) y [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) en mosaico. Los valores de estas tablas no cuentan el empaquetado de la cola MIP.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Subrecursos con recuentos Multimuestra de 1


En esta tabla se muestra cómo se muestran en mosaico los Subrecursos [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) y [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) con recuentos Multimuestra de 1.

| Bits/píxel (1 ejemplo/píxel) | Dimensiones del mosaico (píxeles, WxH) |
|-----------------------------|-------------------------------|
| 8                           | 256x256                       |
| 16                          | 256x128                       |
| 32                          | 128x128                       |
| 64                          | 128x64                        |
| 128                         | 64x64                         |
| BC1, 4                       | 512x256                       |
| BC2, 3, 5, 6, 7                 | 256x256                       |

 

Los recuentos de bits de formato no admitidos en los recursos de streaming son formatos de 96 BPP, formatos de vídeo, formato de DXGI \_ \_ R1 \_ UNORM, formato de dxgi \_ \_ R8G8 \_ B8G8 \_ UNORM y formato de dxgi \_ \_ R8R8 \_ \_ G8B8 UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Subrecursos con varios recuentos de muestras Multimuestra


En esta tabla se muestra cómo se muestran en mosaico los Subrecursos [**Texture2D**](/windows/desktop/direct3dhlsl/sm5-object-texture2d) y [**Texture2DArray**](/windows/desktop/direct3dhlsl/sm5-object-texture2darray) con varios recuentos de muestras.

| Bits/píxel (1 ejemplo/píxel) | Dimensiones del mosaico (píxeles, WxH) |
|-----------------------------|-------------------------------|
| 1                           | asesoramiento                           |
| 2                           | 2x1                           |
| 4                           | 2x2                           |
| 8                           | 4 TB                           |
| 16                          | 4 x 4                           |

 

Solo se requieren (y se permiten) los recuentos de ejemplo 1 y 4 para que se admitan con recursos de streaming. Los recursos de streaming no admiten actualmente 2, 8 y 16 aunque se muestren.

Las implementaciones pueden optar por admitir el modo 2, 8 o 16 de ejemplo de suavizado de contorno de muestreo múltiple (MSAA) para recursos que no son de streaming, aunque el recurso de streaming no los admita.

Los recursos de streaming con recuentos de muestras mayores que 1 no pueden usar formatos de 128 BPP.

Las restricciones en los recuentos de muestras y formatos admitidos se deben a las incoherencias de hardware de la especificación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[¿Cómo se organiza en mosaico el área de un recurso de streaming?](how-a-streaming-resource-s-area-is-tiled.md)

 

 