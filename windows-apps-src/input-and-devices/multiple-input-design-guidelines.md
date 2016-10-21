---
author: Karl-Bridge-Microsoft
Description: "Al igual que las personas usan una combinación de voz y gestos para comunicarse entre sí, cuando interactúes con una aplicación pueden resultarte útiles varios tipos y modos de entrada."
title: "Directrices de diseño de varias entradas"
ms.assetid: 03EB5388-080F-467C-B272-C92BE00F2C69
label: Multiple inputs
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: a2ec5e64b91c9d0e401c48902a18e5496fc987ab
ms.openlocfilehash: d4238e0d1becee719148b38acf5a91d74230601d

---

# Varias entradas

Al igual que las personas usan una combinación de voz y gestos para comunicarse entre sí, cuando interactúes con una aplicación pueden resultarte útiles varios tipos y modos de entrada.


Para acoger al máximo número de usuarios y dispositivos posibles, te recomendamos que diseñes tus aplicaciones para que funcionen con el máximo posible de tipos de entrada (gestos, voz, entrada táctil, panel táctil, mouse y teclado). De este modo, se maximizarán la flexibilidad, la facilidad de uso y la accesibilidad.

Para comenzar, ten en cuenta los distintos escenarios en que la aplicación controla la entrada. Intenta ser coherente en toda la aplicación y recuerda que los controles de plataforma proporcionan compatibilidad integrada para varios tipos de entrada.

-   ¿Los usuarios pueden interactuar con la aplicación a través de varios dispositivos de entrada?
-   ¿Se admiten todos los métodos de entrada en todo momento? ¿Con algunos controles? ¿En determinados momentos o circunstancias?
-   ¿Tiene prioridad un método de entrada?

## Interacciones de modo único (o exclusivo)


Con las interacciones de modo único se admiten varios tipos de entrada, pero solo se puede usar una por acción. Por ejemplo, reconocimiento de voz para los comandos y gestos para la navegación; o bien, escribir texto con entrada táctil o con gestos, según la proximidad.

## Interacciones multimodales


Con las interacciones multimodales, se usan varios métodos de entrada en una secuencia para completar una sola acción.

Voz + gestos  
El usuario apunta a un producto y, a continuación, dice "Agregar al carro".

Voz + función táctil  
El usuario selecciona una foto manteniéndola presionada y, a continuación, dice "Enviar foto".






<!--HONumber=Aug16_HO3-->


