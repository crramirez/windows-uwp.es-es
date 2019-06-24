---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Las API en el espacio de nombres Windows.Media.Editing, te permiten desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo.
title: Composiciones y edición multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6a0b21ac4c2bc1a2278757cdaa542be39c01f481
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318251"
---
# <a name="media-compositions-and-editing"></a>Composiciones y edición multimedia



En este artículo te mostramos cómo usar las API en el espacio de nombres [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing), para desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo. Las características del marco incluyen la capacidad de anexar juntos varios clips de vídeo, agregar superposiciones de vídeo e imágenes, agregar audio en segundo plano y aplicar efectos de audio y vídeos mediante programación. Una vez creado, las composiciones multimedia pueden representarse en un archivo multimedia plano para la reproducción o uso compartido, pero composiciones también se serializa en y desde el disco, lo que permite al usuario cargar y modificar composiciones que han creado anteriormente. Toda esta funcionalidad se proporciona en una interfaz de Windows Runtime de uso fácil que reduce considerablemente la cantidad y la complejidad del código necesario para realizar estas tareas en comparación con el bajo nivel [Microsoft Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) API.

## <a name="create-a-new-media-composition"></a>Crear una nueva composición de multimedia

La clase [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition) es el contenedor de todos los clips multimedia que conforman la composición y es responsable de representar la composición final, cargar y guardar composiciones al disco y proporcionar una secuencia de vista previa de la composición para que el usuario pueda ver en la interfaz de usuario. Para usar la clase **MediaComposition** en tu aplicación, debes incluir el espacio de nombres [**Windows.Media.Editing**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing), así como el espacio de nombres [**Windows.Media.Core**](https://docs.microsoft.com/uwp/api/Windows.Media.Core) que proporciona las API relacionadas que necesitas.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


Puedes acceder al objeto **MediaComposition** desde varios puntos en el código, así que normalmente tendrás que declarar una variable de miembro en la que almacenar el objeto.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

El constructor de **MediaComposition** no toma argumentos.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Agregar clips multimedia a una composición

Las composiciones multimedia normalmente contienen uno o varios clips de vídeo. Puedes usar un [**FileOpenPicker**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) para permitir al usuario seleccionar un archivo de vídeo. Una vez que se haya seleccionado el archivo, crea un objeto [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) nuevo que contenga el clip de vídeo mediante una llamada a [**MediaClip.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync). A continuación, agrega el clip a la lista [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips) del objeto **MediaComposition**.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Los clips multimedia aparecen en el objeto **MediaComposition** en el mismo orden en que aparecen en la lista [**Clips**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.clips).

-   Un **MediaClip** solo se puede incluir en una composición de una vez. Intentar agregar un **MediaClip** que la composición ya está usando tendrá un error como resultado. Para volver a usar un clip de vídeo varias veces en una composición, llama a [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) para crear nuevos objetos **MediaClip** que puedan agregarse a la composición.

-   Las aplicaciones universales de Windows no tienen permiso para acceder al sistema de archivo completo. La propiedad [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la clase [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) permite que la aplicación almacene un registro de un archivo que el usuario haya seleccionado, por lo que puedes conservar los permisos de acceso al archivo. La **FutureAccessList** tiene un máximo de 1000 entradas, por lo que la aplicación necesita administrar la lista para asegurarse de que no se llene. Esto es especialmente importante si tienes previsto admitir la carga y modificar las composiciones creadas anteriormente.

-   Un **MediaComposition** admite clips de vídeo en formato MP4.

-   Si un archivo de vídeo contiene varias pistas de audio incrustadas, puedes seleccionar cuál pista de audio se debe usar en la composición estableciendo la propiedad [**SelectedEmbeddedAudioTrackIndex**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex).

-   Crea un **MediaClip** con un color único llenando todo el marco, mediante una llamada a [**CreateFromColor**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromcolor) y especifica un color y una duración para el clip.

-   Crea un **MediaClip** desde un archivo de imagen mediante una llamada a [**CreateFromImageFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) y especifica un archivo de imagen y una duración para el clip.

-   Crear un **MediaClip** desde una interfaz [**IDirect3DSurface**](https://docs.microsoft.com/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) llamando a [**CreateFromSurface**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromsurface) y especificando una superficie y una duración del clip.

## <a name="preview-the-composition-in-a-mediaelement"></a>Vista previa de la composición de un MediaElement

Para permitir al usuario ver la composición de multimedia, agrega una clase [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) al archivo XAML que defina tu interfaz de usuario.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Declara una variable de miembro de tipo [**MediaStreamSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaStreamSource).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Llama el método [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) del objeto **MediaComposition** para crear un **MediaStreamSource** para la composición. Crea un objeto [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) llamando al método de fábrica [**CreateFromMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) y asígnalo a la propiedad [**origen**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de **MediaPlayerElement**. La composición se puede ver en la interfaz de usuario.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   El objeto **MediaComposition** debe contener al menos un clip multimedia antes de llamar al método [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource), o el valor del objeto devuelto será "null".

-   La escala de tiempo **MediaElement** no se actualiza automáticamente para reflejar los cambios en la composición. Se recomienda que llame a ambos **GeneratePreviewMediaStreamSource** y establezca el **MediaPlayerElement** **origen** propiedad cada vez que realiza un conjunto de cambios en el composición y desea actualizar la interfaz de usuario.

Te recomendamos que establezcas el objeto **MediaStreamSource** y la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.source) de la clase **MediaPlayerElement** en null cuando el usuario abandone la página para liberar los recursos asociados.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Procesar la composición a un archivo de vídeo

Para representar una composición multimedia en un archivo plano de vídeo para que se pueda compartir y ver en otros dispositivos, tendrás que usar las API del espacio de nombres [**Windows.Media.Transcoding**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding). Para actualizar la interfaz de usuario en el progreso de la operación asincrónica, también necesitarás la API del espacio de nombres [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Después de permitir al usuario seleccionar un archivo de salida mediante una clase [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker), procesa la composición al archivo seleccionado mediante una llamada al método [**RenderToFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.rendertofileasync) del objeto **MediaComposition**. El resto del código del siguiente ejemplo simplemente sigue el patrón de control de un [**AsyncOperationWithProgress**](https://docs.microsoft.com/previous-versions/br205807(v=vs.85)).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   La enumeración [**MediaTrimmingPreference**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) permite dar prioridad a la velocidad de la operación de transcodificación en función de la precisión de recorte de clips multimedia adyacentes. **Fast** hace que la transcodificación sea más rápida con el recorte de menor precisión, mientras que **Precise** hace que la transcodificación sea más lenta pero con el recorte más preciso.

## <a name="trim-a-video-clip"></a>Recortar un clip de vídeo

Recorta la duración de un clip de vídeo en una composición estableciendo los objetos [**MediaClip**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaClip) la propiedad [**TrimTimeFromStart**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromstart), la propiedad [**TrimTimeFromEnd**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimtimefromend) o ambas.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Puedes usar cualquier interfaz de usuario que quieras para que el usuario pueda especificar el inicio y fin del recorte de valores. El ejemplo anterior usa la propiedad [**Position**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) de la clase [**MediaPlaybackSession**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackSession) asociada con **MediaPlayerElement** para determinar primero qué **MediaClip** se reproduce en la posición actual en la composición comprobando la [**StartTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) y [**EndTimeInComposition**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.endtimeincomposition). Entonces, las propiedades **Position** y **StartTimeInComposition** se usan de nuevo para calcular la cantidad de tiempo para recortar desde el principio el clip. El método **FirstOrDefault** es un método de extensión del espacio de nombres **System.Linq** que simplifica el código para seleccionar elementos en una lista.
-   La propiedad [**OriginalDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.originalduration) del objeto **MediaClip** permite conocer la duración del clip multimedia sin ningún recorte aplicado.
-   La propiedad [**TrimmedDuration**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.trimmedduration) permite conocer la duración de los clip multimedia después de que se aplique el recorte.
-   Especificar un valor de recorte que sea mayor que la duración de la imagen original no produce un error. Sin embargo, si una composición contiene un único clip y tal clip se recorta a longitud cero, especificando un valor de recorte grande, una llamada posterior a [**GeneratePreviewMediaStreamSource**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) devolverá un valor null, como si la composición no tuviera clips.

## <a name="add-a-background-audio-track-to-a-composition"></a>Agregar una pista de audio de fondo a una composición

Para agregar una pista de fondo a una composición, carga un archivo de audio y, a continuación, crea un objeto [**BackgroundAudioTrack**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) llamando al método de fábrica [**BackgroundAudioTrack.CreateFromFileAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync). A continuación, agrega la clase **BackgroundAudioTrack** a la propiedad de la composición [**BackgroundAudioTracks**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks).

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Un **MediaComposition** admite en segundo plano las pistas de audio en los siguientes formatos: MP3, WAV, FLAC

-   Una pista de audio de fondo

-   Al igual que con archivos de vídeo, debes usar la clase [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) para preservar el acceso a archivos en la composición.

-   Al igual que con la clase **MediaClip**, **BackgroundAudioTrack** solo se puede incluir en una composición de una vez. Intentar agregar una clase **BackgroundAudioTrack** que la composición ya está usando tendrá un error como resultado. Para volver a usar una pista de audio varias veces en una composición, llama a [**Clone**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.clone) para crear nuevos objetos **MediaClip** que se puedan agregar a la composición.

-   De manera predeterminada, las pistas de audio de fondo inician la reproducción en el inicio de la composición. Si hay varias pistas de segundo plano, todas las pistas de empezará a reproducir en el inicio de la composición. Para hacer que una pista de audio de fondo empiece la reproducción en otro momento, establece la propiedad [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.backgroundaudiotrack.delay) para el desplazamiento de tiempo deseado.

## <a name="add-an-overlay-to-a-composition"></a>Agregar una superposición a una composición

Las superposiciones permiten varias capas de vídeo entre sí de una composición de la pila. Una composición puede contener varias capas de superposición, cada una de ellas puede incluir varias superposiciones. Crear un objeto [**MediaOverlay**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlay) pasando un **MediaClip** en su constructor. Establece la posición y la opacidad de la superposición, para después crear una nueva clase [**MediaOverlayLayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaOverlayLayer) y agregar la clase **MediaOverlay** a su lista [**Overlays**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays). Por último, agrega la clase **MediaOverlayLayer** a la lista [**OverlayLayers**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.overlaylayers) de la composición.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Las superposiciones dentro de una capa están ordenadas desde la z, en función del orden en la lista de capa contenedora **Overlays**. Los índices más altos dentro de la lista se representan por encima de índices inferiores. Lo mismo pasa con las capas de superposición de una composición. Una capa con mayor índice en la lista **OverlayLayers** de la composición se representará en índices inferiores.

-   Como las superposiciones están apiladas unas sobre otras en lugar de que se reproduzcan de forma secuencial, todas las superposiciones inician la reproducción al principio de la composición de manera predeterminada. Para hacer una superposición con el fin de empezar la reproducción en otro momento, establece la propiedad [**Delay**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaoverlay.delay) para el desplazamiento de tiempo deseado.

## <a name="add-effects-to-a-media-clip"></a>Agregar efectos a un clip multimedia

Cada **MediaClip** en una composición tiene una lista de los efectos de audio y vídeo a la que se pueden agregar varios efectos. Debes implementar los efectos [**IAudioEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) y [**IVideoEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) respectivamente. En el siguiente ejemplo se usa la posición actual del **MediaPlayerElement** para elegir la vista actual de **MediaClip** y, a continuación, se crea una nueva instancia de la clase [**VideoStabilizationEffectDefinition**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) y la anexa a la lista [**VideoEffectDefinitions**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) del clip multimedia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Guardar una composición en un archivo

Puedes serializar las composiciones multimedia en un archivo y modificarlo más adelante. Para ello, selecciona un archivo de salida y, a continuación, llama al método [**SaveAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.saveasync) de la clase [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition), para guardar la composición.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Cargar una composición desde un archivo

Se pueden deserializar composiciones multimedia desde un archivo para que el usuario pueda ver y modificar la composición. Para ello, selecciona un archivo de composición y, a continuación, llama al método [**LoadAsync**](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition.loadasync) de la clase [**MediaComposition**](https://docs.microsoft.com/uwp/api/Windows.Media.Editing.MediaComposition), para cargar la composición.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Si un archivo multimedia de la composición no está en una ubicación a la que pueda acceder tu aplicación y no está en la propiedad [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la clase [**StorageApplicationPermissions**](https://docs.microsoft.com/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) de tu aplicación, se producirá un error al cargar la composición.

 

 




