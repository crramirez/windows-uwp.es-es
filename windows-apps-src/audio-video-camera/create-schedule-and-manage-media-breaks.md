---
ms.assetid: 0309c7a1-8e4c-4326-813a-cbd9f8b8300d
description: En este artículo se muestra cómo crear, programar y administrar las interrupciones multimedia en tu aplicación de reproducción de contenido multimedia.
title: Crear, programar y administrar interrupciones multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d9743b3c82b935029eeb0ee4c5b9347251fc2
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363978"
---
# <a name="create-schedule-and-manage-media-breaks"></a>Crear, programar y administrar interrupciones multimedia

En este artículo se muestra cómo crear, programar y administrar las interrupciones multimedia en tu aplicación de reproducción de contenido multimedia. Las interrupciones multimedia se suelen usar para insertar anuncios de audio o vídeo en el contenido multimedia. A partir de Windows 10, versión 1607, puedes usar la clase [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) para agregar interrupciones multimedia a cualquier elemento [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) que reproduzcas con [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) de forma rápida y fácil.


Después de programar una o más interrupciones multimedia, el sistema reproducirá automáticamente tu contenido multimedia en el momento especificado durante la reproducción. **MediaBreakManager** proporciona eventos para que la aplicación pueda reaccionar cuando se inicien las interrupciones multimedia, cuando finalicen o cuando el usuario las omita. También puedes acceder a una [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) para que tus interrupciones multimedia supervisen eventos como la descarga y el almacenamiento en búfer de actualizaciones de progreso.

## <a name="schedule-media-breaks"></a>Programar interrupciones multimedia
Cada objeto **MediaPlaybackItem** tiene su propio [**MediaBreakSchedule**](/uwp/api/Windows.Media.Playback.MediaBreakSchedule) que usas para configurar las interrupciones multimedia que se reproducirán cuando lo haga el elemento. El primer paso para usar interrupciones multimedia en tu aplicación es crear un elemento [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) para el contenido de reproducción principal. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMoviePlaybackItem":::

Para obtener más información sobre cómo trabajar con **MediaPlaybackItem**, [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) y otras API de reproducción multimedia fundamentales, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

El siguiente ejemplo muestra cómo agregar una interrupción de preprocesamiento a **MediaPlaybackItem**, lo que significa que el sistema reproducirá la interrupción multimedia antes de reproducir el elemento de reproducción al que pertenece la interrupción. Primero se crea una instancia de un nuevo objeto [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak). En este ejemplo, se llama al constructor con [**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), lo que significa que el contenido principal se pondrá en pausa mientras se reproduce el contenido de la interrupción. 

Luego, se crea un nuevo **MediaPlaybackItem** para el contenido que se reproducirá durante la interrupción, por ejemplo, un anuncio. La propiedad [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) de este elemento de reproducción se establece en false. Esto significa que el usuario no podrá omitir el elemento mediante los controles multimedia integrados. Tu aplicación puede seguir optando por omitir el anuncio mediante programación llamando a [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak). 

La propiedad [**PlaybackList**](/uwp/api/windows.media.playback.mediabreak.playbacklist) de la interrupción multimedia es un elemento [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) que te permite reproducir varios elementos multimedia como una lista de reproducción. Agrega uno o más objetos **MediaPlaybackItem** de la colección de **elementos** de la lista para incluirlos en la lista de reproducción de la interrupción multimedia.

Por último, programa la interrupción multimedia mediante la propiedad [**BreakSchedule**](/uwp/api/windows.media.playback.mediaplaybackitem.breakschedule) del elemento de reproducción de contenido principal. Especifica que la interrupción sea una interrupción de preprocesamiento asignándola a la propiedad [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) del objeto de programación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPreRollBreak":::

Ahora puedes reproducir el elemento multimedia principal y se reproducirá la interrupción multimedia que creaste antes del contenido principal. Crea un nuevo objeto [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) y, opcionalmente, establece la propiedad [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) en true para iniciar automáticamente la reproducción. Establece la propiedad [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) de **MediaPlayer** en tu elemento de reproducción de contenido principal. No es necesario, pero puedes asignar **MediaPlayer** en un elemento [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) para representar el contenido multimedia en una página XAML. Para obtener más información sobre cómo usar **MediaPlayer**, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

Agrega una interrupción de postprocesamiento que se reproduce después de que el elemento **MediaPlaybackItem** que contiene el contenido principal finalice su reproducción, usando la misma técnica de la interrupción de preprocesamiento, salvo que asignes tu objeto **MediaBreak** a la propiedad [**PostrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.postrollbreak).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetPostRollBreak":::

También puedes programar una o más interrupciones intermedias que se reproducen en un momento determinado dentro de la reproducción del contenido principal. En el siguiente ejemplo, [**MediaBreak**](/uwp/api/Windows.Media.Playback.MediaBreak) se crea con la sobrecarga del constructor que acepta un objeto **TimeSpan** , que especifica el momento en la reproducción del elemento multimedia principal en el que se reproducirá la interrupción. De nuevo, [**MediaBreakInsertionMethod.Interrupt**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod) se especifica para indicar que la reproducción del contenido principal se pondrá en pausa mientras se reproduce la interrupción. La interrupción intermedia se agrega a la programación llamando a [**InsertMidrollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.insertmidrollbreak). Puedes obtener una lista de solo lectura de las interrupciones intermedias actuales en la programación accediendo a la propiedad [**MidrollBreaks**](/uwp/api/windows.media.playback.mediabreakschedule.midrollbreaks).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak":::

El siguiente ejemplo de interrupción intermedia mostrado usa el método de inserción [**MediaBreakInsertionMethod.Replace**](/uwp/api/Windows.Media.Playback.MediaBreakInsertionMethod), lo que significa que el sistema seguirá procesando el contenido principal mientras se reproduce la interrupción. Esta opción la suelen usar las aplicaciones multimedia de streaming en vivo en las que no se quiere que el contenido se ponga en pausa y se pierda el streaming en vivo mientras se reproduce el anuncio. 

Este ejemplo también usa una sobrecarga del constructor [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) que acepta dos parámetros [**TimeSpan**](/uwp/api/Windows.Foundation.TimeSpan). El primer parámetro especifica el punto de partida dentro del elemento de interrupción multimedia en el que se iniciará la reproducción. El segundo parámetro especifica la duración de la reproducción del elemento de interrupción multimedia. Por tanto, en el siguiente ejemplo, **MediaBreak** empezará a reproducirse a los 20 minutos dentro del contenido principal. Durante la reproducción, el elemento multimedia se iniciará en 30 segundos desde el inicio del elemento de interrupción multimedia y se reproducirá durante 15 segundos antes de que el contenido multimedia principal reanude la reproducción.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetMidrollBreak2":::

## <a name="skip-media-breaks"></a>Omitir interrupciones multimedia
Como se mencionó anteriormente en este artículo, la propiedad [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) de **MediaPlaybackItem** se puede establecer para impedir que el usuario omita el contenido con los controles integrados. Sin embargo, puedes llamar a [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak) desde el código en cualquier momento para omitir la interrupción actual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetSkipButtonClick":::

## <a name="handle-mediabreak-events"></a>Controlar eventos MediaBreak

Hay varios eventos relacionados con las interrupciones multimedia que se pueden registrar para poder tomar medidas basadas en el estado cambiante de las interrupciones multimedia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterMediaBreakEvents":::

[**BreakStarted**](/uwp/api/windows.media.playback.mediabreakmanager.breakstarted) se genera cuando se inicia una interrupción multimedia. Puedes actualizar la interfaz de usuario para que el usuario sepa que se está reproduciendo contenido de interrupción multimedia. En este ejemplo se usa [**MediaBreakStartedEventArgs**](/uwp/api/Windows.Media.Playback.MediaBreakStartedEventArgs) pasado al controlador para obtener una referencia a la interrupción multimedia que se inició. La propiedad [**CurrentItemIndex**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemindex) se usa para determinar qué elemento multimedia se está reproduciendo en la lista de reproducción de la interrupción multimedia. A continuación, se actualiza la interfaz de usuario para mostrar al usuario el índice de anuncios actual y el número de anuncios restantes en la interrupción. Recuerda que las actualizaciones de la interfaz de usuario deben realizarse en el subproceso de interfaz de usuario, por lo que la llamada debe realizarse dentro de una llamada a [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync). 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakStarted":::

[**BreakEnded**](/uwp/api/windows.media.playback.mediabreakmanager.breakended) se genera cuando haya finalizado la reproducción de todos los elementos multimedia de la interrupción o se hayan omitido. Puedes usar el controlador para este evento con el fin de actualizar la interfaz de usuario para indicar que ya no se está reproduciendo contenido de interrupción multimedia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakEnded":::

El evento **BreakSkipped** se genera cuando el usuario presiona el botón *Siguiente* en la interfaz de usuario integrada durante la reproducción de un elemento para el que [**CanSkip**](/uwp/api/windows.media.playback.mediaplaybackitem.canskip) sea true, o cuando omitas una interrupción en el código mediante una llamada a [**SkipCurrentBreak**](/uwp/api/windows.media.playback.mediabreakmanager.skipcurrentbreak).

El siguiente ejemplo usa la propiedad [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) de **MediaPlayer** para obtener una referencia al elemento multimedia para el contenido principal. La interrupción multimedia omitida pertenece a la programación de interrupción de este elemento. A continuación, el código comprueba si la interrupción multimedia omitida es la misma que la interrupción multimedia establecida en la propiedad [**PrerollBreak**](/uwp/api/windows.media.playback.mediabreakschedule.prerollbreak) de la programación. Si es así, esto significa que la interrupción de preprocesamiento era la interrupción omitida y, en este caso, se crea y se programa una nueva interrupción intermedia para reproducirse durante 10 minutos en el contenido principal.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSkipped":::

[**BreaksSeekedOver**](/uwp/api/windows.media.playback.mediabreakmanager.breaksseekedover) se genera cuando la posición de la reproducción del elemento multimedia principal sobrepasa el tiempo programado de una o más interrupciones multimedia. En el siguiente ejemplo se comprueba si se buscó más de una interrupción multimedia, si la posición de la reproducción se movió hacia adelante y si se movió hacia adelante menos de 10 minutos. Si es así, la primera interrupción que se buscó, obtenida a partir de la colección [**SeekedOverBreaks**](/uwp/api/windows.media.playback.mediabreakseekedovereventargs.seekedoverbreaks) expuesta por los argumentos del evento, se reproduce inmediatamente con una llamada al método [**PlayBreak**](/uwp/api/windows.media.playback.mediabreakmanager.playbreak) de **MediaPlayer.BreakManager**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBreakSeekedOver":::


## <a name="access-the-current-playback-session"></a>Acceso a la sesión de reproducción actual
El objeto [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) usa la clase **MediaPlayer** para proporcionar datos y eventos relacionados con el contenido multimedia que se esté reproduciendo actualmente. [**MediaBreakManager**](/uwp/api/Windows.Media.Playback.MediaBreakManager) también tiene una **MediaPlaybackSession** a la que puedes acceder para obtener datos y eventos específicamente relacionados con el contenido de la interrupción multimedia que se está reproduciendo. La información a la que puedes acceder desde la sesión de reproducción incluye el estado de reproducción actual, en reproducción o en pausa, y la posición de reproducción actual dentro del contenido. Puedes usar las propiedades [**NaturalVideoWidth**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideowidth) y [**NaturalVideoHeight**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideoheight), y el elemento [**NaturalVideoSizeChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.naturalvideosizechanged) para ajustar la interfaz de usuario de vídeo si el contenido de la interrupción multimedia tiene una relación de aspecto diferente a la del contenido principal. También puedes recibir eventos como [**BufferingStarted**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingstarted), [**BufferingEnded**](/uwp/api/windows.media.playback.mediaplaybacksession.bufferingended) y [**DownloadProgressChanged**](/uwp/api/windows.media.playback.mediaplaybacksession.downloadprogresschanged), que pueden proporcionar una telemetría valiosa sobre el rendimiento de la aplicación.

En el siguiente ejemplo se registra un controlador para **evento BufferingProgressChanged**; en el controlador de eventos, actualiza la interfaz de usuario para mostrar el progreso de almacenamiento en búfer actual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterBufferingProgressChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaBreaks_RS1/cs/MainPage.xaml.cs" id="SnippetBufferingProgressChanged":::

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Control manual de los controles de transporte de contenido multimedia del sistema](system-media-transport-controls.md)

 

 
