---
title: Espacio de direcciones disponible para recursos de streaming
description: "En esta sección se especifica el espacio de direcciones virtuales que está disponible para los recursos de streaming."
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords:
- Espacio de direcciones disponible para recursos de streaming
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c120e79518abfed35cbb6b0da6fc659b13602a3f
ms.lasthandoff: 02/07/2017

---

# <a name="address-space-available-for-streaming-resources"></a>Espacio de direcciones disponible para recursos de streaming


En esta sección se especifica el espacio de direcciones virtuales que está disponible para los recursos de streaming.

En sistemas operativos de 64 bits, hay disponibles al menos 40 bits del espacio de direcciones virtuales (1 terabyte).

Para los sistemas operativos de 32 bits, el espacio de direcciones es de 32 bits (4 GB). Para los sistemas ARM de 32 bits, puede que no sea posible crear los recursos de streaming si la asignación usara más de 27 bits del espacio de direcciones (128 MB). Esto incluye el relleno oculto en el espacio de direcciones que el hardware puede usar para los mapas MIP, el relleno en mosaicos y, posiblemente, las dimensiones de la superficie de relleno en potencias de 2.

En sistemas de gráficos con una tabla de páginas independiente para la unidad de procesamiento gráfico (GPU), será la aplicación la que ponga la mayor parte de este espacio de direcciones a disposición de los recursos de la GPU, aunque las asignaciones de GPU a cargo del controlador de pantalla caben en el mismo espacio.

En los sistemas futuros con una tabla de páginas compartida entre la CPU y la GPU, el espacio de direcciones disponible estará compartido entre las asignaciones de la CPU y la GPU en un proceso.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md)

 

 





