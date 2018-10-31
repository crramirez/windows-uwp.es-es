---
author: drewbatgit
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con MediaPlayer.
title: Reproducir audio y vídeo con MediaPlayer
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6b83be1dee4e23fa6974e39fbfb0f9ce26529274
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "5864197"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Reproducir audio y vídeo con MediaPlayer

En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con la clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Con Windows 10, versión 1607, se realizaron mejoras significativas en las API de reproducción de elementos multimedia, como un diseño de proceso único simplificado para el audio en segundo plano, la integración automática con controles de transporte multimedia del sistema (SMTC), la capacidad de sincronizar varios reproductores multimedia de audio y de reproducir en una superficie Windows.UI.Composition y una interfaz sencilla para crear y programar interrupciones multimedia en tu contenido. Para aprovechar estas mejoras, el procedimiento recomendado para reproducir elementos multimedia es usar la clase **MediaPlayer** en lugar de **MediaElement** para reproducir elementos multimedia. El control de XAML ligero, [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), se introdujo para permitir representar contenido multimedia en una página XAML. Muchas API de estado y de control de la reproducción que proporciona la clase **MediaElement** están disponibles a través del nuevo objeto [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). **La clase MediaElement** continúa en funcionamiento para permitir la compatibilidad con versiones anteriores, pero no se le agregarán nuevas características.

En este artículo se muestran las características de **MediaPlayer** que usará una aplicación típica de reproducción de elementos multimedia. Ten en cuenta que **MediaPlayer** usa la clase [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) como un contenedor de todos los elementos multimedia. Esta clase permite cargar y reproducir elementos multimedia de distintos orígenes, como archivos locales, secuencias de memoria y orígenes de redes, todo con la misma interfaz. También hay clases de nivel superior que funcionan con **MediaSource**, como [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) y [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), que proporcionan características más avanzadas, como listas de reproducción y la capacidad para administrar orígenes multimedia con varias pistas de audio, vídeo y metadatos. Para obtener más información sobre **MediaSource** y las API relacionadas, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

> [!NOTE] 
> Las ediciones Windows 10 N y Windows 10 KN no incluyen las funciones multimedia necesarias para usar **MediaPlayer** para la reproducción. Estas características se pueden instalar manualmente. Para obtener más información, consulta [Media feature pack for Windows 10 N and Windows 10 KN editions](https://support.microsoft.com/en-us/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions) (Media Feature Pack para las ediciones Windows 10 N y Windows 10 KN).

## <a name="play-a-media-file-with-mediaplayer"></a>Reproducir un archivo multimedia con MediaPlayer  
La reproducción básica de elementos multimedia con **MediaPlayer** es muy fácil de implementar. En primer lugar, crea una nueva instancia de la clase **MediaPlayer**. Tu aplicación puede tener varias instancias de **MediaPlayer** activas a la vez. Después, establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) del reproductor en un objeto que implemente [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), como un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) o [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). En este ejemplo, un objeto **MediaSource** se crea a partir de un archivo en el almacenamiento local de la aplicación, y después se crea un objeto **MediaPlaybackItem** desde el origen y, se asigna a la propiedad **Source** del reproductor.

A diferencia de la clase **MediaElement**, la clase **MediaPlayer** no inicia automáticamente la reproducción de manera predeterminada. Para iniciar la reproducción, llama al método [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play), establece la propiedad [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) en true o espera a que el usuario inicie la reproducción con los controles multimedia integrados.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Cuando la aplicación haya terminado de usar una clase **MediaPlayer**, debes llamar al método [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (previsto para **Dispose** en C#), para liberar los recursos usados por el reproductor.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Usar MediaPlayerElement para representar vídeo en XAML
Puedes reproducir elementos multimedia en una clase **MediaPlayer** sin que se muestren en XAML, pero muchas aplicaciones de reproducción multimedia querrán representar el contenido multimedia en una página XAML. Para ello, usa el control de [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) ligero. Como **MediaElement**, el elemento **MediaPlayerElement** te permite especificar si se deben mostrar los controles de transporte integrados.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Para establecer la instancia **MediaPlayer** con la que está enlazado el elemento, llama a [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

También puedes establecer el origen de reproducción en el elemento **MediaPlayerElement** y este creará automáticamente una nueva instancia de **MediaPlayer** a la que podrás acceder con la propiedad [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Si deshabilitas la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) al establecer la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en "false", se interrumpirá el vínculo entre **MediaPlayer** y [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) que proporciona el elemento **MediaPlayerElement**, de modo que los controles de transporte integrados ya no controlarán automáticamente la reproducción del reproductor. En su lugar, debes implementar tus propios controles para controlar la clase **MediaPlayer**.

## <a name="common-mediaplayer-tasks"></a>Tareas comunes de MediaPlayer
En esta sección se muestra cómo usar algunas de las características de la clase **MediaPlayer**.

### <a name="set-the-audio-category"></a>Establecer la categoría de audio
Establece la propiedad [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) de una clase **MediaPlayer** en uno de los valores de la enumeración [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) para que el sistema sepa qué tipo de elemento multimedia se está reproduciendo. Los juegos deben clasificar sus secuencias de música como **GameMedia** para que la música del juego se silencie automáticamente si otra aplicación reproduce música en segundo plano. Las aplicaciones de música o vídeo deben clasificar sus secuencias como **Media** o **Movie** para que tengan prioridad sobre las secuencias **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>Salida a un punto de conexión de audio específico
De manera predeterminada, la salida de audio de una clase **MediaPlayer** se enruta hacia el punto de conexión de audio predeterminado para el sistema, pero puedes especificar un punto de conexión de audio que la clase **MediaPlayer** deba usar para la salida. En el ejemplo siguiente, el elemento [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) devuelve una cadena que exclusivamente identifica la categoría de representación de audio de los dispositivos. A continuación, se llama al método [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) para obtener una lista de todos los dispositivos disponibles del tipo seleccionado. Se puede determinar mediante programación el dispositivo que quieres usar o agregar los dispositivos devueltos a un control [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para permitir al usuario seleccionar un dispositivo.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

En el evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) del cuadro combinado de dispositivos, se establece la propiedad [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) de la clase **MediaPlayer** en el dispositivo seleccionado, que se guardó en la propiedad [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) del elemento **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>Sesión de reproducción
Como se describió anteriormente en este artículo, muchas de las funciones que expone la clase **MediaElement** se movieron a la clase [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Esto incluye información sobre el estado de reproducción del reproductor, como la posición de reproducción actual, si está en pausa o en reproducción y la velocidad de reproducción actual. **MediaPlaybackSession** también proporciona varios eventos para notificarte si cambia el estado, como el almacenamiento en búfer actual y el estado de descarga del contenido que se reproduce, y el tamaño natural y la relación de aspecto del contenido de vídeo que se reproduce actualmente.

El siguiente ejemplo muestra cómo implementar un controlador de clic del botón que salta 10 segundos adelante en el contenido. Primero, se recupera el objeto **MediaPlaybackSession** del reproductor con la propiedad [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). A continuación, se establece la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) en la posición de reproducción actual más 10 segundos.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

En el siguiente ejemplo se muestra cómo usar un botón de alternancia para alternar entre la velocidad de reproducción normal y la velocidad 2X con la configuración de la propiedad [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) de la sesión.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

A partir de Windows 10, versión 1803, puedes establecer la rotación con el que se presenta el vídeo en **MediaPlayer** en incrementos de 90 grados.

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>Detectar el almacenamiento en búfer esperado e inesperado
El objeto **MediaPlaybackSession** que se describe en la sección anterior proporciona dos eventos para detectar cuándo el archivo multimedia que se está reproduciendo actualmente empieza y termina el almacenamiento en búfer, **[BufferingStarted](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** y **[BufferingEnded](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**. Esto te permite actualizar la interfaz de usuario para mostrar al usuario que se está produciendo el almacenamiento en búfer. Se espera el almacenamiento en búfer inicial cuando se abre un archivo multimedia por primera vez o cuando el usuario cambia a un nuevo elemento de una lista de reproducción. El almacenamiento en búfer inesperado puede producirse cuando se reduce la velocidad de la red o si el sistema de administración de contenido que proporciona el contenido experimenta problemas técnicos. A partir de RS3, puedes usar el evento **BufferingStarted** para determinar si se espera el evento de almacenamiento en búfer o si es inesperado y está interrumpiendo la reproducción. Puedes usar esta información como datos de telemetría para tu servicio de entrega multimedia o aplicación. 

Registrar controladores para que los eventos **BufferingStarted** y **BufferingEnded** reciban notificaciones de estado de almacenamiento en búfer.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

En el controlador de eventos **BufferingStarted**, convierte los argumentos de evento pasados al evento a un objeto **[MediaPlaybackSessionBufferingStartedEventArgs](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** y comprueba la propiedad **[IsPlaybackInterruption](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)**. Si este valor es true, el almacenamiento en búfer que desencadenó el evento es inesperado y está interrumpiendo la reproducción. De lo contrario, es almacenamiento en búfer inicial esperado. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>Acercar los dedos y hacer zoom de vídeo
**MediaPlayer** te permite especificar el rectángulo de origen con el contenido de vídeo que se debe representar y permite hacer zoom en el vídeo de forma eficaz. El rectángulo que especifiques es relativo a un rectángulo normalizado (0,0,1,1), donde 0,0 es la parte superior izquierda del fotograma y 1,1 especifica el ancho y alto total del fotograma. Por lo tanto, por ejemplo, para establecer el rectángulo de zoom para que se represente el cuadrante superior derecho del vídeo, se especifica el rectángulo (.5,0,.5,.5).  Es importante comprobar los valores para asegurarte de que el rectángulo de origen está dentro del rectángulo normalizado (0,0,1,1). Intentar establecer un valor fuera de este intervalo hará que se inicie una excepción.

Para implementar la opción de acercar los dedos y hacer zoom con gestos multitoque, antes debes especificar qué gestos quieres admitir. En este ejemplo, se solicitan los gestos ampliar y trasladar. El evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) se genera cuando se produce uno de los gestos suscritos. El evento [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) se usará para restablecer el zoom a un fotograma completo. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

A continuación, declara un objeto **Rect** que almacenará el rectángulo de origen de zoom actual.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

El controlador **ManipulationDelta** ajusta la escala o la traslación del rectángulo de zoom. Si el valor de escala delta no es 1, significa que el usuario realizó un gesto de reducir. Si el valor es mayor que 1, el rectángulo de origen debe hacerse más pequeño para acercar el contenido. Si el valor es inferior a 1, el rectángulo de origen debe hacerse más grande para alejar. Antes de establecer los nuevos valores de escala, se comprueba el rectángulo resultante para garantizar que quede por completo dentro de los límites (0,0,1,1).

Si el valor de escala es 1, se controla el gesto de traslación. Simplemente, el número de píxeles en el gesto dividido por el ancho y alto del control traslada el rectángulo. De nuevo, se comprueba el rectángulo resultante para asegurarse de que se encuentra dentro de los límites (0,0,1,1).

Por último, el objeto [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) del elemento **MediaPlaybackSession** se establece en el rectángulo recién ajustado y se especifica el área del fotograma de vídeo que se debe representar.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

En el controlador de eventos [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), el rectángulo de origen se vuelve a establecer en (0,0,1,1) para hacer que se represente todo el fotograma de vídeo.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

### <a name="handling-policy-based-playback-degradation"></a>Controlar la degradación de reproducción basada en directivas

En algunas circunstancias, el sistema puede degradar la reproducción de un elemento multimedia, como la reducción de la resolución (restricción), en función de una directiva en lugar de un problema de rendimiento. Por ejemplo, el vídeo puede verse afectado por el sistema si se está reproduciendo con un controlador de vídeo sin firmar. Puedes llamar a [**MediaPlaybackSession.GetOutputDegradationPolicyState**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) para determinar si, y por qué, se está produciendo esta degradación basada en directivas y alertar al usuario o registrar el motivo con fines de telemetría.

El siguiente ejemplo muestra una implementación de un controlador para el evento **MediaPlayer.MediaOpened** que se genera cuando el Reproductor abre un nuevo elemento multimedia. Se llama a **GetOutputDegradationPolicyState** en el **MediaPlayer** pasado al controlador. El valor de [**VideoConstrictionReason**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) indica el motivo de la directiva por el que el vídeo está restringido. Si el valor no es **None**, este ejemplo registra el motivo de degradación con fines de telemetría. Este ejemplo también muestra la configuración de la velocidad de bits del valor de **AdaptiveMediaSource** que se reproduce actualmente en el mínimo ancho de banda para guardar el uso de datos, puesto que el vídeo está restringido y no se mostrará en alta resolución de todos modos. Para más información sobre el uso de **AdaptiveMediaSource**, consulta [Streaming adaptable](adaptive-streaming.md).

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Usar MediaPlayerSurface para representar vídeo en una superficie Windows.UI.Composition
A partir de Windows 10, versión 1607, puedes usar **MediaPlayer** para representar vídeo en una superficie [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), que permite que el reproductor interopere con las API en el espacio de nombres [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). El fotograma de composición te permite trabajar con gráficos en la capa visual entre XAML y las API de gráficos de DirectX de bajo nivel. Esto permite escenarios como la representación de vídeo en cualquier control XAML. Para obtener más información sobre cómo usar las API de composición, consulta [Capa visual](https://msdn.microsoft.com/windows/uwp/composition/visual-layer).

En el siguiente ejemplo se muestra cómo representar el contenido del reproductor de vídeo en un control [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). Las llamadas específicas del reproductor multimedia de este ejemplo son [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) y [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** indica al sistema el tamaño del búfer que se debe asignar para representar contenido. **GetSurface** toma una clase [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) como argumento y recupera una instancia de la clase [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Esta clase proporciona acceso a las clases **MediaPlayer** y **Compositor** usadas para crear la superficie y expone la misma superficie a través de la propiedad [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

El resto del código del ejemplo crea un objeto [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual) por el que se representa el vídeo y establece el tamaño en el tamaño del elemento de lienzo que mostrará el elemento visual. A continuación, se crea un objeto [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) a partir de la clase [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) y se asigna a la propiedad [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) del elemento visual. A continuación, se crea una clase [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) y se inserta un objeto **SpriteVisual** en la parte superior de su árbol visual. Por último, se llama a un objeto [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) para asignar el elemento visual contenedor a la clase **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Usa MediaTimelineController para sincronizar contenido entre varios reproductores.
Como se explicó anteriormente en este artículo, la aplicación puede tener varios objetos **MediaPlayer** activos a la vez. De manera predeterminada, cada objeto **MediaPlayer** que crees funciona de forma independiente. En algunos escenarios, como la sincronización de una pista de comentarios en un vídeo, es posible que quieras sincronizar el estado del reproductor, la posición de reproducción y la velocidad de reproducción de varios reproductores. A partir de Windows 10, versión 1607, puedes implementar este comportamiento usando la clase [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

### <a name="implement-playback-controls"></a>Implementar controles de reproducción
En el siguiente ejemplo se muestra cómo usar un objeto **MediaTimelineController** para controlar dos instancias del elemento **MediaPlayer**. En primer lugar, se crea una instancia de cada instancia de la clase **MediaPlayer** y la propiedad **Source** se establece en un archivo multimedia. A continuación, se crea un objeto **MediaTimelineController** nuevo. Para cada objeto **MediaPlayer**, se deshabilita la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) asociada a cada reproductor al establecer la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en false. Y, a continuación, la propiedad [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) se establece en el objeto controlador de línea de tiempo.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Precaución** La clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) proporciona integración automática entre **MediaPlayer** y los controles de transporte de contenido multimedia del sistema (SMTC), pero no se puede usar esta integración automática con reproductores multimedia que se controlen con un objeto **MediaTimelineController**. Por lo tanto, debes deshabilitar el administrador de comandos del reproductor multimedia antes de establecer el controlador de línea de tiempo del reproductor. Si no se hace, se generará una excepción con el siguiente mensaje: "Se bloqueó la adjunción del controlador de línea de tiempo multimedia debido al estado actual del objeto". Para obtener más información sobre la integración del reproductor multimedia con los SMTC, consulta [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md). Si estás usando un objeto **MediaTimelineController**, aún puedes controlar los SMTC manualmente. Para obtener más información, consulta [Controles de transporte de contenido multimedia del sistema](system-media-transport-controls.md).

Cuando hayas adjuntado un objeto **MediaTimelineController** a uno o más reproductores multimedia, puedes controlar el estado de reproducción con los métodos que expone el controlador. El siguiente ejemplo llama al método [**Start**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.Start) para iniciar la reproducción de todos los reproductores multimedia adjuntos al inicio del elemento multimedia.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

En este ejemplo se muestra cómo pausar y reanudar todos los reproductores multimedia adjuntos.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Para avanzar rápidamente todos los reproductores multimedia conectados, establece la velocidad de reproducción en un valor mayor que 1.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

En el siguiente ejemplo se muestra cómo usar un **control deslizante** para mostrar la posición de reproducción actual del controlador de línea de tiempo relativo a la duración del contenido de uno de los reproductores multimedia conectados. En primer lugar, se crea un objeto **MediaSource** nuevo y se registra un controlador para el objeto [**OpenOperationCompleted**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource.OpenOperationCompleted) de origen multimedia. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

El controlador **OpenOperationCompleted** se usa como una oportunidad para descubrir la duración del contenido de origen multimedia. Cuando se haya determinado la duración, el valor máximo del **control deslizante** se establece en el número total de segundos del elemento multimedia. El valor se establece dentro de una llamada al método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para asegurarse de que se ejecute en el subproceso de la interfaz de usuario.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

A continuación, se registra un controlador para el evento de controlador de línea de tiempo [**PositionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController.PositionChanged). El sistema lo llama periódicamente, aproximadamente 4 veces por segundo.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

En el controlador del objeto **PositionChanged**, el valor del control deslizante se actualiza para reflejar la posición actual del controlador de la línea de tiempo.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Desplazar la posición de reproducción desde la posición de línea de tiempo
En algunos casos, es posible que quieras que la posición de reproducción de uno o más reproductores multimedia asociados a un controlador de línea de tiempo se desplace desde los otros reproductores. Para ello, establece la propiedad [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) del objeto **MediaPlayer** que quieras desplazar. En el siguiente ejemplo, se usan las duraciones del contenido de dos reproductores multimedia para establecer los valores mínimos y máximos de dos control deslizantes para aumentar y reducir la longitud del elemento.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

En el evento [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) de cada control deslizante, el objeto **TimelineControllerPositionOffset** de cada reproductor está establecido en el valor correspondiente.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Ten en cuenta que si el valor de desplazamiento de un reproductor se asigna a una posición de reproducción negativa, el clip permanecerá en pausa hasta que el desplazamiento llegue a cero y, a continuación, se iniciará la reproducción. Del mismo modo, si el valor de desplazamiento se asigna a una posición de reproducción mayor que la duración del elemento multimedia, se mostrará el último fotograma, igual que cuando un único reproductor multimedia alcanza el final de su contenido.

## <a name="play-spherical-video-with-mediaplayer"></a>Reproducir vídeo esférico con MediaPlayer
A partir de Windows 10, versión 1703, **MediaPlayer** admite la proyección equirectangular para la reproducción de vídeo esférico. El contenido de vídeo esférico no es diferente del vídeo plano normal en el sentido que **MediaPlayer** representará el vídeo siempre y cuando se admita la codificación correspondiente. En el caso de vídeo esférico que contiene una etiqueta de metadatos que especifica que el vídeo usa la proyección equirectangular, **MediaPlayer** puede representar el vídeo con un campo de vista y orientación de vista especificados. Esto permite escenarios como, por ejemplo, la reproducción de vídeo de realidad virtual con una pantalla montada en la cabeza o simplemente permitir al usuario hacer un paneo dentro del contenido de vídeo esférico mediante el mouse o entrada de teclado.

Para reproducir vídeo esférico, sigue los pasos de reproducción de contenido de vídeo que se describe anteriormente en este artículo. El otro paso consiste en registrar un controlador para el evento [**MediaPlayer.MediaOpened**])https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened). Este evento proporciona una oportunidad para habilitar y controlar los parámetros de reproducción de vídeo esférico.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

En el controlador **MediaOpened**, comprueba primero el formato de fotogramas del elemento multimedia recién abierto. Para ello, comprueba la propiedad [**PlaybackSession.SphericalVideoProjection.FrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat). Si este valor es [**SphericaVideoFrameFormat.Equirectangular**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), el sistema puede proyectar automáticamente el contenido de vídeo. Primero, establece la propiedad [**PlaybackSession.SphericalVideoProjection.IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) en **true**. También puedes ajustar las propiedades, como la orientación de vista y el campo de vista que el reproductor multimedia usará para proyectar el contenido de vídeo. En este ejemplo, el campo de vista se estableció en un valor ancho de 120grados al establecer la propiedad [**HorizontalFieldOfViewInDegrees**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees).

Si el contenido de vídeo es esférico, pero está en un formato que no sea equirectangular, puedes implementar tu propio algoritmo de proyección mediante el modo de servidor de fotogramas del reproductor multimedia para recibir y procesar fotogramas individuales.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

En el siguiente código de ejemplo se muestra cómo ajustar la orientación de vista del vídeo esférico mediante las teclas de flecha izquierda y derecha.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

Si tu aplicación admite listas de reproducción de vídeo, es recomendable identificar los elementos de reproducción que contienen vídeo esférico en la interfaz de usuario. Las listas de reproducción multimedia se describen en detalle en el artículo [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). En el siguiente ejemplo se muestra la creación de una nueva lista de reproducción, la adición de un elemento y el registro de un controlador para el evento [**MediaPlaybackItem.VideoTracksChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged), que se produce cuando se resuelven las pistas de vídeo de un elemento multimedia.

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

En el controlador de eventos **VideoTracksChanged**, para obtener las propiedades de codificación de las pistas de vídeo agregadas, llama a [**VideoTrack.GetEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Si la propiedad [**SphericalVideoFrameFormat**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) de las propiedades de codificación es un valor distinto de [**SphericaVideoFrameFormat.None**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), la pista de vídeo contiene vídeo esférico y puedes actualizar la interfaz de usuario según corresponda, si quieres.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>Usar MediaPlayer en modo de servidor de fotogramas
A partir de Windows 10, versión 1703, puedes usar **MediaPlayer** en modo de servidor de fotogramas. En este modo, **MediaPlayer** no representará automáticamente los fotogramas en un objeto **MediaPlayerElement** asociado. En cambio, la aplicación copia el fotograma actual de **MediaPlayer** a un objeto que implementa [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). El principal escenario de esta característica es usar sombreadores de píxeles para procesar los fotogramas de vídeo que proporcione **MediaPlayer**. La aplicación es responsable de mostrar cada fotograma después del procesamiento, por ejemplo, mostrando el fotograma en un control [**Image**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) de XAML.

En el siguiente ejemplo, se inicializa un nuevo objeto **MediaPlayer** y se carga el contenido de vídeo. Luego, se registra un controlador para [**VideoFrameAvailable**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable). El modo de servidor de fotogramas se habilita al establecer la propiedad [**IsVideoFrameServerEnabled**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) del objeto **MediaPlayer** en **true**. Por último, la reproducción multimedia se inicia con una llamada a [**reproducir**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Play).

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

En el siguiente ejemplo se muestra un controlador para **VideoFrameAvailable** que usa [Win2D](https://github.com/Microsoft/Win2D) para agregar un efecto de desenfoque en cada fotograma de vídeo y luego se muestran los fotogramas procesados en un control [Image](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image) de XAML.

Siempre que se llame al controlador **VideoFrameAvailable**, el método [**CopyFrameToVideoSurface**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) se usa para copiar el contenido del fotograma en un objeto [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). También puedes usar [**CopyFrameToStereoscopicVideoSurfaces**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) para copiar contenido 3D en dos superficies con el fin de procesar el contenido del ojo izquierdo y el del ojo derecho por separado. Para obtener un objeto que implemente **IDirect3DSurface**, en este ejemplo se crea un objeto [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap) y luego se usa ese objeto para crear un objeto **CanvasBitmap** Win2D, que implementa la interfaz necesaria. Un objeto **CanvasImageSource** es un objeto de Win2D que se puede usar como el origen de un control **Image**, de modo que uno se crea y se establece como origen para el objeto **Image** en el que se mostrará el contenido. Luego, se crea un objeto **CanvasDrawingSession**. Win2D lo usa para representar el efecto de desenfoque.

Una vez que todos los objetos necesarios se hayan inicializado, se llama a **CopyFrameToVideoSurface**, qué copia el fotograma actual desde **MediaPlayer** a **CanvasBitmap**. Luego, se crea un objeto **GaussianBlurEffect** de Win2D con el objeto **CanvasBitmap** establecido como el origen de la operación. Por último, se llama a **CanvasDrawingSession.DrawImage** para dibujar la imagen de origen, con el efecto de desenfoque aplicado, en el objeto **CanvasImageSource** que se asoció al control **Image**, lo que hace que se dibuje en la interfaz de usuario.

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Para más información sobre el uso de Win2D, consulta el [repositorio GitHub de Win2D](https://github.com/Microsoft/Win2D). Para probar el código de ejemplo que se muestra arriba, tendrás que agregar el paquete de NuGet Win2D al proyecto con las siguientes instrucciones.

**Para agregar el paquete de NuGet Win2D al proyecto de efecto**

1.  En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto y selecciona **Administrar paquetes NuGet**.
2.  En la parte superior de la ventana, selecciona la pestaña **Examinar**.
3.  En el cuadro de búsqueda, escribe **Win2D**.
4.  Selecciona **Win2D.uwp**y luego selecciona **Instalar** en el panel derecho.
5.  En el cuadro de diálogo **Revisar cambios** se muestra el paquete que se instalará. Haz clic en **Aceptar**.
6.  Acepta la licencia del paquete.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar y responder a cambios de nivel de audio por el sistema
A partir de Windows 10, versión 1803, tu aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de un **MediaPlayer** que se está reproduciendo actualmente. Por ejemplo, el sistema puede reducir, o atenuar, el nivel de reproducción de audio cuando suena una alarma. El sistema silenciará la aplicación cuando pasa al segundo plano si la aplicación no ha declarado la funcionalidad *backgroundMediaPlayback* en el manifiesto de la aplicación. La clase [**AudioStateMonitor**](https://docs.microsoft.comuwp/api/windows.media.audio.audiostatemonitor) te permite registrarte para recibir un evento cuando el sistema modifica el volumen de una secuencia de audio. Obtén acceso a la propiedad **AudioStateMonitor** de un **MediaPlayer** y registra un controlador para que se notifique el evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) cuando el nivel de audio para dicho **MediaPlayer** se cambie por el sistema.

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Al controlar el evento **SoundLevelChanged**, puede realizar distintas acciones en función del tipo de contenido que se está reproduciendo. Si actualmente estás reproduciendo música, puedes dejar que la música siga reproduciéndose mientras se atenúa el volumen. Sin embargo, si estás reproduciendo un podcast, es probable que quieras pausar la reproducción mientras el audio esté atenuado para que el usuario no pierda nada del contenido.

Este ejemplo declara una variable para comprobar si el contenido que se está reproduciendo es un podcast; se supone que lo estableces en el valor adecuado al seleccionar el contenido de **MediaPlayer**. También creamos una variable de clase para realizar un seguimiento de cuándo pausamos la reproducción mediante programación cuando cambia el nivel de audio.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

En el controlador de eventos **SoundLevelChanged**, comprueba la propiedad [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) del remitente **AudioStateMonitor** remitente para determinar el nuevo nivel de sonido. En este ejemplo, se comprueba si el nuevo nivel de sonido es el volumen completo, lo que significa que el sistema ha dejado de silenciar o de atenuar el volumen, o si el nivel de sonido se ha reducido pero está reproduciendo contenido que no es podcast. Si alguno de estos es true y el contenido se pausó anteriormente mediante programación, se reanuda la reproducción. Si el nuevo nivel de sonido está silenciado o si el contenido actual es un podcast y el nivel de sonido es bajo, se pausa la reproducción y la variable se establece para comprobar que la pausa se inició mediante programación.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

El usuario puede decidir que quiere pausar o continuar la reproducción, incluso si el sistema atenúa el audio. Este ejemplo muestra los controladores de eventos para un botón y reproducción y pausa. En el controlador de clic del botón de pausa que está pausado, si la reproducción ya había estado pausada mediante programación, actualizamos la variable para indicar que el usuario ha pausado el contenido. En el controlador de clic del botón de reproducción, reanudamos la reproducción y borramos nuestra variable de seguimiento.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Crear, programar y administrar interrupciones multimedia](create-schedule-and-manage-media-breaks.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md)





 

 




