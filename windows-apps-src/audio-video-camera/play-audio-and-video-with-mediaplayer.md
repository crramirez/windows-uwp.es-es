---
ms.assetid: 58af5e9d-37a1-4f42-909c-db7cb02a0d12
description: En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con MediaPlayer.
title: Reproducir audio y vídeo con MediaPlayer
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e29862160adc3a78bd1b83d6869db47903c638b7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163749"
---
# <a name="play-audio-and-video-with-mediaplayer"></a>Reproducir audio y vídeo con MediaPlayer

En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con la clase [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer). Con Windows 10, versión 1607, se realizaron mejoras significativas en las API de reproducción multimedia, incluido un diseño simplificado de un solo proceso para audio de fondo, la integración automática con los controles de transporte de medios del sistema (SMTC), la capacidad de sincronizar varios reproductores multimedia, la capacidad de representar fotogramas de vídeo en una superficie Windows. UI. Composition y una Para aprovechar estas mejoras, el procedimiento recomendado para reproducir elementos multimedia es usar la clase **MediaPlayer** en lugar de **MediaElement** para reproducir elementos multimedia. El control de XAML ligero, [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement), se introdujo para permitir representar contenido multimedia en una página XAML. Muchas API de estado y de control de la reproducción que proporciona la clase **MediaElement** están disponibles a través del nuevo objeto [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession). **La clase MediaElement** continúa en funcionamiento para permitir la compatibilidad con versiones anteriores, pero no se le agregarán nuevas características.

En este artículo se muestran las características de **MediaPlayer** que usará una aplicación típica de reproducción de elementos multimedia. Ten en cuenta que **MediaPlayer** usa la clase [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) como un contenedor de todos los elementos multimedia. Esta clase permite cargar y reproducir elementos multimedia de distintos orígenes, como archivos locales, secuencias de memoria y orígenes de redes, todo con la misma interfaz. También hay clases de nivel superior que funcionan con **MediaSource**, como [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) y [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList), que proporcionan características más avanzadas, como listas de reproducción y la capacidad para administrar orígenes multimedia con varias pistas de audio, vídeo y metadatos. Para obtener más información sobre **MediaSource** y las API relacionadas, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

> [!NOTE] 
> Las ediciones Windows 10 N y Windows 10 KN no incluyen las características multimedia necesarias para usar **MediaPlayer** para la reproducción. Estas características se pueden instalar manualmente. Para obtener más información, consulte [Feature Pack de media para las ediciones Windows 10 N y Windows 10 kN](https://support.microsoft.com/help/3010081/media-feature-pack-for-windows-10-n-and-windows-10-kn-editions).

## <a name="play-a-media-file-with-mediaplayer"></a>Reproducir un archivo multimedia con MediaPlayer  
La reproducción básica de elementos multimedia con **MediaPlayer** es muy fácil de implementar. En primer lugar, crea una nueva instancia de la clase **MediaPlayer**. Tu aplicación puede tener varias instancias de **MediaPlayer** activas a la vez. Después, establece la propiedad [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) del reproductor en un objeto que implemente [**IMediaPlaybackSource**](/uwp/api/Windows.Media.Playback.IMediaPlaybackSource), como un objeto [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) o [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). En este ejemplo, un objeto **MediaSource** se crea a partir de un archivo en el almacenamiento local de la aplicación, y después se crea un objeto **MediaPlaybackItem** desde el origen y, se asigna a la propiedad **Source** del reproductor.

A diferencia de la clase **MediaElement**, la clase **MediaPlayer** no inicia automáticamente la reproducción de manera predeterminada. Para iniciar la reproducción, llama al método [**Play**](/uwp/api/windows.media.playback.mediaplayer.play), establece la propiedad [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) en true o espera a que el usuario inicie la reproducción con los controles multimedia integrados.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Cuando la aplicación haya terminado de usar una clase **MediaPlayer**, debes llamar al método [**Close**](/uwp/api/windows.media.playback.mediaplayer.close) (previsto para **Dispose** en C#), para liberar los recursos usados por el reproductor.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

## <a name="use-mediaplayerelement-to-render-video-in-xaml"></a>Usar MediaPlayerElement para representar vídeo en XAML
Puedes reproducir elementos multimedia en una clase **MediaPlayer** sin que se muestren en XAML, pero muchas aplicaciones de reproducción multimedia querrán representar el contenido multimedia en una página XAML. Para ello, usa el control de [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) ligero. Como **MediaElement**, el elemento **MediaPlayerElement** te permite especificar si se deben mostrar los controles de transporte integrados.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Para establecer la instancia **MediaPlayer** con la que está enlazado el elemento, llama a [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

También puedes establecer el origen de reproducción en el elemento **MediaPlayerElement** y este creará automáticamente una nueva instancia de **MediaPlayer** a la que podrás acceder con la propiedad [**MediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Si deshabilitas la clase [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) de [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) al establecer la propiedad [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) en "false", se interrumpirá el vínculo entre **MediaPlayer** y [**TransportControls**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.transportcontrols) que proporciona el elemento **MediaPlayerElement**, de modo que los controles de transporte integrados ya no controlarán automáticamente la reproducción del reproductor. En su lugar, debes implementar tus propios controles para controlar la clase **MediaPlayer**.

## <a name="common-mediaplayer-tasks"></a>Tareas comunes de MediaPlayer
En esta sección se muestra cómo usar algunas de las características de la clase **MediaPlayer**.

### <a name="set-the-audio-category"></a>Establecer la categoría de audio
Establece la propiedad [**AudioCategory**](/uwp/api/windows.media.playback.mediaplayer.audiocategory) de una clase **MediaPlayer** en uno de los valores de la enumeración [**MediaPlayerAudioCategory**](/uwp/api/Windows.Media.Playback.MediaPlayerAudioCategory) para que el sistema sepa qué tipo de elemento multimedia se está reproduciendo. Los juegos deben clasificar sus secuencias de música como **GameMedia** para que la música del juego se silencie automáticamente si otra aplicación reproduce música en segundo plano. Las aplicaciones de música o vídeo deben clasificar sus secuencias como **Media** o **Movie** para que tengan prioridad sobre las secuencias **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

### <a name="output-to-a-specific-audio-endpoint"></a>Salida a un punto de conexión de audio específico
De manera predeterminada, la salida de audio de una clase **MediaPlayer** se enruta hacia el punto de conexión de audio predeterminado para el sistema, pero puedes especificar un punto de conexión de audio que la clase **MediaPlayer** deba usar para la salida. En el ejemplo siguiente, el elemento [**MediaDevice.GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector) devuelve una cadena que exclusivamente identifica la categoría de representación de audio de los dispositivos. A continuación, se llama al método [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)[**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) para obtener una lista de todos los dispositivos disponibles del tipo seleccionado. Se puede determinar mediante programación el dispositivo que quieres usar o agregar los dispositivos devueltos a un control [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para permitir al usuario seleccionar un dispositivo.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

En el evento [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) del cuadro combinado de dispositivos, se establece la propiedad [**AudioDevice**](/uwp/api/windows.media.playback.mediaplayer.audiodevice) de la clase **MediaPlayer** en el dispositivo seleccionado, que se guardó en la propiedad [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) del elemento **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

### <a name="playback-session"></a>Sesión de reproducción
Como se describió anteriormente en este artículo, muchas de las funciones que expone la clase **MediaElement** se movieron a la clase [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession). Esto incluye información sobre el estado de reproducción del reproductor, como la posición de reproducción actual, si está en pausa o en reproducción y la velocidad de reproducción actual. **MediaPlaybackSession** también proporciona varios eventos para notificarte si cambia el estado, como el almacenamiento en búfer actual y el estado de descarga del contenido que se reproduce, y el tamaño natural y la relación de aspecto del contenido de vídeo que se reproduce actualmente.

El siguiente ejemplo muestra cómo implementar un controlador de clic del botón que salta 10 segundos adelante en el contenido. Primero, se recupera el objeto **MediaPlaybackSession** del reproductor con la propiedad [**PlaybackSession**](/uwp/api/windows.media.playback.mediaplayer.playbacksession). A continuación, se establece la propiedad [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) en la posición de reproducción actual más 10 segundos.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

En el siguiente ejemplo se muestra cómo usar un botón de alternancia para alternar entre la velocidad de reproducción normal y la velocidad 2X con la configuración de la propiedad [**PlaybackRate**](/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate) de la sesión.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

A partir de Windows 10, versión 1803, puede establecer la rotación con la que se presenta el vídeo en el control **MediaPlayer** en incrementos de 90 grados.

[!code-cs[SetRotation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetRotation)]

### <a name="detect-expected-and-unexpected-buffering"></a>Detectar almacenamiento en búfer esperado e inesperado
El objeto **MediaPlaybackSession** que se describe en la sección anterior proporciona dos eventos para detectar cuándo comienza el archivo multimedia que se está reproduciendo y finaliza el almacenamiento en búfer, **[BufferingStarted](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingStarted)** y **[BufferingEnded](/uwp/api/windows.media.playback.mediaplaybacksession.BufferingEnded)**. Esto le permite actualizar la interfaz de usuario para mostrar el usuario que está realizando el almacenamiento en búfer. El almacenamiento en búfer inicial se espera cuando se abre por primera vez un archivo multimedia o cuando el usuario cambia a un nuevo elemento en una lista de reproducción. El almacenamiento en búfer inesperado puede producirse cuando se degrada la velocidad de la red o si el sistema de administración de contenido que proporciona el contenido experimenta problemas técnicos. A partir de RS3, puede usar el evento **BufferingStarted** para determinar si se espera el evento de almacenamiento en búfer o si es inesperado e interrumpiendo la reproducción. Puede usar esta información como datos de telemetría de su aplicación o servicio de entrega multimedia. 

Registre Controladores para los eventos **BufferingStarted** y **BufferingEnded** para recibir notificaciones de estado de almacenamiento en búfer.

[!code-cs[RegisterBufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterBufferingHandlers)]

En el controlador de eventos **BufferingStarted** , convierta los argumentos del evento pasados en el evento a un objeto **[MediaPlaybackSessionBufferingStartedEventArgs](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs)** y Compruebe la propiedad **[IsPlaybackInterruption](/uwp/api/windows.media.playback.mediaplaybacksessionbufferingstartedeventargs.IsPlaybackInterruption)** . Si este valor es true, el almacenamiento en búfer que desencadenó el evento es inesperado e interrumpiendo la reproducción. De lo contrario, se espera el almacenamiento en búfer inicial. 

[!code-cs[BufferingHandlers](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetBufferingHandlers)]


### <a name="pinch-and-zoom-video"></a>Acercar los dedos y hacer zoom de vídeo
**MediaPlayer** te permite especificar el rectángulo de origen con el contenido de vídeo que se debe representar y permite hacer zoom en el vídeo de forma eficaz. El rectángulo que especifiques es relativo a un rectángulo normalizado (0,0,1,1), donde 0,0 es la parte superior izquierda del fotograma y 1,1 especifica el ancho y alto total del fotograma. Por lo tanto, por ejemplo, para establecer el rectángulo de zoom para que se represente el cuadrante superior derecho del vídeo, se especifica el rectángulo (.5,0,.5,.5).  Es importante comprobar los valores para asegurarte de que el rectángulo de origen está dentro del rectángulo normalizado (0,0,1,1). Intentar establecer un valor fuera de este intervalo hará que se inicie una excepción.

Para implementar la opción de acercar los dedos y hacer zoom con gestos multitoque, antes debes especificar qué gestos quieres admitir. En este ejemplo, se solicitan los gestos ampliar y trasladar. El evento [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta) se genera cuando se produce uno de los gestos suscritos. El evento [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped) se usará para restablecer el zoom a un fotograma completo. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

A continuación, declara un objeto **Rect** que almacenará el rectángulo de origen de zoom actual.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

El controlador **ManipulationDelta** ajusta la escala o la traslación del rectángulo de zoom. Si el valor de escala delta no es 1, significa que el usuario realizó un gesto de reducir. Si el valor es mayor que 1, el rectángulo de origen debe hacerse más pequeño para acercar el contenido. Si el valor es menor que 1, el rectángulo de origen debe hacerse más grande para alejarlo. Antes de establecer los nuevos valores de escala, se comprueba el rectángulo resultante para asegurarse de que se encuentra completamente dentro de los límites (0, 0, 1, 1).

Si el valor de escala es 1, se controla el gesto de traslación. Simplemente, el número de píxeles en el gesto dividido por el ancho y alto del control traslada el rectángulo. De nuevo, se comprueba el rectángulo resultante para asegurarse de que se encuentra dentro de los límites (0,0,1,1).

Por último, el objeto [**NormalizedSourceRect**](/uwp/api/windows.media.playback.mediaplaybacksession.normalizedsourcerect) del elemento **MediaPlaybackSession** se establece en el rectángulo recién ajustado y se especifica el área del fotograma de vídeo que se debe representar.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

En el controlador de eventos [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped), el rectángulo de origen se vuelve a establecer en (0,0,1,1) para hacer que se represente todo el fotograma de vídeo.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]

**Nota:** En esta sección se describe la entrada táctil. Touchpad envía eventos de puntero y no envía eventos de manipulación.

### <a name="handling-policy-based-playback-degradation"></a>Control de la degradación de la reproducción basada en directivas

En algunas circunstancias, el sistema puede degradar la reproducción de un elemento multimedia, como reducir la resolución (constriction), en función de una directiva en lugar de un problema de rendimiento. Por ejemplo, el vídeo puede verse afectado por el sistema si se está reproduciendo con un controlador de vídeo sin firmar. Puede llamar a [**MediaPlaybackSession. GetOutputDegradationPolicyState**](/uwp/api/windows.media.playback.mediaplaybacksession.getoutputdegradationpolicystate#Windows_Media_Playback_MediaPlaybackSession_GetOutputDegradationPolicyState) para determinar si se está produciendo esta degradación del basada en directivas y avisar al usuario o registrar el motivo de la telemetría.

En el ejemplo siguiente se muestra una implementación de un controlador para el evento **MediaPlayer. MediaOpened** que se genera cuando el reproductor abre un nuevo elemento multimedia. Se llama a **GetOutputDegradationPolicyState** en el control **MediaPlayer** pasado al controlador. El valor de [**VideoConstrictionReason**](/uwp/api/windows.media.playback.mediaplaybacksessionoutputdegradationpolicystate.videoconstrictionreason#Windows_Media_Playback_MediaPlaybackSessionOutputDegradationPolicyState_VideoConstrictionReason) indica el motivo de la Directiva que el vídeo está restringido. Si el valor no es **ninguno**, este ejemplo registra el motivo de la degradación de la telemetría. En este ejemplo también se muestra cómo establecer la velocidad de bits del **AdaptiveMediaSource** que se está reproduciendo actualmente en el ancho de banda más bajo para ahorrar el uso de datos, ya que el vídeo está restringido y no se mostrará con alta resolución. Para obtener más información sobre el uso de **AdaptiveMediaSource**, consulte [Adaptive streaming](adaptive-streaming.md).

[!code-cs[PolicyDegradation](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPolicyDegradation)]
        
## <a name="use-mediaplayersurface-to-render-video-to-a-windowsuicomposition-surface"></a>Usar MediaPlayerSurface para representar vídeo en una superficie Windows.UI.Composition
A partir de Windows 10, versión 1607, puedes usar **MediaPlayer** para representar vídeo en una superficie [**ICompositionSurface**](/uwp/api/Windows.UI.Composition.ICompositionSurface), que permite que el reproductor interopere con las API en el espacio de nombres [**Windows.UI.Composition**](/uwp/api/Windows.UI.Composition). El fotograma de composición te permite trabajar con gráficos en la capa visual entre XAML y las API de gráficos de DirectX de bajo nivel. Esto permite escenarios como la representación de vídeo en cualquier control XAML. Para obtener más información sobre cómo usar las API de composición, consulta [Capa visual](../composition/visual-layer.md).

En el siguiente ejemplo se muestra cómo representar el contenido del reproductor de vídeo en un control [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas). Las llamadas específicas del reproductor multimedia de este ejemplo son [**SetSurfaceSize**](/uwp/api/windows.media.playback.mediaplayer.setsurfacesize) y [**GetSurface**](/uwp/api/windows.media.playback.mediaplayer.getsurface). **SetSurfaceSize** indica al sistema el tamaño del búfer que se debe asignar para representar contenido. **GetSurface** toma una clase [**Compositor**](/uwp/api/Windows.UI.Composition.Compositor) como argumento y recupera una instancia de la clase [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface). Esta clase proporciona acceso a las clases **MediaPlayer** y **Compositor** usadas para crear la superficie y expone la misma superficie a través de la propiedad [**CompositionSurface**](/uwp/api/windows.media.playback.mediaplayersurface.compositionsurface).

El resto del código del ejemplo crea un objeto [**SpriteVisual**](/uwp/api/Windows.UI.Composition.SpriteVisual) por el que se representa el vídeo y establece el tamaño en el tamaño del elemento de lienzo que mostrará el elemento visual. A continuación, se crea un objeto [**CompositionBrush**](/uwp/api/Windows.UI.Composition.CompositionBrush) a partir de la clase [**MediaPlayerSurface**](/uwp/api/Windows.Media.Playback.MediaPlayerSurface) y se asigna a la propiedad [**Brush**](/uwp/api/windows.ui.composition.spritevisual.brush) del elemento visual. A continuación, se crea una clase [**ContainerVisual**](/uwp/api/Windows.UI.Composition.ContainerVisual) y se inserta un objeto **SpriteVisual** en la parte superior de su árbol visual. Por último, se llama a un objeto [**SetElementChildVisual**](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual) para asignar el elemento visual contenedor a la clase **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
## <a name="use-mediatimelinecontroller-to-synchronize-content-across-multiple-players"></a>Usa MediaTimelineController para sincronizar contenido entre varios reproductores.
Como se explicó anteriormente en este artículo, la aplicación puede tener varios objetos **MediaPlayer** activos a la vez. De manera predeterminada, cada objeto **MediaPlayer** que crees funciona de forma independiente. En algunos escenarios, como la sincronización de una pista de comentarios en un vídeo, es posible que quieras sincronizar el estado del reproductor, la posición de reproducción y la velocidad de reproducción de varios reproductores. A partir de Windows 10, versión 1607, puedes implementar este comportamiento usando la clase [**MediaTimelineController**](/uwp/api/Windows.Media.MediaTimelineController).

### <a name="implement-playback-controls"></a>Implementar controles de reproducción
En el siguiente ejemplo se muestra cómo usar un objeto **MediaTimelineController** para controlar dos instancias del elemento **MediaPlayer**. En primer lugar, se crea una instancia de cada instancia de la clase **MediaPlayer** y la propiedad **Source** se establece en un archivo multimedia. A continuación, se crea un objeto **MediaTimelineController** nuevo. Para cada objeto **MediaPlayer**, se deshabilita la clase [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) asociada a cada reproductor al establecer la propiedad [**IsEnabled**](/uwp/api/windows.media.playback.mediaplaybackcommandmanager.isenabled) en false. Y, a continuación, la propiedad [**TimelineController**](/uwp/api/windows.media.playback.mediaplayer.timelinecontroller) se establece en el objeto de controlador de la escala de tiempo.

[!code-cs[DeclareMediaTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaTimelineController)]

[!code-cs[SetTimelineController](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetTimelineController)]

**Precaución** La clase [**MediaPlaybackCommandManager**](/uwp/api/Windows.Media.Playback.MediaPlaybackCommandManager) proporciona integración automática entre **MediaPlayer** y los controles de transporte de contenido multimedia del sistema (SMTC), pero no se puede usar esta integración automática con reproductores multimedia que se controlen con un objeto **MediaTimelineController**. Por lo tanto, debes deshabilitar el administrador de comandos del reproductor multimedia antes de establecer el controlador de línea de tiempo del reproductor. Si no se hace, se generará una excepción con el siguiente mensaje: "Se bloqueó la adjunción del controlador de línea de tiempo multimedia debido al estado actual del objeto". Para obtener más información sobre la integración del reproductor multimedia con los SMTC, consulta [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md). Si estás usando un objeto **MediaTimelineController**, aún puedes controlar los SMTC manualmente. Para obtener más información, consulta [Controles de transporte de contenido multimedia del sistema](system-media-transport-controls.md).

Cuando hayas adjuntado un objeto **MediaTimelineController** a uno o más reproductores multimedia, puedes controlar el estado de reproducción con los métodos que expone el controlador. El siguiente ejemplo llama al método [**Start**](/uwp/api/windows.media.mediatimelinecontroller.start) para iniciar la reproducción de todos los reproductores multimedia adjuntos al inicio del elemento multimedia.

[!code-cs[PlayButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPlayButtonClick)]

En este ejemplo se muestra cómo pausar y reanudar todos los reproductores multimedia adjuntos.

[!code-cs[PauseButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPauseButtonClick)]

Para avanzar rápidamente todos los reproductores multimedia conectados, establece la velocidad de reproducción en un valor mayor que 1.

[!code-cs[FastForwardButtonClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFastForwardButtonClick)]

En el siguiente ejemplo se muestra cómo usar un **control deslizante** para mostrar la posición de reproducción actual del controlador de línea de tiempo relativo a la duración del contenido de uno de los reproductores multimedia conectados. En primer lugar, se crea un objeto **MediaSource** nuevo y se registra un controlador para el objeto [**OpenOperationCompleted**](/uwp/api/windows.media.core.mediasource.openoperationcompleted) de origen multimedia. 

[!code-cs[CreateSourceWithOpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCreateSourceWithOpenCompleted)]

El controlador **OpenOperationCompleted** se usa como una oportunidad para descubrir la duración del contenido de origen multimedia. Cuando se haya determinado la duración, el valor máximo del **control deslizante** se establece en el número total de segundos del elemento multimedia. El valor se establece dentro de una llamada al método [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para asegurarse de que se ejecute en el subproceso de la interfaz de usuario.

[!code-cs[DeclareDuration](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareDuration)]

[!code-cs[OpenCompleted](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenCompleted)]

A continuación, se registra un controlador para el evento de controlador de línea de tiempo [**PositionChanged**](/uwp/api/windows.media.mediatimelinecontroller.positionchanged). El sistema lo llama periódicamente, aproximadamente 4 veces por segundo.

[!code-cs[RegisterPositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPositionChanged)]

En el controlador del objeto **PositionChanged**, el valor del control deslizante se actualiza para reflejar la posición actual del controlador de la línea de tiempo.

[!code-cs[PositionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetPositionChanged)]

### <a name="offset-the-playback-position-from-the-timeline-position"></a>Desplazar la posición de reproducción desde la posición de línea de tiempo
En algunos casos, es posible que quieras que la posición de reproducción de uno o más reproductores multimedia asociados a un controlador de línea de tiempo se desplace desde los otros reproductores. Para ello, establece la propiedad [**TimelineControllerPositionOffset**](/uwp/api/windows.media.playback.mediaplayer.timelinecontrollerpositionoffset) del objeto **MediaPlayer** que quieras desplazar. En el siguiente ejemplo, se usan las duraciones del contenido de dos reproductores multimedia para establecer los valores mínimos y máximos de dos control deslizantes para aumentar y reducir la longitud del elemento.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

En el evento [**ValueChanged**](/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) de cada control deslizante, el objeto **TimelineControllerPositionOffset** de cada reproductor está establecido en el valor correspondiente.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Ten en cuenta que si el valor de desplazamiento de un reproductor se asigna a una posición de reproducción negativa, el clip permanecerá en pausa hasta que el desplazamiento llegue a cero y, a continuación, se iniciará la reproducción. Del mismo modo, si el valor de desplazamiento se asigna a una posición de reproducción mayor que la duración del elemento multimedia, se mostrará el último fotograma, igual que cuando un único reproductor multimedia alcanza el final de su contenido.

## <a name="play-spherical-video-with-mediaplayer"></a>Reproducir vídeo esférico con MediaPlayer
A partir de Windows 10, versión 1703, **MediaPlayer** admite la proyección equirectangular para la reproducción de vídeo esférica. El contenido de vídeo esférico no es diferente del vídeo normal y plano en **el que se representará** el vídeo, siempre y cuando se admita la codificación de vídeo. En el caso de vídeo esférico que contiene una etiqueta de metadatos que especifica que el vídeo usa la proyección equirectangular, **MediaPlayer** puede representar el vídeo con un campo de vista y una orientación de vista especificados. Esto permite escenarios como la reproducción de vídeo de realidad virtual con una pantalla montada por el cabezal o simplemente permite al usuario desplazarse por el contenido de vídeo esférico mediante la entrada del mouse o del teclado.

Para reproducir vídeo esférico, siga los pasos para reproducir el contenido de vídeo descrito anteriormente en este artículo. El paso adicional consiste en registrar un controlador para el evento [**MediaPlayer. MediaOpened**](/uwp/api/Windows.Media.Playback.MediaPlayer#Windows_Media_Playback_MediaPlayer_MediaOpened) . Este evento le ofrece la oportunidad de habilitar y controlar los parámetros de reproducción de vídeo esférico.

[!code-cs[OpenSphericalVideo](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOpenSphericalVideo)]

En el controlador **MediaOpened** , compruebe primero el formato de marco del elemento multimedia recién abierto comprobando la propiedad [**PlaybackSession. SphericalVideoProjection. FrameFormat**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.FrameFormat) . Si este valor es [**SphericaVideoFrameFormat. equirectangular**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), el sistema puede proyectar automáticamente el contenido del vídeo. En primer lugar, establezca la propiedad [**PlaybackSession. SphericalVideoProjection. IsEnabled**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.IsEnabled) en **true**. También puede ajustar propiedades como la orientación de la vista y el campo de vista que el reproductor de media usará para proyectar el contenido del vídeo. En este ejemplo, el campo de vista se establece en un valor ancho de 120 grados mediante el establecimiento de la propiedad [**HorizontalFieldOfViewInDegrees**](/uwp/api/windows.media.playback.mediaplaybacksphericalvideoprojection.HorizontalFieldOfViewInDegrees) .

Si el contenido del vídeo es esférico, pero está en un formato distinto de equirectangular, puede implementar su propio algoritmo de proyección mediante el modo de servidor de trama del reproductor de media para recibir y procesar fotogramas individuales.

[!code-cs[SphericalMediaOpened](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalMediaOpened)]

En el ejemplo de código siguiente se muestra cómo ajustar la orientación de la vista de vídeo esférica mediante las teclas de flecha izquierda y derecha.

[!code-cs[SphericalOnKeyDown](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalOnKeyDown)]

Si la aplicación admite listas de reproducción de vídeo, es posible que quiera identificar elementos de reproducción que contengan vídeo esférico en la interfaz de usuario. Las listas de reproducción multimedia se tratan en detalle en el artículo [elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). En el ejemplo siguiente se muestra cómo crear una nueva lista de reproducción, agregar un elemento y registrar un controlador para el evento [**MediaPlaybackItem. VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.VideoTracksChanged) , que se produce cuando se resuelven las pistas de vídeo para un elemento multimedia.

[!code-cs[SphericalList](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalList)]

En el controlador de eventos **VideoTracksChanged** , obtenga las propiedades de codificación de las pistas de vídeo agregadas llamando a [**videotrack. GetEncodingProperties**](/uwp/api/windows.media.core.videotrack.GetEncodingProperties). Si la propiedad [**SphericalVideoFrameFormat**](/uwp/api/windows.media.mediaproperties.videoencodingproperties.SphericalVideoFrameFormat) de las propiedades de codificación es un valor distinto de [**SphericaVideoFrameFormat. None**](/uwp/api/windows.media.mediaproperties.sphericalvideoframeformat), la pista de vídeo contiene vídeo esférico y puede actualizar la interfaz de usuario según corresponda si lo desea.

[!code-cs[SphericalTracksChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSphericalTracksChanged)]

## <a name="use-mediaplayer-in-frame-server-mode"></a>Usar MediaPlayer en modo de servidor de marco
A partir de Windows 10, versión 1703, puede usar **MediaPlayer** en modo de servidor de marco. En este modo, el objeto **MediaPlayer** no representa automáticamente fotogramas en un objeto **MediaPlayerElement**asociado. En su lugar, la aplicación copia el marco actual de **MediaPlayer** en un objeto que implementa [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). El escenario principal que esta característica permite es usar sombreadores de píxeles para procesar fotogramas de vídeo proporcionados por el **MediaPlayer**. La aplicación es responsable de mostrar cada fotograma después del procesamiento, como mostrar el marco en un control de [**imagen**](/uwp/api/windows.ui.xaml.controls.image) XAML.

En el ejemplo siguiente, se inicializa un nuevo **MediaPlayer** y se carga el contenido de vídeo. A continuación, se registra un controlador de [**VideoFrameAvailable**](/uwp/api/windows.media.playback.mediaplayer.VideoFrameAvailable) . El modo de servidor de trama se habilita estableciendo la propiedad [**IsVideoFrameServerEnabled**](/uwp/api/windows.media.playback.mediaplayer.IsVideoFrameServerEnabled) del objeto **MediaPlayer** en **true**. Por último, la reproducción multimedia se inicia con una llamada a [**Play**](/uwp/api/windows.media.playback.mediaplayer.Play).

[!code-cs[FrameServerInit](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetFrameServerInit)]

En el ejemplo siguiente se muestra un controlador para **VideoFrameAvailable** que usa [Win2D](https://github.com/Microsoft/Win2D) para agregar un efecto de desenfoque simple a cada fotograma de un vídeo y, a continuación, muestra los marcos procesados en un control de [imagen](/uwp/api/windows.ui.xaml.controls.image) XAML.

Cada vez que se llama al controlador **VideoFrameAvailable** , se usa el método [**CopyFrameToVideoSurface**](/uwp/api/windows.media.playback.mediaplayer.copyframetovideosurface) para copiar el contenido del marco en una [**IDirect3DSurface**](/uwp/api/windows.graphics.directx.direct3d11.idirect3dsurface). También puede usar [**CopyFrameToStereoscopicVideoSurfaces**](/uwp/api/windows.media.playback.mediaplayer.copyframetostereoscopicvideosurfaces) para copiar contenido 3D en dos superficies, para procesar el contenido del ojo izquierdo y el ojo secundario por separado. Para obtener un objeto que implementa **IDirect3DSurface**  en este ejemplo, se crea un objeto [**SoftwareBitmap**](/uwp/api/windows.graphics.imaging.softwarebitmap) y, a continuación, se usa para crear un **CanvasBitmap**de Win2D, que implementa la interfaz necesaria. Un **CanvasImageSource** es un objeto Win2D que se puede usar como origen para un control de **imagen** , por lo que se crea uno nuevo y se establece como el origen de la **imagen** en la que se mostrará el contenido. A continuación, se crea un **CanvasDrawingSession** . Win2D lo usa para presentar el efecto de desenfoque.

Una vez que se han creado instancias de todos los objetos necesarios, se llama a **CopyFrameToVideoSurface** , que copia el marco actual del objeto **MediaPlayer** en el **CanvasBitmap**. A continuación, se crea un **GaussianBlurEffect** de Win2D, con el **CanvasBitmap** establecido como el origen de la operación. Por último, se llama a **CanvasDrawingSession. drawImage** para dibujar la imagen de origen, con el efecto de desenfoque aplicado, en el **CanvasImageSource** que se ha asociado con el control de **imagen** , lo que provoca que se dibuje en la interfaz de usuario.

[!code-cs[VideoFrameAvailable](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetVideoFrameAvailable)]

Para obtener más información sobre Win2D, vea el [repositorio de github de win2d](https://github.com/Microsoft/Win2D). Para probar el código de ejemplo mostrado anteriormente, debe agregar el paquete NuGet de Win2D al proyecto con las siguientes instrucciones.

**Para agregar el paquete de NuGet Win2D al proyecto de efecto**

1.  En el **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y seleccione **Administrar paquetes NuGet**.
2.  En la parte superior de la ventana, selecciona la pestaña **Examinar**.
3.  En el cuadro de búsqueda, escribe **Win2D**.
4.  Selecciona **Win2D.uwp**y luego selecciona **Instalar** en el panel derecho.
5.  En el cuadro de diálogo **Revisar cambios** se muestra el paquete que se instalará. Haga clic en **Aceptar**.
6.  Acepta la licencia del paquete.

## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar y responder a los cambios de nivel de audio por parte del sistema
A partir de la versión 1803 de Windows 10, la aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de un **MediaPlayer**actualmente en reproducción. Por ejemplo, el sistema puede reducir o "pato" el nivel de reproducción de audio cuando se produce un timbre de alarma. El sistema silenciará la aplicación cuando pase a segundo plano si la aplicación no ha declarado la funcionalidad *backgroundMediaPlayback* en el manifiesto de la aplicación. La clase [**AudioStateMonitor**](./uwp/api/windows.media.audio.audiostatemonitor) permite registrarse para recibir un evento cuando el sistema modifica el volumen de una secuencia de audio. Acceda a la propiedad **AudioStateMonitor** de un control **MediaPlayer** y registre un controlador para que se notifique al evento [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) cuando el sistema cambie el nivel de audio de ese **MediaPlayer** .

[!code-cs[RegisterAudioStateMonitor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

Al controlar el evento **SoundLevelChanged** , puede realizar diferentes acciones en función del tipo de contenido que se reproduzca. Si actualmente está reproduciendo música, puede que desee dejar que la música continúe jugando mientras el volumen está pato. Sin embargo, si está reproduciendo un podcast, es probable que desee pausar la reproducción mientras el audio está pato, por lo que el usuario no pierde el contenido.

En este ejemplo se declara una variable para hacer un seguimiento de si el contenido que se está reproduciendo actualmente es un podcast, se supone que se establece en el valor adecuado al seleccionar el contenido de la **MediaPlayer**. También se crea una variable de clase para realizar el seguimiento cuando se pausa la reproducción mediante programación cuando cambia el nivel de audio.

[!code-cs[AudioStateVars](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

En el controlador de eventos **SoundLevelChanged** , Compruebe la propiedad [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) del remitente **AudioStateMonitor** para determinar el nuevo nivel de sonido. En este ejemplo se comprueba si el nuevo nivel de sonido es un volumen completo, lo que significa que el sistema ha dejado de silenciar o no el volumen, o si se ha reducido el nivel de sonido pero está reproduciendo contenido que no es de podcast. Si alguna de estas condiciones es verdadera y el contenido se pausó previamente mediante programación, se reanuda la reproducción. Si el nuevo nivel de sonido está silenciado o si el contenido actual es un podcast y el nivel de sonido es bajo, se pausa la reproducción y se establece la variable para hacer un seguimiento de que la pausa se inició mediante programación.

[!code-cs[SoundLevelChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSoundLevelChanged)]

El usuario puede decidir que desea pausar o continuar la reproducción, incluso si el sistema está pato en el audio. Este ejemplo muestra los controladores de eventos para un botón reproducir y pausa. En el botón pausar, el controlador de clic está en pausa y, si ya se ha pausado la reproducción mediante programación, la variable se actualiza para indicar que el usuario ha pausado el contenido. En el controlador de clic del botón reproducir, se reanuda la reproducción y se borra la variable de seguimiento.

[!code-cs[ButtonUserClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetButtonUserClick)]

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Crear, programar y administrar interrupciones multimedia](create-schedule-and-manage-media-breaks.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md)





 

 