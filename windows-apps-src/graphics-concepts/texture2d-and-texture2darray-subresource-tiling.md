---
title: Organización en mosaico de los subrecursos Texture2D y Texture2DArray
description: Estas tablas muestran cómo los subrecursos Texture2D y Texture2DArray se organizan en mosaico.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Organización en mosaico de los subrecursos Texture2D y Texture2DArray
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 245581e4eb2a8526b242feadb5877590283e24f9
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7445711"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Organización en mosaico de los subrecursos Texture2D y Texture2DArray


Estas tablas muestran cómo los subrecursos [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) se organizan en mosaico. Los valores de estas tablas no cuentan el empaquetado de MIP de cola.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Subrecursos con recuentos de muestras múltiples de 1


En esta tabla se muestra cómo los subrecursos [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) con recuentos de muestras múltiples de 1 se organizan en mosaico.

| Bits/píxel (1 muestra/píxel) | Dimensiones de mosaico (píxeles, A x A) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64 x 64                         |
| BC1, 4                       | 512 x 256                       |
| BC2, 3, 5, 6 y 7                 | 256 x 256                       |

 

Los recuentos de bits de formato no admitidos con los recursos de streaming son los formatos de 96 bpp, los formatos de vídeo, DXGI\_FORMAT\_R1\_UNORM, DXGI\_FORMAT\_R8G8\_B8G8\_UNORM y DXGI\_FORMAT\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Subrecursos con diversos recuentos de muestras múltiples


En esta tabla se muestra cómo los subrecursos [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) con diversos recuentos de muestras múltiples se organizan en mosaico.

| Bits/píxel (1 muestra/píxel) | Dimensiones de mosaico (píxeles, A x A) |
|-----------------------------|-------------------------------|
| 1                           | 1 x 1                           |
| 2                           | 2 x 1                           |
| 4                           | 2 x 2                           |
| 8                           | 4 x 2                           |
| 16                          | 4 x 4                           |

 

Solo son necesarios (y solo se permiten) los recuentos de muestras 1 y 4 para la compatibilidad con los recursos de streaming. Actualmente, los recursos de streaming no admiten 2, 8 y 16, aunque se muestren.

La implementaciones pueden elegir admitir el modo de suavizado de contorno de muestras múltiples (MSAA) de las muestras 2, 8 o 16 para los recursos que no sean de streaming, incluso aunque los recursos de streaming no los admitan.

los recursos de streaming con recuentos de muestras mayores que 1 no pueden usar los formatos de 128 bpp.

Las restricciones sobre los formatos y recuentos de muestras admitidos se deben a incoherencias del hardware respecto a sus especificaciones.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cómo se organiza en mosaico el área de un recurso de streaming](how-a-streaming-resource-s-area-is-tiled.md)

 

 




