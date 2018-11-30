---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: En este artículo se muestra cómo usar MediaFrameReader con MediaCapture para obtener fotogramas multimedia de uno o más orígenes disponibles, lo que incluye cámaras a color, de profundidad y de infrarrojos, dispositivos de audio o incluso orígenes de fotogramas personalizados, como los que producen fotogramas de seguimiento estructurales.
title: Procesar fotogramas multimedia con MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 9940367054ae8771355012492434e12aa97d43ad
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "8202282"
---
# <a name="process-media-frames-with-mediaframereader"></a>Procesar fotogramas multimedia con MediaFrameReader

En este artículo se muestra cómo usar [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) con [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture) para obtener fotogramas multimedia de uno o más orígenes disponibles, lo que incluye cámaras a color, de profundidad y de infrarrojos, dispositivos de audio o incluso orígenes de fotogramas personalizados, como los que producen fotogramas de seguimiento estructurales. Esta característica se diseñó para que la usen las aplicaciones que realizan procesamiento en tiempo real de fotogramas multimedia, como las aplicaciones de realidad aumentada y las de cámara con reconocimiento de profundidad.

Si, simplemente, estás interesado en capturar vídeo o fotos, como una aplicación típica de fotografía, es probable que quieras usar una de las otras técnicas de captura que admite [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture). Para obtener una lista de las técnicas de captura de elementos multimedia disponibles y artículos donde se muestra cómo usarlos, consulta [**Cámara**](camera.md).

> [!NOTE] 
> Las características descritas en este artículo solo están disponibles a partir de Windows 10, versión 1607.

> [!NOTE] 
> Hay una muestra de aplicación universal de Windows que muestra el uso de **MediaFrameReader** para mostrar fotogramas de distintos orígenes de fotogramas, lo que incluye cámaras a color, de profundidad y de infrarrojos. Para obtener más información, consulta [Camera frames sample (Muestra de fotogramas de cámara)](http://go.microsoft.com/fwlink/?LinkId=823230).

> [!NOTE] 
> En Windows 10, versión 1803, se introdujo un nuevo conjunto de API para usar **MediaFrameReader** con datos de audio. Para obtener más información, consulta [Procesar tramas de audio con MediaFrameReader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Configurar tu proyecto
Al igual que con cualquier aplicación que use **MediaCapture**, debes declarar que tu aplicación usa la funcionalidad *cámara web* antes de intentar acceder a cualquier dispositivo de cámara. Si la aplicación captura desde un dispositivo de audio, también debes declarar la funcionalidad *micrófono* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Selecciona la pestaña **Funcionalidades**.
3.  Selecciona las casillas **Cámara web** y **Micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y de **Biblioteca de vídeos**.

En el código de ejemplo de este artículo se usan las API de los siguientes espacios de nombres, además de las que se incluyen con la plantilla de proyecto predeterminada.

[!code-cs[FramesUsing](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFramesUsing)]

## <a name="select-frame-sources-and-frame-source-groups"></a>Seleccionar orígenes de fotogramas y grupos de orígenes de fotogramas
Muchas aplicaciones que procesan los fotogramas multimedia necesitan obtener fotogramas de distintos orígenes al mismo tiempo, como cámaras de profundidad y de color de un dispositivo. El objeto [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) representa un conjunto de orígenes de fotogramas multimedia que se pueden usar simultáneamente. Llama al método estático [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) para obtener una lista de todos los grupos de orígenes de fotogramas que admite el dispositivo actual.

[!code-cs[FindAllAsync](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFindAllAsync)]

También puedes crear un [**DeviceWatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.Devices.Enumeration.DeviceWatcher) usando [**DeviceInformation.CreateWatcher**](https://msdn.microsoft.com/library/windows/apps/br225427) como el valor devuelto de [**MediaFrameSourceGroup.GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.GetDeviceSelector) para recibir notificaciones cuando el origen de fotogramas disponible se agrupa en el dispositivo cambios, como una cámara externa cuando está enchufada. Para obtener más información, consulta [**Enumerar dispositivos**](https://msdn.microsoft.com/windows/uwp/devices-sensors/enumerate-devices).

Una clase [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) tiene una colección de objetos [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) que describen los orígenes de fotogramas que se incluyen en el grupo. Después de recuperar los grupos de origen de fotogramas disponibles en el dispositivo, puedes seleccionar el grupo que expone los orígenes de fotogramas que te interesan.

El siguiente ejemplo muestra la forma más sencilla de seleccionar un grupo de orígenes de fotogramas. Este código, simplemente, recorre todos los grupos disponibles y, a continuación, recorre cada elemento de la colección [**SourceInfos**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.SourceInfos). Cada objeto **MediaFrameSourceInfo** se comprueba para ver si admite las características que nos interesan. En este caso, se comprueba el valor [**VideoPreview**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaStreamType) de la propiedad [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.MediaStreamType), lo que significa que el dispositivo proporciona una secuencia de vista previa de vídeo, y se comprueba el valor [**Color**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceKind) de la propiedad [**SourceKind**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo.SourceKind), que indica que el origen proporciona fotogramas de color.

[!code-cs[SimpleSelect](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSimpleSelect)]

Este método para identificar el grupo de orígenes de fotogramas y los orígenes de fotogramas deseados funciona con casos sencillos, pero si quieres seleccionar orígenes de fotogramas por criterios más complejos, puede volverse más complicado. Otro método es usar la sintaxis de Linq y objetos anónimos para realizar la selección. En el siguiente ejemplo se usa el método de extensión **Select** para transformar los objetos **MediaFrameSourceGroup** de la lista *frameSourceGroups* en un objeto anónimo con dos campos: *sourceGroup*, que representa el grupo en sí mismo, y *colorSourceInfo*, que representa el origen del fotograma de color en el grupo. El campo *colorSourceInfo* se establece en el resultado del elemento **FirstOrDefault**, que selecciona el primer objeto para el que se resuelve el predicado proporcionado en true. En este caso, el predicado es true si el tipo de secuencia es **VideoPreview**, el tipo de origen es **Color** y la cámara está en el panel frontal del dispositivo.

De la lista de objetos anónimos que devuelve la consulta que se describió anteriormente, el método de extensión **Where** se usa para seleccionar solo los objetos en los que el campo *colorSourceInfo* no es nulo. Por último, se llama al objeto **FirstOrDefault** para seleccionar el primer elemento de la lista.

Ahora puedes usar los campos del objeto seleccionado para obtener referencias de los objetos **MediaFrameSourceGroup** y **MediaFrameSourceInfo** que representan la cámara a color. Se usarán más tarde para inicializar el objeto **MediaCapture** y crear una clase **MediaFrameReader** para el origen seleccionado. Por último, debes comprobar si el grupo de orígenes es nulo, lo que significa que el dispositivo actual no tiene los orígenes de captura solicitados.

[!code-cs[SelectColor](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColor)]

En el siguiente ejemplo se usa una técnica similar, como la que se describió anteriormente, para seleccionar un grupo de orígenes que contiene cámaras a color, de profundidad y de infrarrojos.

[!code-cs[ColorInfraredDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetColorInfraredDepth)]

> [!NOTE]
> A partir de Windows 10, versión 1803, puedes usar la clase [**MediaCaptureVideoProfile**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) para seleccionar un origen de marco multimedia con un conjunto de funcionalidades deseadas. Para obtener más información, consulta la sección **Usar perfiles de vídeo para seleccionar un origen de marco** más adelante en este artículo.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Inicializar el objeto MediaCapture para usar el grupo de orígenes de fotogramas seleccionado
El siguiente paso es inicializar el objeto **MediaCapture** para usar el grupo de orígenes de fotogramas seleccionado en el paso anterior.

El objeto **MediaCapture** se usa normalmente desde varias ubicaciones dentro de la aplicación, por lo que debes declarar una variable de miembro de clase para contenerlo.

[!code-cs[DeclareMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Para crear una instancia del objeto **MediaCapture**, llama al constructor. A continuación, crea un objeto [**MediaCaptureSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSettings) que se usará para inicializar el objeto **MediaCapture**. En este ejemplo, se usan los siguientes valores:

* [**SourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SourceGroup): indica al sistema qué grupo de orígenes usarás para obtener fotogramas. Recuerda que el grupo de orígenes define un conjunto de orígenes de fotogramas multimedia que se pueden usar simultáneamente.
* [**SharingMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.SharingMode): indica al sistema si necesitas un control exclusivo de los dispositivos de origen de captura. Si lo estableces en [**ExclusiveControl**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), significa que puedes cambiar la configuración del dispositivo de captura, como el formato de los fotogramas que produce, pero también significa que si otra aplicación ya tiene un control exclusivo, se producirá un error en la aplicación al intentar inicializar el dispositivo de captura multimedia. Si lo estableces en [**SharedReadOnly**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureSharingMode), puede recibir fotogramas de los orígenes de fotogramas incluso si otra aplicación los usa, pero no puedes cambiar la configuración de los dispositivos.
* [**MemoryPreference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.MemoryPreference): si especificas [**CPU**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), el sistema usará la memoria de la CPU que garantiza que, cuando lleguen fotogramas, estarán disponibles como objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap). Si especificas [**Auto**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureMemoryPreference), el sistema elegirá dinámicamente la ubicación de memoria óptima para almacenar los fotogramas. Si el sistema elige usar la memoria de la GPU, los fotogramas multimedia llegarán como un objeto [**IDirect3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) y no como un objeto **SoftwareBitmap**.
* [**StreamingCaptureMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCaptureInitializationSettings.StreamingCaptureMode): establécelo en [**Video**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.StreamingCaptureMode) para indicar que no es necesario transmitir el audio.

Llama al método [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) para inicializar la clase **MediaCapture** con la configuración que quieras. Asegúrate de llamar a este método dentro de un bloque *try* por si se produce un error de inicialización.

[!code-cs[InitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMediaCapture)]

## <a name="set-the-preferred-format-for-the-frame-source"></a>Establecer el formato preferido en el origen de fotogramas
Para establecer el formato preferido en un origen de fotogramas, debes obtener un objeto [**MediaFrameSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource) que represente el origen. Para obtener el objeto, accede al diccionario de la propiedad [**Frames**](https://msdn.microsoft.com/library/windows/apps/Windows.Phone.Media.Capture.CameraCaptureSequence.Frames) del objeto **MediaCapture** y especifica el identificador del origen de fotogramas que quieres usar. Por este motivo guardamos el objeto [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) mientras seleccionábamos un grupo de orígenes de fotogramas.

La propiedad [**MediaFrameSource.SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats) contiene una lista de objetos [**MediaFrameFormat**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameFormat) que describen los formatos admitidos para el origen de fotogramas. Usa el método de extensión de Linq **Where** para seleccionar un formato basado en las propiedades que quieras. En este ejemplo, se selecciona un formato que tiene un ancho de 1080 píxeles y puede suministrar fotogramas en un formato RGB de 32 bits. El método de extensión **FirstOrDefault** selecciona la primera entrada de la lista. Si el formato seleccionado es nulo, el origen de fotogramas no admitirá el formato solicitado. Si se admite el formato, puedes solicitar que el origen use este formato llamando al objeto [**SetFormatAsync**](https://msdn.microsoft.com/library/windows/apps/).

[!code-cs[GetPreferredFormat](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetPreferredFormat)]

## <a name="create-a-frame-reader-for-the-frame-source"></a>Crear un lector de fotogramas para el origen de fotogramas
Para recibir fotogramas de un origen de fotogramas multimedia, usa una clase [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader).

[!code-cs[DeclareMediaFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareMediaFrameReader)]

Para crear una instancia del lector de fotogramas, llama al método [**CreateFrameReaderAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.MediaCapture.CreateFrameReaderAsync) en el objeto **MediaCapture** inicializado. El primer argumento de este método es el origen de fotogramas desde el que quieres recibir fotogramas. Puedes crear un lector de fotogramas distinto para cada origen de fotogramas que quieras usar. El segundo argumento indica al sistema el formato de salida en el que quieres que lleguen los fotogramas. Esto puede ahorrarte tener que realizar tus propias conversiones a fotogramas a medida que llegan. Ten en cuenta que, si especificas un formato que no es compatible con el origen de fotogramas, se generará una excepción, así que asegúrate de que el valor esté en la colección [**SupportedFormats**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSource.SupportedFormats).  

Después de crear el lector de fotogramas, registra un controlador para el evento [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) que se genera siempre que un nuevo fotograma está disponible desde el origen.

Indica al sistema que comience a leer fotogramas desde el origen con una llamada al método [**StartAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StartAsync).

[!code-cs[CreateFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCreateFrameReader)]

## <a name="handle-the-frame-arrived-event"></a>Controlar el evento de llegada de fotogramas
El evento [**MediaFrameReader.FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) se genera siempre que un nuevo fotograma esté disponible. Puedes elegir procesar cada fotograma que llegue o usar solo fotogramas cuando los necesites. Ya que el lector de fotogramas genera el evento en su propio subproceso, es posible que tengas que implementar una lógica de sincronización para asegurarte de que no intentas acceder a los mismos datos desde varios subprocesos. En esta sección se muestra cómo sincronizar dibujando fotogramas de color en un control de imagen de una página XAML. Este escenario trata la restricción de sincronización adicional que necesita que todas las actualizaciones de los controles XAML se realicen en el subproceso de la interfaz de usuario.

El primer paso para mostrar fotogramas en XAML es crear un control de imagen. 

[!code-xml[ImageElementXAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetImageElementXAML)]

En la página de código subyacente, declara una variable de miembro de clase de tipo **SoftwareBitmap** que se usará como un búfer de reserva al que se copiarán todas las imágenes entrantes. Ten en cuenta que no se copian los datos de imagen, solo las referencias de objeto. Además, declara un valor booleano para comprobar si la operación de interfaz de usuario todavía se está ejecutando.

[!code-cs[DeclareBackBuffer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetDeclareBackBuffer)]

Ya que los fotogramas llegarán como objetos **SoftwareBitmap**, debes crear un objeto [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) que te permita usar un objeto **SoftwareBitmap** como el origen de un **Control** XAML. Debes establecer el origen de imagen en algún lugar del código antes de iniciar el lector de fotogramas.

[!code-cs[ImageElementSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetImageElementSource)]

Ahora es el momento de implementar el controlador de eventos **FrameArrived**. Cuando se llama al controlador, el parámetro *sender* contiene una referencia al objeto **MediaFrameReader** que generó el evento. Llama al método [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame) en este objeto para intentar obtener el fotograma más reciente. Como su nombre indica, es posible que el método **TryAcquireLatestFrame** no consiga devolver correctamente un fotograma. Por lo tanto, cuando accedes a las propiedades VideoMediaFrame y SoftwareBitmap, asegúrate de probar que no son nulas. En este ejemplo, el operador condicional nulo ? se usa para acceder a la propiedad **SoftwareBitmap** y, a continuación, se comprueba que el objeto recuperado no sea nulo.

El control **Image** solo puede mostrar imágenes en formato BRGA8 con valores alfa premultiplicados o sin valores alfa. Si el fotograma de llegada no está en ese formato, el método estático [**Convert**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Imaging.SoftwareBitmap.Covert) se usa para convertir el mapa de bits de software al formato correcto.

A continuación, se usa el método [**Interlocked.Exchange**](https://msdn.microsoft.com/library/bb337971) para intercambiar la referencia del mapa de bits de llegada con el mapa de bits del búfer de reserva. Este método intercambia estas referencias en una operación atómica segura para subprocesos. Después del intercambio, se desecha la imagen anterior del búfer de reserva, ubicada ahora en la variable *softwareBitmap*, para limpiar los recursos.

A continuación, se usa la clase [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Core.CoreDispatcher) asociada al elemento **Image** para crear una tarea que se ejecutará en el subproceso de la interfaz de usuario mediante una llamada al método [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/hh750317). Ya que las tareas asincrónicas se realizarán dentro de la tarea, la expresión lambda que se pasa al método **RunAsync** se declara con la palabra clave *async*.

Dentro de la tarea, se comprueba la variable *_taskRunning* para asegurarse de que solo se ejecuta una instancia de la tarea a la vez. Si ya no se está ejecutando la tarea, la variable *_taskRunning* se establece en true para impedir que se ejecute de nuevo la tarea. En un bucle *while*, se llama al método **Interlocked.Exchange** para copiar desde el búfer de reserva a una propiedad temporal **SoftwareBitmap** hasta que la imagen del búfer de reserva sea nula. Cada vez que se rellena el mapa de bits temporal, la propiedad **Source** de la clase **Image** se convierte en una propiedad **SoftwareBitmapSource** y luego se llama al método [**SetBitmapAsync**](https://msdn.microsoft.com/library/windows/apps/dn997856) para establecer el origen de la imagen.

Por último, la variable *_taskRunning* se vuelve a establecer en false para que la tarea se pueda ejecutar de nuevo la próxima vez que se llame al controlador.

> [!NOTE] 
> Si accedes a los objetos [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.SoftwareBitmap) o [**Direct3DSurface**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.VideoMediaFrame.Direct3DSurface) proporcionados por la propiedad [**VideoMediaFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.VideoMediaFrame) de una clase [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), el sistema crea una referencia fuerte a estos objetos, lo que significa que no se eliminarán cuando se llamae a [**Dispose**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference.Close) en la clase **MediaFrameReference** contenedora. Se debe llamar explícitamente al método **Dispose** de **SoftwareBitmap** o **Direct3DSurface** directamente para los objetos que deben eliminarse inmediatamente. De lo contrario, el recolector de elementos no usados al final liberará la memoria de estos objetos, pero no se puede saber cuando ocurrirá, y si el número de superficies o mapas de bits asignados supera la cantidad máxima permitida por el sistema, el nuevo flujo de fotogramas se detendrá. Puedes copiar marcos recuperados con el método [**SoftwareBitmap.Copy**](https://docs.microsoft.com/en-us/uwp/api/windows.graphics.imaging.softwarebitmap.copy), por ejemplo, y después soltar los fotogramas originales para superar esta limitación. Además, si creas el **MediaFrameReader** con [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype, Windows.Graphics.Imaging.BitmapSize outputSize)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) or [CreateFrameReaderAsync(Windows.Media.Capture.Frames.MediaFrameSource inputSource, System.String outputSubtype)](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_) de sobrecarga, los marcos devueltos serán copias de los datos de marcos originales de modo que no se detenga la adquisición de marcos cuando se retengan. 


[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetFrameArrived)]

## <a name="cleanup-resources"></a>Limpiar los recursos
Cuando hayas terminado de leer fotogramas, asegúrate de detener el lector de fotogramas multimedia con una llamada al método [**StopAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.StopAsync). Después, elimina el controlador **FrameArrived** y desecha el objeto **MediaCapture**.

[!code-cs[Cleanup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCleanup)]

Para obtener más información sobre cómo limpiar los objetos de captura multimedia cuando se suspende la aplicación, consulta [**Acceso fácil a la vista previa de cámara**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>La clase auxiliar FrameRenderer
[Camera frames sample (Muestra de fotogramas de cámara)](http://go.microsoft.com/fwlink/?LinkId=823230) universal de Windows proporciona una clase auxiliar que facilita mostrar los fotogramas de orígenes a color, de infrarrojos y en profundidad en la aplicación. Normalmente, querrás hacer más cosas con los datos en profundidad y de infrarrojos que solo mostrarlos en la pantalla, pero esta clase auxiliar es una herramienta útil para demostrar la característica de lector de fotogramas y para depurar tu propia implementación del lector de fotogramas.

La clase auxiliar **FrameRenderer** implementa los métodos siguientes.

* Constructor **FrameRenderer**: el constructor inicializa la clase auxiliar para usar el elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Image) de XAML que pasas para mostrar los fotogramas multimedia.
* **ProcessFrame**: este método muestra un fotograma multimedia, que se representa con una clase [**MediaFrameReference**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReference), en el elemento **Image** que pasaste al constructor. Normalmente, debes llamar a este método desde el controlador de eventos [**FrameArrived**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.FrameArrived) y pasar el fotograma que devuelve el método [**TryAcquireLatestFrame**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader.TryAcquireLatestFrame).
* **ConvertToDisplayableImage**: este método comprueba el formato del fotograma multimedia y, si fuera necesario, lo convierte a un formato que se pueda mostrar. Para las imágenes a color, esto significa asegurarse de que el formato de color sea BGRA8 y que el modo alfa del mapa de bits sea premultiplicado. Para fotogramas de infrarrojos o en profundidad, se procesa cada línea de digitalización para convertir los valores de infrarrojos o en profundidad a un degradado psuedocolor, con la clase **PsuedoColorHelper** que también está incluida en la muestra y se indica a continuación.

> [!NOTE] 
> Para manipular píxeles en imágenes **SoftwareBitmap**, debes acceder a un búfer de memoria nativo. Para ello, debes usar la interfaz COM IMemoryBufferByteAccess incluida en la siguiente lista de códigos y actualizar las propiedades del proyecto para permitir la compilación del código no seguro. Para obtener más información, consulta [Creación de imágenes](imaging.md).

[!code-cs[IMemoryBufferByteAccess](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetIMemoryBufferByteAccess)]

[!code-cs[FrameArrived](./code/Frames_Win10/Frames_Win10/FrameRenderer.cs#SnippetFrameRenderer)]

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Usa MultiSourceMediaFrameReader para obtener fotogramas correlacionados de tiempo de varios orígenes.
A partir de Windows 10, versión 1607, puedes usar [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) para recibir fotogramas correlacionados de tiempo de varios orígenes. Con esta API es más fácil realizar el procesamiento, que requiere fotogramas desde varias fuentes y que se han tomado cercanas en el tiempo, como el uso de la clase [**DepthCorrelatedCoordinateMapper**](https://docs.microsoft.com/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper). Una limitación del uso de este nuevo método es que solo los eventos de llegada de fotogramas se generan a la velocidad de la fuente más lenta de captura. Los fotogramas adicionales de fuentes más rápidas se eliminarán. Además, dado que el sistema espera que los fotogramas lleguen desde orígenes diferentes a diferentes velocidades, no reconoce automáticamente si un origen ha dejado de generar fotogramas por completo. El código de ejemplo en esta sección muestra cómo usar un evento para crear tu propia lógica de tiempo de espera, que se invoca si los fotogramas correlacionados no llegan dentro del límite de tiempo definido por la aplicación.

Los pasos para usar [**MultiSourceMediaFrameReader**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader) son similares a los pasos para usar [**MediaFrameReader**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameReader) descrito anteriormente en este artículo. En este ejemplo se usa una fuente de color y un origen de profundidad. Declara algunas variables de cadena para almacenar los identificadores del origen de fotogramas multimedia que se usan para seleccionar fotogramas de cualquier fuente. A continuación, declara una clase [**ManualResetEventSlim**](https://docs.microsoft.com/dotnet/api/system.threading.manualreseteventslim?view=netframework-4.7), una [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource.aspx)y una [**EventHandler**](https://msdn.microsoft.com/library/system.eventhandler.aspx) que se usarán para implementar la lógica de tiempo de espera para el ejemplo. 

[!code-cs[MultiFrameDeclarations](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameDeclarations)]

Con las técnicas descritas anteriormente en este artículo, realiza una consulta para la clase [**MediaFrameSourceGroup**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup) que incluya los orígenes de profundidad y color necesarios para este escenario de ejemplo. Después de seleccionar el grupo de orígenes de fotogramas deseado obtén la clase [**MediaFrameSourceInfo**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceInfo) para cada origen de fotograma.

[!code-cs[SelectColorAndDepth](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSelectColorAndDepth)]

Crea e inicializa un objeto de **MediaCapture**, pasando el grupo de orígenes de fotogramas seleccionado en la configuración de inicialización.

[!code-cs[MultiFrameInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameInitMediaCapture)]

Después de inicializar el objeto de **MediaCapture**, recupera los objetos de [**MediaFrameSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) para las cámaras de profundidad y de color. Almacena el ID. para cada fuente para poder seleccionar el fotograma de llegada para el origen correspondiente.

[!code-cs[GetColorAndDepthSource](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetColorAndDepthSource)]

Crea e inicializa la clase **MultiSourceMediaFrameReader** llamando a [**CreateMultiSourceFrameReaderAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) y pasando una matriz de orígenes de fotogramas que usará el lector. Registra un controlador de eventos para el evento [**FrameArrived**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived). Este ejemplo crea una instancia de la clase auxiliar **FrameRenderer**, descrita anteriormente en este artículo, para representar fotogramas de un control de **imagen**. Inicia el lector de fotogramas llamando a [**StartAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Registra un controlador de eventos para el evento **CorellationFailed**, explicado anteriormente en el ejemplo. Señalaremos este evento si uno de los orígenes de fotogramas multimedia utilizados deja de producir fotogramas. Por último, llama a [**Task.Run**](https://msdn.microsoft.com/library/hh195051.aspx) para llamar al método auxiliar de tiempo de espera, **NotifyAboutCorrelationFailure**, en un subproceso independiente. La implementación de este método se muestra más adelante en este artículo.

[!code-cs[InitMultiFrameReader](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetInitMultiFrameReader)]

El evento **FrameArrived** se genera siempre que esté disponible un nuevo marco de todos los orígenes de fotogramas multimedia que administran con la clase **MultiSourceMediaFrameReader**. Esto significa que se generará el evento con el ritmo del origen del contenido multimedia más lento. Si un origen produce varios fotogramas en el tiempo en el que una fuente más lenta genera solo uno, estos fotogramas adicionales generadas por el origen rápido se eliminarán. 

Obtén la clase [**MultiSourceMediaFrameReference**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference) asociada al evento mediante una llamada a [**TryAcquireLatestFrame**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame). Obtén la clase **MediaFrameReference** asociada a cada origen de fotogramas multimedia llamando a [**TryGetFrameReferenceBySourceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid) y pasando las cadenas de identificador almacenadas al iniciar el lector de fotogramas.

Llama al método [**Set**](https://msdn.microsoft.com/library/system.threading.manualreseteventslim.set.aspx) del objeto **ManualResetEventSlim** para indicar que los fotogramas han llegado. Comprobaremos este evento en el método **NotifyCorrelationFailure**, que se ejecuta en un subproceso independiente. 

Por último, realiza cualquier tipo de proceso en los fotogramas multimedia correlacionados en el tiempo. Este ejemplo muestra simplemente el fotograma desde el origen de profundidad.

[!code-cs[MultiFrameArrived](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMultiFrameArrived)]

El método auxiliar **NotifyCorrelationFailure** se ejecutó en un subproceso independiente después de iniciar el lector de fotogramas. En este método, comprueba si el evento de fotograma recibido se ha señalado. Recuerda que en el controlador **FrameArrived** establecemos este evento siempre que llega un conjunto de fotogramas correlacionados. Si el evento no ha sido señalado para la aplicación con un periodo de tiempo predefinido (cinco segundos suele ser un valor razonable) y no se ha cancelado la tarea con **CancellationToken**, es probable que uno de los orígenes de fotogramas multimedia haya dejado de leer los fotogramas. En este caso, es normal que quieras apagar el lector de fotogramas, así que genera el evento definido para la aplicación **CorrelationFailed**. En el controlador para este evento, puedes detener el lector de fotogramas y limpiar sus recursos asociados como se ha mostrado anteriormente en este artículo.

[!code-cs[NotifyCorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetNotifyCorrelationFailure)]

[!code-cs[CorrelationFailure](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetCorrelationFailure)]

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Usar el modo de adquisición de fotogramas almacenados en búfer para conservar la secuencia de fotogramas adquiridos
A partir de Windows 10, versión 1709, puedes establecer la propiedad **[AcquisitionMode](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** de un **MediaFrameReader** o **MultiSourceMediaFrameReader** en **Buffered** para conservar la secuencia de fotogramas que se pasa a la aplicación desde el origen del marco.

[!code-cs[SetBufferedFrameAcquisitionMode](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetSetBufferedFrameAcquisitionMode)]

En el modo de adquisición de forma predeterminada, **Realtime**, si se adquieren varios marcos del origen mientras la aplicación aún está controlando el evento **FrameArrived** para un fotograma anterior, el sistema te enviará a tu aplicación el fotograma adquirido más recientemente y colocará fotogramas adicionales que esperan en el búfer. Esto proporciona a la aplicación el marco disponible más reciente en todo momento. Suele ser el modo más útil para aplicaciones de visión de equipo en tiempo real. 

En el modo de adquisición **Buffered**, el sistema conservará todos los fotogramas en el búfer y los proporcionará a tu aplicación a través del evento **FrameArrived** en el orden recibido. Ten en cuenta que en este modo, cuando se llena el búfer del sistema para fotogramas, el sistema detendrá la adquisición de nuevos fotogramas hasta que la aplicación complete el evento **FrameArrived** para fotogramas anteriores, liberando más espacio en el búfer.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Usa MediaSource para mostrar fotogramas en un MediaPlayerElement
A partir de Windows, versión 1709, puedes mostrar fotogramas adquiridos a partir de un **MediaFrameReader** directamente en un control **[MediaPlayerElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** de tu página XAML. Esto se logra usando **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** para crear el objeto **[MediaSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource)** que se puede usar directamente por un **[MediaPlayer](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer)** asociado a un **MediaPlayerElement**. Para obtener información detallada sobre el trabajo con **MediaPlayer** y **MediaPlayerElement**, consulta [Reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

En los siguientes ejemplos de código se ve una implementación sencilla que muestra los fotogramas desde una cámara frontal y posterior al mismo tiempo en una página XAML.

En primer lugar, agrega dos controles **MediaPlayerElement** a la página XAML.

[!code-xml[MediaPlayerElement1XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement1XAML)]

[!code-xml[MediaPlayerElement2XAML](./code/Frames_Win10/Frames_Win10/MainPage.xaml#SnippetMediaPlayerElement2XAML)]

A continuación, con las técnicas que se muestran en las secciones anteriores de este artículo, selecciona un **MediaFrameSourceGroup** que contenga objetos **MediaFrameSourceInfo** para cámaras de color en el panel frontal y posterior. Ten en cuenta que el **MediaPlayer** no convierte automáticamente los fotogramas desde formatos sin color, como datos de profundidad o de infrarrojos, en datos de color. Usar otros tipos de sensores puede producir resultados inesperados. 

[!code-cs[MediaSourceSelectGroup](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceSelectGroup)]

Inicializa el objeto **MediaCapture** para usar el **MediaFrameSourceGroup** seleccionado.

[!code-cs[MediaSourceInitMediaCapture](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceInitMediaCapture)]

Por último, llama a **[MediaSource.CreateFromMediaFrameSource](https://docs.microsoft.com/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** para crear un **MediaSource** para cada origen de marco usando la propiedad **[Id](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** del objeto **MediaFrameSourceInfo** asociado para seleccionar uno de los orígenes de marco de la colección **[FrameSources](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.FrameSources)** del objeto **MediaCapture** del objeto. Inicializa un nuevo objeto **MediaPlayer** y asígnalo a un **MediaPlayerElement** llamando a **[SetMediaPlayer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)**. Después establece la propiedad **[Source](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplayer.Source)** en el objeto **MediaSource** recién creado.

[!code-cs[MediaSourceMediaPlayer](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetMediaSourceMediaPlayer)]

## <a name="use-video-profiles-to-select-a-frame-source"></a>Usar perfiles de vídeo para seleccionar un origen de marco

Un perfil de cámara, representado por un objeto [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694), que representa un conjunto de funcionalidades que un dispositivo de captura concreto proporciona, como velocidades de fotogramas, resoluciones o características avanzadas como la captura HDR. Un dispositivo de captura puede admitir varios perfiles, lo que te permite seleccionar el que está optimizado para tu escenario de captura. A partir de Windows 10, versión 1803, puedes usar la clase **MediaCaptureVideoProfile** para seleccionar un origen de marco multimedia con funcionalidades concretas antes de inicializar el objeto **MediaCapture**. En el siguiente método de ejemplo se busca un perfil de vídeo que admita HDR con una amplia gama de colores (WCG) y devuelve un objeto **MediaCaptureInitializationSettings** que se puede usar para inicializar el valor de **MediaCapture** para usar el perfil y el dispositivo seleccionados.

En primer lugar, llama a [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) para obtener una lista de todos los grupos de origen de fotogramas multimedia disponibles en el dispositivo actual. Recorra cada grupo de origen y llame a [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) para obtener una lista de todos los perfiles de vídeo para el grupo de origen actual que admiten el perfil especificado, en este caso, HDR con fotografía WCG. Si se encuentra un perfil que cumpla con los criterios, crea un nuevo objeto **MediaCaptureInitializationSettings** y establece el **VideoProfile** en el perfil de selección y el **VideoDeviceId** en la propiedad **Id** del grupo de origen de fotogramas multimedia actual.

[!code-cs[GetSettingsWithProfile](./code/Frames_Win10/Frames_Win10/MainPage.xaml.cs#SnippetGetSettingsWithProfile)]

Para obtener más información sobre el uso de perfiles de cámara, consulta [Perfiles de cámara](camera-profiles.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Muestra de fotogramas de cámara](http://go.microsoft.com/fwlink/?LinkId=823230)
 

 




