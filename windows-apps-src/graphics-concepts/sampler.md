---
title: Muestra
description: El muestreo es el proceso de leer los valores de entrada de una textura u otros recursos. Una \ 0034; muestra \ 0034; es cualquier objeto que se lee de recursos.
ms.assetid: 7ECE13BB-9FC5-44A3-B1B2-2FE163F1D627
keywords:
- Muestra
author: michaelfromredmond
ms.author: mithom
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ae15e41ec1d6493f33a776c11d74e28b2c95dc34
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6030918"
---
# <a name="sampler"></a>Muestra


El muestreo es el proceso de leer los valores de entrada de una textura u otros recursos. Una " muestra" es cualquier objeto que se lee de recursos.

Al realizar el muestreo de una textura y representarla en un área de la pantalla se pueden producir muchos problemas y artefactos. Por ejemplo, si el área en la que se quiere representar es de 50 x 50 píxeles y la textura es 16 por 16 píxeles o de 256 por 256 píxeles, es necesario estirar o reducir la textura de forma considerable. Dado que esta diferencia en el tamaño produce artefactos no deseados, se usan técnicas de filtrado para mitigarlos. Un método habitual de filtrado para usar texturas pequeñas para áreas de representación más grandes es el filtrado "bilineal".

Otro problema se produce cuando el área en la que se va a representar está en un ángulo muy oblicuo respecto a la vista (por ejemplo, una textura 256 por 256 que se representa en un área de 100 píxeles de ancho pero de solo 5 píxeles de profundidad debido al ángulo de visión). En este caso, a menudo se aplica el filtrado "anisotrópico". El filtrado anisotrópico proporciona una mejor calidad de imagen que el filtro bilineal, ya que elimina los efectos de dientes de sierra ("aliasing") sin un desenfoque excesivo, pero es consume más recursos del equipo.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Vistas](views.md)

 

 




