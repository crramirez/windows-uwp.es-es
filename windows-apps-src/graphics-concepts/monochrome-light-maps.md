---
title: "Mapas de luz monocromática"
description: "Los mapas de luz monocromática permiten que los adaptadores antiguos realicen la combinación de texturas multipase, cuando una placa aceleradora 3D anterior no admite la combinación de texturas con el valor alfa del píxel de destino."
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords: "Mapas de luz monocromática"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 088e109cde92515305e474b2b03bd03526aaab87
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="monochrome-light-maps"></a>Mapas de luz monocromática


Los mapas de luz monocromática permiten que los adaptadores antiguos realicen la combinación de texturas multipase, cuando una placa aceleradora 3D anterior no admite la combinación de texturas con el valor alfa del píxel de destino.

Algunas placas aceleradoras 3D anteriores no admiten la combinación de texturas con el valor alfa del píxel de destino. Por lo general, estos adaptadores tampoco no admiten la combinación de varias texturas. Si la aplicación se ejecuta en un adaptador como este, puede usar una combinación de texturas multipase para realizar los mapas de luz monocromática.

Para realizar mapas de luz monocromática, una aplicación almacena la información de iluminación en los datos alfa de sus texturas de mapas de luz. La aplicación usa las capacidades de filtrado de texturas de Direct3D para asignar cada píxel de la imagen primitiva a un elemento de textura correspondiente en el mapa de luz. Establece el origen del factor de combinación en el valor alfa del elemento de textura correspondiente.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Mapas de luz con texturas](light-mapping-with-textures.md)

 

 



