---
author: mithom
title: Entrada para juegos
description: En esta sección se muestra cómo trabajar con controladores para juegos y otros dispositivos de entrada para la Plataforma universal de Windows (UWP).
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, entrada, games, input
ms.localizationpriority: medium
ms.openlocfilehash: bb7d70c20aeb2b91d8a6db863e165e017810e924
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "6986149"
---
# <a name="input-for-games"></a>Entrada para juegos

En esta sección se describen los diferentes tipos de dispositivos de entrada que pueden usarse en juegos para la Plataforma universal de Windows (UWP) en Windows 10 y Xbox One, se muestra su uso básico y se recomiendan patrones y técnicas para la programación de entrada eficaz para juegos.

> **Nota**    Existen otros tipos de dispositivos de entrada y están disponibles para usarse en juegos para UWP, como dispositivos de entrada personalizados que pueden ser específicos del juego o género. Dichos dispositivos y su programación no se analizan en esta sección. Para obtener información sobre las interfaces usadas para facilitar los dispositivos de entrada personalizados, consulta el espacio de nombres [Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom).

## <a name="gaming-input-devices"></a>Dispositivos de entrada de juegos

Los dispositivos de entrada de juegos son compatibles con aplicaciones y juegos para UWP para Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input).

### <a name="gamepads"></a>Controladores para juegos

Los controladores para juegos son el dispositivo de entrada estándar en Xbox One y una opción habitual para los jugadores de Windows que no prefieran el teclado y el mouse. Proporcionan una variedad de controles digitales y analógicos que les hacen adecuados para casi cualquier tipo de juego y también ofrecen retroalimentación táctil a través de motores de vibración integrados.

Para obtener información sobre cómo usar los controles para juegos en tu juego para UWP, consulta [Controlador para juegos y vibración](gamepad-and-vibration.md).

### <a name="arcade-sticks"></a>Sticks arcade

Los sticks arcade son dispositivos de entrada totalmente digital apreciados por reproducir la sensación de las máquinas arcade de pie y son el dispositivo de entrada perfecto para el combate cara a cara u otros juegos de estilo arcade.

Para obtener información sobre cómo usar los sticks arcade en tu juego para UWP, consulta [Stick arcade](arcade-stick.md).

### <a name="racing-wheels"></a>Volantes

Los volantes son dispositivos de entrada que se asemejan mucho a una cabina carreras real y el dispositivo de entrada perfecto para cualquier juego de carreras que presente coches o camiones. Muchos de los volantes están equipados con retroalimentación de fuerza real; es decir, pueden aplicar fuerzas reales en el eje de control como el volante, no tan solo vibración.

Para obtener información sobre cómo usar los volantes en tu juego para UWP, consulta [Racing Wheel and force feedback (Volante y retroalimentación de fuerza)](racing-wheel-and-force-feedback.md).

### <a name="flight-sticks"></a>Palancas de mandos

Las palancas de mandos son dispositivos de entrada de juegos que reproducir la sensación de las palancas de mandos que se encontrarían en un avión o espacial cabina de nave. Son el dispositivo de entrada perfecto para un control de vuelo rápido y preciso.

Para obtener más información sobre cómo usar las palancas de mandos en tu juego para UWP, consulta la [palanca de mandos](flight-stick.md).

### <a name="raw-game-controllers"></a>Dispositivos de juego sin procesar

Un dispositivo de juego sin procesar es una representación genérica de un dispositivo de juego, con entradas que se encuentran en muchos tipos diferentes de dispositivos de juego comunes. Estas entradas se exponen como matrices simples de botones, modificadores y ejes sin nombre. Con un dispositivo de juego sin procesar, puedes permitir a los clientes crear asignaciones de entrada personalizadas independientemente de qué tipo de controlador estén usando.

Para obtener más información sobre cómo usar los dispositivos de juego sin procesar en tu juego para UWP, consulta el [dispositivo de juego sin procesar](raw-game-controller.md).

### <a name="ui-navigation-controllers"></a>Controladores de navegación de la interfaz de usuario

Los controladores de navegación de la interfaz de usuario son dispositivos de entrada lógica que sirven para proporcionar un vocabulario común para los comandos de navegación de la interfaz de usuario que favorece una experiencia de usuario coherente entre los distintos juegos y dispositivos de entrada físicos. La interfaz de usuario de un juego debe usar las interfaces UINavigationController en lugar de las interfaces específicas de dispositivo.

Para obtener información sobre cómo usar controladores de navegación de la interfaz de usuario en tu juego para UWP, consulta [Controlador de navegación de la interfaz de usuario](ui-navigation-controller.md).

### <a name="headsets"></a>Auriculares

Los auriculares son dispositivos de captura y reproducción de audio que se asocian con un usuario específico cuando se conecta a través de su dispositivo de entrada. Se usan habitualmente para el chat de voz de los juegos en línea, pero también pueden usarse para mejorar la inmersión o proporcionar características de juego tanto en los juegos en línea como sin conexión.

Para obtener información sobre cómo usar los auriculares en tu juego para UWP, consulta [Auriculares](headset.md)

### <a name="users"></a>Usuarios

Cada dispositivo de entrada y sus auriculares conectados pueden asociarse con un usuario específico para vincular su identidad al juego. La identidad del usuario también es el medio por el cual la entrada desde un dispositivo de entrada físico se correlaciona con la de su controlador de navegación de la interfaz de usuario lógico.

Para obtener información sobre cómo administrar los usuarios y los dispositivos de entrada, consulta [Tracking users and their devices (Seguimiento de usuarios y sus dispositivos)](input-practices-for-games.md#tracking-users-and-their-devices).

## <a name="see-also"></a>Consulta también

* [Prácticas de entrada para juegos](input-practices-for-games.md)
* [Espacio de nombres Windows.Gaming.Input](https://docs.microsoft.com/uwp/api/windows.gaming.input)
* [Espacio de nombres Windows.Gaming.Input.Custom](https://docs.microsoft.com/uwp/api/windows.gaming.input.custom)