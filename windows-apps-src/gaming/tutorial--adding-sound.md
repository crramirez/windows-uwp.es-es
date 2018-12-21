---
title: Agregar sonido
description: Desarrollar un motor de sonido simple mediante las API de XAudio2 para la música del juego de reproducción y efectos de sonido.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, sonidos
ms.localizationpriority: medium
ms.openlocfilehash: 7ceef2da582f5d825949afdf2e116862c990165c
ms.sourcegitcommit: 7d0e6662de336a3d0e82ae9d1b61b1b0edb5aeeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "8981389"
---
# <a name="add-sound"></a>Agregar sonido

En este tema, creamos un sencillo motor de sonido con [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813) API. Si estás familiarizado con __XAudio2__, hemos incluido una breve introducción en [los conceptos de Audio](#audio-concepts).

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Ten en cuenta que este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Agregar los sonidos del juego de muestra con [XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813).

## <a name="define-the-audio-engine"></a>Definir el motor de audio

En la muestra de juego, los comportamientos y objetos de audio se definen en tres archivos:

* __[Audio.h](#audioh)/.cpp__: define el objeto de __Audio__ , que contiene los recursos __XAudio2__ para la reproducción de sonido. También define el método para suspender y reanudar la reproducción de audio si el juego se pausa o desactiva.
* __ [MediaReader.h](#mediareaderh)/.cpp__: define los métodos para leer archivos de audio .wav desde el almacenamiento local.
* __ [SoundEffect.h](#soundeffecth)/.cpp__: define un objeto para la reproducción de sonido en el juego.

## <a name="overview"></a>Introducción

Hay tres partes principales en preparación para la reproducción de audio en un juego.

1. [Crea e inicializa los recursos de audio](#create-and-initialize-the-audio-resources)
2. [Archivo de audio de carga](#load-audio-file)
3. [Asociar el sonido al objeto](#associate-sound-to-object)

Todas se definen en el método [simple3dgame:: Initialize](#simple3dgameinitialize-method) . Así que vamos a examinar en primer lugar este método y, a continuación, entrar en más detalles de cada una de las secciones.

Después de configurar, aprender cómo desencadenar los efectos de sonido para reproducir. Para obtener más información, ve a [reproducir el sonido](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Método simple3dgame:: Initialize

En __simple3dgame:: Initialize__, donde también se inicializan __m\_controller__ y __m\_renderer__ , establecemos el motor de audio y Prepárate reproducir sonidos.

 * Crear __m\_audioController__, que es una instancia de la clase de [Audio](#audioh) .
 * Crear los recursos de audio necesarios, mediante el método [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Aquí, dos objetos de __XAudio2__ &mdash; un objeto de motor de música y un objeto de motor de sonido y una voz de procesamiento para cada uno de ellos se crearon. El objeto de motor de música puede usarse para reproducir música de fondo para tu juego. El motor de sonido puede usarse para reproducir efectos de sonido en el juego. Para obtener más información, consulta [crear e inicializar los recursos de audio](#create-and-initialize-the-audio-resources).
 * Crear __mediaReader__, que es una instancia de clase [MediaReader](#mediareaderh) . [MediaReader](#mediareaderh), que es una clase auxiliar para la clase [SoundEffect](#soundeffecth) , lee pequeños archivos de audio de forma sincrónica de ubicación del archivo y devuelve datos de sonido como una matriz de bytes.
 * Usar [mediareader:: Loadmedia](#mediareaderloadmedia-method) para cargar archivos de sonido desde su ubicación y crea una variable __targetHitSound__ para contener los datos de sonido .wav cargado. Para obtener más información, consulta el [archivo de audio de carga](#load-audio). 

Los efectos de sonido se asocian con el objeto del juego. Por lo tanto, cuando se produce una colisión con ese objeto del juego, activa el efecto de sonido que se reproduzca. En esta muestra de juego tenemos los efectos de sonido para la munición (lo que usamos para disparar a objetivos con) y para el destino. 
    
* En la clase __GameObject__ , hay una propiedad __HitSound__ que se usa para asociar el efecto de sonido en el objeto.
* Crea una nueva instancia de la clase [SoundEffect](#soundeffecth) e inicializarlo. Durante la inicialización, se crea una voz de origen para el efecto de sonido. 
* Esta clase emite un sonido con una voz de procesamiento proporcionada desde la clase de [Audio](#audioh) . Datos de sonido se leen en la ubicación del archivo mediante la clase [MediaReader](#mediareaderh) . Para obtener más información, consulta [asociar sonido al objeto](#associate-sound-to-object).

>[!Note]
>El desencadenador real para reproducir el sonido viene determinado por el movimiento y la colisión de estos objetos del juego. Por lo tanto, la llamada a estos realmente sonidos se definen en el método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) . Para obtener más información, ve a [reproducir el sonido](#play-the-sound).

```cpp
void Simple3DGame::Initialize(
    _In_ MoveLookController^ controller,
    _In_ GameRenderer^ renderer
    )
{
    // ...
    // Create a new Audio class object.
    m_audioController = ref new Audio;

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController->CreateDeviceIndependentResources();

    m_ammo = std::vector<Sphere^>(GameConstants::MaxAmmo);

    // ..
    // Create a media reader which is used to read audio files from its file location.
    MediaReader^ mediaReader = ref new MediaReader;
    auto targetHitSound = mediaReader->LoadMedia("Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        // ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(ref new SoundEffect());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            targetHitSound
            );
        // ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader->LoadMedia("Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = ref new Sphere;
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(ref new SoundEffect());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController->SoundEffectEngine(),
            mediaReader->GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    // ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Crea e inicializa los recursos de audio

* Usa [XAudio2Create](https://msdn.microsoft.com/library/windows/desktop/ee419212), una API de XAudio2, para crear dos nuevos objetos de XAudio2 que definen los motores de música y efectos. Este método devuelve un puntero a la interfaz del objeto [IXAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415908) que administra todos los Estados de motor de audio, el procesamiento de audio subproceso, el gráfico de voz y mucho más.
* Después de los motores se hayan inicializado, usar [ixaudio2:: Createmasteringvoice](https://msdn.microsoft.com/library/windows/desktop/hh405048) para crear una voz de procesamiento para cada uno de los objetos del motor de sonido.

Para obtener más información, ve a [Cómo: inicializar XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx).

### <a name="audiocreatedeviceindependentresources-method"></a>Método audio::CreateDeviceIndependentResources

```cpp

void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    DX::ThrowIfFailed(
        XAudio2Create(&m_musicEngine, flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    DX::ThrowIfFailed(
        XAudio2Create(&m_soundEffectEngine, flags)
        );

    DX::ThrowIfFailed(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Archivo de audio de carga

En la muestra de juego, el código para leer archivos de formato de audio se define en [MediaReader.h](#mediareaderh)/cpp__.  Para leer un archivo de audio .wav codificado, llamar a [mediareader:: Loadmedia](#mediareaderloadmedia-method), pasando el nombre de archivo de la .wav como el parámetro de entrada.

### <a name="mediareaderloadmedia-method"></a>Método mediareader:: Loadmedia

Este método usa las API de [Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) para leer en los archivos de audio .wav como un búfer de modulación por impulsos codificados (PCM, del inglés Pulse Code Modulation).

#### <a name="set-up-the-source-reader"></a>Configurar el lector de origen

1. Usar [MFCreateSourceReaderFromURL](https://msdn.microsoft.com/library/windows/desktop/dd388110) para crear un medio de lector de origen ([IMFSourceReader](https://msdn.microsoft.com/library/windows/desktop/dd374655)).
2. Usar [MFCreateMediaType](https://msdn.microsoft.com/library/windows/desktop/ms693861) para crear un objeto de tipo ([IMFMediaType](https://msdn.microsoft.com/library/windows/desktop/ms704850)) de media (_mediaType_). Representa una descripción de un formato de medio. 
3. Especificar que la salida descodificada del _mediaType_de es audio PCM, que es un tipo de audio que puede usar __XAudio2__ .
4. Conjuntos de tipo de los medios de salida descodificado para el lector de origen mediante la llamada [imfsourcereader:: Setcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx).

Para obtener más información sobre por qué usamos el lector de origen, vaya a [Lector de orígenes](https://msdn.microsoft.com/library/windows/desktop/dd940436.aspx).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Describir el formato de datos de la secuencia de audio

1. Usa [imfsourcereader:: Getcurrentmediatype](https://msdn.microsoft.com/library/windows/desktop/dd374660) para obtener el tipo de medio actual de la secuencia.
2. Usar [imfmediatype:: Mfcreatewaveformatexfrommfmediatype](https://msdn.microsoft.com/library/windows/desktop/ms702177) para convertir el tipo de medio de audio actual en un búfer de [WAVEFORMATEX](https://msdn.microsoft.com/library/windows/hardware/ff538799) , con los resultados de la operación anterior como entrada. Esta estructura especifica el formato de datos de la secuencia de audio de onda que se usa una vez cargado el audio. 

El formato __WAVEFORMATEX__ puede usarse para describir el búfer PCM. En comparación con la estructura [WAVEFORMATEXTENSIBLE](https://msdn.microsoft.com/library/windows/hardware/ff538802) , puede usarse solo para describir un subconjunto de formatos de onda de audio. Para obtener más información sobre las diferencias entre __WAVEFORMATEX__ y __WAVEFORMATEXTENSIBLE__, consulta [Los descriptores de formato de onda Extensible](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Leer la secuencia de audio

1.  Obtener la duración, en segundos, de la secuencia de audio llamando a [imfsourcereader:: Getpresentationattribute](https://msdn.microsoft.com/library/windows/desktop/dd374662) y, a continuación, convierte la duración en bytes.
2.  Leer el archivo de audio como una secuencia mediante una llamada a [imfsourcereader:: Readsample](https://msdn.microsoft.com/library/windows/desktop/dd374665). __ReadSample__ lee la muestra siguiente desde el origen multimedia.
3.  Usa [IMFSample::ConvertToContiguousBuffer](https://msdn.microsoft.com/library/windows/desktop/ms698917.aspx) para copiar el contenido del búfer de la muestra de audio (_ejemplo_) en una matriz (_mediaBuffer_).

```cpp
Platform::Array<byte>^ MediaReader::LoadMedia(_In_ Platform::String^ filename)
{
    DX::ThrowIfFailed(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    ComPtr<IMFSourceReader> reader;
    DX::ThrowIfFailed(
        MFCreateSourceReaderFromURL(
            Platform::String::Concat(m_installedLocationPath, filename)->Data(),
            nullptr,
            &reader
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    Microsoft::WRL::ComPtr<IMFMediaType> mediaType;
    DX::ThrowIfFailed(
        MFCreateMediaType(&mediaType)
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    DX::ThrowIfFailed(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );
    
    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    DX::ThrowIfFailed(
        reader->SetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.Get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
        Microsoft::WRL::ComPtr<IMFMediaType> outputMediaType;
    DX::ThrowIfFailed(
        reader->GetCurrentMediaType(static_cast<uint32>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), &outputMediaType)
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    DX::ThrowIfFailed(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.Get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    DX::ThrowIfFailed(
        reader->GetPresentationAttribute(static_cast<uint32>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    Platform::Array<byte>^ fileData = ref new Platform::Array<byte>(maxStreamLengthInBytes);

    ComPtr<IMFSample> sample;
    ComPtr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        //...
        // Read audio data.
    }

    return fileData;
}
```
## <a name="associate-sound-to-object"></a>Asociar el sonido al objeto

Asociar los sonidos para el objeto tiene lugar cuando se inicializa el juego, en el método [simple3dgame:: Initialize](#simple3dgameinitialize-method) .

Resumen:
* En la clase __GameObject__ , hay una propiedad __HitSound__ que se usa para asociar el efecto de sonido en el objeto.
* Crear una nueva instancia del objeto de clase [SoundEffect](#soundeffecth) y asociarlo con el objeto del juego. Esta clase reproducirá un sonido con __XAudio2__ API.  Usa una voz de procesamiento proporcionada por la clase de [Audio](#audioh) . Los datos de sonido se pueden leer desde la ubicación del archivo mediante la clase [MediaReader](#mediareaderh) .

[SoundEffect:: Initialize](#soundeffectinitialize-method) se usa para inicializar la __SoundEffect__ instancia con los siguientes parámetros de entrada: puntero al objeto de motor de sonido (IXAudio2 objetos creados en el método [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), puntero para dar formato a de la .wav con __mediareader:: Getoutputwaveformatex__y los datos de sonido del archivo cargado mediante el método [mediareader:: Loadmedia](#mediareaderloadmedia-method) . Durante la inicialización, también se crea la voz de origen para el efecto de sonido.

### <a name="soundeffectinitialize-method"></a>Método SoundEffect:: Initialize

```cpp
void SoundEffect::Initialize(
    _In_ IXAudio2 *masteringEngine,
    _In_ WAVEFORMATEX *sourceFormat,
    _In_ Platform::Array<byte>^ soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    DX::ThrowIfFailed(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Reproducir el sonido

Desencadenadores para reproducir efectos de sonido se definen en el método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) dado que es donde se actualizan el movimiento de los objetos y se determinan colisiones entre objetos.

Dado que la interacción de entre objetos en gran medida, varía en función del juego, no vamos a explicar la dinámica de los objetos del juego aquí. Si estás interesado comprender su implementación, ve al método [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) .

En principio, cuando se produce una colisión, desencadena el efecto de sonido para reproducir mediante una llamada a **SoundEffect:: PlaySound**. Este método impide que los efectos de sonido que se está reproduciendo actualmente y coloca el búfer de memoria con los datos de sonido deseados. Usa voz de origen para establecer el volumen, enviar datos de sonido e iniciar la reproducción.

### <a name="soundeffectplaysound-method"></a>Método SoundEffect:: PlaySound

* Usa el objeto de voz de origen **m\_sourceVoice** para iniciar la reproducción del búfer de datos de sonido **m\_soundData**
* Crea un [XAUDIO2\_BUFFER](https://msdn.microsoft.com/library/windows/desktop/ee419228), al que proporciona una referencia al búfer de datos de sonido y, a continuación, se envía con una llamada a [ixaudio2sourcevoice:: Submitsourcebuffer](https://msdn.microsoft.com/library/windows/desktop/ee418473). 
* Con los datos de sonido en cola, **SoundEffect::PlaySound** empieza la reproducción llamando a [IXAudio2SourceVoice::Start](https://msdn.microsoft.com/library/windows/desktop/ee418471).

```cpp
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = {0};
    XAUDIO2_VOICE_STATE state = {0};

    if (!m_audioAvailable)
    {
        // Audio is not available, so just return.
        return;
    }

    // Interrupt sound effect if currently playing.
    DX::ThrowIfFailed(
        m_sourceVoice->Stop()
        );
    DX::ThrowIfFailed(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue in-memory buffer for playback and start the voice.
    buffer.AudioBytes = m_soundData->Length;
    buffer.pAudioData = m_soundData->Data;
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    m_sourceVoice->SetVolume(volume);
    DX::ThrowIfFailed(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    DX::ThrowIfFailed(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Método Simple3DGame::UpdateDynamics

El método __Simple3DGame::UpdateDynamics__ encarga de la interacción y las colisiones entre objetos del juego. Cuando los objetos entran en conflicto (o se cruzan), que desencadene el efecto de sonido asociado a reproducir.

```cpp
void Simple3DGame::UpdateDynamics()
{
    // ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
    if (m_ammoCount > 1)
    {
       // ...
       // Check collision between instances One and Two.
       // ...
       if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
       {
           // The two ammo are intersecting.
           // ...
           // Start playing the sounds for the impact between the two balls.
              m_ammo[one]->PlaySound(impact, m_player->Position());
              m_ammo[two]->PlaySound(impact, m_player->Position());

       }
    }
#pragma endregion

#pragma region Ammo-Object intersections
    // Check for intersections between the ammo and the other objects in the scene.
    // ...
    // Ball is in contact with Object.
    // ...

    // Make sure that the ball is actually headed towards the object. At grazing angles there
    // could appear to be an impact when the ball is actually already hit and moving away.
    if (impact > 0.0f)
    {
        // ...
        // Play the sound associated with the Ammo hitting something.
           m_ammo[one]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark it as hit and
            // play the sound associated with the impact.
             m_objects[i]->Hit(true);
             m_objects[i]->HitTime(timeTotal);
             m_totalHits++;

             m_objects[i]->PlaySound(impact, m_player->Position());
        }
        // ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
    // Apply gravity and check for collision against enclosing volume.
    // ...
    if (position.z < limit)
    {
        // The ammo instance hit the a wall in the min Z direction.
        // Align the ammo instance to the wall, invert the Z component of the velocity and
        // play the impact sound.
           position.z = limit;
           m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
           velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
    }
    // ...
#pragma endregion
}
```
## <a name="next-steps"></a>Pasos siguientes

Ya hemos hablado el marco UWP, gráficos, controles, interfaz de usuario y audio de un juego de Windows 10. La siguiente parte de este tutorial, [Extender la muestra de juego](tutorial-resources.md), explica otras opciones que pueden usarse al desarrollar un juego.

## <a name="audio-concepts"></a>Conceptos de audio

Para el desarrollo de juegos de Windows 10, usar XAudio2 versión 2.9. Esta versión se incluye con Windows 10. Para obtener más información, ve a [Las versiones de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415802.aspx).

__AudioX2__ es una API de bajo nivel que proporciona procesamiento de señal y mezcla foundation. Para obtener más información, consulta [Los conceptos clave de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415764.aspx).

### <a name="xaudio2-voices"></a>Voces de XAudio2

Hay tres tipos de objetos de voz XAudio2: origen, submezcla y las voces de procesamiento. Las voces son que los objetos XAudio2 se usan para procesar y manipular y reproducir datos de audio. 
* Las voces de origen operan en datos de audio proporcionados por el cliente. 
* Las voces de origen y de submezcla envían su resultado a una o más voces de submezcla o de procesamiento. 
* Las voces de submezcla y de procesamiento mezclan el audio de todas las voces que les alimentan y trabajan con el resultado. 
* Voces de procesamiento recibir datos de voces de origen y de voces de submezcla y envía datos al hardware de audio.

Para obtener más información, ve a [las voces de XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415824.aspx).

### <a name="audio-graph"></a>Gráfico de audio

Gráfico de audio es una colección de [voces de XAudio2](#xaudio2-voice-objects). Audio comienza en un lado de un gráfico de audio en las voces de origen, opcionalmente, pasa a través de uno o más voces de submezcla y termina en una voz de procesamiento. Un gráfico de audio contiene una voz de origen para cada sonido que se está reproduciendo actualmente, cero o más voces de submezcla y una voz de procesamiento. El gráfico de audio más simple y el mínimo necesario para realizar un ruido en XAudio2, es una sola voz de origen representando directamente a una voz de procesamiento. Para obtener más información, ve a [gráficos de Audio](https://msdn.microsoft.com/library/windows/desktop/ee415739.aspx).

### <a name="additional-reading"></a>Lecturas adicionales

* [Cómo: inicializar XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415779.aspx)
* [Cómo: cargar archivos de datos de audio en XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415781(v=vs.85).aspx)
* [Cómo: reproducir un sonido con XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415787.aspx)

## <a name="key-audio-h-files"></a>Archivos .h de audio clave

### <a name="audioh"></a>Audio.h

```cpp
// Audio:
// This class uses XAudio2 to provide sound output.  It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

protected:
    // ...
};
```
### <a name="mediareaderh"></a>MediaReader.h

```cpp
// MediaReader:
// This is a helper class for the SoundEffect class.  It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// byte array.

ref class MediaReader
{
internal:
    MediaReader();

    Platform::Array<byte>^          LoadMedia(_In_ Platform::String^ filename);
    WAVEFORMATEX*                   GetOutputWaveFormatEx();

protected private:
    Windows::Storage::StorageFolder^ m_installedLocation;
    Platform::String^               m_installedLocationPath;
    WAVEFORMATEX                    m_waveFormat;
};
```
### <a name="soundeffecth"></a>SoundEffect.h

```cpp
// SoundEffect:
// This class plays a sound using XAudio2.  It uses a mastering voice provided
// from the Audio class.  The sound data can be read from disk using the MediaReader
// class.

ref class SoundEffect
{
internal:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2*              masteringEngine,
        _In_ WAVEFORMATEX*          sourceFormat,
        _In_ Platform::Array<byte>^ soundData
        );

    void PlaySound(_In_ float volume);

protected private:
    //...

};
```


