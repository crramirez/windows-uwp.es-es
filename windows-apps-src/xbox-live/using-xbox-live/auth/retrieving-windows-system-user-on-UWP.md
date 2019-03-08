---
title: Recuperación de usuario del sistema de Windows en UWP
description: Obtenga información sobre cómo recuperar el usuario del sistema de Windows en un juego de plataforma Universal de Windows (UWP).
ms.date: 06/07/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, el usuario del sistema
ms.localizationpriority: medium
ms.openlocfilehash: c46f7e98c2dea3b23beb2cec80816067d4c4e341
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632760"
---
# <a name="retrieving-the-windows-system-user-in-a-universal-windows-platform-uwp-title"></a>Recuperar el usuario del sistema de Windows en un título de la plataforma Universal de Windows (UWP)

## <a name="windowssystemuser"></a>Windows.System.User

Un [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) objeto representa un usuario de Windows local. En la consola Xbox, permite que varios usuario de windows iniciar sesión al mismo tiempo en una única sesión interactiva, si la aplicación es una aplicación de varios usuario, entonces se necesitaría un objeto Windows.System.User para construir un XboxLiveUser con el fin de obtener acceso a servicios de live. En otras plataformas de windows como PC o teléfono, lo ha sólo permitir que a un usuario de windows en un dispositivo o una sesión interactiva, no es necesario pasar un Windows.System.User para construir un XboxLiveUser.

## <a name="ways-to-retrieve-windows-system-user"></a>Maneras de recuperar el usuario del sistema de Windows

* **Uso de métodos estáticos de [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) clase.**

  [Windows.System.User](https://docs.microsoft.com/en-us/uwp/api/windows.system.user) clase proporciona un conjunto de método estático para ayudar a recuperar objetos Windows.System.User. Por ejemplo puede llamar a FindAllAsync para obtener todos los usuarios de ventanas activas.

* **[UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker)**

  [Windows.System.UserPicker](https://docs.microsoft.com/en-us/uwp/api/windows.system.userpicker) clase proporciona métodos para iniciar el selector de usuario la interfaz de usuario y seleccione un usuario de sistema de windows en escenarios de varios usuarios.

* **[IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller)**

  [Windows.Gaming.Input.IGameController](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.igamecontroller) es la interfaz principal para todos los dispositivos del controlador (gamepad, ruedas de carreras, stick flight etcetera). Puede obtener el objeto Windows.System.User asociado con el dispositivo de juego mediante una llamada a su propiedad de usuario.  

  Presentamos el dispositivo de juego par implementada por windows [ArcadeStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.arcadestick), [FlightStick](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.flightstick), [Gamepad](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.gamepad), [RacingWheel](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.input.racingwheel).

* **[UserDeviceAssociation](https://docs.microsoft.com/en-us/uwp/api/windows.system.userdeviceassociation)**

  Puede llamar al método estático FindUserFromDeviceId para encontrar el usuario asociado con el identificador de dispositivo. Normalmente, puede obtener el identificador de dispositivo de los eventos de entrada de windows, como [Windows.UI.Xaml.Input.KeyRoutedEventArgs](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs), [Windows.UI.Core.KeyEventArgs](https://docs.microsoft.com/en-us/uwp/api/windows.ui.core.keyeventargs)
