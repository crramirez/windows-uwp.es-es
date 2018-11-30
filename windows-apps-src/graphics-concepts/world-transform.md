---
title: Transformación de mundos
description: Una transformación de mundos cambia las coordenadas de un espacio de modelo, donde se definen los vértices con respecto al origen local de un modelo, a un espacio de mundo.
ms.assetid: 767B032C-69D0-4583-8FEB-247F4C41E31D
keywords:
- Transformación de mundos
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: af694ed32d845e518f4189f75309f1f371743f90
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8203262"
---
# <a name="world-transform"></a>Transformación de mundos


Una transformación de mundos cambia las coordenadas de un espacio de modelo, donde se definen los vértices con respecto al origen local de un modelo, a un espacio de mundo. En el espacio de mundo, se definen vértices en relación a un origen común a todos los objetos de una escena. La transformación de mundos coloca un modelo en el mundo.

## <span id="What_Is_a_World_Transform"></span><span id="what_is_a_world_transform"></span><span id="WHAT_IS_A_WORLD_TRANSFORM"></span>


El siguiente diagrama muestra la relación entre el sistema de coordenadas del mundo y el sistema de coordenadas local de un modelo.

![diagrama de cómo se relacionan las coordenadas del mundo y las coordenadas locales](images/worldcrd.png)

La transformación de mundos puede incluir cualquier combinación de traducciones, rotaciones y escalas.

## <a name="span-idsettingupaworldmatrixxmlspansetting-up-a-world-matrix"></a><span id="SETTING_UP_A_WORLD_MATRIX.XML"></span>Configurar una matriz del mundo


Al igual que con cualquier otra transformación, crea la transformación de mundos concatenando una serie de matrices en una única matriz que contiene la suma total de sus efectos. En el caso más simple, cuando un modelo está en el origen del mundo y sus ejes de coordenadas locales están orientados igual que el espacio de mundo, la matriz del mundo es la matriz de identidad. Normalmente, la matriz de mundo es una combinación de una traslación en el espacio de mundo y posiblemente una o más rotaciones para girar el modelo según sea necesario.

Direct3D usa las matrices de vista y mundo que estableces para configurar varias estructuras de datos internas. Cada vez que defines una nueva matriz de mundo o vista, el sistema vuelve a calcular las estructuras internas asociadas. Establecer estas matrices con frecuencia (por ejemplo, miles de veces por estructura) requiere mucho tiempo en términos de computación. Puedes minimizar el número de cálculos necesarios concatenando las matrices de vista y mundo en una matriz de vista-mundo que se establece como la matriz de mundo y estableciendo después la matriz de vista a la identidad. Mantén copias en caché de matrices de vista y mundo individuales para que se puede modificar, concatenar y restablecer la matriz de mundo, según sea necesario.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Temas relacionados


[Transformaciones](transforms.md)

 

 




