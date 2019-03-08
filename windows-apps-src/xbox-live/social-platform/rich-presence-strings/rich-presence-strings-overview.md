---
title: Presencia avanzada
description: Obtenga información sobre cómo puede ayudar a promover el título de la presencia de Xbox Live enriquecido.
ms.assetid: 00042359-f877-4b26-9067-58834590b1dd
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, presencia enriquecida
ms.localizationpriority: medium
ms.openlocfilehash: 0ae0d0189954ed8fa9a0bc7651d6a90b9789c388
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604120"
---
# <a name="rich-presence"></a>Presencia avanzada

Mediante el uso de presencia enriquecida, su juego puede anunciar lo que está haciendo un reproductor ahora mismo. Por ejemplo, su juego puede usar cadenas de presencia enriquecida para mostrar todos los jugadores, como el estado de los reproductores de tu juego, *ausente*. Información de presencia enriquecida es visible para jugadores conectados a Xbox Live. Lo ideal es que, una cadena de presencia enriquecida indica a otros jugadores de Xbox Live lo que está haciendo un reproductor y donde en el juego el Reproductor es hacerlo. El concepto de cadenas de presencia enriquecida es la misma en Xbox One como era en Xbox 360, pero la nueva implementación sigue el entretenimiento como una iniciativa de servicio. Los temas de esta sección describen cómo configurar las cadenas de presencia enriquecida y, a continuación, cómo establecer la cadena de un usuario reproducir el título.


## <a name="definitions"></a>Definiciones

**Enumeraciones**  
Una enumeración es una lista de algunas dimensiones en el juego. Ejemplos de estas dimensiones en los juegos son armas, clases de caracteres, mapas y así sucesivamente. Queremos ver una lista de la armas posible en el juego, una lista de todas las clases de caracteres posible o mapas y así sucesivamente.

**Par de la cadena de configuración regional**  
Cada cadena de presencia enriquecida posible debe tener una configuración regional asociada para especificar en qué configuraciones regionales se puede o debe usar la cadena. Cada enumeración también tendrá un conjunto de pares de cadena de configuración regional también.

**String-set**  
Un conjunto de cadena se compone de un grupo de pares de cadena de configuración regional. Este conjunto define los valores posibles de una cadena de presencia enriquecida para todas las configuraciones regionales posibles o los valores posibles para una enumeración de todas las configuraciones regionales posibles.

**Nombres descriptivos**  
Hay dos tipos de nombres descriptivos:

**Cadena de presencia enriquecida**  
El nombre descriptivo para un conjunto de cadena es un identificador único en forma de cadena utilizada para hacer referencia a un conjunto de cadena.

**Enumeración**  
Estos nombres descriptivos se usan para identificar de forma única una enumeración, como la enumeración de armas o la enumeración de la clase de caracteres determinada.


## <a name="in-this-section"></a>En esta sección

[Configuración de presencia enriquecida](rich-presence-strings-configuration.md)  
Cómo configurar presencia enriquecida para su uso en su título.

[Actualizando cadenas de presencia de enriquecido](rich-presence-strings-updating-strings.md)  
Cómo actualizar las cadenas de presencia enriquecida de su título.

[Prácticas recomendadas de presencia enriquecida](rich-presence-strings-best-practices.md)  
Procedimientos recomendados para el uso de presencia enriquecido en su título.

[Limitaciones y las directivas de presencia enriquecida](rich-presence-strings-policies-and-limitations.md)  
Las directivas sobre el uso de presencia enriquecido en su título.

[Apéndice de presencia enriquecida](rich-presence-strings-appendix.md)  
Ejemplos adicionales y detalles sobre la plataforma de datos relacionados con presencia enriquecida.

[Programación de Xbox Live presencia enriquecida](programming-rich-presence.md)  
Muestra cómo utilizar la presencia de enriquecido con Xbox Live.
