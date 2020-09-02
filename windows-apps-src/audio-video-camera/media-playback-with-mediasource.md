---
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: En este artículo se muestra cómo usar la clase MediaSource, que proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde diferentes orígenes (como archivos locales o remotos) y se expone un modelo común para acceder a datos multimedia, independientemente del formato multimedia subyacente.
title: Elementos multimedia, listas de reproducción y pistas
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f1f428bb8beb4bb933387a77e5a74819016a4c64
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363888"
---
# <a name="media-items-playlists-and-tracks"></a>Elementos multimedia, listas de reproducción y pistas


 En este artículo se muestra cómo usar la clase [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource), que proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde diferentes orígenes (como archivos locales o remotos) y se expone un modelo común para acceder a datos multimedia, independientemente del formato multimedia subyacente. La clase [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) amplía la funcionalidad del objeto **MediaSource**, lo que permite administrar y seleccionar entre varias pistas de audio, vídeo y metadatos incluidos en un elemento multimedia. [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) permite crear listas de reproducción de uno o más elementos de reproducción de contenido multimedia.


## <a name="create-and-play-a-mediasource"></a>Crear y reproducir un objeto MediaSource

Crea una nueva instancia del objeto **MediaSource** con una llamada a uno de los métodos de fábrica expuestos por la clase:

-   [**CreateFromAdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.createfromadaptivemediasource)
-   [**CreateFromIMediaSource**](/uwp/api/windows.media.core.mediasource.createfromimediasource)
-   [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource)
-   [**CreateFromMseStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommsestreamsource)
-   [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile)
-   [**CreateFromStream**](/uwp/api/windows.media.core.mediasource.createfromstream)
-   [**CreateFromStreamReference**](/uwp/api/windows.media.core.mediasource.createfromstreamreference)
-   [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri)
-   [**CreateFromDownloadOperation**](/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Después de crear un objeto **MediaSource**, puedes reproducirlo con una clase [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) estableciendo la propiedad [**Source**](/uwp/api/windows.media.playback.mediaplayer.source). A partir de Windows 10, versión 1607, se puede asignar un objeto **MediaPlayer** a una clase [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) mediante una llamada al método [**SetMediaPlayer**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.setmediaplayer) para representar el contenido del reproductor multimedia en una página XAML. Este es el método preferido en comparación con el uso de **MediaElement**. Para obtener más información sobre cómo usar **MediaPlayer**, consulta [**Reproducir audio y vídeo con MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

En el siguiente ejemplo se muestra cómo reproducir un archivo multimedia seleccionado por el usuario en un objeto **MediaPlayer** con **MediaSource**.

Para completar este escenario, tendrás que incluir los espacios de nombres [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) y [**Windows.Media.Playback**](/uwp/api/Windows.Media.Playback).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetUsing":::

Declara una variable de tipo **MediaSource**. Para los ejemplos de este artículo, el origen de contenido multimedia se declara como un miembro de clase para que se pueda acceder a él desde varias ubicaciones.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSource":::

Declara una variable para almacenar el objeto **MediaPlayer** y, si quieres representar el contenido multimedia en XAML, agrega un control **MediaPlayerElement** a la página.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlayer":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMediaPlayerElement":::

Para permitir al usuario seleccionar un archivo multimedia para su reproducción, usa una clase [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker). Con el objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) devuelto desde el método [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) del selector, inicializa un nuevo objeto MediaObject con una llamada a [**MediaSource.CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile). Por último, establece el origen del contenido multimedia como el origen de reproducción del objeto **MediaElement** con una llamada al método [**SetPlaybackSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.setplaybacksource).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaSource":::

De manera predeterminada, el objeto **MediaPlayer** no comienza a reproducirse automáticamente cuando se establece el origen del contenido multimedia. Puedes comenzar la reproducción de forma manual con una llamada al método [**Play**](/uwp/api/windows.media.playback.mediaplayer.play).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlay":::

También puedes establecer la propiedad [**AutoPlay**](/uwp/api/windows.media.playback.mediaplayer.autoplay) de **MediaPlayer** en true para indicar al reproductor que comience la reproducción en cuanto se establezca el origen del contenido multimedia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAutoPlay":::

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Crear un MediaSource desde un DownloadOperation
A partir de Windows, versión 1803, puede crear un objeto **MediaSource** a partir de un **DownloadOperation**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceFromDownload":::

Tenga en cuenta que, si bien puede crear un **MediaSource** a partir de una descarga sin iniciarlo o establecer su propiedad **IsRandomAccessRequired** en true, debe hacer ambas cosas antes de intentar adjuntar el **MediaSource** a un **MediaPlayer** o **MediaPlayerElement** para su reproducción.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetStartDownload":::


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Controlar varias pistas de audio, vídeo y metadatos con MediaPlaybackItem

El uso de un objeto [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) para la reproducción es conveniente porque proporciona una forma común de reproducir contenido multimedia desde diferentes tipos de orígenes, pero puedes acceder a un comportamiento más avanzado al crear una clase [**MediaPlaybackItem**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) a partir de **MediaSource**. Esto incluye la posibilidad de acceder a varias pistas de audio, vídeo y datos de un elemento multimedia y administrarlas.

Declara una variable para almacenar el objeto **MediaPlaybackItem**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackItem":::

Crea un objeto **MediaPlaybackItem** con una llamada al constructor y pasándolo en un objeto **MediaSource** inicializado.

Si tu aplicación admite varias pistas de audio, vídeo o datos en un elemento de reproducción de contenido multimedia, registra controladores de eventos para los eventos [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) o [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged).

Por último, establece el origen de reproducción de la clase **MediaElement** o **MediaPlayer** en tu objeto **MediaPlaybackItem**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackItem":::

> [!NOTE] 
> Un objeto **MediaSource** únicamente se puede asociar a un solo objeto **MediaPlaybackItem**. Después de crear un objeto **MediaPlaybackItem** a partir de un origen, si intentas crear otro elemento de reproducción desde el mismo origen, se generará un error. Además, después de crear un objeto **MediaPlaybackItem** desde un origen de contenido multimedia, no puedes establecer el objeto **MediaSource** directamente como el origen de un objeto **MediaPlayer**. En cambio, debes usar el objeto **MediaPlaybackItem**.

El evento [**VideoTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.videotrackschanged) se genera después de que un objeto **MediaPlaybackItem** que contiene varias pistas de vídeos se asigne como origen de reproducción y se puede volver a lanzar si la lista de pistas de vídeo cambia para los cambios de elemento. El controlador de este evento te permite actualizar la interfaz de usuario para permitir al usuario cambiar entre pistas disponibles. En este ejemplo se usa un objeto [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox) para mostrar las pistas de vídeo disponibles.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetVideoComboBox":::

En el controlador **VideoTracksChanged**, repite todas las pistas de la lista [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) del elemento de reproducción. Para cada pista, se crea un nuevo objeto [**ComboBoxItem**](/uwp/api/Windows.UI.Xaml.Controls.ComboBoxItem). Si la pista todavía no tiene etiqueta, esta se genera a partir del índice de pistas. La propiedad [**Tag**](/uwp/api/windows.ui.xaml.frameworkelement.tag) del elemento de cuadro combinado se establece en el índice de pistas para que se pueda identificar más adelante. Por último, el elemento se agrega al cuadro combinado. Ten en cuenta que estas operaciones se realizan en una llamada [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) porque todos los cambios de la interfaz de usuario deben realizarse en el subproceso de interfaz de usuario y este evento se lanza en un subproceso diferente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksChanged":::

En el controlador [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged) del cuadro combinado, el índice de pistas se recupera de la propiedad **Tag** del elemento seleccionado. Si estableces la propiedad [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackvideotracklist.selectedindex) en la lista [**VideoTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.videotracks) del elemento de reproducción de contenido multimedia, el objeto **MediaElement** o **MediaPlayer** cambiará la pista de vídeo activa al índice especificado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetVideoTracksSelectionChanged":::

La administración de elementos multimedia con varias pistas de audio funciona exactamente del mismo modo que con las pistas de vídeo. Controla el objeto [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged) para actualizar la interfaz de usuario con las pistas de audio que se encuentran en la lista [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) del elemento de reproducción. Cuando el usuario selecciona una pista de audio, establece la propiedad [**SelectedIndex**](/uwp/api/windows.media.playback.mediaplaybackaudiotracklist.selectedindex) de la lista **AudioTracks** para hacer que el objeto **MediaElement** o **MediaPlayer** cambie la pista de audio activa al índice especificado.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetAudioComboBox":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksSelectionChanged":::

Además de contenido de audio y vídeo, un objeto **MediaPlaybackItem** puede contener cero o más objetos [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack). Una pista de metadatos temporizada puede contener texto de subtítulo o título, o puede contener datos personalizados que son propiedad de la aplicación. Una pista de metadatos temporizada contiene una lista de indicaciones representadas por objetos que heredan del objeto [**IMediaCue**](/uwp/api/Windows.Media.Core.IMediaCue), como un objeto [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) o [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue). Cada indicación tiene una hora de inicio y una duración que determina el momento de activación de la indicación y durante cuánto tiempo.

Similar a las pistas de audio y vídeos, las pistas de metadatos temporizadas de un elemento multimedia se pueden descubrir al controlar el evento [**TimedMetadataTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.timedmetadatatrackschanged) de un objeto **MediaPlaybackItem**. Sin embargo, con las pistas de metadatos temporizadas, quizás el usuario quiera habilitar más de una pista de metadatos a la vez. Además, en función del escenario de la aplicación, es posible que quieras habilitar o deshabilitar las pitas de metadatos automáticamente, sin la intervención del usuario. A efectos de Ilustración, en este ejemplo se agrega un control [**ToggleButton**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton) para cada pista de metadatos en un elemento multimedia para permitir que el usuario habilite y deshabilite la pista. La propiedad **Tag** de cada botón está establecida en el índice de la pista de metadatos asociada para que se pueda identificar cuando se alterne el botón.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml" id="SnippetMetaStackPanel":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedMetadataTrackschanged":::

Dado que puede haber más de una pista de metadatos activa a la vez, no se establece simplemente el índice activo de la lista de pistas de metadatos. En cambio, llama al método [**SetPresentationMode**](/previous-versions/windows/dn986977(v=win.10)) del objeto **MediaPlaybackItem**, pasando el índice de la pista que quieras cambiar y luego proporcionando un valor de la enumeración [**TimedMetadataTrackPresentationMode**](/uwp/api/Windows.Media.Playback.TimedMetadataTrackPresentationMode). El modo de presentación que elijas depende de la implementación de la aplicación. En este ejemplo, la pista de metadatos se establece en **PlatformPresented** cuando está habilitada. En el caso de las pistas basadas en texto, esto significa que el sistema mostrará automáticamente las indicaciones de texto en la pista. Cuando el botón de alternancia está desactivado, el modo de presentación se establece en **deshabilitado**, lo que significa que no se muestra ningún texto y no se genera ningún evento de indicación. Los eventos de indicación se describen más adelante en este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleChecked":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetToggleUnchecked":::

Mientras se procesan las pistas de metadatos, puedes acceder al conjunto de indicaciones de la pista mediante el acceso a las propiedades [**Cues**](/uwp/api/windows.media.core.timedmetadatatrack.cues) o [**ActiveCues**](/uwp/api/windows.media.core.timedmetadatatrack.activecues). Puedes hacerlo para actualizar la interfaz de usuario con el fin de mostrar las ubicaciones de indicación de un elemento multimedia.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Controlar errores desconocidos y códecs no admitidos al abrir elementos multimedia
A partir de Windows 10, versión 1607, puedes comprobar si el códec necesario para la reproducción de un elemento multimedia se admite parcial o totalmente en el dispositivo en el que se ejecuta la aplicación. En el controlador de eventos para los eventos de cambio de seguimiento de **MediaPlaybackItem** , como [**AudioTracksChanged**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotrackschanged), primero Compruebe si el cambio de seguimiento es una inserción de una nueva pista. Si es así, puede obtener una referencia a la pista que se va a insertar utilizando el índice pasado en el parámetro **IVectorChangedEventArgs. index** con la colección de seguimiento adecuada del parámetro **MediaPlaybackItem** , como la colección [**AudioTracks**](/uwp/api/windows.media.playback.mediaplaybackitem.audiotracks) .

Una vez que tengas una referencia a la pista insertada, comprueba el valor de [**DecoderStatus**](/uwp/api/windows.media.core.audiotracksupportinfo.decoderstatus) de la propiedad [**SupportInfo**](/uwp/api/windows.media.core.audiotrack.supportinfo) de la pista. Si el valor es [**FullySupported**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), entonces el códec adecuado que se necesita para reproducir la pista está presente en el dispositivo. Si el valor es [**Degraded**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), el sistema puede reproducir la pista, pero la reproducción se degradará de alguna manera. Por ejemplo, una pista de audio 5.1 puede reproducirse como estéreo de dos canales. Si este es el caso, quizás quieras actualizar tu interfaz de usuario para alertar al usuario sobre la degradación. Si el valor es [**UnsupportedSubtype**](/uwp/api/Windows.Media.Core.MediaDecoderStatus) o [**UnsupportedEncoderProperties**](/uwp/api/Windows.Media.Core.MediaDecoderStatus), la pista no se puede reproducir en absoluto con los códecs actuales del dispositivo. Quizás quieras alertar al usuario y omitir la reproducción del elemento o implementar la interfaz de usuario para que el usuario pueda descargar el códec correcto. El método [**GetEncodingProperties**](/uwp/api/windows.media.core.audiotrack.getencodingproperties) de la pista puede usarse para determinar el códec necesario para la reproducción.

Por último, puedes registrar el evento [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed) de la pista, que se genera si se admite la pista en el dispositivo, pero no se pudo abrir debido a un error desconocido en la canalización.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAudioTracksChanged_CodecCheck":::

En el controlador del evento [**OpenFailed**](/uwp/api/windows.media.core.audiotrack.openfailed), puedes comprobar si se desconoce el estado de **MediaSource** y, si es así, puedes seleccionar mediante programación una pista diferente para reproducir, permitir al usuario que elija una pista diferente o abandonar la reproducción.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetOpenFailed":::

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Establecer las propiedades de visualización que se usan en los controles de transporte de contenido multimedia del sistema
A partir de Windows 10, versión 1607, el contenido multimedia que se reproduce en una clase [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer) se integra automáticamente en los controles de transporte de contenido multimedia del sistema (SMTC) de manera predeterminada. Puedes especificar los metadatos que se mostrarán en SMTC actualizando las propiedades de visualización de **MediaPlaybackItem**. Obtén un objeto que representa las propiedades de visualización de un elemento mediante una llamada a [**GetDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.getdisplayproperties). Establece si el elemento de reproducción es música o vídeo al establecer el valor de la propiedad [**Type**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.type). A continuación, establece las propiedades [**VideoProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.videoproperties) o [**MusicProperties**](/uwp/api/windows.media.playback.mediaitemdisplayproperties.musicproperties) del objeto. Llama a [**ApplyDisplayProperties**](/uwp/api/windows.media.playback.mediaplaybackitem.applydisplayproperties) para actualizar las propiedades del elemento en los valores que hayas especificado. Por lo general, una aplicación recuperará los valores de visualización dinámicamente desde un servicio web, pero en el siguiente ejemplo se muestra este proceso con los valores codificados de forma rígida.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetVideoProperties":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetSetMusicProperties":::

## <a name="add-external-timed-text-with-timedtextsource"></a>Agregar texto temporizado externo con TimedTextSource

En algunos escenarios, puedes tener archivos externos que contienen texto temporizado asociado con un elemento multimedia, tales como archivos independientes que contienen los subtítulos en las distintas configuraciones regionales. Usa la clase [**TimedTextSource**](/uwp/api/Windows.Media.Core.TimedTextSource) para cargarla en archivos de texto temporizado externos de una secuencia o un URI.

En este ejemplo, se usa una colección **Dictionary** para almacenar una lista de los orígenes de texto temporizado del elemento multimedia con el URI de origen y el objeto **TimedTextSource** como el par clave y valor con el fin de identificar las pistas después de que se hayan resuelto.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceMap":::

Crea un nuevo objeto **TimedTextSource** para cada archivo de texto temporizado externo con una llamada a [**CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri). Agrega una entrada al objeto **Dictionary** para el origen de texto temporizado. Agrega un controlador para el evento [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved) con el fin de controlar si el elemento no se pudo cargar o para establecer propiedades adicionales después de que el elemento se haya cargado correctamente.

Registra todos los objetos **TimedTextSource** en el objeto **MediaSource** agregándolos a la colección [**ExternalTimedTextSources**](/uwp/api/windows.media.core.mediasource.externaltimedtextsources). Ten en cuenta que los orígenes de texto temporizado externos se agregan directamente al objeto **MediaSource** y no al objeto **MediaPlaybackItem** creado a partir del origen. Para actualizar la interfaz de usuario y reflejar las pistas de texto externo, registra y controla el evento **TimedMetadataTracksChanged** según se describió anteriormente en este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSource":::

En el controlador del evento [**TimedTextSource.Resolved**](/uwp/api/windows.media.core.timedtextsource.resolved), comprueba la propiedad **Error** del objeto [**TimedTextSourceResolveResultEventArgs**](/uwp/api/Windows.Media.Core.TimedTextSourceResolveResultEventArgs) que se pasó al controlador para determinar si se produjo un error al intentar cargar los datos de texto temporizado. Si el elemento se resolvió correctamente, puede usar este controlador para actualizar las propiedades adicionales de la pista resuelta. En este ejemplo se agrega una etiqueta para cada pista basada en el URI previamente almacenado en el **Diccionario**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTimedTextSourceResolved":::

## <a name="add-additional-metadata-tracks"></a>Agregar pistas de metadatos adicionales

Puedes crear pistas de metadatos personalizadas dinámicamente en el código y asociarlas con un origen de contenido multimedia. Las pistas que creas pueden contener texto de subtítulo o título, o pueden contener los datos de propiedad de la aplicación.

Crea un nuevo objeto [**TimedMetadataTrack**](/uwp/api/Windows.Media.Core.TimedMetadataTrack) con una llamada al constructor y especificando el identificador, el identificador de idioma y un valor de la enumeración [**TimedMetadataKind**](/uwp/api/Windows.Media.Core.TimedMetadataKind). Registra los controladores para los eventos [**CueEntered**](/uwp/api/windows.media.core.timedmetadatatrack.cueentered) y [**CueExited**](/uwp/api/windows.media.core.timedmetadatatrack.cueexited). Estos eventos se generan cuando se alcanza el tiempo de inicio para una identificación y cuando la duración de una identificación ha expirado, respectivamente.

Cree un nuevo objeto de pila, adecuado para el tipo de seguimiento de metadatos que ha creado, y establezca el identificador, la hora de inicio y la duración de la pista. En este ejemplo se crea una pista de datos, por lo que se genera un conjunto de objetos [**DataCue**](/uwp/api/Windows.Media.Core.DataCue) y se proporciona un búfer que contiene datos específicos de la aplicación para cada pila. Para registrar la pista nueva, agrégala a la colección [**ExternalTimedMetadataTracks**](/uwp/api/windows.media.core.mediasource.externaltimedmetadatatracks) del objeto **MediaSource**.

A partir de Windows 10, versión 1703, la propiedad **DataCue. Properties** expone un [**PropertySet**](/uwp/api/windows.foundation.collections.propertyset) que puede usar para almacenar propiedades personalizadas en pares de clave/datos que se pueden recuperar en los eventos **CueEntered** y **CueExited** .  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddDataTrack":::

El evento **CueEntered** se genera cuando se ha alcanzado la hora de inicio de una indicación, siempre que la pista asociada tenga un modo de presentación de **ApplicationPresented**, **Hidden** o **PlatformPresented**. Los eventos de indicación no se generan para pistas de metadatos mientras el modo de presentación de la pista es **Disabled**. En este ejemplo, simplemente se envían a la ventana de depuración los datos personalizados asociados con la indicación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDataCueEntered":::

En este ejemplo se agrega una pista de texto personalizado al especificar **TimedMetadataKind.Caption** durante la creación de la pista y usando objetos [**TimedTextCue**](/uwp/api/Windows.Media.Core.TimedTextCue) para agregar indicaciones a la pista.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetAddTextTrack":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetTextCueEntered":::

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Reproducir una lista de elementos multimedia con MediaPlaybackList

El objeto [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) permite crear una lista de reproducción de elementos multimedia, que se representan con objetos **MediaPlaybackItem**.

**Nota:**    Los elementos de una [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList) se representan mediante la reproducción de sin huecos. El sistema usará los metadatos proporcionados en archivos codificados MP3 o AAC para determinar la compensación del retraso o el espaciado interno necesaria para la reproducción sin pausas. Si los archivos codificados MP3 o AAC no proporcionan estos metadatos, el sistema determina el retraso o el espaciado interno de forma heurística. Para los formatos sin pérdida, como PCM, FLAC o ALAC, el sistema no realiza ninguna acción porque estos codificadores no introducen ningún retraso ni espaciado interno.

Para empezar, declara una variable para almacenar el objeto **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareMediaPlaybackList":::

Crear un objeto **MediaPlaybackItem** para cada elemento multimedia que quieras agregar a tu lista usando el mismo procedimiento descrito anteriormente en este artículo. Inicializa el objeto **MediaPlaybackList** y agrégale los elementos de reproducción de contenido multimedia. Registra el controlador del evento [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.currentitemchanged). Este evento permite actualizar la interfaz de usuario para reflejar el elemento multimedia que se encuentra en reproducción. También puede registrarse para el evento [ItemOpened](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened) , que se genera cuando un elemento de la lista se abre correctamente y el evento [ItemFailed](/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed) , que se genera cuando no se puede abrir un elemento de la lista.

A partir de Windows 10, versión 1703, puede especificar el número máximo de objetos de **MediaPlaybackItem** en el objeto **MediaPlaybackList** que el sistema mantendrá abierto después de que se hayan jugado estableciendo la propiedad [MaxPlayedItemsToKeepOpen](/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen) . Cuando se mantiene abierto un objeto **MediaPlaybackItem** , la reproducción del elemento puede iniciarse de forma instantánea cuando el usuario cambia a ese elemento porque no es necesario volver a cargar el elemento. Pero mantener los elementos abiertos también aumenta el consumo de memoria de la aplicación, por lo que debe tener en cuenta el equilibrio entre la capacidad de respuesta y el uso de memoria al establecer este valor. 

Para habilitar la reproducción de la lista, establezca el origen de reproducción de la **MediaPlayer** en el **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPlayMediaPlaybackList":::

En el controlador de eventos **CurrentItemChanged**, actualiza la interfaz de usuario para reflejar el elemento en reproducción, que se puede recuperar con la propiedad [**NewItem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.newitem) del objeto [**CurrentMediaPlaybackItemChangedEventArgs**](/uwp/api/Windows.Media.Playback.CurrentMediaPlaybackItemChangedEventArgs) pasado al evento. Recuerda que si actualizas la interfaz de usuario de este evento, debes hacerlo dentro de una llamada a [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) para que las actualizaciones se hagan en el subproceso de la interfaz de usuario.

A partir de Windows 10, versión 1703, puede comprobar la propiedad [CurrentMediaPlaybackItemChangedEventArgs. Reason](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) para obtener un valor que indica la razón por la que el elemento ha cambiado, como la aplicación que cambia los elementos mediante programación, el elemento que se está reproduciendo anteriormente hasta su final o un error que se produce.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetMediaPlaybackListItemChanged":::


Llama a [**MovePrevious**](/uwp/api/windows.media.playback.mediaplaybacklist.moveprevious) o [**MoveNext**](/uwp/api/windows.media.playback.mediaplaybacklist.movenext) para hacer que el reproductor de contenido multimedia reproduzca el elemento anterior o siguiente en tu objeto **MediaPlaybackList**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetPrevButton":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNextButton":::

Establece la propiedad [**ShuffleEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.shuffleenabled) para especificar si el reproductor de contenido multimedia debe reproducir los elementos de la lista en orden aleatorio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetShuffleButton":::

Establece la propiedad [**AutoRepeatEnabled**](/uwp/api/windows.media.playback.mediaplaybacklist.autorepeatenabled) para especificar si el reproductor de contenido multimedia debe repetir la reproducción de la lista.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRepeatButton":::


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Controlar los errores de los elementos multimedia en una lista de reproducción
El evento [**ItemFailed**](/uwp/api/windows.media.playback.mediaplaybacklist.itemfailed) se genera cuando se produce un error al intentar abrir un elemento de la lista. La propiedad [**ErrorCode**](/uwp/api/windows.media.playback.mediaplaybackitemerror.errorcode) del objeto [**MediaPlaybackItemError**](/uwp/api/Windows.Media.Playback.MediaPlaybackItemError) pasado al controlador enumera la causa específica del error cuando es posible, incluidos los errores de red, descodificación o de cifrado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetItemFailed":::

### <a name="disable-playback-of-items-in-a-playback-list"></a>Deshabilitar la reproducción de elementos en una lista de reproducción
A partir de Windows 10, versión 1703, puede deshabilitar la reproducción de uno o más elementos en un **MediaPlaybackItemList** estableciendo la propiedad [IsDisabledInPlaybackList](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) de un elemento [MediaPlaybackItem](/uwp/api/Windows.Media.Playback.MediaPlaybackItem) en false. 

Un escenario típico para esta característica es para las aplicaciones que reproducen música transmitida desde Internet. La aplicación puede escuchar los cambios en el estado de conexión de red del dispositivo y deshabilitar la reproducción de elementos que no se descargan por completo. En el ejemplo siguiente, se registra un controlador para el evento [NetworkInformation. NetworkStatusChanged](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged) .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetRegisterNetworkStatusChanged":::

En el controlador de **NetworkStatusChanged**, compruebe si [GetInternetConnectionProfile](/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) devuelve null, lo que indica que la red no está conectada. En este caso, recorra en bucle todos los elementos de la lista de reproducción y si el [TotalDownloadProgress](/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) del elemento es menor que 1, lo que significa que el elemento no se ha descargado completamente, deshabilite el elemento. Si está habilitada la conexión de red, recorra en bucle todos los elementos de la lista de reproducción y habilite cada elemento.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetNetworkStatusChanged":::

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Aplazar el enlace de contenido multimedia para los elementos de una lista de reproducción mediante MediaBinder
En los ejemplos anteriores, se crea un objeto **MediaSource** a partir de un archivo, una dirección URL o una secuencia, después del cual se crea un objeto **MediaPlaybackItem** y se agrega a un objeto **MediaPlaybackList**. En algunos escenarios, como si se cobra al usuario para ver el contenido, es posible que desee aplazar la recuperación del contenido de un objeto **MediaSource** hasta que el elemento de la lista de reproducción esté listo para reproducirse realmente. Para implementar este escenario, cree una instancia de la clase [**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) . Establezca la propiedad [**token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) en una cadena definida por la aplicación que identifique el contenido para el que desea aplazar la recuperación y, a continuación, registre un controlador para el evento de [**enlace**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) . A continuación, cree un **MediaSource** desde el **enlazador** llamando a [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder). A continuación, cree un **MediaPlaybackItem** a partir de **MediaSource** y agréguelo a la lista de reproducción como de costumbre.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetInitMediaBinder":::

Cuando el sistema determina que es necesario recuperar el contenido asociado a **MediaBinder** , se generará el evento de **enlace** . En el controlador de este evento, puede recuperar la instancia de **MediaBinder** desde el [**MediaBindingEventArgs**](/uwp/api/windows.media.core.mediabindingeventargs) que se pasa al evento. Recupere la cadena que especificó para la propiedad **token** y Úsela para determinar qué contenido se debe recuperar. **MediaBindingEventArgs** proporciona métodos para establecer el contenido enlazado en varias representaciones diferentes, como [**SetStorageFile**](/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference)y [**SetUri**](/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaSource_RS1/cs/MainPage.xaml.cs" id="SnippetBinderBinding":::

Tenga en cuenta que si realiza operaciones asincrónicas, como solicitudes Web, en el controlador de eventos de **enlace** , debe llamar al método [**MediaBindingEventArgs. GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para indicar al sistema que espere a que la operación se complete antes de continuar. Realice una llamada a [**aplazamiento. Complete**](/uwp/api/windows.foundation.deferral.Complete) después de que se complete la operación para indicar al sistema que continúe.

A partir de Windows 10, versión 1703, puede proporcionar un [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como contenido enlazado llamando a [**SetAdaptiveMediaSource**](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource). Para más información sobre el uso de streaming adaptable en su aplicación, consulte [Adaptive streaming](adaptive-streaming.md).



## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md)
