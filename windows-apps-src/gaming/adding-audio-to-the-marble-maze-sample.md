---
author: mtoepke
title: Agregar audio a la muestra de Marble Maze
description: "En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con audio y se muestra la aplicación de dichas prácticas en MarbleMaze."
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.sourcegitcommit: c663692e31a62fdf40df9d706070d0d2ce0e1cdd
ms.openlocfilehash: 0b2a0cb240431a49ef2bdb82a188f3dcb0294fc5

---

# Agregar audio al ejemplo de Marble Maze


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con audio y se muestra la aplicación de dichas prácticas en MarbleMaze. Marble Maze utiliza Microsoft Media Foundation para cargar recursos de audio de un archivo, y XAudio2 para mezclar y reproducir y audio, y para aplicar efectos al audio.

Marble Maze reproduce música en segundo plano, y también utiliza sonidos de juego para indicar los eventos del juego, como el momento en que una canica toca la pared. Una parte importante de la implementación es que Marble Maze utiliza un efecto de reverberación, o eco, para simular el sonido de una canica al rebotar. La implementación del efecto de reverberación provoca ecos para que te lleguen con mayor rapidez y más volumen en espacios pequeños; los ecos son más silenciosos y te llegan más lentamente en espacios más grandes.

> 
            **Nota**   El código de ejemplo correspondiente a este documento se encuentra en el [ejemplo de juego de Marble Maze con DirectX](http://go.microsoft.com/fwlink/?LinkId=624011).

A continuación se indican algunos de los puntos principales que se tratan en este documento para trabajar con audio en el juego:

-   Considera la posibilidad de utilizar Media Foundation para descodificar los activos de audio y XAudio2 para reproducir audio. No obstante, si dispones de un mecanismo de carga de activos de audio que funcione en una aplicación para Plataforma universal de Windows (UWP), puedes usarlo.
-   Un gráfico de audio contiene una voz de origen para cada sonido activo, cero o más voces de submezcla, y una voz de procesamiento. Las voces de origen pueden incorporarse en las voces de submezcla y/o en la voz de procesamiento. Las voces de submezcla se incorporan en otras voces de submezcla o en la voz de procesamiento.
-   Si los archivos de música de fondo son de gran tamaño, considera la posibilidad de transmitir la música en búferes más pequeños de modo que se utilice menos memoria.
-   Si tiene sentido hacerlo, detén la reproducción de audio cuando la aplicación pierda el foco o la visibilidad, o se suspenda. Reanuda la reproducción cuando la aplicación recupere el foco de nuevo, pase a ser visible o se reanude.
-   Establece las categorías de audio para reflejar el rol de cada sonido. Por ejemplo, generalmente usas **AudioCategory\_GameMedia** para el audio de fondo de los juegos y **AudioCategory\_GameEffects** para los efectos de sonido.
-   Controla los cambios de los dispositivos, incluidos los auriculares, liberando y creando de nuevo todos los recursos e interfaces de audio.
-   Determina si es necesario comprimir los archivos de audio si es un requisito minimizar el espacio en disco y reducir los costes. En caso contrario, puedes dejar el audio sin comprimir para que se cargue con mayor rapidez.

## Introducción a XAudio2 y Microsoft Media Foundation


XAudio2 es una biblioteca de audio de bajo nivel para Windows que admite específicamente el audio de los juegos. Proporciona procesamiento de señal digital (DSP) y un motor de gráficos de audio para juegos. XAudio2 es una ampliación de sus predecesores, DirectSound y XAudio, ya que admite nuevas tendencias informáticas, tales como arquitecturas de coma flotante SIMD y audio HD. Asimismo, responde a las demandas más complejas de procesamiento de texto de los juegos de hoy en día.

En el documento [XAudio2 Key Concepts](https://msdn.microsoft.com/library/windows/desktop/ee415764) (conceptos clave de XAudio2) se explican los conceptos clave para usar XAudio2. En pocas palabras, los conceptos son los siguientes:

-   La interfaz [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) es el núcleo del motor XAudio2. Marble Maze usa esta interfaz para crear voces y recibir notificaciones cuando el dispositivo de salida cambia o tiene un error.
-   Una voz procesa, ajusta y reproduce datos de audio.
-   Una voz de origen es una colección de canales de audio (mono, 5.1, etc.) y representa una secuencia de datos de audio. En XAudio2, una voz de origen es el punto en el que comienza el procesamiento de audio. Generalmente, los datos de sonido se cargan de una fuente externa, como un archivo o una red, y se envían a una voz de origen. Marble Maze utiliza [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) para cargar datos de sonido de archivos. Más adelante en este documento se habla sobre Media Foundation.
-   Una voz de submezcla procesa datos de audio. Este procesamiento puede incluir cambiar la secuencia de audio o combinar varias secuencias en una. Marble Maze utiliza submezclas para crear el efecto de reverberación.
-   Una voz de procesamiento combina datos de voces de origen y de submezcla, y envía datos al hardware de audio.
-   Un gráfico de audio contiene una voz de origen para cada sonido activo, cero o más voces de submezcla, y solo una voz de procesamiento.
-   Una función de devolución de llamada informa al código de cliente que se ha producido algún tipo de evento en una voz o en un objeto de motor. El uso de devoluciones de llamada te permite reutilizar la memoria cuando XAudio2 ha finalizado con un búfer, reaccionar cuando el dispositivo de audio cambia (por ejemplo, cuando conectas o desconectas los auriculares), y muchas cosas más. En el apartado [Control de cambios de auriculares y dispositivos](#phones) más adelante en este documento se describe cómo se usa este mecanismo en Marble Maze para controlar los cambios de dispositivo.

Marble Maze usa dos motores de audio (es decir, dos objetos [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908)) para procesar el audio. Uno de los motores procesa la música de fondo y el otro procesa los sonidos de los juegos.

Marble Maze también debe crear una voz de procesamiento para cada motor. Recuerda que un motor de procesamiento combina secuencias de audio en una secuencia y envía dicha secuencia al hardware de audio. La secuencia de música de fondo, una voz de origen, emite datos a una voz de procesamiento y a dos voces de submezcla. Las voces de submezcla realizan el efecto de reverberación.

Media Foundation es una biblioteca multimedia que admite muchos formatos de audio y vídeo. XAudio2 y Media Foundation se complementan entre sí. Marble Maze utiliza Media Foundation para cargar activos de audio de un archivo y utiliza XAudio2 para reproducir el audio. No es necesario que utilices Media Foundation para cargar activos de audio. Si dispones de un mecanismo de carga de activos de audio que funcione en una aplicación para Plataforma universal de Windows (UWP), puedes usarlo.

Para más información sobre XAudio2, consulta la [Guía de programación](https://msdn.microsoft.com/library/windows/desktop/ee415737). Para obtener más información sobre Media Foundation, consulta [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197).

## Inicialización de recursos de audio


Marble Maze utiliza un archivo de audio de Windows Media (.wma) para la música de fondo y archivos WAV (.wav) para los sonidos de los juegos. Estos formatos se admiten en Media Foundation. Si bien el formato de archivo .wav es compatible de forma nativa con XAudio2, un juego debe analizar el formato de archivo manualmente para rellenar las estructuras de datos apropiadas de XAudio2. Marble Maze utiliza Media Foundation para trabajar con mayor facilidad con los archivos .wav. Para obtener una lista completa de los formatos multimedia admitidos por Media Foundation, consulta el tema [Supported Media Formats in Media Foundation](https://msdn.microsoft.com/library/windows/desktop/dd757927) (Formatos multimedia admitidos en Media Foundation). Marble Maze no usa formatos distintos de audio en tiempo de diseño y de ejecución, y no usa la compatibilidad con la compresión ADPCM de XAudio2. Para obtener más información sobre la compresión ADPCM en XAudio2, consulta [ADPCM Overview](https://msdn.microsoft.com/library/windows/desktop/ee415711) (Introducción a ADPCM).

El método **Audio::CreateResources**, que se llama desde **MarbleMaze::CreateDeviceIndependentResources**, carga secuencias de audio de un archivo, inicializa los objetos del motor de XAudio2 y crea las voces de origen, submezcla y procesamiento.

###  Crear los motores de XAudio2

Recuerda que Marble Maze crea un objeto [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) para representar cada motor de audio que usa. Para crear un motor de audio, llama a la función [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212). En el siguiente ejemplo se muestra cómo Marble Maze crea el motor de audio que procesa música de fondo.

```cpp
DX::ThrowIfFailed(
    XAudio2Create(&m_musicEngine)
    );
```

Marble Maze realiza un paso similar para crear el motor de audio que reproduce sonidos de los juegos.

La forma de trabajar con la interfaz [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) en una aplicación para UWP difiere de una aplicación de escritorio en dos puntos. En primer lugar, no tienes que llamar a **CoInitializeEx** antes de llamar a [**XAudio2Create**](https://msdn.microsoft.com/library/windows/desktop/ee419212). Además, **IXAudio2** ya no admite la enumeración de dispositivos. Para obtener información sobre cómo enumerar dispositivos de audio, consulta [Enumeración de dispositivos](https://msdn.microsoft.com/library/windows/apps/hh464977).

###  Crear las voces de procesamiento

En el siguiente ejemplo se muestra cómo el método **Audio::CreateResources** crea la voz de procesamiento para la música de fondo. La llamada a [**IXAudio2::CreateMasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/hh405048) especifica dos canales de entrada. De este modo, se simplifica la lógica para el efecto de reverberación. La especificación **XAUDIO2\_DEFAULT\_SAMPLERATE** indica al motor de audio que use la frecuencia de muestreo especificada en el Panel de control Sonido. En este ejemplo, **m\_musicMasteringVoice** es un objeto [**IXAudio2MasteringVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415912).

```cpp
// This sample plays the equivalent of background music, which we tag on the  
// mastering voice as AudioCategory_GameMedia. In ordinary usage, if we were  
// playing the music track with no effects, we could route it entirely through 
// Media Foundation. Here, we are using XAudio2 to apply a reverb effect to the 
// music, so we use Media Foundation to decode the data then we feed it through 
// the XAudio2 pipeline as a separate Mastering Voice, so that we can tag it 
// as Game Media. We default the mastering voice to 2 channels to simplify  
// the reverb logic.
DX::ThrowIfFailed(
    m_musicEngine->CreateMasteringVoice(
        &m_musicMasteringVoice, 
        2, 
        48000, 
        0, 
        nullptr, 
        nullptr, 
        AudioCategory_GameMedia
        )
);
```

El método **Audio::CreateResources** realiza un paso similar para crear la voz de procesamiento para los sonidos de los juegos, excepto que especifica **AudioCategory\_GameEffects** para el parámetro *StreamCategory*, que es el valor predeterminado. Marble Maze especifica **AudioCategory\_GameMedia** para la música de fondo de modo que los usuarios puedan escucharla desde otra aplicación mientras ejecutan el juego. Cuando se ejecuta una aplicación de música, Windows silencia las voces creadas por la opción **AudioCategory\_GameMedia**. El usuario sigue escuchando los sonidos del juego porque se crean mediante la opción **AudioCategory\_GameEffects**. Para obtener más información sobre las categorías de audio, consulta la enumeración [**AUDIO\_STREAM\_CATEGORY**](https://msdn.microsoft.com/library/windows/desktop/hh404178).

###  Crear el efecto de reverberación

Para cada voz, puedes usar XAudio2 para crear secuencias de efectos que procesen audio. Una secuencia de este tipo se conoce como cadena de efectos. Utiliza las cadenas de efectos cuando quieras aplicar uno o más efectos a una voz. Las cadenas de efectos pueden ser destructivas; es decir, cada uno de los efectos de la cadena puede sobrescribir el búfer de audio. Esta propiedad es importante porque XAudio2 no garantiza que los búferes de salida se inicialicen con silencio. Los objetos de efectos se representan en XAudio2 mediante objetos de procesamiento de audio multi-plataforma (XAPO). Para obtener más información sobre XAPO, consulta la [introducción a XAPO](https://msdn.microsoft.com/library/windows/desktop/ee415735).

Para crear una cadena de efectos, sigue estos pasos:

1.  Crea el objeto de efecto.
2.  Rellenar una estructura [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) con datos de efectos.
3.  Rellena una estructura [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) con datos.
4.  Aplica la cadena de efectos a una voz.
5.  Rellena una estructura de parámetros de efectos y aplícala al efecto.
6.  Deshabilita o habilita el efecto siempre que sea apropiado.

La clase **Audio** define el método **CreateReverb** para crear la cadena de efectos que implementa la reverberación. Este método llama a la función [**XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213) para crear un objeto [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915), que actúa como voz de submezcla para el efecto de reverberación.

```cpp
DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

La estructura [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) contiene información sobre un XAPO que se usa en una cadena de efectos, por ejemplo, el número de destino de canales de salida. El método **Audio::CreateReverb** crea un objeto **XAUDIO2\_EFFECT\_DESCRIPTOR** que está establecido en el estado Deshabilitado, usa dos canales de salida y hace referencia al objeto [**IXAudio2SubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415915) para el efecto de reverberación. El objeto **XAUDIO2\_EFFECT\_DESCRIPTOR** se inicia en el estado Deshabilitado porque el juego debe establecer los parámetros antes de que el efecto empiece a modificar los sonidos del juego. Marble Maze utiliza dos canales de salida para simplificar la lógica para el efecto de reverberación

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Si la cadena de efectos tiene varios efectos, cada efecto necesita un objeto. La estructura [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) contiene la matriz de objetos [**XAUDIO2\_EFFECT\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419236) que participan en el efecto. En el siguiente ejemplo se muestra cómo el método **Audio::CreateReverb** especifica el efecto para implementar la reverberación.

```cpp
soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

El método **Audio::CreateReverb** llamada al método [**IXAudio2::CreateSubmixVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418608) para crear la voz de submezcla para el efecto. Especifica el objeto [**XAUDIO2\_EFFECT\_CHAIN**](https://msdn.microsoft.com/library/windows/desktop/ee419235) del parámetro *pEffectChain* para asociar la cadena de efectos a la voz. Marble Maze también especifica dos canales de salida y una frecuencia de muestreo de 48kilohercios. Se optó por esta velocidad de muestra porque representaba un equilibrio entre la calidad de audio y la cantidad de procesamiento de la CPU necesaria. Una frecuencia de muestreo superior hubiera necesitado más procesamiento de la CPU sin que ello reportara beneficios notables en cuanto a la calidad.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> 
            **Sugerencia**: Si quieres adjuntar una cadena de efectos existente a una voz de submezcla existente, o quieres sustituir la cadena de efectos actual, usa el método [**IXAudio2Voice::SetEffectChain**](https://msdn.microsoft.com/library/windows/desktop/ee418594).

 

El método [**Audio::XAudio2CreateReverb**](https://msdn.microsoft.com/library/windows/desktop/ee419213) llama a [**IXAudio2Voice::SetEffectParameters**](https://msdn.microsoft.com/library/windows/desktop/ee418595) para establecer parámetros adicionales asociados al efecto. Este método usa una estructura de parámetros que es específica del efecto. Se inicializa un objeto [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224) (que contiene los parámetros del efecto para reverberación) en el método **Audio::Initialize**, porque cada efecto de reverberación comparte los mismos parámetros. En el siguiente ejemplo se muestra cómo el método **Audio::Initialize** inicializa los parámetros de reverberación para la reverberación en proximidad.

```cpp
m_reverbParametersSmall.ReflectionsDelay = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_DELAY;
m_reverbParametersSmall.ReverbDelay = XAUDIO2FX_REVERB_DEFAULT_REVERB_DELAY;
m_reverbParametersSmall.RearDelay = XAUDIO2FX_REVERB_DEFAULT_REAR_DELAY;
m_reverbParametersSmall.PositionLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionRight = XAUDIO2FX_REVERB_DEFAULT_POSITION;
m_reverbParametersSmall.PositionMatrixLeft = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.PositionMatrixRight = XAUDIO2FX_REVERB_DEFAULT_POSITION_MATRIX;
m_reverbParametersSmall.EarlyDiffusion = 4;
m_reverbParametersSmall.LateDiffusion = 15;
m_reverbParametersSmall.LowEQGain = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_GAIN;
m_reverbParametersSmall.LowEQCutoff = XAUDIO2FX_REVERB_DEFAULT_LOW_EQ_CUTOFF;
m_reverbParametersSmall.HighEQGain = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_GAIN;
m_reverbParametersSmall.HighEQCutoff = XAUDIO2FX_REVERB_DEFAULT_HIGH_EQ_CUTOFF;
m_reverbParametersSmall.RoomFilterFreq = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_FREQ;
m_reverbParametersSmall.RoomFilterMain = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_MAIN;
m_reverbParametersSmall.RoomFilterHF = XAUDIO2FX_REVERB_DEFAULT_ROOM_FILTER_HF;
m_reverbParametersSmall.ReflectionsGain = XAUDIO2FX_REVERB_DEFAULT_REFLECTIONS_GAIN;
m_reverbParametersSmall.ReverbGain = XAUDIO2FX_REVERB_DEFAULT_REVERB_GAIN;
m_reverbParametersSmall.DecayTime = XAUDIO2FX_REVERB_DEFAULT_DECAY_TIME;
m_reverbParametersSmall.Density = XAUDIO2FX_REVERB_DEFAULT_DENSITY;
m_reverbParametersSmall.RoomSize = XAUDIO2FX_REVERB_DEFAULT_ROOM_SIZE;
m_reverbParametersSmall.WetDryMix = XAUDIO2FX_REVERB_DEFAULT_WET_DRY_MIX;
m_reverbParametersSmall.DisableLateField = TRUE;
```

En este ejemplo se usan los valores predeterminados para la mayor parte de los parámetros de reverberación, pero se establece **DisableLateField** en TRUE para especificar la reverberación en proximidad, **EarlyDiffusion** en 4 para simular superficies planas cercanas y **LateDiffusion** en 15 para simular superficies distantes muy difusas. Las superficies planas cercanas crean ecos para llegar a ti con mayor rapidez y más volumen; las superficies distantes crean ecos más silenciosos que te llegan más lentamente. Puedes experimentar con los valores de reverberación para obtener el efecto deseado en tu juego, o bien puedes usar la función **ReverbConvertI3DL2ToNative** para usar parámetros I3DL2 (Interactive 3D Audio Rendering Guidelines Level 2.0) estándares del sector.

En el siguiente ejemplo se muestra cómo **Audio::CreateReverb** establece los parámetros de reverberación. El parámetro parameters es un objeto [**XAUDIO2FX\_REVERB\_PARAMETERS**](https://msdn.microsoft.com/library/windows/desktop/ee419224).

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

El método **Audio::CreateReverb** termina al habilitar el efecto si la marca **enableEffect** está establecida y al establecer su matriz de volumen y salida. Esta parte establece el volumen en completo (1.0) y luego establece la matriz de volumen en silencio para las entradas izquierda y derecha y para los altavoces de salida izquierdo y derecho. Lo hacemos porque más adelante otro código se encadena entre las dos reverberaciones (simulando la transición de estar cerca de una pared a pasar a estar en una gran habitación) o silencia ambas reverberaciones si es necesario. Cuando más adelante se reactiva el audio de la vía de reverberación, el juego establece una matriz de {1.0f, 0.0f, 0.0f, 1.0f} para direccionar la salida de reverberación izquierda a la entrada izquierda de la voz de procesamiento y la salida de reverberación derecha a la entrada derecha de la voz de procesamiento.

```cpp
if (enableEffect)
{
    DX::ThrowIfFailed(
        (*newSubmix)->EnableEffect(0)
        );    
}

DX::ThrowIfFailed(
    (*newSubmix)->SetVolume (1.0f)
    );

float outputMatrix[4] = {0, 0, 0, 0};
DX::ThrowIfFailed(
    (*newSubmix)->SetOutputMatrix(masteringVoice, 2, 2, outputMatrix)
    );
```

Marble Maze llama al método **CreateReverb** cuatro veces: dos veces para la música de fondo y dos veces para los sonidos del juego. En el siguiente ejemplo se muestra cómo Marble Maze llama al método **CreateReverb** para la música de fondo.

```cpp
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersSmall, 
    &m_musicReverbVoiceSmallRoom, 
    true
    );
CreateReverb(
    m_musicEngine, 
    m_musicMasteringVoice, 
    &m_reverbParametersLarge, 
    &m_musicReverbVoiceLargeRoom, 
    true
    );
```

Para obtener una lista de las posibles orígenes de efectos que puedes usar con XAudio2, consulta [XAudio2 Audio Effects](https://msdn.microsoft.com/library/windows/desktop/ee415756) (Efectos de audio de XAudio2).

### Cargar datos de audio desde un archivo

Marble Maze define la clase **MediaStreamer**, que usa Media Foundation para cargar recursos de audio a partir de un archivo. Marble Maze usa un objeto **MediaStreamer** para cargar cada uno de los archivos de audio.

Marble Maze llama al método **MediaStreamer::Initialize** para inicializar cada una de las secuencias de audio. A continuación se describe cómo el método **Audio::CreateResources** llama a **MediaStreamer::Initialize** para inicializar la secuencia de audio de la música de fondo:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

El método **MediaStreamer::Initialize** empieza llamando a la función [**MFStartup**](https://msdn.microsoft.com/library/windows/desktop/ms702238) para inicializar Media Foundation.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```


            A continuación, **MediaStreamer::Initialize** llama a [**MFCreateSourceReaderFromURL**](https://msdn.microsoft.com/library/windows/desktop/dd388110) para crear un objeto [**IMFSourceReader**](https://msdn.microsoft.com/library/windows/desktop/dd374655). Un objeto **IMFSourceReader** lee datos multimedia del archivo especificado por la dirección URL.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

Después, el método **MediaStreamer::Initialize** crea un objeto [**IMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms704850) para describir el formato de la secuencia de audio. Un formato de audio usa dos tipos: un tipo principal y un subtipo. El tipo principal define el formato general del medio, como vídeo, audio, script, etc. El subtipo define el formato, como PCM, ADPCM o WMA. El método **MediaStreamer::Initialize** usa el método [**IMFMediaType::SetGUID**](https://msdn.microsoft.com/library/windows/desktop/bb970530) para especificar el tipo principal como audio (**MFMediaType\_Audio**) y el secundario como audio PCM sin comprimir (**MFAudioFormat\_PCM**). El método [**IMFSourceReader::SetCurrentMediaType**](https://msdn.microsoft.com/library/windows/desktop/bb970432) asocia el tipo de medio con el lector de secuencias.

```cpp
// Set the decoded output format as PCM. 
// XAudio2 on Windows can process PCM and ADPCM-encoded buffers. 
// When this sample uses Media Foundation, it always decodes into PCM.

DX::ThrowIfFailed(
    MFCreateMediaType(&mediaType)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
    );

DX::ThrowIfFailed(
    mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
    );

DX::ThrowIfFailed(
    m_reader->SetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, 0, mediaType.Get())
    );
```

Posteriormente, el método **MediaStreamer::Initialize** obtiene el formato de medio de salida completo de Media Foundation y llama a la función [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) para convertir el tipo de medio de audio a una estructura [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799). La estructura **WAVEFORMATEX** define el formato de datos de audio Waveform. Marble Maze usa esta estructura para crear las voces de origen y para aplicar el filtro de paso bajo al sonido de canica rodando.

```cpp
// Get the complete WAVEFORMAT from the Media Type.
DX::ThrowIfFailed(
    m_reader->GetCurrentMediaType(MF_SOURCE_READER_FIRST_AUDIO_STREAM, &outputMediaType)
    );

uint32 formatSize = 0;
WAVEFORMATEX* waveFormat;
DX::ThrowIfFailed(
    MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &formatSize)
    );
CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
CoTaskMemFree(waveFormat);
```

> 
            **Importante**   La función [**MFCreateWaveFormatExFromMFMediaType**](https://msdn.microsoft.com/library/windows/desktop/ms702177) usa **CoTaskMemAlloc** para asignar el objeto [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799). Por consiguiente, asegúrate de llamar a **CoTaskMemFree** cuando termines de usar este objeto.

 

El método **MediaStreamer::Initialize** termina al calcular la longitud de la secuencia, m\_*maxStreamLengthInBytes*, en bytes. Para hacerlo, llama al método [**IMFSourceReader::IMFSourceReader::GetPresentationAttribute**](https://msdn.microsoft.com/library/windows/desktop/dd374662) para obtener la duración de la secuencia de audio en unidades de 100 nanosegundos, convierte la duración en secciones y luego multiplica por la velocidad media de transferencia de datos en bytes por segundo. Marble Maze utiliza más adelante este valor para asignar el búfer que contiene cada uno de los sonidos del juego.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );
LONGLONG duration = var.uhVal.QuadPart;
// The duration is in 100ns units; convert the value to seconds. 
double durationInSeconds = (duration / static_cast<double>(10000000)); 
m_maxStreamLengthInBytes = 
    static_cast<unsigned int>(durationInSeconds * m_waveFormat.nAvgBytesPerSec);

// Round up the buffer size to the nearest four bytes.
m_maxStreamLengthInBytes = (m_maxStreamLengthInBytes + 3) / 4 * 4;
```

### Creación de las voces de origen

Marble Maze crea voces de origen XAudio2 para reproducir cada uno de sus sonidos de juego y música en voces de origen. La clase **Audio** define un objeto [**IXAudio2SourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee415914) para la música de fondo y una matriz de objetos **SoundEffectData** para contener los sonidos del juego. La estructura **SoundEffectData** contiene el objeto **IXAudio2SourceVoice** para un efecto y también define otros datos relacionados con el efecto, como el búfer de audio. Audio.h define la enumeración **SoundEvent**. Marble Maze usa esta enumeración para identificar cada uno de los sonidos del juego. La clase Audio también usa esta enumeración para indexar la matriz de objetos **SoundEffectData**.

```cpp
enum SoundEvent
{
    RollingEvent        = 0,
    FallingEvent        = 1,
    CollisionEvent      = 2,
    CheckpointEvent     = 3,
    MenuChangeEvent     = 4,
    MenuSelectedEvent   = 5,
    LastSoundEvent,
};
```

En la siguiente tabla se muestra la relación entre cada uno de estos valores, el archivo que contiene los datos del sonido asociado y una breve descripción de qué representa cada sonido. Los archivos de audio se encuentran en la carpeta \\Media\\Audio.

| Valor SoundEvent  | Nombre del archivo      | Descripción                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Se reproduce cuando gira la canica.                              |
| FallingEvent      | MarbleFall.wav | Se reproduce cuando la canica sale del laberinto.               |
| CollisionEvent    | MarbleHit.wav  | Se reproduce cuando la canica colisiona con el laberinto.           |
| CheckpointEvent   | Checkpoint.wav | Se reproduce cuando la canica pasa por un punto de control.         |
| MenuChangeEvent   | MenuChange.wav | Se reproduce cuando el usuario del juego cambia el elemento de menú actual. |
| MenuSelectedEvent | MenuSelect.wav | Se reproduce cuando el usuario del juego selecciona un elemento de menú.           |

 

En el siguiente ejemplo se muestra cómo el método **Audio::CreateResources** crea la voz de origen para la música de fondo. El método [**IXAudio2::CreateSourceVoice**](https://msdn.microsoft.com/library/windows/desktop/ee418607) crea y configura una voz de origen. Usa una estructura [**WAVEFORMATEX**](https://msdn.microsoft.com/library/windows/hardware/ff538799) que define el formato de los búferes de audio que se envían a la voz. Como se ha indicado anteriormente, Marble Maze usa el formato PCM. La estructura [**XAUDIO2\_SEND\_DESCRIPTOR**](https://msdn.microsoft.com/library/windows/desktop/ee419244) define la voz de destino de otra voz y especifica si debe usarse un filtro. Marble Maze llama a la función **Audio::SetSoundEffectFilter** con el fin de usar filtros para cambiar el sonido de la bola a medida que rueda. La estructura [**XAUDIO2\_VOICE\_SENDS**](https://msdn.microsoft.com/library/windows/desktop/ee419246) define el conjunto de voces para recibir datos de una única voz de salida. Marble Maze envía datos de la voz de origen a la voz de procesamiento (para la parte seca, o no modificada, de un sonido de reproducción) y a las dos voces de submezcla que implementan la parte húmeda, o reverberante, del sonido de reproducción.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_soundEffectMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_soundEffectReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_soundEffectReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;

// The rolling sound can have pitch shifting and a low-pass filter. 
if (sound == RollingEvent)
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            XAUDIO2_VOICE_USEFILTER,
            2.0f,
            &m_voiceContext,
            &sends)
        );
}
else
{
    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateSourceVoice(
            &m_soundEffects[sound].m_soundEffectSourceVoice,
            &(soundEffectStream.GetOutputWaveFormatEx()),
            0,
            1.0f,
            &m_voiceContext,
            &sends)
        );
}
```

## Reproducción de la música de fondo


Una voz de origen se crea en estado de parada. Marble Maze inicia la música de fondo en el bucle del juego. La primera llamada a **MarbleMaze::Update** llamada a **Audio::Start** para iniciar la música de fondo.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

El método **Audio::Start** llama a [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) para empezar a procesar la voz de origen para la música de fondo.

```cpp
void Audio::Start()
{     
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicSourceVoice->Start(0);

    if SUCCEEDED(hr) {
        m_isAudioStarted = true;
    }
    else
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

La voz de origen pasa dichos datos de audio a la siguiente etapa del gráfico de audio. En el caso de Marble Maze, la siguiente etapa contiene dos voces de submezcla que aplican al audio los dos efectos de reverberación. Una voz de submezcla aplica una reverberación cercana de campo lejano; la segunda aplica una reverberación lejana de campo lejano. La cantidad en que cada voz de submezcla contribuye a la mezcla final se determina a partir del tamaño y la forma de la sala. La reverberación de campo cercano contribuye más cuando la bola está cerca de una pared o en una sala pequeña, y la reverberación de campo lejano contribuye más cuando la bola se encuentra en un espacio grande. Esta técnica produce un efecto de eco más real a medida que la canica se desplaza por el laberinto. Para obtener más información sobre cómo Marble Maze implementa este efecto, consulta **Audio::SetRoomSize** y **Physics::CalculateCurrentRoomSize** en el código fuente de MarbleMaze.

> 
            **Nota**  En un juego en el que la mayor parte de espacios tienen más o menos el mismo tamaño, puedes usar un modelo de reverberación más básico. Por ejemplo, puedes usar una configuración de reverberación para todas las salas, o bien puedes crear una configuración de reverberación predefinida para cada sala.

 

El método **Audio::CreateResources** usa Media Foundation para cargar la música de fondo. Sin embargo, en este punto la voz de origen no tiene datos de audio con los que trabajar. Además, dado que la música de fondo entra en un bucle, la voz de origen debe actualizarse periódicamente con datos para que la música siga reproduciéndose. Para que la voz de origen vaya teniendo datos, el bucle del juego actualiza los búferes de audio cada fotograma. El método **MarbleMaze::Render** llama a **Audio::Render** para procesar el búfer de audio de música de fondo. **Audio::Render** define una matriz de tres búferes de audio, **m\_audioBuffers**. Cada búfer contiene 64KB (65536bytes) de datos. El bucle lee datos del objeto de Media Foundation y escribe dichos datos en la voz de origen hasta que esta tiene tres búferes en cola.

> 
            **Precaución**  Si bien Marble Maze usa un búfer de 64 KB para los datos de música, es posible que tengas que usar un búfer de mayor o menor tamaño. El tamaño depende de los requisitos del juego.

 

```cpp
void Audio::Render()
{
    if (m_engineExperiencedCriticalError)
    {
        m_engineExperiencedCriticalError = false;
        ReleaseResources();
        Initialize();
        CreateResources();
        Start();
        if (m_engineExperiencedCriticalError)
        {
            return;
        }
    }

    try
    {
        bool streamComplete;
        XAUDIO2_VOICE_STATE state;
        uint32 bufferLength;
        XAUDIO2_BUFFER buf = {0};

        // Use MediaStreamer to stream the buffers.
        m_musicSourceVoice->GetState(&state);
        while (state.BuffersQueued <= MAX_BUFFER_COUNT - 1)
        {
            streamComplete = m_musicStreamer.GetNextBuffer(
                m_audioBuffers[m_currentBuffer], 
                STREAMING_BUFFER_SIZE, 
                &bufferLength
                );

            if (bufferLength > 0)
            {
                buf.AudioBytes = bufferLength;
                buf.pAudioData = m_audioBuffers[m_currentBuffer];
                buf.Flags = (streamComplete) ? XAUDIO2_END_OF_STREAM : 0;
                buf.pContext = 0;
                DX::ThrowIfFailed(
                    m_musicSourceVoice->SubmitSourceBuffer(&buf)
                    );

                m_currentBuffer++;
                m_currentBuffer %= MAX_BUFFER_COUNT;
            }

            if (streamComplete)
            {
                // Loop the stream.
                m_musicStreamer.Restart();
                break;
            }

            m_musicSourceVoice->GetState(&state);
        }
    }
    catch(...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

El bucle también controla el momento en que el objeto de Media Foundation llega al final de la secuencia. En este caso, llama al método [**MediaStreamer::OnClockRestart**](https://msdn.microsoft.com/library/windows/desktop/ms697215) para restablecer la posición del origen de audio.

```cpp
void MediaStreamer::Restart()
{
    if (m_reader == nullptr)
    {
        return;
    }

    PROPVARIANT var = {0};
    var.vt = VT_I8;

    DX::ThrowIfFailed(
        m_reader->SetCurrentPosition(GUID_NULL, var)
        );
}
```

Si quieres implementar el bucle de audio para un único búfer (o para la totalidad de un sonido cargado por completo en memoria), puedes establecer el valor del campo **LoopCount** en **XAUDIO2\_LOOP\_INFINITE** al inicializar el sonido. Marble Maze usa esta técnica para reproducir el sonido de rodadura de la canica.

```cpp
if(sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

No obstante, para la música de fondo, Marble Maze administra los búferes directamente a fin de tener mayor control sobre la cantidad de memoria utilizada. Si los archivos de música son grandes, puedes secuenciar los datos de música en búferes más pequeños. Si haces esto te ayudará a equilibrar el tamaño de la memoria con la frecuencia de la capacidad del juego para procesar y hacer streaming de datos de audio.

> 
            **Sugerencia**  Si el juego tiene una velocidad de fotogramas baja o variable, procesar el audio en el subproceso principal puede generar pausas o cortes inesperados en el audio porque el motor de audio no tiene suficientes datos de audio en el búfer con los que trabajar. Si el juego es sensible a este problema, considera la posibilidad de procesar audio en un subproceso independiente que no realice representación. Este enfoque es especialmente útil en equipos con varios procesadores, porque el juego puede usar procesadores inactivos.

 

##  Reaccionar ante eventos del juego


La clase **MarbleMaze** proporciona métodos como **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch** y **SetSoundEffectFilter** para habilitar el control del juego cuando se reproducen y se detienen los sonidos y para controlar las propiedades de sonido como el volumen y tono. Por ejemplo, si la canica se sale del laberinto, el método **MarbleMaze::Update** llama al método **Audio::PlaySoundEffect** para que reproduzca el sonido **FallingEvent**.

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

El método **Audio::PlaySoundEffect** llama al método [**IXAudio2SourceVoice::Start**](https://msdn.microsoft.com/library/windows/desktop/ee418471) para comenzar la reproducción del sonido. Si ya se ha llamado al método **IXAudio2SourceVoice::Start**, la reproducción no se inicia de nuevo. 
            De este modo, **Audio::PlaySoundEffect** utiliza una lógica personalizada para determinados sonidos.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError) {
        // If there's an error, then we'll recreate the engine on the next  
        // render pass. 
        return;
    }

    SoundEffectData* soundEffect = &m_soundEffects[sound];
    HRESULT hr = soundEffect->m_soundEffectSourceVoice->Start();
    if FAILED(hr)
    {
        m_engineExperiencedCriticalError = true;
        return;
    }

    // For one-off voices, submit a new buffer if there's none queued up, 
    // and allow up to two collisions to be queued up. 
    if(sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};
        soundEffect->m_soundEffectSourceVoice->GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);
        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click right away. 
        // Note that stopping and then flushing could cause a glitch due to the 
        // waveform not being at a zero-crossing, but due to the nature of the sound  
        // (fast and 'clicky'), we don't mind. 
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();
            soundEffect->m_soundEffectSourceVoice->SubmitSourceBuffer(&soundEffect->m_audioBuffer);
            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Para sonidos que no sean de rodadura, el método **Audio::PlaySoundEffect** llama a [**IXAudio2SourceVoice::GetState**](https://msdn.microsoft.com/library/windows/desktop/hh405047) para determinar el número de búferes que reproduce la voz de origen. Este método llama a [**IXAudio2SourceVoice::SubmitSourceBuffer**](https://msdn.microsoft.com/library/windows/desktop/ee418473) para agregar los datos de audio del sonido a la cola de entrada de la voz si no hay ningún búfer activo. El método **Audio::PlaySoundEffect** también permite la reproducción del sonido de colisión dos veces seguidas. Esto ocurre, por ejemplo, cuando la canica colisiona con una esquina del laberinto.

Tal como ya hemos descrito, la clase Audio usa la marca **XAUDIO2\_LOOP\_INFINITE** cuando inicializa el sonido para el evento de rodadura. El sonido inicia una reproducción en bucle la primera vez que se llama a **Audio::PlaySoundEffect** para este evento. Para simplificar la lógica de reproducción del sonido de rodadura, Marble Maze silencia el sonido en lugar de detenerlo. A medida que la canica cambia de velocidad, Marble Maze cambia el tono y el volumen del sonido para que tenga un efecto más real. A continuación se muestra cómo el método **MarbleMaze::Update** actualiza el tono y el volumen de la canica a medida que cambia su velocidad, y cómo silencia el sonido estableciendo su volumen en cero cuando la canica se detiene.

```cpp
// Play the roll sound only if the marble is actually rolling. 
if (ci.isRollingOnFloor && volume > 0)
{
    if (!m_audio.IsSoundEffectStarted(RollingEvent))
    {
        m_audio.PlaySoundEffect(RollingEvent);
    }

    // Update the volume and pitch by the velocity.
    m_audio.SetSoundEffectVolume(RollingEvent, volume);
    m_audio.SetSoundEffectPitch(RollingEvent, pitch);

    // The rolling sound has at most 8000Hz sounds, so we linearly  
    // ramp up the low-pass filter the faster we go. 
    // We also reduce the Q-value of the filter, starting with a  
    // relatively broad cutoff and get progressively tighter.
    m_audio.SetSoundEffectFilter(
        RollingEvent, 
        600.0f + 8000.0f * volume,
        XAUDIO2_MAX_FILTER_ONEOVERQ - volume*volume
        );
}
else
{
    m_audio.SetSoundEffectVolume(RollingEvent, 0);
}
```

## Reacción a eventos de suspensión y reanudación


En el documento sobre la estructura de la aplicación Marble Maze se describe cómo Marble Maze admite la suspensión y reanudación. Cuando se suspende el juego, el juego pone el audio en pausa. Cuando se reanuda el juego, el juego reanuda el audio en el punto en el que lo paró. Esto se hace para seguir el procedimiento recomendado que indica no usar recursos cuando sabemos que no son necesarios.

Cuando se suspende el juego, se llama al método **Audio::SuspendAudio**. Este método llama al método [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628) para detener todo el audio. Aunque **IXAudio2::StopEngine** detiene inmediatamente toda la salida del audio, mantiene el gráfico de audio y sus parámetros de efectos (por ejemplo, el efecto de reverberación que se aplica cuando la canica rebota).

```cpp
// Uses the IXAudio2::StopEngine method to stop all audio immediately.  
// It leaves the audio graph untouched, which preserves all effect parameters   
// and effect histories (like reverb effects) voice states, pending buffers,  
// cursor positions and so on. 
// When the engines are restarted, the resulting audio will sound as if it had  
// never been stopped except for the period of silence. 
void Audio::SuspendAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    if (m_isAudioStarted)
    {
        m_musicEngine->StopEngine();
        m_soundEffectEngine->StopEngine();
    }
    m_isAudioStarted = false;
}
```

Cuando se reanuda el juego, se llama al método **Audio::ResumeAudio**. Este método usa el método [**IXAudio2::StartEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418626) para reiniciar el audio. Dado que la llamada a [**IXAudio2::StopEngine**](https://msdn.microsoft.com/library/windows/desktop/ee418628) conserva el gráfico de audio y sus parámetros de efectos, la salida de audio se reanuda en el punto en que se detuvo.

```cpp
// Restarts the audio streams. A call to this method must match a previous call  
// to SuspendAudio. This method causes audio to continue where it left off. 
// If there is a problem with the restart, the m_engineExperiencedCriticalError  
// flag is set. The next call to Render will recreate all the resources and  
// reset the audio pipeline. 
void Audio::ResumeAudio()
{
    if (m_engineExperiencedCriticalError)
    {
        return;
    }

    HRESULT hr = m_musicEngine->StartEngine();
    HRESULT hr2 = m_soundEffectEngine->StartEngine();

    if (FAILED(hr) || FAILED(hr2))
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

## Control de cambios de auriculares y dispositivos


Marble Maze utiliza devoluciones de llamada del motor para controlar los errores del motor XAudio2, como cuando cambia el dispositivo de audio. Una causa muy probable de un cambio de dispositivo puede ser cuando el usuario del juego conecta o desconecta los auriculares. Te recomendamos que implementes la devolución de llamada del motor que controla los cambios de dispositivo. En caso contrario, el juego dejará de reproducir el sonido cuando el usuario conecte o desconecte los auriculares, hasta que se reinicie el juego.

Audio.h define la clase **AudioEngineCallbacks**. Esta clase implementa la interfaz [**IXAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910).

```cpp
class AudioEngineCallbacks: public IXAudio2EngineCallback
{
private: 
    Audio* m_audio;

public :
    AudioEngineCallbacks(){};
    void Initialize(Audio* audio);

    // Called by XAudio2 just before an audio processing pass begins.
    void _stdcall OnProcessingPassStart(){};

    // Called just after an audio processing pass ends.
    void  _stdcall OnProcessingPassEnd(){};

    // Called when a critical system error causes XAudio2
    // to be closed and restarted. The error code is given in Error.
    void  _stdcall OnCriticalError(HRESULT Error);
};
```

La interfaz [**XAudio2EngineCallback**](https://msdn.microsoft.com/library/windows/desktop/ee415910) permite que el código reciba una notificación cuando se producen eventos de procesamiento de audio y cuando el motor tiene un error crítico. Para registrar las devoluciones de llamada, Marble Maze llama al método [**IXAudio2::RegisterForCallbacks**](https://msdn.microsoft.com/library/windows/desktop/ee418620) después de crear el objeto [**IXAudio2**](https://msdn.microsoft.com/library/windows/desktop/ee415908) del motor de música.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze no requiere notificación cuando se inicia o finaliza el procesamiento del audio. Por consiguiente, implementa el método [**IXAudio2EngineCallback::OnProcessingPassStart**](https://msdn.microsoft.com/library/windows/desktop/ee418463) y el método [**IXAudio2EngineCallback::OnProcessingPassEnd**](https://msdn.microsoft.com/library/windows/desktop/ee418462) para no hacer nada. En el caso del método [**IXAudio2EngineCallback::OnCriticalError**](https://msdn.microsoft.com/library/windows/desktop/ee418461), Marble Maze llama al método **SetEngineExperiencedCriticalError**, que establece la marca **m\_engineExperiencedCriticalError**.

```cpp
// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Cuando se produce un error crítico, el procesamiento de audio se detiene y todas las llamadas adicionales a XAudio2 generan un error. Para que te puedas recuperar de esta situación, debes liberar la instancia de XAudio2 y crear una nueva. El método **Audio::Render**, que se llama desde el bucle del juego en cada fotograma, primero comprueba la marca **m\_engineExperiencedCriticalError**. Si esta marca está establecida, la borra, libera la instancia actual de XAudio2, inicializa los recursos y luego inicia la música de fondo.

```cpp
if (m_engineExperiencedCriticalError)
{
    m_engineExperiencedCriticalError = false;
    ReleaseResources();
    Initialize();
    CreateResources();
    Start();
    if (m_engineExperiencedCriticalError)
    {
        return;
    }
}
```

Marble Maze también usa la marca **m\_engineExperiencedCriticalError** para protegerse frente a llamadas a XAudio2 si no hay ningún dispositivo disponible. Por ejemplo, el método **MarbleMaze::Update** no procesa el audio para los eventos de rodadura o colisión si está establecida esta marca. La aplicación intenta reparar el motor de audio en cada fotograma si es necesario. No obstante, la marca **m\_engineExperiencedCriticalError** puede estar siempre establecida si el equipo no tiene ningún dispositivo de audio o si se han desconectado los auriculares y no hay ningún otro dispositivo de audio disponible.

> 
            **Precaución**   Como regla, no ejecutes operaciones de bloqueo en el cuerpo de una devolución de llamada del motor. Si lo haces, generarás problemas de rendimiento. Marble Maze establece una marca en la devolución de llamada **OnCriticalError** y luego controla el error durante la fase de procesamiento normal de audio. Para obtener más información sobre las devoluciones de llamada de XAudio2, consulta [XAudio2 Callbacks](https://msdn.microsoft.com/library/windows/desktop/ee415745) (Devoluciones de llamada de XAudio2).

 

## Temas relacionados


* [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Desarrollar Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 







<!--HONumber=Jun16_HO4-->


