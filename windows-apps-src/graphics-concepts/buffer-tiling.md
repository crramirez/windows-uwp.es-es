---
title: Mosaico de búfer
description: Un recurso de búfer se divide en mosaicos de 64 KB, con un espacio vacío en el último mosaico si el tamaño no es múltiplo de 64 KB.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaico de búfer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156359"
---
# <a name="buffer-tiling"></a>Mosaico de búfer


Un recurso de [búfer](introduction-to-buffers.md) se divide en mosaicos de 64 KB, con un espacio vacío en el último mosaico si el tamaño no es múltiplo de 64 KB.

Los búferes estructurados no deben tener ninguna restricción en el intervalo que se va a colocar en mosaico. Sin embargo, las posibles optimizaciones de rendimiento en el hardware para el uso de [**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) se pueden sacrificar haciéndolos en primer lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[¿Cómo se organiza en mosaico el área de un recurso de streaming?](how-a-streaming-resource-s-area-is-tiled.md)

 

 