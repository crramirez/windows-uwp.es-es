---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: En este artículo se muestra cómo controlar la orientación del dispositivo al capturar fotos y vídeos con una clase auxiliar.
title: Controlar la orientación del dispositivo con MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 18e135e85902bb3e00b7a09b8ee98a4e02b71f15
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362648"
---
# <a name="handle-device-orientation-with-mediacapture"></a>Controlar la orientación del dispositivo con MediaCapture
Cuando la aplicación captura una foto o un vídeo para su visualización en otra ubicación (por ejemplo, si se guarda en un archivo en el dispositivo del usuario o se comparte en línea), es importante codificar la imagen con los metadatos de orientación correctos para que, cuando la imagen se muestre en otro dispositivo o aplicación, su orientación sea correcta. Determinar los datos de orientación correctos que se deben incluir en un archivo multimedia puede ser una tarea compleja, ya que hay que tener en cuenta distintas variables, incluida la orientación del chasis del dispositivo, la orientación de la pantalla y la colocación de la cámara en el chasis (si es una cámara frontal o posterior). 

Para simplificar el control de la orientación, se recomienda usar una clase auxiliar, **CameraRotationHelper**, cuya definición completa se proporciona al final de este artículo. Puedes agregar esta clase al proyecto y, después, seguir los pasos de este artículo para que la aplicación de cámara admita la orientación. La clase auxiliar también facilita el giro de los controles de la interfaz de usuario de cámara para que se representen correctamente desde el punto de visión del usuario.

> [!NOTE] 
> Este artículo se basa en el código y los conceptos tratados en el artículo [**Captura básica de fotos, audio y vídeo con MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). Se recomienda familiarizarse con los conceptos básicos del uso de la clase **MediaCapture** antes de incorporar compatibilidad con la orientación a la aplicación.

## <a name="namespaces-used-in-this-article"></a>Espacios de nombres usados en este artículo
El código de ejemplo de este artículo usa las API de los siguientes espacios de nombres, que se deben incluir en el código. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetOrientationUsing":::

El primer paso para incorporar compatibilidad con la orientación a la aplicación es bloquear la pantalla para que no gire automáticamente al girar el dispositivo. La rotación automática de la interfaz de usuario funciona bien para la mayoría de tipos de aplicaciones, pero no es intuitiva para los usuarios cuando la vista previa de cámara gira. Para bloquear la orientación de la pantalla, establece la propiedad [**DisplayInformation.AutoRotationPreferences**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) en [**DisplayOrientations.Landscape**](/uwp/api/Windows.Graphics.Display.DisplayOrientations). 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetAutoRotationPreference":::

## <a name="tracking-the-camera-device-location"></a>Seguimiento de la ubicación del dispositivo de cámara
Para calcular la orientación correcta para los archivos multimedia capturados, la aplicación debe determinar la ubicación del dispositivo de cámara en el chasis. Agrega una variable de miembro booleano para comprobar si la cámara es externa al dispositivo, como una cámara web USB. Agrega otra variable booleana para comprobar si la vista previa debe reflejarse, como sucede al usar una cámara frontal. Asimismo, agrega una variable para almacenar un objeto **DeviceInformation** que represente la cámara seleccionada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCameraDeviceLocationBools":::
:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareCameraDevice":::

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>Seleccionar un dispositivo de cámara e inicializar el objeto MediaCapture
El artículo [**Captura básica de fotos, audio y vídeo con MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) muestra cómo inicializar el objeto **MediaCapture** con solo un par de líneas de código. Para admitir la orientación de la cámara, agregaremos algunos pasos más al proceso de inicialización.

En primer lugar, llama a [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) y pasa el selector de dispositivos [**DeviceClass.VideoCapture**](/uwp/api/Windows.Devices.Enumeration.DeviceClass) para obtener una lista de todos los dispositivos de captura de vídeo disponibles. A continuación, selecciona el primer dispositivo de la lista cuya ubicación del panel de la cámara se conozca y cuyo valor proporcionado coincida, que en este ejemplo es una cámara frontal. Si no se encuentra ninguna cámara en el panel deseado, se usa la primera cámara disponible o la predeterminada.

Si se encuentra un dispositivo de cámara, se crea un nuevo objeto [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings) y la propiedad [**VideoDeviceId**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid) se establece en el dispositivo seleccionado. A continuación, crea el objeto MediaCapture, llama a [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) y pasa el objeto de configuración para indicar al sistema que use la cámara seleccionada.

Por último, comprueba si el panel de dispositivo seleccionado es nulo o desconocido. Si es así, la cámara es externa, lo que significa que su rotación no está relacionada con la rotación del dispositivo. Si el panel es conocido y se encuentra en la parte frontal del chasis del dispositivo, sabemos que la vista previa debe reflejarse, por lo que se establece la variable que lo controla.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitMediaCaptureWithOrientation":::
## <a name="initialize-the-camerarotationhelper-class"></a>Inicializar la clase CameraRotationHelper

Ahora empezamos usando la clase **CameraRotationHelper**. Declara una variable de miembro de clase para almacenar el objeto. Llama al constructor y pasa la ubicación de la carcasa de la cámara seleccionada. La clase auxiliar usa esta información para calcular la orientación correcta para los archivos multimedia capturados, la secuencia de vista previa y la interfaz de usuario. Registra un controlador para el evento **OrientationChanged** de la clase auxiliar, que se generará cuando necesitemos actualizar la orientación de la interfaz de usuario o la secuencia de vista previa.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetDeclareRotationHelper":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetInitRotationHelper":::

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>Agregar datos de orientación a la secuencia de vista previa de la cámara
La adición de la orientación correcta a los metadatos de la secuencia de vista previa no afecta al modo en que la vista previa se muestra al usuario, pero ayuda al sistema a codificar los fotogramas capturados de la secuencia de vista previa correctamente.

Para iniciar la vista previa de cámara, llama a [**MediaCapture.StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Antes de hacerlo, comprueba la variable de miembro para ver si la vista previa debe reflejarse (para una cámara frontal). Si es así, establece la propiedad [**FlowDirection**](/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) de la clase [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement), denominada *PreviewControl* en este ejemplo, en [**FlowDirection.RightToLeft**](/uwp/api/Windows.UI.Xaml.FlowDirection). Después de iniciar la vista previa, llama al método auxiliar **SetPreviewRotationAsync** para establecer la rotación de la vista previa. A continuación se muestra la implementación de este método.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartPreviewWithRotationAsync":::

Establecemos la rotación de la vista previa en un método separado para que se pueda actualizar cuando la orientación del teléfono cambie sin reinicializar la secuencia de vista previa. Si la cámara es externa al dispositivo, no se realiza ninguna acción. De lo contrario, se llama al método de **CameraRotationHelper****GetCameraPreviewOrientation**, que devuelve la orientación correcta para la secuencia de vista previa. 

Para establecer los metadatos, las propiedades de secuencia de vista previa se recuperan llamando a [**VideoDeviceController.GetMediaStreamProperties**](/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties). A continuación, crea un GUID que represente el atributo de Media Foundation Transform (MFT) para la rotación de la secuencia de vídeo. En C++, puedes usar la constante [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation), pero en C#, debes especificar manualmente el valor GUID. 

Agrega un valor de propiedad al objeto de propiedades de secuencia, y especifica el GUID como la clave y la rotación de la vista previa como el valor. Esta propiedad espera que los valores estén en unidades de grados en sentido contrario a las agujas del reloj, por lo que el método de **CameraRotationHelper****ConvertSimpleOrientationToClockwiseDegrees** se usa para convertir el valor de orientación simple. Por último, llama a [**SetEncodingPropertiesAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_) para aplicar la nueva propiedad de rotación a la secuencia.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetSetPreviewRotationAsync":::

A continuación, agrega el controlador para el evento **CameraRotationHelper.OrientationChanged**. Este evento pasa un argumento que te permite saber si la secuencia de vista previa se debe girar. Si la orientación del dispositivo cambia a boca arriba o boca abajo, este valor será false. Si la vista previa no se debe girar, llama al método **SetPreviewRotationAsync** definido anteriormente.

Luego, en el controlador de eventos **OrientationChanged**, actualiza la interfaz de usuario si es necesario. Obtén la orientación de la interfaz de usuario recomendada actualmente de la clase auxiliar llamando a **GetUIOrientation** y convierte el valor a grados en el sentido de las agujas del reloj, que se usa para transformaciones de XAML. Crea una clase [**RotateTransform**](/uwp/api/Windows.UI.Xaml.Media.RotateTransform) a partir del valor de orientación y establece la propiedad [**RenderTransform**](/uwp/api/windows.ui.xaml.uielement.rendertransform) de los controles XAML. Según el diseño de la interfaz de usuario, deberás realizar ajustes adicionales, aparte de la simple rotación de los controles. Recuerda también que todas las actualizaciones de la interfaz de usuario deben realizarse en el subproceso de la interfaz de usuario, por lo que debes colocar este código dentro de una llamada a [**RunAsync**](/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_).  

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetHelperOrientationChanged":::

## <a name="capture-a-photo-with-orientation-data"></a>Capturar una foto con datos de orientación
En el artículo [**Captura básica de fotos, audio y vídeo con MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) se muestra cómo capturar una foto en un archivo mediante la captura en primer lugar en una secuencia en memoria y, a continuación, el uso de un descodificador para leer los datos de imagen de la secuencia y un codificador para transcodificar los datos de imagen en un archivo. Los datos de orientación, obtenidos de la clase **CameraRotationHelper**, se pueden agregar al archivo de imagen durante la operación de transcodificación.

En el ejemplo siguiente, se captura una foto en una clase [**InMemoryRandomAccessStream**](/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) con una llamada a [**CapturePhotoToStreamAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync) y se crea una clase [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) a partir de la secuencia. A continuación, se crea una clase [**StorageFile**](/uwp/api/Windows.Storage.StorageFile), que se abre para recuperar una interfaz [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) para escribir en el archivo. 

Antes de transcodificar el archivo, la orientación de la foto se recupera del método de clase auxiliar **GetCameraCaptureOrientation**. Este método devuelve un objeto [**SimpleOrientation**](/uwp/api/Windows.Devices.Sensors.SimpleOrientation) que se convierte en un objeto [**PhotoOrientation**](/uwp/api/Windows.Storage.FileProperties.PhotoOrientation) con el método auxiliar **ConvertSimpleOrientationToPhotoOrientation**. Luego, se crea un nuevo objeto **BitmapPropertySet** y se agrega una propiedad donde la clave es "System.Photo.Orientation" y el valor es la orientación de la foto, expresado como **BitmapTypedValue**. "System.Photo.Orientation" es una de muchas propiedades de Windows que se pueden agregar a un archivo de imagen como metadatos. Para obtener una lista de todas las propiedades relacionadas con fotos, consulta [**Windows Properties - Photo**](/previous-versions/ff516600(v=vs.85)). (Propiedades de Windows: Foto) Para obtener más información sobre el trabajo con metadatos en las imágenes, consulta [**Metadatos de imagen**](image-metadata.md).

Por último, el conjunto de propiedades que incluye los datos de orientación se establece para el codificador con una llamada a [**SetPropertiesAsync**](../develop/index.md) y la imagen se transcodifica con una llamada a [**FlushAsync**](/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetCapturePhotoWithOrientation":::

## <a name="capture-a-video-with-orientation-data"></a>Capturar un vídeo con datos de orientación
La captura de vídeo básica se describe en el artículo [**Captura básica de fotos, audio y vídeo con MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). La adición de datos de orientación a la codificación del vídeo capturado se realiza con la misma técnica descrita anteriormente en la sección sobre la adición de datos de orientación a la secuencia de vista previa.

En el ejemplo siguiente, se crea un archivo en el que se escribirá el vídeo capturado. Un perfil de codificación MP4 se crea con el método estático [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4). La orientación correcta del vídeo se obtiene de la clase **CameraRotationHelper** con una llamada a **GetCameraCaptureOrientation**. Dado que la propiedad de rotación requiere que la orientación se exprese en grados en sentido contrario a las agujas del reloj, se llama al método auxiliar **ConvertSimpleOrientationToClockwiseDegrees** para convertir el valor de orientación. A continuación, crea un GUID que represente el atributo de Media Foundation Transform (MFT) para la rotación de la secuencia de vídeo. En C++, puedes usar la constante [**MF_MT_VIDEO_ROTATION**](/windows/desktop/medfound/mf-mt-video-rotation), pero en C#, debes especificar manualmente el valor GUID. Agrega un valor de propiedad al objeto de propiedades de secuencia, y especifica el GUID como la clave y la rotación como el valor. Por último, llama a [**StartRecordToStorageFileAsync**](/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_) para empezar a grabar vídeo codificado con datos de orientación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs" id="SnippetStartRecordingWithOrientationAsync":::

## <a name="camerarotationhelper-full-code-listing"></a>Listado de código completo de CameraRotationHelper
El siguiente fragmento de código muestra el código completo de la clase **CameraRotationHelper** que administra los sensores de orientación de hardware, calcula los valores de orientación correctos para fotos y vídeos, y proporciona métodos auxiliares para la conversión entre las diferentes representaciones de orientación que se usan en las distintas características de Windows. Si sigues las instrucciones que se muestran en el artículo anterior, puedes agregar esta clase al proyecto tal cual sin tener que realizar ningún cambio. Por supuesto, puedes personalizar el código siguiente para satisfacer las necesidades de tu escenario concreto.

Esta clase auxiliar usa la clase [**SimpleOrientationSensor**](/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) del dispositivo para determinar la orientación actual del chasis del dispositivo y la clase [**DisplayInformation**](/uwp/api/Windows.Graphics.Display.DisplayInformation) para determinar la orientación actual de la pantalla. Cada una de estas clases proporciona eventos que se generan cuando la orientación actual cambia. El panel en el que se monta el dispositivo de captura (frontal, posterior o externo) se usa para determinar si la secuencia de vista previa debe reflejarse. Además, la propiedad [**EnclosureLocation.RotationAngleInDegreesClockwise**](/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise), compatible con algunos dispositivos, se usa para determinar la orientación de montaje de la cámara en el chasis.

Los siguientes métodos pueden usarse para obtener los valores de orientación recomendados para las tareas de la aplicación de cámara especificada:

* **GetUIOrientation**: devuelve la orientación sugerida para los elementos de la interfaz de usuario de la cámara.
* **GetCameraCaptureOrientation**: devuelve la orientación sugerida para codificar los metadatos de imagen.
* **GetCameraPreviewOrientation**: devuelve la orientación sugerida para la secuencia de vista previa a fin de proporcionar una experiencia de usuario natural.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs" id="SnippetCameraRotationHelperFull":::



## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
