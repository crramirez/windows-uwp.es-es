---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Actualmente, esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH).
title: Streaming adaptable
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9ee2f8fd670da6bf843962e6b4dcac7a4b0c516d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359141"
---
# <a name="adaptive-streaming"></a>Streaming adaptable


En este artículo se describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a una aplicación para la Plataforma universal de Windows (UWP). Esta característica admite la reproducción de contenido HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH). A partir de Windows 10, versión 1803, Smooth Streaming se admite en **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** .

Para obtener una lista de las etiquetas de protocolo HLS admitidas, consulta [Compatibilidad con etiquetas HLS](hls-tag-support.md). 

Para obtener una lista de perfiles DASH compatibles, consulta [Soporte técnico de perfil DASH](dash-profile-support.md). 

> [!NOTE] 
> El código de este artículo es una adaptación de la [Adaptive streaming sample](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/AdaptiveStreaming) (Muestra de streaming adaptable) para la UWP.

## <a name="simple-adaptive-streaming-with-mediaplayer-and-mediaplayerelement"></a>Streaming adaptable simple con MediaPlayer y MediaPlayerElement

Para reproducir contenido multimedia adaptable en una aplicación para UWP, crea un objeto **Uri** que apunte a un archivo de manifiesto DASH o HLS. Crea una instancia de la clase [**MediaPlayer**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlayer). Llama a [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) para crear un nuevo objeto **MediaSource** y, a continuación, establécelo en la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.source) de la clase **MediaPlayer**. Llama a [**Play**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.play) para iniciar la reproducción del contenido multimedia.

[!code-cs[DeclareMediaPlayer](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetDeclareMediaPlayer)]

[!code-cs[ManifestSourceNoUI](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSourceNoUI)]

El ejemplo anterior reproducirá el audio del contenido multimedia, pero no representa automáticamente el contenido en la interfaz de usuario. La mayoría de las aplicaciones que reproducen contenido de vídeo querrán representar dicho contenido en una página XAML.  Para ello, agrega un control [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement) a la página XAML.

[!code-xml[MediaPlayerElementXAML](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml#SnippetMediaPlayerElementXAML)]

Llama a [**MediaSource.CreateFromUri**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfromuri) para crear un objeto **MediaSource** desde el identificador URI de un archivo de manifiesto DASH o HLS. A continuación, establece la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaelement.sourceproperty) del control **MediaPlayerElement**. El control **MediaPlayerElement** creará automáticamente un nuevo objeto **MediaPlayer** para el contenido. Puedes llamar a **Play** en el objeto **MediaPlayer** para iniciar la reproducción del contenido.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetManifestSource)]

> [!NOTE] 
> A partir de Windows 10, versión 1607, se recomienda usar la clase **MediaPlayer** para reproducir elementos multimedia. **MediaPlayerElement** es un control XAML ligero que se usa para representar el contenido de un objeto **MediaPlayer** en una página XAML. El control **MediaElement** se sigue admitiendo para la compatibilidad con versiones anteriores. Para obtener más información sobre el uso de **MediaPlayer** y **MediaPlayerElement** para reproducir contenido multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md). Para obtener información sobre cómo usar **MediaSource** y las API relacionada para trabajar con contenido multimedia, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md).

## <a name="adaptive-streaming-with-adaptivemediasource"></a>Streaming adaptable con AdaptiveMediaSource

Si la aplicación requiere características de streaming adaptable más avanzadas, como proporcionar encabezados HTTP personalizados, la supervisión de las velocidades de bits actuales de descarga y reproducción o el ajuste de las proporciones que determinan cuándo cambia el sistema las velocidades de bits de la secuencia adaptable, usa el objeto **[AdaptiveMediaSource](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource)** .

Las API de streaming adaptable se encuentran en el espacio de nombres [**Windows.Media.Streaming.Adaptive**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive). En los ejemplos de este artículo se usan las API de los siguientes espacios de nombres.

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

## <a name="initialize-an-adaptivemediasource-from-a-uri"></a>Inicializar un objeto AdaptiveMediaSource desde un URI.

Inicializa el objeto **AdaptiveMediaSource** con el URI de un archivo de manifiesto de streaming adaptable llamando al método [**CreateFromUriAsync**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync). El valor de [**AdaptiveMediaSourceCreationStatus**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceCreationStatus) devuelto desde este método te permite saber si el origen multimedia se creó correctamente. De ser así, puedes establecer el objeto como el origen de la secuencia para tu objeto **MediaPlayer** al crear un objeto **MediaSource** llamando al método  [**MediaSource.CreateFromAdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource.AdaptiveMediaSource) y luego asignándolo a la propiedad [**Source**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source) del reproductor multimedia. En este ejemplo, se consulta la propiedad [**AvailableBitrates**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.availablebitrates) para determinar la máxima velocidad de bits admitida para esta secuencia y luego ese valor se establece como la velocidad de bits inicial. En este ejemplo también registra los controladores para varios eventos **AdaptiveMediaSource** que se describen más adelante en este artículo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

## <a name="initialize-an-adaptivemediasource-using-httpclient"></a>Inicializar un objeto AdaptiveMediaSource mediante HttpClient

Si necesitas definir encabezados HTTP personalizados para obtener el archivo de manifiesto, puedes crear un objeto [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient), definir los encabezados deseados y, a continuación, pasar el objeto a la sobrecarga de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

El evento [**DownloadRequested**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.downloadrequested) se genera cuando el sistema está a punto de recuperar un recurso del servidor. El [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadRequestedEventArgs) que se pasa al controlador de eventos expone las propiedades que proporcionan información sobre el recurso solicitado, como el tipo y el identificador URI del recurso.

## <a name="modify-resource-request-properties-using-the-downloadrequested-event"></a>Modificar las propiedades de solicitud de recursos con el evento DownloadRequested

Puedes usar el controlador de eventos **DownloadRequested** para modificar la solicitud de recursos mediante la actualización de las propiedades del objeto [**AdaptiveMediaSourceDownloadResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadResult) proporcionado por los argumentos de evento. En el ejemplo siguiente, se modifica el URI desde el que se recuperará el recurso actualizando las propiedades [**ResourceUri**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.resourceuri) del objeto de resultado. También puedes reescribir el desplazamiento de intervalo de bytes y la longitud de los segmentos multimedia o, como se muestra en el siguiente ejemplo, cambiar el recurso de URI para descargar el recurso completo y establecer el desplazamiento de intervalo de bytes y la longitud en nulo.

Puedes reemplazar el contenido del recurso solicitado estableciendo las propiedades [**Buffer**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.buffer) o [**InputStream**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadresult.inputstream) del objeto de resultado. En el ejemplo siguiente, se reemplaza el contenido del recurso de manifiesto estableciendo la propiedad **Buffer**. Ten en cuenta que, si actualizas la solicitud de recursos con los datos que se obtienen de forma asincrónica (por ejemplo, la recuperación de datos de un servidor remoto o la autenticación de usuario asincrónica), debes llamar a [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequestedeventargs.getdeferral) para obtener un aplazamiento y, después, llamar a [**Complete**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadrequesteddeferral.complete) cuando la operación se complete para indicar al sistema que la operación de solicitud de descarga puede continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

## <a name="use-bitrate-events-to-manage-and-respond-to-bitrate-changes"></a>Usar eventos de velocidad de bits para administrar los cambios de velocidad de bits y responder a ellos

El objeto **AdaptiveMediaSource** proporciona eventos que te permiten reaccionar cuando cambie la velocidad de bits de descarga o reproducción. En este ejemplo, la velocidad de bits actual se actualiza simplemente en la interfaz de usuario. Ten en cuenta que puedes modificar las proporciones que determinan cuándo el sistema cambia velocidades de bits de la secuencia adaptable. Para obtener más información, consulta la propiedad [**AdvancedSettings**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.advancedsettings).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

## <a name="handle-download-completion-and-failure-events"></a>Controlar eventos de error y finalización de descarga
El objeto **AdaptiveMediaSource** genera el evento [**DownloadFailed**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadFailed) cuando se produce un error en la descarga de un recurso solicitado. Puedes usar este evento para actualizar la interfaz de usuario en respuesta al error. También puedes usar el evento para registrar información estadística sobre la operación de descarga y el error. 

El objeto [**AdaptiveMediaSourceDownloadFailedEventArgs**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs) que se pasa al controlador de eventos contiene metadatos sobre la descarga de recursos con error, tal como el tipo de recurso, el URI del recurso y la posición dentro de la emisión donde se produjo el error. El objeto [**RequestId**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.RequestId) obtiene un identificador único generado por el sistema para la solicitud que se puede usar para correlacionar la información de estado de una solicitud individual entre varios eventos.

La propiedad [**Statistics**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSourceDownloadFailedEventArgs.Statistics) propiedad devuelve un objeto [**AdaptiveMediaSourceDownloadStatistics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcedownloadstatistics) que proporciona información detallada sobre el número de bytes recibidos en el momento del evento y el tiempo de las distintas etapas de la operación de descarga. Puedes registrar esta información para identificar los problemas de rendimiento en la implementación de streaming adaptable.

[!code-cs[AMSDownloadFailed](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadFailed)]


El evento [**DownloadCompleted**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource.DownloadCompleted) se produce cuando se completa una descarga de recursos y proporciona datos similares al evento **DownloadFailed**. Una vez más, se proporciona un objeto **RequestId** para correlacionar los eventos de una sola solicitud. Además, se proporciona un objeto **AdaptiveMediaSourceDownloadStatistics** para habilitar el registro de estadísticas de descarga.

[!code-cs[AMSDownloadCompleted](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDownloadCompleted)]

## <a name="gather-adaptive-streaming-telemetry-data-with-adaptivemediasourcediagnostics"></a>Recopilar datos de telemetría de streaming adaptable con AdaptiveMediaSourceDiagnostics
El objeto **AdaptiveMediaSource** expone una propiedad [**diagnósticos**](https://docs.microsoft.com/uwp/api/Windows.Media.Streaming.Adaptive.AdaptiveMediaSource) que devuelve un objeto [**AdaptiveMediaSourceDiagnostics**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics). Usa este objeto para registrar el evento [**DiagnosticAvailable**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostics.DiagnosticAvailable). Este evento está pensado para usarse en la recopilación de telemetría y no debe usarse para modificar el comportamiento de la aplicación en tiempo de ejecución. Este evento de diagnóstico se genera por muchos motivos distintos. Comprueba la propiedad [**DiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs.DiagnosticType) del objeto [**AdaptiveMediaSourceDiagnosticAvailableEventArgs**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnosticavailableeventargs) que se pasa al evento para determinar el motivo por el que se generó el evento. Entre los motivos posibles se incluyen errores de acceso al recurso solicitado y errores de análisis del archivo de manifiesto de streaming. Para obtener una lista de las situaciones que pueden desencadenar un evento de diagnóstico, consulta [**AdaptiveMediaSourceDiagnosticType**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasourcediagnostictype). Al igual que los argumentos de otros eventos de streaming adaptables, el objeto **AdaptiveMediaSourceDiagnosticAvailableEventArgs** proporciona una propiedad **RequestId** para correlacionar la información de solicitud entre distintos eventos.

[!code-cs[AMSDiagnosticAvailable](./code/AdaptiveStreaming_RS1/cs/MainPage.xaml.cs#SnippetAMSDiagnosticAvailable)]

## <a name="defer-binding-of-adaptive-streaming-content-for-items-in-a-playback-list-by-using-mediabinder"></a>Aplazar el enlazado de contenido de streaming adaptable para los elementos de una lista de reproducción mediante MediaBinder
La clase [**MediaBinder**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder) permite aplazar el enlazado de contenido multimedia en un objeto [**MediaPlaybackList**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackList). A partir de Windows 10, versión 1703, puedes proporcionar un objeto [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource) como contenido enlazado. El proceso para el enlazado diferido de un origen multimedia adaptable es prácticamente el mismo que el enlazado de otros tipos de contenido multimedia, que se describen en [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). 

Crea una instancia **MediaBinder**, establece una cadena [**Token**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Token) definida por la aplicación para identificar el contenido que se va a enlazar y regístrate en el evento [**Binding**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaBinder.Binding). Crea un objeto **MediaSource** a partir del objeto **Binder** llamando a [**MediaSource.CreateFromMediaBinder**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediabinder). Luego, crea un objeto **MediaPlaybackItem** a partir del objeto **MediaSource** y agrégalo a la lista de reproducción.

[!code-cs[InitMediaBinder](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetInitMediaBinder)]

En el controlador de eventos **Binding**, usa la cadena de token para identificar el contenido que se va a enlazar y luego crea el origen multimedia adaptable llamando a una de las sobrecargas de **[CreateFromStreamAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromstreamasync)** o **[CreateFromUriAsync](https://docs.microsoft.com/uwp/api/windows.media.streaming.adaptive.adaptivemediasource.createfromuriasync)** . Dado que estos son métodos asincrónicos, debes llamar primero al método [**MediaBindingEventArgs.GetDeferral**](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.GetDeferral) para indicarle al sistema que debe esperar a que se complete la operación antes de continuar.  Establece el origen multimedia adaptable en el contenido enlazado llamando a **[SetAdaptiveMediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediabindingeventargs.setadaptivemediasource)** . Por último, llama a [**Deferral.Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.Complete) una vez finalizada la operación para indicar al sistema que debe continuar.

[!code-cs[BinderBindingAMS](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetBinderBindingAMS)]

Si quieres registrar controladores de eventos en el origen multimedia adaptable enlazado, puedes hacer esto en el controlador del evento [**CurrentItemChanged**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacklist.CurrentItemChanged) del objeto **MediaPlaybackList**. La propiedad [**CurrentMediaPlaybackItemChangedEventArgs.NewItem**](https://docs.microsoft.com/uwp/api/windows.media.playback.currentmediaplaybackitemchangedeventargs.NewItem) contiene el nuevo objeto **MediaPlaybackItem** en reproducción en la lista. Obtén una instancia del objeto **AdaptiveMediaSource** que representa el elemento nuevo. Para ello, accede a la propiedad [**Source**](https://docs.microsoft.com/uwp/api/Windows.Media.Playback.MediaPlaybackItem.Source) del objeto **MediaPlaybackItem** y luego a la propiedad [**AdaptiveMediaSource**](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.AdaptiveMediaSource) del origen multimedia. Esta propiedad será nula si el nuevo elemento de reproducción no es un objeto **AdaptiveMediaSource**, por lo que debes probar valores nulos antes de intentar registrar controladores para cualquiera de los eventos del objeto.

[!code-cs[AMSBindingCurrentItemChanged](./code/MediaSource_RS1/cs/MainPage.xaml.cs#SnippetAMSBindingCurrentItemChanged)]

## <a name="related-topics"></a>Temas relacionados
* [Reproducción de multimedia](media-playback.md)
* [Compatibilidad con etiquetas HLS](hls-tag-support.md) 
* [Compatibilidad con el perfil dash](dash-profile-support.md) 
* [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md)
* [Reproducir archivos multimedia en segundo plano](background-audio.md) 





