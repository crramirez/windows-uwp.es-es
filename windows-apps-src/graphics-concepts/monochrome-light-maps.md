---
title: Mapas de luz monocromática
description: Los mapas de luz monocromática permiten que los adaptadores antiguos realicen la combinación de texturas multipase, cuando una placa aceleradora 3D anterior no admite la combinación de texturas con el valor alfa del píxel de destino.
ms.assetid: 60F8F8F6-9DB7-452B-8DC0-407FFAA4BFE1
keywords:
- Mapas de luz monocromática
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b81838393d7b2692e6fd04b7ce535f58dc773780
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641270"
---
# <a name="monochrome-light-maps"></a>Mapas de luz monocromática


Los mapas de luz monocromática permiten que los adaptadores antiguos realicen la combinación de texturas multipase, cuando una placa aceleradora 3D anterior no admite la combinación de texturas con el valor alfa del píxel de destino.

Algunas placas aceleradoras 3D anteriores no admiten la combinación de texturas con el valor alfa del píxel de destino. Por lo general, estos adaptadores tampoco no admiten la combinación de varias texturas. Si la aplicación se ejecuta en un adaptador como este, puede usar una combinación de texturas multipase para realizar los mapas de luz monocromática.

Para realizar mapas de luz monocromática, una aplicación almacena la información de iluminación en los datos alfa de sus texturas de mapas de luz. La aplicación usa las capacidades de filtrado de texturas de Direct3D para asignar cada píxel de la imagen primitiva a un elemento de textura correspondiente en el mapa de luz. Establece el origen del factor de combinación en el valor alfa del elemento de textura correspondiente.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Asignación clara con texturas](light-mapping-with-textures.md)

 

 




