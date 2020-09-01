---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Actualmente, esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH).
title: Streaming adaptable
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0ecfe0b8e48a0810614cad6fde91d9a429956bf9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161239"
---
# <a name="adaptive-streaming"></a>Streaming adaptable


En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Esta característica admite la reproducción de contenido de transmisión por secuencias de http Live Streaming (HLS) y Dynamic streaming sobre HTTP (DASH). A partir de Windows 10, versión 1803, Smooth Streaming es compatible con  **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Para obtener una lista de las etiquetas de protocolo HLS admitidas, consulta [Compatibilidad con etiquetas HLS](hls-tag-support.md). 

Para obtener una lista de perfiles de GUIONes admitidos, consulte [compatibilidad con el perfil de Dash](dash-profile-support.md). 

> [!NOTE] 
> El código de este artículo es una adaptación de la [Adaptive streaming sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) (Muestra de streaming adaptable) para la UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptable simple con MediaPlayer y MediaPlayerElement

Para reproducir contenido multimedia adaptable en una aplicación para UWP, crea un objeto **Uri** que apunte a un archivo de manifiesto DASH o HLS. Crea una instancia de la clase [**MediaPlayer**](/uwp/api/Windows.Media.Playback.MediaPlayer). Llama a [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) para crear un nuevo objeto **MediaSource** y, a continuación, establécelo en la propiedad [**Source**](/uwp/api/windows.media.playback.mediaplayer.source) de la clase **MediaPlayer**. Llama a [**Play**](/uwp/api/windows.media.playback.mediaplayer.play) para iniciar la reproducción del contenido multimedia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

El ejemplo anterior reproducirá el audio del contenido multimedia, pero no representa automáticamente el contenido en la interfaz de usuario. La mayoría de las aplicaciones que reproducen contenido de vídeo querrán representar dicho contenido en una página XAML.  Para ello, agrega un control [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) a la página XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Llama a [**MediaSource.CreateFromUri**](/uwp/api/windows.media.core.mediasource.createfromuri) para crear un objeto **MediaSource** desde el identificador URI de un archivo de manifiesto DASH o HLS. A continuación, establece la propiedad [**Source**](/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) del control **MediaPlayerElement**. El control **MediaPlayerElement** creará automáticamente un nuevo objeto **MediaPlayer** para el contenido. Puedes llamar a **Play** en el objeto **MediaPlayer** para iniciar la reproducción del contenido.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> A partir de Windows 10, versión 1607, se recomienda usar la clase **MediaPlayer** para reproducir elementos multimedia. **MediaPlayerElement** es un control XAML ligero que se usa para representar el contenido de un objeto **MediaPlayer** en una página XAML. El control **MediaElement** se sigue admitiendo para la compatibilidad con versiones anteriores. Para obtener más información sobre el uso de **MediaPlayer** y **MediaPlayerElement** para reproducir contenido multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obtener información sobre cómo usar **MediaSource** y las API relacionada para trabajar con contenido multimedia, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptable con AdaptiveMediaSource

Si la aplicación requiere características de streaming adaptable más avanzadas, como proporcionar encabezados HTTP personalizados, la supervisión de las velocidades de bits actuales de descarga y reproducción o el ajuste de las proporciones que determinan cuándo cambia el sistema las velocidades de bits de la secuencia adaptable, usa el objeto **[AdaptiveMediaSource](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)**.

Las API de streaming adaptable se encuentran en el espacio de nombres [**Windows.Media.Streaming.Adaptive**](/uwp/api/Windows.Media.Streaming.Adaptive). En los ejemplos de este artículo se usan las API de los siguientes espacios de nombres.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Inicializa un AdaptiveMediaSource desde un URI.

Inicializa el objeto **AdaptiveMediaSource** con el URI de un archivo de manifiesto de streaming adaptable llamando al método [**CreateFromUriAsync**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync). El valor de [**AdaptiveMediaSourceCreationStatus**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) devuelto desde este método te permite saber si el origen multimedia se creó correctamente. Si es así, puede establecer el objeto como el origen de la secuencia para su **MediaPlayer** mediante la creación de un objeto **MediaSource** llamando a  [**MediaSource. CreateFromAdaptiveMediaSource**](/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource)y, a continuación, asignarlo a la propiedad [**source**](/uwp/api/windows.media.playback.mediaplayer.Source) del reproductor de media. En este ejemplo, se consulta la propiedad [**AvailableBitrates**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) para determinar la máxima velocidad de bits admitida para esta secuencia y luego ese valor se establece como la velocidad de bits inicial. En este ejemplo también se registran Controladores para los diversos eventos **AdaptiveMediaSource** que se describen más adelante en este artículo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Inicializar un AdaptiveMediaSource con HttpClient

Si necesitas definir encabezados HTTP personalizados para obtener el archivo de manifiesto, puedes crear un objeto [**HttpClient**](/uwp/api/Windows.Web.Http.HttpClient), definir los encabezados deseados y, a continuación, pasar el objeto a la sobrecarga de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

El evento [**DownloadRequested**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) se genera cuando el sistema está a punto de recuperar un recurso del servidor. El [**AdaptiveMediaSourceDownloadRequestedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) que se pasa al controlador de eventos expone las propiedades que proporcionan información sobre el recurso solicitado, como el tipo y el identificador URI del recurso.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Modificar las propiedades de la solicitud de recursos mediante el evento DownloadRequested

Puedes usar el controlador de eventos **DownloadRequested** para modificar la solicitud de recursos mediante la actualización de las propiedades del objeto [**AdaptiveMediaSourceDownloadResult**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) proporcionado por los argumentos de evento. En el ejemplo siguiente, se modifica el URI desde el que se recuperará el recurso actualizando las propiedades [**ResourceUri**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) del objeto de resultado. También puede volver a escribir el desplazamiento de intervalo de bytes y la longitud de los segmentos multimedia o, como se muestra en el ejemplo siguiente, cambiar el URI de recurso para descargar el recurso completo y establecer el desplazamiento de intervalo de bytes y la longitud en NULL.

Puedes reemplazar el contenido del recurso solicitado estableciendo las propiedades [**Buffer**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) o [**InputStream**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) del objeto de resultado. En el ejemplo siguiente, se reemplaza el contenido del recurso de manifiesto estableciendo la propiedad **Buffer**. Ten en cuenta que, si actualizas la solicitud de recursos con los datos que se obtienen de forma asincrónica (por ejemplo, la recuperación de datos de un servidor remoto o la autenticación de usuario asincrónica), debes llamar a [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) para obtener un aplazamiento y, después, llamar a [**Complete**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) cuando la operación se complete para indicar al sistema que la operación de solicitud de descarga puede continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Usar eventos de velocidad de bits para administrar y responder a los cambios de velocidad de bits

El objeto **AdaptiveMediaSource** proporciona eventos que te permiten reaccionar cuando cambie la velocidad de bits de descarga o reproducción. En este ejemplo, la velocidad de bits actual se actualiza simplemente en la interfaz de usuario. Ten en cuenta que puedes modificar las proporciones que determinan cuándo el sistema cambia velocidades de bits de la secuencia adaptable. Para obtener más información, consulta la propiedad [**AdvancedSettings**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Control de eventos de finalización y error de descarga
El objeto **AdaptiveMediaSource** genera el evento [**DownloadFailed**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) cuando se produce un error en la descarga de un recurso solicitado. Puede usar este evento para actualizar la interfaz de usuario en respuesta al error. También puede utilizar el evento para registrar información estadística acerca de la operación de descarga y el error. 

El objeto [**AdaptiveMediaSourceDownloadFailedEventArgs**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) que se pasa al controlador de eventos contiene metadatos sobre la descarga de recursos con errores, como el tipo de recurso, el URI del recurso y la posición dentro de la secuencia en la que se produjo el error. El [**RequestId**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) obtiene un identificador único generado por el sistema para la solicitud que se puede utilizar para correlacionar la información de estado de una solicitud individual en varios eventos.

La propiedad [**Statistics**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) devuelve un objeto [**AdaptiveMediaSourceDownloadStatistics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) que proporciona información detallada sobre el número de bytes recibidos en el momento del evento y el tiempo de varios hitos en la operación de descarga. Puede registrar esta información para identificar los problemas de rendimiento en la implementación de streaming adaptable.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


El evento  [**DownloadCompleted**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) se produce cuando se completa una descarga de recursos y provdes datos similares al evento **DownloadFailed** . Una vez más, se proporciona un **RequestId** para correlacionar eventos para una única solicitud. Además, se proporciona un objeto **AdaptiveMediaSourceDownloadStatistics** para habilitar el registro de estadísticas de descarga.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Recopilación de datos de telemetría de streaming adaptable con AdaptiveMediaSourceDiagnostics
**AdaptiveMediaSource** expone una propiedad de [**diagnóstico**](/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) que devuelve un objeto [**AdaptiveMediaSourceDiagnostics**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics) . Use este objeto para registrarse para el evento [**DiagnosticAvailable**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable) . Este evento está pensado para su uso en la colección de telemetría y no debe usarse para modificar el comportamiento de la aplicación en tiempo de ejecución. Este evento de diagnóstico se genera por muchos motivos diferentes. Compruebe la propiedad [**DiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) del objeto [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) pasado en el evento para determinar la razón por la que se generó el evento. Entre los posibles motivos se incluyen errores de acceso al recurso solicitado y errores de análisis del archivo de manifiesto de streaming. Para obtener una lista de las situaciones que pueden desencadenar un evento de diagnóstico, vea [**AdaptiveMediaSourceDiagnosticType**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). Al igual que los argumentos de otros eventos de streaming adaptable, **AdaptiveMediaSourceDiagnosticAvailableEventArgs** proporciona un **RequestId** adecuado para correlacionar la información de solicitud entre distintos eventos.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Aplazar el enlace del contenido de streaming adaptable para los elementos de una lista de reproducción mediante MediaBinder
La clase [**MediaBinder**](/uwp/api/Windows.Media.Core.MediaBinder) permite diferir el enlace del contenido multimedia en un [**MediaPlaybackList**](/uwp/api/Windows.Media.Playback.MediaPlaybackList). A partir de Windows 10, versión 1703, puede proporcionar un [**AdaptiveMediaSource**](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como contenido enlazado. El proceso para el enlace diferido de un origen multimedia adaptable es en gran medida igual que enlazar otros tipos de medios, lo que se describe en [elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). 

Cree una instancia de **MediaBinder** , establezca una cadena de [**token**](/uwp/api/Windows.Media.Core.MediaBinder.Token) definida por la aplicación para identificar el contenido que se va a enlazar y regístrese para el evento de [**enlace**](/uwp/api/Windows.Media.Core.MediaBinder.Binding) . Cree un **MediaSource** desde el **enlazador** llamando a [**MediaSource. CreateFromMediaBinder**](/uwp/api/windows.media.core.mediasource.createfrommediabinder). A continuación, cree un **MediaPlaybackItem** a partir de **MediaSource** y agréguelo a la lista de reproducción.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

En el controlador de eventos de **enlace** , use la cadena de token para identificar el contenido que se va a enlazar y, a continuación, cree el origen de medios adaptables mediante una llamada a una de las sobrecargas de **[CreateFromStreamAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** o **[CreateFromUriAsync](/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)**. Dado que se trata de métodos asincrónicos, primero debe llamar al método [**MediaBindingEventArgs. GetDeferral**](/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para indicar al sistema que espere a que la operación se complete antes de continuar.  Establezca el origen de medios adaptable como el contenido enlazado llamando a **[SetAdaptiveMediaSource](/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)**. Por último, llame a [**aplazamiento. Complete**](/uwp/api/windows.foundation.deferral.Complete) después de que se complete la operación para indicar al sistema que continúe.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Si desea registrar controladores de eventos para el origen de medios adaptables enlazados, puede hacerlo en el controlador para el evento [**CurrentItemChanged**](/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) de **MediaPlaybackList**. La propiedad [**CurrentMediaPlaybackItemChangedEventArgs. newitem**](/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) contiene el nuevo **MediaPlaybackItem** que se está reproduciendo en la lista. Obtiene una instancia de **AdaptiveMediaSource** que representa el nuevo elemento mediante el acceso a la propiedad [**source**](/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) de **MediaPlaybackItem** y, a continuación, la propiedad [**AdaptiveMediaSource**](/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) del origen multimedia. Esta propiedad será NULL si el nuevo elemento de reproducción no es un **AdaptiveMediaSource**, por lo que debe comprobar si hay valores NULL antes de intentar registrar Controladores para cualquiera de los eventos del objeto.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Compatibilidad con etiquetas HLS](hls-tag-support.md) 
* [Compatibilidad con perfiles de Dash](dash-profile-support.md) 
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Reproducir elementos multimedia en segundo plano](background-audio.md) 