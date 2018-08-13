---
title: Mosaico de búfer
description: Un recurso de búfer se divide en mosaicos de 64KB, con algo de espacio vacío en el último mosaico si el tamaño no es un múltiplo de 64KB.
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- Mosaico de búfer
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 03769964bfe3eff13314e62b8594edd5509b26fb
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "1044254"
---
# <a name="buffer-tiling"></a>Mosaico de búfer


Un recurso de [búfer](introduction-to-buffers.md) se divide en mosaicos de 64KB, con algo de espacio vacío en el último mosaico si el tamaño no es un múltiplo de 64KB.

Los búferes estructurados no deben tener ninguna limitación en el intervalo para organizarse en mosaico. Pero las posibles optimizaciones de rendimiento en el hardware por usar [**StructuredBuffers**](https://msdn.microsoft.com/library/windows/desktop/ff471514) podrían verse invalidadas si se organizan en mosaico en primer lugar.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Cómo se organiza en mosaico el área de un recurso de streaming](how-a-streaming-resource-s-area-is-tiled.md)

 

 




