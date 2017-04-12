---
author: mithom
title: Auriculares
description: Usa las API de auriculares Windows.Gaming.Input para detectar los auriculares, capturar la voz del jugador y reproducir audio.
ms.assetid: 021CCA26-D339-4C8B-B084-0D499BD83ABE
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, juegos, auriculares
ms.openlocfilehash: 04baee2a3011cee63933fe1fdab759d1b6d29c89
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="headset"></a>Auriculares

En esta página se describen los conceptos básicos de programación para auriculares que usan [Windows.Gaming.Input.Headset][headset] y las API relacionadas para la Plataforma universal de Windows (UWP).

En esta página encontrarás información sobre:
* Cómo obtener acceso a auriculares conectados a un dispositivo de entrada o de navegación
* Cómo detectar que se han conectado o desconectado unos auriculares


## <a name="headset-overview"></a>Información general sobre auriculares

Los auriculares son dispositivos de captura y reproducción de audio que se suelen usar para comunicarse con otros jugadores en los juegos en línea, pero también pueden usarse en un juego o para otros usos creativos. Los auriculares son compatibles con aplicaciones para UWP de Xbox y Windows 10 en el espacio de nombres [Windows.Gaming.Input][].


## <a name="detect-and-track-headsets"></a>Detección y seguimiento de auriculares

El sistema administra los auriculares, por lo tanto, no tendrás que crearlos ni inicializarlos. El sistema proporciona acceso a los auriculares a través del dispositivo de entrada al que están conectados y eventos que te notifican cuando se conectan o desconectan unos auriculares.

### <a name="igamecontrollerheadset"></a>IGameController.Headset

Todos los dispositivos de entrada del espacio de nombres [Windows.Gaming.Input][] implementan la interfaz [IGameController][] que define la propiedad [Headset][igamecontroller.headset] en los auriculares actualmente conectados al dispositivo.

### <a name="connecting-and-disconnecting-headsets"></a>Conexión y desconexión de auriculares.

Cuando se conectan o desconectan unos auriculares, se generan los eventos [HeadsetConnected][igamecontroller.headsetconnected] y [HeadsetDisconnected][igamecontroller.headsetdisconnected]. Puedes registrar controladores para estos eventos para realizar un seguimiento de si un dispositivo de entrada tiene actualmente unos auriculares conectados.

En el siguiente ejemplo se muestra cómo registrar un controlador para el evento `HeadsetConnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetConnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // enable headset capture and playback on this device
}
```

En el siguiente ejemplo se muestra cómo registrar un controlador para el evento `HeadsetDisconnected`.

```cpp
auto inputDevice = myGamepads[0]; // or arcade stick, racing wheel

inputDevice.HeadsetDisconnected += ref new TypedEventHandler<IGameController^, Headset^>(IGameController^ device, Headset^ headset)
{
    // disable headset capture and playback on this device
}
```

## <a name="using-the-headset"></a>Uso de los auriculares

La clase [Headset][] se compone de dos cadenas que representan identificadores de punto de conexión XAudio: uno para la captura de audio (grabación desde el micrófono de los auriculares) y otro para la representación de audio (reproducción a través del auricular de los auriculares).

Los detalles del trabajo con XAudio no se explican aquí; para obtener más información, consulta el [Guía de programación de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737.aspx) y [XAudio2 API reference (Referencia de API XAudio)](https://msdn.microsoft.com/library/windows/desktop/ee415899.aspx).


[Windows.Gaming.Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[igamecontroller]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[igamecontroller.headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headset.aspx
[igamecontroller.headsetconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetconnected.aspx
[igamecontroller.headsetdisconnected]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.headsetdisconnected.aspx
[headset]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.headset.aspx
