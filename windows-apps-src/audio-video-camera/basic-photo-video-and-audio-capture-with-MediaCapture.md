---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: Este artículo muestra la forma más sencilla de capturar fotos y vídeos mediante la clase MediaCapture.
title: Captura básica de fotos, audio y vídeo con MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d0aa227a47fc3940f3fa5c3010eed70f37067935
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8686479"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Captura básica de fotos, audio y vídeo con MediaCapture


Este artículo muestra la forma más sencilla de capturar fotos y vídeos mediante la clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). La clase **MediaCapture** expone un eficaz conjunto de API que proporcionan control de bajo nivel sobre la canalización de captura y habilitan escenarios de captura avanzados, pero este artículo está pensado para ayudar a agregar una captura multimedia básica a la aplicación de forma rápida y fácil. Para obtener más información sobre otras características que proporciona **MediaCapture**, consulta [**Cámara**](camera.md).

Si simplemente deseas capturar una foto o un vídeo y no quieres agregar otras características de captura multimedia, o si no deseas crear tu propia interfaz de usuario de cámara, es aconsejable usar la clase [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CameraCaptureUI), que te permite simplemente iniciar la aplicación de cámara integrada de Windows y recibir el archivo de foto o vídeo que se captura. Para obtener más información, consulta [**Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows**](capture-photos-and-video-with-cameracaptureui.md).

El código de este artículo es una adaptación de la muestra del [**Camera starter kit**](https://go.microsoft.com/fwlink/?linkid=619479) (Starter Kit de la cámara). Puedes descargar la muestra para ver el código usado en contexto o para emplearla como punto de partida para tu propia aplicación.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades *webcam* y *microphone* del dispositivo. Si quieres guardar las fotos y los vídeos capturados en la biblioteca de imágenes de los usuarios, también debes declarar las funcionalidades *picturesLibrary* y *videosLibrary*.

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y el cuadro de **Biblioteca de vídeos**.


## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
Todos los métodos de captura que se describe en este artículo requieren el primer paso de inicializar el objeto [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) mediante una llamada al constructor y, a continuación, llamar a [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.InitializeAsync). Dado que se accederá al objeto **MediaCapture** desde varias ubicaciones de la aplicación, declara una variable de clase para que contenga el objeto.  Implementa un controlador para el evento [**Failed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.Failed) del objeto **MediaCapture** para recibir una notificación si se produce un error en una operación de captura.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Configurar la vista previa de la cámara
Es posible capturar fotos, vídeos y audio usando **MediaCapture** sin mostrar la vista previa de la cámara, pero lo normal es que desees mostrar la vista previa de la secuencia para que el usuario pueda ver lo que se captura. Además, algunas características de **MediaCapture** requieren que se ejecute la vista previa de la secuencia para poder habilitarlas, incluidos el enfoque automático, la exposición automática y el balance de blancos automático. Para ver cómo configurar la vista previa de la cámara, consulta [**Mostrar la vista previa de la cámara**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturar una foto en un objeto SoftwareBitmap
La clase [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap) se introdujo en Windows10 para proporcionar una representación común de las imágenes en distintas características. Si quieres capturar una foto y usar inmediatamente la imagen capturada en la aplicación, como mostrarla en XAML, en lugar de capturarla en un archivo, deberías capturarla en un objeto **SoftwareBitmap**. Seguirás teniendo la opción de guardar dicha imagen en el disco más adelante.

Tras inicializar el objeto **MediaCapture**, puedes capturar una foto en un objeto **SoftwareBitmap** mediante la clase [**LowLagPhotoCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture). Para obtener una instancia de esta clase, llama a [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync), pasando un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique el formato de imagen que desees. [**CreateUncompressed**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateUncompressed) crea una codificación sin comprimir con el formato de píxeles especificado. Para capturar una foto, llama a [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync), lo que devolverá un objeto [**CapturedPhoto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto). Para obtener un objeto **SoftwareBitmap**, accede a la propiedad [**Frame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedPhoto.Frame) y luego a la propiedad [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.CapturedFrame.SoftwareBitmap).

Si lo deseas, puedes capturar varias fotos si llamas repetidamente a **CaptureAsync**. Cuando hayas terminado de capturar, llama a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.AdvancedPhotoCapture.FinishAsync) para cerrar la sesión de **LowLagPhotoCapture** y liberar los recursos asociados. Tras llamar a **FinishAsyncpara**, si deseas volver a empezar a capturar fotos, deberás llamar a [**PrepareLowLagPhotoCaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagPhotoCaptureAsync) otra vez para reinicializar la sesión de captura antes de llamar a [**CaptureAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoCapture.CaptureAsync).

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

A partir de Windows, versión 1803, puedes acceder a la propiedad [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de la clase **CapturedFrame** devuelta de **CaptureAsync** para recuperar metadatos acerca de la fotografía capturada. Puedes pasar estos datos a un **BitmapEncoder** para guardar los metadatos en un archivo. Anteriormente, no había una manera de obtener acceso a estos datos para formatos de imagen sin comprimir. También puedes obtener acceso a la propiedad [**ControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.controlvalues) para recuperar un objeto [**CapturedFrameControlValues**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframecontrolvalues) que describe el control de valores, como la exposición y el balance de blancos, del marco capturado.

Para obtener información acerca de cómo usar **BitmapEncoder** y acerca de cómo trabajar con el objeto **SoftwareBitmap**, incluyendo cómo mostrar uno en una página XAML, consulta [**Crear, editar y guardar imágenes de mapa de bits**](imaging.md). 

Para obtener más información sobre los valores de control de dispositivo de captura de configuración, consulta [Capturar los controles del dispositivo para captura de fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md).

A partir de Windows 10, versión 1803, puedes obtener los metadatos, como la información de EXIF, para las fotos que se capturan en formato no comprimido mediante el acceso a la propiedad [**BitmapProperties**](https://docs.microsoft.com/uwp/api/windows.media.capture.capturedframe.bitmapproperties) del valor de **CapturedFrame** devuelto por **MediaCapture**. En versiones anteriores, estos datos solo eran accesibles en el encabezado de fotos capturados para un formato de archivo comprimido. Puedes proporcionar estos datos a un valor de [**BitmapEncoder**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder) al escribir manualmente un archivo de imagen. Para obtener más información sobre la codificación de mapas de bits, consulta [Crear, editar y guardar imágenes de mapa de bits](imaging.md).  También puedes obtener acceso a los valores de control de fotograma, como la configuración del flash y la configuración, que se usa cuando se capturó la imagen mediante el acceso a la propiedad [**ControlValues**](https://docs.microsoft.com/en-us/uwp/api/windows.media.capture.capturedframe.controlvalues). Para más información, consulta [Capturar los controles del dispositivo para captura de fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Capturar una foto a un archivo
Una aplicación de fotografía típica guardará una foto en el disco o en el almacenamiento en la nube y tendrá que agregar metadatos al archivo, como la orientación de la fotografía. El siguiente ejemplo muestra cómo capturar una foto en un archivo. Seguirás teniendo la opción de crear un objeto **SoftwareBitmap** a partir del archivo de imagen más adelante. 

La técnica que se muestra en este ejemplo captura la foto en una secuencia de memoria y, a continuación, transcodifica la foto desde dicha secuencia a un archivo del disco. Este ejemplo usa [**GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.GetLibraryAsync) para obtener la biblioteca de imágenes del usuario y luego la propiedad [**SaveFolder**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageLibrary.SaveFolder) para conseguir una carpeta para guardar predeterminada de referencia. Recuerda que tienes que agregar la funcionalidad **Pictures Library** al manifiesto de la aplicación para acceder a esta carpeta. [**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFolder.CreateFileAsync) crea un nuevo objeto [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile) en el que se guardará la foto capturada.

Crea un objeto [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.Streams.InMemoryRandomAccessStream) y, a continuación, llama a [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CapturePhotoToStreamAsync) para capturar una foto en la secuencia, pasando la secuencia y un objeto [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique el formato de imagen que debe usarse. Puedes crear propiedades de codificación personalizadas si inicializas el objeto por ti mismo, pero la clase proporciona métodos estáticos, como [**ImageEncodingProperties.CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.ImageEncodingProperties.CreateJpeg), para formatos de codificación comunes. A continuación, crea una secuencia de archivos en el archivo de salida llamando a [**OpenAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Storage.StorageFile.OpenAsync). Crea un objeto [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapDecoder) para descodificar la imagen de la secuencia de memoria y, a continuación, crea un objeto [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder) para codificar la imagen en el archivo mediante una llamada a [**CreateForTranscodingAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.CreateForTranscodingAsync).

También tienes la opción de crear un objeto [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapPropertySet) y luego llamar a [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252.aspx) en el codificador de imágenes para incluir metadatos sobre la foto en el archivo de imagen. Para obtener más información sobre las propiedades de codificación, consulta [**Metadatos de imagen**](image-metadata.md). Administrar la orientación del dispositivo correctamente es esencial para la mayoría de las aplicaciones de fotografía. Para obtener más información, consulta [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por último, llama a [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.BitmapEncoder.FlushAsync) en el objeto del codificador para transcodificar la foto desde la secuencia de memoria al archivo.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Para obtener más información sobre cómo trabajar con archivos y carpetas, consulta [**Archivos, carpetas y bibliotecas**](https://msdn.microsoft.com/windows/uwp/files/index).

## <a name="capture-a-video"></a>Capturar un vídeo
Agrega rápidamente la captura de vídeo a tu aplicación mediante la clase [**LowLagMediaRecording**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording). En primer lugar, declara una variable de clase para el objeto.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

A continuación, crea un objeto **StorageFile** en el que se guardará el vídeo. Ten en cuenta que para guardar en la biblioteca de vídeos del usuario, como se muestra en este ejemplo, debes agregar la funcionalidad **Videos Library** al manifiesto de la aplicación. Llama a [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) para inicializar la grabación multimedia, pasando el archivo de almacenamiento y un objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile) que especifique la codificación del vídeo. La clase proporciona métodos estáticos, como [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp4), para crear perfiles de las codificaciones de vídeo más comunes.

Por último, llama a [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync) para empezar a capturar vídeo.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Para detener la grabación de vídeo, llama a [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Puedes seguir llamando a **StartAsync** y **StopAsync** para capturar más vídeos. Cuando hayas terminado de capturar vídeos, llama a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) para desechar la sesión de captura y limpiar los recursos asociados. Después de esta llamada, debes llamar a **PrepareLowLagRecordToStorageFileAsync** otra vez para reinicializar la sesión de captura antes de llamar a **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

A la hora de capturar un vídeo, debes registrar un controlador para el evento [**RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.RecordLimitationExceeded) del objeto **MediaCapture**, que el sistema operativo generará si se supera el límite para una única grabación, actualmente 3horas como máximo. En el controlador del evento, se deberá finalizar la grabación con una llamada a [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopAsync).

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Reproducir y editar archivos de vídeo capturado
Una vez que hayas capturado un vídeo en un archivo, es posible que quieras cargar el archivo y reproducirlo dentro de la interfaz de usuario de la aplicación. Puedes hacerlo mediante el control XAML **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** y un valor de **[MediaPlayer ](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** asociado. Para obtener más información sobre la reproducción de contenido multimedia en una página XAML, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

También puedes crear un objeto **[MediaClip](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip)** desde un archivo de vídeo mediante una llamada a **[CreateFromFileAsync ](https://docs.microsoft.com/uwp/api/windows.media.editing.mediaclip.createfromfileasync)**.  Un valor de **[MediaComposition](https://docs.microsoft.com/uwp/api/windows.media.editing.mediacomposition)** proporciona funcionalidad de edición de vídeo básico como organizar la secuencia de los objetos **MediaClip**, recortar la duración del vídeo, crear capas, agregar música de fondo y aplicar efectos de vídeo. Para más información sobre el trabajo con composiciones multimedia, consulta [Composiciones multimedia y edición](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Pausar y reanudar la grabación de vídeo
Puedes pausar una grabación de vídeo y luego reanudarla sin tener que crear un archivo de salida independiente mediante una llamada a [**PauseAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseAsync) y, a continuación, una llamada a [**ResumeAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.ResumeAsync).

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

A partir de Windows10, versión 1607, puedes pausar una grabación de vídeo y recibir el último fotograma capturado antes de que se haya pausado dicha grabación. A continuación, se puede superponer este fotograma en la vista previa de la cámara para permitir al usuario alinear la cámara con el fotograma en pausa antes de reanudar la grabación. Una llamada a [**PauseWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.PauseWithResultAsync) devuelve un objeto [**MediaCapturePauseResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult). La propiedad [**LastFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapturePauseResult.LastFrame) es un objeto [**VideoFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.VideoFrame) que representa el último fotograma. Para mostrar el fotograma en XAML, obtén la representación de **SoftwareBitmap** del fotograma de vídeo. Actualmente, solo se admiten imágenes en formato BGRA8 con canal alfa premultiplicado o vacío, por lo que debes llamar a [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) si es necesario para obtener el formato correcto.  Crea un nuevo objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) y llama a [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource.SetBitmapAsync) para inicializarlo. Por último, establece la propiedad **Source** de un control [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) de XAML para mostrar la imagen. Para que esto funcione, la imagen debe estar alineada con el control **CaptureElement** y debe poseer un valor de opacidad inferior a 1. No olvides que solo puedes modificar la interfaz de usuario en el subproceso de la interfaz de usuario, por lo que debes hacer esta llamada dentro de [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher.RunAsync).

**PauseWithResultAsync** también devuelve la duración del vídeo que se haya grabado en el segmento anterior, en caso de que debas controlar cuánto tiempo se ha grabado en total.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Al reanudar la grabación, puedes establecer el origen de la imagen en null y ocultarla.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Ten en cuenta que también puedes obtener un fotograma de resultado si detienes el vídeo mediante una llamada a [**StopWithResultAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StopWithResultAsync).


## <a name="capture-audio"></a>Capturar audio 
Puedes agregar rápidamente captura de audio a tu aplicación mediante el uso de la misma técnica que se muestra anteriormente para la captura de vídeo. El ejemplo que se muestra a continuación crea un objeto **StorageFile** en la carpeta de datos de la aplicación. Llama a [**PrepareLowLagRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.PrepareLowLagRecordToStorageFileAsync) para inicializar la sesión de captura, pasando el archivo y un objeto [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile), que en este ejemplo lo genera el método estático [**CreateMp3**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.MediaProperties.MediaEncodingProfile.CreateMp3). Para comenzar la grabación, llama a [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.StartAsync).

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Llama a [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagPhotoSequenceCapture.StopAsync) para detener la grabación de audio.

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Puedes llamar a **StartAsync** y **StopAsync** varias veces para grabar varios archivos de audio. Cuando hayas terminado de capturar audio, llama a [**FinishAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.LowLagMediaRecording.FinishAsync) para desechar la sesión de captura y limpiar los recursos asociados. Después de esta llamada, debes llamar a **PrepareLowLagRecordToStorageFileAsync** otra vez para reinicializar la sesión de captura antes de llamar a **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar y responder a cambios de nivel de audio por el sistema
A partir de Windows 10, versión 1803, la aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de la captura de audio de la aplicación y las secuencias de representación de audio. Por ejemplo, el sistema puede silenciar secuencias de la aplicación cuando pasa a segundo plano. La clase [**AudioStateMonitor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor) te permite registrarte para recibir un evento cuando el sistema modifica el volumen de una secuencia de audio. Obtén una instancia de **AudioStateMonitor** para supervisar las secuencias de captura de audio llamando a [**CreateForCaptureMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring). Obtén una instancia para supervisar las secuencias de representación de audio llamando a [**CreateForRenderMonitoring**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring). Registra un controlador para el evento [**SoundLevelChanged**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) de cada monitor al que se notificará cuando el sistema cambia el audio de la categoría de secuencia correspondiente.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

En el controlador **SoundLevelChanged** para la secuencia de captura, puedes comprobar la propiedad [**SoundLevel**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) del remitente **AudioStateMonitor** remitente para determinar el nuevo nivel de sonido. Ten en cuenta que el sistema nunca debe reducir o "atenuar" una secuencia de captura. Solo debe silenciarse o volver al volumen completo. Si la secuencia de audio está silenciada, puedes detener una captura en curso. Si la secuencia de audio se restaura a volumen completo, puedes iniciar la captura de nuevo. El siguiente ejemplo usa algunas variables de clase booleana para realizar un seguimiento de si la aplicación está capturando audio realmente y si la captura se detuvo debido al cambio de estado de audio. Estas variables se usan para determinar cuándo es apropiado detener o iniciar mediante programación la captura de audio.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

El siguiente ejemplo de código muestra una implementación del controlador **SoundLevelChanged** para la representación de audio. En función del escenario de la aplicación y del tipo de contenido que reproduzcas, es posible que quieras pausar la reproducción de audio cuando se atenúe el nivel de sonido. Para obtener más información sobre cómo controlar los cambios de nivel de sonidos para la reproducción multimedia, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Capturar fotos y vídeos con la interfaz de usuario de la cámara integrada en Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Controlar la orientación del dispositivo con MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Crear, editar y guardar imágenes de mapa de bits](imaging.md)
* [Archivos, carpetas y bibliotecas](https://msdn.microsoft.com/windows/uwp/files/index)

