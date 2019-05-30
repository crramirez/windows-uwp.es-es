---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: En este artículo se explica cómo las aplicaciones para UWP pueden detectar y responder a los cambios iniciados por el sistema en los niveles de secuencia de audio
title: Detectar y responder a cambios de estado de audio
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1a680347e9d1a749cc6e1d86ef1f02da280b4b74
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66361781"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Detectar y responder a cambios de estado de audio
A partir de Windows 10, versión 1803, la aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de una secuencia de audio que tu aplicación está usando. Puedes recibir notificaciones para capturar y representar secuencias, para una categoría de audio y dispositivo de audio concretos, o para un objeto [**MediaPlayer**](https://docs.microsoft.com/en-us/uwp/api/Windows.Media.Playback.MediaPlayer) que tu aplicación está usando para la reproducción multimedia. Por ejemplo, el sistema puede reducir, o atenuar, el nivel de reproducción de audio cuando suena una alarma. El sistema silenciará la aplicación cuando pasa al segundo plano si la aplicación no ha declarado la funcionalidad *backgroundMediaPlayback* en el manifiesto de la aplicación. 

El patrón para controlar los cambios de estado de audio es el mismo para todas las secuencias de audio compatibles. En primer lugar, crea una instancia de la clase [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor). En el siguiente ejemplo, la aplicación está usando la clase [**MediaCapture**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para capturar audio para el chat de juego. Se llama a un método de fábrica para obtener un monitor de estado de audio asociado a la secuencia de captura de audio del juego de chat del dispositivo de comunicaciones predeterminado.  A continuación, se registra un controlador para el evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged), que se generará cuando el sistema cambie el nivel de audio de la secuencia asociada.

[!code-cs[DeviceIdCategoryVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeviceIdCategoryVars)]

[!code-cs[SoundLevelDeviceIdCategory](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSoundLevelDeviceIdCategory)]

En el **SoundLevelChanged** controlador de eventos, compruebe el [ **SoundLevel** ](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) propiedad de la **AudioStateMonitor** remitente pasa a la controlador para determinar cuál es el nuevo nivel de audio para el flujo. En este ejemplo, la aplicación detiene la captura de audio cuando el nivel de sonido se silencia y reanuda la captura cuando el nivel de audio vuelve al volumen completo.

[!code-cs[GameChatSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetGameChatSoundLevelChanged)]

Para más información sobra la captura de audio con **MediaCapture**, consulta [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Todas las instancias de la clase [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer) tienen un **AudioStateMonitor** asociado que puedes usar para detectar cuándo cambia el sistema el nivel de volumen del contenido que se está reproduciendo actualmente. Puedes decidir controlar los cambios de estado de audio de manera diferente según el tipo de contenido que se está reproduciendo. Por ejemplo, puedes decidir pausar la reproducción de un podcast cuando se reduce el audio, pero continuar la reproducción si el contenido es música. 

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

Para obtener más información sobre cómo usar **MediaPlayer**, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Temas relacionados