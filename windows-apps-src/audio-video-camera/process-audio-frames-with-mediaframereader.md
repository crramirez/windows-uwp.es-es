---
author: drewbatgit
ms.assetid: D6A785C6-DF28-47E6-BDC1-7A7129EC40A0
description: En este artículo se muestra cómo usar un MediaFrameReader con MediaCapture para obtener AudioFrames que contienen datos de audio desde un origen de captura.
title: Procesar tramas de audio con MediaFrameReader
ms.author: drewbat
ms.date: 04/18/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d69e8d8cca3932045d4b43d727210f84e816f30b
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7560523"
---
# <a name="process-audio-frames-with-mediaframereader"></a>Procesar tramas de audio con MediaFrameReader

En este artículo se muestra cómo usar un [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) con [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para obtener datos de audio desde un origen de fotograma multimedia. Para obtener información sobre el uso de un **MediaFrameReader** para obtener datos de imagen, como una cámara de profundidad, infrarrojos o color, consulta [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md). En este artículo se proporciona una introducción general sobre el patrón de uso del lector de fotogramas y se describen algunas características adicionales de la clase **MediaFrameReader**, como el uso de **MediaFrameSourceGroup** para recuperar fotogramas desde varios orígenes al mismo tiempo. 

> [!NOTE] 
> Las características descritas en este artículo solo están disponibles a partir de Windows 10, versión 1803.

> [!NOTE] 
> Hay una muestra de aplicación universal de Windows que muestra el uso de **MediaFrameReader** para mostrar fotogramas de distintos orígenes de fotogramas, lo que incluye cámaras a color, de profundidad y de infrarrojos. Para obtener más información, consulta [Camera frames sample (Muestra de fotogramas de cámara)](http://go.microsoft.com/fwlink/?LinkId=823230).

## <a name="setting-up-your-project"></a>Configurar tu proyecto
El proceso para adquirir fotogramas de audio es prácticamente igual al de adquirir otros tipos de fotogramas multimedia. Al igual que con cualquier aplicación que use **MediaCapture**, debes declarar que tu aplicación usa la funcionalidad *cámara web* antes de intentar acceder a cualquier dispositivo de cámara. Si la aplicación captura desde un dispositivo de audio, también debes declarar la funcionalidad *micrófono* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y el cuadro de **Biblioteca de vídeos**.



## <a name="select-frame-sources-and-frame-source-groups"></a>Seleccionar orígenes de fotogramas y grupos de orígenes de fotogramas

El primer paso en la captura de tramas de audio es inicializar un [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) que representa el origen de los datos de audio, como un micrófono u otro dispositivo de captura de audio. Para ello, debes crear una nueva instancia del objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). Para este ejemplo, la única configuración de inicialización de **MediaCapture** es la configuración del [**StreamingCaptureMode**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode) para indicar que deseamos transmitir audio desde el dispositivo de captura. 

Después de llamar a [**MediaCapture.InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), puedes obtener la lista de orígenes de fotogramas multimedia accesibles con la propiedad [**orígenes de marcos**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.framesources). Este ejemplo usa una consulta Linq para seleccionar todos los orígenes de marco donde la [**MediaFrameSourceInfo**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo) que describe el origen de marco tiene un [**MediaStreamType**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype) de **Audio**, que indica que el origen multimedia genera datos de audio.

Si la consulta devuelve uno o más orígenes de marco, puedes comprobar la propiedad [**CurrentFormat**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.currentformat) para ver si el origen admite el formato de audio que deseas, en este ejemplo, datos de audio float. Comprueba las [**AudioEncodingProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframeformat.audioencodingproperties) para asegurarte de que la codificación del audio que deseas es compatible con el origen.

[!code-cs[InitAudioFrameSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitAudioFrameSource)]

## <a name="create-and-start-the-mediaframereader"></a>Crear e iniciar el MediaFrameReader

Obtén una nueva instancia de **MediaFrameReader** llamando a [**MediaCapture.CreateFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_), pasando el objeto **MediaFrameSource** que has seleccionado en el paso anterior. De manera predeterminada, los tramas de audio se obtienen en el modo de almacenamiento en búfer, haciendo que sea menos probable que se descarten los fotogramas, aunque esto puede producirse igualmente si no se están procesando fotogramas de audio lo suficientemente rápido y se llena el búfer de memoria asignado del sistema.

Registra un controlador para el evento [**MediaFrameReader.FrameArrived**](*https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.framearrived), que se genera por el sistema cuando hay un nuevo fotograma de datos de audio disponible. Llama a [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.startasync) para comenzar la adquisición de fotogramas de audio. Si el lector de fotogramas no se puede iniciar, el valor del estado devuelto desde la llamada tendrá un valor distinto de [**Success**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereaderstartstatus).

[!code-cs[CreateAudioFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateAudioFrameReader)]

En el controlador de eventos **FrameArrived**, llama a [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) en el objeto **MediaFrameReader** pasado como el remitente al controlador para intentar recuperar una referencia a la última versión de fotograma multimedia. Ten en cuenta que este objeto puede ser null, por lo que siempre debes comprobarlo antes de usar el objeto. Los tipos de fotograma multimedia del valor de **MediaFrameReference** devuelto de **TryAcquireLatestFrame** dependen de qué tipo de origen u orígenes de fotogramas has configurado el lector de fotogramas para que se adquieran. Dado que el lector de fotogramas de este ejemplo se configuró para adquirir fotogramas de audio, obtiene el marco subyacente mediante la propiedad [**AudioMediaFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereference.audiomediaframe). 

Este método auxiliar **ProcessAudioFrame** del ejemplo siguiente muestra cómo obtener un [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) que proporciona información como la marca de tiempo del fotograma y si es discontinuo desde el objeto **AudioMediaFrame**. Para leer o procesar los datos de muestra de audio, tendrás que obtener el objeto [**AudioBuffer**](https://docs.microsoft.com/uwp/api/windows.media.audiobuffer) desde el objeto **AudioMediaFrame**, crear una [**IMemoryBufferReference**](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference) y, a continuación, llamar al método COM **IMemoryBufferByteAccess::GetBuffer** para recuperar los datos. Consulta la nota que aparece debajo de la lista para obtener más información sobre cómo acceder a los búferes nativos.

El formato de los datos depende del origen de fotogramas. En este ejemplo, al seleccionar un origen de fotograma multimedia, nos aseguramos de manera explícita de que el origen de fotograma seleccionado usó un canal único de datos float. El resto del código de ejemplo muestra cómo determinar el número de muestras y la duración de los datos de audio en el fotograma.  

[!code-cs[ProcessAudioFrame](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetProcessAudioFrame)]

> [!NOTE] 
> Para operar en los datos de audio, debes tener acceso a un búfer de memoria nativo. Para ello, debes usar la interfaz COM **IMemoryBufferByteAccess** incluyendo la lista de códigos a continuación. Las operaciones en el búfer nativo se deben realizar en un método que use la palabra clave **unsafe**. También debes activar la casilla para permitir código no seguro en la pestaña **Compilar** del diálogo **Proyecto-> Propiedades**.

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

## <a name="additional-information-on-using-mediaframereader-with-audio-data"></a>Información adicional sobre el uso de MediaFrameReader con datos de audio

Puedes recuperar el [**AudioDeviceController**](https://docs.microsoft.com/uwp/api/Windows.Media.Devices.AudioDeviceController) asociado a la trama de audio accediendo a la propiedad [**MediaFrameSource.Controller**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesource.controller). Este objeto se puede usar para obtener o establecer las propiedades de secuencia del dispositivo de captura o para controlar el nivel de captura. El siguiente ejemplo silencia el dispositivo de audio para que el lector de fotogramas continúe adquiriendo fotogramas, pero todas las muestras tienen el valor de 0.

[!code-cs[AudioDeviceControllerMute](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetAudioDeviceControllerMute)]

Puedes usar un objeto [**AudioFrame**](https://docs.microsoft.com/uwp/api/windows.media.audioframe) para pasar datos de audio capturados por un origen de fotograma multimedia en un [**AudioGraph**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph). Pasa el fotograma en el método [**AddFrame**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.addframe) de un [**AudioFrameInputNode**](https://docs.microsoft.com/en-us/uwp/api/windows.media.audio.audioframeinputnode). Para obtener más información sobre cómo usar gráficos de audio para capturar, procesar y mezclar señales de audio, consulta [Gráficos de Audio](audio-graphs.md).

## <a name="related-topics"></a>Temas relacionados

* [Procesar fotogramas multimedia con MediaFrameReader](process-media-frames-with-mediaframereader.md)
* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Muestra de fotogramas de cámara](http://go.microsoft.com/fwlink/?LinkId=823230)
* [Gráficos de audio](audio-graphs.md)
 






