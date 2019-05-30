---
title: Mosaico de subrecurso Texture3D
description: Esta tabla muestra cómo los subrecursos Texture3D se organizan en mosaico.
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords:
- Mosaico de subrecurso Texture3D
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b63fdeeffd4b95afab6556b6f0318732ff988b0
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370906"
---
# <a name="texture3d-subresource-tiling"></a>Mosaico de subrecurso Texture3D


Esta tabla muestra cómo los subrecursos [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) se organizan en mosaico. Los valores de esta tabla no cuentan el empaquetado de MIP de cola.

Esta tabla toma la organización en mosaico de [**Texture2D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture2d), divide las dimensiones x/y por 4 y agrega 16 capas de profundidad. Todos los mosaicos del primer plano (plano 2D del mosaicos que definen las primeras 16 capas de profundidad) aparecen antes que los planos posteriores.

**Nota** En la implementación inicial de los recursos de streaming no se expone la compatibilidad de   [**Texture3D**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/sm5-object-texture3d) con dichos recursos de streaming, pero aquí se incluyen las formas de mosaico deseadas por su posible compatibilidad en una versión futura.

 

| Bits/píxel (1 muestra/píxel) | Dimensiones de mosaico (píxeles, A x A x P) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1, 4                       | 128 x 64 x 16                       |
| BC2, 3, 5, 6 y 7                 | 64 x 64 x 16                        |

 

Recuentos de bits de formato no compatibles con los recursos de streaming son 96 formatos bpp, formatos de vídeo, DXGI\_formato\_R1\_UNORM, DXGI\_formato\_R8G8\_B8G8\_UNORM, y DXGI\_formato\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[¿Cómo se coloca en mosaico de área de un recurso de transmisión por secuencias](how-a-streaming-resource-s-area-is-tiled.md)

 

 




