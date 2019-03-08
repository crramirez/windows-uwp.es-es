---
title: Mosaico de subrecurso Texture2D y Texture2DArray
description: Estas tablas muestran cómo los subrecursos Texture2D y Texture2DArray se organizan en mosaico.
ms.assetid: 2DC14DFC-5299-44D9-895F-5A223D3FD530
keywords:
- Mosaico de subrecurso Texture2D y Texture2DArray
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2175ce19824068a850ff70340b467f09e5c76540
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592750"
---
# <a name="texture2d-and-texture2darray-subresource-tiling"></a>Mosaico de subrecurso Texture2D y Texture2DArray


Estas tablas muestran cómo los subrecursos [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) se organizan en mosaico. Los valores de estas tablas no cuentan el empaquetado de MIP de cola.

## <a name="span-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spanspan-idsubresources-with-multisample-counts-of-1spansubresources-with-multisample-counts-of-1"></a><span id="Subresources-with-multisample-counts-of-1"></span><span id="subresources-with-multisample-counts-of-1"></span><span id="SUBRESOURCES-WITH-MULTISAMPLE-COUNTS-OF-1"></span>Recursos secundarios de Metadata con un número del 1 de muestreo múltiple


En esta tabla se muestra cómo los subrecursos [**Texture2D**](https://msdn.microsoft.com/library/windows/desktop/ff471525) y [**Texture2DArray**](https://msdn.microsoft.com/library/windows/desktop/ff471526) con recuentos de muestras múltiples de 1 se organizan en mosaico.

| Bits/píxel (1 muestra/píxel) | Dimensiones de mosaico (píxeles, A x A) |
|-----------------------------|-------------------------------|
| 8                           | 256 x 256                       |
| 16                          | 256 x 128                       |
| 32                          | 128 x 128                       |
| 64                          | 128 x 64                        |
| 128                         | 64x64                         |
| BC1, 4                       | 512 x 256                       |
| BC2, 3, 5, 6 y 7                 | 256 x 256                       |

 

Recuentos de bits de formato no compatibles con los recursos de streaming son 96 formatos bpp, formatos de vídeo, DXGI\_formato\_R1\_UNORM, DXGI\_formato\_R8G8\_B8G8\_UNORM, y DXGI\_formato\_R8R8\_G8B8\_UNORM.

## <a name="span-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspanspan-idsubresources-with-various-multisample-countsspansubresources-with-various-multisample-counts"></a><span id="Subresources-with-various-multisample-counts"></span><span id="subresources-with-various-multisample-counts"></span><span id="SUBRESOURCES-WITH-VARIOUS-MULTISAMPLE-COUNTS"></span>Recursos secundarios de Metadata con distintos números de muestreo múltiple


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


[¿Cómo se coloca en mosaico de área de un recurso de transmisión por secuencias](how-a-streaming-resource-s-area-is-tiled.md)

 

 




