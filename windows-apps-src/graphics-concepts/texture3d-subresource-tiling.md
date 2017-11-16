---
title: "Organización en mosaico de los subrecursos Texture3D"
description: "Esta tabla muestra cómo los subrecursos Texture3D se organizan en mosaico."
ms.assetid: 210D03E4-CF12-47E0-BA2F-C8D059B17D3E
keywords: "Organización en mosaico de los subrecursos Texture3D"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 3232ac03c2d0079faabff40b9325e819f8aca147
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="texture3d-subresource-tiling"></a>Organización en mosaico de los subrecursos Texture3D


Esta tabla muestra cómo los subrecursos [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) se organizan en mosaico. Los valores de esta tabla no cuentan el empaquetado de MIP de cola.

Esta tabla toma la organización en mosaico de [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525), divide las dimensiones x/y por 4 y agrega 16 capas de profundidad. Todos los mosaicos del primer plano (plano 2D del mosaicos que definen las primeras 16 capas de profundidad) aparecen antes que los planos posteriores.

**Nota** En la implementación inicial de los recursos de streaming no se expone la compatibilidad de [**Texture3D**](https://msdn.microsoft.com/library/windows/desktop/ff471562) con dichos recursos de streaming, pero aquí se incluyen las formas de mosaico deseadas por su posible compatibilidad en una versión futura.

 

| Bits/píxel (1 muestra/píxel) | Dimensiones de mosaico (píxeles, A x A x P) |
|-----------------------------|---------------------------------|
| 8                           | 64 x 32 x 32                        |
| 16                          | 32 x 32 x 32                        |
| 32                          | 32 x 32 x 16                        |
| 64                          | 32 x 16 x 16                        |
| 128                         | 16 x 16 x 16                        |
| BC1, 4                       | 128 x 64 x 16                       |
| BC2, 3, 5, 6 y 7                 | 64 x 64 x 16                        |

 

Los recuentos de bits de formato no admitidos con los recursos de streaming son los formatos de 96 bpp, los formatos de vídeo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM y DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cómo se organiza en mosaico el área de un recurso de streaming](how-a-streaming-resource-s-area-is-tiled.md)

 

 



