---
author: drewbatgit
ms.assetid: 1361E82A-202F-40F7-9239-56F00DFCA54B
description: "En este artículo se describen los pasos necesarios para capturar fotos y vídeo mediante la API MediaCapture, incluidas la inicialización y el apagado de MediaCapture y el control de los cambios de orientación del dispositivo."
title: "Capturar fotografías y vídeos con MediaCapture"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c20c735d38e6baabe2f8bc0c7c682706d3946ed9

---

# Capturar fotografías y vídeos con MediaCapture

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este artículo se describen los pasos para capturar fotos y vídeo mediante las API [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124), incluidas la inicialización y el apagado de **MediaCapture** y el control de los cambios de orientación del dispositivo.

**MediaCapture** se proporciona para admitir las aplicaciones que requieren un control del proceso de captura de multimedia de bajo nivel y que implementan escenarios que requieren funcionalidades avanzadas de captura. El uso de **MediaCapture** también requiere que proporciones tu propia interfaz de usuario de captura. Si tu aplicación solo necesita capturar una foto o un vídeo y es ajeno a técnicas avanzadas de captura, el [**CameraCaptureUI**](https://msdn.microsoft.com/library/windows/apps/br241030) facilita el proceso de capturar una foto o un vídeo con tan solo unas líneas de código. Para más información, consulta [Capturar fotos y vídeos con CameraCaptureUI](capture-photos-and-video-with-cameracaptureui.md).

El código de este artículo es una adaptación de la [muestra de CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479). Puedes descargar la muestra para ver el código usado en contexto o para usar la muestra como punto de partida para tu propia aplicación.

## Configurar tu proyecto

### Agregar declaraciones de funcionalidades al manifiesto de la aplicación

Para que tu aplicación tenga acceso a la cámara de un dispositivo, debes declarar que esta usa las funcionalidades de *webcam* y *microphone* del dispositivo. Si quieres guardar las fotos y vídeos capturados en la biblioteca de imágenes de los usuarios, también debes declarar las funcionalidades *picturesLibrary* y *videosLibrary*.

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y el cuadro de **Biblioteca vídeos**.

### Agregar directivas de uso para API multimedia relacionadas con la captura

El siguiente código muestra los espacios de nombres que son referencia del código muestra en este artículo y describe la funcionalidad que proporciona cada espacio de nombres.

[!code-cs[Uso de](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## Inicializar el objeto MediaCapture

La clase [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124) en el espacio de nombres [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) es la interfaz fundamental para las operaciones de captura de todos los medios. Las aplicaciones suelen declarar una variable de este tipo de ámbito de una sola página. La aplicación necesita realizar un seguimiento del estado actual de la **MediaCapture**, por lo que debes declarar variables booleanas para la inicialización, en la vista previa y estado de grabación del objeto.

[!code-cs[MediaCaptureVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaCaptureVariables)]

Para ayudar a orientar la vista previa de vídeo correctamente, cree variables miembro para realizar un seguimiento de si la cámara es externa y si la aplicación está actualmente creación de reflejo de la secuencia de vista previa. La aplicación debería reflejar la secuencia de vista previa cuando crees que la fuente de vídeo captura al usuario porque es una experiencia de usuario más natural.

[!code-cs[PreviewVariables](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewVariables)]

El siguiente método de ejemplo inicializa el objeto de captura de multimedia. En primer lugar, el código busca un dispositivo de captura de vídeo que puede usarse para la captura de multimedia. Una vez encontrado, el objeto **MediaCapture** se inicializa y se registran los controladores para sus eventos. Junto a un objeto [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/desktop/hh802710), se crea mediante el identificador del dispositivo de captura de vídeo. El objeto **MediaCapture** se inicializa entonces con una llamada a [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598).

[!code-cs[InitializeCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitializeCameraAsync)]

-   El método [**DeviceInformation.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) puede usarse para buscar todos los dispositivos de un tipo especificado. En este ejemplo, el valor de enumeración **DeviceClass.VideoCapture** se pasa para indicar que se deben devolver los dispositivos de captura de vídeo. Ten en cuenta que un dispositivo de captura de vídeo se usa para capturar fotos y vídeos.

-   **FindAllAsync** devuelve un objeto [**DeviceInformationCollection**](https://msdn.microsoft.com/library/windows/apps/br225395) que contiene un objeto [**DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para cada dispositivo encontrado del tipo solicitado. El método de extensión **FirstOrDefault** del espacio de nombres **System.Linq** proporciona una sintaxis fácil para seleccionar un elemento de una lista en función de las condiciones especificadas. La primera llamada intenta seleccionar la primera **DeviceInformation** en la lista que tiene un valor [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) de **Panel.Back**, que indica que la cámara está en el panel posterior del gabinete del dispositivo. Si el dispositivo no tiene una cámara en el panel posterior, se usa la primera cámara disponible.

-   Si no se especifica un identificador de dispositivo para inicializar la [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573), el sistema elegirá el primer dispositivo en la lista interna de dispositivos.

-   La llamada a [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) inicializa el objeto para usar el dispositivo de captura especificado. Esta llamada se realiza dentro de un bloque **try** porque producirá una **UnauthorizedAccessException** si el usuario ha denegado el acceso de aplicación al llamar a la cámara. Si la llamada se realiza correctamente, la variable **\_isInitialized** se establece en true para que las llamadas a métodos posteriores puedan determinar si se ha inicializado el dispositivo de captura.

- **Importante** En algunas familias de dispositivos, se muestra al usuario una petición de consentimiento antes de que se conceda acceso a la aplicación para usar la cámara del dispositivo. Por este motivo, solo debes llamar a [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) desde la conversación principal de la interfaz de usuario. Si intentas inicializar la cámara desde otro subproceso, se pueden producir errores de inicialización.

-   Si la inicialización del dispositivo de captura se realiza correctamente, se establecen las variables para reflejar si el dispositivo de captura es externo, o si está en el panel frontal del dispositivo. Estos valores se usarán para orientar la vista previa de captura correctamente para el usuario. Por último, la interfaz de usuario se actualiza para reflejar que la captura está disponible y se inicia la secuencia de vista previa desde el dispositivo de captura. Todas estas tareas se realizan en métodos auxiliares que se describirán más adelante en este artículo.

## Iniciar la vista previa de captura

Para que el usuario pueda ver lo que se captura, debes proporcionar una vista previa de lo que el dispositivo de captura de vídeo actualmente está viendo en la interfaz de usuario.

**Importante** Debes iniciar la vista previa de captura en el dispositivo de captura para habilitar el enfoque automático, la exposición automática y el balance de blancos automático.

El control [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) se proporciona para habilitar la vista previa de captura. A continuación muestra código XAML de ejemplo que define el elemento de captura.

[!code-xml[CaptureElement](./code/BasicMediaCaptureWin10/cs/MainPage.xaml#SnippetCaptureElement)]

Los usuarios esperan que la pantalla esta activa mientras están viendo la pantalla de captura de vídeo y que no se desactive debido a la inactividad. Para habilitar esto, debes crear un objeto [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Declara esta variable con definición de ámbito de página para que persista durante la sesión de captura.

[!code-cs[DisplayRequest](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayRequest)]

El siguiente método inicia la vista previa de captura de multimedia. En primer lugar, solicitará que la pantalla permanezca activa llamando a [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) en la [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816). Después, la vista previa se inicia mediante llamadas a [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613).

[!code-cs[StartPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

-   El método [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/br241818) se llama en el objeto **DisplayRequest** para solicitar que el sistema deje la pantalla encendida.

-   La propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de la **CaptureElement** se establece en el objeto de la aplicación **MediaCapture** para definir el origen de la vista previa.

-   La propiedad [**FlowDirection**](https://msdn.microsoft.com/library/windows/apps/br208716) se proporciona por el marco XAML para admitir las interfaces de usuario bidireccional. Establecer la dirección del flujo de la **CaptureElement** a [**FlowDirection.RightToLeft**](https://msdn.microsoft.com/library/windows/apps/br242397) hace que el vídeo de vista previa se voltee horizontalmente. Esto se usa cuando el dispositivo de captura está en el panel frontal del dispositivo de modo que la vista previa está en la dirección correcta desde la perspectiva del usuario.

-   El método [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613) inicia la presentación de la secuencia de vista previa dentro del **CaptureElement**. Si la vista previa se inicia correctamente, la variable **\_isPreviewing** se establece para permitir que otras partes de la aplicación sepan que actualmente la aplicación está en vista previa y se llama al método auxiliar para establecer la rotación de vista previa. Este método se define en la siguiente sección.

## Detectar la pantalla y orientación del dispositivo

Existen varias áreas de una aplicación de captura de multimedia que, cuando se ejecutan en un dispositivo móvil como un teléfono o una tableta, se ven afectadas por la orientación actual del dispositivo. Estas áreas incluyen girar correctamente la secuencia de vista previa de la cámara y codificar correctamente imágenes y vídeos capturados para que, cuando los vea el usuario, estén orientados correctamente.

El término "orientación de pantalla" se refiere a la forma en que el sistema gira la página XAML en el dispositivo para mantenerla la posición vertical para el usuario. "La orientación del dispositivo" se refiere a la orientación del dispositivo en el espacio global y, por lo tanto, la orientación del dispositivo físico de cámara en el espacio global. Ambos tipos de orientación son relevantes en una aplicación de captura de multimedia. Para controlar la orientación de pantalla, declara e inicializa una variable de ámbito de la página para la clase [**DisplayInformation**](https://msdn.microsoft.com/library/windows/apps/dn264258). Declara otra variable de tipo [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) para realizar un seguimiento de la orientación actual de la pantalla. Declara una variable [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) y una variable [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) para realizar un seguimiento de la orientación del dispositivo.

[!code-cs[DisplayInformationAndOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayInformationAndOrientation)]

Los siguientes métodos auxiliares registran y anulan los controladores de eventos para los eventos [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) y [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) e inicializan las variables de seguimiento con la orientación actual. Ten en cuenta que no todos los dispositivos tienen un [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400), por lo que debes comprobarlo antes de registrar el controlador o al intentar obtener la orientación actual.

[!code-cs[RegisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterOrientationEventHandlers)]

[!code-cs[UnregisterOrientationEventHandlers](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterOrientationEventHandlers)]

En el controlador de eventos para el evento **SimpleOrientationSensor.OrientationChanged**, actualiza la variable de orientación del dispositivo con la orientación actual. No debes actualizar la orientación si el dispositivo está apuntando hacia arriba o hacia abajo.

[!code-cs[SimpleOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSimpleOrientationChanged)]

En el controlador de eventos para el evento **DisplayInformation.OrientationChanged**, actualiza la variable de orientación de la pantalla con la orientación actual. Si se está mostrando la vista previa de vídeo del dispositivo de captura, actualiza la rotación de la secuencia de vídeo de vista previa. El método auxiliar **SetPreviewRotationAsync** se describe en la siguiente sección.

[!code-cs[DisplayOrientationChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDisplayOrientationChanged)]

## Establecer los medios de rotación de la vista previa de captura

Los usuarios esperan que los controles de interfaz de usuario giren cuando cambia la orientación de sus dispositivos móviles, de modo que el texto de la interfaz de usuario se alinee verticalmente y legible. Para el control **CaptureElement**, sin embargo, los usuarios normalmente no desean que la orientación de la vista previa del vídeo gire cuando el dispositivo lo haga. Para proporcionar la experiencia de usuario esperada, debes girar la secuencia de vista previa para que coincida con la orientación del dispositivo.

La orientación de la secuencia de vista previa se debe expresar en grados. El siguiente método auxiliar que convierte los valores de enumeración [**DisplayOrientations**](https://msdn.microsoft.com/library/windows/apps/br226142) en grados.

[!code-cs[ConvertDisplayOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDisplayOrientationToDegrees)]

Y este método auxiliar que convierte un valor de enumeración [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399), que se usa en el [**SimpleOrientationSensor**](https://msdn.microsoft.com/library/windows/apps/br206400) para expresar la rotación del dispositivo, en grados.

[!code-cs[ConvertDeviceOrientationToDegrees](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertDeviceOrientationToDegrees)]

En un nivel bajo, el marco de Microsoft Media Foundation en realidad es el que realiza la rotación de una secuencia. La rotación se especifica mediante el atributo [MF\_MT\_VIDEO\_ROTATION](https://msdn.microsoft.com/library/windows/desktop/hh162880). Dado que se trata de una aplicación de Windows Runtime, la rotación se especifica usando el GUID para el atributo, en lugar de usar nombre de atributo. Definir el siguiente GUID para identificar el atributo de rotación del vídeo.

[!code-cs[RotationKey](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRotationKey)]

El siguiente método establece el rotación de la secuencia de vista previa. El método [**GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) de la captura multimedia [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) devuelve un conjunto de propiedades que se compone de pares clave/valor. [
              **MediaStreamType.VideoPreview**
            ](https://msdn.microsoft.com/library/windows/apps/br226640) se especifica para indicar que deseamos las propiedades de la secuencia de vista previa de vídeo, en lugar de la secuencia de grabación de vídeo o la secuencia de audio. El conjunto de propiedades es una interfaz de propósito general para establecer las propiedades de la secuencia, pero para esta tarea de rotación de vídeo GUID definido anteriormente se agrega como la clave de propiedad y la orientación deseada de la secuencia de vídeo, en grados, se especifica como el valor. [
              **SetEncodingPropertiesAsync**
            ](https://msdn.microsoft.com/library/windows/apps/dn297781) actualiza las propiedades de codificación con los nuevos valores. Una vez más, **MediaStreamType.VideoPreview** se especifica para indicar que las propiedades se establecen para la secuencia de vista previa de vídeo.

[!code-cs[SetPreviewRotation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetPreviewRotation)]

-   Para dispositivos con cámaras externos, el usuario no espera a que la secuencia de la cámara se gire cuando el dispositivo lo haga.

-   Si se está reflejando la vista previa de una cámara en el panel frontal, el sentido de rotación se debe invertir para que coincida con la rotación del dispositivo.

-   Algunos dispositivos, por lo general, teléfonos, soportan la configuración de [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) a un valor de orientación como [**DisplayOrientations.Landscape**](https://msdn.microsoft.com/library/windows/apps/br226142) para forzar a la pantalla a girar con el dispositivo. Debes establecer este valor porque proporciona una buena experiencia en los dispositivos que lo admiten, pero aún así debes implementar el patrón anterior en tu aplicación para admitir dispositivos que no son compatibles con las preferencias de rotación automática.

-   En versiones anteriores, el método [**SetPreviewRotation**](https://msdn.microsoft.com/library/windows/apps/br226611) era la única manera de girar la secuencia de vista previa. Este método está presente en la superficie de API para admitir aplicaciones existentes, pero este método es ineficiente y no debe usarse para nuevas aplicaciones.

## Capturar una foto

El siguiente método captura una fotografía mediante el método [**CapturePhotoToStreamAsync**](https://msdn.microsoft.com/library/windows/apps/hh700840) y se pasa las propiedades de codificación solicitadas y a un objeto [**InMemoryRandomAccessStream**](https://msdn.microsoft.com/library/windows/apps/br241720) que contenga el resultado de la operación de captura. La clase [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) proporciona métodos auxiliares, como [**CreateJpeg**](https://msdn.microsoft.com/library/windows/apps/hh700994), para generar la codificación de las propiedades de los tipos de archivo compatibles con captura de multimedia.

[!code-cs[TakePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetTakePhotoAsync)]

Antes de guardar la foto en un archivo, debes determinar la orientación correcta de la foto. El objeto **MediaCapture** no sabe sobre la orientación del dispositivo y por lo tanto, codifica los datos de la foto como si el dispositivo de captura estuviera en su orientación predeterminada. Esto puede resultar en una experiencia de usuario negativa cuando el usuario está viendo la foto, ya que la foto se orientará incorrectamente. Los siguientes métodos auxiliares determinan la orientación de la fotografía correcta y, después, guardan el archivo con la orientación correcta.

El método **GetCameraOrientation** auxiliar comienza con la orientación actual del dispositivo y, a continuación, gira ese valor según la orientación nativa del dispositivo y la ubicación de la cámara en el dispositivo. Si la cámara está montada en el panel frontal del dispositivo, como se indica en este ejemplo por medio de la variable **\_mirroringPreview**, la orientación de la cámara se debe invertir.

[!code-cs[GetCameraOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetCameraOrientation)]

El siguiente método auxiliar simplemente convierte los valores de los valores [**SimpleOrientation**](https://msdn.microsoft.com/library/windows/apps/br206399) de enumeración usado por el sensor de orientación a su equivalente en el valor [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) usado por el codificador de mapa de bits que se usará para guardar el archivo.

[!code-cs[ConvertOrientationToPhotoOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetConvertOrientationToPhotoOrientation)]

Por último, la foto puede codificarse y guardarse Crea un objeto [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/br226176) de la secuencia de entrada que contenga los datos de la foto. Crea un nuevo [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) y ábrelo para lectura y escritura. Crea un objeto [**BitmapEncoder**](https://msdn.microsoft.com/library/windows/apps/br226206), pasando al archivo de salida y al descodificador que contiene los datos de imagen. Crea un nuevo [**BitmapPropertySet**](https://msdn.microsoft.com/library/windows/apps/hh974338) y agrega una nueva propiedad. La clave para la propiedad, "System.Photo.Orientation" especifica que la propiedad representa la orientación de la foto. El valor es igual al valor [**PhotoOrientation**](https://msdn.microsoft.com/library/windows/apps/hh965476) calculado anteriormente. Llama a [**SetPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br226252) para actualizar las propiedades en el codificador y, a continuación, llama a [**FlushAsync**](https://msdn.microsoft.com/library/dn237883) para escribir la foto en el archivo de almacenamiento.

[!code-cs[ReencodeAndSavePhotoAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetReencodeAndSavePhotoAsync)]

-   Establecer la propiedad de mapa de bits "System.Photo.Orientation" codifica la orientación de la foto en los metadatos del archivo. No hace que los datos de imagen real se deban codificar de forma diferente. Para más información sobre la incorporación de metadatos en los archivos de imagen, consulta [Metadatos de imagen](image-metadata.md).

-   Para más información sobre cómo trabajar con las imágenes, incluidas su codificación y descodificación, consulta [Imágenes](imaging.md).

## Capturar un vídeo

Para iniciar la captura de vídeo, crea un archivo de almacenamiento en el que se registrará el vídeo. Después, crea el [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) que la **MediaCapture** usará para codificar el vídeo en el archivo. La clase **MediaEncodingProfile** proporciona métodos, como [**CreateMp4**](https://msdn.microsoft.com/library/windows/apps/hh701078), que crea los perfiles de codificación para los formatos de vídeo compatibles. Usa los métodos auxiliares que se han descrito anteriormente para obtener la rotación correcta para el vídeo, en grados. A diferencia del escenario de fotos, **MediaCapture** codifica la información de rotación del vídeo en la secuencia. Agrega la información de rotación en el perfil de codificación agregándola a la colección [**VideoEncodingProperties.Properties**](https://msdn.microsoft.com/library/windows/apps/hh701254). El GUID definido previamente para el rotación del vídeo se usa como clave y la rotación, en grados, es el valor. Por último, se llama a [**MediaCapture.StartRecordToStorageFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh700863), especificando las propiedades de codificación y el archivo de salida para empezar a grabar.

[!code-cs[StartRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStartRecordingAsync)]

Para detener la grabación, basta con llamar a [**MediaCapture.StopRecordAsync**](https://msdn.microsoft.com/library/windows/apps/br226623).

[!code-cs[StopRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopRecordingAsync)]

Un controlador para el evento [**MediaCapture.RecordLimitationExceeded**](https://msdn.microsoft.com/library/windows/apps/hh973312) se registró al inicializar el objeto **MediaCapture**. En el controlador llama al método **StopRecordingAsync** para detener la grabación de vídeo.

[!code-cs[RecordLimitationExceeded](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRecordLimitationExceeded)]

## Pausar y reanudar la captura de vídeo

Es posible que desees pausar y reanudar una captura de vídeo en curso en algunos escenarios, en lugar de detener la captura y comenzar una nueva. Para pausar la grabación llama al [**PauseRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858102). Si especificas [**MediaCapturePauseBehavior.RetainHardwareResources**](https://msdn.microsoft.com/library/windows/apps/dn926686), entonces, en dispositivos que no admiten la captura simultánea de fotos y vídeo, la aplicación no podrá capturar fotos mientras se pausa el vídeo. Para obtener información acerca de cómo determinar si un dispositivo admite la captura simultánea de fotos y vídeo, consulta [Perfiles de cámara](camera-profiles.md).

[!code-cs[PauseRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPauseRecordingAsync)]

Para reanudar la captura previa de vídeo en pausa, llama a [**ResumeRecordAsync**](https://msdn.microsoft.com/library/windows/apps/dn858103).

[!code-cs[ResumeRecordingAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResumeRecordingAsync)]

## Limpiar los recursos de captura

Es muy importante que apagues y liberes los recursos de captura de multimedia correctamente. Si no lo haces puedes causar un comportamiento inesperado del dispositivo de la cámara después de que la aplicación cierra, lo que resultará en una experiencia negativa del usuario para tu aplicación. Las siguientes secciones recorren los distintos pasos que debes seguir para apagar la cámara.

### Apagar la vista previa de captura

Para apagar la vista previa de captura, primero llama a [**MediaCapture.StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622). Establece la propiedad [**Source**](https://msdn.microsoft.com/library/windows/apps/br227419) de tu [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926) en null. A continuación, llama a la [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/br241819) en tu variable [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/br241816) para permitir que el sistema apague la pantalla cuando sea necesario.

[!code-cs[StopPreviewAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetStopPreviewAsync)]

-   No tendrás acceso a la interfaz de usuario desde un subproceso sin interfaz de usuario, por lo tanto, la configuración de la propiedad **MediaElement.Source** y la llamada a **RequestRelease** deben realizarse con el método [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317) para que las llamadas se ejecuten en el subproceso de interfaz de usuario.

### Apagar y eliminar el objeto MediaCapture

Antes de eliminar el objeto **MediaCapture**, debes detener cualquier grabación que esté en curso y detener la secuencia de vista previa, llamando a los métodos auxiliares definidos anteriormente. Una vez que hecho esto, quita los controladores de eventos registrados con el **MediaCapture** y después llama a [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858) para liberar los recursos asociados con el objeto y establecer la variable **MediaCapture** en null.

[!code-cs[CleanupCameraAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Debes llamar a este método para apagar y eliminar el objeto **MediaCapture** desde dentro del controlador para el evento [**MediaCapture.Failed**](https://msdn.microsoft.com/library/windows/apps/br226593).

[!code-cs[CaptureFailed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCaptureFailed)]

### Controlar eventos de navegación de página y ciclo de vida de aplicación

Los eventos de ciclo de vida de la aplicación le brindan a tu aplicación una oportunidad para inicializar y liberar los recursos. Esto es especialmente importante con el evento **Application.Suspending**, donde es fundamental que la aplicación deseche correctamente los recursos de captura de multimedia.

Puedes registrar controladores para los eventos de ciclo de vida de aplicación en el constructor de la página.

[!code-cs[RegisterAppLifetimeEvents](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterAppLifetimeEvents)]

En el controlador para el evento **Application.Suspending**, se deben anular el registro de los controladores para los eventos de orientación de pantalla y del dispositivo y apagar el objeto **MediaCapture**. El evento [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720) no registrado aquí se necesita para otra tarea relacionada con el ciclo de vida de aplicación que se describe más adelante en este artículo.

**Precaución** Debes solicitar un aplazamiento de suspensión llamando a [**SuspendingOperation.GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) al principio del controlador de evento de suspensión. Esto solicita que el sistema esperare a que la operación está completa antes de finalizar tu aplicación. Esto es necesario porque las operaciones **MediaCapture** de apagado son asincrónicas, por lo que el controlador **Application.Suspending** de eventos puede completarse antes de que la cámara se apague correctamente. Una vez completadas tus llamadas asincrónicas esperadas, debes liberar el aplazamiento mediante una llamada a [**SuspendingDeferral.Complete**](https://msdn.microsoft.com/library/windows/apps/br224685).

[!code-cs[Suspensión](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSuspending)]

En el controlador para el evento **Application.Resuming**, debes registrar los controladores para los eventos de orientación de pantalla y del dispositivo, registrar el evento **SystemMediaTransportControls.PropertyChanged** e inicializar el objeto **MediaCapture**.

[!code-cs[Reanudar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetResuming)]

El evento [**OnNavigatedTo**](https://msdn.microsoft.com/library/windows/apps/br227508) proporciona una oportunidad para registrar los controladores para los eventos de orientación de pantalla y del dispositivo inicialmente e inicializar el objeto **MediaCapture**.

[!code-cs[OnNavigatedTo](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatedTo)]

Si la aplicación tiene varias páginas, debes limpiar los objetos de capturas multimedia en el controlador de eventos [**OnNavigatingFrom**](https://msdn.microsoft.com/library/windows/apps/br227509).

[!code-cs[OnNavigatingFrom](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetOnNavigatingFrom)]

Para que la aplicación se comporte correctamente en dispositivos que admiten varias ventanas simultáneas, debes responder cuándo se minimiza o restaura la aplicación. Para ello, debes administrar el evento [**SystemMediaTransportControls.PropertyChanged**](https://msdn.microsoft.com/library/windows/apps/dn278720). Inicializar una variable de miembro para almacenar una referencia al objeto [**SystemMediaTransportControls**](https://msdn.microsoft.com/library/windows/apps/dn278677) de la aplicación.

[!code-cs[DeclareSystemMediaTransportControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareSystemMediaTransportControls)]

El código para registrar y anular el registro del evento **PropertyChanged** debe agregarse a los eventos de ciclo de vida de la aplicación, como se muestra en los ejemplos anteriores. En el controlador para el evento, comprueba si el cambio de propiedad que desencadenó el evento fue la propiedad [**SystemMediaTransportControlsProperty.SoundLevel**](https://msdn.microsoft.com/library/windows/apps/dn278721). Si esto se debió a un cambio de la propiedad, comprueba el valor de la propiedad. Si el valor es [**SoundLevel.Muted**](https://msdn.microsoft.com/library/windows/apps/hh700852), se minimiza la aplicación y se deben limpiar los recursos de captura multimedia correctamente. De lo contrario, el evento indica que se ha restaurado la ventana de aplicación y debes reinicializar recursos de captura multimedia. Se debe acceder a la propiedad **SoundLevel** en el subproceso de interfaz de usuario, por lo que el código de este método se encapsula en una llamada a [**Dispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317).

[!code-cs[SystemMediaControlsPropertyChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSystemMediaControlsPropertyChanged)]

## Otras consideraciones de interfaz de usuario para la captura multimedia

### Establecer las preferencias de rotación automática

Como se mencionó en la sección anterior sobre la rotación de la secuencia de vista previa, algunos dispositivos admiten la configuración de [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/dn264259) para evitar que la página, incluido el [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) que muestra la vista previa, giren a medida que el dispositivo también gire. Esto proporciona una buena experiencia del usuario en dispositivos que lo admitan. Establece este valor cuando se inicia la aplicación o al empezar a mostrar la vista previa. Ten en cuenta que debes implementar un control de giro de vista previa para dispositivos que no admitan las preferencias de rotación automática.

[!code-cs[SetAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetAutoRotationPreferences)]

### Controlar la orientación del elemento de interfaz de usuario

Por lo general, los usuarios esperan que los elementos de interfaz de usuario en una aplicación de cámara, como los botones para iniciar la captura de foto o un vídeo, giren junto con la vista previa de vídeo. El siguiente método muestra el uso de los métodos auxiliares de orientación definida previamente para orientar correctamente los botones en la interfaz de usuario de cámara. Debes llamar a este método desde los controladores de evento [**DisplayInformation.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/dn264268) y [**SimpleOrientationSensor.OrientationChanged**](https://msdn.microsoft.com/library/windows/apps/br206407) y cuando se inicie la aplicación por primera vez. La implementación puede variar en función de la interfaz de usuario de la aplicación.

[!code-cs[UpdateButtonOrientation](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateButtonOrientation)]

Cuando se cierra la aplicación o si navegas a una página que no está relacionada con la captura de multimedia, establece la preferencia de rotación automática en [**None**](https://msdn.microsoft.com/library/windows/apps/br226142) para permitir que la interfaz de usuario gire normalmente.

[!code-cs[RevertAutoRotationPreferences](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRevertAutoRotationPreferences)]

### Compatibilidad con captura simultánea de fotos y vídeo

La API [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/br226738) te permite capturar fotos y vídeos simultáneamente en los dispositivos que lo admitan. Por razones de brevedad, este ejemplo usa la propiedad [**ConcurrentRecordAndPhotoSupported**](https://msdn.microsoft.com/library/windows/apps/dn278843) para determinar si se admite la captura de fotos y vídeo simultánea, pero una manera más eficaz y recomendada para hacerlo es usar perfiles de cámara. Para más información, consulta [Perfiles de cámara](camera-profiles.md).

El siguiente método auxiliar actualiza los controles de la aplicación para que coincidan con el estado actual de la captura de la aplicación. Llama a este método cada vez que cambia el estado de la captura de la aplicación, como cuando se inicia o se detiene la captura de vídeo.

[!code-cs[UpdateCaptureControls](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUpdateCaptureControls)]

### Admitir características de interfaz de usuario específicas para móviles

Todos los códigos que se muestran en este artículo funcionarán en una aplicación universal de Windows. Con unas pocas líneas de código adicionales, puedes aprovechar características especiales de interfaz de usuario que solo están presentes en los dispositivos móviles. Para usar estas características, debes agregar una referencia al SDK de extensión móvil de Microsoft para la plataforma de aplicación universal al proyecto.

**Para agregar una referencia al SDK de extensión móvil para admitir el soporte del botón de cámara hardware**

1.  En el **Explorador de soluciones**, haz clic con el botón secundario en **Referencias** y selecciona **Agregar referencia...**

2.  Expande el nodo **Windows Universal** y selecciona **extensiones**.

3.  Haz clic en la casilla junto al **SDK de extensión móvil de Microsoft para la plataforma de aplicación universal**.

Los dispositivos móviles tienen un control [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) que proporciona al usuario información del estado del dispositivo. Este control ocupa espacio en la pantalla que puede interferir con la interfaz de usuario de captura de multimedia. Puedes ocultar la barra de estado llamando a [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339), pero debes hacer esta llamada desde dentro de un bloque condicional cuando usas el método [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) para determinar si la API está disponible. Este método solo devolverá true en dispositivos móviles que admitan la barra de estado. Debes ocultar la barra de estado cuando se inicie la aplicación o al empezar a obtener una vista previa de la cámara.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Cuando se cierra la aplicación o cuando el usuario abandona la página de captura multimedia de la aplicación, haces que el control vuelva a ser visible.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

Algunos dispositivos móviles tienen un botón dedicado a la cámara de hardware, que algunos usuarios prefieren por encima de un control en pantalla. Para recibir notificaciones cuando se presiona el botón de hardware de cámara, registra un controlador para el evento [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805). Como esta API está disponible solo en dispositivos móviles, debes volver a usar el **IsTypePresent** para asegurarte de que la API es compatible con el dispositivo actual antes de intentar acceder a este.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

En el controlador para el evento **CameraPressed**, puedes iniciar una captura de fotos.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Cuando se cierra la aplicación o el usuario abandona la página de captura multimedia de la aplicación, anula el registro del controlador de botón de hardware.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

**Nota** Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Temas relacionados

* [Perfiles de cámara](camera-profiles.md)
* [Captura de fotos de alto rango dinámico (HDR)](high-dynamic-range-hdr-photo-capture.md)
* [Controles de dispositivos de captura para captura de fotos y vídeos](capture-device-controls-for-photo-and-video-capture.md)
* [Controles de dispositivo de captura para captura de vídeos](capture-device-controls-for-video-capture.md)
* [Efectos para captura de vídeos](effects-for-video-capture.md)
* [Análisis de la escena para la captura multimedia](scene-analysis-for-media-capture.md)
* [Secuencia de fotos variable](variable-photo-sequence.md)
* [Obtener un marco de vista previa](get-a-preview-frame.md)
* [Muestra de CameraStarterKit](http://go.microsoft.com/fwlink/?LinkId=619479)



<!--HONumber=Jun16_HO4-->


