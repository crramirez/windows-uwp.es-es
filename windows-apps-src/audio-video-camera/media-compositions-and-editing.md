---
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Las API en el espacio de nombres Windows.Media.Editing, te permiten desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo.
title: Composiciones y edición multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b1ae11d8f065cf72202365c36a9d69164fc2daa3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163879"
---
# <a name="media-compositions-and-editing"></a>Composiciones y edición multimedia



En este artículo te mostramos cómo usar las API en el espacio de nombres [**Windows.Media.Editing**](/uwp/api/Windows.Media.Editing), para desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo. Las características del marco incluyen la capacidad de anexar juntos varios clips de vídeo, agregar superposiciones de vídeo e imágenes, agregar audio en segundo plano y aplicar efectos de audio y vídeos mediante programación. Una vez creado, las composiciones multimedia pueden representarse en un archivo multimedia plano para la reproducción o uso compartido, pero composiciones también se serializa en y desde el disco, lo que permite al usuario cargar y modificar composiciones que han creado anteriormente. Toda esta funcionalidad se proporciona en una interfaz de Windows Runtime de uso fácil que reduce considerablemente la cantidad y la complejidad del código necesario para realizar estas tareas en comparación con el bajo nivel [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) API.

## <a name="create-a-new-media-composition"></a>Crear una nueva composición de multimedia

La clase [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition) es el contenedor de todos los clips multimedia que conforman la composición y es responsable de representar la composición final, cargar y guardar composiciones al disco y proporcionar una secuencia de vista previa de la composición para que el usuario pueda ver en la interfaz de usuario. Para usar la clase **MediaComposition** en tu aplicación, debes incluir el espacio de nombres [**Windows.Media.Editing**](/uwp/api/Windows.Media.Editing), así como el espacio de nombres [**Windows.Media.Core**](/uwp/api/Windows.Media.Core) que proporciona las API relacionadas que necesitas.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


Puedes acceder al objeto **MediaComposition** desde varios puntos en el código, así que normalmente tendrás que declarar una variable de miembro en la que almacenar el objeto.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

El constructor de **MediaComposition** no toma argumentos.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Agregar clips multimedia a una composición

Las composiciones multimedia normalmente contienen uno o varios clips de vídeo. Puedes usar un [**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker) para permitir al usuario seleccionar un archivo de vídeo. Una vez que se haya seleccionado el archivo, crea un objeto [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) nuevo que contenga el clip de vídeo mediante una llamada a [**MediaClip.CreateFromFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromfileasync). A continuación, agrega el clip a la lista [**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips) del objeto **MediaComposition**.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Los clips multimedia aparecen en el objeto **MediaComposition** en el mismo orden en que aparecen en la lista [**Clips**](/uwp/api/windows.media.editing.mediacomposition.clips).

-   Un **MediaClip** solo se puede incluir en una composición de una vez. Intentar agregar un **MediaClip** que la composición ya está usando tendrá un error como resultado. Para volver a usar un clip de vídeo varias veces en una composición, llama a [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) para crear nuevos objetos **MediaClip** que puedan agregarse a la composición.

-   Las aplicaciones universales de Windows no tienen permiso para acceder al sistema de archivo completo. La propiedad [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la clase [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) permite que la aplicación almacene un registro de un archivo que el usuario haya seleccionado, por lo que puedes conservar los permisos de acceso al archivo. La **FutureAccessList** tiene un máximo de 1000 entradas, por lo que la aplicación necesita administrar la lista para asegurarse de que no se llene. Esto es especialmente importante si tienes previsto admitir la carga y modificar las composiciones creadas anteriormente.

-   Un **MediaComposition** admite clips de vídeo en formato MP4.

-   Si un archivo de vídeo contiene varias pistas de audio incrustadas, puedes seleccionar cuál pista de audio se debe usar en la composición estableciendo la propiedad [**SelectedEmbeddedAudioTrackIndex**](/uwp/api/windows.media.editing.mediaclip.selectedembeddedaudiotrackindex).

-   Crea un **MediaClip** con un color único llenando todo el marco, mediante una llamada a [**CreateFromColor**](/uwp/api/windows.media.editing.mediaclip.createfromcolor) y especifica un color y una duración para el clip.

-   Crea un **MediaClip** desde un archivo de imagen mediante una llamada a [**CreateFromImageFileAsync**](/uwp/api/windows.media.editing.mediaclip.createfromimagefileasync) y especifica un archivo de imagen y una duración para el clip.

-   Crear un **MediaClip** desde una interfaz [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) llamando a [**CreateFromSurface**](/uwp/api/windows.media.editing.mediaclip.createfromsurface) y especificando una superficie y una duración del clip.

## <a name="preview-the-composition-in-a-mediaelement"></a>Vista previa de la composición de un MediaElement

Para permitir al usuario ver la composición de multimedia, agrega una clase [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) al archivo XAML que defina tu interfaz de usuario.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Declare una variable miembro de tipo [**MediaStreamSource**](/uwp/api/Windows.Media.Core.MediaStreamSource).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Llama el método [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) del objeto **MediaComposition** para crear un **MediaStreamSource** para la composición. Crea un objeto [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) llamando al método de fábrica [**CreateFromMediaStreamSource**](/uwp/api/windows.media.core.mediasource.createfrommediastreamsource) y asígnalo a la propiedad [**origen**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source) de **MediaPlayerElement**. La composición se puede ver en la interfaz de usuario.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   El objeto **MediaComposition** debe contener al menos un clip multimedia antes de llamar al método [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource), o el valor del objeto devuelto será "null".

-   La escala de tiempo **MediaElement** no se actualiza automáticamente para reflejar los cambios en la composición. Se recomienda que llames a **GeneratePreviewMediaStreamSource** y que definas la propiedad **MediaPlayerElement** **Source** cada vez que realices un conjunto de cambios en la composición y si deseas actualizar la interfaz de usuario.

Te recomendamos que establezcas el objeto **MediaStreamSource** y la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.source) de la clase **MediaPlayerElement** en null cuando el usuario abandone la página para liberar los recursos asociados.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Procesar la composición a un archivo de vídeo

Para representar una composición multimedia en un archivo plano de vídeo para que se pueda compartir y ver en otros dispositivos, tendrás que usar las API del espacio de nombres [**Windows.Media.Transcoding**](/uwp/api/Windows.Media.Transcoding). Para actualizar la interfaz de usuario en el progreso de la operación asincrónica, también necesitarás la API del espacio de nombres [**Windows.UI.Core**](/uwp/api/Windows.UI.Core).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Después de permitir al usuario seleccionar un archivo de salida mediante una clase [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker), procesa la composición al archivo seleccionado mediante una llamada al método [**RenderToFileAsync**](/uwp/api/windows.media.editing.mediacomposition.rendertofileasync) del objeto **MediaComposition**. El resto del código del siguiente ejemplo simplemente sigue el patrón de control de un [**AsyncOperationWithProgress**](/previous-versions/br205807(v=vs.85)).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   La enumeración [**MediaTrimmingPreference**](/uwp/api/Windows.Media.Editing.MediaTrimmingPreference) permite dar prioridad a la velocidad de la operación de transcodificación en función de la precisión de recorte de clips multimedia adyacentes. **Fast** hace que la transcodificación sea más rápida con el recorte de menor precisión, mientras que **Precise** hace que la transcodificación sea más lenta pero con el recorte más preciso.

## <a name="trim-a-video-clip"></a>Recortar un clip de vídeo

Recorta la duración de un clip de vídeo en una composición estableciendo los objetos [**MediaClip**](/uwp/api/Windows.Media.Editing.MediaClip) la propiedad [**TrimTimeFromStart**](/uwp/api/windows.media.editing.mediaclip.trimtimefromstart), la propiedad [**TrimTimeFromEnd**](/uwp/api/windows.media.editing.mediaclip.trimtimefromend) o ambas.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Puedes usar cualquier interfaz de usuario que quieras para que el usuario pueda especificar el inicio y fin del recorte de valores. El ejemplo anterior usa la propiedad [**Position**](/uwp/api/windows.media.playback.mediaplaybacksession.position) de la clase [**MediaPlaybackSession**](/uwp/api/Windows.Media.Playback.MediaPlaybackSession) asociada con **MediaPlayerElement** para determinar primero qué **MediaClip** se reproduce en la posición actual en la composición comprobando la [**StartTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.starttimeincomposition) y [**EndTimeInComposition**](/uwp/api/windows.media.editing.mediaclip.endtimeincomposition). Entonces, las propiedades **Position** y **StartTimeInComposition** se usan de nuevo para calcular la cantidad de tiempo para recortar desde el principio el clip. El método **FirstOrDefault** es un método de extensión del espacio de nombres **System.Linq** que simplifica el código para seleccionar elementos en una lista.
-   La propiedad [**OriginalDuration**](/uwp/api/windows.media.editing.mediaclip.originalduration) del objeto **MediaClip** permite conocer la duración del clip multimedia sin ningún recorte aplicado.
-   La propiedad [**TrimmedDuration**](/uwp/api/windows.media.editing.mediaclip.trimmedduration) permite conocer la duración de los clip multimedia después de que se aplique el recorte.
-   Especificar un valor de recorte que sea mayor que la duración de la imagen original no produce un error. Sin embargo, si una composición contiene un único clip y tal clip se recorta a longitud cero, especificando un valor de recorte grande, una llamada posterior a [**GeneratePreviewMediaStreamSource**](/uwp/api/windows.media.editing.mediacomposition.generatepreviewmediastreamsource) devolverá un valor null, como si la composición no tuviera clips.

## <a name="add-a-background-audio-track-to-a-composition"></a>Agregar una pista de audio de fondo a una composición

Para agregar una pista de fondo a una composición, carga un archivo de audio y, a continuación, crea un objeto [**BackgroundAudioTrack**](/uwp/api/Windows.Media.Editing.BackgroundAudioTrack) llamando al método de fábrica [**BackgroundAudioTrack.CreateFromFileAsync**](/uwp/api/windows.media.editing.backgroundaudiotrack.createfromfileasync). A continuación, agrega la clase **BackgroundAudioTrack** a la propiedad de la composición [**BackgroundAudioTracks**](/uwp/api/windows.media.editing.mediacomposition.backgroundaudiotracks).

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Un objeto **MediaComposition** admite, en segundo plano, pistas de audio en los siguientes formatos: MP3, WAV, FLAC

-   Una pista de audio de fondo

-   Al igual que con archivos de vídeo, debes usar la clase [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) para preservar el acceso a archivos en la composición.

-   Al igual que con la clase **MediaClip**, **BackgroundAudioTrack** solo se puede incluir en una composición de una vez. Intentar agregar una clase **BackgroundAudioTrack** que la composición ya está usando tendrá un error como resultado. Para volver a usar una pista de audio varias veces en una composición, llama a [**Clone**](/uwp/api/windows.media.editing.mediaclip.clone) para crear nuevos objetos **MediaClip** que se puedan agregar a la composición.

-   De manera predeterminada, las pistas de audio de fondo inician la reproducción en el inicio de la composición. Si hay varias pistas de segundo plano, todas las pistas de empezará a reproducir en el inicio de la composición. Para hacer que una pista de audio de fondo empiece la reproducción en otro momento, establece la propiedad [**Delay**](/uwp/api/windows.media.editing.backgroundaudiotrack.delay) para el desplazamiento de tiempo deseado.

## <a name="add-an-overlay-to-a-composition"></a>Agregar una superposición a una composición

Las superposiciones permiten varias capas de vídeo entre sí de una composición de la pila. Una composición puede contener varias capas de superposición, cada una de ellas puede incluir varias superposiciones. Crear un objeto [**MediaOverlay**](/uwp/api/Windows.Media.Editing.MediaOverlay) pasando un **MediaClip** en su constructor. Establece la posición y la opacidad de la superposición, para después crear una nueva clase [**MediaOverlayLayer**](/uwp/api/Windows.Media.Editing.MediaOverlayLayer) y agregar la clase **MediaOverlay** a su lista [**Overlays**](/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgioutput2-supportsoverlays). Por último, agrega la clase **MediaOverlayLayer** a la lista [**OverlayLayers**](/uwp/api/windows.media.editing.mediacomposition.overlaylayers) de la composición.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Las superposiciones dentro de una capa están ordenadas desde la z, en función del orden en la lista de capa contenedora **Overlays**. Los índices más altos dentro de la lista se representan por encima de índices inferiores. Lo mismo pasa con las capas de superposición de una composición. Una capa con mayor índice en la lista **OverlayLayers** de la composición se representará en índices inferiores.

-   Como las superposiciones están apiladas unas sobre otras en lugar de que se reproduzcan de forma secuencial, todas las superposiciones inician la reproducción al principio de la composición de manera predeterminada. Para hacer una superposición con el fin de empezar la reproducción en otro momento, establece la propiedad [**Delay**](/uwp/api/windows.media.editing.mediaoverlay.delay) para el desplazamiento de tiempo deseado.

## <a name="add-effects-to-a-media-clip"></a>Agregar efectos a un clip multimedia

Cada **MediaClip** en una composición tiene una lista de los efectos de audio y vídeo a la que se pueden agregar varios efectos. Debes implementar los efectos [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) y [**IVideoEffectDefinition**](/uwp/api/Windows.Media.Effects.IVideoEffectDefinition) respectivamente. En el siguiente ejemplo se usa la posición actual del **MediaPlayerElement** para elegir la vista actual de **MediaClip** y, a continuación, se crea una nueva instancia de la clase [**VideoStabilizationEffectDefinition**](/uwp/api/Windows.Media.Core.VideoStabilizationEffectDefinition) y la anexa a la lista [**VideoEffectDefinitions**](/uwp/api/windows.media.editing.mediaclip.videoeffectdefinitions) del clip multimedia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Guardar una composición en un archivo

Puedes serializar las composiciones multimedia en un archivo y modificarlo más adelante. Para ello, selecciona un archivo de salida y, a continuación, llama al método [**SaveAsync**](/uwp/api/windows.media.editing.mediacomposition.saveasync) de la clase [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition), para guardar la composición.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Cargar una composición desde un archivo

Se pueden deserializar composiciones multimedia desde un archivo para que el usuario pueda ver y modificar la composición. Para ello, selecciona un archivo de composición y, a continuación, llama al método [**LoadAsync**](/uwp/api/windows.media.editing.mediacomposition.loadasync) de la clase [**MediaComposition**](/uwp/api/Windows.Media.Editing.MediaComposition), para cargar la composición.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Si un archivo multimedia de la composición no está en una ubicación a la que pueda acceder tu aplicación y no está en la propiedad [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) de la clase [**StorageApplicationPermissions**](/uwp/api/Windows.Storage.AccessCache.StorageApplicationPermissions) de tu aplicación, se producirá un error al cargar la composición.

 

 