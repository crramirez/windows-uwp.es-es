---
ms.assetid: AE98C22B-A071-4206-ABBB-C0F0FB7EF33C
description: En este artículo se describe el proceso para agregar la reproducción de contenido multimedia de streaming adaptable a las aplicaciones para Plataforma universal de Windows (UWP). Esta característica admite actualmente la reproducción de contenido de HTTP Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH).
title: Streaming adaptable
---

# Streaming adaptable

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artículo describe cómo agregar la reproducción de contenido multimedia de streaming adaptable a las aplicaciones para Plataforma universal de Windows (UWP). Esta característica admite actualmente la reproducción de contenido de Http Live Streaming (HLS) y Dynamic Adaptive Streaming over HTTP (DASH).

## Streaming adaptable simple con MediaElement

Para mostrar streaming adaptable multimedia en una aplicación basada en XAML, agrega un control [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) a la página.

[!code-xml[MediaElementXAML](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml#SnippetMediaElementXAML)]

Establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br227420) del control **MediaElement** en el URI de un archivo de manifiesto de DASH o HLS.

[!code-cs[ManifestSource](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetManifestSource)]

## Streaming adaptable con AdaptiveMediaSource

Si la aplicación requiere características de streaming adaptable más avanzadas, como los encabezados HTTP personalizados, la supervisión de las velocidades de bits actuales de descarga y reproducción o el ajuste de las proporciones que determinan cuándo cambia el sistema las velocidades de bits de la secuencia adaptable, usa el objeto [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

Las API de streaming adaptable se encuentran en el espacio de nombres [**Windows.Media.Streaming.Adaptive**](https://msdn.microsoft.com/library/windows/apps/dn931279).

[!code-cs[AdaptiveStreamingUsing](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAdaptiveStreamingUsing)]

Inicializa el objeto **AdaptiveMediaSource** con el URI de un archivo de manifiesto de streaming adaptable llamando al método [**CreateFromUriAsync**](https://msdn.microsoft.com/library/windows/apps/dn931261). El valor de [**AdaptiveMediaSourceCreationStatus**](https://msdn.microsoft.com/library/windows/apps/dn946917) devuelto desde este método te permite saber si el origen multimedia se creó correctamente. De este modo, puedes establecer el objeto como el origen de la secuencia de tu control **MediaElement** llamando al método [**SetMediaStreamSource**](https://msdn.microsoft.com/library/windows/apps/dn299029). En este ejemplo, se consulta la propiedad [**AvailableBitrates**](https://msdn.microsoft.com/library/windows/apps/dn931257) para determinar la velocidad de bits máxima admitida para esta secuencia y luego ese valor se establece como la velocidad de bits inicial. En este ejemplo también se registran los controladores para los eventos [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272), [**DownloadBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931269) y [**PlaybackBitrateChanged**](https://msdn.microsoft.com/library/windows/apps/dn931278) que se describen más adelante en este artículo.

[!code-cs[InitializeAMS](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMS)]

Si necesitas establecer encabezados HTTP personalizados para obtener el archivo de manifiesto, puedes crear un objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), establecer los encabezados deseados y luego pasar el objeto a la sobrecarga de **CreateFromUriAsync**.

[!code-cs[InitializeAMSWithHttpClient](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetInitializeAMSWithHttpClient)]

El evento [**DownloadRequested**](https://msdn.microsoft.com/library/windows/apps/dn931272) se genera cuando el sistema está a punto de recuperar un recurso desde el servidor. El [**AdaptiveMediaSourceDownloadRequestedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn946935) que se pasa al controlador de eventos expone las propiedades que proporcionan información sobre el recurso solicitado, como el tipo y el identificador URI del recurso.

Puedes usar el controlador de eventos **DownloadRequested** para modificar la solicitud de recursos mediante la actualización de las propiedades del objeto [**AdaptiveMediaSourceDownloadResult**](https://msdn.microsoft.com/library/windows/apps/dn946942) proporcionado por los argumentos de evento. En el ejemplo siguiente, se modifica el URI desde el que se recuperará el recurso actualizando las propiedades [**ResourceUri**](https://msdn.microsoft.com/library/windows/apps/dn931250) del objeto de resultado.

Puedes reemplazar el contenido del recurso solicitado estableciendo las propiedades [**Buffer**](https://msdn.microsoft.com/library/windows/apps/dn946943) o [**InputStream**](https://msdn.microsoft.com/library/windows/apps/dn931249) del objeto de resultado. En el ejemplo siguiente, se reemplaza el contenido del recurso de manifiesto estableciendo la propiedad **Buffer**. Ten en cuenta que si vas a actualizar la solicitud de recursos con los datos que se obtienen de forma asincrónica (por ejemplo, la recuperación de datos de un servidor remoto o la autenticación de usuario asincrónica), debes llamar a [**AdaptiveMediaSourceDownloadRequestedEventArgs.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/dn946936) para obtener un aplazamiento y luego llamar a [**Complete**](https://msdn.microsoft.com/library/windows/apps/dn946934) cuando la operación se complete para indicar al sistema que la operación de solicitud de descarga puede continuar.

[!code-cs[AMSDownloadRequested](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSDownloadRequested)]

El objeto **AdaptiveMediaSource** proporciona eventos que te permiten reaccionar cuando cambie la velocidad de bits de descarga o reproducción. En este ejemplo, la velocidad de bits actual se actualiza simplemente en la interfaz de usuario. Ten en cuenta que puedes modificar las proporciones que determinan cuándo el sistema cambia velocidades de bits de la secuencia adaptable. Para obtener más información, consulta la propiedad [**AdvancedSettings**](https://msdn.microsoft.com/library/windows/apps/mt628697).

[!code-cs[AMSBitrateEvents](./code/AdaptiveStreaming_Win10/cs/MainPage.xaml.cs#SnippetAMSBitrateEvents)]

 

 






<!--HONumber=Mar16_HO1-->


