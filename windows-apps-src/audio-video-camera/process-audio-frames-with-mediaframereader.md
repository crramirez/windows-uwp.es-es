---
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: En este artículo se muestra cómo usar MediaFrameReader con MediaCapture para obtener AudioFrames que contengan datos de audio de un origen de captura.
title: Procesar tramas de audio con MediaFrameReader
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18ef0ee1efb7a69a8b305c9b95e84938fe6fde32
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363898"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Procesar tramas de audio con MediaFrameReader

En este artículo se muestra cómo usar [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) con [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) para obtener datos de audio de un origen de fotogramas multimedia. Para obtener información sobre el uso de **MediaFrameReader** para obtener datos de imagen, como, por ejemplo, una cámara de color, de infrarrojos o de profundidad, consulte [procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md). En este artículo se proporciona información general sobre el patrón de uso del lector de fotogramas y se describen algunas características adicionales de la clase **MediaFrameReader** , como el uso de **MediaFrameSourceGroup** para recuperar fotogramas de varios orígenes al mismo tiempo. 

> [!NOTE] 
> Las características que se describen en este artículo solo están disponibles a partir de Windows 10, versión 1803.

> [!NOTE] 
> Hay una muestra de aplicación universal de Windows que muestra el uso de **MediaFrameReader** para mostrar fotogramas de distintos orígenes de fotogramas, lo que incluye cámaras a color, de profundidad y de infrarrojos. Para obtener más información, consulta [Camera frames sample (Muestra de fotogramas de cámara)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

## <a name="setting-up-your-project"></a>Configurar tu proyecto
El proceso para adquirir fotogramas de audio es, en gran medida, lo mismo que adquirir otros tipos de fotogramas multimedia. Al igual que con cualquier aplicación que use **MediaCapture**, debes declarar que tu aplicación usa la funcionalidad *cámara web* antes de intentar acceder a cualquier dispositivo de cámara. Si la aplicación captura desde un dispositivo de audio, también debes declarar la funcionalidad *micrófono* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Seleccione la pestaña **Funcionalidades**.
3.  Active la casilla de la **cámara web** y el cuadro de **micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y de **Biblioteca de vídeos**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Seleccionar orígenes de fotogramas y grupos de orígenes de fotogramas

El primer paso para capturar fotogramas de audio es inicializar un [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) que represente el origen de los datos de audio, como un micrófono u otro dispositivo de captura de audio. Para ello, debe crear una nueva instancia del objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) . En este ejemplo, la única configuración de inicialización de **MediaCapture** es establecer [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) para indicar que queremos transmitir audio desde el dispositivo de captura. 

Después de llamar a [**MediaCapture.InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync), puede obtener la lista de orígenes de fotogramas multimedia accesibles con la propiedad [**FrameSources**](/uwp/api/windows.media.capture.mediacapture.framesources) . En este ejemplo se usa una consulta LINQ para seleccionar todos los orígenes de fotogramas en los que el [**MediaFrameSourceInfo**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo) que describe el origen del marco tiene un  [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) de **audio**, lo que indica que el origen multimedia genera datos de audio.

Si la consulta devuelve uno o varios orígenes de fotogramas, puede comprobar la propiedad [**CurrentFormat**](/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) para ver si el origen es compatible con el formato de audio que desee: en este ejemplo, los datos de audio float. Compruebe la [**AudioEncodingProperties**](/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) para asegurarse de que la codificación de audio que desea es compatible con el origen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitAudioFrameSource":::

## <a name="create-and-start-the-mediaframereader"></a>Creación e inicio de MediaFrameReader

Obtenga una nueva instancia de **MediaFrameReader** llamando a [**MediaCapture. CreateFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), pasando el objeto **MediaFrameSource** seleccionado en el paso anterior. De forma predeterminada, los fotogramas de audio se obtienen en modo de almacenamiento en búfer, por lo que es menos probable que se quiten los fotogramas, aunque esto se puede producir si no se procesan fotogramas de audio lo suficientemente rápido y se llena el búfer de memoria asignado del sistema.

Registra un controlador para el evento [**MediaFrameReader. FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) , que genera el sistema cuando hay disponible un nuevo marco de datos de audio. Llame a [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync) para iniciar la adquisición de fotogramas de audio. Si el lector de fotogramas no se inicia, el valor de estado devuelto por la llamada tendrá un valor distinto de [**Success**](/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateAudioFrameReader":::

En el controlador de eventos **FrameArrived** , llame a [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) en el objeto **MediaFrameReader** pasado como remitente al controlador para intentar recuperar una referencia al fotograma multimedia más reciente. Tenga en cuenta que este objeto puede ser null, por lo que siempre debe comprobar antes de usar el objeto. El tipos de fotogramas multimedia incluido en el **MediaFrameReference** devuelto desde **TryAcquireLatestFrame** depende del tipo de origen del marco o de los orígenes que haya configurado para adquirir el lector de fotogramas. Como el lector de fotogramas de este ejemplo se configuró para adquirir fotogramas de audio, obtiene el fotograma subyacente mediante la propiedad [**AudioMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe) . 

Este método auxiliar **ProcessAudioFrame** en el ejemplo siguiente muestra cómo obtener un [**AudioFrame**](/uwp/api/windows.media.audioframe) que proporciona información como la marca de tiempo del marco y si es discontinuo del objeto **AudioMediaFrame** . Para leer o procesar los datos de ejemplo de audio, tendrá que obtener el objeto [**AudioBuffer**](/uwp/api/windows.media.audiobuffer) del objeto **AudioMediaFrame** , crear un [**IMemoryBufferReference**](/uwp/api/windows.foundation.imemorybufferreference)y, a continuación, llamar al método com **IMemoryBufferByteAccess:: getBuffer** para recuperar los datos. Vea la nota que aparece debajo de la lista de código para obtener más información sobre el acceso a los búferes nativos.

El formato de los datos depende del origen del marco. En este ejemplo, al seleccionar un origen de fotogramas multimedia, se ha determinado explícitamente que el origen de fotogramas seleccionado usó un solo canal de datos flotantes. En el resto del código de ejemplo se muestra cómo determinar la duración y el recuento de muestras de los datos de audio del marco.  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetProcessAudioFrame":::

> [!NOTE] 
> Para poder realizar operaciones en los datos de audio, debe tener acceso a un búfer de memoria nativo. Para ello, debe usar la interfaz com **IMemoryBufferByteAccess** incluyendo la siguiente lista de código. Las operaciones en el búfer nativo deben realizarse en un método que use la palabra clave **Unsafe** . También debe activar la casilla para permitir el código no seguro en la pestaña **compilar** del cuadro de diálogo **propiedades del proyecto >** .

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Información adicional sobre el uso de MediaFrameReader con datos de audio

Puede recuperar el [**AudioDeviceController**](/uwp/api/Windows.Media.Devices.AudioDeviceController) asociado con el origen de fotogramas de audio mediante el acceso a la propiedad [**MediaFrameSource. Controller**](/uwp/api/windows.media.capture.frames.mediaframesource.controller) . Este objeto se puede usar para obtener o establecer las propiedades de la secuencia del dispositivo de captura o para controlar el nivel de captura. En el siguiente ejemplo se silencia el dispositivo de audio para que el lector de fotogramas siga adquiriendo fotogramas, pero todos los ejemplos tienen el valor 0.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetAudioDeviceControllerMute":::

Puede usar un objeto [**AudioFrame**](/uwp/api/windows.media.audioframe) para pasar los datos de audio capturados por un origen de fotogramas multimedia a [**AudioGraph**](/uwp/api/windows.media.audio.audiograph). Pase el marco al método [**AddFrame**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) de un [**AudioFrameInputNode**](/uwp/api/windows.media.audio.audioframeinputnode). Para obtener más información sobre el uso de gráficos de audio para capturar, procesar y mezclar señales de audio, consulte [gráficos de audio](audio-graphs.md).

## <a name="related-topics"></a>Temas relacionados

* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Muestra de fotogramas de cámara](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [Gráficos de audio](audio-graphs.md)
 
