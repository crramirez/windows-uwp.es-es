---
author: mijacobs
Description: TODO
title: Interacciones con el controlador para juegos y el control remoto
ms.assetid: 784a08dc-2736-4bd3-bea0-08da16b1bd47
label: Gamepad and remote interactions
template: detail.hbs
isNew: true
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da1248937d8f7d1a5a1da27e376690cde2ac7ef6
ms.sourcegitcommit: 517c83baffd344d4c705bc644d7c6d2b1a4c7e1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2018
ms.locfileid: "1842855"
---
# <a name="gamepad-and-remote-control-interactions"></a>Interacciones con el controlador para juegos y el control remoto

![Control remoto y pad-D](images/dpad-remote/dpad-remote.png)

Las aplicaciones de la Plataforma universal de Windows (UWP) ahora admiten la entrada del controlador para juegos y control remoto, que son los dispositivos de entrada principales para experiencias de Xbox y televisión.

Las aplicaciones para UWP deberían optimizarse para estos tipos de dispositivos de entrada, al igual que lo están para la entrada de teclado y mouse en un equipo y la entrada táctil en un teléfono o tableta.

El hecho de asegurarte de que la aplicación funciona correctamente con estos dispositivos de entrada es el paso fundamental a la hora de optimizar para Xbox y la TV.

> [!NOTE] 
> Ahora puedes enchufar y usar el controlador para juegos con aplicaciones para UWP en el equipo, lo que permite validar el trabajo más fácilmente.

Para garantizar una experiencia de usuario correcta y divertida para tu aplicación para UWP al usar un controlador para juegos o el control remoto, ten en cuenta lo siguiente:

* [Botones de hardware](../devices/designing-for-tv.md#hardware-buttons): el controlador para juegos y el control remoto proporcionan configuraciones y botones muy diferentes.

* [Interacción y navegación con foco XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction): la navegación con foco XY permite al usuario navegar por la interfaz de usuario de la aplicación.

* [Modo de mouse](../devices/designing-for-tv.md#mouse-mode): el modo de mouse permite a tu aplicación emular una experiencia de mouse cuando la navegación con foco XY no es suficiente.
