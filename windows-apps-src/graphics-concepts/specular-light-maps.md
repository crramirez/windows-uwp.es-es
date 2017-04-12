---
title: Mapas de luces especulares
description: Cuando se iluminan por una fuente de luz, los objetos brillantes que usen materiales muy reflectantes reciben resaltados especulares.
ms.assetid: 9B3AC5EC-DDAA-4671-BC81-0E3C79D87A81
keywords: Mapas de luces especulares
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 4e2037bff175484aab10d8c650647fd26ed5e6fd
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="specular-light-maps"></a>Mapas de luces especulares


Cuando se iluminan por una fuente de luz, los objetos brillantes que usen materiales muy reflectantes reciben resaltados especulares. En ocasiones, es posible obtener resaltados más precisos al aplicar mapas de luces especulares primitivos, en lugar de usar los resaltados especulares creados por el módulo de iluminación.

Para realizar la asignación de luz especular, agrega el mapa de luz especular a la textura del primitivo y, a continuación, modula (multiplicando el resultado) el mapa de luz RGB.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Mapas de luz con texturas](light-mapping-with-textures.md)

 

 




