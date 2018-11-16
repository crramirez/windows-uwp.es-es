---
author: drewbatgit
ms.assetid: C4DB495D-1F91-40EF-A55C-5CABBF3269A2
description: Las API en el espacio de nombres Windows.Media.Editing, te permiten desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo.
title: Composiciones y edición multimedia
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f32d63bf03a469d8282262c358153140587d9033
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "6834794"
---
# <a name="media-compositions-and-editing"></a>Composiciones y edición multimedia



En este artículo te mostramos cómo usar las API en el espacio de nombres [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565), para desarrollar rápidamente aplicaciones que permitan a los usuarios crear composiciones multimedia desde archivos de origen de audio y vídeo. Las características del marco incluyen la capacidad de anexar juntos varios clips de vídeo, agregar superposiciones de vídeo e imágenes, agregar audio en segundo plano y aplicar efectos de audio y vídeos mediante programación. Una vez creado, las composiciones multimedia pueden representarse en un archivo multimedia plano para la reproducción o uso compartido, pero composiciones también se serializa en y desde el disco, lo que permite al usuario cargar y modificar composiciones que han creado anteriormente. Toda esta funcionalidad se proporciona en una interfaz de Windows Runtime de uso fácil que reduce considerablemente la cantidad y la complejidad del código necesario para realizar estas tareas en comparación con el bajo nivel [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) API.

## <a name="create-a-new-media-composition"></a>Crear una nueva composición de multimedia

La clase [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646) es el contenedor de todos los clips multimedia que conforman la composición y es responsable de representar la composición final, cargar y guardar composiciones al disco y proporcionar una secuencia de vista previa de la composición para que el usuario pueda ver en la interfaz de usuario. Para usar la clase **MediaComposition** en tu aplicación, debes incluir el espacio de nombres [**Windows.Media.Editing**](https://msdn.microsoft.com/library/windows/apps/dn640565), así como el espacio de nombres [**Windows.Media.Core**](https://msdn.microsoft.com/library/windows/apps/dn278962) que proporciona las API relacionadas que necesitas.

[!code-cs[Namespace1](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace1)]


Puedes acceder al objeto **MediaComposition** desde varios puntos en el código, así que normalmente tendrás que declarar una variable de miembro en la que almacenar el objeto.

[!code-cs[DeclareMediaComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaComposition)]

El constructor de **MediaComposition** no toma argumentos.

[!code-cs[MediaCompositionConstructor](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetMediaCompositionConstructor)]

## <a name="add-media-clips-to-a-composition"></a>Agregar clips multimedia a una composición

Las composiciones multimedia normalmente contienen uno o varios clips de vídeo. Puedes usar un [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369) para permitir al usuario seleccionar un archivo de vídeo. Una vez que se haya seleccionado el archivo, crea un objeto [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) nuevo que contenga el clip de vídeo mediante una llamada a [**MediaClip.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652607). A continuación, agrega el clip a la lista [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648) del objeto **MediaComposition**.

[!code-cs[PickFileAndAddClip](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetPickFileAndAddClip)]

-   Los clips multimedia aparecen en el objeto **MediaComposition** en el mismo orden en que aparecen en la lista [**Clips**](https://msdn.microsoft.com/library/windows/apps/dn652648).

-   Un **MediaClip** solo se puede incluir en una composición de una vez. Intentar agregar un **MediaClip** que la composición ya está usando tendrá un error como resultado. Para volver a usar un clip de vídeo varias veces en una composición, llama a [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) para crear nuevos objetos **MediaClip** que puedan agregarse a la composición.

-   Las aplicaciones universales de Windows no tienen permiso para acceder al sistema de archivo completo. La propiedad [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de la clase [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) permite que la aplicación almacene un registro de un archivo que el usuario haya seleccionado, por lo que puedes conservar los permisos de acceso al archivo. La **FutureAccessList** tiene un máximo de 1000 entradas, por lo que la aplicación necesita administrar la lista para asegurarse de que no se llene. Esto es especialmente importante si tienes previsto admitir la carga y modificar las composiciones creadas anteriormente.

-   Un **MediaComposition** admite clips de vídeo en formato MP4.

-   Si un archivo de vídeo contiene varias pistas de audio incrustadas, puedes seleccionar cuál pista de audio se debe usar en la composición estableciendo la propiedad [**SelectedEmbeddedAudioTrackIndex**](https://msdn.microsoft.com/library/windows/apps/dn652627).

-   Crea un **MediaClip** con un color único llenando todo el marco, mediante una llamada a [**CreateFromColor**](https://msdn.microsoft.com/library/windows/apps/dn652605) y especifica un color y una duración para el clip.

-   Crea un **MediaClip** desde un archivo de imagen mediante una llamada a [**CreateFromImageFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652610) y especifica un archivo de imagen y una duración para el clip.

-   Crear un **MediaClip** desde una interfaz [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/dn965505) llamando a [**CreateFromSurface**](https://msdn.microsoft.com/library/dn764774) y especificando una superficie y una duración del clip.

## <a name="preview-the-composition-in-a-mediaelement"></a>Vista previa de la composición de un MediaElement

Para permitir al usuario ver la composición de multimedia, agrega una clase [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) al archivo XAML que defina tu interfaz de usuario.

[!code-xml[MediaElement](./code/MediaEditing/cs/MainPage.xaml#SnippetMediaElement)]

Declara una variable de miembro de tipo [**MediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn282716).


[!code-cs[DeclareMediaStreamSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetDeclareMediaStreamSource)]

Llama el método [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) del objeto **MediaComposition** para crear un **MediaStreamSource** para la composición. Crea un objeto [**MediaSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Core.MediaSource) llamando al método de fábrica [**CreateFromMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn930907) y asígnalo a la propiedad [**origen**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement.Source) de **MediaPlayerElement**. La composición se puede ver en la interfaz de usuario.


[!code-cs[UpdateMediaElementSource](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetUpdateMediaElementSource)]

-   El objeto **MediaComposition** debe contener al menos un clip multimedia antes de llamar al método [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674), o el valor del objeto devuelto será "null".

-   La escala de tiempo **MediaElement** no se actualiza automáticamente para reflejar los cambios en la composición. Se recomienda que llames a **GeneratePreviewMediaStreamSource** y que definas la propiedad **MediaPlayerElement** **Source** cada vez que realices un conjunto de cambios en la composición y si deseas actualizar la interfaz de usuario.

Te recomendamos que establezcas el objeto **MediaStreamSource** y la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de la clase **MediaPlayerElement** en null cuando el usuario abandone la página para liberar los recursos asociados.

[!code-cs[OnNavigatedFrom](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

## <a name="render-the-composition-to-a-video-file"></a>Procesar la composición a un archivo de vídeo

Para representar una composición multimedia en un archivo plano de vídeo para que se pueda compartir y ver en otros dispositivos, tendrás que usar las API del espacio de nombres [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105). Para actualizar la interfaz de usuario en el progreso de la operación asincrónica, también necesitarás la API del espacio de nombres [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383).

[!code-cs[Namespace2](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetNamespace2)]

Después de permitir al usuario seleccionar un archivo de salida mediante una clase [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871), procesa la composición al archivo seleccionado mediante una llamada al método [**RenderToFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652690) del objeto **MediaComposition**. El resto del código del siguiente ejemplo simplemente sigue el patrón de control de un [**AsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/desktop/br205807).

[!code-cs[RenderCompositionToFile](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetRenderCompositionToFile)]

-   La enumeración [**MediaTrimmingPreference**](https://msdn.microsoft.com/library/windows/apps/dn640561) permite dar prioridad a la velocidad de la operación de transcodificación en función de la precisión de recorte de clips multimedia adyacentes. **Fast** hace que la transcodificación sea más rápida con el recorte de menor precisión, mientras que **Precise** hace que la transcodificación sea más lenta pero con el recorte más preciso.

## <a name="trim-a-video-clip"></a>Recortar un clip de vídeo

Recorta la duración de un clip de vídeo en una composición estableciendo los objetos [**MediaClip**](https://msdn.microsoft.com/library/windows/apps/dn652596) la propiedad [**TrimTimeFromStart**](https://msdn.microsoft.com/library/windows/apps/dn652637), la propiedad [**TrimTimeFromEnd**](https://msdn.microsoft.com/library/windows/apps/dn652634) o ambas.

[!code-cs[TrimClipBeforeCurrentPosition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetTrimClipBeforeCurrentPosition)]

-   Puedes usar cualquier interfaz de usuario que quieras para que el usuario pueda especificar el inicio y fin del recorte de valores. El ejemplo anterior usa la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession.Position) de la clase [**MediaPlaybackSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlaybackSession) asociada con **MediaPlayerElement** para determinar primero qué **MediaClip** se reproduce en la posición actual en la composición comprobando la [**StartTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652629) y [**EndTimeInComposition**](https://msdn.microsoft.com/library/windows/apps/dn652618). Entonces, las propiedades **Position** y **StartTimeInComposition** se usan de nuevo para calcular la cantidad de tiempo para recortar desde el principio el clip. El método **FirstOrDefault** es un método de extensión del espacio de nombres **System.Linq** que simplifica el código para seleccionar elementos en una lista.
-   La propiedad [**OriginalDuration**](https://msdn.microsoft.com/library/windows/apps/dn652625) del objeto **MediaClip** permite conocer la duración del clip multimedia sin ningún recorte aplicado.
-   La propiedad [**TrimmedDuration**](https://msdn.microsoft.com/library/windows/apps/dn652631) permite conocer la duración de los clip multimedia después de que se aplique el recorte.
-   Especificar un valor de recorte que sea mayor que la duración de la imagen original no produce un error. Sin embargo, si una composición contiene un único clip y tal clip se recorta a longitud cero, especificando un valor de recorte grande, una llamada posterior a [**GeneratePreviewMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn652674) devolverá un valor null, como si la composición no tuviera clips.

## <a name="add-a-background-audio-track-to-a-composition"></a>Agregar una pista de audio de fondo a una composición

Para agregar una pista de fondo a una composición, carga un archivo de audio y, a continuación, crea un objeto [**BackgroundAudioTrack**](https://msdn.microsoft.com/library/windows/apps/dn652544) llamando al método de fábrica [**BackgroundAudioTrack.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/dn652561). A continuación, agrega la clase **BackgroundAudioTrack** a la propiedad de la composición [**BackgroundAudioTracks**](https://msdn.microsoft.com/library/windows/apps/dn652647).

[!code-cs[AddBackgroundAudioTrack](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddBackgroundAudioTrack)]

-   Un objeto **MediaComposition** admite, en segundo plano, pistas de audio en los siguientes formatos: MP3, WAV, FLAC

-   Una pista de audio de fondo

-   Al igual que con archivos de vídeo, debes usar la clase [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) para preservar el acceso a archivos en la composición.

-   Al igual que con la clase **MediaClip**, **BackgroundAudioTrack** solo se puede incluir en una composición de una vez. Intentar agregar una clase **BackgroundAudioTrack** que la composición ya está usando tendrá un error como resultado. Para volver a usar una pista de audio varias veces en una composición, llama a [**Clone**](https://msdn.microsoft.com/library/windows/apps/dn652599) para crear nuevos objetos **MediaClip** que se puedan agregar a la composición.

-   De manera predeterminada, las pistas de audio de fondo inician la reproducción en el inicio de la composición. Si hay varias pistas de segundo plano, todas las pistas de empezará a reproducir en el inicio de la composición. Para hacer que una pista de audio de fondo empiece la reproducción en otro momento, establece la propiedad [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn652563) para el desplazamiento de tiempo deseado.

## <a name="add-an-overlay-to-a-composition"></a>Agregar una superposición a una composición

Las superposiciones permiten varias capas de vídeo entre sí de una composición de la pila. Una composición puede contener varias capas de superposición, cada una de ellas puede incluir varias superposiciones. Crear un objeto [**MediaOverlay**](https://msdn.microsoft.com/library/windows/apps/dn764793) pasando un **MediaClip** en su constructor. Establece la posición y la opacidad de la superposición, para después crear una nueva clase [**MediaOverlayLayer**](https://msdn.microsoft.com/library/windows/apps/dn764795) y agregar la clase **MediaOverlay** a su lista [**Overlays**](https://msdn.microsoft.com/library/windows/desktop/dn280411). Por último, agrega la clase **MediaOverlayLayer** a la lista [**OverlayLayers**](https://msdn.microsoft.com/library/windows/apps/dn764791) de la composición.

[!code-cs[AddOverlay](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddOverlay)]

-   Las superposiciones dentro de una capa están ordenadas desde la z, en función del orden en la lista de capa contenedora **Overlays**. Los índices más altos dentro de la lista se representan por encima de índices inferiores. Lo mismo pasa con las capas de superposición de una composición. Una capa con mayor índice en la lista **OverlayLayers** de la composición se representará en índices inferiores.

-   Como las superposiciones están apiladas unas sobre otras en lugar de que se reproduzcan de forma secuencial, todas las superposiciones inician la reproducción al principio de la composición de manera predeterminada. Para hacer una superposición con el fin de empezar la reproducción en otro momento, establece la propiedad [**Delay**](https://msdn.microsoft.com/library/windows/apps/dn764810) para el desplazamiento de tiempo deseado.

## <a name="add-effects-to-a-media-clip"></a>Agregar efectos a un clip multimedia

Cada **MediaClip** en una composición tiene una lista de los efectos de audio y vídeo a la que se pueden agregar varios efectos. Debes implementar los efectos [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) y [**IVideoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608047) respectivamente. En el siguiente ejemplo se usa la posición actual del **MediaPlayerElement** para elegir la vista actual de **MediaClip** y, a continuación, se crea una nueva instancia de la clase [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) y la anexa a la lista [**VideoEffectDefinitions**](https://msdn.microsoft.com/library/windows/apps/dn652643) del clip multimedia.

[!code-cs[AddVideoEffect](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetAddVideoEffect)]

## <a name="save-a-composition-to-a-file"></a>Guardar una composición en un archivo

Puedes serializar las composiciones multimedia en un archivo y modificarlo más adelante. Para ello, selecciona un archivo de salida y, a continuación, llama al método [**SaveAsync**](https://msdn.microsoft.com/library/windows/apps/dn640554) de la clase [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), para guardar la composición.

[!code-cs[SaveComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetSaveComposition)]

## <a name="load-a-composition-from-a-file"></a>Cargar una composición desde un archivo

Se pueden deserializar composiciones multimedia desde un archivo para que el usuario pueda ver y modificar la composición. Para ello, selecciona un archivo de composición y, a continuación, llama al método [**LoadAsync**](https://msdn.microsoft.com/library/windows/apps/dn652684) de la clase [**MediaComposition**](https://msdn.microsoft.com/library/windows/apps/dn652646), para cargar la composición.

[!code-cs[OpenComposition](./code/MediaEditing/cs/MainPage.xaml.cs#SnippetOpenComposition)]

-   Si un archivo multimedia de la composición no está en una ubicación a la que pueda acceder tu aplicación y no está en la propiedad [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) de la clase [**StorageApplicationPermissions**](https://msdn.microsoft.com/library/windows/apps/br207456) de tu aplicación, se producirá un error al cargar la composición.

 

 




