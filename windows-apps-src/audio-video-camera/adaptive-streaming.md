---
author: drewbatgit
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: "En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Actualmente, esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH)."
title: Streaming adaptable
translationtype: Human Translation
ms.sourcegitcommit: d0941887ebc17f3665302fae6c7b0a124dfb5a0b
ms.openlocfilehash: 431fa345c0135a08c1da68904a8d58d969490a8d

---

# Streaming adaptable

\[ Actualizado para aplicaciones para UWP en Windows10. Para leer más artículos sobre Windows8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Actualmente, esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH).

Para obtener una lista de las etiquetas de protocolo HLS admitidas, consulta [Compatibilidad con etiquetas HLS](hls-tag-support.md). 

> [!NOTE] 
> El código de este artículo es una adaptación de la [Adaptive streaming sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) (Muestra de streaming adaptable) para la UWP.

## Streaming adaptable simple con MediaPlayer y MediaPlayerElement

Para reproducir contenido multimedia adaptable en una aplicación para UWP, crea un objeto **Uri** que apunte a un archivo de manifiesto DASH o HLS. Crea una instancia de la clase [**MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer). Llama a [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para crear un nuevo objeto **MediaSource** y, a continuación, establécelo en la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Source) de la clase **MediaPlayer**. Llama a [**Play**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Playback.MediaPlayer.Play) para iniciar la reproducción del contenido multimedia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

El ejemplo anterior reproducirá el audio del contenido multimedia, pero no representa automáticamente el contenido en la interfaz de usuario. La mayoría de las aplicaciones que reproducen contenido de vídeo querrán representar dicho contenido en una página XAML.  Para ello, agrega un control [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.MediaPlayerElement) a la página XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Llama a [**MediaSource.CreateFromUri**](https://msdn.microsoft.com/library/windows/apps/dn930912) para crear un objeto **MediaSource** desde el identificador URI de un archivo de manifiesto DASH o HLS. A continuación, establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) del control **MediaPlayerElement**. El control **MediaPlayerElement** creará automáticamente un nuevo objeto **MediaPlayer** para el contenido. Puedes llamar a **Play** en el objeto **MediaPlayer** para iniciar la reproducción del contenido.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> A partir de Windows10, versión 1607, se recomienda usar la clase **MediaPlayer** para reproducir elementos multimedia. El control **MediaPlayerElement** es un control XAML ligero que se usa para representar el contenido de un objeto **MediaPlayer** en una página XAML. El control **MediaElement** se sigue admitiendo para la compatibilidad con versiones anteriores. Para obtener más información sobre el uso de **MediaPlayer** y **MediaPlayerElement** para reproducir contenido multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obtener información sobre cómo usar **MediaSource** y las API relacionada para trabajar con contenido multimedia, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## Streaming adaptable con AdaptiveMediaSource

Si la aplicación requiere características de streaming adaptable más avanzadas, como proporcionar encabezados HTTP personalizados, la supervisión de las velocidades de bits actuales de descarga y reproducción o el ajuste de las proporciones que determinan cuándo cambia el sistema las velocidades de bits de la secuencia adaptable, usa el objeto [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

Las API de streaming adaptable se encuentran en el espacio de nombres [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279).

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Inicializa el objeto **AdaptiveMediaSource** con el URI de un archivo de manifiesto de streaming adaptable llamando al método [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). El valor de [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) devuelto desde este método te permite saber si el origen multimedia se creó correctamente. De este modo, puedes establecer el objeto como el origen de la secuencia de tu objeto **MediaPlayer** llamando al método [**SetMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn652653). En este ejemplo, se consulta la propiedad [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) para determinar la máxima velocidad de bits admitida para esta secuencia y luego ese valor se establece como la velocidad de bits inicial. En este ejemplo también se registran los controladores para los eventos [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) y [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) que se describen más adelante en este artículo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Si necesitas definir encabezados HTTP personalizados para obtener el archivo de manifiesto, puedes crear un objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), definir los encabezados deseados y, a continuación, pasar el objeto a la sobrecarga de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

El evento [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) se genera cuando el sistema está a punto de recuperar un recurso del servidor. El [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) que se pasa al controlador de eventos expone las propiedades que proporcionan información sobre el recurso solicitado, como el tipo y el identificador URI del recurso.

Puedes usar el controlador de eventos **DownloadRequested** para modificar la solicitud de recursos mediante la actualización de las propiedades del objeto [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) proporcionado por los argumentos de evento. En el ejemplo siguiente, se modifica el URI desde el que se recuperará el recurso actualizando las propiedades [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) del objeto de resultado.

Puedes reemplazar el contenido del recurso solicitado estableciendo las propiedades [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) o [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) del objeto de resultado. En el ejemplo siguiente, se reemplaza el contenido del recurso de manifiesto estableciendo la propiedad **Buffer**. Ten en cuenta que, si actualizas la solicitud de recursos con los datos que se obtienen de forma asincrónica (por ejemplo, la recuperación de datos de un servidor remoto o la autenticación de usuario asincrónica), debes llamar a [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) para obtener un aplazamiento y, después, llamar a [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) cuando la operación se complete para indicar al sistema que la operación de solicitud de descarga puede continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

El objeto **AdaptiveMediaSource** proporciona eventos que te permiten reaccionar cuando cambie la velocidad de bits de descarga o reproducción. En este ejemplo, la velocidad de bits actual se actualiza simplemente en la interfaz de usuario. Ten en cuenta que puedes modificar las proporciones que determinan cuándo el sistema cambia velocidades de bits de la secuencia adaptable. Para obtener más información, consulta la propiedad [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## Temas relacionados
* [Reproducción de contenido multimedia](media-playback.md)
* [Compatibilidad con etiquetas HLS](hls-tag-support.md) 
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md) 








<!--HONumber=Aug16_HO3-->


