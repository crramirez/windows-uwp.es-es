---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: En este artículo se muestra cómo usar las API del espacio de nombres Windows.Media.Audio para crear gráficos de audio para escenarios de enrutamiento, mezcla y procesamiento de audio.
title: Gráficos de audio
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 72a6fe2e704bc419306c74f410ed51e8e8560fa6
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362978"
---
# <a name="audio-graphs"></a>Gráficos de audio



En este artículo se muestra cómo usar las API en el espacio de nombres [**Windows. Media. audio**](/uwp/api/Windows.Media.Audio) para crear gráficos de audio para escenarios de enrutamiento, combinación y procesamiento de audio.

Un *gráfico de audio* es un conjunto de nodos de audio interconectados a través del cual fluyen los datos de audio. 

- Los *nodos de entrada de audio* proporcionan datos de audio al grafo desde dispositivos de entrada de audio, archivos de audio o desde código personalizado. 

- Los *nodos de salida de audio* son el destino para el audio procesado por el gráfico. El audio del gráfico se puede enrutar a dispositivos de salida de audio, archivos de audio o código personalizado. 

- Los *nodos de submezcla* toman audio desde uno o varios nodos y lo combinan en una sola salida que puede enrutarse a otros nodos del gráfico. 

Después de que se hayan creado todos los nodos y se hayan configurado las conexiones entre ellos, simplemente inicia el gráfico de audio y los datos de audio fluirán desde los nodos de entrada a través de los nodos de submezcla hasta llegar a los nodos de salida. Este modelo permite escenarios como la grabación desde el micrófono de un dispositivo a un archivo de audio, la reproducción de audio desde un archivo al altavoz de un dispositivo o la mezcla de audio desde varias fuentes de forma rápida y fácil de implementar.

Se habilitan escenarios adicionales con la incorporación de efectos de audio al gráfico de audio. Todos los nodos de un gráfico de audio se pueden rellenar con cero o más efectos de audio que realizan el procesamiento de audio en el audio que pasa a través del nodo. Existen varios efectos integrados como eco, ecualizador, limitación y reverberación que se pueden anexar a un nodo de audio con unas pocas líneas de código. También puedes crear tus propios efectos de audio personalizados que funcionan exactamente igual que los efectos integrados.

> [!NOTE]
> La [muestra de AudioGraph para UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AudioCreation) implementa el código analizado en esta introducción. Puedes descargar la muestra para ver el código en contexto o para usarla como punto de partida para tu propia aplicación.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Selección de AudioGraph o XAudio2 de Windows Runtime

Las API de gráfico de audio de Windows Runtime ofrecen una funcionalidad que se puede implementar mediante las [XAudio2 APIs](/windows/desktop/xaudio2/xaudio2-apis-portal) (API de XAudio2) basadas en COM. Las siguientes son características del marco de gráficos de audio de Windows Runtime que difieren de XAudio2.

API de gráficos de audio de Windows Runtime:

-   Son significativamente más fáciles de usar que XAudio2.
-   Pueden usarse desde C#, además de ser compatibles con C++.
-   Pueden usar archivos de audio directamente, incluidos formatos de archivo comprimido. XAudio2 solo funciona en búferes de audio y no proporciona ninguna funcionalidad de E/S de archivo.
-   Puedes usar la canalización de audio de latencia baja de Windows 10.
-   Admite la conmutación automática de los extremos cuando se usan los parámetros de extremo predeterminados. Por ejemplo, si el usuario cambia del altavoz de un dispositivo a unos auriculares, el audio se redirige automáticamente a la entrada nueva.

## <a name="audiograph-class"></a>Clase AudioGraph

La clase [**AudioGraph**](/uwp/api/Windows.Media.Audio.AudioGraph) es el elemento primario de todos los nodos que conforman el gráfico. Usa este objeto para crear instancias de todos los tipos de nodo de audio. Crea una instancia de la clase **AudioGraph** inicializando un objeto [**AudioGraphSettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings) que contenga opciones de configuración para el gráfico y, a continuación, llama a [**AudioGraph.CreateAsync**](/uwp/api/windows.media.audio.audiograph.createasync). El valor [**CreateAudioGraphResult**](/uwp/api/Windows.Media.Audio.CreateAudioGraphResult) devuelto proporciona acceso a un gráfico de audio creado o proporciona un valor de error si se produce un error en la creación del gráfico de audio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareAudioGraph":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetInitAudioGraph":::

-   Todos los tipos de nodo de audio se crean mediante los \* métodos Create de la clase **AudioGraph** .
-   El método [**AudioGraph.Start**](/uwp/api/windows.media.audio.audiograph.start) hace que el gráfico de audio comience a procesar datos de audio. El método [**AudioGraph.Stop**](/uwp/api/windows.media.audio.audiograph.stop) detiene el procesamiento de audio. Cada nodo del gráfico se puede iniciar y detener de forma independiente mientras se está ejecutando el gráfico, pero ningún nodo está activo cuando se detiene el gráfico. [**ResetAllNodes**](/uwp/api/windows.media.audio.audiograph.resetallnodes) hace que todos los nodos del gráfico descarten cualquier dato almacenado actualmente en sus búferes de audio.
-   El evento [**QuantumStarted**](/uwp/api/windows.media.audio.audiograph.quantumstarted) se produce cuando el gráfico está iniciando el procesamiento de un nuevo cuanto de datos de audio. El evento [**QuantumProcessed**](/uwp/api/windows.media.audio.audiograph.quantumprocessed) se produce cuando se completa el procesamiento de un cuanto.

-   La única propiedad [**AudioGraphSettings**](/uwp/api/Windows.Media.Audio.AudioGraphSettings) requerida es [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory). La especificación de este valor permite que el sistema optimice la canalización de audio para la categoría especificada.
-   El tamaño del cuanto del gráfico de audio determina el número de muestras que se procesan a la vez. De manera predeterminada, el tamaño del cuanto es 10 ms, basado en la velocidad de muestra predeterminada. Si se especifica un tamaño de cuanto personalizado estableciendo la propiedad [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum), también debes establecer la propiedad [**QuantumSizeSelectionMode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) en **ClosestToDesired** o se omitirá el valor proporcionado. Si se usa este valor, el sistema elige un tamaño de cuanto lo más cerca posible al especificado. Para determinar el tamaño real del cuanto, comprueba [**SamplesPerQuantum**](/uwp/api/windows.media.audio.audiograph.samplesperquantum) de **AudioGraph** después de que se haya creado.
-   Si solo vas a usar el gráfico de audio con los archivos y no pretendes una salida a un dispositivo de audio, se recomienda que uses el tamaño de cuanto predeterminado no estableciendo la propiedad [**DesiredSamplesPerQuantum**](/uwp/api/windows.media.audio.audiographsettings.desiredsamplesperquantum).
-   La propiedad [**DesiredRenderDeviceAudioProcessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing) determina la cantidad de procesamiento que realiza el dispositivo de representación principal en la salida del gráfico de audio. La configuración **Default** permite que el sistema use el procesamiento de audio predeterminado para la categoría de representación de audio especificada. Este procesamiento puede mejorar considerablemente el sonido de audio en algunos dispositivos, especialmente en los dispositivos móviles con altavoces pequeños. La configuración **Raw** puede mejorar el rendimiento al reducir la cantidad de procesamiento de señal realizado, pero puede producir una calidad de sonido inferior en algunos dispositivos.
-   Si el valor de [**QuantumSizeSelectionMode**](/uwp/api/windows.media.audio.audiographsettings.quantumsizeselectionmode) se establece en **LowestLatency**, el gráfico de audio usará automáticamente **raw** para [**DesiredRenderDeviceAudioProcessing**](/uwp/api/windows.media.audio.audiographsettings.desiredrenderdeviceaudioprocessing).
- A partir de Windows 10, versión 1803, puede establecer la propiedad [**AudioGraphSettings. MaxPlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) para establecer un valor máximo que se usa para las propiedades [**AudioFileInputNode. PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**AudioFrameInputNode. PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor)y [**MediaSourceInputNode. PlaybackSpeedFactor**](/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor) . Cuando un gráfico de audio admite un factor de velocidad de reproducción mayor que 1, el sistema debe asignar memoria adicional para mantener un búfer suficiente de datos de audio. Por esta razón, al establecer **MaxPlaybackSpeedFactor** en el valor más bajo requerido por la aplicación, se reducirá el consumo de memoria de la aplicación. Si la aplicación solo va a reproducir contenido a velocidad normal, se recomienda que establezca MaxPlaybackSpeedFactor en 1.
-   [**EncodingProperties**](/uwp/api/windows.media.audio.audiographsettings.encodingproperties) determina el formato de audio usado por el gráfico. Se admiten los formatos flotantes de 32 bits únicamente.
-   [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) establece el dispositivo de representación principal para el gráfico de audio. Si no estableces esto, se usa el dispositivo predeterminado del sistema. El dispositivo de representación principal se usa para calcular los tamaños de cuanto de otros nodos en el gráfico. Si no hay ningún dispositivo de representación de audio presente en el sistema, se producirá un error en la creación del gráfico de audio.

Puedes permitir que el gráfico de audio use el dispositivo de representación de audio predeterminado o que use la clase [**Windows.Devices.Enumeration.DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para obtener una lista dispositivos de representación de audio disponibles del sistema mediante una llamada a [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) y pasando el selector de dispositivos de representación de audio devuelto por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiorenderselector). Puedes elegir uno de los objetos **DeviceInformation** devueltos mediante programación o mostrar la interfaz de usuario para que el usuario pueda seleccionar un dispositivo y, a continuación, usarlo para establecer la propiedad [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioRenderDevices":::

##  <a name="device-input-node"></a>Nodo de entrada de dispositivo

Un nodo de entrada de dispositivo envía audio al gráfico desde un dispositivo de captura de audio conectado al sistema, como un micrófono. Crea un objeto [**DeviceInputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) que use el dispositivo de captura de audio predeterminado del sistema mediante una llamada a [**CreateDeviceInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync). Proporciona una [**AudioRenderCategory**](/uwp/api/Windows.Media.Render.AudioRenderCategory) para permitir que el sistema optimice la canalización de audio para la categoría especificada.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceInputNode":::

Si desea especificar un dispositivo de captura de audio específico para el nodo de entrada de dispositivo, puede usar la clase [**Windows. Devices. Enumeration. DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) para obtener una lista de los dispositivos de captura de audio disponibles del sistema llamando a [**FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) y pasando el selector de dispositivos de representación de audio devuelto por [**Windows. Media. Devices. MediaDevice. GetAudioCaptureSelector**](/uwp/api/windows.media.devices.mediadevice.getaudiocaptureselector). Puede elegir uno de los objetos **DeviceInformation** devueltos mediante programación o Mostrar la interfaz de usuario para permitir que el usuario seleccione un dispositivo y, a continuación, pasarlo a [**CreateDeviceInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createdeviceinputnodeasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetEnumerateAudioCaptureDevices":::

##  <a name="device-output-node"></a>Nodo de dispositivo de salida

Un nodo de dispositivo de salida envía audio del gráfico a un dispositivo de representación de audio, como los altavoces o auriculares. Crea un [**DeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) llamando a [**CreateDeviceOutputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createdeviceoutputnodeasync). El nodo de salida usa el [**PrimaryRenderDevice**](/uwp/api/windows.media.audio.audiographsettings.primaryrenderdevice) del gráfico de audio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateDeviceOutputNode":::

##  <a name="file-input-node"></a>Nodo de entrada de archivo

Un nodo de entrada de archivo te permite enviar datos desde un archivo de audio al gráfico. Crea un [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) llamando a [**CreateFileInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileInputNode":::

-   Los nodos de entrada de archivos admiten los siguientes formatos de archivo: mp3, wav, wma y m4a.
-   Establece la propiedad [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime) para especificar el desplazamiento de tiempo en el archivo donde debe comenzar la reproducción. Si esta propiedad es null, se usa el comienzo del archivo. Establece la propiedad [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime) para especificar el desplazamiento de tiempo en el archivo donde debe finalizar la reproducción. Si esta propiedad es null, se usa el final del archivo. El valor de tiempo de inicio debe ser menor que el valor de tiempo final y el valor de hora final debe ser menor o igual a la duración del archivo de audio, que puede determinarse comprobando el valor de la propiedad [**Duration**](/uwp/api/windows.media.audio.audiofileinputnode.duration).
-   Busca una posición en el archivo de audio llamando a [**Seek**](/uwp/api/windows.media.audio.audiofileinputnode.seek) y especificando el desplazamiento de tiempo en el archivo al que se moverá la posición de reproducción. El valor especificado no debe superar el rango de [**StartTime**](/uwp/api/windows.media.audio.audiofileinputnode.starttime) y [**EndTime**](/uwp/api/windows.media.audio.audiofileinputnode.endtime). Obtén la posición de reproducción actual del nodo con la propiedad [**Position**](/uwp/api/windows.media.audio.audiofileinputnode.position) de solo lectura.
-   Habilita la función de repetición del archivo de audio estableciendo la propiedad [**LoopCount**](/uwp/api/windows.media.audio.audiofileinputnode.loopcount). Cuando no sea null, este valor indica el número de veces que el archivo se reproducirá después de la reproducción inicial. Por lo tanto, por ejemplo, establecer **LoopCount** en 1 hará que el archivo se reproduzca 2 veces en total y, si se establece en 5, hará que el archivo que se reproduzca 6 veces en total. Establecer **LoopCount** en null hace que el archivo se repita indefinidamente. Para detener la repetición, establece este valor en 0.
-   Ajusta la velocidad a la que el archivo de audio se reproduce estableciendo [**PlaybackSpeedFactor**](/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor). Un valor de 1 indica la velocidad original del archivo, .5 es la velocidad media y 2 es el doble de velocidad.

##  <a name="mediasource-input-node"></a>Nodo de entrada MediaSource

La clase [**MediaSource**](/uwp/api/Windows.Media.Core.MediaSource) proporciona una manera común de hacer referencia a los elementos multimedia de distintos orígenes y expone un modelo común para acceder a los datos multimedia, independientemente del formato multimedia subyacente, que podría ser un archivo en disco, una secuencia o un origen de red de streaming adaptable. Un nodo [* * MediaSourceAudioInputNode](/uwp/api/windows.media.audio.mediasourceaudioinputnode) le permite dirigir datos de audio de un **MediaSource** al gráfico de audio. Cree un **MediaSourceAudioInputNode** mediante una llamada a [**CreateMediaSourceAudioInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), pasando un objeto **MediaSource** que represente el contenido que desea reproducir. Se devuelve un [* * CreateMediaSourceAudioInputNodeResult](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) que puede usar para determinar el estado de la operación comprobando la propiedad de [**Estado**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status) . Si el estado es **correcto**, puede obtener el **MediaSourceAudioInputNode** creado mediante el acceso a la propiedad de [**nodo**](/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node) . En el ejemplo siguiente se muestra la creación de un nodo a partir de un objeto AdaptiveMediaSource que representa el streaming de contenido a través de la red. Para más información sobre cómo trabajar con **MediaSource**, consulte [elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). Para obtener más información sobre el streaming de contenido multimedia a través de Internet, consulte [Adaptive streaming](adaptive-streaming.md).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareMediaSourceInputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateMediaSourceInputNode":::

Para recibir una notificación cuando la reproducción ha alcanzado el final del contenido de **MediaSource** , registre un controlador para el evento [**MediaSourceCompleted**](/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted) . 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterMediaSourceCompleted":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetMediaSourceCompleted":::

Al reproducirse un archivo desde el disco, es probable que siempre se complete correctamente, la transmisión por secuencias de multimedia desde un origen de red puede producir un error durante la reproducción debido a un cambio en la conexión de red u otros problemas que se encuentran fuera del control del gráfico de audio. Si un **MediaSource** deja de ser reproducible durante la reproducción, el gráfico de audio generará el evento [**UnrecoverableErrorOccurred**](/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred) . Puede usar el controlador de este evento para detener y eliminar el gráfico de audio y, a continuación, reinicializar el gráfico. 

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetRegisterUnrecoverableError":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUnrecoverableError":::

##  <a name="file-output-node"></a>Nodo de salida de archivo

Un nodo de salida de archivo permite dirigir datos de audio desde el gráfico a un archivo de audio. Crea un [**AudioFileOutputNode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode) llamando a [**CreateFileOutputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileoutputnodeasync).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFileOutputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFileOutputNode":::

-   Los nodos de salida de archivos admiten los siguientes formatos de archivo: mp3, wav, wma y m4a.
-   Es necesario llamar a [**AudioFileOutputNode.Stop**](/uwp/api/windows.media.audio.audiofileoutputnode.stop) para detener el procesamiento del nodo antes de llamar a [**AudioFileOutputNode.FinalizeAsync**](/uwp/api/windows.media.audio.audiofileoutputnode.finalizeasync) o se generará una excepción.

##  <a name="audio-frame-input-node"></a>Nodo de entrada de fotogramas de audio

Un nodo de entrada de fotogramas de audio permite insertar datos de audio que se generan en su propio código en el gráfico de audio. Esto permite escenarios como crear un sintetizador de software personalizado. Cree un [**AudioFrameInputNode**](/uwp/api/Windows.Media.Audio.AudioFrameInputNode) llamando a [**CreateFrameInputNode**](/uwp/api/windows.media.audio.audiograph.createframeinputnode).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameInputNode":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameInputNode":::

El evento [**FrameInputNode.QuantumStarted**](/uwp/api/windows.media.audio.audioframeinputnode.quantumstarted) se genera cuando el gráfico de audio está listo para empezar a procesar el siguiente cuanto de datos de audio. Proporcionas los datos de audio generados de forma personalizada desde dentro del controlador a este evento.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStarted":::

-   El objeto [**FrameInputNodeQuantumStartedEventArgs**](/uwp/api/Windows.Media.Audio.FrameInputNodeQuantumStartedEventArgs) pasado al controlador de eventos **QuantumStarted** expone la propiedad [**RequiredSamples**](/uwp/api/windows.media.audio.frameinputnodequantumstartedeventargs.requiredsamples) que indica cuántas muestras el gráfico de audio necesita para rellenar el cuanto para su procesamiento.
-   Realiza una llamada a [**AudioFrameInputNode.AddFrame**](/uwp/api/windows.media.audio.audioframeinputnode.addframe) para pasar un objeto [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) rellenado con datos de audio al gráfico.
- En Windows 10, versión 1803, se presentó un nuevo conjunto de API para usar **MediaFrameReader** con datos de audio. Estas API permiten obtener objetos **AudioFrame** a partir de un origen de fotogramas multimedia, que se puede pasar a un **FrameInputNode** mediante el método **AddFrame** . Para obtener más información, consulte [procesar fotogramas de audio con MediaFrameReader](process-audio-frames-with-mediaframereader.md).
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **GenerateAudioData**.

Para rellenar un [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) con datos de audio, debes obtener acceso al búfer de memoria subyacente del fotograma de audio. Para ello, debes inicializar la interfaz COM **IMemoryBufferByteAccess** agregando el siguiente código dentro de tu espacio de nombres.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetComImportIMemoryBufferByteAccess":::

El siguiente código muestra un ejemplo de implementación de un método auxiliar **GenerateAudioData** que crea un [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) y se rellena con datos de audio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetGenerateAudioData":::

-   Como este método tiene acceso el búfer sin procesar subyacente a los tipos de Windows Runtime, se debe declarar con la palabra clave **unsafe**. También debes configurar el proyecto en Microsoft Visual Studio para permitir la compilación del código no seguro; para ello, abre la página **Propiedades** del proyecto, haz clic en la página de propiedades de **Compilación** y selecciona la casilla **Permitir código no seguro**.
-   Inicializa una nueva instancia de [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) en el espacio de nombres **Windows.Media** pasando el tamaño del búfer deseado al constructor. El tamaño de búfer es el número de muestras multiplicado por el tamaño de cada muestra.
-   Obtén el [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer) de la trama de audio llamando a [**LockBuffer**](/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenga una instancia de la interfaz com [**IMemoryBufferByteAccess**](/previous-versions/mt297505(v=vs.85)) del búfer de audio llamando a [**CreateReference**](/uwp/api/windows.media.audiobuffer.createreference).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a [**IMemoryBufferByteAccess.GetBuffer**](/windows/desktop/WinRT/imemorybufferbyteaccess-getbuffer) y conviértelo en el tipo de datos de muestra de los datos de audio.
-   Rellena el búfer con datos y devuelve el [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) para el envío en el gráfico de audio.

##  <a name="audio-frame-output-node"></a>Nodo de salida de fotogramas de audio

Un nodo de salida de fotogramas de audio permite recibir y procesar la salida de datos de audio desde el gráfico de audio con código personalizado que crees. Un escenario de ejemplo para esto es la realización de análisis de señal en la salida de audio. Crea un [**AudioFrameOutputNode**](/uwp/api/Windows.Media.Audio.AudioFrameOutputNode) llamando a [**CreateFrameOutputNode**](/uwp/api/windows.media.audio.audiograph.createframeoutputnode).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetDeclareFrameOutputNode":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateFrameOutputNode":::

El evento [**AudioGraph. QuantumStarted**](/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) se genera cuando el gráfico de audio ha comenzado a procesar un Quantum de datos de audio. Puedes obtener acceso a los datos de audio desde dentro del controlador para este evento. 

> [!NOTE]
> Si desea recuperar fotogramas de audio a una cadencia regular, sincronizados con el gráfico de audio, llame a [AudioFrameOutputNode. GetFrame](/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) desde dentro del controlador de eventos **QuantumStarted** sincrónicos. El evento **QuantumProcessed** se genera de forma asincrónica después de que el motor de audio haya completado el procesamiento de audio, lo que significa que su cadencia puede ser irregular. Por lo tanto, no debe usar el evento **QuantumProcessed** para el procesamiento sincronizado de los datos de fotogramas de audio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetQuantumStartedFrameOutput":::

-   Realiza una llamada a [**GetFrame**](/uwp/api/windows.media.audio.audioframeoutputnode.getframe) para obtener un objeto [**AudioFrame**](/uwp/api/Windows.Media.AudioFrame) rellenado con datos de audio del gráfico.
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **ProcessFrameOutput**.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetProcessFrameOutput":::

-   Al igual que el ejemplo de nodo de entrada de fotogramas de audio anterior, tendrás que declarar la interfaz COM **IMemoryBufferByteAccess** y configurar el proyecto para permitir que el código no seguro tenga acceso al búfer subyacente de audio.
-   Obtén el [**AudioBuffer**](/uwp/api/Windows.Media.AudioBuffer) de la trama de audio llamando a [**LockBuffer**](/uwp/api/windows.media.audioframe.lockbuffer).
-   Obtenga una instancia de la interfaz com **IMemoryBufferByteAccess** del búfer de audio llamando a [**CreateReference**](/uwp/api/windows.media.audiobuffer.createreference).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a **IMemoryBufferByteAccess.GetBuffer** y conviértelo en el tipo de datos de muestra de los datos de audio.

## <a name="node-connections-and-submix-nodes"></a>Conexiones de nodo y nodos de submezcla

Todos los tipos de nodos de entrada exponen el método **AddOutgoingConnection** que enruta el audio generado por el nodo al que se pasa al método. El siguiente ejemplo conecta un [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) a un [**AudioDeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode), que es una configuración simple para reproducir un archivo de audio en el altavoz del dispositivo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection1":::

Puedes crear más de una conexión desde un nodo de entrada a otros nodos. El siguiente ejemplo agrega otra conexión desde [**AudioFileInputNode**](/uwp/api/Windows.Media.Audio.AudioFileInputNode) a un [**AudioFileOutputNode**](/uwp/api/Windows.Media.Audio.AudioFileOutputNode). Ahora, el audio desde el archivo de audio se reproduce en el altavoz del dispositivo y también se escribe en un archivo de audio.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection2":::

Los nodos de salida también pueden recibir más de una conexión desde otros nodos. En el siguiente ejemplo, se establece una conexión desde un [**AudioDeviceInputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceInputNode) al nodo [**AudioDeviceOutput**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode). Dado que el nodo de salida tiene conexiones desde el nodo de entrada del archivo y el nodo de entrada del dispositivo, la salida contendrá una mezcla de audio de ambas fuentes. **AddOutgoingConnection** proporciona una sobrecarga que te permite especificar un valor de ganancia para la señal que pasa a través de la conexión.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddOutgoingConnection3":::

Aunque los nodos de salida pueden aceptar conexiones de varios nodos, puedes crear una mezcla intermedia de señales desde uno o varios nodos antes de pasar la combinación a una salida. Por ejemplo, puedes establecer el nivel o aplicar efectos a un subconjunto de las señales de audio en un gráfico. Para llevarlo a cabo, puedes usar el elemento [**AudioSubmixNode**](/uwp/api/Windows.Media.Audio.AudioSubmixNode). Puedes conectarte a un nodo de submezcla de uno o más nodos de entrada u otros nodos de submezcla. En el ejemplo siguiente, se crea un nuevo nodo de submezcla con [**AudioGraph.CreateSubmixNode**](/uwp/api/windows.media.audio.audiograph.createsubmixnode). A continuación, se agregan las conexiones desde un nodo de entrada de archivo y un nodo de salida de fotogramas al nodo de submezcla. Por último, el nodo de submezcla está conectado a un nodo de salida de archivo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateSubmixNode":::

## <a name="starting-and-stopping-audio-graph-nodes"></a>Iniciar y detener los nodos del gráfico de audio

Cuando se realiza la llamada a [**AudioGraph.Start**](/uwp/api/windows.media.audio.audiograph.start), el audio gráfico comienza el procesamiento de datos de audio. Cada tipo de nodo proporciona métodos **Start** y **Stop** que permiten que el nodo individual inicie o detenga el procesamiento de datos. Cuando se realiza una llamada a [**AudioGraph.Stop**](/uwp/api/windows.media.audio.audiograph.stop), se detiene todo el procesamiento de audio en todos los nodos independientemente del estado de los nodos individuales, pero puedes establecer el estado de cada nodo mientras el gráfico de audio está detenido. Por ejemplo, podrías realizar una llamada a **Stop** en un nodo individual mientras se detiene el gráfico y después realizar una llamada a **AudioGraph.Start**, y el nodo individual permanecerá en estado de detención.

Todos los tipos de nodos exponen la propiedad **ConsumeInput** que, cuando se establece en false, permite que el nodo continúe con el procesamiento de audio, pero no permite que consuma ningún dato de audio que se envíe desde otros nodos.

Todos los tipos de nodos exponen el método **Reset**, que permite que el nodo descarte todos los datos de audio actualmente en el búfer.

## <a name="adding-audio-effects"></a>Agregar efectos de audio

La API de gráfico de audio te permite agregar efectos de audio para cada tipo de nodo en un gráfico. Los nodos de salida, nodos de entrada y nodos de submezcla pueden tener un número ilimitado de efectos de audio, limitados solo por las funcionalidades del hardware. El siguiente ejemplo muestra cómo agregar el efecto de eco integrado a un nodo de submezcla.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetAddEffect":::

-   Todos los efectos de audio implementan [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition). Cada nodo expone una propiedad **EffectDefinitions** que representa la lista de efectos aplicados a ese nodo. Agrega un efecto agregando su objeto de definición a la lista.
-   Existen varias clases de definición de efectos que se proporcionan en el espacio de nombres **Windows.Media.Audio**. Entre ellas se incluyen las siguientes:
    -   [**EchoEffectDefinition**](/uwp/api/Windows.Media.Audio.EchoEffectDefinition)
    -   [**EqualizerEffectDefinition**](/uwp/api/Windows.Media.Audio.EqualizerEffectDefinition)
    -   [**LimiterEffectDefinition**](/uwp/api/Windows.Media.Audio.LimiterEffectDefinition)
    -   [**ReverbEffectDefinition**](/uwp/api/Windows.Media.Audio.ReverbEffectDefinition)
-   Puedes crear tus propios efectos de audio que implementen [**IAudioEffectDefinition**](/uwp/api/Windows.Media.Effects.IAudioEffectDefinition) y aplicarlos a cualquier nodo en un gráfico de audio.
-   Cada tipo de nodo expone un método **DisableEffectsByDefinition** que deshabilita todos los efectos de la lista **EffectDefinitions** del nodo que se agregaron mediante la definición especificada. **EnableEffectsByDefinition** habilita los efectos con la definición especificada.

## <a name="spatial-audio"></a>Audio espacial
A partir de Windows 10, versión 1607, **AudioGraph** admite audio espacial, que te permite especificar la ubicación en el espacio 3D desde la que se emite el audio de cualquier nodo de entrada o de submezcla. También puedes especificar una forma y una dirección en la que se emita el audio y una velocidad que se usará para cambiar el audio del nodo a Doppler, así como definir un modelo de caída que describa cómo se atenuará el audio con la distancia. 

Para crear un emisor, antes puedes crear una forma en la que el sonido se proyectará desde dicho emisor, que puede ser un cono o una forma omnidireccional. La clase [**AudioNodeEmitterShape**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterShape) proporciona métodos estáticos para crear cada una de estas formas. A continuación, crea un modelo de caída. Esto define cómo disminuye el volumen del audio desde el emisor a medida que aumenta la distancia desde el oyente. El método [**CreateNatural**](/uwp/api/windows.media.audio.audionodeemitterdecaymodel.createnatural) crea un modelo de caída que emula la caída natural del sonido mediante un modelo disminución de distancia cuadrado. Por último, crea un objeto [**AudioNodeEmitterSettings**](/uwp/api/Windows.Media.Audio.AudioNodeEmitterSettings). Actualmente, este objeto solo se usa para habilitar y deshabilitar la atenuación de Doppler basada en velocidad del audio del emisor. Llama al constructor [**AudioNodeEmitter**](/uwp/api/windows.media.audio.audionodeemitter.-ctor) pasando los objetos de inicialización que acabas de crear. De manera predeterminada, el emisor se coloca en el origen, pero puedes establecer la posición de dicho emisor con la propiedad [**Position**](/uwp/api/windows.media.audio.audionodeemitter.position).

> [!NOTE]
> Los emisores del nodo de audio solo pueden procesar audio en formato mono con una frecuencia de muestreo de 48 kHz. Si se intenta usar audio estéreo o audio con una frecuencia de muestreo diferente, se producirá una excepción.

Asigna el emisor a un nodo de audio cuando lo crees mediante el método de creación sobrecargado correspondiente al tipo de nodo que desees. En este ejemplo, se usa [**CreateFileInputNodeAsync**](/uwp/api/windows.media.audio.audiograph.createfileinputnodeasync) para crear un nodo de entrada de archivos desde un archivo especificado y el objeto [**AudioNodeEmitter**](/uwp/api/Windows.Media.Audio.AudioNodeEmitter) que se desee asociar con el nodo.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetCreateEmitter":::

La clase [**AudioDeviceOutputNode**](/uwp/api/Windows.Media.Audio.AudioDeviceOutputNode) que envía audio desde el gráfico al usuario tiene un objeto de oyente, al que se accede con la propiedad [**Listener**](/uwp/api/windows.media.audio.audiodeviceoutputnode.listener), que representa la ubicación, la orientación y la velocidad del usuario en el espacio 3D. Las posiciones de todos los emisores en el gráfico son relativas a la posición y la orientación del objeto emisor. De manera predeterminada, el oyente se encuentra en el origen (0,0,0) orientado de frente en el eje Z, pero puedes establecer su posición y su orientación con las propiedades [**Position**](/uwp/api/windows.media.audio.audionodelistener.position) y [**Orientation**](/uwp/api/windows.media.audio.audionodelistener.orientation).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetListener":::

Puedes actualizar la ubicación, la velocidad y la dirección de los emisores en el tiempo de ejecución para simular el movimiento de un origen de audio a través del espacio 3D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateEmitter":::

También puedes actualizar la ubicación, la velocidad y la orientación del objeto Listener en el tiempo de ejecución para simular el movimiento del usuario en el espacio 3D.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/AudioGraph/cs/MainPage.xaml.cs" id="SnippetUpdateListener":::

De manera predeterminada, el audio espacial se calcula mediante el algoritmo de función de transferencia relativo a la cabeza (HRTF) de Microsoft para atenuar el audio en función de su forma, su velocidad y su posición en relación con el oyente. Puedes establecer la propiedad [**SpatialAudioModel**](/uwp/api/windows.media.audio.audionodeemitter.spatialaudiomodel)**en FoldDown** para usar un método simple de mezcla estéreo de simulación de audio espacial, que es menos preciso pero también requiere menos recursos de CPU y memoria.

## <a name="see-also"></a>Consulte también
- [Reproducción de multimedia](media-playback.md)
 

 
