---
title: Formatos de galería de símbolos no admitidos con recursos de streaming
description: Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming.
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- Formatos de galerías de símbolos no compatibles con recursos de streaming
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: db6476afe265ea4a2556a5f787a14daa2e212e61
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74735140"
---
# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Formatos de galerías de símbolos no compatibles con recursos de streaming


Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming.

Los formatos que contienen la galería de símbolos incluyen DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (y formatos relacionados en la familia R24G8) y el formato de DXGI\_\_D32\_FLOAT\_S8X24\_UINT (y los formatos relacionados de la familia R32G8X24).

Algunas implementaciones almacenan la profundidad y la galería de símbolos en asignaciones independientes, mientras que otras las guardan juntas. La administración de los icono de los dos esquemas tendría que ser diferente, y ninguna API puede abstraer o racionalizar las diferencias. Nuestra recomendación para el hardware futuro es que admita superficies independientes de profundidad y de galería de símbolos, cada una con iconos independientes.

La profundidad de 32 bits tendría iconos de 128 x 128, y la galería de símbolos de 8 bits tendría iconos de 256 x 256. Por lo tanto, las aplicaciones tendrían que vivir con una falta de alineación entre la profundidad y la galería de símbolos en la forma de los iconos. Pero ya existe el mismo problema con los diferentes formatos de superficie de destino de representación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recurso compartido de recursos de streaming y uso compartido de dispositivos](streaming-resource-cross-process-and-device-sharing.md)

 

 




