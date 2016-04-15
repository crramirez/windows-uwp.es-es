---
ms.assetid: CB924E17-C726-48E7-A445-364781F4CCA1
description: Este artículo muestra cómo usar las API en el espacio de nombres Windows.Media.Audio para crear gráficos para escenarios de enrutamiento, mezcla y procesamiento de audio.
title: Gráficos de audio
---

# Gráficos de audio

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este artículo muestra cómo usar las API en el espacio de nombres [**Windows.Media.Audio**](https://msdn.microsoft.com/library/windows/apps/dn914341) para crear gráficos para escenarios de enrutamiento, mezcla y procesamiento de audio.

Un gráfico de audio es un conjunto de nodos de audio interconectados a través de que fluyen los datos de audio. Los nodos de entrada de audio suministran los datos de audio al gráfico desde los dispositivos de entrada de audio, archivos de audio o desde un código personalizado. Los nodos de salida de audio son el destino para el audio procesado por el gráfico. Se pueden enrutar el audio del gráfico a los dispositivos de salida de audio, archivos de audio o código personalizado. El último tipo de nodo es un nodo de submezcla que toma audio desde uno o varios nodos y los combina en una sola salida que pueden enrutarse a otros nodos en el gráfico. Después de que se hayan creado todos los nodos y configurado las conexiones entre ellos, simplemente inicia el gráfico de audio y los datos de audio fluyen desde los nodos de entrada, a través de los nodos de submezcla, a los nodos de salida. Este modelo permite escenarios como la grabación desde el micrófono de un dispositivo a un archivo de audio, la reproducción de audio desde un archivo al altavoz de un dispositivo o la mezcla de audio desde varias fuentes de forma rápida y fácil de implementar.

Se habilitan escenarios adicionales con la incorporación de efectos de audio al gráfico de audio. Todos los nodos de un gráfico de audio se pueden rellenar con cero o más efectos de audio que realizan el procesamiento de audio en el audio que pasa a través del nodo. Existen varios efectos integrados como eco, ecualizador, limitación y reverberación que se pueden anexar a un nodo de audio con unas pocas líneas de código. También puedes crear tus propios efectos de audio personalizados que funcionan exactamente igual que los efectos integrados.

**Nota**  
La [muestra de AudioGraph para UWP](http://go.microsoft.com/fwlink/?LinkId=619481) implementa el código analizado en esta introducción. Puedes descargar la muestra para ver el código en contexto o para usarla como punto de partida para tu propia aplicación.

## Selección de AudioGraph o XAudio2 de Windows Runtime

Las API de gráfico de audio de Windows Runtime ofrecen una funcionalidad que se puede implementar mediante [API de Audio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) basadas en COM. Las siguientes son características del marco de gráfico de audio de Windows Runtime que difieren de XAudio2.

-   Las API de gráfico de audio de Windows Runtime son mucho más fáciles de usar que XAudio2.
-   Las API de gráfico de audio de Windows Runtime pueden usarse desde C# -, además de ser compatibles con C++.
-   Las API de gráfico de audio de Windows Runtime puede directamente usar archivos de audio, incluidos los formatos de archivo comprimido. XAudio2 solo funciona en búferes de audio y no proporciona ninguna funcionalidad de E/S de archivo.
-   Las API de gráfico de audio de Windows Runtime pueden usar la canalización de audio de latencia baja de Windows 10.
-   Las API de gráfico de audio de Windows Runtime admiten la conmutación de extremos automática cuando se usan los parámetros de extremo predeterminados. Por ejemplo, si el usuario cambia del altavoz de un dispositivo a unos auriculares, el audio se redirige automáticamente a la entrada nueva.

## Clase AudioGraph

La clase [**AudioGraph**](https://msdn.microsoft.com/library/windows/apps/dn914176) es el elemento primario de todos los nodos que conforman el gráfico. Usa este objeto para crear instancias de todos los tipos de nodo de audio. Crea una instancia de la clase **AudioGraph** inicializando un objeto [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185), que contiene opciones de configuración para el gráfico y, a continuación, realizando una llamada a [**AudioGraph.CreateAsync**](https://msdn.microsoft.com/library/windows/apps/dn914216). El valor [**CreateAudioGraphResult**](https://msdn.microsoft.com/library/windows/apps/dn914273) devuelto proporciona acceso a un gráfico de audio creado o proporciona un valor de error si se produce un error en la creación del gráfico de audio.

[!code-cs[DeclareAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareAudioGraph)]

[!code-cs[InitAudioGraph](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetInitAudioGraph)]

-   Todos los tipos de nodo de audio se crean mediante los métodos Crear\* de la clase **AudioGraph**.
-   El método [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244) hace que el gráfico de audio comience a procesar datos de audio. El método [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245) detiene el procesamiento de audio. Cada nodo del gráfico se puede iniciar y detener de forma independiente mientras se está ejecutando el gráfico, pero ningún nodo está activo cuando se detiene el gráfico. [**ResetAllNodes**](https://msdn.microsoft.com/library/windows/apps/dn914242) hace que todos los nodos del gráfico descarten cualquier dato actualmente almacenado en sus búferes de audio.
-   El evento [**QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn914241) se produce cuando el gráfico está iniciando el procesamiento de un nuevo cuanto de datos de audio. El evento [**QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) se produce cuando se completa el procesamiento de un cuanto.

-   La única propiedad [**AudioGraphSettings**](https://msdn.microsoft.com/library/windows/apps/dn914185) requerida es [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724). La especificación de este valor permite que el sistema optimice la canalización de audio para la categoría especificada.
-   El tamaño del cuanto del gráfico de audio determina el número de muestras que se procesan a la vez. De manera predeterminada, el tamaño del cuanto es 10 ms, basado en la velocidad de muestra predeterminada. Si se especifica un tamaño de cuanto personalizado estableciendo la propiedad [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205), también debes establecer la propiedad [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) en **ClosestToDesired** o se omitirá el valor proporcionado. Si se usa este valor, el sistema elige un tamaño de cuanto lo más cerca posible al especificado. Para determinar el tamaño real del cuanto, comprueba [**SamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914243) de **AudioGraph** después de que se haya creado.
-   Si solo vas a usar el gráfico de audio con los archivos y no pretendes una salida a un dispositivo de audio, se recomienda que uses el tamaño de cuanto predeterminado no estableciendo la propiedad [**DesiredSamplesPerQuantum**](https://msdn.microsoft.com/library/windows/apps/dn914205).
-   La propiedad [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522) determina la cantidad de procesamiento que realiza el dispositivo de representación principal en la salida del gráfico de audio. La configuración **Default** permite que el sistema use el procesamiento de audio predeterminado para la categoría de representación de audio especificada. Este procesamiento puede mejorar considerablemente el sonido de audio en algunos dispositivos, especialmente en los dispositivos móviles con altavoces pequeños. La configuración **Raw** puede mejorar el rendimiento al reducir la cantidad de procesamiento de señal realizado, pero puede producir una calidad de sonido inferior en algunos dispositivos.
-   Si el [**QuantumSizeSelectionMode**](https://msdn.microsoft.com/library/windows/apps/dn914208) se establece en **LowestLatency**, el gráfico de audio usará automáticamente **Raw** para [**DesiredRenderDeviceAudioProcessing**](https://msdn.microsoft.com/library/windows/apps/dn958522).
-   [
            **EncodingProperties**](https://msdn.microsoft.com/library/windows/apps/dn958523) determina el formato de audio usado por el gráfico. Se admiten los formatos flotantes de 32 bits únicamente.
-   [
            **PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) establece el dispositivo de representación principal para el gráfico de audio. Si no estableces esto, se usa el dispositivo predeterminado del sistema. El dispositivo de representación principal se usa para calcular los tamaños de cuanto de otros nodos en el gráfico. Si no hay ningún dispositivo de representación de audio presente en el sistema, se producirá un error en la creación del gráfico de audio.

Puedes permitir que el gráfico de audio use el dispositivo de representación de audio predeterminado o que use la clase [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obtener una lista dispositivos de representación de audio disponibles del sistema mediante una llamada a [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) y pasando el selector de dispositivos de representación de audio devuelto por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). Puedes elegir uno de los objetos **DeviceInformation** devueltos mediante programación o mostrar la interfaz de usuario para que el usuario pueda seleccionar un dispositivo y, a continuación, usarlo para establecer la propiedad [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524).

[!code-cs[EnumerateAudioRenderDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioRenderDevices)]

##  Nodo de entrada de dispositivo

Un nodo de entrada de dispositivo envía audio al gráfico desde un dispositivo de captura de audio conectado al sistema, como un micrófono. Crea un objeto [**DeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) que use el dispositivo de captura de audio predeterminado del sistema mediante una llamada a [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218). Proporciona una [**AudioRenderCategory**](https://msdn.microsoft.com/library/windows/apps/dn297724) para permitir que el sistema optimice la canalización de audio para la categoría especificada.

[!code-cs[DeclareDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceInputNode)]


[!code-cs[CreateDeviceInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceInputNode)]

Si quieres especificar un dispositivo de captura de audio específico para el nodo de entrada de dispositivo, puedes usar la clase [**Windows.Devices.Enumeration.DeviceInformation**](https://msdn.microsoft.com/library/windows/apps/br225393) para obtener una lista de los dispositivos de captura de audio disponibles del sistema mediante una llamada a [**FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) y pasando el selector de dispositivo de representación de audio devuelto por [**Windows.Media.Devices.MediaDevice.GetAudioRenderSelector**](https://msdn.microsoft.com/library/windows/apps/br226817). Puedes elegir uno de los objetos **DeviceInformation** devueltos mediante programación o mostrar la interfaz de usuario para que el usuario pueda seleccionar un dispositivo y luego pasarlo a [**CreateDeviceInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914218).

[!code-cs[EnumerateAudioCaptureDevices](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetEnumerateAudioCaptureDevices)]

##  Nodo de dispositivo de salida

Un nodo de dispositivo de salida envía audio desde el gráfico a un dispositivo de representación de audio, como los altavoces o auriculares. Crea un [**DeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098) llamando a [**CreateDeviceOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn958525). El nodo de salida usa el [**PrimaryRenderDevice**](https://msdn.microsoft.com/library/windows/apps/dn958524) del gráfico de audio.

[!code-cs[DeclareDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareDeviceOutputNode)]

[!code-cs[CreateDeviceOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateDeviceOutputNode)]

##  Nodo de entrada de archivo

Un nodo de entrada de archivo te permite enviar datos desde un archivo de audio al gráfico. Crea un [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) llamando a [**CreateFileInputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914226).

[!code-cs[DeclareFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileInputNode)]


[!code-cs[CreateFileInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileInputNode)]

-   Los nodos de entrada de archivo son compatibles con los siguientes formatos de archivo: mp3, wav, wma, m4a.
-   Establece la propiedad [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) para especificar el desplazamiento de tiempo en el archivo donde debe comenzar la reproducción. Si esta propiedad es null, se usa el comienzo del archivo. Establece la propiedad [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118) para especificar el desplazamiento de tiempo en el archivo donde debe finalizar la reproducción. Si esta propiedad es null, se usa el final del archivo. El valor de tiempo de inicio debe ser menor que el valor de tiempo final y el valor de hora final debe ser menor o igual a la duración del archivo de audio, que puede determinarse comprobando el valor de la propiedad [**Duration**](https://msdn.microsoft.com/library/windows/apps/dn914116).
-   Busca una posición en el archivo de audio llamando a [**Seek**](https://msdn.microsoft.com/library/windows/apps/dn914127) y especificando el desplazamiento de tiempo en el archivo al que se moverá la posición de reproducción. El valor especificado no debe superar el rango de [**StartTime**](https://msdn.microsoft.com/library/windows/apps/dn914130) y [**EndTime**](https://msdn.microsoft.com/library/windows/apps/dn914118). Obtén la posición de reproducción actual del nodo con la propiedad [**Position**](https://msdn.microsoft.com/library/windows/apps/dn914124) de solo lectura.
-   Habilita la función de repetición del archivo de audio estableciendo la propiedad [**LoopCount**](https://msdn.microsoft.com/library/windows/apps/dn914120). Cuando no sea null, este valor indica el número de veces que el archivo se reproducirá después de la reproducción inicial. Por lo tanto, por ejemplo, establecer **LoopCount** en 1 hará que el archivo se reproduzca 2 veces en total y, si se establece en 5, hará que el archivo que se reproduzca 6 veces en total. Establecer **LoopCount** en null hace que el archivo se repita indefinidamente. Para detener la repetición, establece este valor en 0.
-   Ajusta la velocidad a la que el archivo de audio se reproduce estableciendo [**PlaybackSpeedFactor**](https://msdn.microsoft.com/library/windows/apps/dn914123). Un valor de 1 indica la velocidad original del archivo, .5 es la velocidad media y 2 es el doble de velocidad.

##  Nodo de salida de archivo

Un nodo de salida de archivo permite dirigir datos de audio desde el gráfico a un archivo de audio. Crea un [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133) llamando a [**CreateFileOutputNodeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914227).

[!code-cs[DeclareFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFileOutputNode)]


[!code-cs[CreateFileOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFileOutputNode)]

-   Los nodos de salida de archivo son compatibles con los siguientes formatos de archivo: mp3, wav, wma, m4a.
-   Se debe llamar a [**AudioFileOutputNode.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914144) para detener el procesamiento del nodo antes de llamar a [**AudioFileOutputNode.FinalizeAsync**](https://msdn.microsoft.com/library/windows/apps/dn914140) o se generará una excepción.

##  Nodo de entrada de fotogramas de audio

Un nodo de entrada de fotogramas de audio permite insertar datos de audio que se generan en su propio código en el gráfico de audio. Esto permite escenarios como crear un sintetizador de software personalizado. Crea un [**AudioFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914147) llamando a [**CreateFrameInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914230).

[!code-cs[DeclareFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameInputNode)]


[!code-cs[CreateFrameInputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameInputNode)]

El evento [**FrameInputNode.QuantumStarted**](https://msdn.microsoft.com/library/windows/apps/dn958507) se genera cuando el gráfico de audio está listo para empezar a procesar el siguiente cuanto de datos de audio. Proporcionas los datos de audio generados de forma personalizada desde dentro del controlador a este evento.

[!code-cs[QuantumStarted](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumStarted)]

-   El objeto [**FrameInputNodeQuantumStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn958533) pasado al controlador de eventos **QuantumStarted** expone la propiedad [**RequiredSamples**](https://msdn.microsoft.com/library/windows/apps/dn958534) que indica cuántas muestras el gráfico de audio necesita para rellenar el cuanto para su procesamiento.
-   Realiza una llamada a [**AudioFrameInputNode.AddFrame**](https://msdn.microsoft.com/library/windows/apps/dn914148) para pasar un objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) rellenado con datos de audio al gráfico.
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **GenerateAudioData**.

Para rellenar un [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) con datos de audio, debes obtener acceso al búfer de memoria subyacente del fotograma de audio. Para ello, debes inicializar la interfaz COM **IMemoryBufferByteAccess** agregando el siguiente código dentro de tu espacio de nombres.

[!code-cs[ComImportIMemoryBufferByteAccess](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetComImportIMemoryBufferByteAccess)]

El siguiente código muestra un ejemplo de implementación de un método auxiliar **GenerateAudioData** que crea un [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) y se rellena con datos de audio.

[!code-cs[GenerateAudioData](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetGenerateAudioData)]

-   Dado que este método tiene acceso el búfer sin procesar subyacente a los tipos de Windows Runtime, se debe declarar con la palabra clave **unsafe**. También debes configurar el proyecto en Microsoft Visual Studio para permitir la compilación del código no seguro abriendo la página **Properties** del proyecto, haciendo clic en la página de propiedades de **Build** y seleccionando la casilla **Permitir código no seguro**.
-   Inicializa una nueva instancia de [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) en el espacio de nombres **Windows.Media** pasando el tamaño del búfer deseado al constructor. El tamaño de búfer es el número de muestras multiplicado por el tamaño de cada muestra.
-   Obtén el [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) de la trama de audio llamando a [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878).
-   Obtén una instancia de la interfaz COM [**IMemoryBufferByteAccess**](https://msdn.microsoft.com/library/windows/desktop/mt297505) desde el búfer de audio realizando una llamada a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a [**IMemoryBufferByteAccess.GetBuffer**](https://msdn.microsoft.com/library/windows/desktop/mt297506) y conviértelo en el tipo de datos de muestra de los datos de audio.
-   Rellena el búfer con datos y devuelve el [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) para el envío en el gráfico de audio.

##  Nodo de salida de fotogramas de audio

Un nodo de salida de fotogramas de audio permite recibir y procesar la salida de datos de audio desde el gráfico de audio con código personalizado que crees. Un escenario de ejemplo para esto es la realización de análisis de señal en la salida de audio. Crea un [**AudioFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914166) llamando a [**CreateFrameOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914233).

[!code-cs[DeclareFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetDeclareFrameOutputNode)]

[!code-cs[CreateFrameOutputNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateFrameOutputNode)]

El evento [**AudioGraph.QuantumProcessed**](https://msdn.microsoft.com/library/windows/apps/dn914240) se genera cuando el gráfico de audio haya terminado de procesar un cuanto de datos de audio. Puedes obtener acceso a los datos de audio desde dentro del controlador para este evento.

[!code-cs[QuantumProcessed](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetQuantumProcessed)]

-   Realiza una llamada a [**GetFrame**](https://msdn.microsoft.com/library/windows/apps/dn914171) para obtener un objeto [**AudioFrame**](https://msdn.microsoft.com/library/windows/apps/dn930871) rellenado con datos de audio del gráfico.
-   Se muestra a continuación un ejemplo de implementación del método auxiliar **ProcessFrameOutput**.

[!code-cs[ProcessFrameOutput](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetProcessFrameOutput)]

-   Al igual que el ejemplo de nodo de entrada de fotogramas de audio anterior, tendrás que declarar la interfaz COM **IMemoryBufferByteAccess** y configurar el proyecto para permitir que el código no seguro tenga acceso al búfer subyacente de audio.
-   Obtén el [**AudioBuffer**](https://msdn.microsoft.com/library/windows/apps/dn958454) de la trama de audio llamando a [**LockBuffer**](https://msdn.microsoft.com/library/windows/apps/dn930878).
-   Obtén una instancia de la interfaz COM **IMemoryBufferByteAccess** desde el búfer de audio realizando una llamada a [**CreateReference**](https://msdn.microsoft.com/library/windows/apps/dn958457).
-   Obtén un puntero para los datos del búfer de audio sin procesar mediante una llamada a **IMemoryBufferByteAccess.GetBuffer** y conviértelo en el tipo de datos de muestra de los datos de audio.

## Conexiones de nodo y nodos de submezcla

Todos los tipos de nodos de entrada exponen el método **AddOutgoingConnection** que enruta el audio generado por el nodo al que se pasa al método. El siguiente ejemplo conecta un [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) a un [**AudioDeviceOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914098), que es una configuración simple para reproducir un archivo de audio en el altavoz del dispositivo.

[!code-cs[AddOutgoingConnection1](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection1)]

Puedes crear más de una conexión desde un nodo de entrada a otros nodos. El siguiente ejemplo agrega otra conexión desde [**AudioFileInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914108) a un [**AudioFileOutputNode**](https://msdn.microsoft.com/library/windows/apps/dn914133). Ahora, el audio desde el archivo de audio se reproduce en el altavoz del dispositivo y también se escribe en un archivo de audio.

[!code-cs[AddOutgoingConnection2](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection2)]

Los nodos de salida también pueden recibir más de una conexión desde otros nodos. En el siguiente ejemplo, se establece una conexión desde un [**AudioDeviceInputNode**](https://msdn.microsoft.com/library/windows/apps/dn914082) al nodo [**AudioDeviceOutput**](https://msdn.microsoft.com/library/windows/apps/dn914098). Dado que el nodo de salida tiene conexiones desde el nodo de entrada del archivo y el nodo de entrada del dispositivo, la salida contendrá una mezcla de audio de ambas fuentes. **AddOutgoingConnection** proporciona una sobrecarga que te permite especificar un valor de ganancia para la señal que pasa a través de la conexión.

[!code-cs[AddOutgoingConnection3](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetAddOutgoingConnection3)]

Aunque los nodos de salida pueden aceptar conexiones de varios nodos, puedes crear una mezcla intermedia de señales desde uno o varios nodos antes de pasar la combinación a una salida. Por ejemplo, puedes establecer el nivel o aplicar efectos a un subconjunto de las señales de audio en un gráfico. Para llevarlo a cabo, puedes usar el elemento [**AudioSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914247). Puedes conectarte a un nodo de submezcla de uno o más nodos de entrada u otros nodos de submezcla. En el ejemplo siguiente, se crea un nuevo nodo de submezcla con [**AudioGraph.CreateSubmixNode**](https://msdn.microsoft.com/library/windows/apps/dn914236). A continuación, se agregan las conexiones desde un nodo de entrada de archivo y un nodo de salida de fotogramas al nodo de submezcla. Por último, el nodo de submezcla está conectado a un nodo de salida de archivo.

[!code-cs[CreateSubmixNode](./code/AudioGraph/cs/MainPage.xaml.cs#SnippetCreateSubmixNode)]

## Iniciar y detener los nodos del gráfico de audio

Cuando se realiza la llamada a [**AudioGraph.Start**](https://msdn.microsoft.com/library/windows/apps/dn914244), el audio gráfico comienza el procesamiento de datos de audio. Cada tipo de nodo proporciona métodos **Start** y **Stop** que permiten que el nodo individual inicie o detenga el procesamiento de datos. Cuando se realiza una llamada a [**AudioGraph.Stop**](https://msdn.microsoft.com/library/windows/apps/dn914245), se detiene todo el procesamiento de audio en todos los nodos independientemente del estado de los nodos individuales, pero puedes establecer el estado de cada nodo mientras el gráfico de audio está detenido. Por ejemplo, podrías realizar una llamada a **Stop** en un nodo individual mientras se detiene el gráfico y después realizar una llamada a **AudioGraph.Start**, y el nodo individual permanecerá en estado de detención.

Todos los tipos de nodos exponen la propiedad **ConsumeInput** que, cuando se establece en false, permite que el nodo continúe con el procesamiento de audio, pero no permite que consuma ningún dato de audio que se envíe desde otros nodos.

Todos los tipos de nodos exponen el método **Reset**, que permite que el nodo descarte todos los datos de audio actualmente en el búfer.

## Agregar efectos de audio

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

 

 






<!--HONumber=Mar16_HO1-->


