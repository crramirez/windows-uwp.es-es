---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: En este artículo se muestra cómo usar las API del espacio de nombres Windows.Media.Audio para crear gráficos de audio para escenarios de enrutamiento, mezcla y procesamiento de audio.
title: Gráficos de audio
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0211e451c3e700da34d24e39a5045f9e046020a8
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8787319"
---
# <a name="audio-graphs"></a>Gráficos de audio



Este artículo muestra cómo usar las API en el espacio de nombres [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341) para crear gráficos para escenarios de enrutamiento, mezcla y procesamiento de audio.

Un *gráfico de audio* es un conjunto de nodos de audio interconectados a través del cual fluyen datos de audio. 

- Los *nodos de entrada de audio* suministran datos de audio al gráfico desde dispositivos de entrada de audio, archivos de audio o un código personalizado. 

- Los *nodos de salida de audio* son el destino para el audio procesado por el gráfico. El audio del gráfico se puede enrutar a dispositivos de salida de audio, archivos de audio o código personalizado. 

- Los *nodos de submezcla* toman audio desde uno o varios nodos y lo combinan en una sola salida que puede enrutarse a otros nodos del gráfico. 

Después de que se hayan creado todos los nodos y se hayan configurado las conexiones entre ellos, simplemente inicia el gráfico de audio y los datos de audio fluirán desde los nodos de entrada a través de los nodos de submezcla hasta llegar a los nodos de salida. Este modelo permite escenarios como la grabación desde el micrófono de un dispositivo a un archivo de audio, la reproducción de audio desde un archivo al altavoz de un dispositivo o la mezcla de audio desde varias fuentes de forma rápida y fácil de implementar.

Se habilitan escenarios adicionales con la incorporación de efectos de audio al gráfico de audio. Todos los nodos de un gráfico de audio se pueden rellenar con cero o más efectos de audio que realizan el procesamiento de audio en el audio que pasa a través del nodo. Existen varios efectos integrados como eco, ecualizador, limitación y reverberación que se pueden anexar a un nodo de audio con unas pocas líneas de código. También puedes crear tus propios efectos de audio personalizados que funcionan exactamente igual que los efectos integrados.

> [!NOTE]
> La [muestra de AudioGraph para UWP](http://go.microsoft.com/fwlink/?LinkId=619481) implementa el código analizado en esta introducción. Puedes descargar la muestra para ver el código en contexto o para usarla como punto de partida para tu propia aplicación.

## <a name="choosing-windows-runtime-audiograph-or-xaudio2"></a>Selección de AudioGraph o XAudio2 de Windows Runtime

Las API de gráfico de audio de Windows Runtime ofrecen una funcionalidad que se puede implementar mediante las [XAudio2 APIs](https://msdn.microsoft.com/library/windows/desktop/hh405049) (API de XAudio2) basadas en COM. Las siguientes son características del marco de gráficos de audio de Windows Runtime que difieren de XAudio2.

API de gráficos de audio de Windows Runtime:

-   Son significativamente más fáciles de usar que XAudio2.
-   Pueden usarse desde C#, además de ser compatibles con C++.
-   Pueden usar archivos de audio directamente, incluidos formatos de archivo comprimido. XAudio2 solo funciona en búferes de audio y no proporciona ninguna funcionalidad de E/S de archivo.
-   Usar la canalización de audio de latencia baja en Windows 10.
-   Admite la conmutación automática de los extremos cuando se usan los parámetros de extremo predeterminados. Por ejemplo, si el usuario cambia del altavoz de un dispositivo a unos auriculares, el audio se redirige automáticamente a la entrada nueva.

## <a name="audiograph-class"></a>Clase AudioGraph

La clase [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176) es el elemento primario de todos los nodos que conforman el gráfico. Usa este objeto para crear instancias de todos los tipos de nodo de audio. Crea una instancia de la clase **AudioGraph** inicializando un objeto [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) que contenga opciones de configuración para el gráfico y, a continuación, llama a [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216). El valor [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273) devuelto proporciona acceso a un gráfico de audio creado o proporciona un valor de error si se produce un error en la creación del gráfico de audio.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Todos los tipos de nodo de audio se crean mediante los métodos Crear\* de la clase **AudioGraph**.
-   El método [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) hace que el gráfico de audio comience a procesar datos de audio. El método [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) detiene el procesamiento de audio. Cada nodo del gráfico se puede iniciar y detener de forma independiente mientras se está ejecutando el gráfico, pero ningún nodo está activo cuando se detiene el gráfico. [**ResetAllNodes**](https://msdn.microsoft.com/library/windows/apps/dn914242) hace que todos los nodos del gráfico descarten cualquier dato almacenado actualmente en sus búferes de audio.
-   El evento [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241) se produce cuando el gráfico está iniciando el procesamiento de un nuevo cuanto de datos de audio. El evento [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) se produce cuando se completa el procesamiento de un cuanto.

-   La única propiedad [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) requerida es [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724). La especificación de este valor permite que el sistema optimice la canalización de audio para la categoría especificada.
-   El tamaño del cuanto del gráfico de audio determina el número de muestras que se procesan a la vez. De manera predeterminada, el tamaño del cuanto es 10 ms, basado en la velocidad de muestra predeterminada. Si se especifica un tamaño de cuanto personalizado estableciendo la propiedad [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205), también debes establecer la propiedad [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) en **ClosestToDesired** o se omitirá el valor proporcionado. Si se usa este valor, el sistema elige un tamaño de cuanto lo más cerca posible al especificado. Para determinar el tamaño real del cuanto, comprueba [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243) de **AudioGraph** después de que se haya creado.
-   Si solo vas a usar el gráfico de audio con los archivos y no pretendes una salida a un dispositivo de audio, se recomienda que uses el tamaño de cuanto predeterminado no estableciendo la propiedad [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205).
-   La propiedad [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) determina la cantidad de procesamiento que realiza el dispositivo de representación principal en la salida del gráfico de audio. La configuración **Default** permite que el sistema use el procesamiento de audio predeterminado para la categoría de representación de audio especificada. Este procesamiento puede mejorar considerablemente el sonido de audio en algunos dispositivos, especialmente en los dispositivos móviles con altavoces pequeños. La configuración **Raw** puede mejorar el rendimiento al reducir la cantidad de procesamiento de señal realizado, pero puede producir una calidad de sonido inferior en algunos dispositivos.
-   Si el [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) se establece en **LowestLatency**, el gráfico de audio usará automáticamente **Raw** para [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522).
- A partir de Windows 10, versión 1803, puedes establecer la propiedad [**AudioGraphSettings.MaxPlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiographsettings.maxplaybackspeedfactor) para establecer un valor máximo usado para las propiedades [**AudioFileInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiofileinputnode.playbackspeedfactor), [**AudioFrameInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeinputnode.playbackspeedfactor) y [**MediaSourceInputNode.PlaybackSpeedFactor**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceinputnode.playbackspeedfactor). Cuando un gráfico de audio admite un factor de velocidad de reproducción mayor que 1, el sistema debe asignar memoria adicional para mantener un búfer de datos de audio suficiente. Por este motivo, al establecer **MaxPlaybackSpeedFactor** en el valor mínimo requerido por la aplicación se reducirá el consumo de memoria de la aplicación. Si tu aplicación solo va a reproducir contenido a velocidad normal, se recomienda que establezcas MaxPlaybackSpeedFactor en 1.
-   [**EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523) determina el formato de audio usado por el gráfico. Se admiten los formatos flotantes de 32 bits únicamente.
-   [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) establece el dispositivo de representación principal para el gráfico de audio. Si no estableces esto, se usa el dispositivo predeterminado del sistema. El dispositivo de representación principal se usa para calcular los tamaños de cuanto de otros nodos en el gráfico. Si no hay ningún dispositivo de representación de audio presente en el sistema, se producirá un error en la creación del gráfico de audio.

Puedes permitir que el gráfico de audio use el dispositivo de representación de audio predeterminado o que use la clase [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obtener una lista dispositivos de representación de audio disponibles del sistema mediante una llamada a [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) y pasando el selector de dispositivos de representación de audio devuelto por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). Puedes elegir uno de los objetos **DeviceInformation** devueltos mediante programación o mostrar la interfaz de usuario para que el usuario pueda seleccionar un dispositivo y, a continuación, usarlo para establecer la propiedad [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524).

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  <a name="device-input-node"></a>Nodo de entrada de dispositivo

Un nodo de entrada de dispositivo envía audio al gráfico desde un dispositivo de captura de audio conectado al sistema, como un micrófono. Crea un objeto [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) que use el dispositivo de captura de audio predeterminado del sistema mediante una llamada a [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218). Proporciona una [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724) para permitir que el sistema optimice la canalización de audio para la categoría especificada.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Si quieres especificar un dispositivo de captura de audio específico para el nodo de entrada de dispositivo, puedes usar la clase [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obtener una lista de los dispositivos de captura de audio disponibles del sistema mediante una llamada a [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) y pasando el selector de dispositivo de representación de audio devuelto por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). Puedes elegir uno de los objetos **DeviceInformation** devueltos mediante programación o mostrar la interfaz de usuario para que el usuario pueda seleccionar un dispositivo y luego pasarlo a [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218).

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  <a name="device-output-node"></a>Nodo de dispositivo de salida

Un nodo de dispositivo de salida envía audio del gráfico a un dispositivo de representación de audio, como los altavoces o auriculares. Crea un [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098) llamando a [**CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525). El nodo de salida usa el [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) del gráfico de audio.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  <a name="file-input-node"></a>Nodo de entrada de archivo

Un nodo de entrada de archivo te permite enviar datos desde un archivo de audio al gráfico. Crea un [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) llamando a [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226).

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Los nodos de entrada de archivos admiten los siguientes formatos de archivo: mp3, wav, wma y m4a.
-   Establece la propiedad [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) para especificar el desplazamiento de tiempo en el archivo donde debe comenzar la reproducción. Si esta propiedad es null, se usa el comienzo del archivo. Establece la propiedad [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) para especificar el desplazamiento de tiempo en el archivo donde debe finalizar la reproducción. Si esta propiedad es null, se usa el final del archivo. El valor de tiempo de inicio debe ser menor que el valor de tiempo final y el valor de hora final debe ser menor o igual a la duración del archivo de audio, que puede determinarse comprobando el valor de la propiedad [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116).
-   Busca una posición en el archivo de audio llamando a [**Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127) y especificando el desplazamiento de tiempo en el archivo al que se moverá la posición de reproducción. El valor especificado no debe superar el rango de [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) y [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118). Obtén la posición de reproducción actual del nodo con la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124) de solo lectura.
-   Habilita la función de repetición del archivo de audio estableciendo la propiedad [**LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120). Cuando no sea null, este valor indica el número de veces que el archivo se reproducirá después de la reproducción inicial. Por lo tanto, por ejemplo, establecer **LoopCount** en 1 hará que el archivo se reproduzca 2 veces en total y, si se establece en 5, hará que el archivo que se reproduzca 6 veces en total. Establecer **LoopCount** en null hace que el archivo se repita indefinidamente. Para detener la repetición, establece este valor en 0.
-   Ajusta la velocidad a la que el archivo de audio se reproduce estableciendo [**PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123). Un valor de 1 indica la velocidad original del archivo, 0,5 es la velocidad media y 2 es el doble de velocidad.

##  <a name="mediasource-input-node"></a>Nodo de entrada MediaSource

La clase [**MediaSource**](https://docs.microsoft.com/uwp/api/Windows.Media.Core.MediaSource) proporciona una forma común de hacer referencia a contenido multimedia desde diferentes orígenes y se expone un modelo común para acceder a datos multimedia, independientemente del formato multimedia subyacente que podría ser un archivo en disco, una secuencia o un origen de red de streaming. Un nodo [**MediaSourceAudioInputNode](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode) permite dirigir datos de audio desde un **MediaSource** al archivo de gráfico. Crea un **MediaSourceAudioInputNode** llamando a [**CreateMediaSourceAudioInputNodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.createmediasourceaudioinputnodeasync#Windows_Media_Audio_AudioGraph_CreateMediaSourceAudioInputNodeAsync_Windows_Media_Core_MediaSource_), pasando un objeto **MediaSource** que representa el contenido que deseas reproducir. Se devuelve un [**CreateMediaSourceAudioInputNodeResult](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult) que puedes usar para determinar el estado de la operación comprobando la propiedad [**Status**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.status). Si el estado es **Correcto**, puedes obtener el **MediaSourceAudioInputNode** creado obteniendo acceso a la propiedad [**Node**](https://docs.microsoft.com/uwp/api/windows.media.audio.createmediasourceaudioinputnoderesult.node). En el siguiente ejemplo se muestra la creación de un nodo desde un objeto AdaptiveMediaSource que representa el streaming de contenido en la red. Para obtener más información sobre el trabajo con **MediaSource**, consulta [Elementos multimedia, listas de reproducción y pistas](media-playback-with-mediasource.md). Para obtener más información sobre el streaming de contenido multimedia a través de Internet, consulta [Streaming adaptable](adaptive-streaming.md).

[!code-cs[DeclareMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareMediaSourceInputNode)]

[!code-cs[CreateMediaSourceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateMediaSourceInputNode)]

Para recibir una notificación cuando la reproducción ha llegado al final del contenido **MediaSource**, registra un controlador para el evento [**MediaSourceCompleted**](https://docs.microsoft.com/uwp/api/windows.media.audio.mediasourceaudioinputnode.mediasourcecompleted). 

[!code-cs[RegisterMediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterMediaSourceCompleted)]

[!code-cs[MediaSourceCompleted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetMediaSourceCompleted)]

Mientras que reproducir un archivo desde un disco es probable que siempre se complete correctamente, el contenido multimedia que se transmite desde un origen de red puede presentar errores durante la reproducción debido a un cambio en la conexión de red u otros problemas que están fuera del control del gráfico de audio. Si un **MediaSource** se vuelve no reproducible durante la reproducción, el gráfico de audio generará el evento [**UnrecoverableErrorOccurred**](https://docs.microsoft.com/uwp/api/windows.media.audio.audiograph.unrecoverableerroroccurred). Puedes usar el controlador para este evento para detener y deshacerte del gráfico de audio y, a continuación, reinicializar el gráfico. 

[!code-cs[RegisterUnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetRegisterUnrecoverableError)]

[!code-cs[UnrecoverableError](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUnrecoverableError)]

##  <a name="file-output-node"></a>Nodo de salida de archivo

Un nodo de salida de archivo permite dirigir datos de audio desde el gráfico a un archivo de audio. Crea un [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133) llamando a [**CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227).

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Los nodos de salida de archivos admiten los siguientes formatos de archivo: mp3, wav, wma y m4a.
-   Es necesario llamar a [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144) para detener el procesamiento del nodo antes de llamar a [**AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140) o se generará una excepción.

##  <a name="audio-frame-input-node"></a>Nodo de entrada de fotogramas de audio

Un nodo de entrada de fotogramas de audio permite insertar datos de audio que se generan en su propio código en el gráfico de audio. Esto permite escenarios como crear un sintetizador de software personalizado. Crea un [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147) llamando a [**CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230).

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

El evento [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507) se genera cuando el gráfico de audio está listo para empezar a procesar el siguiente cuanto de datos de audio. Proporcionas los datos de audio generados de forma personalizada desde dentro del controlador a este evento.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   El objeto [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533) pasado al controlador de eventos **QuantumStarted** expone la propiedad [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534) que indica cuántas muestras el gráfico de audio necesita para rellenar el cuanto para su procesamiento.
-   Realiza una llamada a [**AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148) para pasar un objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) rellenado con datos de audio al gráfico.
- En Windows 10, versión 1803, se introdujo un nuevo conjunto de API para usar **MediaFrameReader** con datos de audio. Estas API te permiten obtener objetos **AudioFrame** desde un origen de trama multimedia, que se pueden pasar a un **FrameInputNode** con el método **AddFrame**. Para obtener más información, consulta [Procesar tramas de audio con MediaFrameReader](process-audio-frames-with-mediaframereader.md).
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **GenerateAudioData**.

Para rellenar un [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) con datos de audio, debes obtener acceso al búfer de memoria subyacente del fotograma de audio. Para ello, debes inicializar la interfaz COM **IMemoryBufferByteAccess** agregando el siguiente código dentro de tu espacio de nombres.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

El siguiente código muestra un ejemplo de implementación de un método auxiliar **GenerateAudioData** que crea un [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) y se rellena con datos de audio.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Como este método tiene acceso el búfer sin procesar subyacente a los tipos de Windows Runtime, se debe declarar con la palabra clave **unsafe**. También debes configurar el proyecto en Microsoft Visual Studio para permitir la compilación del código no seguro abriendo la página **Properties** del proyecto, haciendo clic en la página de propiedades de **Build** y seleccionando la casilla **Permitir código no seguro**.
-   Inicializa una nueva instancia de [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) en el espacio de nombres **Windows.Media** pasando el tamaño del búfer deseado al constructor. El tamaño de búfer es el número de muestras multiplicado por el tamaño de cada muestra.
-   Obtén el [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) de la trama de audio llamando a [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878).
-   Obtén una instancia de la interfaz COM [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) desde el búfer de audio realizando una llamada a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506) y conviértelo en el tipo de datos de muestra de los datos de audio.
-   Rellena el búfer con datos y devuelve el [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) para el envío en el gráfico de audio.

##  <a name="audio-frame-output-node"></a>Nodo de salida de fotogramas de audio

Un nodo de salida de fotogramas de audio permite recibir y procesar la salida de datos de audio desde el gráfico de audio con código personalizado que crees. Un escenario de ejemplo para esto es la realización de análisis de señal en la salida de audio. Crea un [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166) llamando a [**CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233).

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

El evento [**AudioGraph.QuantumStarted**](https://docs.microsoft.com/uwp/api/Windows.Media.Audio.AudioGraph.QuantumStarted) se genera cuando el gráfico de audio comienza a procesar un cuanto de datos de audio. Puedes acceder a los datos de audio desde el controlador para este evento. 

> [!NOTE]
> Si quieres recuperar las tramas de audio en una cadencia regular, sincronizadas con el gráfico de audio, llama a [AudioFrameOutputNode.GetFrame](https://docs.microsoft.com/uwp/api/windows.media.audio.audioframeoutputnode.GetFrame) desde el controlador del evento **QuantumStarted** sincrónico. El evento **QuantumProcessed** se genera asincrónicamente después de que el motor de audio ha completado el procesamiento de audio, por lo que la cadencia puede ser irregular. Por lo tanto, no debes usar el evento **QuantumProcessed** para el procesamiento sincronizado de los datos de tramas de audio.

[!code-cs[SnippetQuantumStartedFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStartedFrameOutput)]

-   Llama a [**GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171) para obtener un objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) rellenado con datos de audio del gráfico.
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **ProcessFrameOutput**.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Al igual que el ejemplo de nodo de entrada de fotogramas de audio anterior, tendrás que declarar la interfaz COM **IMemoryBufferByteAccess** y configurar el proyecto para permitir que el código no seguro tenga acceso al búfer subyacente de audio.
-   Obtén el [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) de la trama de audio llamando a [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878).
-   Obtén una instancia de la interfaz COM **IMemoryBufferByteAccess** desde el búfer de audio realizando una llamada a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a **IMemoryBufferByteAccess.GetBuffer** y conviértelo en el tipo de datos de muestra de los datos de audio.

## <a name="node-connections-and-submix-nodes"></a>Conexiones de nodo y nodos de submezcla

Todos los tipos de nodos de entrada exponen el método **AddOutgoingConnection** que enruta el audio generado por el nodo al que se pasa al método. El siguiente ejemplo conecta un [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) a un [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098), que es una configuración simple para reproducir un archivo de audio en el altavoz del dispositivo.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Puedes crear más de una conexión desde un nodo de entrada a otros nodos. El siguiente ejemplo agrega otra conexión desde [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) a un [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133). Ahora, el audio desde el archivo de audio se reproduce en el altavoz del dispositivo y también se escribe en un archivo de audio.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Los nodos de salida también pueden recibir más de una conexión desde otros nodos. En el siguiente ejemplo, se establece una conexión desde un [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) al nodo [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098). Dado que el nodo de salida tiene conexiones desde el nodo de entrada del archivo y el nodo de entrada del dispositivo, la salida contendrá una mezcla de audio de ambas fuentes. **AddOutgoingConnection** proporciona una sobrecarga que te permite especificar un valor de ganancia para la señal que pasa a través de la conexión.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Aunque los nodos de salida pueden aceptar conexiones de varios nodos, puedes crear una mezcla intermedia de señales desde uno o varios nodos antes de pasar la combinación a una salida. Por ejemplo, puedes establecer el nivel o aplicar efectos a un subconjunto de las señales de audio en un gráfico. Para llevarlo a cabo, puedes usar el elemento [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247). Puedes conectarte a un nodo de submezcla de uno o más nodos de entrada u otros nodos de submezcla. En el ejemplo siguiente, se crea un nuevo nodo de submezcla con [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236). A continuación, se agregan las conexiones desde un nodo de entrada de archivo y un nodo de salida de fotogramas al nodo de submezcla. Por último, el nodo de submezcla está conectado a un nodo de salida de archivo.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## <a name="starting-and-stopping-audio-graph-nodes"></a>Iniciar y detener los nodos del gráfico de audio

Cuando se realiza la llamada a [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244), el audio gráfico comienza el procesamiento de datos de audio. Cada tipo de nodo proporciona métodos **Start** y **Stop** que permiten que el nodo individual inicie o detenga el procesamiento de datos. Cuando se realiza una llamada a [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245), se detiene todo el procesamiento de audio en todos los nodos independientemente del estado de los nodos individuales, pero puedes establecer el estado de cada nodo mientras el gráfico de audio está detenido. Por ejemplo, podrías realizar una llamada a **Stop** en un nodo individual mientras se detiene el gráfico y después realizar una llamada a **AudioGraph.Start**, y el nodo individual permanecerá en estado de detención.

Todos los tipos de nodos exponen la propiedad **ConsumeInput** que, cuando se establece en false, permite que el nodo continúe con el procesamiento de audio, pero no permite que consuma ningún dato de audio que se envíe desde otros nodos.

Todos los tipos de nodos exponen el método **Reset**, que permite que el nodo descarte todos los datos de audio actualmente en el búfer.

## <a name="adding-audio-effects"></a>Agregar efectos de audio

La API de gráfico de audio te permite agregar efectos de audio para cada tipo de nodo en un gráfico. Los nodos de salida, nodos de entrada y nodos de submezcla pueden tener un número ilimitado de efectos de audio, limitados solo por las funcionalidades del hardware. El siguiente ejemplo muestra cómo agregar el efecto de eco integrado a un nodo de submezcla.

[!code-cs[AddEffect](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddEffect)]

-   Todos los efectos de audio implementan [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044). Cada nodo expone una propiedad **EffectDefinitions** que representa la lista de efectos aplicados a ese nodo. Agrega un efecto agregando su objeto de definición a la lista.
-   Existen varias clases de definición de efectos que se proporcionan en el espacio de nombres **Windows.Media.Audio**. Estos son:
    -   [**EchoEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914276)
    -   [**EqualizerEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914287)
    -   [**LimiterEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914306)
    -   [**ReverbEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn914313)
-   Puedes crear tus propios efectos de audio que implementen [**IAudioEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn608044) y aplicarlos a cualquier nodo en un gráfico de audio.
-   Cada tipo de nodo expone un método **DisableEffectsByDefinition** que deshabilita todos los efectos de la lista **EffectDefinitions** del nodo que se agregaron mediante la definición especificada. **EnableEffectsByDefinition** habilita los efectos con la definición especificada.

## <a name="spatial-audio"></a>Audio espacial
A partir de Windows10, versión 1607, **AudioGraph** admite audio espacial, que te permite especificar la ubicación en el espacio 3D desde la que se emite el audio de cualquier nodo de entrada o de submezcla. También puedes especificar una forma y una dirección en la que se emita el audio y una velocidad que se usará para cambiar el audio del nodo a Doppler, así como definir un modelo de caída que describa cómo se atenuará el audio con la distancia. 

Para crear un emisor, antes puedes crear una forma en la que el sonido se proyectará desde dicho emisor, que puede ser un cono o una forma omnidireccional. La clase [**AudioNodeEmitterShape**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeEmitterShape) proporciona métodos estáticos para crear cada una de estas formas. A continuación, crea un modelo de caída. Esto define cómo disminuye el volumen del audio desde el emisor a medida que aumenta la distancia desde el oyente. El método [**CreateNatural**](https://msdn.microsoft.com/library/windows/apps/mt711740) crea un modelo de caída que emula la caída natural del sonido mediante un modelo disminución de distancia cuadrado. Por último, crea un objeto [**AudioNodeEmitterSettings**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeEmitterSettings). Actualmente, este objeto solo se usa para habilitar y deshabilitar la atenuación de Doppler basada en velocidad del audio del emisor. Llama al constructor [**AudioNodeEmitter**](https://msdn.microsoft.com/library/windows/apps/mt694324.aspx) pasando los objetos de inicialización que acabas de crear. De manera predeterminada, el emisor se coloca en el origen, pero puedes establecer la posición de dicho emisor con la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeEmitter.Position).

> [!NOTE]
> Los emisores del nodo de audio solo pueden procesar audio en formato mono con una frecuencia de muestreo de 48kHz. Si se intenta usar audio estéreo o audio con una frecuencia de muestreo diferente, se producirá una excepción.

Asigna el emisor a un nodo de audio cuando lo crees mediante el método de creación sobrecargado correspondiente al tipo de nodo que desees. En este ejemplo, se usa [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914225) para crear un nodo de entrada de archivos desde un archivo especificado y el objeto [**AudioNodeEmitter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeEmitter) que se desee asociar con el nodo.

[!code-cs[CreateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateEmitter)]

La clase [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioDeviceOutputNode) que envía audio desde el gráfico al usuario tiene un objeto de oyente, al que se accede con la propiedad [**Listener**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioDeviceOutputNode.Listener), que representa la ubicación, la orientación y la velocidad del usuario en el espacio 3D. Las posiciones de todos los emisores en el gráfico son relativas a la posición y la orientación del objeto emisor. De manera predeterminada, el oyente se encuentra en el origen (0,0,0) orientado de frente en el eje Z, pero puedes establecer su posición y su orientación con las propiedades [**Position**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeListener.Position) y [**Orientation**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeListener.Orientation).

[!code-cs[Listener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetListener)]

Puedes actualizar la ubicación, la velocidad y la dirección de los emisores en el tiempo de ejecución para simular el movimiento de un origen de audio a través del espacio 3D.

[!code-cs[UpdateEmitter](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateEmitter)]

También puedes actualizar la ubicación, la velocidad y la orientación del objeto Listener en el tiempo de ejecución para simular el movimiento del usuario en el espacio 3D.

[!code-cs[UpdateListener](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetUpdateListener)]

De manera predeterminada, el audio espacial se calcula mediante el algoritmo de función de transferencia relativo a la cabeza (HRTF) de Microsoft para atenuar el audio en función de su forma, su velocidad y su posición en relación con el oyente. Puedes establecer la propiedad [**SpatialAudioModel**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Audio.AudioNodeEmitter.SpatialAudioModel) **en FoldDown** para usar un método simple de mezcla estéreo de simulación de audio espacial, que es menos preciso pero también requiere menos recursos de CPU y memoria.

## <a name="see-also"></a>Consulta también
- [Reproducción de contenido multimedia](media-playback.md)
 

 




