---
title: Mosaico de búfer
description: Un recurso de búfer se divide en mosaicos de 64 KB, con algo de espacio vacío en el último mosaico si el tamaño no es un múltiplo de 64 KB.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaico de búfer
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f3e5e117e05cef478ede508240a6b1d1022dea70
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655730"
---
# <a name="buffer-tiling"></a>Mosaico de búfer


Un recurso de [búfer](introduction-to-buffers.md) se divide en iconos de 64 KB, con algo de espacio vacío en el último icono si el tamaño no es un múltiplo de 64 KB.

Los búferes estructurados no deben tener ninguna limitación en el intervalo para organizarse en mosaico. Pero las posibles optimizaciones de rendimiento en el hardware por usar [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) podrían verse invalidadas si se organizan en mosaico en primer lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[¿Cómo se coloca en mosaico de área de un recurso de transmisión por secuencias](how-a-streaming-resource-s-area-is-tiled.md)

 

 




