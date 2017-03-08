---
title: "Mosaico de búfer"
description: "Un recurso de búfer se divide en mosaicos de 64 KB, con algo de espacio vacío en el último mosaico si el tamaño no es un múltiplo de 64 KB."
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- "Mosaico de búfer"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2382974de7146f4ba64c2422a01c4d3d297a8977
ms.lasthandoff: 02/07/2017

---

# <a name="buffer-tiling"></a>Mosaico de búfer


Un recurso de [búfer](introduction-to-buffers.md) se divide en mosaicos de 64 KB, con algo de espacio vacío en el último mosaico si el tamaño no es un múltiplo de 64 KB.

Los búferes estructurados no deben tener ninguna limitación en el intervalo para organizarse en mosaico. Pero las posibles optimizaciones de rendimiento en el hardware por usar [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) podrían verse invalidadas si se organizan en mosaico en primer lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cómo se organiza en mosaico el área de un recurso de streaming](how-a-streaming-resource-s-area-is-tiled.md)

 

 





