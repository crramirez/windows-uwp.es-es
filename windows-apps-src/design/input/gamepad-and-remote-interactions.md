---
author: Karl-Bridge-Microsoft
Description: Optimize your app for input from Xbox gamepad and remote control.
title: Interacciones con el controlador para juegos y el control remoto
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 022724064ad1e7f5551b6676bf256ca5cf6e4b8e
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9048562"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interacciones con el controlador para juegos y el control remoto

![Imagen del teclado y el controlador para juegos](images/keyboard/keyboard-gamepad.jpg)

***Patrones comunes de la interacción se comparten entre el controlador para juegos, control remoto y el teclado***

Asegurarse de que la aplicación funcione bien tanto con el controlador para juegos como con el control remoto es el paso más importante en la optimización de experiencias de 10 pies. Hay varias mejoras específicas para los controladores para juegos y los controles remotos que puedes hacer para optimizar la interacción con el usuario en un dispositivo en el que sus acciones son un poco limitadas.

Las aplicaciones de la Plataforma universal de Windows (UWP) ahora admiten la entrada del controlador para juegos y el control remoto. 

Los controladores para juegos y los controles remotos son los dispositivos de entrada principales para las experiencias de Xbox y TV. 

Las aplicaciones para UWP deberían optimizarse para estos tipos de dispositivos de entrada, al igual que lo están para la entrada de teclado y mouse en un equipo y la entrada táctil en un teléfono o tableta. 

El hecho de asegurarte de que la aplicación funciona correctamente con estos dispositivos de entrada es el paso más importante a la hora de optimizar para Xbox y la TV.

Ahora puedes enchufar y usar el controlador para juegos con aplicaciones para UWP en el equipo, lo que permite validar el trabajo más fácilmente.

Para garantizar una experiencia de usuario correcta y divertida para tu aplicación para UWP al usar un controlador para juegos o el control remoto, ten en cuenta lo siguiente:

* [Botones de hardware](../devices/designing-for-tv.md#hardware-buttons): el controlador para juegos y el control remoto proporcionan configuraciones y botones muy diferentes.

* [Interacción y navegación con foco XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction): la navegación con foco XY permite al usuario navegar por la interfaz de usuario de la aplicación.

* [Modo de mouse](../devices/designing-for-tv.md#mouse-mode): el modo de mouse permite a tu aplicación emular una experiencia de mouse cuando la navegación con foco XY no es suficiente.
