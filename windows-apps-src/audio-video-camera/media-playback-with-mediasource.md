---
author: drewbatgit
ms.assetid: C5623861-6280-4352-8F22-80EB009D662C
description: "La clase MediaSource proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde orígenes diferentes, tales como archivos locales o remotos, y expone un modelo común para acceder a datos multimedia, independientemente del formato del contenido multimedia subyacente."
title: "Reproducción de contenido multimedia con MediaSource"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: d64f4484566d80eaf2a353b1aba954c15079343c

---

# Reproducción de contenido multimedia con MediaSource

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Parte de la información hace referencia a la versión preliminar del producto, el cual puede sufrir importantes modificaciones antes de que se publique la versión comercial. Microsoft no ofrece ninguna garantía, expresa o implícita, con respecto a la información que se ofrece aquí.\]

La clase [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) proporciona una forma común de hacer referencia a contenido multimedia y reproducirlo desde orígenes diferentes, tales como archivos locales o remotos, y expone un modelo común para acceder a datos multimedia, independientemente del formato del contenido multimedia subyacente. La clase [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939) amplía la funcionalidad del objeto **MediaSource**, lo que permite administrar y seleccionar entre varias pistas de audio, vídeo y metadatos incluidos en un elemento multimedia. [
              **MediaPlaybackList**
            ](https://msdn.microsoft.com/library/windows/apps/dn930955) permite crear listas de reproducción desde uno o más elementos de la reproducción de contenido multimedia.

El código de este artículo es una adaptación de la muestra [Video Playback SDK](http://go.microsoft.com/fwlink/p/?LinkId=620020&clcid=0x409) (SDK de reproducción de vídeo). Puedes descargar esta muestra para ver el código en contexto o para usarla como punto de partida para tu propia aplicación.

## Crear y reproducir un MediaSource

Crea una nueva instancia del objeto **MediaSource** con una llamada a uno de los métodos de fábrica expuestos por la clase:

-   [**CreateFromAdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930906)
-   [**CreateFromIMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn965527)
-   [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907)
-   [**CreateFromMseStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930908)
-   [**CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909)
-   [**CreateFromStream**](https://msdn.microsoft.com/library/windows/apps/dn930910)
-   [**CreateFromStreamReference**](https://msdn.microsoft.com/library/windows/apps/dn930911)
-   [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912)

Después de crear un objeto **MediaSource**, puedes reproducir el origen directamente con un objeto [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926), llamando a [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085) o con un objeto [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/dn652535) estableciendo la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/dn987010). En el siguiente ejemplo se muestra cómo reproducir un archivo multimedia seleccionado por el usuario en un objeto **MediaElement** con **MediaSource**.

Para completar este escenario, tendrás que incluir los espacios de nombres [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) y [**Windows.Media.Playback**](https://msdn.microsoft.com/library/windows/apps/dn640562).

[!code-cs[Uso](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

Declara una variable de tipo **MediaSource**. Para los ejemplos de este artículo, el origen de contenido multimedia se declara como un miembro de clase para que se puede acceder a él desde varias ubicaciones.

[!code-cs[DeclareMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaSource)]

Para permitir al usuario seleccionar un archivo multimedia para su reproducción, usa un objeto [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847). Con el objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) devuelto desde el método [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275) del selector, inicializa un nuevo objeto MediaObject con una llamada a [**MediaSource.CreateFromStorageFile**](https://msdn.microsoft.com/library/windows/apps/dn930909). Por último, establece el origen del contenido multimedia como el origen de reproducción del objeto **MediaElement** con una llamada al método [**SetPlaybackSource**](https://msdn.microsoft.com/library/windows/apps/dn899085).

[!code-cs[PlayMediaSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaSource)]

## Controlar varias pistas de audio, vídeo y metadatos con MediaPlaybackItem

El uso de un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/dn930905) para la reproducción es conveniente porque proporciona una forma común de reproducir contenido multimedia desde diferentes tipos de orígenes, pero puedes acceder a un comportamiento más avanzado mediante un objeto [**MediaPlaybackItem**](https://msdn.microsoft.com/library/windows/apps/dn930939). Esto incluye la capacidad de acceder a varias pistas de audio, vídeo y datos de un elemento multimedia, y administrarlas.

Declara una variable para almacenar el objeto **MediaPlaybackItem**.

[!code-cs[DeclareMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackItem)]

Crea un objeto **MediaPlaybackItem** con una llamada al constructor y pasándolo en un objeto **MediaSource** inicializado.

Si tu aplicación admite varias pistas de audio, vídeo o datos en un elemento de reproducción de contenido multimedia, registra controladores de eventos para los eventos [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948), [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) o [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952).

Por último, establece el origen de reproducción de la clase **MediaElement** o **MediaPlayer** en tu **MediaPlaybackItem**.

[!code-cs[PlayMediaPlaybackItem](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackItem)]

**Nota**  
Un objeto **MediaSource** solo se puede asociar con un solo objeto **MediaPlaybackItem**. Después de crear un objeto **MediaPlaybackItem** a partir de un origen, si intentas crear otro elemento de reproducción desde el mismo origen, se generará un error. Además, después de crear un objeto **MediaPlaybackItem** desde un origen de contenido multimedia, no puedes establecer el objeto **MediaSource** directamente como el origen de un objeto **MediaElement** o **MediaPlayer**. En cambio, deberías usar el objeto **MediaPlaybackItem**.

El evento [**VideoTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930954) se genera después de que un objeto **MediaPlaybackItem** que contiene varias pistas de vídeos se asigne como origen de reproducción y se puede volver a lanzar si la lista de pistas de vídeo cambia para los cambios de elemento. El controlador de este evento te permite actualizar la interfaz de usuario para permitir al usuario cambiar entre pistas disponibles. En este ejemplo se usa un objeto [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/br209348) para mostrar las pistas de vídeo disponibles.

[!code-xml[VideoComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetVideoComboBox)]

En el controlador **VideoTracksChanged**, repite todas las pistas de la lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) del elemento de reproducción. Para cada pista, se crea un nuevo objeto [**ComboBoxItem**](https://msdn.microsoft.com/library/windows/apps/br209349). Si la pista todavía no tiene etiqueta, esta se genera a partir del índice de pistas. La propiedad [**Tag**](https://msdn.microsoft.com/library/windows/apps/br208745) del elemento de cuadro combinado se establece en el índice de pistas para que se pueda identificar más adelante. Por último, el elemento se agrega al cuadro combinado. Ten en cuenta que estas operaciones se realizan en una llamada [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) porque todos los cambios de la interfaz de usuario deben realizarse en el subproceso de interfaz de usuario y este evento se lanza en un subproceso diferente.

[!code-cs[VideoTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksChanged)]

En el controlador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/br209776) del cuadro combinado, el índice de pistas se recupera de la propiedad **Tag** del elemento seleccionado. Si estableces la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn956634) en la lista [**VideoTracks**](https://msdn.microsoft.com/library/windows/apps/dn930953) del elemento de reproducción de contenido multimedia, el objeto **MediaElement** o **MediaPlayer** cambiará la pista de vídeo activa al índice especificado.

[!code-cs[VideoTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetVideoTracksSelectionChanged)]

La administración de elementos multimedia con varias pistas de audio funciona exactamente del mismo modo que con las pistas de vídeo. Controla el objeto [**AudioTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930948) para actualizar la interfaz de usuario con las pistas de audio que se encuentran en la lista [**AudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn930947) del elemento de reproducción. Cuando el usuario selecciona una pista de audio, establece la propiedad [**SelectedIndex**](https://msdn.microsoft.com/library/windows/apps/dn930937) de la lista **AudioTracks** para hacer que el objeto **MediaElement** o **MediaPlayer** cambie la pista de audio activa al índice especificado.

[!code-xml[AudioComboBox](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetAudioComboBox)]

[!code-cs[AudioTracksChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksChanged)]

[!code-cs[AudioTracksSelectionChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAudioTracksSelectionChanged)]

Además de contenido de audio y vídeo, un objeto **MediaPlaybackItem** puede contener cero o más objetos [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580). Una pista de metadatos temporizada puede contener texto de subtítulo o título, o puede contener datos personalizados que son propiedad de la aplicación. Una pista de metadatos temporizada contiene una lista de indicaciones representadas por objetos que heredan del objeto [**IMediaCue**](https://msdn.microsoft.com/library/windows/apps/dn930899), como un objeto [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) o [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655). Cada indicación tiene una hora de inicio y una duración que determina el momento de activación de la indicación y durante cuánto tiempo.

Similar a las pistas de audio y vídeos, las pistas de metadatos temporizadas de un elemento multimedia se pueden descubrir al controlar el evento [**TimedMetadataTracksChanged**](https://msdn.microsoft.com/library/windows/apps/dn930952) de un objeto **MediaPlaybackItem**. Sin embargo, con las pistas de metadatos temporizadas, quizás el usuario quiera habilitar más de una pista de metadatos a la vez. Además, en función del escenario de la aplicación, es posible que quieras habilitar o deshabilitar las pitas de metadatos automáticamente, sin la intervención del usuario. Para fines de ilustración, en este ejemplo se agrega un objeto [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/br209795) para cada pista de metadatos en un elemento multimedia de modo que el usuario pueda habilitar y deshabilitar la pista. La propiedad **Tag** de cada botón se establece en el índice de la pista de metadatos asociada para que pueda identificarse cuando se active o desactive el botón.

[!code-xml[MetaStackPanel](./code/MediaSource_Win10/cs/MainPage.xaml#SnippetMetaStackPanel)]

[!code-cs[TimedMetadataTrackschanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedMetadataTrackschanged)]

Dado que puede haber más de una pista de metadatos activa a la vez, no se establece simplemente el índice activo de la lista de pistas de metadatos. En cambio, llama al método [**SetPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn986977) del objeto **MediaPlaybackItem**, pasando el índice de la pista que quieras cambiar y luego proporcionando un valor de la enumeración [**TimedMetadataTrackPresentationMode**](https://msdn.microsoft.com/library/windows/apps/dn987016). El modo de presentación que elijas depende de la implementación de la aplicación. En este ejemplo, la pista de metadatos se establece en **PlatformPresented** cuando está habilitada. Para las pistas basadas en texto, esto significa que el sistema mostrará automáticamente las indicaciones de texto de la pista. Cuando se desactiva el botón de alternancia, el modo de presentación se establece en **Disabled**, lo que significa que no se muestra ningún texto y no se genera ningún evento de indicación. Los eventos de indicación se describen más adelante en este artículo.

[!code-cs[ToggleChecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleChecked)]

[!code-cs[ToggleUnchecked](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetToggleUnchecked)]

## Agregar texto temporizado externo con TimedTextSource

En algunos escenarios, puedes tener archivos externos que contienen texto temporizado asociada con un elemento multimedia, tales como archivos independientes que contienen los subtítulos en las distintas configuraciones regionales. Usa la clase [**TimedTextSource**](https://msdn.microsoft.com/library/windows/apps/dn956679) para cargarla en archivos de texto temporizado externos de una secuencia o un URI.

En este ejemplo, se usa una colección **Dictionary** para almacenar una lista de los orígenes de texto temporizado del elemento multimedia con el URI de origen y el objeto **TimedTextSource** como el par clave y valor con el fin de identificar las pistas después de que se hayan resuelto.

[!code-cs[TimedTextSourceMap](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceMap)]

Crea un nuevo objeto **TimedTextSource** para cada archivo de texto temporizado externo con una llamada a [**CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn708190). Agrega una entrada al objeto **Dictionary** para el origen de texto temporizado. Agrega un controlador para el evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540) con el fin de controlar si el elemento no se pudo cargar o para establecer propiedades adicionales después de que el elemento se haya cargado correctamente.

Registra todos los objetos **TimedTextSource** en el objeto **MediaSource** agregándolos a la colección [**ExternalTimedTextSources**](https://msdn.microsoft.com/library/windows/apps/dn930916). Ten en cuenta que los orígenes de texto temporizado externos se agregan directamente al objeto **MediaSource** y no al objeto **MediaPlaybackItem** creado a partir del origen. Para actualizar la interfaz de usuario y reflejar las pistas de texto externo, registra y controla el evento **TimedMetadataTracksChanged** según se describió anteriormente en este artículo.

[!code-cs[TimedTextSource](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSource)]

En el controlador del evento [**TimedTextSource.Resolved**](https://msdn.microsoft.com/library/windows/apps/dn965540), comprueba la propiedad **Error** del objeto [**TimedTextSourceResolveResultEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn965537) que se pasó al controlador para determinar si se produjo un error al intentar cargar los datos de texto temporizado. Si el elemento se resolvió correctamente, puedes usar este controlador para actualizar las propiedades adicionales de la pista resuelta. En este ejemplo se agrega una etiqueta para cada pista en función del URI almacenado anteriormente en el objeto **Dictionary**.

[!code-cs[TimedTextSourceResolved](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTimedTextSourceResolved)]

## Agregar pistas de metadatos adicionales

Puedes crear pistas de metadatos personalizadas dinámicamente en el código y asociarlas con un origen de contenido multimedia. Las pistas que creas pueden contener texto de subtítulo o título, o pueden contener los datos de propiedad de la aplicación.

Crea un nuevo objeto [**TimedMetadataTrack**](https://msdn.microsoft.com/library/windows/apps/dn956580) con una llamada al constructor y especificando el identificador, el identificador de idioma y un valor de la enumeración [**TimedMetadataKind**](https://msdn.microsoft.com/library/windows/apps/dn956578). Registra los controladores para los eventos [**CueEntered**](https://msdn.microsoft.com/library/windows/apps/dn956583) y [**CueExited**](https://msdn.microsoft.com/library/windows/apps/dn956584). Estos eventos se generan cuando se alcanza el tiempo de inicio para una identificación y cuando la duración de una identificación ha expirado, respectivamente.

Crea un nuevo objeto de indicación, apropiado para el tipo de pista de metadatos creado, y establece el identificador, la hora de inicio y la duración de la pista. En este ejemplo se crea una pista de datos, por lo que se genera un conjunto de objetos [**DataCue**](https://msdn.microsoft.com/library/windows/apps/dn930892) y se proporciona para cada cola un búfer que contiene datos específicos de la aplicación. Para registrar la pista nueva, agrégala a la colección [**ExternalTimedMetadataTracks**](https://msdn.microsoft.com/library/windows/apps/dn930915) del objeto **MediaSource**.

[!code-cs[AddDataTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddDataTrack)]

El evento **CueEntered** se genera cuando se ha alcanzado la hora de inicio de una indicación, siempre que la pista asociada tenga un modo de presentación de **ApplicationPresented**, **Hidden** o **PlatformPresented**. Los eventos de indicación no se generan para pistas de metadatos mientras el modo de presentación de la pista es **Disabled**. En este ejemplo, simplemente se envían a la ventana de depuración los datos personalizados asociados con la indicación.

[!code-cs[DataCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDataCueEntered)]

En este ejemplo se agrega una pista de texto personalizado al especificar **TimedMetadataKind.Caption** durante la creación de la pista y usando objetos [**TimedTextCue**](https://msdn.microsoft.com/library/windows/apps/dn956655) para agregar indicaciones a la pista.

[!code-cs[AddTextTrack](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetAddTextTrack)]

[!code-cs[TextCueEntered](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetTextCueEntered)]

## Reproducir una lista de elementos multimedia con MediaPlaybackList

El objeto [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) permite crear una lista de reproducción de elementos multimedia, que se representan con objetos **MediaPlaybackItem**.

**Nota**  Los elementos de una [**MediaPlaybackList**](https://msdn.microsoft.com/library/windows/apps/dn930955) se representan mediante la reproducción sin pausas. El sistema usará los metadatos proporcionados en archivos codificados MP3 o AAC para determinar la compensación del retraso o el espaciado interno necesaria para la reproducción sin pausas. Si los archivos codificados MP3 o AAC no proporcionan estos metadatos, el sistema determina el retraso o el espaciado interno de forma heurística. Para los formatos sin pérdida, como PCM, FLAC o ALAC, el sistema no realiza ninguna acción porque estos codificadores no introducen ningún retraso ni espaciado interno.

Para empezar, declara una variable para almacenar el objeto **MediaPlaybackList**.

[!code-cs[DeclareMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaPlaybackList)]

Crear un objeto **MediaPlaybackItem** para cada elemento multimedia que quieras agregar a tu lista usando el mismo procedimiento descrito anteriormente en este artículo. Inicializa el objeto **MediaPlaybackList** y agrégale los elementos de reproducción de contenido multimedia. Registra el controlador del evento [**CurrentItemChanged**](https://msdn.microsoft.com/library/windows/apps/dn930957). Este evento permite actualizar la interfaz de usuario para reflejar el elemento multimedia en reproducción. Por último, establece el origen de reproducción de la clase **MediaElement** o **MediaPlayer** en tu **MediaPlaybackList**.

[!code-cs[PlayMediaPlaybackList](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPlayMediaPlaybackList)]

En el controlador de eventos **CurrentItemChanged**, actualiza la interfaz de usuario para reflejar el elemento en reproducción, que se puede recuperar con la propiedad [**NewItem**](https://msdn.microsoft.com/library/windows/apps/dn930930) del objeto [**CurrentMediaPlaybackItemChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn930929) pasado al evento. Recuerda que si actualizas la interfaz de usuario de este evento, deberías hacerlo dentro de una llamada a [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que las actualizaciones se hagan en el subproceso de la interfaz de usuario.

[!code-cs[MediaPlaybackListItemChanged](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetMediaPlaybackListItemChanged)]

Llama a [**MovePrevious**](https://msdn.microsoft.com/library/windows/apps/mt146455) o [**MoveNext**](https://msdn.microsoft.com/library/windows/apps/mt146454) para hacer que el reproductor de contenido multimedia reproduzca el elemento anterior o siguiente en tu objeto **MediaPlaybackList**.

[!code-cs[PrevButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetPrevButton)]

[!code-cs[NextButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetNextButton)]

Establece la propiedad [**ShuffleEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146457) para especificar si el reproductor de contenido multimedia debe reproducir los elementos de la lista en orden aleatorio.

[!code-cs[ShuffleButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetShuffleButton)]

Establece la propiedad en [**AutoRepeatEnabled**](https://msdn.microsoft.com/library/windows/apps/mt146452) para especificar si el reproductor de contenido multimedia debe repetir la reproducción de la lista.

[!code-cs[RepeatButton](./code/MediaSource_Win10/cs/MainPage.xaml.cs#SnippetRepeatButton)]

 

 







<!--HONumber=Jun16_HO4-->


