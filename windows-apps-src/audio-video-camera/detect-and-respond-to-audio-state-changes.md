---
ms.assetid: EE0C1B28-EF9C-4BD9-A3C0-BDF11E75C752
description: En este artículo se explica cómo las aplicaciones UWP pueden detectar y responder a los cambios iniciados por el sistema en los niveles de secuencia de audio.
title: Detectar y responder a cambios de estado de audio
ms.date: 04/03/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a997a3c4d46bbd71c6ac4ae0d78b60edd8bd3ca1
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362728"
---
# <a name="detect-and-respond-to-audio-state-changes"></a>Detectar y responder a cambios de estado de audio
A partir de Windows 10, versión 1803, la aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de una secuencia de audio que usa la aplicación. Puede recibir notificaciones de secuencias de captura y representación, para un dispositivo de audio y una categoría de audio determinados, o para un objeto [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) que la aplicación usa para la reproducción multimedia. Por ejemplo, el sistema puede reducir o "pato" el nivel de reproducción de audio cuando se produce un timbre de alarma. El sistema silenciará la aplicación cuando pase a segundo plano si la aplicación no ha declarado la funcionalidad *backgroundMediaPlayback* en el manifiesto de la aplicación. 

El patrón para controlar los cambios de estado de audio es el mismo para todas las secuencias de audio compatibles. En primer lugar, cree una instancia de la clase [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) . En el ejemplo siguiente, la aplicación usa la clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) para capturar audio para el chat de juegos. Se llama a Factory Method para obtener un monitor de estado de audio asociado al flujo de captura de audio de chat de juegos del dispositivo de comunicaciones predeterminado.  A continuación, se registra un controlador para el evento [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) , que se generará cuando el sistema cambie el nivel de audio de la secuencia asociada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeviceIdCategoryVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSoundLevelDeviceIdCategory":::

En el controlador de eventos **SoundLevelChanged** , Compruebe la propiedad [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) del remitente **AudioStateMonitor** pasado en el controlador para determinar cuál es el nuevo nivel de audio de la secuencia. En este ejemplo, la aplicación deja de capturar audio cuando el nivel sonoro se silencia y reanuda la captura cuando el nivel de audio vuelve al volumen completo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetGameChatSoundLevelChanged":::

Para obtener más información sobre cómo capturar audio con **mediacapture**, consulte [captura básica de fotos, vídeos y audio con mediacapture](basic-photo-video-and-audio-capture-with-MediaCapture.md).

Cada instancia de la clase [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) tiene un **AudioStateMonitor** asociado que se puede usar para detectar cuándo el sistema cambia el nivel de volumen del contenido que se está reproduciendo actualmente. Puede decidir controlar los cambios de estado de audio de forma diferente en función del tipo de contenido que se reproduzca. Por ejemplo, puede decidir pausar la reproducción de un podcast cuando se reduce el audio, pero continuar la reproducción si el contenido es música. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetAudioStateVars":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaPlayer_RS1/cs/MainPage.xaml.cs" id="SnippetSoundLevelChanged":::

Para obtener más información sobre cómo usar **MediaPlayer**, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). 

## <a name="related-topics"></a>Temas relacionados
