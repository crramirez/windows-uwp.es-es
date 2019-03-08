---
Description: Al igual que las personas usan una combinación de voz y gestos para comunicarse entre sí, cuando interactúes con una aplicación pueden resultarte útiles varios tipos y modos de entrada.
title: Directrices de diseño de varias entradas
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c67430680854e7940d12af15ecd3c07dcd976802
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601300"
---
# <a name="multiple-inputs"></a>Varias entradas


Al igual que las personas usan una combinación de voz y gestos para comunicarse entre sí, cuando interactúes con una aplicación pueden resultarte útiles varios tipos y modos de entrada.


Para acoger al máximo número de usuarios y dispositivos posibles, te recomendamos que diseñes tus aplicaciones para que funcionen con el máximo posible de tipos de entrada (gestos, voz, entrada táctil, panel táctil, mouse y teclado). De este modo, se maximizarán la flexibilidad, la facilidad de uso y la accesibilidad.

Para comenzar, ten en cuenta los distintos escenarios en que la aplicación controla la entrada. Intenta ser coherente en toda la aplicación y recuerda que los controles de plataforma proporcionan compatibilidad integrada para varios tipos de entrada.

-   ¿Los usuarios pueden interactuar con la aplicación a través de varios dispositivos de entrada?
-   ¿Se admiten todos los métodos de entrada en todo momento? ¿Con algunos controles? ¿En determinados momentos o circunstancias?
-   ¿Tiene prioridad un método de entrada?

## <a name="single-or-exclusive-mode-interactions"></a>Interacciones de modo único (o exclusivo)


Con las interacciones de modo único se admiten varios tipos de entrada, pero solo se puede usar una por acción. Por ejemplo, reconocimiento de voz para los comandos y gestos para la navegación; o bien, escribir texto con entrada táctil o con gestos, según la proximidad.

## <a name="multimodal-interactions"></a>Interacciones multimodales

Con las interacciones multimodales, se usan varios métodos de entrada en una secuencia para completar una sola acción.

Voz + gestos  
El usuario apunta a un producto y, a continuación, dice "Agregar al carro".

Voz + función táctil  
El usuario selecciona una foto manteniéndola presionada y, a continuación, dice "Enviar foto".



