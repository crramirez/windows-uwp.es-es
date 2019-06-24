---
title: Agregar sonido
description: Desarrolle un motor de sonido simple mediante las API de XAudio2 para música del juego de reproducción y efectos de sonido.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, juegos, sonidos
ms.localizationpriority: medium
ms.openlocfilehash: 06c06e1ffe52cae37a000f748076d78ebf6afff4
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318865"
---
# <a name="add-sound"></a>Agregar sonido

En este tema, crearemos un sencillo motor sonido con [XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction) API. Si está familiarizado con __XAudio2__, hemos incluido una breve introducción en [conceptos de Audio](#audio-concepts).

>[!Note]
>Si no has descargado el código del juego más reciente para esta muestra, ve a [Muestra de juego de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de funciones para UWP. Si necesitas instrucciones sobre cómo descargar el ejemplo, consulta [Obtener las muestras de UWP desde GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Agregar sonidos en el ejemplo de juego mediante [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Definir el motor de audio

En la muestra del juego, los comportamientos y objetos de audio se definen en tres archivos:

* __[Audio.h](#audioh)/.cpp__: Define el __Audio__ objeto, que contiene el __XAudio2__ recursos para la reproducción de sonido. También define el método para suspender y reanudar la reproducción de audio si el juego se pausa o desactiva.
* __[MediaReader.h](#mediareaderh)/.cpp__: Define los métodos para leer los archivos de sonido .wav desde el almacenamiento local.
* __[SoundEffect.h](#soundeffecth)/.cpp__: Define un objeto para la reproducción de sonido en el juego.

## <a name="overview"></a>Información general

Hay tres partes principales en la configuración de reproducción de audio en su juego.

1. [Crear e inicializar los recursos de sonido](#create-and-initialize-the-audio-resources)
2. [Cargar archivo de audio](#load-audio-file)
3. [Asociar el sonido a objeto](#associate-sound-to-object)

Se definen en el [Simple3DGame::Initialize](#simple3dgameinitialize-method) método. Así que vamos a examinar primero este método y, a continuación, profundizar en más detalles en cada una de las secciones.

Una vez configurada, aprendemos a desencadenar los efectos de sonido para reproducir. Para obtener más información, vaya a [reproducir el sonido](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Método Simple3DGame::Initialize

En __Simple3DGame::Initialize__, donde __m\_controlador__ y __m\_representador__ son también se inicializa, configure el motor de audio y obtenerla listo para reproducir sonidos.

 * Crear __m\_audioController__, que es una instancia de la [Audio](#audioh) clase.
 * Crear los recursos audio necesarios mediante el [Audio::CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) método. Aquí, dos __XAudio2__ objetos &mdash; crearon un objeto de motor de música y un objeto de motor de sonido y una voz de punto de dominar para cada uno de ellos. El objeto de motor de música puede usarse para reproducir música de fondo para su juego. El motor de sonido puede usarse para reproducir los efectos de sonido en el juego. Para obtener más información, consulte [crear e inicializar los recursos de sonido](#create-and-initialize-the-audio-resources).
 * Crear __mediaReader__, que es una instancia de [MediaReader](#mediareaderh) clase. [MediaReader](#mediareaderh), que es una clase auxiliar para el [SoundEffect](#soundeffecth) clase lecturas audio pequeño archivos de forma sincrónica desde la ubicación del archivo y devuelve datos sonidos como una matriz de bytes.
 * Use [MediaReader::LoadMedia](#mediareaderloadmedia-method) para cargar los archivos de sonido de su ubicación y crear un __targetHitSound__ variable que contenga los datos de sonido .wav cargado. Para obtener más información, consulte [cargar archivo de audio](#load-audio-file). 

Efectos de sonido están asociados con el objeto de juego. Por lo que cuando se produce una colisión con ese objeto de juego, desencadena el efecto de sonido que se reproduzca. En este ejemplo de juego, tenemos los efectos de sonido para la munición (lo que usamos para solucionar los destinos con) y para el destino. 
    
* En el __GameObject__ class, hay un __HitSound__ propiedad que se usa para asociar el efecto de sonido en el objeto.
* Crear una nueva instancia de la [SoundEffect](#soundeffecth) clase e inicialícelo. Durante la inicialización, se crea una voz de origen para el efecto de sonido. 
* Esta clase reproduce un sonido con una punto de dominar voz proporcionada a partir de la [Audio](#audioh) clase. Datos de sonido se leen desde la ubicación de archivo con el [MediaReader](#mediareaderh) clase. Para obtener más información, consulte [asociar sonido al objeto](#associate-sound-to-object).

>[!Note]
>El desencadenador para reproducir el sonido real viene determinada por el movimiento y la colisión de estos objetos del juego. Por lo tanto, la llamada para jugar realmente estos sonidos se definen en el [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método. Para obtener más información, vaya a [reproducir el sonido](#play-the-sound).

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

## <a name="create-and-initialize-the-audio-resources"></a>Crear e inicializar los recursos de sonido

* Use [XAudio2Create](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), una API de XAudio2, para crear dos nuevos objetos XAudio2 que definen los motores de efecto de música y sonido. Este método devuelve un puntero para el objeto [IXAudio2](https://docs.microsoft.com/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) interfaz que administra el motor de audio en todos los Estados, el procesamiento, el gráfico de voz y de subproceso más de audio.
* Después de que se han creado instancias de los motores, utilice [IXAudio2::CreateMasteringVoice](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) para crear una punto de dominar voz para cada uno de los objetos del motor de sonido.

Para obtener más información, vaya a [Cómo: Inicializar XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2).

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

## <a name="load-audio-file"></a>Cargar archivo de audio

En el ejemplo de juego, el código para leer los archivos de formato de audio se define en [MediaReader.h](#mediareaderh)/cpp__.  Para leer un archivo de audio wav codificado, llame a [MediaReader::LoadMedia](#mediareaderloadmedia-method), pasando el nombre de archivo de la .wav como parámetro de entrada.

### <a name="mediareaderloadmedia-method"></a>Método MediaReader::LoadMedia

Este método usa las API de [Media Foundation](https://docs.microsoft.com/windows/desktop/medfound/microsoft-media-foundation-sdk) para leer en los archivos de audio .wav como un búfer de modulación por impulsos codificados (PCM, del inglés Pulse Code Modulation).

#### <a name="set-up-the-source-reader"></a>Configurar el lector de código fuente

1. Use [MFCreateSourceReaderFromURL](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para crear un medio de lector de código fuente ([IMFSourceReader](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Use [MFCreateMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para crear un tipo de medio ([IMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) objeto (_mediaType_). Representa una descripción de un formato de medios. 
3. Especificar que el _mediaType_puede descodificar de la salida es audio PCM, que es audio de un tipo que __XAudio2__ puede usar.
4. Conjuntos de tipo de archivo multimedia de salida descodificado del lector de código fuente mediante una llamada a [IMFSourceReader::SetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Para obtener más información sobre por qué usamos el lector de código fuente, vaya a [origen lector](https://docs.microsoft.com/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Describir el formato de datos de la secuencia de audio

1. Use [IMFSourceReader::GetCurrentMediaType](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) para obtener el tipo de medio actual para la secuencia.
2. Use [IMFMediaType::MFCreateWaveFormatExFromMFMediaType](https://docs.microsoft.com/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para convertir tipo de los medios de audio actuales a un [WAVEFORMATEX](https://docs.microsoft.com/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) del búfer, con los resultados de la operación anterior como entrada. Esta estructura especifica el formato de datos de la secuencia de audio de onda que se usa una vez cargado el audio. 

El __WAVEFORMATEX__ formato puede usarse para describir el búfer PCM. En comparación con el [WAVEFORMATEXTENSIBLE](https://docs.microsoft.com/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) estructura, sólo puede utilizarse para describir un subconjunto de formatos de audio de onda. Para obtener más información sobre las diferencias entre __WAVEFORMATEX__ y __WAVEFORMATEXTENSIBLE__, consulte [Extensible descriptores de formato de onda](https://docs.microsoft.com/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Leer la secuencia de audio

1.  Obtener la duración, en segundos, de la secuencia de audio mediante una llamada a [IMFSourceReader::GetPresentationAttribute](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) y, a continuación, convierte la duración en bytes.
2.  Leer en el archivo de audio como una secuencia mediante una llamada a [IMFSourceReader::ReadSample](https://docs.microsoft.com/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lee el siguiente ejemplo desde el origen de medios.
3.  Use [IMFSample::ConvertToContiguousBuffer](https://docs.microsoft.com/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) para copiar el contenido del búfer de muestra de sonido (_ejemplo_) en una matriz (_mediaBuffer_).

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
## <a name="associate-sound-to-object"></a>Asociar el sonido a objeto

Asociación de sonidos para el objeto tiene lugar cuando se inicializa el juego, en el [Simple3DGame::Initialize](#simple3dgameinitialize-method) método.

Resumen:
* En el __GameObject__ class, hay un __HitSound__ propiedad que se usa para asociar el efecto de sonido en el objeto.
* Crear una nueva instancia de la [SoundEffect](#soundeffecth) clase de objeto y su asociación con el objeto de juego. Esta clase reproduce un sonido mediante __XAudio2__ API.  Usa una punto de dominar voz proporcionada por el [Audio](#audioh) clase. Los datos de sonido pueden leerse desde la ubicación de archivo mediante el [MediaReader](#mediareaderh) clase.

[SoundEffect::Initialize](#soundeffectinitialize-method) se usa para inicializar el __SoundEffect__ instancia con los siguientes parámetros de entrada: puntero al objeto de motor de sonido (objeto IXAudio2 creado en el [Audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) método), puntero al formato del archivo .wav mediante __MediaReader::GetOutputWaveFormatEx__, y los datos de sonido cargan mediante [MediaReader::LoadMedia ](#mediareaderloadmedia-method) método. Durante la inicialización, también se crea la voz de origen para el efecto de sonido.

### <a name="soundeffectinitialize-method"></a>Método SoundEffect::Initialize

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

Se definen desencadenadores para reproducir los efectos de sonido en [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método porque es donde se actualizan el movimiento de los objetos y colisión entre los objetos se determina.

Puesto que la interacción de entre los objetos en gran medida, varía en función del juego, no vamos a explicar la dinámica de los objetos de juego. Si le interesa entender su implementación, vaya a [Simple3DGame::UpdateDynamics](#simple3dgameupdatedynamics-method) método.

En principio, cuando se produce una colisión, desencadena el efecto de sonido reproducir mediante una llamada a **SoundEffect::PlaySound**. Este método detiene los efectos de sonido que se está reproduciendo y pone en cola el búfer en memoria con los datos deseados de sonido. Voz de origen usa para establecer el volumen, enviar datos de sonido e iniciar la reproducción.

### <a name="soundeffectplaysound-method"></a>Método SoundEffect::PlaySound

* Usa el objeto de origen de voz **m\_sourceVoice** para iniciar la reproducción del búfer de datos de sonido **m\_soundData**
* Crea un [XAUDIO2\_búfer](https://docs.microsoft.com/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), que proporciona una referencia al búfer de datos de sonido y, a continuación, se envía con una llamada a [IXAudio2SourceVoice::SubmitSourceBuffer](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Con los datos de sonido en la cola, **SoundEffect::PlaySound** reproducir se inicia mediante una llamada a [IXAudio2SourceVoice::Start](https://docs.microsoft.com/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

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

El __Simple3DGame::UpdateDynamics__ método encarga de la interacción y colisión entre los objetos del juego. Cuando los objetos entran en conflicto (o forman una intersección), desencadena el efecto de sonido asociado para reproducir.

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

Hemos tratado el framework UWP, gráficos, controles, interfaz de usuario y el audio de un juego de Windows 10. La siguiente parte de este tutorial, [ampliar el ejemplo de juego](tutorial-resources.md), explica otras opciones que se pueden usar al desarrollar un juego.

## <a name="audio-concepts"></a>Conceptos de audio

Para el desarrollo de juegos de Windows 10, usar XAudio2 versión 2.9. Esta versión se incluye con Windows 10. Para obtener más información, vaya a [XAudio2 versiones](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ es una API de bajo nivel que proporciona el procesamiento de señales y mezclar foundation. Para obtener más información, consulte [XAudio2 Key Concepts](https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>Voces de XAudio2

Hay tres tipos de objetos de voz de XAudio2: origen, submezcla y dominar las voces. Las voces son que los objetos XAudio2 se usan para procesar y manipular para reproducir datos de audio. 
* Las voces de origen operan en datos de audio proporcionados por el cliente. 
* Las voces de origen y de submezcla envían su resultado a una o más voces de submezcla o de procesamiento. 
* Las voces de submezcla y de procesamiento mezclan el audio de todas las voces que les alimentan y trabajan con el resultado. 
* Punto de dominar voces recibir datos de las voces de origen y las voces submezcla y envía los datos para el hardware de audio.

Para obtener más información, vaya a [XAudio2 voces](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Gráfico de audio

Elemento audiograph es una colección de [XAudio2 voces](/windows/desktop/xaudio2/xaudio2-voices). Audio comienza en un lado de un elemento audiograph en voces de origen, pasa opcionalmente a través de uno o más voces submezcla y termina en una punto de dominar voz. Un elemento audiograph contendrá una voz de origen para cada sonido se reproduce, voces submezcla de cero o más y una voz de punto de dominar. El gráfico más sencillo de audio y el mínimo necesario para realizar un ruido en XAudio2, es una voz de origen único escribir directamente en una punto de dominar voz. Para obtener más información, vaya a [gráficos de Audio](https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Lecturas adicionales

* [Cómo: Inicializar XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Cómo: Cargar archivos de datos de Audio en XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Cómo: Reproducir un sonido con XAudio2](https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

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


