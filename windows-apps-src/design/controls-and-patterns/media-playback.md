---
Description: El reproductor multimedia se usa para ver y escuchar vídeo, audio e imágenes.
title: Reproductor multimedia
ms.assetid: 9AABB5DE-1D81-4791-AB47-7F058F64C491
dev.assetid: AF2F2008-9B53-430C-BBC3-8888F631B0B0
label: Media player
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: afda713e4a3d0ae30a4ee1ad9e9308d835062108
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640030"
---
# <a name="media-player"></a>Reproductor multimedia



El reproductor multimedia se usa para ver y escuchar vídeo y audio. La reproducción de contenido multimedia puede efectuarse incorporada (incrustada en una página o con un grupo de otros controles) o en una vista de pantalla completa específica. Puedes modificar el conjunto de botones del reproductor, cambiar el fondo de la barra de control y organizar los diseños como consideres más oportuno. Ten en cuenta que los usuarios esperan un conjunto de controles básicos (Reproducir/pausar, Saltar atrás, Saltar adelante).

![Elemento del reproductor multimedia con controles de transporte](images/controls/mtc_double_video_inprod.png)

> **API importantes**: [Clase MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx), [MediaTransportControls clase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediatransportcontrols)


> [!NOTE]
> **MediaPlayerElement** solo está disponible en Windows 10, versión 1607 y posteriores. Si vas a desarrollar una aplicación para una versión anterior de Windows 10, tendrás que usar [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) en su lugar. Todas las recomendaciones que se ofrecen en esta página también se aplican a MediaElement.

## <a name="is-this-the-right-control"></a>¿Es este el control adecuado?

Usa un reproductor multimedia cuando quieras reproducir audio o vídeo en tu aplicación. Para mostrar una colección de imágenes, usa una [Vista invertida](flipview.md).

## <a name="examples"></a>Ejemplos

<table>
<th align="left">Galería de controles XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si tienes instalada la aplicación <strong style="font-weight: semi-bold">Galería de controles de XAML</strong>, haz clic aquí para abrir la aplicación y ver <a href="xamlcontrolsgallery:/item/MediaPlayerElement">MediaPlayerElement</a> o <a href="xamlcontrolsgallery:/item/MediaPlayer">MediaPlayer</a> en acción.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtener la aplicación XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtener el código fuente (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Un reproductor multimedia en la aplicación Introducción de Windows 10.

![Un elemento multimedia en la aplicación Introducción de Windows 10](images/control-examples/mtc_getstarted_example.png)

## <a name="create-a-media-player"></a>Crear un reproductor multimedia
Agrega elementos multimedia a tu aplicación al crear un objeto [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) en XAML y establece el valor de [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) en una clase [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) que apunte a un archivo de audio o vídeo.

Este XAML crea un objeto [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) y establece su propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) como el URI de un archivo de vídeo que es local para la aplicación. El objeto **MediaPlayerElement** comienza a reproducirse cuando se carga la página. Para evitar que el contenido multimedia comience a reproducirse inmediatamente, puedes establecer la propiedad [AutoPlay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) en **false**.

```xaml
<MediaPlayerElement x:Name="mediaSimple"
                    Source="Videos/video1.mp4"
                    Width="400" AutoPlay="True"/>
```

Este XAML crea un objeto [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) con los controles de transporte integrados habilitados y la propiedad [AutoPlay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.autoplay.aspx) establecida en **false**.


```xaml
<MediaPlayerElement x:Name="mediaPlayer"
                    Source="Videos/video1.mp4"
                    Width="400"
                    AutoPlay="False"
                    AreTransportControlsEnabled="True"/>
```

### <a name="media-transport-controls"></a>Controles de transporte de contenido multimedia
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) tiene controles de transporte integrados para reproducir, detener, pausar, cambiar el volumen, silenciar, realizar búsquedas/comprobar el progreso, subtítulos y elegir una pista de audio. Para habilitar estos controles, establece [AreTransportControlsEnabled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.AreTransportControlsEnabled.aspx) en **true**. Para deshabilitarlos, establece **AreTransportControlsEnabled** en **false**. Los controles de transporte se representan mediante la clase [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962). Puedes usar los controles de transporte tal como están o personalizarlos de diversas maneras. Para obtener más información, consulta la referencia de la clase [MediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn831962) y el artículo [Crear controles de transporte personalizados](custom-transport-controls.md).

Los controles de transporte admiten diseños de fila única y doble. El primer ejemplo mostrado aquí es un diseño de fila única, con el botón de reproducir/pausar situado a la izquierda de la escala de tiempo multimedia. Este diseño se reserva para pantallas de reproducción de contenido multimedia compactas e incorporadas.

![Ejemplo de controles MTC, fila única](images/controls/mtc_single_inprod_02.png)

Se recomienda el diseño de controles de fila doble (a continuación) en la mayoría de los escenarios de uso, especialmente en las pantallas grandes. Este diseño proporciona más espacio para los controles y facilita el uso de la escala de tiempo para el usuario.

![Ejemplo de controles MTC en el teléfono, fila doble](images/controls/mtc_double_inprod.png)

**Controles de transporte de medios del sistema**

[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) se integra automáticamente con los controles de transporte de contenido multimedia del sistema. Los controles de transporte de contenido multimedia del sistema son los controles que aparecen cuando se presionan teclas multimedia de hardware, como los botones multimedia de los teclados. Para obtener más información, consulta [SystemMediaTransportControls](https://msdn.microsoft.com/library/windows/apps/dn278677).

> **Tenga en cuenta** &nbsp; &nbsp; [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926) no se integra automáticamente con el sistema controla el transporte de medios por lo que debe conectarlas usted mismo. Para obtener más información, consulta [Controles de transporte de contenido multimedia del sistema](https://msdn.microsoft.com/library/windows/apps/mt228338).


### <a name="set-the-media-source"></a>Establecer el origen del contenido multimedia
Para reproducir archivos de la red o archivos insertados en la aplicación, establece la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) en [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) con la ruta de acceso del archivo.

**Sugerencia**  para abrir archivos desde internet, debe declarar la **Internet (cliente)** capacidad en el manifiesto de la aplicación (Package.appxmanifest). Para obtener más información sobre las funciones de declaración, consulta [Declaraciones de funcionalidades de las aplicaciones](https://msdn.microsoft.com/library/windows/apps/mt270968).

 

En este código se intenta establecer la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de la clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) definida en XAML en la ruta de acceso de un archivo especificado en una clase [TextBox](https://msdn.microsoft.com/library/windows/apps/br209683).

```xaml
<TextBox x:Name="txtFilePath" Width="400"
         FontSize="20"
         KeyUp="TxtFilePath_KeyUp"
         Header="File path"
         PlaceholderText="Enter file path"/>
```

```csharp
private void TxtFilePath_KeyUp(object sender, KeyRoutedEventArgs e)
{
    if (e.Key == Windows.System.VirtualKey.Enter)
    {
        TextBox tbPath = sender as TextBox;

        if (tbPath != null)
        {
            LoadMediaFromString(tbPath.Text);
        }
    }
}

private void LoadMediaFromString(string path)
{
    try
    {
        Uri pathUri = new Uri(path);
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

Para establecer el origen del contenido multimedia en un archivo multimedia insertado en la aplicación, inicializa un constructor [Uri](https://msdn.microsoft.com/library/windows/apps/br226017) con la ruta de acceso con el prefijo **ms-appx:///**, crea una clase [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) con el Uri y luego establece la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) en dicho Uri. Por ejemplo, para un archivo llamado **video1.mp4** que se encuentra en una subcarpeta **Vídeos**, la ruta de acceso tendría este aspecto: **ms-appx:///Videos/video1.mp4**

Este código establece la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de la clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) previamente definida en XAML como **ms-appx:///Videos/video1.mp4**.

```csharp
private void LoadEmbeddedAppFile()
{
    try
    {
        Uri pathUri = new Uri("ms-appx:///Videos/video1.mp4");
        mediaPlayer.Source = MediaSource.CreateFromUri(pathUri);
    }
    catch (Exception ex)
    {
        if (ex is FormatException)
        {
            // handle exception.
            // For example: Log error or notify user problem with file
        }
    }
}
```

### <a name="open-local-media-files"></a>Abrir archivos multimedia locales
Para abrir archivos del sistema local o de OneDrive, puedes usar [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para obtener el archivo y [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) para establecer el origen del contenido multimedia, o bien puedes acceder a las carpetas de contenido multimedia del usuario mediante programación.

Si la aplicación necesita acceso a las carpetas **Música** o **Vídeo** sin interacción con el usuario, por ejemplo al enumerar todos los archivos de música o vídeo en la colección del usuario y mostrarlos en la aplicación, entonces debes declarar las funcionalidades **Biblioteca de música** y **Biblioteca de vídeos** . Para obtener más información, consulta [Archivos y carpetas en las bibliotecas de música, imágenes y vídeos](https://msdn.microsoft.com/library/windows/apps/mt188703).

El control [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) no requiere funcionalidades especiales para acceder a archivos en el sistema de archivos local, por ejemplo en las carpetas **Música** o **Vídeo** del usuario, porque el usuario tiene control total sobre el archivo al cual se accede. Desde una perspectiva de seguridad y privacidad, es mejor minimizar la cantidad de funcionalidades que usa la aplicación.

**Para abrir medios locales mediante FileOpenPicker**

1.  Llama a [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para que el usuario pueda seleccionar un archivo multimedia.

    Usa la clase [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para seleccionar un archivo multimedia. Establece el [FileTypeFilter](https://msdn.microsoft.com/library/windows/apps/br207850) para especificar qué tipos de archivos muestra el **FileOpenPicker**. Llama a [PickSingleFileAsync](https://msdn.microsoft.com/library/windows/apps/jj635275) para iniciar el selector de archivos y obtener el archivo.

2.  Usa una clase [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) para establecer el archivo multimedia elegido como la propiedad [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx).

    Para usar la clase [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171) devuelta desde la clase [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847), tienes que llamar al método [CreateFromStorageFile](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.createfromstoragefile.aspx) en [MediaSource](https://msdn.microsoft.com/library/windows/apps/windows.media.core.mediasource.aspx) y establecerlo como la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx). Después, llama a [Play](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.play.aspx) en [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) para iniciar el archivo multimedia.


En este ejemplo se muestra cómo usar la clase [FileOpenPicker](https://msdn.microsoft.com/library/windows/apps/br207847) para elegir un archivo y establecerlo como la propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) de una clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx).

```xaml
<MediaPlayerElement x:Name="mediaPlayer"/>
...
<Button Content="Choose file" Click="Button_Click"/>
```

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    await SetLocalMedia();
}

async private System.Threading.Tasks.Task SetLocalMedia()
{
    var openPicker = new Windows.Storage.Pickers.FileOpenPicker();

    openPicker.FileTypeFilter.Add(".wmv");
    openPicker.FileTypeFilter.Add(".mp4");
    openPicker.FileTypeFilter.Add(".wma");
    openPicker.FileTypeFilter.Add(".mp3");

    var file = await openPicker.PickSingleFileAsync();

    // mediaPlayer is a MediaPlayerElement defined in XAML
    if (file != null)
    {
        mediaPlayer.Source = MediaSource.CreateFromStorageFile(file);

        mediaPlayer.MediaPlayer.Play();
    }
}
```

### <a name="set-the-poster-source"></a>Establecer el origen de póster
Puedes usar la propiedad [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) para proporcionar a la clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) una representación visual antes de cargar el contenido multimedia. Un **PosterSource** es una imagen, como una captura de pantalla o el póster de una película que se muestra en lugar del contenido multimedia. El **PosterSource** se muestra en las siguientes situaciones:

-   Cuando no hay un origen válido establecido. Por ejemplo, [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) no está establecido, **Source** se estableció en **Null** o el origen no es válido (como en el caso en el que se desencadena un evento [MediaFailed](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediafailed.aspx)).
-   Mientras se carga el contenido multimedia. Por ejemplo, se estableció un origen válido, pero aún no se desencadenó el evento [MediaOpened](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.mediaopened.aspx).
-   Cuando se hace streaming del contenido multimedia a otro dispositivo.
-   Cuando el contenido multimedia es solo audio.

Esta es una clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) con su propiedad [Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) establecida en una pista de álbum y su propiedad [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.PosterSource.aspx) establecida en una imagen de portada de álbum.

```xaml
<MediaPlayerElement Source="Media/Track1.mp4" PosterSource="Media/AlbumCover.png"/>
```

### <a name="keep-the-devices-screen-active"></a>Mantener activa la pantalla del dispositivo
Por lo general, los dispositivos oscurecen la pantalla (y terminan apagándola) para ahorrar batería cuando el usuario se ausenta, pero las aplicaciones de vídeo necesitan mantenerla activada para que el usuario pueda ver el vídeo. Para evitar que se desactive la pantalla cuando no se detecte ninguna acción del usuario (por ejemplo, cuando una aplicación reproduce un vídeo), puedes llamar al método [DisplayRequest.RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818). La clase [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) te permite indicarle a Windows que mantenga activada la pantalla para que el usuario pueda ver el vídeo.

Para ahorrar energía y duración de la batería, debes llamar a [DisplayRequest.RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) para liberar la solicitud de pantalla cuando ya no sea necesaria. Windows desactiva automáticamente la pantalla activa de la aplicación cuando la aplicación se quita de la pantalla y vuelve a activarla cuando la aplicación vuelve a estar en primer plano.

Estas son algunas situaciones en las que debes liberar la solicitud de pantalla:

-   La reproducción de vídeo se pone en pausa, por ejemplo, por una acción del usuario, por almacenamiento en búfer o por un ajuste a causa de ancho de banda limitado.
-   La reproducción se detiene. Por ejemplo, se terminó de reproducir el vídeo o finalizó la presentación.
-   Error de reproducción. Por ejemplo, problemas de conectividad de red o un archivo dañado.

> **Nota**&nbsp;&nbsp; Si [MediaPlayerElement.IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.IsFullWindow.aspx) se establece en true y el contenido multimedia está en reproducción, automáticamente se impedirá que la pantalla se desactive.

**Para mantener activa la pantalla**

1.  Crea una variable [DisplayRequest](https://msdn.microsoft.com/library/windows/apps/br241816) global. Inicialízala como nula.
```csharp
// Create this variable at a global scope. Set it to null.
private DisplayRequest appDisplayRequest = null;
```

2.  Llama a [RequestActive](https://msdn.microsoft.com/library/windows/apps/br241818) para notificar a Windows que la aplicación requiere que la pantalla permanezca activada.

3.  Llama a [RequestRelease](https://msdn.microsoft.com/library/windows/apps/br241819) para liberar la solicitud de pantalla siempre que la reproducción de vídeo se detenga, ponga en pausa o se interrumpa por un error de reproducción. Cuando la aplicación ya no tiene ninguna solicitud de pantalla activa, Windows ahorra batería oscureciendo la pantalla (y terminará apagándola) cuando el dispositivo no esté en uso.

    Cada propiedad [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) tiene una propiedad [PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) de tipo [MediaPlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.aspx) que controla diversos aspectos de la reproducción de contenido multimedia como [PlaybackRate](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx), [PlaybackState](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstate.aspx) y [Position](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx). En este ejemplo se usa el evento [PlaybackStateChanged](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackstatechanged.aspx) en [MediaPlayer.PlaybackSession](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.playbacksession.aspx) para detectar situaciones en las que debes liberar la solicitud de pantalla. Por lo tanto, usa la propiedad [NaturalVideoHeight](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.naturalvideoheight.aspx) para determinar si hay un archivo de audio o vídeo en reproducción y mantener la pantalla activa solo si el vídeo se está reproduciendo.
    ```xaml
<MediaPlayerElement x:Name="mpe" Source="Media/video1.mp4"/>
    ```

    ```csharp
    protected override void OnNavigatedTo(NavigationEventArgs e)
    {
        mpe.MediaPlayer.PlaybackSession.PlaybackStateChanged += MediaPlayerElement_CurrentStateChanged;
        base.OnNavigatedTo(e);
    }

    private void MediaPlayerElement_CurrentStateChanged(object sender, RoutedEventArgs e)
    {
        MediaPlaybackSession playbackSession = sender as MediaPlaybackSession;
        if (playbackSession != null && playbackSession.NaturalVideoHeight != 0)
        {
            if(playbackSession.PlaybackState == MediaPlaybackState.Playing)
            {
                if(appDisplayRequest == null)
                {
                    // This call creates an instance of the DisplayRequest object
                    appDisplayRequest = new DisplayRequest();
                    appDisplayRequest.RequestActive();
                }
            }
            else // PlaybackState is Buffering, None, Opening or Paused
            {
                if(appDisplayRequest != null)
                {
                      // Deactivate the displayr request and set the var to null
                      appDisplayRequest.RequestRelease();
                      appDisplayRequest = null;
                }
            }
        }

    }
    ```

### <a name="control-the-media-player-programmatically"></a>Controlar el reproductor multimedia mediante programación
[MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) proporciona varias propiedades, métodos y eventos para controlar la reproducción de audio y vídeo a través de la propiedad [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx). Para obtener una lista completa de propiedades, métodos y eventos, consulta la página de referencia de [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx).

### <a name="advanced-media-playback-scenarios"></a>Escenarios de reproducción avanzada de contenido multimedia
Para los escenarios más complejos de reproducción de contenido multimedia, como reproducir una lista de reproducción, cambiar entre idiomas de audio o crear pistas de metadatos personalizadas, establece la propiedad [MediaPlayerElement.Source](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) en una clase [MediaPlaybackItem](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybackitem.aspx) o [MediaPlaybackList](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacklist.aspx). Consulte la [reproducción multimedia](https://msdn.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource) para obtener más información sobre cómo habilitar diversas funcionalidades avanzadas para medios.

### <a name="enable-full-window-video-rendering"></a>Habilitar la representación de vídeo a pantalla completa

Establece la propiedad [IsFullWindow](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) para habilitar y deshabilitar la representación a pantalla completa. Al establecer la representación a pantalla completa mediante programación en la aplicación, debes usar siempre **IsFullWindow** en lugar de hacerlo manualmente. **IsFullWindow** garantiza que se llevarán a cabo las optimizaciones en el nivel del sistema que mejoran el rendimiento y la duración de la batería. Si la representación a pantalla completa no se configura correctamente, es posible que estas optimizaciones no se habiliten.

Con el siguiente código se crea un [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244) que alterna la representación a pantalla completa.

```xaml
<AppBarButton Icon="FullScreen"
              Label="Full Window"
              Click="FullWindow_Click"/>>
```

```csharp
private void FullWindow_Click(object sender, object e)
{
    mediaPlayer.IsFullWindow = !media.IsFullWindow;
}
```

### <a name="resize-and-stretch-video"></a>Cambiar el tamaño del vídeo y ampliarlo

Usa la propiedad [Stretch](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.stretch.aspx) para cambiar la forma en que el contenido de vídeo o [PosterSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) rellenan el contenedor en el que se encuentran. Esto amplía el vídeo y cambia su tamaño según el valor de [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968). Los estados de **Stretch** son similares a la configuración de tamaño de imagen en muchos televisores. Puedes enlazarlo con un botón y dejar que el usuario elija qué configuración prefiere.

-   [None](https://msdn.microsoft.com/library/windows/apps/br242968) muestra la resolución nativa del contenido en su tamaño original.
-   [Uniform](https://msdn.microsoft.com/library/windows/apps/br242968) rellena la mayor cantidad de espacio posible, a la vez que conserva la relación de aspecto y el contenido de la imagen. Esto puede hacer que aparezcan barras negras horizontales o verticales en los bordes del vídeo. Esto es similar a los modos de pantalla panorámica.
-   [UniformToFill](https://msdn.microsoft.com/library/windows/apps/br242968) rellena todo el espacio, pero conserva la relación de aspecto. Esto puede hacer que se recorte parte de la imagen. Esto es similar a los modos de pantalla completa.
-   [Fill](https://msdn.microsoft.com/library/windows/apps/br242968) rellena todo el espacio, pero no conserva la relación de aspecto. La imagen no se recorta, pero se podría estirar. Esto es similar a los modos con ajuste.

![Ampliar los valores de las enumeraciones](images/Image_Stretch.jpg)

Aquí, un [AppBarButton](https://msdn.microsoft.com/library/windows/apps/dn279244) se usa para recorrer las opciones [Stretch](https://msdn.microsoft.com/library/windows/apps/br242968). Una instrucción **switch** comprueba el estado actual de la propiedad [Stretch](https://msdn.microsoft.com/library/windows/apps/br227422) y la establece en el siguiente valor en la enumeración **Stretch**. Esto permite que el usuario recorra los distintos estados de imagen ampliada.

```xaml
<AppBarButton Icon="Switch"
              Label="Resize Video"
              Click="PictureSize_Click" />
```

```csharp
private void PictureSize_Click(object sender, RoutedEventArgs e)
{
    switch (mediaPlayer.Stretch)
    {
        case Stretch.Fill:
            mediaPlayer.Stretch = Stretch.None;
            break;
        case Stretch.None:
            mediaPlayer.Stretch = Stretch.Uniform;
            break;
        case Stretch.Uniform:
            mediaPlayer.Stretch = Stretch.UniformToFill;
            break;
        case Stretch.UniformToFill:
            mediaPlayer.Stretch = Stretch.Fill;
            break;
        default:
            break;
    }
}
```

### <a name="enable-low-latency-playback"></a>Habilitar la reproducción de latencia baja

Establece la propiedad [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) en **true** en una propiedad [MediaPlayerElement.MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) para permitir que el elemento de reproductor multimedia reduzca la latencia inicial de la reproducción. Esto es fundamental para las aplicaciones de comunicaciones bidireccionales y se puede aplicar a algunos escenarios de juegos. Ten en cuenta que este modo consume más recursos y es menos eficiente desde el punto de vista energético.

En este ejemplo se crea una clase [MediaPlayerElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) y se establece [RealTimePlayback](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.realtimeplayback.aspx) en **true**.


```csharp
MediaPlayerElement mp = new MediaPlayerElement();
mp.MediaPlayer.RealTimePlayback = true;
```

## <a name="recommendations"></a>Recomendaciones

El reproductor multimedia admite temas claros y oscuros, pero el tema oscuro proporciona una mejor experiencia para la mayoría de los escenarios de entretenimiento. El fondo oscuro proporciona un mejor contraste, en particular para condiciones de poca luz, y limita la interferencia de la barra de control en la experiencia de visualización.

Durante la reproducción de contenido de vídeo, fomenta una experiencia de visualización dedicada promocionando el modo de pantalla completa sobre el modo incorporado. La experiencia de visualización en pantalla completa es óptima, y las opciones están restringidas en el modo incorporado.

Si tienes espacio en la pantalla o diseñas para la experiencia de 10 pies, opta por el diseño de doble fila. Proporciona más espacio para los controles que el diseño compacto de una sola fila y resulta más fácil navegar con el controlador para juegos en la experiencia de 10 pies.

> **Nota**&nbsp;&nbsp; Consulta el artículo [Diseño para Xbox y televisión](../devices/designing-for-tv.md) para obtener más información sobre cómo optimizar la aplicación para la experiencia de 10 pies.

Los controles predeterminados se han optimizado para la reproducción de contenido multimedia; sin embargo, tienes la posibilidad de las agregar opciones personalizadas que necesites en el reproductor multimedia con el fin de proporcionar experiencia óptima para tu aplicación. Consulta el artículo [Crear controles de transporte personalizados](custom-transport-controls.md) para obtener más información sobre cómo agregar controles personalizados.

## <a name="get-the-sample-code"></a>Obtener el código de ejemplo

- [Ejemplo de Galería de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery): ve todos los controles XAML en un formato interactivo.

## <a name="related-articles"></a>Artículos relacionados

- [Conceptos básicos sobre el diseño de comandos de aplicaciones para la Plataforma universal de Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/dn958433)
- [Conceptos básicos del diseño de contenido para aplicaciones UWP](https://msdn.microsoft.com/library/windows/apps/dn958434)
