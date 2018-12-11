---
title: Formatos de galerías de símbolos no compatibles con recursos de streaming
description: Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Formatos de galerías de símbolos no compatibles con recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1d35813a6242abd555e87329c25a413285d1d948
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8893263"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Formatos de galerías de símbolos no compatibles con recursos de streaming


Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming.

Los formatos que contienen la galería de símbolos incluyen DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (y formatos relacionados de la familia de R24G8) y DXGI\_FORMAT\_D32\_FLOAT\_S8X24\_UINT (y formatos relacionados de la familia de R32G8X24).

Algunas implementaciones almacenan la profundidad y la galería de símbolos en asignaciones independientes, mientras que otras las guardan juntas. La administración de los icono de los dos esquemas tendría que ser diferente, y ninguna API puede abstraer o racionalizar las diferencias. Nuestra recomendación para el hardware futuro es que admita superficies independientes de profundidad y de galería de símbolos, cada una con iconos independientes.

La profundidad de 32bits tendría iconos de 128 x 128, y la galería de símbolos de 8bits tendría iconos de 256 x 256. Por lo tanto, las aplicaciones tendrían que vivir con una falta de alineación entre la profundidad y la galería de símbolos en la forma de los iconos. Pero ya existe el mismo problema con los diferentes formatos de superficie de destino de representación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Uso compartido de dispositivos y entre procesos de recursos de streaming](streaming-resource-cross-process-and-device-sharing.md)

 

 




