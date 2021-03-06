---
title: Agregar audio a la muestra de Marble Maze
description: En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con audio y se muestra la aplicación de dichas prácticas en Marble Maze.
ms.assetid: 77c23d0a-af6d-17b5-d69e-51d9885b0d44
ms.date: 10/18/2017
ms.topic: article
keywords: Windows 10, UWP, audio, juegos, ejemplo
ms.localizationpriority: medium
ms.openlocfilehash: df1be02cd962f086e52609550e9cae65e5280b71
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172139"
---
# <a name="adding-audio-to-the-marble-maze-sample"></a>Agregar audio a la muestra de Marble Maze

En este documento se describen las prácticas clave que se deben tener en cuenta al trabajar con audio y se muestra la aplicación de dichas prácticas en Marble Maze. Marble Maze usa [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) para cargar recursos de audio de archivos y [XAudio2](/windows/desktop/xaudio2/xaudio2-apis-portal) para mezclar y reproducir audio y aplicar efectos a audio.

Marble Maze reproduce música en segundo plano, y también usa sonidos de juego para indicar eventos de juego, como cuando la canica alcanza un muro. Una parte importante de la implementación es que Marble Maze utiliza un efecto de reverberación, o eco, para simular el sonido de una canica al rebotar. La implementación del efecto de reverberación provoca ecos para que te lleguen con mayor rapidez y más volumen en espacios pequeños; los ecos son más silenciosos y te llegan más lentamente en espacios más grandes.

> [!NOTE]
> El código de ejemplo que corresponde a este documento se encuentra en el [ejemplo Game Marble Maze de DirectX](https://github.com/microsoft/Windows-appsample-marble-maze).

A continuación se indican algunos de los puntos principales que se tratan en este documento para trabajar con audio en el juego:

- Considera la posibilidad de utilizar Media Foundation para descodificar los activos de audio y XAudio2 para reproducir audio. No obstante, si dispones de un mecanismo de carga de activos de audio que funcione en una aplicación para Plataforma universal de Windows (UWP), puedes usarlo.

- Un gráfico de audio contiene una voz de origen para cada sonido activo, cero o más voces de submezcla, y una voz de procesamiento. Las voces de origen pueden incorporarse en las voces de submezcla y/o en la voz de procesamiento. Las voces de submezcla se incorporan en otras voces de submezcla o en la voz de procesamiento.

- Si los archivos de música de fondo son de gran tamaño, considera la posibilidad de transmitir la música en búferes más pequeños de modo que se utilice menos memoria.

- Si tiene sentido hacerlo, detén la reproducción de audio cuando la aplicación pierda el foco o la visibilidad, o se suspenda. Reanuda la reproducción cuando la aplicación recupere el foco de nuevo, pase a ser visible o se reanude.

- Establece las categorías de audio para reflejar el rol de cada sonido. Por ejemplo, normalmente se usa **AudioCategory \_ GameMedia** para el sonido de fondo del juego y **AudioCategory \_ GameEffects** para efectos sonoros.

- Controla los cambios de los dispositivos, incluidos los auriculares, liberando y creando de nuevo todos los recursos e interfaces de audio.

- Determina si es necesario comprimir los archivos de audio si es un requisito minimizar el espacio en disco y reducir los costes. En caso contrario, puedes dejar el audio sin comprimir para que se cargue con mayor rapidez.

## <a name="introducing-xaudio2-and-microsoft-media-foundation"></a>Introducción a XAudio2 y Microsoft Media Foundation

XAudio2 es una biblioteca de audio de bajo nivel para Windows que admite específicamente el audio de los juegos. Proporciona procesamiento de señal digital (DSP) y un motor de gráficos de audio para juegos. XAudio2 es una ampliación de sus predecesores, DirectSound y XAudio, ya que admite nuevas tendencias informáticas, tales como arquitecturas de coma flotante SIMD y audio HD. Asimismo, responde a las demandas más complejas de procesamiento de texto de los juegos de hoy en día.

En el documento [XAudio2 Key Concepts](/windows/desktop/xaudio2/xaudio2-key-concepts) (conceptos clave de XAudio2) se explican los conceptos clave para usar XAudio2. En pocas palabras, los conceptos son los siguientes:

- La interfaz [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) es el núcleo del motor XAudio2. Marble Maze usa esta interfaz para crear voces y recibir notificaciones cuando el dispositivo de salida cambia o tiene un error.

- Una **voz** procesa, ajusta y reproduce datos de audio.

- Una **voz de origen** es una colección de canales de audio (mono, 5,1, etc.) y representa una secuencia de datos de audio. En XAudio2, una voz de origen es el punto en el que comienza el procesamiento de audio. Generalmente, los datos de sonido se cargan de una fuente externa, como un archivo o una red, y se envían a una voz de origen. Marble Maze utiliza [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) para cargar datos de sonido de archivos. Más adelante en este documento se habla sobre Media Foundation.

- Una **voz de submezcla** procesa los datos de audio. Este procesamiento puede incluir cambiar la secuencia de audio o combinar varias secuencias en una. Marble Maze utiliza submezclas para crear el efecto de reverberación.

- Una **voz de mastering** combina datos de voces de origen y de submezcla y envía los datos al hardware de audio.

- Un **gráfico de audio** contiene una voz de origen para cada sonido activo, cero o más voces de submezcla y solo una voz de maestro.

- Una **devolución de llamada** informa al código de cliente de que se ha producido algún evento en una voz o en un objeto de motor. El uso de devoluciones de llamada te permite reutilizar la memoria cuando XAudio2 ha finalizado con un búfer, reaccionar cuando el dispositivo de audio cambia (por ejemplo, cuando conectas o desconectas los auriculares), y muchas cosas más. [Control de los auriculares y los cambios de dispositivo](#handling-headphones-and-device-changes) más adelante en este documento se explica cómo Marble Maze usa este mecanismo para controlar los cambios en los dispositivos.

Marble Maze usa dos motores de audio (es decir, dos objetos [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2)) para procesar el audio. Un motor procesa la música de fondo y el otro motor procesa sonidos de juegos.

Marble Maze también debe crear una voz de procesamiento para cada motor. Recuerda que un motor de procesamiento combina secuencias de audio en una secuencia y envía dicha secuencia al hardware de audio. La secuencia de música de fondo, una voz de origen, emite datos a una voz de procesamiento y a dos voces de submezcla. Las voces de submezcla realizan el efecto de reverberación.

Media Foundation es una biblioteca multimedia que admite muchos formatos de audio y vídeo. XAudio2 y Media Foundation se complementan entre sí. Marble Maze usa Media Foundation para cargar recursos de audio desde archivos y usa XAudio2 para reproducir audio. No es necesario que utilices Media Foundation para cargar activos de audio. Si dispones de un mecanismo de carga de activos de audio que funcione en una aplicación para Plataforma universal de Windows (UWP), puedes usarlo. [Audio, vídeo y cámaras](../audio-video-camera/index.md) describe varias formas de implementar audio en una aplicación de UWP.

Para más información sobre XAudio2, consulta la [Guía de programación](/windows/desktop/xaudio2/programming-guide). Para obtener más información sobre Media Foundation, consulta [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk).

## <a name="initializing-audio-resources"></a>Inicialización de recursos de audio

Marble Maze usa un archivo Windows Media Audio (. WMA) para los archivos de música de fondo y WAV (. wav) para los sonidos de juegos. Estos formatos se admiten en Media Foundation. Si bien el formato de archivo .wav es compatible de forma nativa con XAudio2, un juego debe analizar el formato de archivo manualmente para rellenar las estructuras de datos apropiadas de XAudio2. Marble Maze utiliza Media Foundation para trabajar con mayor facilidad con los archivos .wav. Para obtener una lista completa de los formatos multimedia admitidos por Media Foundation, consulta el tema [Supported Media Formats in Media Foundation](/windows/desktop/medfound/supported-media-formats-in-media-foundation) (Formatos multimedia admitidos en Media Foundation). Marble Maze no usa formatos distintos de audio en tiempo de diseño y de ejecución, y no usa la compatibilidad con la compresión ADPCM de XAudio2. Para obtener más información sobre la compresión ADPCM en XAudio2, consulta [ADPCM Overview](/windows/desktop/xaudio2/adpcm-overview) (Introducción a ADPCM).

El método **audio:: CreateResources** , al que se llama desde **MarbleMazeMain:: LoadDeferredResources**, carga los flujos de audio de los archivos, inicializa los objetos del motor de XAudio2 y crea las voces de origen, submezcla y maestra.

### <a name="creating-the-xaudio2-engines"></a>Crear los motores de XAudio2

Recuerda que Marble Maze crea un objeto [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) para representar cada motor de audio que usa. Para crear un motor de audio, llame al método [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create) . En el siguiente ejemplo se muestra cómo Marble Maze crea el motor de audio que procesa música de fondo.

```cpp
// In Audio.h
class Audio
{
private:
    IXAudio2*                   m_musicEngine;
// ...
}

// In Audio.cpp
void Audio::CreateResources()
{
    try
    {
        // ...
        DX::ThrowIfFailed(
            XAudio2Create(&m_musicEngine)
            );
        // ...
    }
    // ...
}
```

Marble Maze realiza un paso similar para crear el motor de audio que reproduce sonidos de juegos.

La forma de trabajar con la interfaz [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) en una aplicación para UWP difiere de una aplicación de escritorio en dos puntos. En primer lugar, no tienes que llamar a [CoInitializeEx](/windows/desktop/api/combaseapi/nf-combaseapi-coinitializeex) antes de llamar a [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create). Además, **IXAudio2** ya no admite la enumeración de dispositivos. Para obtener información sobre cómo enumerar dispositivos de audio, consulta [Enumeración de dispositivos](/previous-versions/windows/apps/hh464977(v=win.10)).

### <a name="creating-the-mastering-voices"></a>Crear las voces de procesamiento

En el ejemplo siguiente se muestra cómo el método **audio:: CreateResources** crea la voz de maestro para la música de fondo mediante el método [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) . En este ejemplo, **m \_ musicMasteringVoice** es un objeto [IXAudio2MasteringVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2masteringvoice) . Especificamos dos canales de entrada: Esto simplifica la lógica del efecto de reverberación. 

Especificamos 48000 como la velocidad de muestra de entrada. Se optó por esta velocidad de muestra porque representaba un equilibrio entre la calidad de audio y la cantidad de procesamiento de la CPU necesaria. Una frecuencia de muestreo superior hubiera necesitado más procesamiento de la CPU sin que ello reportara beneficios notables en cuanto a la calidad. 

Por último, se especifica **AudioCategory_GameMedia** como la categoría de flujo de audio para que los usuarios puedan escuchar música desde otra aplicación cuando reproduzcan el juego. Cuando se reproduce una aplicación de música, Windows silencia las voces creadas por la opción ** \_ GameMedia de AudioCategory** . El usuario sigue escuchando sonidos porque los crea la opción ** \_ GameEffects de AudioCategory** . Para obtener más información sobre las categorías de audio, consulte la [ \_ \_ categoría secuencia de audio](/windows/desktop/api/audiosessiontypes/ne-audiosessiontypes-_audio_stream_category).

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

El método **audio:: CreateResources** realiza un paso similar para crear la voz de maestro para los sonidos del juego, salvo que especifica **AudioCategory \_ GameEffects** para el parámetro *StreamCategory* , que es el valor predeterminado.

### <a name="creating-the-reverb-effect"></a>Crear el efecto de reverberación

Para cada voz, puedes usar XAudio2 para crear secuencias de efectos que procesen audio. Una secuencia de este tipo se conoce como cadena de efectos. Utiliza las cadenas de efectos cuando quieras aplicar uno o más efectos a una voz. Las cadenas de efectos pueden ser destructivas; es decir, cada uno de los efectos de la cadena puede sobrescribir el búfer de audio. Esta propiedad es importante porque XAudio2 no garantiza que los búferes de salida se inicialicen con silencio. Los objetos de efectos se representan en XAudio2 mediante objetos de procesamiento de audio multi-plataforma (XAPO). Para obtener más información sobre XAPO, consulta la [introducción a XAPO](/windows/desktop/xaudio2/xapo-overview).

Para crear una cadena de efectos, sigue estos pasos:

1. Crea el objeto de efecto.

2. Rellenar una estructura de [ \_ \_ descriptor de efectos de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) con datos de efectos.

3. Rellenar una estructura de [ \_ \_ cadena de efectos de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) con datos.

4. Aplica la cadena de efectos a una voz.

5. Rellena una estructura de parámetros de efectos y aplícala al efecto.

6. Deshabilita o habilita el efecto siempre que sea apropiado.

La clase **Audio** define el método **CreateReverb** para crear la cadena de efectos que implementa la reverberación. Este método llama al método [XAudio2CreateReverb](/windows/desktop/api/xaudio2fx/nf-xaudio2fx-xaudio2createreverb) para crear un **objeto &lt; IUnknown &gt; ComPtr** , **soundEffectXAPO**, que actúa como la voz de submezcla del efecto de reverberación.

```cpp
Microsoft::WRL::ComPtr<IUnknown> soundEffectXAPO;

DX::ThrowIfFailed(
    XAudio2CreateReverb(&soundEffectXAPO)
    );
```

La estructura de [ \_ \_ descriptor de efectos de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) contiene información sobre un XAPO para su uso en una cadena de efectos, por ejemplo, el número objetivo de canales de salida. El método **audio:: CreateReverb** crea un objeto de ** \_ \_ descriptor de efectos de XAUDIO2** , **soundEffectdescriptor**, que se establece en el Estado deshabilitado, usa dos canales de salida y hace referencia a **soundEffectXAPO** para el efecto de reverberación. **soundEffectdescriptor** se inicia en el Estado deshabilitado porque el juego debe establecer los parámetros antes de que el efecto comience a modificar los sonidos del juego. Marble Maze utiliza dos canales de salida para simplificar la lógica para el efecto de reverberación

```cpp
soundEffectdescriptor.InitialState = false;
soundEffectdescriptor.OutputChannels = 2;
soundEffectdescriptor.pEffect = soundEffectXAPO.Get();
```

Si la cadena de efectos tiene varios efectos, cada efecto necesita un objeto. La estructura de la [ \_ \_ cadena de efectos de xaudio2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) contiene la matriz de objetos de [ \_ \_ descriptor de efectos de xaudio2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_descriptor) que participan en el efecto. En el siguiente ejemplo se muestra cómo el método **Audio::CreateReverb** especifica el efecto para implementar la reverberación.

```cpp
XAUDIO2_EFFECT_CHAIN soundEffectChain;

// ...

soundEffectChain.EffectCount = 1;
soundEffectChain.pEffectDescriptors = &soundEffectdescriptor;
```

El método **Audio::CreateReverb** llamada al método [IXAudio2::CreateSubmixVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsubmixvoice) para crear la voz de submezcla para el efecto. Especifica el objeto [de \_ \_ cadena de efectos de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_effect_chain) , **soundEffectChain**, para que el parámetro *pEffectChain* asocie la cadena de efectos con la voz. Marble Maze también especifica dos canales de salida y una frecuencia de muestreo de 48 kilohercios.

```cpp
DX::ThrowIfFailed(
    engine->CreateSubmixVoice(newSubmix, 2, 48000, 0, 0, nullptr, &soundEffectChain)
    );
```

> [!TIP]
> Si desea adjuntar una cadena de efecto existente a una voz de submezcla existente o desea reemplazar la cadena de efecto actual, use el método [IXAudio2Voice:: SetEffectChain](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectchain) .

El método **audio:: CreateReverb** llama a [IXAudio2Voice:: SetEffectParameters](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-seteffectparameters) para establecer parámetros adicionales asociados al efecto. Este método usa una estructura de parámetros que es específica del efecto. Un objeto de [ \_ \_ parámetros de reverberación XAUDIO2FX](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters) , **m_reverbParametersSmall**, que contiene los parámetros de efecto para la reverberación, se inicializa en el método **audio:: Initialize** porque cada efecto de reverberación comparte los mismos parámetros. En el siguiente ejemplo se muestra cómo el método **Audio::Initialize** inicializa los parámetros de reverberación para la reverberación en proximidad.

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

En este ejemplo se usan los valores predeterminados para la mayor parte de los parámetros de reverberación, pero se establece **DisableLateField** en TRUE para especificar la reverberación en proximidad, **EarlyDiffusion** en 4 para simular superficies planas cercanas y **LateDiffusion** en 15 para simular superficies distantes muy difusas. Las superficies planas cercanas crean ecos para llegar a ti con mayor rapidez y más volumen; las superficies distantes crean ecos más silenciosos que te llegan más lentamente. Puede experimentar con valores de reverberación para obtener el efecto deseado en el juego o usar el método **ReverbConvertI3DL2ToNative** para usar los parámetros estándar del sector I3DL2 (nivel de instrucciones de representación de audio 3d interactivo 2,0).

En el siguiente ejemplo se muestra cómo **Audio::CreateReverb** establece los parámetros de reverberación. **newSubmix** es un objeto [IXAudio2SubmixVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2submixvoice)* *. **Parameters** es un objeto de [ \_ \_ parámetros de reverberación XAUDIO2FX](/windows/desktop/api/xaudio2fx/ns-xaudio2fx-xaudio2fx_reverb_parameters)*.

```cpp
DX::ThrowIfFailed(
    (*newSubmix)->SetEffectParameters(0, parameters, sizeof(m_reverbParametersSmall))
    );
```

El método **audio:: CreateReverb** finaliza al habilitar el efecto mediante [IXAudio2Voice:: EnableEffect](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-enableeffect) si se establece la marca **EnableEffect** . También establece su volumen mediante [IXAudio2Voice:: SetVolume](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setvolume) y la matriz de salida mediante [IXAudio2Voice:: SetOutputMatrix](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2voice-setoutputmatrix). Esta parte establece el volumen en completo (1.0) y luego establece la matriz de volumen en silencio para las entradas izquierda y derecha y para los altavoces de salida izquierdo y derecho. Lo hacemos porque más adelante otro código se encadena entre las dos reverberaciones (simulando la transición de estar cerca de una pared a pasar a estar en una gran habitación) o silencia ambas reverberaciones si es necesario. Cuando más adelante se reactiva el audio de la vía de reverberación, el juego establece una matriz de {1.0f, 0.0f, 0.0f, 1.0f} para direccionar la salida de reverberación izquierda a la entrada izquierda de la voz de procesamiento y la salida de reverberación derecha a la entrada derecha de la voz de procesamiento.

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

Marble Maze llama al método **audio:: CreateReverb** cuatro veces: dos veces para la música de fondo y dos veces para los sonidos de juego. En el siguiente ejemplo se muestra cómo Marble Maze llama al método **CreateReverb** para la música de fondo.

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

Para obtener una lista de las posibles orígenes de efectos que puedes usar con XAudio2, consulta [XAudio2 Audio Effects](/windows/desktop/xaudio2/xaudio2-audio-effects) (Efectos de audio de XAudio2).

### <a name="loading-audio-data-from-file"></a>Cargar datos de audio desde un archivo

Marble Maze define la clase **MediaStreamer** , que usa Media Foundation para cargar recursos de audio desde archivos. Marble Maze usa un objeto **MediaStreamer** para cargar cada uno de los archivos de audio.

Marble Maze llama al método **MediaStreamer::Initialize** para inicializar cada una de las secuencias de audio. A continuación se describe cómo el método **Audio::CreateResources** llama a **MediaStreamer::Initialize** para inicializar la secuencia de audio de la música de fondo:

```cpp
// Media Foundation is a convenient way to get both file I/O and format decode for 
// audio assets. You can replace the streamer in this sample with your own file I/O 
// and decode routines.
m_musicStreamer.Initialize(L"Media\\Audio\\background.wma");
```

El método **MediaStreamer:: Initialize** comienza llamando al método [MFStartup](/windows/desktop/api/mfapi/nf-mfapi-mfstartup) para inicializar Media Foundation. **MF_VERSION** es una macro definida en **mfapi. h**y es lo que se especifica como la versión de Media Foundation que se va a usar.

```cpp
DX::ThrowIfFailed(
    MFStartup(MF_VERSION)
    );
```

A continuación, **MediaStreamer::Initialize** llama a [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para crear un objeto [IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader). Un objeto **IMFSourceReader** , **m_reader**, Lee los datos multimedia del archivo especificado por la **dirección URL**.

```cpp
DX::ThrowIfFailed(
    MFCreateSourceReaderFromURL(url, nullptr, &m_reader)
    );
```

A continuación, el método **MediaStreamer:: Initialize** crea un objeto [IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype) mediante [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para describir el formato de la secuencia de audio. Un formato de audio usa dos tipos: un tipo principal y un subtipo. El tipo principal define el formato general del medio, como vídeo, audio, script, etc. El subtipo define el formato, como PCM, ADPCM o WMA.

El **método MediaStreamer:: Initialize** usa el método [IMFAttributes:: SetGUID](/windows/desktop/api/mfobjects/nf-mfobjects-imfattributes-setguid) para especificar el tipo principal ([MF_MT_MAJOR_TYPE](/windows/desktop/medfound/mf-mt-major-type-attribute)) como audio (audio MFMediaType) y el tipo secundario ([MF_MT_SUBTYPE](/windows/desktop/medfound/mf-mt-subtype-attribute)) como audio PCM sin comprimir (**MFAudioFormat \_ PCM**).** \_ ** **MF_MT_MAJOR_TYPE** y **MF_MT_SUBTYPE** son [atributos de Media Foundation](/windows/desktop/medfound/media-foundation-attributes). **MFMediaType_Audio** y **MFAudioFormat_PCM** son GUID de tipo y subtipo; para obtener más información, consulte [tipos de medios de audio](/windows/desktop/medfound/audio-media-types) . El método [IMFSourceReader::SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype) asocia el tipo de medio con el lector de secuencias.

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

A continuación, el método **MediaStreamer:: Initialize** obtiene el formato multimedia de salida completo de Media Foundation mediante [IMFSourceReader:: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) y llama al método [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para convertir el tipo de medio de audio Media Foundation en una estructura [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) . La estructura **WAVEFORMATEX** define el formato de datos de audio Waveform. Marble Maze usa esta estructura para crear las voces de origen y para aplicar el filtro de paso bajo al sonido de canica rodando.

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

> [!IMPORTANT]
> El método [MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) usa **CoTaskMemAlloc** para asignar el objeto [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) . Por consiguiente, asegúrate de llamar a **CoTaskMemFree** cuando termines de usar este objeto.

 

El método **MediaStreamer:: Initialize** finaliza al calcular la longitud de la secuencia, **m \_ maxStreamLengthInBytes**, en bytes. Para ello, llama al método [IMFSourceReader:: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) para obtener la duración de la secuencia de audio en unidades de 100-nanosegundos, convierte la duración en secciones y, a continuación, multiplica por la velocidad de transferencia de datos promedio en bytes por segundo. Después, Marble Maze usa este valor para asignar el búfer que contiene cada sonido de juego.

```cpp
// Get the total length of the stream, in bytes.
PROPVARIANT var;
DX::ThrowIfFailed(
    m_reader->
        GetPresentationAttribute(MF_SOURCE_READER_MEDIASOURCE, MF_PD_DURATION, &var)
    );

// duration is in 100ns units; convert to seconds, and round up
// to the nearest whole byte.
ULONGLONG duration = var.uhVal.QuadPart;
m_maxStreamLengthInBytes =
    static_cast<unsigned int>(
        ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000)
        / 10000000
        );
```

### <a name="creating-the-source-voices"></a>Creación de las voces de origen

Marble Maze crea voces de origen XAudio2 para reproducir cada uno de sus sonidos de juego y música en voces de origen. La clase **audio** define un objeto [IXAudio2SourceVoice](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2sourcevoice) para la música de fondo y una matriz de objetos **SoundEffectData** para contener los sonidos de juego. La estructura **SoundEffectData** contiene el objeto **IXAudio2SourceVoice** para un efecto y también define otros datos relacionados con el efecto, como el búfer de audio. **Audio. h** define la enumeración **SoundEvent** . Marble Maze usa esta enumeración para identificar cada sonido de juego. La clase **audio** también usa esta enumeración para indizar la matriz de objetos **SoundEffectData** .

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

En la siguiente tabla se muestra la relación entre cada uno de estos valores, el archivo que contiene los datos del sonido asociado y una breve descripción de qué representa cada sonido. Los archivos de audio se encuentran en la carpeta ** \\ media \\ audio** .

| Valor SoundEvent  | Nombre de archivo      | Descripción                                              |
|-------------------|----------------|----------------------------------------------------------|
| RollingEvent      | MarbleRoll.wav | Se reproduce cuando gira la canica.                              |
| FallingEvent      | MarbleFall.wav | Se reproduce cuando la canica sale del laberinto.               |
| CollisionEvent    | MarbleHit.wav  | Se reproduce cuando la canica colisiona con el laberinto.           |
| CheckpointEvent   | Checkpoint.wav | Se reproduce cuando la canica pasa por un punto de control.         |
| MenuChangeEvent   | MenuChange.wav | Se reproduce cuando el usuario cambia el elemento de menú actual. |
| MenuSelectedEvent | MenuSelect.wav | Se reproduce cuando el usuario selecciona un elemento de menú.           |

 

En el siguiente ejemplo se muestra cómo el método **Audio::CreateResources** crea la voz de origen para la música de fondo. La estructura de [ \_ \_ descriptor de envío de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_send_descriptor) define la voz de destino de destino de otra voz y especifica si se debe usar un filtro. Marble Maze llama al método **audio:: SetSoundEffectFilter** para usar los filtros con el fin de cambiar el sonido de la bola mientras se revierte. La estructura de la [voz de XAUDIO2 \_ \_ envía](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_voice_sends) un conjunto de voces para recibir datos de una sola voz de salida. Marble Maze envía datos de la voz de origen a la voz de procesamiento (para la parte seca, o no modificada, de un sonido de reproducción) y a las dos voces de submezcla que implementan la parte húmeda, o reverberante, del sonido de reproducción.

El método [IXAudio2::CreateSourceVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createsourcevoice) crea y configura una voz de origen. Usa una estructura [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) que define el formato de los búferes de audio que se envían a la voz. Como se ha indicado anteriormente, Marble Maze usa el formato PCM.

```cpp
XAUDIO2_SEND_DESCRIPTOR descriptors[3];
descriptors[0].pOutputVoice = m_musicMasteringVoice;
descriptors[0].Flags = 0;
descriptors[1].pOutputVoice = m_musicReverbVoiceSmallRoom;
descriptors[1].Flags = 0;
descriptors[2].pOutputVoice = m_musicReverbVoiceLargeRoom;
descriptors[2].Flags = 0;
XAUDIO2_VOICE_SENDS sends = {0};
sends.SendCount = 3;
sends.pSends = descriptors;
WAVEFORMATEX& waveFormat = m_musicStreamer.GetOutputWaveFormatEx();

DX::ThrowIfFailed(
    m_musicEngine->CreateSourceVoice(&m_musicSourceVoice, &waveFormat, 0, 1.0f, &m_voiceContext, &sends, nullptr)
    );

DX::ThrowIfFailed(
    m_musicMasteringVoice->SetVolume(0.4f)
    );
```

## <a name="playing-background-music"></a>Reproducción de la música de fondo


Una voz de origen se crea en estado de parada. Marble Maze inicia la música de fondo en el bucle del juego. La primera llamada a **MarbleMazeMain:: Update** llama a **audio:: Start** para iniciar la música de fondo.

```cpp
if (!m_audio.m_isAudioStarted)
{
    m_audio.Start();
}
```

El método **Audio::Start** llama a [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) para empezar a procesar la voz de origen para la música de fondo.

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

La voz de origen pasa dichos datos de audio a la siguiente etapa del gráfico de audio. En el caso de Marble Maze, la siguiente etapa contiene dos voces de submezcla que aplican al audio los dos efectos de reverberación. Una voz de submezcla aplica una reverberación cercana de campo lejano; la segunda aplica una reverberación lejana de campo lejano.

La cantidad en que cada voz de submezcla contribuye a la mezcla final se determina a partir del tamaño y la forma de la sala. La reverberación de campo cercano contribuye más cuando la bola está cerca de una pared o en una sala pequeña, y la reverberación de campo lejano contribuye más cuando la bola se encuentra en un espacio grande. Esta técnica produce un efecto de eco más real a medida que la canica se desplaza por el laberinto. Para obtener más información sobre cómo Marble Maze implementa este efecto, consulta **Audio::SetRoomSize** y **Physics::CalculateCurrentRoomSize** en el código fuente de Marble Maze.

> [!NOTE]
> En un juego en el que la mayoría de los tamaños de sala son relativamente iguales, puede usar un modelo de reverberación más básico. Por ejemplo, puedes usar una configuración de reverberación para todas las salas, o bien puedes crear una configuración de reverberación predefinida para cada sala.

El método **Audio::CreateResources** usa Media Foundation para cargar la música de fondo. Sin embargo, en este punto la voz de origen no tiene datos de audio con los que trabajar. Además, dado que la música de fondo entra en un bucle, la voz de origen debe actualizarse periódicamente con datos para que la música siga reproduciéndose.

Para que la voz de origen vaya teniendo datos, el bucle del juego actualiza los búferes de audio cada fotograma. El método **MarbleMazeMain:: Render** llama a **audio:: Render** para procesar el búfer de audio de música de fondo. La clase **audio** define una matriz de tres búferes de audio, **m \_ audioBuffers**. Cada búfer contiene 64 KB (65536 bytes) de datos. El bucle lee datos del objeto de Media Foundation y escribe dichos datos en la voz de origen hasta que esta tiene tres búferes en cola.

> [!CAUTION]
> Aunque Marble Maze usa un búfer de 64 KB para almacenar los datos de música, puede que tenga que usar un búfer mayor o menor. El tamaño depende de los requisitos del juego.

```cpp
// This sample processes audio buffers during the render cycle of the application.
// As long as the sample maintains a high-enough frame rate, this approach should
// not glitch audio. In game code, it is best for audio buffers to be processed
// on a separate thread that is not synced to the main render loop of the game.
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
    catch (...)
    {
        m_engineExperiencedCriticalError = true;
    }
}
```

El bucle también controla el momento en que el objeto de Media Foundation llega al final de la secuencia. En este caso, llama al método [IMFSourceReader:: SetCurrentPosition](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentposition) para restablecer la posición del origen de audio.

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

Para implementar bucles de audio para un solo búfer (o para un sonido completo que está totalmente cargado en la memoria), puede establecer el campo [XAUDIO2_BUFFER](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer):: LoopCount en el **bucle de XAUDIO2 \_ \_ infinito** cuando se inicializa el sonido. Marble Maze usa esta técnica para reproducir el sonido de rodadura de la canica.

```cpp
if (sound == RollingEvent)
{
    m_soundEffects[sound].m_audioBuffer.LoopCount = XAUDIO2_LOOP_INFINITE;
}
```

No obstante, para la música de fondo, Marble Maze administra los búferes directamente a fin de tener mayor control sobre la cantidad de memoria utilizada. Si los archivos de música son grandes, puedes secuenciar los datos de música en búferes más pequeños. Si haces esto te ayudará a equilibrar el tamaño de la memoria con la frecuencia de la capacidad del juego para procesar y hacer streaming de datos de audio.

> [!TIP]
> Si el juego tiene una velocidad de fotogramas reducida o variable, el procesamiento de audio en el subproceso principal puede producir pausas o detonaciones inesperadas en el audio porque el motor de audio no tiene suficientes datos de audio almacenados en búfer para trabajar con ellos. Si el juego es sensible a este problema, considera la posibilidad de procesar audio en un subproceso independiente que no realice representación. Este enfoque es especialmente útil en equipos con varios procesadores, porque el juego puede usar procesadores inactivos.

## <a name="reacting-to-game-events"></a>Reaccionar ante eventos del juego

La clase de **audio** proporciona métodos como **PlaySoundEffect**, **IsSoundEffectStarted**, **StopSoundEffect**, **SetSoundEffectVolume**, **SetSoundEffectPitch**y **SetSoundEffectFilter** para que el juego pueda controlar cuándo se reproducen y se detienen los sonidos, y para controlar las propiedades de sonido como el volumen y el tono. Por ejemplo, si la canica cae fuera del laberinto, **MarbleMazeMain:: Update** llama al método **audio::P laysoundeffect** para reproducir el sonido **FallingEvent** .

```cpp
m_audio.PlaySoundEffect(FallingEvent);
```

El método **Audio::PlaySoundEffect** llama al método [IXAudio2SourceVoice::Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start) para comenzar la reproducción del sonido. Si ya se ha llamado al método **IXAudio2SourceVoice::Start**, la reproducción no se inicia de nuevo. De este modo, **Audio::PlaySoundEffect** ejecuta una lógica personalizada para determinados sonidos.

```cpp
void Audio::PlaySoundEffect(SoundEvent sound)
{
    XAUDIO2_BUFFER buf = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (m_engineExperiencedCriticalError)
    {
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
    if (sound != RollingEvent)
    {
        XAUDIO2_VOICE_STATE state = {0};

        soundEffect->m_soundEffectSourceVoice->
            GetState(&state, XAUDIO2_VOICE_NOSAMPLESPLAYED);

        if (state.BuffersQueued == 0)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }
        else if (state.BuffersQueued < 2 && sound == CollisionEvent)
        {
            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);
        }

        // For the menu clicks, we want to stop the voice and replay the click
        // right away.
        // Note that stopping and then flushing could cause a glitch due to the
        // waveform not being at a zero-crossing, but due to the nature of the 
        // sound (fast and 'clicky'), we don't mind.
        if (state.BuffersQueued > 0 && sound == MenuChangeEvent)
        {
            soundEffect->m_soundEffectSourceVoice->Stop();
            soundEffect->m_soundEffectSourceVoice->FlushSourceBuffers();

            soundEffect->m_soundEffectSourceVoice->
                SubmitSourceBuffer(&soundEffect->m_audioBuffer);

            soundEffect->m_soundEffectSourceVoice->Start();
        }
    }

    m_soundEffects[sound].m_soundEffectStarted = true;
}
```

Para sonidos que no sean de rodadura, el método **Audio::PlaySoundEffect** llama a [IXAudio2SourceVoice::GetState](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-getstate) para determinar el número de búferes que reproduce la voz de origen. Este método llama a [IXAudio2SourceVoice::SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer) para agregar los datos de audio del sonido a la cola de entrada de la voz si no hay ningún búfer activo. El método **Audio::PlaySoundEffect** también permite la reproducción del sonido de colisión dos veces seguidas. Esto ocurre, por ejemplo, cuando la canica colisiona con una esquina del laberinto.

Como ya se ha descrito, la clase de audio usa la marca ** \_ \_ infinita del bucle de XAUDIO2** cuando inicializa el sonido para el evento de desplazamiento. El sonido inicia una reproducción en bucle la primera vez que se llama a **Audio::PlaySoundEffect** para este evento. Para simplificar la lógica de reproducción del sonido de rodadura, Marble Maze silencia el sonido en lugar de detenerlo. A medida que la canica cambia de velocidad, Marble Maze cambia el tono y el volumen del sonido para que tenga un efecto más real. A continuación se muestra cómo el método **MarbleMazeMain:: Update** actualiza el tono y el volumen de la canica a medida que cambia su velocidad y cómo silencia el sonido estableciendo su volumen en cero cuando se detiene la canica.

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

## <a name="reacting-to-suspend-and-resume-events"></a>Reacción a eventos de suspensión y reanudación

La [estructura de aplicación de Marble Maze](marble-maze-application-structure.md) describe cómo Marble Maze admite suspender y reanudar. Cuando se suspende el juego, el juego pone el audio en pausa. Cuando se reanuda el juego, el juego reanuda el audio en el punto en el que lo paró. Esto se hace para seguir el procedimiento recomendado que indica no usar recursos cuando sabemos que no son necesarios.

Cuando se suspende el juego, se llama al método **Audio::SuspendAudio**. Este método llama al método [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) para detener todo el audio. Aunque **IXAudio2::StopEngine** detiene inmediatamente toda la salida del audio, mantiene el gráfico de audio y sus parámetros de efectos (por ejemplo, el efecto de reverberación que se aplica cuando la canica rebota).

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

Cuando se reanuda el juego, se llama al método **Audio::ResumeAudio**. Este método usa el método [IXAudio2::StartEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-startengine) para reiniciar el audio. Dado que la llamada a [IXAudio2::StopEngine](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-stopengine) conserva el gráfico de audio y sus parámetros de efectos, la salida de audio se reanuda en el punto en que se detuvo.

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

## <a name="handling-headphones-and-device-changes"></a>Control de cambios de auriculares y dispositivos

Marble Maze usa las devoluciones de llamada del motor para controlar los errores del motor de XAudio2, como cuando cambia el dispositivo de audio. Una causa muy probable de un cambio de dispositivo puede ser cuando el usuario del juego conecta o desconecta los auriculares. Te recomendamos que implementes la devolución de llamada del motor que controla los cambios de dispositivo. En caso contrario, el juego dejará de reproducir el sonido cuando el usuario conecte o desconecte los auriculares, hasta que se reinicie el juego.

**Audio. h** define la clase **AudioEngineCallbacks** . Esta clase implementa la interfaz [IXAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback).

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

La interfaz [XAudio2EngineCallback](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2enginecallback) permite que el código reciba una notificación cuando se producen eventos de procesamiento de audio y cuando el motor tiene un error crítico. Para registrarse en las devoluciones de llamada, Marble Maze llama al método [IXAudio2:: RegisterForCallbacks](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-registerforcallbacks) en **audio:: CreateResources**, después de crear el objeto [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) para el motor de música.

```cpp
m_musicEngineCallback.Initialize(this);
m_musicEngine->RegisterForCallbacks(&m_musicEngineCallback);
```

Marble Maze no requiere notificación cuando se inicia o finaliza el procesamiento del audio. Por consiguiente, implementa el método [IXAudio2EngineCallback::OnProcessingPassStart](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassstart) y el método [IXAudio2EngineCallback::OnProcessingPassEnd](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-onprocessingpassend) para no hacer nada. En el método [IXAudio2EngineCallback:: OnCriticalError](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2enginecallback-oncriticalerror) , Marble Maze llama al método **SetEngineExperiencedCriticalError** , que establece la marca **m \_ engineExperiencedCriticalError** .

```cpp
// Audio.cpp

// Called when a critical system error causes XAudio2 
// to be closed and restarted. The error code is given in Error. 
void  _stdcall AudioEngineCallbacks::OnCriticalError(HRESULT Error)
{
    m_audio->SetEngineExperiencedCriticalError();
}
```

```cpp
// Audio.h (Audio class)

// This flag can be used to tell when the audio system 
// is experiencing critial errors.
// XAudio2 gives a critical error when the user unplugs
// the headphones and a new speaker configuration is generated.
void SetEngineExperiencedCriticalError()
{
    m_engineExperiencedCriticalError = true;
}
```

Cuando se produce un error crítico, el procesamiento de audio se detiene y todas las llamadas adicionales a XAudio2 generan un error. Para que te puedas recuperar de esta situación, debes liberar la instancia de XAudio2 y crear una nueva. El método **audio:: Render** , al que se llama desde el bucle de juego cada fotograma, comprueba primero la marca **m \_ engineExperiencedCriticalError** . Si esta marca está establecida, la borra, libera la instancia actual de XAudio2, inicializa los recursos y luego inicia la música de fondo.

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

Marble Maze también usa la marca **m \_ engineExperiencedCriticalError** para protegerse de la llamada a XAudio2 cuando no hay ningún dispositivo de audio disponible. Por ejemplo, el método **MarbleMazeMain:: Update** no procesa audio para eventos graduales o de colisión cuando se establece esta marca. La aplicación intenta reparar el motor de audio cada fotograma si es necesario; sin embargo, la marca **m \_ engineExperiencedCriticalError** siempre se puede establecer si el equipo no tiene un dispositivo de audio o si los auriculares están desenchufados y no hay otro dispositivo de audio disponible.

> [!CAUTION]
> Como norma general, no realice operaciones de bloqueo en el cuerpo de una devolución de llamada del motor. Si lo haces, generarás problemas de rendimiento. Marble Maze establece una marca en la devolución de llamada **OnCriticalError** y luego controla el error durante la fase de procesamiento normal de audio. Para obtener más información sobre las devoluciones de llamada de XAudio2, consulta [XAudio2 Callbacks](/windows/desktop/xaudio2/xaudio2-callbacks) (Devoluciones de llamada de XAudio2).

## <a name="conclusion"></a>Conclusión

Esto incluye el ejemplo de juego Marble Maze. Aunque es un juego relativamente sencillo, contiene muchas de las partes importantes que entran en cualquier juego DirectX de UWP y es un buen ejemplo que debe seguir al crear su propio juego.

Ahora que ha terminado de seguir adelante, pruebe a Tinkering con el código fuente y vea lo que sucede. O bien, consulte [crear un juego de UWP sencillo con DirectX](tutorial--create-your-first-uwp-directx-game.md), otro ejemplo de juego DirectX de UWP.

¿Está listo para continuar con DirectX? A continuación, eche un vistazo a nuestras guías en [programación con DirectX](directx-programming.md).

Si le interesa el desarrollo de juegos en UWP en general, consulte la documentación en [programación de juegos](index.md).

## <a name="related-topics"></a>Temas relacionados

* [Agregar métodos de entrada e interactividad en la muestra de Marble Maze](adding-input-and-interactivity-to-the-marble-maze-sample.md)
* [Desarrollo de Marble Maze, un juego para UWP en C++ y DirectX](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)