---
ms.assetid: a128edc8-8a80-4645-ac29-908ede2d1c72
description: En este artículo se muestra cómo usar MediaFrameReader con MediaCapture para obtener fotogramas multimedia de uno o más orígenes disponibles, lo que incluye cámaras a color, de profundidad y de infrarrojos, dispositivos de audio o incluso orígenes de fotogramas personalizados, como los que producen fotogramas de seguimiento estructurales.
title: Procesar fotogramas multimedia con MediaFrameReader
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b53103f0d0c67bd18b71ac94812f4cef53ca8ac0
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363818"
---
# <a name="process-media-frames-with-mediaframereader"></a>Procesar fotogramas multimedia con MediaFrameReader

En este artículo se muestra cómo usar un [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) con [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture) para obtener fotogramas multimedia de uno o varios orígenes disponibles, incluidos el color, la profundidad, las cámaras de infrarrojos, los dispositivos de audio o incluso los orígenes de fotogramas personalizados, como los que producen fotogramas de seguimiento de esqueleto. Esta característica se diseñó para que la usen las aplicaciones que realizan procesamiento en tiempo real de fotogramas multimedia, como las aplicaciones de realidad aumentada y las de cámara con reconocimiento de profundidad.

Si, simplemente, estás interesado en capturar vídeo o fotos, como una aplicación típica de fotografía, es probable que quieras usar una de las otras técnicas de captura que admite [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture). Para obtener una lista de las técnicas de captura de elementos multimedia disponibles y artículos donde se muestra cómo usarlos, consulta [**Cámara**](camera.md).

> [!NOTE] 
> Las características descritas en este artículo solo están disponibles a partir de Windows 10, versión 1607.

> [!NOTE] 
> Hay una muestra de aplicación universal de Windows que muestra el uso de **MediaFrameReader** para mostrar fotogramas de distintos orígenes de fotogramas, lo que incluye cámaras a color, de profundidad y de infrarrojos. Para obtener más información, consulta [Camera frames sample (Muestra de fotogramas de cámara)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames).

> [!NOTE] 
> En Windows 10, versión 1803, se presentó un nuevo conjunto de API para usar **MediaFrameReader** con datos de audio. Para obtener más información, consulte [procesar fotogramas de audio con MediaFrameReader](process-audio-frames-with-mediaframereader.md).


## <a name="setting-up-your-project"></a>Configurar tu proyecto
Al igual que con cualquier aplicación que use **MediaCapture**, debes declarar que tu aplicación usa la funcionalidad *cámara web* antes de intentar acceder a cualquier dispositivo de cámara. Si la aplicación captura desde un dispositivo de audio, también debes declarar la funcionalidad *micrófono* del dispositivo. 

**Agregar funcionalidades al manifiesto de la aplicación**

1.  En Microsoft Visual Studio, en el **Explorador de soluciones**, abre el diseñador para el manifiesto de la aplicación haciendo doble clic en el elemento **package.appxmanifest**.
2.  Seleccione la pestaña **Funcionalidades**.
3.  Active la casilla de la **cámara web** y el cuadro de **micrófono**.
4.  Para obtener acceso a la biblioteca de imágenes y vídeos, marca las casillas de **Biblioteca de imágenes** y de **Biblioteca de vídeos**.

En el código de ejemplo de este artículo se usan las API de los siguientes espacios de nombres, además de las que se incluyen con la plantilla de proyecto predeterminada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFramesUsing":::

## <a name="select-frame-sources-and-frame-source-groups"></a>Seleccionar orígenes de fotogramas y grupos de orígenes de fotogramas
Muchas aplicaciones que procesan los fotogramas multimedia necesitan obtener fotogramas de distintos orígenes al mismo tiempo, como cámaras de profundidad y de color de un dispositivo. El objeto [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) representa un conjunto de orígenes de fotogramas multimedia que se pueden usar simultáneamente. Llama al método estático [**MediaFrameSourceGroup.FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) para obtener una lista de todos los grupos de orígenes de fotogramas que admite el dispositivo actual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFindAllAsync":::

También puede crear una [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) mediante [**DeviceInformation. CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher) y el valor devuelto por [**MediaFrameSourceGroup. GetDeviceSelector**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) para recibir notificaciones cuando cambien los grupos de orígenes de fotogramas disponibles en el dispositivo, como cuando una cámara externa esté conectada. Para obtener más información, consulta [**Enumerar dispositivos**](../devices-sensors/enumerate-devices.md).

Una clase [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) tiene una colección de objetos [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) que describen los orígenes de fotogramas que se incluyen en el grupo. Después de recuperar los grupos de origen de fotogramas disponibles en el dispositivo, puedes seleccionar el grupo que expone los orígenes de fotogramas que te interesan.

El siguiente ejemplo muestra la forma más sencilla de seleccionar un grupo de orígenes de fotogramas. Este código, simplemente, recorre todos los grupos disponibles y, a continuación, recorre cada elemento de la colección [**SourceInfos**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.sourceinfos). Cada objeto **MediaFrameSourceInfo** se comprueba para ver si admite las características que nos interesan. En este caso, se comprueba el valor [**VideoPreview**](/uwp/api/Windows.Media.Capture.MediaStreamType) de la propiedad [**MediaStreamType**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.mediastreamtype), lo que significa que el dispositivo proporciona una secuencia de vista previa de vídeo, y se comprueba el valor [**Color**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceKind) de la propiedad [**SourceKind**](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.sourcekind), que indica que el origen proporciona fotogramas de color.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSimpleSelect":::

Este método para identificar el grupo de orígenes de fotogramas y los orígenes de fotogramas deseados funciona con casos sencillos, pero si quieres seleccionar orígenes de fotogramas por criterios más complejos, puede volverse más complicado. Otro método es usar la sintaxis de Linq y objetos anónimos para realizar la selección. En el siguiente ejemplo se usa el método de extensión **Select** para transformar los objetos **MediaFrameSourceGroup** de la lista *frameSourceGroups* en un objeto anónimo con dos campos: *sourceGroup*, que representa el grupo en sí mismo, y *colorSourceInfo*, que representa el origen del fotograma de color en el grupo. El campo *colorSourceInfo* se establece en el resultado del elemento **FirstOrDefault**, que selecciona el primer objeto para el que se resuelve el predicado proporcionado en true. En este caso, el predicado es true si el tipo de secuencia es **VideoPreview**, el tipo de origen es **Color** y la cámara está en el panel frontal del dispositivo.

De la lista de objetos anónimos que devuelve la consulta que se describió anteriormente, el método de extensión **Where** se usa para seleccionar solo los objetos en los que el campo *colorSourceInfo* no es nulo. Por último, se llama al objeto **FirstOrDefault** para seleccionar el primer elemento de la lista.

Ahora puedes usar los campos del objeto seleccionado para obtener referencias de los objetos **MediaFrameSourceGroup** y **MediaFrameSourceInfo** que representan la cámara a color. Se usarán más tarde para inicializar el objeto **MediaCapture** y crear una clase **MediaFrameReader** para el origen seleccionado. Por último, debes comprobar si el grupo de orígenes es nulo, lo que significa que el dispositivo actual no tiene los orígenes de captura solicitados.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColor":::

En el siguiente ejemplo se usa una técnica similar, como la que se describió anteriormente, para seleccionar un grupo de orígenes que contiene cámaras a color, de profundidad y de infrarrojos.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetColorInfraredDepth":::

> [!NOTE]
> A partir de Windows 10, versión 1803, puede usar la clase [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) para seleccionar un origen de fotogramas multimedia con un conjunto de funcionalidades deseadas. Para obtener más información, consulte la sección **uso de perfiles de vídeo para seleccionar un origen de fotogramas** más adelante en este artículo.


## <a name="initialize-the-mediacapture-object-to-use-the-selected-frame-source-group"></a>Inicializar el objeto MediaCapture para usar el grupo de orígenes de fotogramas seleccionado
El siguiente paso es inicializar el objeto **MediaCapture** para usar el grupo de orígenes de fotogramas seleccionado en el paso anterior.

El objeto **MediaCapture** se usa normalmente desde varias ubicaciones dentro de la aplicación, por lo que debes declarar una variable de miembro de clase para contenerlo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaCapture":::

Para crear una instancia del objeto **MediaCapture**, llama al constructor. A continuación, cree un objeto [**MediaCaptureInitializationSettings**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings) que se usará para inicializar el objeto **MediaCapture** . En este ejemplo, se usan los siguientes valores:

* [**SourceGroup**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sourcegroup): indica al sistema qué grupo de orígenes usarás para obtener fotogramas. Recuerda que el grupo de orígenes define un conjunto de orígenes de fotogramas multimedia que se pueden usar simultáneamente.
* [**SharingMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.sharingmode): indica al sistema si necesitas un control exclusivo de los dispositivos de origen de captura. Si lo estableces en [**ExclusiveControl**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode), significa que puedes cambiar la configuración del dispositivo de captura, como el formato de los fotogramas que produce, pero también significa que si otra aplicación ya tiene un control exclusivo, se producirá un error en la aplicación al intentar inicializar el dispositivo de captura multimedia. Si lo estableces en [**SharedReadOnly**](/uwp/api/Windows.Media.Capture.MediaCaptureSharingMode), puede recibir fotogramas de los orígenes de fotogramas incluso si otra aplicación los usa, pero no puedes cambiar la configuración de los dispositivos.
* [**MemoryPreference**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.memorypreference): si especificas [**CPU**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference), el sistema usará la memoria de la CPU que garantiza que, cuando lleguen fotogramas, estarán disponibles como objetos [**SoftwareBitmap**](/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap). Si especificas [**Auto**](/uwp/api/Windows.Media.Capture.MediaCaptureMemoryPreference), el sistema elegirá dinámicamente la ubicación de memoria óptima para almacenar los fotogramas. Si el sistema elige usar la memoria de la GPU, los fotogramas multimedia llegarán como un objeto [**IDirect3DSurface**](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) y no como un objeto **SoftwareBitmap**.
* [**StreamingCaptureMode**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.streamingcapturemode): establécelo en [**Video**](/uwp/api/Windows.Media.Capture.StreamingCaptureMode) para indicar que no es necesario transmitir el audio.

Llama al método [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) para inicializar la clase **MediaCapture** con la configuración que quieras. Asegúrate de llamar a este método dentro de un bloque *try* por si se produce un error de inicialización.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMediaCapture":::

## <a name="set-the-preferred-format-for-the-frame-source"></a>Establecer el formato preferido en el origen de fotogramas
Para establecer el formato preferido en un origen de fotogramas, debes obtener un objeto [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) que represente el origen. Para obtener el objeto, accede al diccionario de la propiedad [**Frames**](/previous-versions/windows/apps/phone/jj207578(v=win.10)) del objeto **MediaCapture** y especifica el identificador del origen de fotogramas que quieres usar. Por este motivo guardamos el objeto [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) mientras seleccionábamos un grupo de orígenes de fotogramas.

La propiedad [**MediaFrameSource.SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats) contiene una lista de objetos [**MediaFrameFormat**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameFormat) que describen los formatos admitidos para el origen de fotogramas. Usa el método de extensión de Linq **Where** para seleccionar un formato basado en las propiedades que quieras. En este ejemplo, se selecciona un formato que tiene un ancho de 1080 píxeles y puede suministrar fotogramas en un formato RGB de 32 bits. El método de extensión **FirstOrDefault** selecciona la primera entrada de la lista. Si el formato seleccionado es nulo, el origen de fotogramas no admitirá el formato solicitado. Si se admite el formato, puedes solicitar que el origen use este formato llamando al objeto [**SetFormatAsync**](../develop/index.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetPreferredFormat":::

## <a name="create-a-frame-reader-for-the-frame-source"></a>Crear un lector de fotogramas para el origen de fotogramas
Para recibir fotogramas de un origen de fotogramas multimedia, usa una clase [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareMediaFrameReader":::

Para crear una instancia del lector de fotogramas, llama al método [**CreateFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createframereaderasync) en el objeto **MediaCapture** inicializado. El primer argumento de este método es el origen de fotogramas desde el que quieres recibir fotogramas. Puedes crear un lector de fotogramas distinto para cada origen de fotogramas que quieras usar. El segundo argumento indica al sistema el formato de salida en el que quieres que lleguen los fotogramas. Esto puede ahorrarte tener que realizar tus propias conversiones a fotogramas a medida que llegan. Ten en cuenta que, si especificas un formato que no es compatible con el origen de fotogramas, se generará una excepción, así que asegúrate de que el valor esté en la colección [**SupportedFormats**](/uwp/api/windows.media.capture.frames.mediaframesource.supportedformats).  

Después de crear el lector de fotogramas, registra un controlador para el evento [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) que se genera siempre que un nuevo fotograma está disponible desde el origen.

Indica al sistema que comience a leer fotogramas desde el origen con una llamada al método [**StartAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.startasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCreateFrameReader":::

## <a name="handle-the-frame-arrived-event"></a>Controlar el evento de llegada de fotogramas
El evento [**MediaFrameReader.FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) se genera siempre que un nuevo fotograma esté disponible. Puedes elegir procesar cada fotograma que llegue o usar solo fotogramas cuando los necesites. Ya que el lector de fotogramas genera el evento en su propio subproceso, es posible que tengas que implementar una lógica de sincronización para asegurarte de que no intentas acceder a los mismos datos desde varios subprocesos. En esta sección se muestra cómo sincronizar dibujando fotogramas de color en un control de imagen de una página XAML. Este escenario trata la restricción de sincronización adicional que necesita que todas las actualizaciones de los controles XAML se realicen en el subproceso de la interfaz de usuario.

El primer paso para mostrar fotogramas en XAML es crear un control de imagen. 

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetImageElementXAML":::

En la página de código subyacente, declara una variable de miembro de clase de tipo **SoftwareBitmap** que se usará como un búfer de reserva al que se copiarán todas las imágenes entrantes. Ten en cuenta que no se copian los datos de imagen, solo las referencias de objeto. Además, declara un valor booleano para comprobar si la operación de interfaz de usuario todavía se está ejecutando.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetDeclareBackBuffer":::

Ya que los fotogramas llegarán como objetos **SoftwareBitmap**, debes crear un objeto [**SoftwareBitmapSource**](/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) que te permita usar un objeto **SoftwareBitmap** como el origen de un **Control** XAML. Debes establecer el origen de imagen en algún lugar del código antes de iniciar el lector de fotogramas.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetImageElementSource":::

Ahora es el momento de implementar el controlador de eventos **FrameArrived**. Cuando se llama al controlador, el parámetro *sender* contiene una referencia al objeto **MediaFrameReader** que generó el evento. Llama al método [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe) en este objeto para intentar obtener el fotograma más reciente. Como su nombre indica, es posible que el método **TryAcquireLatestFrame** no consiga devolver correctamente un fotograma. Por lo tanto, cuando accedes a las propiedades VideoMediaFrame y SoftwareBitmap, asegúrate de probar que no son nulas. En este ejemplo, el operador condicional nulo ? se usa para acceder a la propiedad **SoftwareBitmap** y, a continuación, se comprueba que el objeto recuperado no sea nulo.

El control **Image** solo puede mostrar imágenes en formato BRGA8 con valores alfa premultiplicados o sin valores alfa. Si el fotograma de llegada no está en ese formato, el método estático [**Convert**](/uwp/api/windows.graphics.imaging.softwarebitmap.convert) se usa para convertir el mapa de bits de software al formato correcto.

A continuación, se usa el método [**Interlocked.Exchange**](/dotnet/api/system.threading.interlocked.exchange#System_Threading_Interlocked_Exchange__1___0____0_) para intercambiar la referencia del mapa de bits de llegada con el mapa de bits del búfer de reserva. Este método intercambia estas referencias en una operación atómica segura para subprocesos. Después del intercambio, se desecha la imagen anterior del búfer de reserva, ubicada ahora en la variable *softwareBitmap*, para limpiar los recursos.

A continuación, se usa la clase [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) asociada al elemento **Image** para crear una tarea que se ejecutará en el subproceso de la interfaz de usuario mediante una llamada al método [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync). Ya que las tareas asincrónicas se realizarán dentro de la tarea, la expresión lambda que se pasa al método **RunAsync** se declara con la palabra clave *async*.

Dentro de la tarea, se comprueba la variable *_taskRunning* para asegurarse de que solo se ejecuta una instancia de la tarea a la vez. Si ya no se está ejecutando la tarea, la variable *_taskRunning* se establece en true para impedir que se ejecute de nuevo la tarea. En un bucle *while*, se llama al método **Interlocked.Exchange** para copiar desde el búfer de reserva a una propiedad temporal **SoftwareBitmap** hasta que la imagen del búfer de reserva sea nula. Cada vez que se rellena el mapa de bits temporal, la propiedad **Source** de la clase **Image** se convierte en una propiedad **SoftwareBitmapSource** y luego se llama al método [**SetBitmapAsync**](/uwp/api/windows.ui.xaml.media.imaging.softwarebitmapsource.setbitmapasync) para establecer el origen de la imagen.

Por último, la variable *_taskRunning* se vuelve a establecer en false para que la tarea se pueda ejecutar de nuevo la próxima vez que se llame al controlador.

> [!NOTE] 
> Si accedes a los objetos [**SoftwareBitmap**](/uwp/api/windows.media.capture.frames.videomediaframe.softwarebitmap) o [**Direct3DSurface**](/uwp/api/windows.media.capture.frames.videomediaframe.direct3dsurface) proporcionados por la propiedad [**VideoMediaFrame**](/uwp/api/windows.media.capture.frames.mediaframereference.videomediaframe) de una clase [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference), el sistema crea una referencia fuerte a estos objetos, lo que significa que no se eliminarán cuando se llamae a [**Dispose**](/uwp/api/windows.media.capture.frames.mediaframereference.close) en la clase **MediaFrameReference** contenedora. Se debe llamar explícitamente al método **Dispose** de **SoftwareBitmap** o **Direct3DSurface** directamente para los objetos que deben eliminarse inmediatamente. De lo contrario, el recolector de elementos no usados al final liberará la memoria de estos objetos, pero no se puede saber cuando ocurrirá, y si el número de superficies o mapas de bits asignados supera la cantidad máxima permitida por el sistema, el nuevo flujo de fotogramas se detendrá. Puede copiar fotogramas recuperados mediante el método [**SoftwareBitmap. Copy**](/uwp/api/windows.graphics.imaging.softwarebitmap.copy) por ejemplo y, a continuación, liberar los fotogramas originales para superar esta limitación. Además, si crea **MediaFrameReader** con la sobrecarga [CreateFrameReaderAsync (Windows. Media. Capture. frames. MediaFrameSource inputSource, System. String outputSubtype, Windows. Graphics. Imaging. BitmapSize outlocate)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_Windows_Graphics_Imaging_BitmapSize_) o [CreateFrameReaderAsync (Windows. Media. Capture. frames. MediaFrameSource InputSource, System. String outputSubtype)](/uwp/api/windows.media.capture.mediacapture.createframereaderasync#Windows_Media_Capture_MediaCapture_CreateFrameReaderAsync_Windows_Media_Capture_Frames_MediaFrameSource_System_String_), los fotogramas devueltos son copias de los datos de fotogramas originales, por lo que no hacen que la adquisición de fotogramas se detenga 


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetFrameArrived":::

## <a name="cleanup-resources"></a>Limpieza de recursos
Cuando hayas terminado de leer fotogramas, asegúrate de detener el lector de fotogramas multimedia con una llamada al método [**StopAsync**](/uwp/api/windows.media.capture.frames.mediaframereader.stopasync). Después, elimina el controlador **FrameArrived** y desecha el objeto **MediaCapture**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCleanup":::

Para obtener más información sobre cómo limpiar los objetos de captura multimedia cuando se suspende la aplicación, consulta [**Acceso fácil a la vista previa de cámara**](simple-camera-preview-access.md).

## <a name="the-framerenderer-helper-class"></a>La clase auxiliar FrameRenderer
[Camera frames sample (Muestra de fotogramas de cámara)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames) universal de Windows proporciona una clase auxiliar que facilita mostrar los fotogramas de orígenes a color, de infrarrojos y en profundidad en la aplicación. Normalmente, querrás hacer más cosas con los datos en profundidad y de infrarrojos que solo mostrarlos en la pantalla, pero esta clase auxiliar es una herramienta útil para demostrar la característica de lector de fotogramas y para depurar tu propia implementación del lector de fotogramas.

La clase auxiliar **FrameRenderer** implementa los métodos siguientes.

* Constructor **FrameRenderer**: el constructor inicializa la clase auxiliar para usar el elemento [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) de XAML que pasas para mostrar los fotogramas multimedia.
* **ProcessFrame**: este método muestra un fotograma multimedia, que se representa con una clase [**MediaFrameReference**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReference), en el elemento **Image** que pasaste al constructor. Normalmente, debes llamar a este método desde el controlador de eventos [**FrameArrived**](/uwp/api/windows.media.capture.frames.mediaframereader.framearrived) y pasar el fotograma que devuelve el método [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.mediaframereader.tryacquirelatestframe).
* **ConvertToDisplayableImage**: este método comprueba el formato del fotograma multimedia y, si fuera necesario, lo convierte a un formato que se pueda mostrar. Para las imágenes a color, esto significa asegurarse de que el formato de color sea BGRA8 y que el modo alfa del mapa de bits sea premultiplicado. Para fotogramas de infrarrojos o en profundidad, se procesa cada línea de digitalización para convertir los valores de infrarrojos o en profundidad a un degradado psuedocolor, con la clase **PsuedoColorHelper** que también está incluida en la muestra y se indica a continuación.

> [!NOTE] 
> Para manipular píxeles en imágenes **SoftwareBitmap**, debes acceder a un búfer de memoria nativo. Para ello, debes usar la interfaz COM IMemoryBufferByteAccess incluida en la siguiente lista de códigos y actualizar las propiedades del proyecto para permitir la compilación del código no seguro. Para obtener más información, consulta [Creación de imágenes](imaging.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetIMemoryBufferByteAccess":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/FrameRenderer.cs" id="SnippetFrameRenderer":::

## <a name="use-multisourcemediaframereader-to-get-time-corellated-frames-from-multiple-sources"></a>Uso de MultiSourceMediaFrameReader para obtener fotogramas de tiempo corellated de varios orígenes
A partir de Windows 10, versión 1607, puede usar [**MultiSourceMediaFrameReader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) para recibir fotogramas corellated de tiempo de varios orígenes. Esta API facilita el procesamiento que requiere fotogramas de varios orígenes que se han tomado en proximidad temporal de cierre, como el uso de la clase [**DepthCorrelatedCoordinateMapper**](/uwp/api/windows.media.devices.core.depthcorrelatedcoordinatemapper) . Una limitación del uso de este nuevo método es que los eventos de llegada de fotogramas solo se producen a la velocidad del origen de captura más lento. Se quitarán los fotogramas adicionales de los orígenes más rápidos. Además, dado que el sistema espera que los fotogramas lleguen desde distintos orígenes con diferentes tasas, no reconoce automáticamente si un origen ha dejado de generar fotogramas por completo. En el código de ejemplo de esta sección se muestra cómo usar un evento para crear su propia lógica de tiempo de espera que se invoca si los marcos correlacionados no llegan dentro de un límite de tiempo definido por la aplicación.

Los pasos para usar [**MultiSourceMediaFrameReader**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader) son similares a los pasos para usar [**MediaFrameReader**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) descritos anteriormente en este artículo. En este ejemplo se utiliza un origen de color y un origen de profundidad. Declare algunas variables de cadena para almacenar los identificadores de origen de fotogramas multimedia que se usarán para seleccionar fotogramas de cada origen. A continuación, declare un [**ManualResetEventSlim**](/dotnet/api/system.threading.manualreseteventslim), un [**CancellationTokenSource**](/dotnet/api/system.threading.cancellationtokensource)y un [**EventHandler**](/dotnet/api/system.eventhandler) que se usarán para implementar la lógica de tiempo de espera para el ejemplo. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameDeclarations":::

Con las técnicas descritas anteriormente en este artículo, consulte un [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) que incluya los orígenes de color y profundidad necesarios para este escenario de ejemplo. Después de seleccionar el grupo de origen de fotograma deseado, obtenga el [**MediaFrameSourceInfo**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceInfo) para cada origen de fotogramas.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSelectColorAndDepth":::

Cree e inicialice un objeto **MediaCapture** , pasando el grupo de origen del marco seleccionado en la configuración de inicialización.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameInitMediaCapture":::

Después de inicializar el objeto **MediaCapture** , recupere los objetos [**MediaFrameSource**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSource) para las cámaras de color y de profundidad. Almacene el identificador de cada origen para que pueda seleccionar el marco de llegada del origen correspondiente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetColorAndDepthSource":::

Cree e inicialice el **MultiSourceMediaFrameReader** llamando a [**CreateMultiSourceFrameReaderAsync**](/uwp/api/windows.media.capture.mediacapture.createmultisourceframereaderasync) y pasando una matriz de orígenes de fotogramas que usará el lector. Registre un controlador de eventos para el evento [**FrameArrived**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.FrameArrived) . En este ejemplo se crea una instancia de la clase auxiliar **FrameRenderer** , descrita anteriormente en este artículo, para representar fotogramas en un control de **imagen** . Inicie el lector de fotogramas llamando a [**StartAsync**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.StartAsync).

Registre un controlador de eventos para el evento **CorellationFailed** declarado anteriormente en el ejemplo. Indicaremos este evento si uno de los orígenes de fotogramas multimedia que se usan deja de generar fotogramas. Por último, llame a [**Task. Run**](/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run_System_Action_) para llamar al método de la aplicación auxiliar timeout, **NotifyAboutCorrelationFailure**, en un subproceso independiente. La implementación de este método se muestra más adelante en este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetInitMultiFrameReader":::

El evento **FrameArrived** se desencadena cuando un nuevo marco está disponible en todos los orígenes de fotogramas multimedia administrados por **MultiSourceMediaFrameReader**. Esto significa que el evento se generará a la cadencia del origen multimedia más lento. Si un origen produce varios fotogramas en el momento en que un origen más lento produce un fotograma, se quitarán los fotogramas adicionales del origen rápido. 

Obtiene el [**MultiSourceMediaFrameReference**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference) asociado al evento llamando a [**TryAcquireLatestFrame**](/uwp/api/windows.media.capture.frames.multisourcemediaframereader.TryAcquireLatestFrame). Obtiene el **MediaFrameReference** asociado a cada origen de fotogramas multimedia llamando a [**TryGetFrameReferenceBySourceId**](/uwp/api/windows.media.capture.frames.multisourcemediaframereference.trygetframereferencebysourceid), pasando las cadenas de identificador almacenadas al inicializarse el lector de fotogramas.

Llame al método [**set**](/dotnet/api/system.threading.manualreseteventslim.set#System_Threading_ManualResetEventSlim_Set) del objeto **ManualResetEventSlim** para indicar que han llegado los fotogramas. Este evento se comprobará en el método **NotifyCorrelationFailure** que se ejecuta en un subproceso independiente. 

Por último, realice cualquier procesamiento en los fotogramas multimedia correlacionados con el tiempo. En este ejemplo se muestra simplemente el marco del origen de profundidad.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMultiFrameArrived":::

El método auxiliar **NotifyCorrelationFailure** se ejecutó en un subproceso independiente después del inicio del lector de fotogramas. En este método, compruebe si se ha señalado el evento de fotograma recibido. Recuerde que, en el controlador **FrameArrived** , se establece este evento cada vez que llega un conjunto de fotogramas correlacionados. Si el evento no se ha señalado para algún período de tiempo definido por la aplicación, 5 segundos es un valor razonable, y la tarea no se ha cancelado mediante **CancellationToken**, es probable que uno de los orígenes de fotogramas multimedia haya dejado de leer fotogramas. En este caso, normalmente desea apagar el lector de fotogramas, por lo que debe generar el evento **CorrelationFailed** definido por la aplicación. En el controlador de este evento puede detener el lector de fotogramas y limpiar sus recursos asociados, tal como se muestra anteriormente en este artículo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetNotifyCorrelationFailure":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetCorrelationFailure":::

## <a name="use-buffered-frame-acquisition-mode-to-preserve-the-sequence-of-acquired-frames"></a>Usar el modo de adquisición de tramas almacenadas en búfer para conservar la secuencia de fotogramas adquiridos
A partir de Windows 10, versión 1709, puede establecer la propiedad **[AcquisitionMode](/uwp/api/windows.media.capture.frames.mediaframereader.AcquisitionMode)** de **MediaFrameReader** o **MultiSourceMediaFrameReader** en el **almacenamiento en búfer** para conservar la secuencia de fotogramas pasados a la aplicación desde el origen del marco.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetSetBufferedFrameAcquisitionMode":::

En el modo de adquisición predeterminado, en **tiempo real**, si se adquieren varios fotogramas del origen mientras la aplicación sigue controlando el evento **FrameArrived** de un fotograma anterior, el sistema enviará la aplicación al fotograma adquirido más recientemente y quitará los fotogramas adicionales que estén esperando en el búfer. Esto proporciona a la aplicación el fotograma más reciente disponible en todo momento. Este suele ser el modo más útil para aplicaciones informáticas en tiempo real. 

En el modo de adquisición **almacenado en búfer** , el sistema conservará todos los fotogramas en el búfer y los proporcionará a la aplicación a través del evento **FrameArrived** en el orden recibido. Tenga en cuenta que, en este modo, cuando se rellena el búfer del sistema para fotogramas, el sistema dejará de adquirir nuevos fotogramas hasta que la aplicación complete el evento **FrameArrived** para los fotogramas anteriores, liberando más espacio en el búfer.

## <a name="use-mediasource-to-display-frames-in-a-mediaplayerelement"></a>Usar MediaSource para mostrar fotogramas en un MediaPlayerElement
A partir de Windows, versión 1709, puede mostrar fotogramas adquiridos de un **MediaFrameReader** directamente en un control **[MediaPlayerElement](/uwp/api/windows.ui.xaml.controls.mediaplayerelement)** en la página XAML. Esto se logra mediante el uso de **[MediaSource. CreateFromMediaFrameSource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** para crear el objeto **[MediaSource](/uwp/api/windows.media.core.mediasource)** que puede usar directamente un objeto **[MediaPlayer](/uwp/api/windows.media.playback.mediaplayer)** asociado a una clase **MediaPlayerElement**. Para obtener información detallada sobre cómo trabajar con **MediaPlayer** y **MediaPlayerElement**, vea [reproducir audio y vídeo con MediaPlayer](play-audio-and-video-with-mediaplayer.md).

En los ejemplos de código siguientes se muestra una implementación simple que muestra los fotogramas de una cámara orientada hacia delante y hacia atrás simultáneamente en una página XAML.

En primer lugar, agregue dos controles **MediaPlayerElement** a la página XAML.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement1XAML":::

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml" id="SnippetMediaPlayerElement2XAML":::

Después, con las técnicas que se muestran en las secciones anteriores de este artículo, seleccione un **MediaFrameSourceGroup** que contenga objetos **MediaFrameSourceInfo** para cámaras de color en el panel frontal y el panel posterior. Tenga en cuenta que la **MediaPlayer** no convierte automáticamente fotogramas de formatos que no sean de color, como datos de profundidad o de infrarrojos, en datos de color. El uso de otros tipos de sensor puede producir resultados inesperados. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceSelectGroup":::

Inicialice el objeto **MediaCapture** para usar el **MediaFrameSourceGroup**seleccionado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceInitMediaCapture":::

Por último, llame a **[MediaSource. CreateFromMediaFrameSource](/uwp/api/windows.media.core.mediasource.createfrommediaframesource)** para crear un objeto **MediaSource** para cada origen de marco mediante la propiedad **[ID](/uwp/api/windows.media.capture.frames.mediaframesourceinfo.Id)** del objeto **MediaFrameSourceInfo** asociado para seleccionar uno de los orígenes de fotogramas de la colección **[FrameSources](/uwp/api/windows.media.capture.mediacapture.FrameSources)** del objeto **MediaCapture** . Inicialice un nuevo objeto **MediaPlayer** y asígnelo a **MediaPlayerElement** llamando a **[SetMediaPlayer](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.MediaPlayer)**. A continuación, establezca la propiedad **[source](/uwp/api/windows.media.playback.mediaplayer.Source)** en el objeto **MediaSource** recién creado.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetMediaSourceMediaPlayer":::

## <a name="use-video-profiles-to-select-a-frame-source"></a>Usar perfiles de vídeo para seleccionar un origen de fotogramas

Un perfil de cámara, representado por un objeto [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile) , representa un conjunto de funcionalidades que proporciona un dispositivo de captura determinado, como velocidades de fotogramas, resoluciones o características avanzadas, como la captura de HDR. Un dispositivo de captura puede admitir varios perfiles, lo que le permite seleccionar el que está optimizado para el escenario de captura. A partir de Windows 10, versión 1803, puede usar **MediaCaptureVideoProfile** para seleccionar un origen de fotogramas multimedia con capacidades específicas antes de inicializar el objeto **MediaCapture** . El siguiente método de ejemplo busca un perfil de vídeo que admita HDR con una gama de colores anchos (WCG) y devuelve un objeto **MediaCaptureInitializationSettings** que se puede usar para inicializar el **MediaCapture** para usar el dispositivo y el perfil seleccionado.

En primer lugar, llame a [**MediaFrameSourceGroup. FindAllAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) para obtener una lista de todos los grupos de orígenes de fotogramas multimedia disponibles en el dispositivo actual. Recorra cada grupo de origen y llame a [**MediaCapture. FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) para obtener una lista de todos los perfiles de vídeo del grupo de origen actual que admiten el perfil especificado, en este caso HDR con WCG Photo. Si se encuentra un perfil que cumpla los criterios, cree un nuevo objeto **MediaCaptureInitializationSettings** y establezca el **perfil** de videosource en el perfil SELECT y el **VideoDeviceId** en la propiedad **ID** del grupo de origen del marco multimedia actual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/MainPage.xaml.cs" id="SnippetGetSettingsWithProfile":::

Para obtener más información sobre el uso de perfiles de cámara, consulte [perfiles de cámara](camera-profiles.md).

## <a name="related-topics"></a>Temas relacionados

* [Cámara](camera.md)
* [Captura básica de fotos, audio y vídeo con MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Muestra de fotogramas de cámara](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
 

 
