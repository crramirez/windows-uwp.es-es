---
title: Espacio de direcciones disponible para recursos de streaming
description: "En esta sección se especifica el espacio de direcciones virtuales que está disponible para los recursos de streaming."
ms.assetid: 145EB4A3-3910-4126-BC7E-A4CF53E2A098
keywords: Espacio de direcciones disponible para recursos de streaming
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: a63136f04570c4bf964c461f7296c930f5e168b5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="address-space-available-for-streaming-resources"></a>Espacio de direcciones disponible para recursos de streaming


En esta sección se especifica el espacio de direcciones virtuales que está disponible para los recursos de streaming.

En sistemas operativos de 64bits, hay disponibles al menos 40bits del espacio de direcciones virtuales (1terabyte).

Para los sistemas operativos de 32bits, el espacio de direcciones es de 32bits (4GB). Para los sistemas ARM de 32bits, puede que no sea posible crear los recursos de streaming si la asignación usara más de 27bits del espacio de direcciones (128MB). Esto incluye el relleno oculto en el espacio de direcciones que el hardware puede usar para los mapas MIP, el relleno en mosaicos y, posiblemente, las dimensiones de la superficie de relleno en potencias de 2.

En sistemas de gráficos con una tabla de páginas independiente para la unidad de procesamiento gráfico (GPU), será la aplicación la que ponga la mayor parte de este espacio de direcciones a disposición de los recursos de la GPU, aunque las asignaciones de GPU a cargo del controlador de pantalla caben en el mismo espacio.

En los sistemas futuros con una tabla de páginas compartida entre la CPU y la GPU, el espacio de direcciones disponible estará compartido entre las asignaciones de la CPU y la GPU en un proceso.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Parámetros de creación de recursos de streaming](streaming-resource-creation-parameters.md)

 

 



