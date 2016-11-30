---
author: drewbatgit
ms.assetid: 
description: "En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con MediaPlayer."
title: "Reproducir audio y vídeo con MediaPlayer"
translationtype: Human Translation
ms.sourcegitcommit: 34cb2fec3071add8617fe2bee2eaf50356611ac6
ms.openlocfilehash: 66240809d47247312d9d4c49c7bf36ff70295559

---

# Reproducir audio y vídeo con MediaPlayer

En este artículo se muestra cómo reproducir elementos multimedia en la aplicación universal de Windows con la clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Con Windows 10, versión 1607, se realizaron mejoras significativas en las API de reproducción de elementos multimedia, como un diseño de proceso único simplificado para el audio en segundo plano, la integración automática con controles de transporte multimedia del sistema (SMTC), la capacidad de sincronizar varios reproductores multimedia de audio y de reproducir en una superficie Windows.UI.Composition y una interfaz sencilla para crear y programar interrupciones multimedia en tu contenido. Para aprovechar estas mejoras, el procedimiento recomendado para reproducir elementos multimedia es usar la clase **MediaPlayer** en lugar de **MediaElement** para reproducir elementos multimedia. El control de XAML ligero, [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement), se introdujo para permitir representar contenido multimedia en una página XAML. Muchas API de estado y de control de la reproducción que proporciona la clase **MediaElement** están disponibles a través del nuevo objeto [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). **La clase MediaElement** continúa en funcionamiento para permitir la compatibilidad con versiones anteriores, pero no se le agregarán nuevas características.

En este artículo se muestran las características de **MediaPlayer** que usará una aplicación típica de reproducción de elementos multimedia. Ten en cuenta que **MediaPlayer** usa la clase [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) como un contenedor de todos los elementos multimedia. Esta clase permite cargar y reproducir elementos multimedia de distintos orígenes, como archivos locales, secuencias de memoria y orígenes de redes, todo con la misma interfaz. También hay clases de nivel superior que funcionan con **MediaSource**, como [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) y [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList), que proporcionan características más avanzadas, como listas de reproducción y la capacidad para administrar orígenes multimedia con varias pistas de audio, vídeo y metadatos. Para obtener más información sobre **MediaSource** y las API relacionadas, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).


##Reproducir un archivo multimedia con MediaPlayer  
La reproducción básica de elementos multimedia con **MediaPlayer** es muy fácil de implementar. En primer lugar, crea una nueva instancia de la clase **MediaPlayer**. Tu aplicación puede tener varias instancias de **MediaPlayer** activas a la vez. Después, establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) del reproductor en un objeto que implemente [**IMediaPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.IMediaPlaybackSource), como un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem) o [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList). En este ejemplo, un objeto **MediaSource** se crea a partir de un archivo en el almacenamiento local de la aplicación, y después se crea un objeto **MediaPlaybackItem** desde el origen y, se asigna a la propiedad **Source** del reproductor.

A diferencia de la clase **MediaElement**, la clase **MediaPlayer** no inicia automáticamente la reproducción de manera predeterminada. Para iniciar la reproducción, llama al método [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play), establece la propiedad [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) en true o espera a que el usuario inicie la reproducción con los controles multimedia integrados.

[!code-cs[SimpleFilePlayback](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSimpleFilePlayback)]

Cuando la aplicación haya terminado de usar una clase **MediaPlayer**, debes llamar al método [**Close**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Close) (previsto para **Dispose** en C#), para liberar los recursos usados por el reproductor.

[!code-cs[CloseMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCloseMediaPlayer)]

##Usar MediaPlayerElement para representar vídeo en XAML
Puedes reproducir elementos multimedia en una clase **MediaPlayer** sin que se muestren en XAML, pero muchas aplicaciones de reproducción multimedia querrán representar el contenido multimedia en una página XAML. Para ello, usa el control de [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) ligero. Como **MediaElement**, el elemento **MediaPlayerElement** te permite especificar si se deben mostrar los controles de transporte integrados.

[!code-xml[MediaPlayerElementXAML](./code/MediaPlayer_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Para establecer la instancia **MediaPlayer** con la que está enlazado el elemento, llama a [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764).

[!code-cs[SetMediaPlayer](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetMediaPlayer)]

También puedes establecer el origen de reproducción en el elemento **MediaPlayerElement** y este creará automáticamente una nueva instancia de **MediaPlayer** a la que podrás acceder con la propiedad [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.MediaPlayer).

[!code-cs[GetPlayerFromElement](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetGetPlayerFromElement)]

> [!NOTE] 
> Si deshabilitas la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) de [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) al establecer la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en "false", se interrumpirá el vínculo entre **MediaPlayer** y [**TransportControls**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.TransportControls) que proporciona el elemento **MediaPlayerElement**, de modo que los controles de transporte integrados ya no controlarán automáticamente la reproducción del reproductor. En su lugar, debes implementar tus propios controles para controlar la clase **MediaPlayer**.

##Tareas comunes de MediaPlayer
En esta sección se muestra cómo usar algunas de las características de la clase **MediaPlayer**.

###Establecer la categoría de audio
Establece la propiedad [**AudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioCategory) de una clase **MediaPlayer** en uno de los valores de la enumeración [**MediaPlayerAudioCategory**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerAudioCategory) para que el sistema sepa qué tipo de elemento multimedia se está reproduciendo. Los juegos deben clasificar sus secuencias de música como **GameMedia** para que la música del juego se silencie automáticamente si otra aplicación reproduce música en segundo plano. Las aplicaciones de música o vídeo deben clasificar sus secuencias como **Media** o **Movie** para que tengan prioridad sobre las secuencias **GameMedia**.

[!code-cs[SetAudioCategory](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioCategory)]

###Salida a un punto de conexión de audio específico
De manera predeterminada, la salida de audio de una clase **MediaPlayer** se enruta hacia el punto de conexión de audio predeterminado para el sistema, pero puedes especificar un punto de conexión de audio que la clase **MediaPlayer** deba usar para la salida. En el ejemplo siguiente, el elemento [**MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Devices.MediaDevice.GetAudioRenderSelector) devuelve una cadena que exclusivamente identifica la categoría de representación de audio de los dispositivos. A continuación, se llama al método [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation) [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceInformation.FindAllAsync) para obtener una lista de todos los dispositivos disponibles del tipo seleccionado. Se puede determinar mediante programación el dispositivo que quieres usar o agregar los dispositivos devueltos a un control [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para permitir al usuario seleccionar un dispositivo.

[!code-cs[SetAudioEndpointEnumerate](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpointEnumerate)]

En el evento [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) del cuadro combinado de dispositivos, se establece la propiedad [**AudioDevice**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AudioDevice) de la clase **MediaPlayer** en el dispositivo seleccionado, que se guardó en la propiedad [**Tag**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.FrameworkElement.Tag) del elemento **ComboBoxItem**.

[!code-cs[SetAudioEndpontSelectionChanged](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSetAudioEndpontSelectionChanged)]

###Sesión de reproducción
Como se describió anteriormente en este artículo, muchas de las funciones que expone la clase **MediaElement** se movieron a la clase [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession). Esto incluye información sobre el estado de reproducción del reproductor, como la posición de reproducción actual, si está en pausa o en reproducción y la velocidad de reproducción actual. **MediaPlaybackSession** también proporciona varios eventos para notificarte si cambia el estado, como el almacenamiento en búfer actual y el estado de descarga del contenido que se reproduce, y el tamaño natural y la relación de aspecto del contenido de vídeo que se reproduce actualmente.

El siguiente ejemplo muestra cómo implementar un controlador de clic del botón que salta 10 segundos adelante en el contenido. Primero, se recupera el objeto **MediaPlaybackSession** del reproductor con la propiedad [**PlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.PlaybackSession). A continuación, se establece la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) en la posición de reproducción actual más 10 segundos.

[!code-cs[SkipForwardClick](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSkipForwardClick)]

En el siguiente ejemplo se muestra cómo usar un botón de alternancia para alternar entre la velocidad de reproducción normal y la velocidad 2X con la configuración de la propiedad [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.PlaybackRate) de la sesión.

[!code-cs[SpeedChecked](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetSpeedChecked)]

###Acercar los dedos y hacer zoom de vídeo
**MediaPlayer** te permite especificar el rectángulo de origen con el contenido de vídeo que se debe representar y permite hacer zoom en el vídeo de forma eficaz. El rectángulo que especifiques es relativo a un rectángulo normalizado (0,0,1,1), donde 0,0 es la parte superior izquierda del fotograma y 1,1 especifica el ancho y alto total del fotograma. Por lo tanto, por ejemplo, para establecer el rectángulo de zoom para que se represente el cuadrante superior derecho del vídeo, se especifica el rectángulo (.5,0,.5,.5).  Es importante comprobar los valores para asegurarte de que el rectángulo de origen está dentro del rectángulo normalizado (0,0,1,1). Intentar establecer un valor fuera de este intervalo hará que se inicie una excepción.

Para implementar la opción de acercar los dedos y hacer zoom con gestos multitoque, antes debes especificar qué gestos quieres admitir. En este ejemplo, se solicitan los gestos ampliar y trasladar. El evento [**ManipulationDelta**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.ManipulationDelta) se genera cuando se produce uno de los gestos suscritos. El evento [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped) se usará para restablecer el zoom a un fotograma completo. 

[!code-cs[RegisterPinchZoomEvents](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetRegisterPinchZoomEvents)]

A continuación, declara un objeto **Rect** que almacenará el rectángulo de origen de zoom actual.

[!code-cs[DeclareSourceRect](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDeclareSourceRect)]

El controlador **ManipulationDelta** ajusta la escala o la traslación del rectángulo de zoom. Si el valor de escala delta no es 1, significa que el usuario realizó un gesto de reducir. Si el valor es mayor que 1, el rectángulo de origen debe hacerse más pequeño para acercar el contenido. Si el valor es menor que 1, el rectángulo de origen debe hacerse más grande para alejar. Antes de configurar los nuevos valores de escala, se comprueba el rectángulo resultante para asegurarse de que quede por completo dentro de los límites (0,0,1,1).

Si el valor de escala es 1, se controla el gesto de traslación. Simplemente, el número de píxeles en el gesto dividido por el ancho y alto del control traslada el rectángulo. De nuevo, se comprueba el rectángulo resultante para asegurarse de que se encuentra dentro de los límites (0,0,1,1).

Por último, el objeto [**NormalizedSourceRect**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.NormalizedSourceRect) del elemento **MediaPlaybackSession** se establece en el rectángulo recién ajustado y se especifica el área del fotograma de vídeo que se debe representar.

[!code-cs[ManipulationDelta](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetManipulationDelta)]

En el controlador de eventos [**DoubleTapped**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.UIElement.DoubleTapped), el rectángulo de origen se vuelve a establecer en (0,0,1,1) para hacer que se represente todo el fotograma de vídeo.

[!code-cs[DoubleTapped](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetDoubleTapped)]
        
##Usar MediaPlayerSurface para representar vídeo en una superficie Windows.UI.Composition
A partir de Windows 10, versión 1607, puedes usar **MediaPlayer** para representar vídeo en una superficie [**ICompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ICompositionSurface), que permite que el reproductor interopere con las API en el espacio de nombres [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition). El fotograma de composición te permite trabajar con gráficos en la capa visual entre XAML y las API de gráficos de DirectX de bajo nivel. Esto permite escenarios como la representación de vídeo en cualquier control XAML. Para obtener más información sobre cómo usar las API de composición, consulta [Capa visual](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer).

En el siguiente ejemplo se muestra cómo representar el contenido del reproductor de vídeo en un control [**Canvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Canvas). Las llamadas específicas del reproductor multimedia de este ejemplo son [**SetSurfaceSize**](https://msdn.microsoft.com/library/windows/apps/mt489968) y [**GetSurface**](https://msdn.microsoft.com/library/windows/apps/mt489963). **SetSurfaceSize** indica al sistema el tamaño del búfer que se debe asignar para representar contenido. **GetSurface** toma una clase [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.Compositor) como argumento y recupera una instancia de la clase [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface). Esta clase proporciona acceso a las clases **MediaPlayer** y **Compositor** usadas para crear la superficie y expone la misma superficie a través de la propiedad [**CompositionSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface.CompositionSurface).

El resto del código del ejemplo crea un objeto [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual) por el que se representa el vídeo y establece el tamaño en el tamaño del elemento de lienzo que mostrará el elemento visual. A continuación, se crea un objeto [**CompositionBrush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.CompositionBrush) a partir de la clase [**MediaPlayerSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayerSurface) y se asigna a la propiedad [**Brush**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.SpriteVisual.Brush) del elemento visual. A continuación, se crea una clase [**ContainerVisual**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Composition.ContainerVisual) y se inserta un objeto **SpriteVisual** en la parte superior de su árbol visual. Por último, se llama a un objeto [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/mt608981) para asignar el elemento visual contenedor a la clase **Canvas**.

[!code-cs[Compositor](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetCompositor)]
        
##Usa MediaTimelineController para sincronizar contenido entre varios reproductores.
Como se explicó anteriormente en este artículo, la aplicación puede tener varios objetos **MediaPlayer** activos a la vez. De manera predeterminada, cada objeto **MediaPlayer** que crees funciona de forma independiente. En algunos escenarios, como la sincronización de una pista de comentarios en un vídeo, es posible que quieras sincronizar el estado del reproductor, la posición de reproducción y la velocidad de reproducción de varios reproductores. A partir de Windows 10, versión 1607, puedes implementar este comportamiento usando la clase [**MediaTimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaTimelineController).

###Implementar controles de reproducción
En el siguiente ejemplo se muestra cómo usar un objeto **MediaTimelineController** para controlar dos instancias del elemento **MediaPlayer**. En primer lugar, se crea una instancia de cada instancia de la clase **MediaPlayer** y la propiedad **Source** se establece en un archivo multimedia. A continuación, se crea un objeto **MediaTimelineController** nuevo. Para cada objeto **MediaPlayer**, se deshabilita la clase [**MediaPlaybackCommandManager**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager) asociada a cada reproductor al establecer la propiedad [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackCommandManager.IsEnabled) en false. Y, a continuación, se establece la propiedad [**TimelineController**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineController) en el objeto controlador de línea de tiempo.

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

###Desplazar la posición de reproducción desde la posición de línea de tiempo
En algunos casos, es posible que quieras que la posición de reproducción de uno o más reproductores multimedia asociados a un controlador de línea de tiempo se desplace desde los otros reproductores. Para ello, establece la propiedad [**TimelineControllerPositionOffset**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.TimelineControllerPositionOffset) del objeto **MediaPlayer** que quieras desplazar. En el siguiente ejemplo, se usan las duraciones del contenido de dos reproductores multimedia para establecer los valores mínimos y máximos de dos control deslizantes para aumentar y reducir la longitud del elemento.  

[!code-cs[OffsetSliders](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetOffsetSliders)]

En el evento [**ValueChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.RangeBase.ValueChanged) de cada control deslizante, el objeto **TimelineControllerPositionOffset** de cada reproductor está establecido en el valor correspondiente.

[!code-cs[TimelineOffset](./code/MediaPlayer_RS1/cs/MainPage.xaml.cs#SnippetTimelineOffset)]

Ten en cuenta que si el valor de desplazamiento de un reproductor se asigna a una posición de reproducción negativa, el clip permanecerá en pausa hasta que el desplazamiento llegue a cero y, a continuación, se iniciará la reproducción. Del mismo modo, si el valor de desplazamiento se asigna a una posición de reproducción mayor que la duración del elemento multimedia, se mostrará el último fotograma, igual que cuando un único reproductor multimedia alcanza el final de su contenido.

## Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Crear, programar y administrar interrupciones multimedia](create-schedule-and-manage-media-breaks.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md)





 

 







<!--HONumber=Nov16_HO1-->


