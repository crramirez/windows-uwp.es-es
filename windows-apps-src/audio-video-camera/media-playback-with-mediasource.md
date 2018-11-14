---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: En este artículo se muestra cómo usar la clase MediaSource, que proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde diferentes orígenes (como archivos locales o remotos) y se expone un modelo común para acceder a datos multimedia, independientemente del formato multimedia subyacente.
title: Elementos multimedia, listas de reproducción y pistas
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 73b6a19e2385f1a9b8afa4672df50d17ac16ec97
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "6190684"
---
# <a name="media-items-playlists-and-tracks"></a>Elementos multimedia, listas de reproducción y pistas


 En este artículo se muestra cómo usar la clase [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource), que proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde diferentes orígenes (como archivos locales o remotos) y se expone un modelo común para acceder a datos multimedia, independientemente del formato multimedia subyacente. La clase [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) amplía la funcionalidad del objeto **MediaSource**, lo que permite administrar y seleccionar entre varias pistas de audio, vídeo y metadatos incluidos en un elemento multimedia. [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite crear listas de reproducción de uno o más elementos de reproducción de contenido multimedia.


## <a name="create-and-play-a-mediasource"></a>Crear y reproducir un objeto MediaSource

Crea una nueva instancia del objeto **MediaSource** con una llamada a uno de los métodos de fábrica expuestos por la clase:

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)
-   [**CreateFromDownloadOperation**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromdownloadoperation)

Después de crear un objeto **MediaSource**, puedes reproducirlo con una clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) estableciendo la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). A partir de Windows 10, versión 1607, se puede asignar un objeto **MediaPlayer** a una clase [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) mediante una llamada al método [**SetMediaPlayer**](https://msdn.microsoft.com/library/windows/apps/mt708764) para representar el contenido del reproductor multimedia en una página XAML. Este es el método preferido en comparación con el uso de **MediaElement**. Para obtener más información sobre cómo usar **MediaPlayer**, consulta [**Reproducir audio y vídeo con MediaPlayer**](play-audio-and-video-with-mediaplayer.md).

En el siguiente ejemplo se muestra cómo reproducir un archivo multimedia seleccionado por el usuario en un objeto **MediaPlayer** con **MediaSource**.

Para completar este escenario, tendrás que incluir los espacios de nombres [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) y [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562).

[!code-cs[Using](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetUsing)]

Declara una variable de tipo **MediaSource**. Para los ejemplos de este artículo, el origen de contenido multimedia se declara como un miembro de clase para que se pueda acceder a él desde varias ubicaciones.

[!code-cs[DeclareMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Declara una variable para almacenar el objeto **MediaPlayer** y, si quieres representar el contenido multimedia en XAML, agrega un control **MediaPlayerElement** a la página.

[!code-cs[DeclareMediaPlayer](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-xml[MediaPlayerElement](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMediaPlayerElement)]

Para permitir al usuario seleccionar un archivo multimedia para su reproducción, usa una clase [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Con el objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) devuelto desde el método [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) del selector, inicializa un nuevo objeto MediaObject con una llamada a [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Por último, establece el origen del contenido multimedia como el origen de reproducción del objeto **MediaElement** con una llamada al método [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

De manera predeterminada, el objeto **MediaPlayer** no comienza a reproducirse automáticamente cuando se establece el origen del contenido multimedia. Puedes comenzar la reproducción de forma manual con una llamada al método [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play).

[!code-cs[Play](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlay)]

También puedes establecer la propiedad [**AutoPlay**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.AutoPlay) de **MediaPlayer** en true para indicar al reproductor que comience la reproducción en cuanto se establezca el origen del contenido multimedia.

[!code-cs[AutoPlay](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAutoPlay)]

### <a name="create-a-mediasource-from-a-downloadoperation"></a>Crear un valor de MediaSource de una DownloadOperation
A partir de Windows, versión 1803, puedes crear un objeto **MediaSource** desde un valor de **DownloadOperation**.

[!code-cs[CreateMediaSourceFromDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetCreateMediaSourceFromDownload)]

Ten en cuenta que si bien puedes crear un **MediaSource** desde una descarga sin iniciarla o establecer su propiedad **IsRandomAccessRequired** en true, debes hacer ambas acciones antes de intentar adjuntar el **MediaSource** a un **MediaPlayer** o **MediaPlayerElement** para la reproducción.

[!code-cs[StartDownload](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetStartDownload)]


## <a name="handle-multiple-audio-video-and-metadata-tracks-with-mediaplaybackitem"></a>Controlar varias pistas de audio, vídeo y metadatos con MediaPlaybackItem

El uso de un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) para la reproducción es conveniente porque proporciona una forma común de reproducir contenido multimedia desde diferentes tipos de orígenes, pero puedes acceder a un comportamiento más avanzado al crear una clase [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) a partir de **MediaSource**. Esto incluye la posibilidad de acceder a varias pistas de audio, vídeo y datos de un elemento multimedia y administrarlas.

Declara una variable para almacenar el objeto **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Crea un objeto **MediaPlaybackItem** con una llamada al constructor y pasándolo en un objeto **MediaSource** inicializado.

Si tu aplicación admite varias pistas de audio, vídeo o datos en un elemento de reproducción de contenido multimedia, registra controladores de eventos para los eventos [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) o [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Por último, establece el origen de reproducción de la clase **MediaElement** o **MediaPlayer** en tu objeto **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

> [!NOTE] 
> Un objeto **MediaSource** únicamente se puede asociar a un solo objeto **MediaPlaybackItem**. Después de crear un objeto **MediaPlaybackItem** a partir de un origen, si intentas crear otro elemento de reproducción desde el mismo origen, se generará un error. Además, después de crear un objeto **MediaPlaybackItem** desde un origen de contenido multimedia, no puedes establecer el objeto **MediaSource** directamente como el origen de un objeto **MediaPlayer**. En cambio, debes usar el objeto **MediaPlaybackItem**.

El evento [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) se genera después de que un objeto **MediaPlaybackItem** que contiene varias pistas de vídeos se asigne como origen de reproducción y se puede volver a lanzar si la lista de pistas de vídeo cambia para los cambios de elemento. El controlador de este evento te permite actualizar la interfaz de usuario para permitir al usuario cambiar entre pistas disponibles. En este ejemplo se usa un objeto [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para mostrar las pistas de vídeo disponibles.

[!code-xml[VideoComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetVideoComboBox)]

En el controlador **VideoTracksChanged**, repite todas las pistas de la lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) del elemento de reproducción. Para cada pista, se crea un nuevo objeto [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349). Si la pista todavía no tiene etiqueta, esta se genera a partir del índice de pistas. La propiedad [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) del elemento de cuadro combinado se establece en el índice de pistas para que se pueda identificar más adelante. Por último, el elemento se agrega al cuadro combinado. Ten en cuenta que estas operaciones se realizan en una llamada [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) porque todos los cambios de la interfaz de usuario deben realizarse en el subproceso de interfaz de usuario y este evento se lanza en un subproceso diferente.

[!code-cs[VideoTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

En el controlador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) del cuadro combinado, el índice de pistas se recupera de la propiedad **Tag** del elemento seleccionado. Si estableces la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) en la lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) del elemento de reproducción de contenido multimedia, el objeto **MediaElement** o **MediaPlayer** cambiará la pista de vídeo activa al índice especificado.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

La administración de elementos multimedia con varias pistas de audio funciona exactamente del mismo modo que con las pistas de vídeo. Controla el objeto [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) para actualizar la interfaz de usuario con las pistas de audio que se encuentran en la lista [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) del elemento de reproducción. Cuando el usuario selecciona una pista de audio, establece la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) de la lista **AudioTracks** para hacer que el objeto **MediaElement** o **MediaPlayer** cambie la pista de audio activa al índice especificado.

[!code-xml[AudioComboBox](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Además de contenido de audio y vídeo, un objeto **MediaPlaybackItem** puede contener cero o más objetos [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Una pista de metadatos temporizada puede contener texto de subtítulo o título, o puede contener datos personalizados que son propiedad de la aplicación. Una pista de metadatos temporizada contiene una lista de indicaciones representadas por objetos que heredan del objeto [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), como un objeto [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) o [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655). Cada indicación tiene una hora de inicio y una duración que determina el momento de activación de la indicación y durante cuánto tiempo.

Similar a las pistas de audio y vídeos, las pistas de metadatos temporizadas de un elemento multimedia se pueden descubrir al controlar el evento [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) de un objeto **MediaPlaybackItem**. Sin embargo, con las pistas de metadatos temporizadas, quizás el usuario quiera habilitar más de una pista de metadatos a la vez. Además, en función del escenario de la aplicación, es posible que quieras habilitar o deshabilitar las pitas de metadatos automáticamente, sin la intervención del usuario. Para fines de ilustración, en este ejemplo se agrega un objeto [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) para cada pista de metadatos en un elemento multimedia de modo que el usuario pueda habilitar y deshabilitar la pista. La propiedad **Tag** de cada botón se establece en el índice de la pista de metadatos asociada para que pueda identificarse cuando se active o desactive el botón.

[!code-xml[MetaStackPanel](./code/MediaSource_RS1/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Dado que puede haber más de una pista de metadatos activa a la vez, no se establece simplemente el índice activo de la lista de pistas de metadatos. En cambio, llama al método [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) del objeto **MediaPlaybackItem**, pasando el índice de la pista que quieras cambiar y luego proporcionando un valor de la enumeración [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). El modo de presentación que elijas depende de la implementación de la aplicación. En este ejemplo, la pista de metadatos se establece en **PlatformPresented** cuando está habilitada. Para las pistas basadas en texto, esto significa que el sistema mostrará automáticamente las indicaciones de texto en la pista. Cuando se desactiva el botón de alternancia, el modo de presentación se establece en **Disabled**, lo que significa que se muestra ningún texto y no se genera ningún evento de indicación. Los eventos de indicación se describen más adelante en este artículo.

[!code-cs[ToggleChecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

Mientras se procesan las pistas de metadatos, puedes acceder al conjunto de indicaciones de la pista mediante el acceso a las propiedades [**Cues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.Cues) o [**ActiveCues**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.TimedMetadataTrack.ActiveCues). Puedes hacerlo para actualizar la interfaz de usuario con el fin de mostrar las ubicaciones de indicación de un elemento multimedia.

## <a name="handle-unsupported-codecs-and-unknown-errors-when-opening-media-items"></a>Controlar errores desconocidos y códecs no admitidos al abrir elementos multimedia
A partir de Windows 10, versión 1607, puedes comprobar si el códec necesario para la reproducción de un elemento multimedia se admite parcial o totalmente en el dispositivo en el que se ejecuta la aplicación. En el controlador de eventos para los eventos cambiados por pistas **MediaPlaybackItem**, como [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracksChanged), comprueba primero si el cambio de la pista es una inserción de una pista nueva. Si es así, puede obtener una referencia a la pista que se va a insertar utilizando el índice pasado en el parámetro **IVectorChangedEventArgs.Index** con la colección de pistas adecuada del parámetro **MediaPlaybackItem**, como la colección [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.AudioTracks).

Una vez que tengas una referencia a la pista insertada, comprueba el valor de [**DecoderStatus**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrackSupportInfo.DecoderStatus) de la propiedad [**SupportInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.SupportInfo) de la pista. Si el valor es [**FullySupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), entonces el códec adecuado que se necesita para reproducir la pista está presente en el dispositivo. Si el valor es [**Degraded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), el sistema puede reproducir la pista, pero la reproducción se degradará de alguna manera. Por ejemplo, una pista de audio 5.1 puede reproducirse como estéreo de dos canales. Si este es el caso, quizás quieras actualizar tu interfaz de usuario para alertar al usuario sobre la degradación. Si el valor es [**UnsupportedSubtype**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus) o [**UnsupportedEncoderProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaDecoderStatus), la pista no se puede reproducir en absoluto con los códecs actuales del dispositivo. Quizás quieras alertar al usuario y omitir la reproducción del elemento o implementar la interfaz de usuario para que el usuario pueda descargar el códec correcto. El método [**GetEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.GetEncodingProperties) de la pista puede usarse para determinar el códec necesario para la reproducción.

Por último, puedes registrar el evento [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed) de la pista, que se genera si se admite la pista en el dispositivo, pero no se pudo abrir debido a un error desconocido en la canalización.

[!code-cs[AudioTracksChanged_CodecCheck](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAudioTracksChanged_CodecCheck)]

En el controlador del evento [**OpenFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.AudioTrack.OpenFailed), puedes comprobar si se desconoce el estado de **MediaSource** y, si es así, puedes seleccionar mediante programación una pista diferente para reproducir, permitir al usuario que elija una pista diferente o abandonar la reproducción.

[!code-cs[OpenFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetOpenFailed)]

## <a name="set-display-properties-used-by-the-system-media-transport-controls"></a>Establecer las propiedades de visualización que se usan en los controles de transporte de contenido multimedia del sistema
A partir de Windows 10, versión 1607, el contenido multimedia que se reproduce en una clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer) se integra automáticamente en los controles de transporte de contenido multimedia del sistema (SMTC) de manera predeterminada. Puedes especificar los metadatos que se mostrarán en SMTC actualizando las propiedades de visualización de **MediaPlaybackItem**. Obtén un objeto que representa las propiedades de visualización de un elemento mediante una llamada a [**GetDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItem.GetDisplayProperties). Establece si el elemento de reproducción es música o vídeo al establecer el valor de la propiedad [**Type**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.Type). A continuación, establece las propiedades [**VideoProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.VideoProperties) o [**MusicProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaItemDisplayProperties.MusicProperties) del objeto. Llama a [**ApplyDisplayProperties**](https://msdn.microsoft.com/library/windows/apps/mt489923) para actualizar las propiedades del elemento en los valores que hayas especificado. Por lo general, una aplicación recuperará los valores de visualización dinámicamente desde un servicio web, pero en el siguiente ejemplo se muestra este proceso con los valores codificados de forma rígida.

[!code-cs[SetVideoProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetVideoProperties)]

[!code-cs[SetMusicProperties](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetSetMusicProperties)]

## <a name="add-external-timed-text-with-timedtextsource"></a>Agregar texto temporizado externo con TimedTextSource

En algunos escenarios, puedes tener archivos externos que contienen texto temporizado asociado con un elemento multimedia, tales como archivos independientes que contienen los subtítulos en las distintas configuraciones regionales. Usa la clase [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) para cargarla en archivos de texto temporizado externos de una secuencia o un URI.

En este ejemplo, se usa una colección **Dictionary** para almacenar una lista de los orígenes de texto temporizado del elemento multimedia con el URI de origen y el objeto **TimedTextSource** como el par clave y valor con el fin de identificar las pistas después de que se hayan resuelto.

[!code-cs[TimedTextSourceMap](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Crea un nuevo objeto **TimedTextSource** para cada archivo de texto temporizado externo con una llamada a [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Agrega una entrada al objeto **Dictionary** para el origen de texto temporizado. Agrega un controlador para el evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) con el fin de controlar si el elemento no se pudo cargar o para establecer propiedades adicionales después de que el elemento se haya cargado correctamente.

Registra todos los objetos **TimedTextSource** en el objeto **MediaSource** agregándolos a la colección [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Ten en cuenta que los orígenes de texto temporizado externos se agregan directamente al objeto **MediaSource** y no al objeto **MediaPlaybackItem** creado a partir del origen. Para actualizar la interfaz de usuario y reflejar las pistas de texto externo, registra y controla el evento **TimedMetadataTracksChanged** según se describió anteriormente en este artículo.

[!code-cs[TimedTextSource](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

En el controlador del evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), comprueba la propiedad **Error** del objeto [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) que se pasó al controlador para determinar si se produjo un error al intentar cargar los datos de texto temporizado. Si el elemento se resolvió correctamente, puedes usar este controlador para actualizar las propiedades adicionales de la pista resuelta. En este ejemplo se agrega una etiqueta para cada pista basada en el URI almacenado anteriormente en el **Dictionary**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## <a name="add-additional-metadata-tracks"></a>Agregar pistas de metadatos adicionales

Puedes crear pistas de metadatos personalizadas dinámicamente en el código y asociarlas con un origen de contenido multimedia. Las pistas que creas pueden contener texto de subtítulo o título, o pueden contener los datos de propiedad de la aplicación.

Crea un nuevo objeto [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) con una llamada al constructor y especificando el identificador, el identificador de idioma y un valor de la enumeración [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Registra los controladores para los eventos [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) y [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Estos eventos se generan cuando se alcanza el tiempo de inicio para una identificación y cuando la duración de una identificación ha expirado, respectivamente.

Crea un nuevo objeto de indicación, apropiado para el tipo de pista de metadatos que has creado, y establece el id., la hora de inicio y la duración de la pista. En este ejemplo se crea una pista de datos, por lo que se genera un conjunto de objetos [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) y se proporciona un búfer que contiene datos específicos de la aplicación. Para registrar la pista nueva, agrégala a la colección [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) del objeto **MediaSource**.

A partir de Windows 10, versión 1703, la propiedad **DataCue.Properties** expone un objeto [**PropertySet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset) que puedes usar para almacenar las propiedades personalizadas en pares de clave o datos que se pueden recuperar en los eventos **CueEntered** y **CueExited**.  

[!code-cs[AddDataTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

El evento **CueEntered** se genera cuando se ha alcanzado la hora de inicio de una indicación, siempre que la pista asociada tenga un modo de presentación de **ApplicationPresented**, **Hidden** o **PlatformPresented**. Los eventos de indicación no se generan para pistas de metadatos mientras el modo de presentación de la pista es **Disabled**. En este ejemplo, simplemente se envían a la ventana de depuración los datos personalizados asociados con la indicación.

[!code-cs[DataCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

En este ejemplo se agrega una pista de texto personalizado al especificar **TimedMetadataKind.Caption** durante la creación de la pista y usando objetos [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) para agregar indicaciones a la pista.

[!code-cs[AddTextTrack](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## <a name="play-a-list-of-media-items-with-mediaplaybacklist"></a>Reproducir una lista de elementos multimedia con MediaPlaybackList

El objeto [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite crear una lista de reproducción de elementos multimedia, que se representan con objetos **MediaPlaybackItem**.

**Nota**elementos en una [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) se representan mediante la reproducción sin pausas. El sistema usará los metadatos proporcionados en archivos codificados MP3 o AAC para determinar la compensación del retraso o el espaciado interno necesaria para la reproducción sin pausas. Si los archivos codificados MP3 o AAC no proporcionan estos metadatos, el sistema determina el retraso o el espaciado interno de forma heurística. Para los formatos sin pérdida, como PCM, FLAC o ALAC, el sistema no realiza ninguna acción porque estos codificadores no introducen ningún retraso ni espaciado interno.

Para empezar, declara una variable para almacenar el objeto **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Crear un objeto **MediaPlaybackItem** para cada elemento multimedia que quieras agregar a tu lista usando el mismo procedimiento descrito anteriormente en este artículo. Inicializa el objeto **MediaPlaybackList** y agrégale los elementos de reproducción de contenido multimedia. Registra el controlador del evento [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Este evento permite actualizar la interfaz de usuario para reflejar el elemento multimedia que se encuentra en reproducción. También puedes registrarte en el evento [ItemOpened](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemOpened), que se genera cuando un elemento de la lista no se abre correctamente, y en el evento [ItemFailed](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.ItemFailed), que se genera cuando no se puede abrir un elemento en la lista.

A partir de Windows 10, versión 1703, puedes especificar el número máximo de objetos **MediaPlaybackItem** en el objeto **MediaPlaybackList** que el sistema mantendrá abiertos después de que se hayan reproducido si estableces la propiedad [MaxPlayedItemsToKeepOpen](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList.MaxPlayedItemsToKeepOpen). Cuando un objeto **MediaPlaybackItem** se mantiene abierto, la reproducción del elemento pueda iniciarse de forma inmediata cuando el usuario cambia a ese elemento porque no hace falta que el elemento se vuelva a cargar. Sin embargo, mantener los elementos abiertos aumenta el consumo de memoria de la aplicación, por lo que, al establecer este valor, deberías tener en cuenta el equilibrio entre la capacidad de respuesta y el uso de memoria. 

Para habilitar la reproducción de la lista, establece el origen de reproducción del objeto **MediaPlayer** en el objeto **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

En el controlador de eventos **CurrentItemChanged**, actualiza la interfaz de usuario para reflejar el elemento en reproducción, que se puede recuperar con la propiedad [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) del objeto [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) pasado al evento. Recuerda que si actualizas la interfaz de usuario de este evento, debes hacerlo dentro de una llamada a [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que las actualizaciones se hagan en el subproceso de la interfaz de usuario.

A partir de Windows 10, versión 1703, puedes comprobar la propiedad [CurrentMediaPlaybackItemChangedEventArgs.Reason](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.Reason) para obtener un valor que indica el motivo por el que el elemento cambió, por ejemplo, la aplicación cambia elementos mediante programación, el elemento en reproducción anterior alcanzó su fin o que se produce un error.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]


Llama a [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) o [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) para hacer que el reproductor de contenido multimedia reproduzca el elemento anterior o siguiente en tu objeto **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNextButton)]

Establece la propiedad [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) para especificar si el reproductor de contenido multimedia debe reproducir los elementos de la lista en orden aleatorio.

[!code-cs[ShuffleButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Establece la propiedad [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) para especificar si el reproductor de contenido multimedia debe repetir la reproducción de la lista.

[!code-cs[RepeatButton](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRepeatButton)]


### <a name="handle-the-failure-of-media-items-in-a-playback-list"></a>Controlar los errores de los elementos multimedia en una lista de reproducción
El evento [**ItemFailed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackList.ItemFailed) se genera cuando se produce un error al intentar abrir un elemento de la lista. La propiedad [**ErrorCode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError.ErrorCode) del objeto [**MediaPlaybackItemError**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackItemError) pasado al controlador enumera la causa específica del error cuando es posible, incluidos los errores de red, descodificación o de cifrado.

[!code-cs[ItemFailed](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetItemFailed)]

### <a name="disable-playback-of-items-in-a-playback-list"></a>Deshabilitar la reproducción de elementos de una lista de reproducción
A partir de Windows 10, versión 1703, puedes deshabilitar la reproducción de uno o más elementos en un objeto **MediaPlaybackItemList** si estableces la propiedad [IsDisabledInPlaybackList](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.IsDisabledInPlaybackList) de un objeto [MediaPlaybackItem](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem) en false. 

Un escenario típico de esta característica es para las aplicaciones que reproducen música emitida desde internet. La aplicación puede estar a la escucha de cambios en el estado de conexión de la red del dispositivo y deshabilitar la reproducción de elementos que no se descarguen por completo. En el ejemplo siguiente, se registra un controlador para el evento [NetworkInformation.NetworkStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.NetworkStatusChanged).

[!code-cs[RegisterNetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetRegisterNetworkStatusChanged)]

En el controlador para **NetworkStatusChanged**, comprueba si [GetInternetConnectionProfile](https://docs.microsoft.com/uwp/api/Windows.Networking.Connectivity.NetworkInformation.GetInternetConnectionProfile) devuelve un valor nulo, lo que indica que la red no está conectada. Si este es el caso, crea un bucle de todos los elementos de la lista de reproducción y, si el objeto [TotalDownloadProgress](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybackitem.TotalDownloadProgress) del elemento es menor que 1, lo que significa que el elemento no se descargó por completo, deshabilita el elemento. Si se habilita la conexión de red, crea un bucle de todos los elementos de la lista de reproducción y habilita cada elemento.

[!code-cs[NetworkStatusChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetNetworkStatusChanged)]

### <a name="defer-binding-of-media-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Aplazar el enlazado de contenido multimedia para los elementos de una lista de reproducción mediante MediaBinder
En los ejemplos anteriores, un objeto **MediaSource** se crea a partir de un archivo, una dirección URL o una emisión, tras el cual se crea un objeto **MediaPlaybackItem** y se agrega a un objeto **MediaPlaybackList**. En algunos escenarios, por ejemplo, si el usuario debe pagar por la visualización de contenido, es aconsejable aplazar la recuperación del contenido de un objeto **MediaSource** hasta que el elemento de la lista de reproducción esté listo para su reproducción. Para implementar este escenario, crea una instancia de la clase [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder). Establece la propiedad [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) en una cadena definida por la aplicación que identifique el contenido cuya recuperación quieras aplazar y luego registra un controlador para el evento [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Luego, crea un objeto **MediaSource** a partir del objeto **Binder** llamando a [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Luego, crea un objeto **MediaPlaybackItem** a partir del objeto **MediaSource** y agrégalo a la lista de reproducción, del modo habitual.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

Cuando el sistema determina que el contenido asociado al objeto **MediaBinder** se debe recuperar, generará el evento **Binding**. En el controlador de este evento, puedes recuperar la instancia de **MediaBinder** desde el objeto [**MediaBindingEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs) pasado al evento. Recupera la cadena que especificaste para la propiedad **Token** y úsala para determinar qué contenido se debe recuperar. El objeto **MediaBindingEventArgs** proporciona métodos para establecer el contenido enlazado en diferentes representaciones, como por ejemplo, [**SetStorageFile**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstoragefile), [**SetStream**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstream), [**SetStreamReference**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setstreamreference) y [**SetUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.seturi). 

[!code-cs[BinderBinding](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBinding)]

Ten en cuenta que si estás realizando operaciones asincrónicas, como solicitudes web, en el controlador de eventos **Binding**, debes llamar al método [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para indicar al sistema que debe esperar a que la operación se complete antes de continuar. Llama a [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) una vez finalizada la operación para indicar al sistema que debe continuar.

A partir de Windows 10, versión 1703, puedes proporcionar un objeto [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como contenido enlazado si llamas a [**SetAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource). Para más información sobre el uso de streaming adaptable en la aplicación, consulta [Streaming adaptable](adaptive-streaming.md).



## <a name="related-topics"></a>Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Integrar con los controles de transporte multimedia del sistema](integrate-with-systemmediatransportcontrols.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md)

