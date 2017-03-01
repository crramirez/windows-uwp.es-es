---
title: "Formatos de galerías de símbolos no compatibles con recursos de streaming"
description: "Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming."
ms.assetid: 90A572A4-3C76-4795-BAE9-FCC72B5F07AD
keywords:
- "Formatos de galerías de símbolos no compatibles con recursos de streaming"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 96eb1ec610b5aba743caf92584f52f5785046155
ms.lasthandoff: 02/07/2017

---

# <a name="stencil-formats-not-supported-with-streaming-resources"></a>Formatos de galerías de símbolos no compatibles con recursos de streaming


Los formatos que contienen la galería de símbolos no son compatibles con los recursos de streaming.

Los formatos que contienen la galería de símbolos incluyen DXGI\_FORMAT\_D24\_UNORM\_S8\_UINT (y formatos relacionados de la familia de R24G8) y DXGI\_FORMAT\_D32\_FLOAT\_S8X24\_UINT (y formatos relacionados de la familia de R32G8X24).

Algunas implementaciones almacenan la profundidad y la galería de símbolos en asignaciones independientes, mientras que otras las guardan juntas. La administración de los icono de los dos esquemas tendría que ser diferente, y ninguna API puede abstraer o racionalizar las diferencias. Nuestra recomendación para el hardware futuro es que admita superficies independientes de profundidad y de galería de símbolos, cada una con iconos independientes.

La profundidad de 32 bits tendría iconos de 128 x 128, y la galería de símbolos de 8 bits tendría iconos de 256 x 256. Por lo tanto, las aplicaciones tendrían que vivir con una falta de alineación entre la profundidad y la galería de símbolos en la forma de los iconos. Pero ya existe el mismo problema con los diferentes formatos de superficie de destino de representación.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Uso compartido de dispositivos y entre procesos de recursos de streaming](streaming-resource-cross-process-and-device-sharing.md)

 

 





