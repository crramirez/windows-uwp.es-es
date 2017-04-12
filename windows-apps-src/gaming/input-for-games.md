---
author: mithom
title: Entrada para juegos
description: "En esta sección se muestra cómo trabajar con controladores para juegos y otros dispositivos de entrada para la Plataforma universal de Windows (UWP)."
ms.assetid: 2DD0B384-8776-4599-9E52-4FC0AA682735
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP, juegos, entrada, games, input
ms.openlocfilehash: ee6017648974d5283f59708550092f26d88388ce
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="input-for-games"></a>Entrada para juegos

En esta sección se describen los diferentes tipos de dispositivos de entrada que pueden usarse en juegos para la Plataforma universal de Windows (UWP) en Windows 10 y Xbox One, se muestra su uso básico y se recomiendan patrones y técnicas para la programación de entrada eficaz para juegos.

> **Nota**    Existen otros tipos de dispositivos de entrada y están disponibles para usarse en juegos para UWP, como dispositivos de entrada personalizados que pueden ser específicos del juego o género. Dichos dispositivos y su programación no se analizan en esta sección. Para obtener información sobre las interfaces usadas para facilitar los dispositivos de entrada personalizados, consulta el espacio de nombres [Windows.Gaming.Input.Custom][].

## <a name="gaming-input-devices"></a>Dispositivos de entrada de juegos

Los dispositivos de entrada de juegos son compatibles con aplicaciones y juegos para UWP para Windows 10 y Xbox One en el espacio de nombres [Windows.Gaming.Input][].

### <a name="gamepads"></a>Controladores para juegos

Los controladores para juegos son el dispositivo de entrada estándar en Xbox One y una opción habitual para los jugadores de Windows que no prefieran el teclado y el mouse. Proporcionan una variedad de controles digitales y analógicos que les hacen adecuados para casi cualquier tipo de juego y también ofrecen retroalimentación táctil a través de motores de vibración integrados.

Para obtener información sobre cómo usar los controles para juegos en tu juego para UWP, consulta [Controlador para juegos y vibración](gamepad-and-vibration.md).

### <a name="arcade-sticks"></a>Sticks arcade

Los sticks arcade son dispositivos de entrada totalmente digital apreciados por reproducir la sensación de las máquinas arcade de pie y son el dispositivo de entrada perfecto para el combate cara a cara u otros juegos de estilo arcade.

Para obtener información sobre cómo usar los sticks arcade en tu juego para UWP, consulta [Stick arcade](arcade-stick.md).

### <a name="racing-wheels"></a>Volantes

Los volantes son dispositivos de entrada que se asemejan mucho a una cabina carreras real y el dispositivo de entrada perfecto para cualquier juego de carreras que presente coches o camiones. Muchos de los volantes están equipados con retroalimentación de fuerza real; es decir, pueden aplicar fuerzas reales en el eje de control como el volante, no tan solo vibración.

Para obtener información sobre cómo usar los volantes en tu juego para UWP, consulta [Racing Wheel and force feedback (Volante y retroalimentación de fuerza)](racing-wheel-and-force-feedback.md).

### <a name="ui-navigation-controller"></a>Controlador de navegación de la interfaz de usuario

Los controladores de navegación de la interfaz de usuario son dispositivos de entrada lógica que sirven para proporcionar un vocabulario común para los comandos de navegación de la interfaz de usuario que favorece una experiencia de usuario coherente entre los distintos juegos y dispositivos de entrada físicos. La interfaz de usuario de un juego debe usar las interfaces UINavigationController en lugar de las interfaces específicas de dispositivo.

Para obtener información sobre cómo usar controladores de navegación de la interfaz de usuario en tu juego para UWP, consulta [Controlador de navegación de la interfaz de usuario](ui-navigation-controller.md).

### <a name="headsets"></a>Auriculares

Los auriculares son dispositivos de captura y reproducción de audio que se asocian con un usuario específico cuando se conecta a través de su dispositivo de entrada. Se usan habitualmente para el chat de voz de los juegos en línea, pero también pueden usarse para mejorar la inmersión o proporcionar características de juego tanto en los juegos en línea como sin conexión.

Para obtener información sobre cómo usar los auriculares en tu juego para UWP, consulta [Auriculares](headset.md)

### <a name="users"></a>Usuarios

Cada dispositivo de entrada y sus auriculares conectados pueden asociarse con un usuario específico para vincular su identidad al juego. La identidad del usuario también es el medio por el cual la entrada desde un dispositivo de entrada físico se correlaciona con la de su controlador de navegación de la interfaz de usuario lógico.

Para obtener información sobre cómo administrar los usuarios y los dispositivos de entrada, consulta [Tracking users and their devices (Seguimiento de usuarios y sus dispositivos)](input-practices-for-games.md#tracking-users-and-their-devices).

## <a name="see-also"></a>Consulta también
[Windows.Gaming.Input.Custom][]


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.Custom]: https://msdn.microsoft.com/en-us/library/windows/apps/windows.gaming.input.custom.aspx
