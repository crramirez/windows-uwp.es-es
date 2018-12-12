---
title: Formatos de texturas comprimidos
description: Esta sección contiene información sobre la organización interna de formatos de texturas comprimidos.
ms.assetid: 24D17B9F-8CA7-4006-9E0F-178C6B3CAEC9
keywords:
- Formatos de texturas comprimidos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3171eba376911157a6ad2687fe3879df751615ac
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938947"
---
# <a name="compressed-texture-formats"></a>Formatos de texturas comprimidos


Esta sección contiene información sobre la organización interna de formatos de texturas comprimidos. No necesitas estos detalles para usar texturas comprimidas, porque puedes usar las funciones de Direct3D para la conversión desde y hacia los formatos comprimidos. Sin embargo, esta información resulta útil si quieres trabajar directamente sobre los datos de superficies comprimidos.

Direct3D usa un formato de compresión que divide los mapas de texturas en bloques de elementos de textura de 4×4. Si la textura no contiene ninguna transparencia —es opaca— o si la transparencia se especifica con un alfa de 1bit, un bloque de 8bytes representa el bloque del mapa de texturas. Si el mapa de texturas contiene elementos de textura transparentes, usando un canal alfa, un bloque de 16bytes lo representa.

Cada textura única debe especificar que sus datos se almacenan como 64 o 128bits por grupo de 16elementos de textura. Si para la textura se usan bloques de 64bits —es decir, el formato BC1—, es posible combinar los formatos opaco y de alfa de 1bit por cada bloque dentro de la misma textura. En otras palabras, la comparación de la magnitud de entero sin signo de color\_0 y color\_1 se realiza de forma exclusiva para cada bloque de 16elementos de textura.

Cuando se usan bloques de 128bits, el canal alfa debe especificarse de modo explícito (formato BC2) o interpolado (formato BC3) para la textura entera. Al igual que con el color, cuando se selecciona el modo interpolado, pueden usarse ocho alfas interpolados o seis alfas interpolados en cada bloque. Nuevamente, la comparación de la magnitud de alpha\_0 y alpha\_1 se realiza exclusivamente para cada bloque.

El pitch para los formatos BCn se mide en bytes (no en bloques). Por ejemplo, si tienes un ancho de 16, tendrás un pitch de cuatro bloques (4\*8 para BC1, 4\*16 para BC2 o BC3).

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Recursos de texturas comprimidas](compressed-texture-resources.md)

 

 




