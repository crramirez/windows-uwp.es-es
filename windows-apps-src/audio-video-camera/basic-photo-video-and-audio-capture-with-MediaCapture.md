---
ms.assetid: 66d0c3dc-81f6-4d9a-904b-281f8a334dd0
description: Este artículo muestra la forma más sencilla de capturar fotos y vídeos mediante la clase MediaCapture.
title: Captura básica de fotos, audio y vídeo con MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 2a1bdb033d9c0d47973c26b28dc357a4000d4099
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161129"
---
# <a name="basic-photo-video-and-audio-capture-with-mediacapture"></a>Captura básica de fotos, audio y vídeo con MediaCapture


Este artículo muestra la forma más sencilla de capturar fotos y vídeos mediante la clase [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture). La clase **MediaCapture** expone un eficaz conjunto de API que proporciona control de bajo nivel sobre la canalización de captura y habilita escenarios de captura avanzados, pero este artículo está pensado para ayudar a agregar la captura multimedia básica a la aplicación de forma rápida y fácil. Para obtener más información sobre otras características que proporciona **MediaCapture**, consulta [**Cámara**](camera.md).

Si simplemente deseas capturar una foto o un vídeo y no quieres agregar otras características de captura multimedia, o si no deseas crear tu propia interfaz de usuario de cámara, es aconsejable usar la clase [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI), que te permite simplemente iniciar la aplicación de cámara integrada de Windows y recibir el archivo de foto o vídeo que se captura. Para obtener más información, consulta [**Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows**](capture-photos-and-video-with-cameracaptureui.md).

El código de este artículo es una adaptación de la muestra del [**Camera starter kit**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraStarterKit) (Starter Kit de la cámara). Puedes descargar la muestra para ver el código usado en contexto o para emplearla como punto de partida para tu propia aplicación.

## <a name="add-capability-declarations-to-the-app-manifest"></a>Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades *webcam* y *microphone* del dispositivo. Si quieres guardar las fotos y los vídeos capturados en la biblioteca de imágenes de los usuarios, también debes declarar las funcionalidades *picturesLibrary* y *videosLibrary*.

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Seleccione la pestaña **Funcionalidades**.
3.  Active la casilla de la **cámara web** y el cuadro de **micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y de **Biblioteca de vídeos**.


## <a name="initialize-the-mediacapture-object"></a>Inicializar el objeto MediaCapture
Todos los métodos de captura que se describe en este artículo requieren el primer paso de inicializar el objeto [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) mediante una llamada al constructor y, a continuación, llamar a [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync). Dado que se accederá al objeto **MediaCapture** desde varias ubicaciones de la aplicación, declara una variable de clase para que contenga el objeto.  Implementa un controlador para el evento [**Failed**](/uwp/api/windows.media.capture.mediacapture.failed) del objeto **MediaCapture** para recibir una notificación si se produce un error en una operación de captura.

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

[!code-cs[InitMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-up-the-camera-preview"></a>Configurar la vista previa de la cámara
Es posible capturar fotos, vídeos y audio usando **MediaCapture** sin mostrar la vista previa de la cámara, pero lo normal es que desees mostrar la vista previa de la secuencia para que el usuario pueda ver lo que se captura. Además, algunas características de **MediaCapture** requieren que se ejecute la vista previa de la secuencia para poder habilitarlas, incluidos el enfoque automático, la exposición automática y el balance de blancos automático. Para ver cómo configurar la vista previa de la cámara, consulta [**Mostrar la vista previa de la cámara**](simple-camera-preview-access.md).

## <a name="capture-a-photo-to-a-softwarebitmap"></a>Capturar una foto en un objeto SoftwareBitmap
La clase [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) se introdujo en Windows 10 para proporcionar una representación común de las imágenes en distintas características. Si quieres capturar una foto y usar inmediatamente la imagen capturada en la aplicación, como mostrarla en XAML, en lugar de capturarla en un archivo, deberías capturarla en un objeto **SoftwareBitmap**. Seguirás teniendo la opción de guardar dicha imagen en el disco más adelante.

Tras inicializar el objeto **MediaCapture**, puedes capturar una foto en un objeto **SoftwareBitmap** mediante la clase [**LowLagPhotoCapture**](/uwp/api/Windows.Media.Capture.LowLagPhotoCapture). Para obtener una instancia de esta clase, llama a [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync), pasando un objeto [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique el formato de imagen que desees. [**CreateUncompressed**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createuncompressed) crea una codificación sin comprimir con el formato de píxeles especificado. Para capturar una foto, llama a [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync), lo que devolverá un objeto [**CapturedPhoto**](/uwp/api/Windows.Media.Capture.CapturedPhoto). Para obtener un objeto **SoftwareBitmap**, accede a la propiedad [**Frame**](/uwp/api/windows.media.capture.capturedphoto.frame) y luego a la propiedad [**SoftwareBitmap**](/uwp/api/windows.media.capture.capturedframe.softwarebitmap).

Si lo deseas, puedes capturar varias fotos si llamas repetidamente a **CaptureAsync**. Cuando hayas terminado de capturar, llama a [**FinishAsync**](/uwp/api/windows.media.capture.advancedphotocapture.finishasync) para cerrar la sesión de **LowLagPhotoCapture** y liberar los recursos asociados. Tras llamar a **FinishAsyncpara**, si deseas volver a empezar a capturar fotos, deberás llamar a [**PrepareLowLagPhotoCaptureAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagphotocaptureasync) otra vez para reinicializar la sesión de captura antes de llamar a [**CaptureAsync**](/uwp/api/windows.media.capture.lowlagphotocapture.captureasync).

[!code-cs[CaptureToSoftwareBitmap](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToSoftwareBitmap)]

A partir de Windows, versión 1803, puede tener acceso a la propiedad [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) de la clase **CapturedFrame** devuelta desde **CaptureAsync** para recuperar los metadatos sobre la foto capturada. Puede pasar estos datos a un **BitmapEncoder** para guardar los metadatos en un archivo. Anteriormente, no había forma de tener acceso a estos datos para los formatos de imagen no comprimidos. También puede tener acceso a la propiedad [**ControlValues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) para recuperar un objeto [**CapturedFrameControlValues**](/uwp/api/windows.media.capture.capturedframecontrolvalues) que describe los valores de control, como la exposición y el balance de blanco, para el fotograma capturado.

Para obtener información sobre cómo usar **BitmapEncoder** y sobre cómo trabajar con el objeto **SoftwareBitmap** , incluido cómo mostrar uno en una página XAML, vea [**crear, editar y guardar imágenes de mapa de bits**](imaging.md). 

Para obtener más información sobre la configuración de los valores de control de dispositivos de captura, consulte [capturar controles de dispositivo para fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md).

A partir de la versión 1803 de Windows 10, puede obtener los metadatos, como la información EXIF, para las fotos capturadas en formato sin comprimir mediante el acceso a la propiedad [**BitmapProperties**](/uwp/api/windows.media.capture.capturedframe.bitmapproperties) del **CapturedFrame** devuelto por **MediaCapture**. En versiones anteriores, solo se podía tener acceso a estos datos en el encabezado de las fotos capturadas en un formato de archivo comprimido. Puede proporcionar estos datos a un [**BitmapEncoder**](/uwp/api/windows.graphics.imaging.bitmapencoder) cuando escribe manualmente un archivo de imagen. Para obtener más información sobre la codificación de mapas de bits, vea [crear, editar y guardar imágenes de mapa de bits](imaging.md).  También puede tener acceso a los valores de control de marco, como la configuración de exposición y Flash, que se usa cuando se captura la imagen mediante el acceso a la propiedad [**ControlValues**](/uwp/api/windows.media.capture.capturedframe.controlvalues) . Para obtener más información, vea [capturar controles de dispositivo para la captura de fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md).

## <a name="capture-a-photo-to-a-file"></a>Capturar una foto a un archivo
Una aplicación de fotografía típica guardará una foto en el disco o en el almacenamiento en la nube y tendrá que agregar metadatos al archivo, como la orientación de la fotografía. El siguiente ejemplo muestra cómo capturar una foto en un archivo. Seguirás teniendo la opción de crear un objeto **SoftwareBitmap** a partir del archivo de imagen más adelante. 

La técnica que se muestra en este ejemplo captura la foto en una secuencia de memoria y, a continuación, transcodifica la foto desde dicha secuencia a un archivo del disco. Este ejemplo usa [**GetLibraryAsync**](/uwp/api/windows.storage.storagelibrary.getlibraryasync) para obtener la biblioteca de imágenes del usuario y luego la propiedad [**SaveFolder**](/uwp/api/windows.storage.storagelibrary.savefolder) para conseguir una carpeta para guardar predeterminada de referencia. Recuerda que tienes que agregar la funcionalidad **Pictures Library** al manifiesto de la aplicación para acceder a esta carpeta. [**CreateFileAsync**](/uwp/api/windows.storage.storagefolder.createfileasync) crea un nuevo objeto [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) en el que se guardará la foto capturada.

Crea un objeto [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) y, a continuación, llama a [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) para capturar una foto en la secuencia, pasando la secuencia y un objeto [**ImageEncodingProperties**](/uwp/api/Windows.Media.MediaProperties.ImageEncodingProperties) que especifique el formato de imagen que debe usarse. Puedes crear propiedades de codificación personalizadas si inicializas el objeto por ti mismo, pero la clase proporciona métodos estáticos, como [**ImageEncodingProperties.CreateJpeg**](/uwp/api/windows.media.mediaproperties.imageencodingproperties.createjpeg), para formatos de codificación comunes. A continuación, crea una secuencia de archivos en el archivo de salida llamando a [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync). Crea un objeto [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) para descodificar la imagen de la secuencia de memoria y, a continuación, crea un objeto [**BitmapEncoder**](/uwp/api/Windows.Graphics.Imaging.BitmapEncoder) para codificar la imagen en el archivo mediante una llamada a [**CreateForTranscodingAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.createfortranscodingasync).

También tienes la opción de crear un objeto [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) y luego llamar a [**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) en el codificador de imágenes para incluir metadatos sobre la foto en el archivo de imagen. Para obtener más información sobre las propiedades de codificación, consulta [**Metadatos de imagen**](image-metadata.md). Administrar la orientación del dispositivo correctamente es esencial para la mayoría de las aplicaciones de fotografía. Para obtener más información, consulta [**Controlar la orientación del dispositivo con MediaCapture**](handle-device-orientation-with-mediacapture.md).

Por último, llama a [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync) en el objeto del codificador para transcodificar la foto desde la secuencia de memoria al archivo.

[!code-cs[CaptureToFile](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureToFile)]

Para obtener más información sobre cómo trabajar con archivos y carpetas, consulta [**Archivos, carpetas y bibliotecas**](../files/index.md).

## <a name="capture-a-video"></a>Capturar un vídeo
Agrega rápidamente la captura de vídeo a tu aplicación mediante la clase [**LowLagMediaRecording**](/uwp/api/Windows.Media.Capture.LowLagMediaRecording). En primer lugar, declara una variable de clase para el objeto.

[!code-cs[LowLagMediaRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetLowLagMediaRecording)]

A continuación, crea un objeto **StorageFile** en el que se guardará el vídeo. Ten en cuenta que para guardar en la biblioteca de vídeos del usuario, como se muestra en este ejemplo, debes agregar la funcionalidad **Videos Library** al manifiesto de la aplicación. Llama a [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) para inicializar la grabación multimedia, pasando el archivo de almacenamiento y un objeto [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) que especifique la codificación del vídeo. La clase proporciona métodos estáticos, como [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), para crear perfiles de las codificaciones de vídeo más comunes.

Por último, llama a [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync) para empezar a capturar vídeo.

[!code-cs[StartVideoCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartVideoCapture)]

Para detener la grabación de vídeo, llama a [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Puedes seguir llamando a **StartAsync** y **StopAsync** para capturar más vídeos. Cuando hayas terminado de capturar vídeos, llama a [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) para desechar la sesión de captura y limpiar los recursos asociados. Después de esta llamada, debes llamar a **PrepareLowLagRecordToStorageFileAsync** otra vez para reinicializar la sesión de captura antes de llamar a **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]

A la hora de capturar un vídeo, debes registrar un controlador para el evento [**RecordLimitationExceeded**](/uwp/api/windows.media.capture.mediacapture.recordlimitationexceeded) del objeto **MediaCapture**, que el sistema operativo generará si se supera el límite para una única grabación, actualmente 3 horas como máximo. En el controlador del evento, se deberá finalizar la grabación con una llamada a [**StopAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopasync).

[!code-cs[RecordLimitationExceeded](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

[!code-cs[RecordLimitationExceededHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceededHandler)]

### <a name="play-and-edit-captured-video-files"></a>Reproducir y editar archivos de vídeo capturados
Una vez que haya capturado un vídeo en un archivo, puede que desee cargar el archivo y reproducirlo en la interfaz de usuario de la aplicación. Para ello, puede usar el control XAML de **[MediaPlayerElement](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)** y un objeto **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** asociado. Para obtener información sobre cómo reproducir archivos multimedia en una página XAML, vea [reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

También puede crear un objeto **[MediaClip](/uwp/api/windows.media.editing.mediaclip)** desde un archivo de vídeo llamando a **[CreateFromFileAsync](/uwp/api/windows.media.editing.mediaclip.createfromfileasync)**.  Un objeto **[MediaComposition](/uwp/api/windows.media.editing.mediacomposition)** proporciona funcionalidad básica de edición de vídeo, como organizar la secuencia de objetos **MediaClip** , recortar la longitud del vídeo, crear capas, agregar música de fondo y aplicar efectos de vídeo. Para obtener más información sobre cómo trabajar con composiciones multimedia, consulte [composiciones y edición de elementos multimedia](media-compositions-and-editing.md).

## <a name="pause-and-resume-video-recording"></a>Pausar y reanudar la grabación de vídeo
Puedes pausar una grabación de vídeo y luego reanudarla sin tener que crear un archivo de salida independiente mediante una llamada a [**PauseAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pauseasync) y, a continuación, una llamada a [**ResumeAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.resumeasync).

[!code-cs[PauseRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseRecordingSimple)]

[!code-cs[ResumeRecordingSimple](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeRecordingSimple)]

A partir de Windows 10, versión 1607, puedes pausar una grabación de vídeo y recibir el último fotograma capturado antes de que se haya pausado dicha grabación. A continuación, se puede superponer este fotograma en la vista previa de la cámara para permitir al usuario alinear la cámara con el fotograma en pausa antes de reanudar la grabación. Una llamada a [**PauseWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.pausewithresultasync) devuelve un objeto [**MediaCapturePauseResult**](/uwp/api/Windows.Media.Capture.MediaCapturePauseResult). La propiedad [**LastFrame**](/uwp/api/windows.media.capture.mediacapturepauseresult.lastframe) es un objeto [**VideoFrame**](/uwp/api/Windows.Media.VideoFrame) que representa el último fotograma. Para mostrar el fotograma en XAML, obtén la representación de **SoftwareBitmap** del fotograma de vídeo. Actualmente, solo se admiten imágenes en formato BGRA8 con canal alfa premultiplicado o vacío, por lo que debes llamar a [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) si es necesario para obtener el formato correcto.  Crea un nuevo objeto [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) y llama a [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para inicializarlo. Por último, establece la propiedad **Source** de un control [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) de XAML para mostrar la imagen. Para que esto funcione, la imagen debe estar alineada con el control **CaptureElement** y debe poseer un valor de opacidad inferior a 1. No olvides que solo puedes modificar la interfaz de usuario en el subproceso de la interfaz de usuario, por lo que debes hacer esta llamada dentro de [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync).

**PauseWithResultAsync** también devuelve la duración del vídeo que se haya grabado en el segmento anterior, en caso de que debas controlar cuánto tiempo se ha grabado en total.

[!code-cs[PauseCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetPauseCaptureWithResult)]

Al reanudar la grabación, puedes establecer el origen de la imagen en null y ocultarla.

[!code-cs[ResumeCaptureWithResult](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetResumeCaptureWithResult)]

Ten en cuenta que también puedes obtener un fotograma de resultado si detienes el vídeo mediante una llamada a [**StopWithResultAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.stopwithresultasync).


## <a name="capture-audio"></a>Capturar audio 
Puedes agregar rápidamente captura de audio a tu aplicación mediante el uso de la misma técnica que se muestra anteriormente para la captura de vídeo. El ejemplo que se muestra a continuación crea un objeto **StorageFile** en la carpeta de datos de la aplicación. Llama a [**PrepareLowLagRecordToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.preparelowlagrecordtostoragefileasync) para inicializar la sesión de captura, pasando el archivo y un objeto [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile), que en este ejemplo lo genera el método estático [**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3). Para comenzar la grabación, llama a [**StartAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.startasync).

[!code-cs[StartAudioCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartAudioCapture)]


Llama a [**StopAsync**](/uwp/api/windows.media.capture.lowlagphotosequencecapture.stopasync) para detener la grabación de audio.

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)  
[!code-cs[StopRecording](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStopRecording)]

Puedes llamar a **StartAsync** y **StopAsync** varias veces para grabar varios archivos de audio. Cuando hayas terminado de capturar audio, llama a [**FinishAsync**](/uwp/api/windows.media.capture.lowlagmediarecording.finishasync) para desechar la sesión de captura y limpiar los recursos asociados. Después de esta llamada, debes llamar a **PrepareLowLagRecordToStorageFileAsync** otra vez para reinicializar la sesión de captura antes de llamar a **StartAsync**.

[!code-cs[FinishAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetFinishAsync)]


## <a name="detect-and-respond-to-audio-level-changes-by-the-system"></a>Detectar y responder a los cambios de nivel de audio por parte del sistema
A partir de Windows 10, versión 1803, la aplicación puede detectar cuándo el sistema reduce o silencia el nivel de audio de las secuencias de audio y captura de audio de la aplicación. Por ejemplo, el sistema puede silenciar los flujos de la aplicación cuando entra en segundo plano. La clase [**AudioStateMonitor**](/uwp/api/windows.media.audio.audiostatemonitor) permite registrarse para recibir un evento cuando el sistema modifica el volumen de una secuencia de audio. Obtener una instancia de **AudioStateMonitor** para supervisar las secuencias de captura de audio llamando a [**CreateForCaptureMonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforcapturemonitoring#Windows_Media_Audio_AudioStateMonitor_CreateForCaptureMonitoring). Obtenga una instancia de para supervisar las secuencias de representación de audio llamando a [**CreateForRenderMonitoring**](/uwp/api/windows.media.audio.audiostatemonitor.createforrendermonitoring). Registra un controlador para el evento [**SoundLevelChanged**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevelchanged) de cada monitor que se va a notificar cuando el sistema cambie el audio de la categoría de flujo correspondiente.

[!code-cs[AudioStateMonitorUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateMonitorUsing)]

[!code-cs[AudioStateVars](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAudioStateVars)]

[!code-cs[RegisterAudioStateMonitor](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterAudioStateMonitor)]

En el controlador **SoundLevelChanged** del flujo de captura, puede comprobar la propiedad [**SoundLevel**](/uwp/api/windows.media.audio.audiostatemonitor.soundlevel) del remitente **AudioStateMonitor** para determinar el nuevo nivel de sonido. Tenga en cuenta que el sistema nunca debe reducir ni "pato" un flujo de captura. Nunca se debe silenciar o volver a cambiar al volumen completo. Si la secuencia de audio está silenciada, puede detener una captura en curso. Si la secuencia de audio se restaura en el volumen completo, puede volver a iniciar la captura. En el ejemplo siguiente se usan algunas variables de clase booleana para realizar el seguimiento de si la aplicación está capturando audio y si la captura se detuvo debido al cambio de estado de audio. Estas variables se usan para determinar cuándo es adecuado detener o iniciar la captura de audio mediante programación.

[!code-cs[CaptureSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCaptureSoundLevelChanged)]

En el ejemplo de código siguiente se muestra una implementación del controlador **SoundLevelChanged** para la representación de audio. En función del escenario de la aplicación y del tipo de contenido que esté reproduciendo, puede que quiera pausar la reproducción de audio cuando el nivel de sonido esté sobrescrito. Para obtener más información sobre cómo controlar los cambios de nivel de sonido para la reproducción multimedia, consulte [reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

[!code-cs[RenderSoundLevelChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRenderSoundLevelChanged)]


* [Capturar fotografías y vídeos con la interfaz de usuario de la cámara integrada de Windows](capture-photos-and-video-with-cameracaptureui.md)
* [Controlar la orientación del dispositivo con MediaCapture](handle-device-orientation-with-mediacapture.md)
* [Crear, editar y guardar imágenes de mapa de bits](imaging.md)
* [Archivos, carpetas y bibliotecas](../files/index.md)