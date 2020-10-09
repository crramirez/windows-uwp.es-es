---
title: Captura de pantalla a vídeo
description: En este artículo se describe cómo codificar fotogramas capturados desde la pantalla con las API de Windows. Graphics. Capture en un archivo de vídeo.
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: Windows 10, UWP, captura de pantalla, vídeo
ms.localizationpriority: medium
ms.openlocfilehash: 9f95e310fb93292db7dc348493487fada9c6d66e
ms.sourcegitcommit: 83eb36047380501fd1e4d023d593904ad783365b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2020
ms.locfileid: "91852382"
---
# <a name="screen-capture-to-video"></a>Captura de pantalla a vídeo

En este artículo se describe cómo codificar fotogramas capturados desde la pantalla con las API de Windows. Graphics. Capture en un archivo de vídeo. Para obtener información sobre la captura de pantalla de imágenes fijas, consulte [captura de Screeen](./screen-capture.md). Para obtener una aplicación de ejemplo de un extremo a otro simple que use los conceptos y las técnicas que se muestran en este artículo, vea [SimpleRecorder](https://github.com/MicrosoftDocs/SimpleRecorder/).

## <a name="overview-of-the-video-capture-process"></a>Información general del proceso de captura de vídeo
En este artículo se proporciona un tutorial de una aplicación de ejemplo que registra el contenido de una ventana en un archivo de vídeo. Aunque puede parecer que hay una gran cantidad de código necesaria para implementar este escenario, la estructura de alto nivel de una aplicación de grabadora de pantalla es bastante simple. El proceso de captura de pantalla usa tres características principales de UWP:

- Las API de [Windows. GraphicsCapture](/uwp/api/windows.graphics.capture) realizan el trabajo de captar realmente los píxeles de la pantalla. La clase [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) representa la ventana o la pantalla que se va a capturar. [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) se usa para iniciar y detener la operación de captura. La clase [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) mantiene un búfer de fotogramas en el que se copia el contenido de la pantalla.
- La clase [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource) recibe los fotogramas capturados y genera una secuencia de vídeo.
- La clase [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) recibe la secuencia generada por **MediaStreamSource** y la codifica en un archivo de vídeo.

El código de ejemplo que se muestra en este artículo se puede clasificar en varias tareas diferentes:

- **Inicialización** : Esto incluye la configuración de las clases de UWP descritas anteriormente, la inicialización de las interfaces de dispositivo gráfico, la selección de una ventana para capturar y la configuración de los parámetros de codificación, como la velocidad de fotogramas y la resolución.
- **Controladores de eventos y subprocesos** : el controlador principal del bucle de captura principal es el **MediaStreamSource** que solicita tramas periódicamente a través del evento [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) . En este ejemplo se usan eventos para coordinar las solicitudes de nuevos fotogramas entre los diferentes componentes del ejemplo. La sincronización es importante para permitir que los fotogramas se capturen y codifiquen simultáneamente.
- La **copia de Marcos** -fotogramas se copia desde el búfer de fotogramas de captura en una superficie de Direct3D independiente que se puede pasar a **MediaStreamSource** para que el recurso no se sobrescriba mientras se codifica. Las API de Direct3D se usan para realizar esta operación de copia rápidamente. 

### <a name="about-the-direct3d-apis"></a>Acerca de las API de Direct3D
Como se indicó anteriormente, la copia de cada fotograma capturado es probablemente la parte más compleja de la implementación que se muestra en este artículo. En un nivel bajo, esta operación se realiza mediante Direct3D. En este ejemplo, se usa la biblioteca [SharpDX](http://sharpdx.org/) para realizar las operaciones de Direct3D desde C#. Esta biblioteca ya no es compatible oficialmente, pero se eligió porque su rendimiento en operaciones de copia de bajo nivel es adecuado para este escenario. Hemos intentado mantener las operaciones de Direct3D lo más discretas posible para que sea más fácil sustituir su propio código u otras bibliotecas para estas tareas.

## <a name="setting-up-your-project"></a>Configurar tu proyecto
El código de ejemplo de este tutorial se creó con la plantilla de proyecto de C# **aplicación vacía (Windows universal)** en Visual Studio 2019. Para poder usar las API de **Windows. Graphics. Capture** en la aplicación, debe incluir la funcionalidad de **captura de gráficos** en el archivo package. appxmanifest del proyecto. En este ejemplo se guardan los archivos de vídeo generados en la biblioteca de vídeos del dispositivo. Para tener acceso a esta carpeta, debe incluir la capacidad de la **biblioteca de vídeos** .

Para instalar el paquete Nuget de SharpDX, en Visual Studio, seleccione **administrar paquetes Nuget**. En la pestaña examinar, busque el paquete "SharpDX. Direct3D 11" y haga clic en **instalar**.

Tenga en cuenta que para reducir el tamaño de los listados de código de este artículo, el código del tutorial siguiente omite las referencias de espacio de nombres explícitas y la declaración de las variables de miembro de clase MainPage que se denominan con un carácter de subrayado inicial, "_".

## <a name="setup-for-encoding"></a>Configuración de la codificación

El método **SetupEncoding** descrito en esta sección inicializa algunos de los objetos principales que se usarán para capturar y codificar fotogramas de vídeo y configura los parámetros de codificación para el vídeo capturado. Se puede llamar a este método mediante programación o en respuesta a una interacción del usuario como un clic de botón. La lista de código para **SetupEncoding** se muestra a continuación después de las descripciones de los pasos de inicialización.

- **Compruebe la compatibilidad con la captura.** Antes de comenzar el proceso de captura, debe llamar a [GraphicsCaptureSession. IsSupported](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) para asegurarse de que se admite la característica de captura de pantalla en el dispositivo actual.

- **Inicializar interfaces de Direct3D.** En este ejemplo se usa Direct3D para copiar los píxeles capturados de la pantalla en una textura que está codificada como un fotograma de vídeo. Los métodos auxiliares que se usan para inicializar las interfaces de Direct3D, **CreateD3DDevice** y **CreateSharpDXDevice**, se muestran más adelante en este artículo.

- **Inicializa un GraphicsCaptureItem.** Un [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem) representa un elemento de la pantalla que se va a capturar, ya sea una ventana o toda la pantalla. Permite al usuario seleccionar un elemento para capturar creando un [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) y llamando a [PickSingleItemAsync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync).

- **Cree una textura de composición.** Cree un recurso de textura y una vista de destino de representación asociada que se usará para copiar cada fotograma de vídeo. Esta textura no se puede crear hasta que se haya creado el **GraphicsCaptureItem** y se conozcan sus dimensiones. Vea la descripción de **WaitForNewFrame** para ver cómo se usa esta textura de composición. El método auxiliar para crear esta textura también se muestra más adelante en este artículo.

- **Cree un MediaEncodingProfile y un VideoStreamDescriptor.** Una instancia de la clase [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource) tomará las imágenes capturadas de la pantalla y las codificará en una secuencia de vídeo. A continuación, la secuencia de vídeo se transcodificará en un archivo de vídeo por la clase [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) . Un [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor) proporciona parámetros de codificación, como resolución y velocidad de fotogramas, para el **MediaStreamSource**. Los parámetros de codificación del archivo de vídeo para **MediaTranscoder** se especifican con un [MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile). Tenga en cuenta que el tamaño utilizado para la codificación de vídeo no tiene que ser el mismo que el tamaño de la ventana que se va a capturar, pero para simplificar este ejemplo, la configuración de codificación está codificada de forma rígida para usar las dimensiones reales del elemento de captura.

- **Cree los objetos MediaStreamSource y MediaTranscoder.** Como se mencionó anteriormente, el objeto **MediaStreamSource** codifica fotogramas individuales en una secuencia de vídeo. Llame al constructor para esta clase, pasando el **MediaEncodingProfile** creado en el paso anterior. Establezca el tiempo de búfer en cero y los controladores de registro para los eventos de [Inicio](/uwp/api/windows.media.core.mediastreamsource.starting) y [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) , que se mostrarán más adelante en este artículo. A continuación, cree una nueva instancia de la clase **MediaTranscoder** y habilite la aceleración de hardware.

- **Crear un archivo de salida** El último paso de este método es crear un archivo en el que se transcodificará el vídeo. En este ejemplo, solo se creará un archivo con un nombre único en la carpeta videos Library del dispositivo. Tenga en cuenta que para tener acceso a esta carpeta, la aplicación debe especificar la funcionalidad "biblioteca de vídeos" en el manifiesto de la aplicación. Una vez creado el archivo, ábralo para lectura y escritura, y pase el flujo resultante en el método **EncodeAsync** que se mostrará a continuación.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>Iniciar codificación
Ahora que se han inicializado los objetos principales, se implementa el método **EncodeAsync** para iniciar la operación de captura. Este método comprueba primero para asegurarse de que aún no se está grabando y, si no es así, llama al método auxiliar **StartCapture** para empezar a capturar fotogramas de la pantalla. Este método se muestra más adelante en este artículo. A continuación, se llama a [PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) para obtener la **MediaTranscoder** lista para transcodificar la secuencia de vídeo generada por el objeto **MediaStreamSource** en el flujo de archivos de salida, mediante el perfil de codificación que hemos creado en la sección anterior. Una vez preparado el transcodificador, llame a [TranscodeAsync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) para iniciar la transcodificación. Para obtener más información sobre el uso de **MediaTranscoder**, vea [transcodificar archivos multimedia](./transcode-media-files.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>Controlar eventos MediaStreamSource
El objeto **MediaStreamSource** toma fotogramas que se capturan de la pantalla y los transforma en una secuencia de vídeo que se puede guardar en un archivo mediante **MediaTranscoder**. Pasamos los fotogramas a **MediaStreamSource** a través de los controladores de los eventos del objeto.

El evento [SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested) se genera cuando el **MediaStreamSource** está listo para un nuevo fotograma de vídeo. Después de asegurarse de que estamos grabando actualmente, se llama al método auxiliar **WaitForNewFrame** para obtener un nuevo fotograma capturado de la pantalla. Este método, que se muestra más adelante en este artículo, devuelve un objeto [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) que contiene el fotograma capturado. En este ejemplo, se ajusta la interfaz **IDirect3DSurface** en una clase auxiliar que también almacena la hora del sistema en la que se capturó el fotograma. Tanto el marco como la hora del sistema se pasan en el Factory Method [MediaStreamSample. CreateFromDirect3D11Surface](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) y el [MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample) resultante se establece en la propiedad [MediaStreamSourceSampleRequest. Sample](/uwp/api/windows.media.core.mediastreamsourcesamplerequest.sample) del [MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs). Así es como se proporciona el fotograma capturado al **MediaStreamSource**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

En el controlador del evento de [Inicio](/uwp/api/windows.media.core.mediastreamsource.starting) , se llama a **WaitForNewFrame**, pero solo se pasa la hora del sistema que el fotograma se capturó al método [MediaStreamSourceStartingRequest. SetActualStartPosition](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition) , que el **MediaStreamSource** usa para codificar correctamente el tiempo de los fotogramas posteriores.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>Iniciar captura
El método **StartCapture** que se muestra en este paso se llama desde el método auxiliar **EncodeAsync** mostrado en un paso anterior. En primer lugar, este método inicializa un conjunto de objetos de evento que se usan para controlar el flujo de la operación de captura.

- **_multithread** es una clase auxiliar que ajusta el objeto **multiproceso** de la biblioteca SharpDX que se usará para asegurarse de que ningún otro subproceso tenga acceso a la textura de SharpDX mientras se copia.
- **_frameEvent** se usa para indicar que se ha capturado un nuevo fotograma y se puede pasar a **MediaStreamSource**
- **_closedEvent** indica que se ha detenido la grabación y que no se debe esperar ningún fotograma nuevo.

El evento de marco y el evento cerrado se agregan a una matriz para que podamos esperar a uno de ellos en el bucle de captura.

El resto del método **StartCapture** configura las API de Windows. Graphics. Capture que realizarán la captura de pantalla real. En primer lugar, se registra un evento para el evento **CaptureItem. Closed** . A continuación, se crea un [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) , que permite almacenar en búfer varios fotogramas capturados a la vez. El método [CreateFreeThreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded) se usa para crear el grupo de marcos de modo que se llame al evento [FrameArrived](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived) en el subproceso de trabajo del grupo en lugar de en el subproceso principal de la aplicación. A continuación, se registra un controlador para el evento **FrameArrived** . Por último, se crea un [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) para el **CaptureItem** seleccionado y la captura de fotogramas se inicia mediante una llamada a [StartCapture](/uwp/api/windows.graphics.capture.graphicscapturesession).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>Control de eventos de captura de gráficos
En el paso anterior, registramos dos controladores para los eventos de captura de gráficos y configuramos algunos eventos para ayudar a administrar el flujo del bucle de captura.

El evento **FrameArrived** se genera cuando **Direct3D11CaptureFramePool** tiene disponible un nuevo fotograma capturado. En el controlador de este evento, llame a [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) en el remitente para obtener el siguiente fotograma capturado. Una vez recuperado el fotograma, se establece el **_frameEvent** para que el bucle de captura sepa que hay un nuevo marco disponible.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

En el controlador de eventos **Closed** , se indica el **_closedEvent** para que el bucle de captura sepa cuándo detenerse.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>Esperar nuevos marcos
El método auxiliar **WaitForNewFrame** descrito en esta sección es donde se produce el pesado trabajo del bucle de captura. Recuerde que se llama a este método desde el controlador de eventos **OnMediaStreamSourceSampleRequested** cada vez que el **MediaStreamSource** está listo para que se agregue un nuevo marco a la secuencia de vídeo. En un nivel alto, esta función simplemente copia cada fotograma de vídeo capturado en pantalla de una superficie de Direct3D a otra, de modo que se pueda pasar a **MediaStreamSource** para la codificación mientras se captura un nuevo fotograma. En este ejemplo se usa la biblioteca SharpDX para realizar la operación de copia real.

Antes de esperar a un nuevo fotograma, el método desecha cualquier fotograma anterior almacenado en la variable de clase, **_currentFrame**y restablece el **_frameEvent**. A continuación, el método espera a que se Señalice el **_frameEvent** o el **_closedEvent** . Si se establece el evento closed, la aplicación llama a un método auxiliar para limpiar los recursos de captura. Este método se muestra más adelante en este artículo.

Si se establece el evento Frame, sabemos que se ha llamado al controlador de eventos **FrameArrived** definido en el paso anterior y que comenzamos el proceso de copia de los datos de fotogramas capturados en una superficie de Direct3D 11 que se pasará a **MediaStreamSource**.

En este ejemplo se usa una clase auxiliar, **SurfaceWithInfo**, que simplemente nos permite pasar el fotograma de vídeo y la hora del sistema del fotograma, ambos requeridos por **MediaStreamSource** , como un solo objeto. El primer paso del proceso de copia de fotogramas consiste en crear una instancia de esta clase y establecer la hora del sistema.

Los pasos siguientes son la parte de este ejemplo que se basa específicamente en la biblioteca SharpDX. Las funciones auxiliares que se usan aquí se definen al final de este artículo. En primer lugar, usamos **MultiThreadLock** para asegurarse de que ningún otro subproceso tiene acceso al búfer de fotogramas de vídeo mientras hacemos la copia. A continuación, llamamos al método auxiliar **CreateSharpDXTexture2D** para crear un objeto SharpDX **Texture2D** desde el fotograma de vídeo. Será la textura de origen para la operación de copia. 

A continuación, copiaremos del objeto **Texture2D** creado en el paso anterior en la textura de composición que hemos creado anteriormente en el proceso. Esta textura de composición actúa como búfer de intercambio para que el proceso de codificación pueda funcionar en los píxeles mientras se captura el siguiente fotograma. Para realizar la copia, borramos la vista de destino de representación asociada a la textura de composición y, a continuación, definimos la región dentro de la textura que queremos copiar (toda la textura en este caso) y, a continuación, llamamos a **CopySubresourceRegion** para copiar realmente los píxeles en la textura de composición.

Creamos una copia de la descripción de textura que se va a usar cuando se crea la textura de destino, pero se modifica la descripción y se establece **BindFlags** en **RenderTarget** para que la nueva textura tenga acceso de escritura. Establecer **CpuAccessFlags** en **None** permite que el sistema optimice la operación de copia. La descripción de la textura se usa para crear un nuevo recurso de textura y el recurso de textura de composición se copia en este nuevo recurso con una llamada a **CopyResource**. Por último, se llama a **CreateDirect3DSurfaceFromSharpDXTexture** para crear el objeto **IDirect3DSurface** que se devuelve desde este método.


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>Detener la captura y limpiar los recursos
El método **Stop** proporciona una manera de detener la operación de captura. La aplicación puede llamar a este método mediante programación o en respuesta a una interacción del usuario, como un clic de botón. Este método simplemente establece el **_closedEvent**. El método **WaitForNewFrame** definido en los pasos anteriores busca este evento y, si se establece, cierra la operación de captura.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

El método **Cleanup** se usa para eliminar correctamente los recursos creados durante la operación de copia. Esto incluye:

- El objeto **Direct3D11CaptureFramePool** usado por la sesión de captura
- **GraphicsCaptureSession** y **GraphicsCaptureItem**
- Dispositivos Direct3D y SharpDX
- La vista de destino de representación y textura SharpDX utilizada en la operación de copia.
- **Direct3D11CaptureFrame** que se usa para almacenar el marco actual.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>Clases contenedoras auxiliares
Las siguientes clases auxiliares se definieron para ayudar con el código de ejemplo de este artículo.

La clase auxiliar **MultithreadLock** incluye la clase **multiproceso** SharpDX que garantiza que otros subprocesos no tengan acceso a los recursos de textura mientras se copian.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo** se usa para asociar un **IDirect3DSurface** con un **SystemRelativeTime** que representa la trama capturada y la hora en que se capturó, respectivamente.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>API auxiliares de Direct3D y SharpDX
Las siguientes API auxiliares están definidas para abstraer la creación de recursos de Direct3D y SharpDX. Una explicación detallada de estas tecnologías está fuera del ámbito de este artículo, pero aquí se proporciona el código para que pueda implementar el código de ejemplo que se muestra en el tutorial.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>Vea también

* [Espacio de nombres Windows. Graphics. Capture](/uwp/api/windows.graphics.capture)
* [Captura de pantalla](screen-capture.md)