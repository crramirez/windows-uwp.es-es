---
title: Agregar sonido
description: Desarrolle un motor de sonido sencillo con las API de XAudio2 para reproducir música y efectos sonoros de juegos.
ms.assetid: aa05efe2-2baa-8b9f-7418-23f5b6cd2266
ms.date: 10/24/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, sonido
ms.localizationpriority: medium
ms.openlocfilehash: 0e624c750bfce0633bc91d440fd883341b831836
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409654"
---
# <a name="add-sound"></a>Agregar sonido

> [!NOTE]
> Este tema forma parte de la serie de tutoriales de [creación de una plataforma universal de Windows sencilla (UWP) con DirectX](tutorial--create-your-first-uwp-directx-game.md) . El tema de ese vínculo establece el contexto de la serie.

En este tema, se creará un motor de sonido sencillo con las API de [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction) . Si no está familiarizado con __XAudio2__, hemos incluido una breve introducción en [conceptos de audio](#audio-concepts).

>[!Note]
>Si no ha descargado el código de juego más reciente para este ejemplo, vaya a [juego de ejemplo de Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este ejemplo forma parte de una gran colección de ejemplos de características de UWP. Para obtener instrucciones sobre cómo descargar el ejemplo, consulte [obtener los ejemplos de UWP en github](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Agregue sonidos al Juego de ejemplo con [XAudio2](/windows/desktop/xaudio2/xaudio2-introduction).

## <a name="define-the-audio-engine"></a>Definir el motor de audio

En el juego de ejemplo, los objetos de audio y los comportamientos se definen en tres archivos:

* __[Audio. h](#audioh)/.cpp__: define el objeto de __audio__ , que contiene los recursos de __XAudio2__ para la reproducción de sonido. También define el método para suspender y reanudar la reproducción de audio si el juego se pausa o desactiva.
* __ [MediaReader. h](#mediareaderh)/.cpp__: define los métodos para leer archivos. wav de audio desde el almacenamiento local.
* __ [SoundEffect. h](#soundeffecth)/.cpp__: define un objeto para la reproducción de sonido en el juego.

## <a name="overview"></a>Información general

Hay tres partes principales en la configuración para la reproducción de audio en el juego.

1. [Crear e inicializar los recursos de audio](#create-and-initialize-the-audio-resources)
2. [Cargar archivo de audio](#load-audio-file)
3. [Asociar sonido a objeto](#associate-sound-to-object)

Todos se definen en el método [Simple3DGame:: Initialize](#simple3dgameinitialize-method) . Vamos a examinar primero este método y, a continuación, profundizaremos en más detalles en cada una de las secciones.

Después de configurar, aprenderá a desencadenar los efectos sonoros que se van a reproducir. Para obtener más información, vaya a [reproducir el sonido](#play-the-sound).

### <a name="simple3dgameinitialize-method"></a>Simple3DGame:: Initialize (método)

En __Simple3DGame:: Initialize__, donde también se inicializan el __ \_ controlador m__ y el __ \_ representador m__ , se configura el motor de audio y se prepara para reproducir sonidos.

 * Cree __m \_ audioController__, que es una instancia de la clase [audio](#audioh) .
 * Cree los recursos de audio necesarios con el método [audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) . Aquí, dos objetos de __XAudio2__ &mdash; son un objeto de motor de música y un objeto de motor de sonido, y se ha creado una voz de maestro para cada uno de ellos. El objeto de motor de música se puede usar para reproducir música en segundo plano del juego. El motor de sonido se puede usar para reproducir efectos sonoros en el juego. Para obtener más información, consulte [crear e inicializar los recursos de audio](#create-and-initialize-the-audio-resources).
 * Cree __mediaReader__, que es una instancia de la clase [mediaReader](#mediareaderh) . [MediaReader](#mediareaderh), que es una clase auxiliar para la clase [SoundEffect](#soundeffecth) , Lee los archivos de audio pequeños sincrónicamente desde la ubicación del archivo y devuelve datos de sonido como una matriz de bytes.
 * Use [MediaReader:: LoadMedia](#mediareaderloadmedia-method) para cargar archivos de sonido desde su ubicación y crear una variable __targetHitSound__ que contenga los datos de sonido. wav cargados. Para obtener más información, consulte [carga de archivo de audio](#load-audio-file). 

Los efectos sonoros están asociados al objeto de juego. Así, cuando se produce una colisión con ese objeto de juego, se desencadena el efecto de sonido que se va a reproducir. En este juego de ejemplo, tenemos efectos sonoros para la munición (lo que usamos para captar los destinos con) y para el destino. 
    
* En la clase __GameObject__ , hay una propiedad __HitSound__ que se usa para asociar el efecto de sonido al objeto.
* Cree una nueva instancia de la clase [SoundEffect](#soundeffecth) e inicialícela. Durante la inicialización, se crea una voz de origen para el efecto de sonido. 
* Esta clase reproduce un sonido mediante una voz de masterización proporcionada por la clase [audio](#audioh) . Los datos de sonido se leen desde la ubicación del archivo mediante la clase [MediaReader](#mediareaderh) . Para obtener más información, vea [asociar sonido a objeto](#associate-sound-to-object).

>[!Note]
>El desencadenador real para reproducir el sonido viene determinado por el movimiento y la colisión de estos objetos de juego. Por lo tanto, la llamada para reproducir realmente estos sonidos se define en el método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) . Para obtener más información, vaya a [reproducir el sonido](#play-the-sound).

```cppwinrt
void Simple3DGame::Initialize(
    _In_ std::shared_ptr<MoveLookController> const& controller,
    _In_ std::shared_ptr<GameRenderer> const& renderer
    )
{
    // The following member is defined in the header file:
    // Audio m_audioController;

    ...

    // Create the audio resources needed.
    // Two XAudio2 objects are created - one for music engine,
    // the other for sound engine. A mastering voice is also
    // created for each of the objects.
    m_audioController.CreateDeviceIndependentResources();

    m_ammo.resize(GameConstants::MaxAmmo);

    ...

    // Create a media reader which is used to read audio files from its file location.
    MediaReader mediaReader;
    auto targetHitSoundX = mediaReader.LoadMedia(L"Assets\\hit.wav");

    // Instantiate the targets for use in the game.
    // Each target has a different initial position, size, and orientation.
    // But share a common set of material properties.
    for (int a = 1; a < GameConstants::MaxTargets; a++)
    {
        ...
        // Create a new sound effect object and associate it
        // with the game object's (target) HitSound property.
        target->HitSound(std::make_shared<SoundEffect>());

        // Initialize the sound effect object with
        // the sound effect engine, format of the audio wave, and audio data
        // During initialization, source voice of this sound effect is also created.
        target->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            targetHitSoundX
            );
        ...
    }

    // Instantiate a set of spheres to be used as ammunition for the game
    // and set the material properties of the spheres.
    auto ammoHitSound = mediaReader.LoadMedia(L"Assets\\bounce.wav");

    for (int a = 0; a < GameConstants::MaxAmmo; a++)
    {
        m_ammo[a] = std::make_shared<Sphere>();
        m_ammo[a]->Radius(GameConstants::AmmoRadius);
        m_ammo[a]->HitSound(std::make_shared<SoundEffect>());
        m_ammo[a]->HitSound()->Initialize(
            m_audioController.SoundEffectEngine(),
            mediaReader.GetOutputWaveFormatEx(),
            ammoHitSound
            );
        m_ammo[a]->Active(false);
        m_renderObjects.push_back(m_ammo[a]);
    }
    ...
}
```

## <a name="create-and-initialize-the-audio-resources"></a>Crear e inicializar los recursos de audio

* Use [XAudio2Create](/windows/desktop/api/xaudio2/nf-xaudio2-xaudio2create), una API de xaudio2, para crear dos nuevos objetos de xaudio2 que definen los motores de música y efecto sonoro. Este método devuelve un puntero a la interfaz [IXAudio2](/windows/desktop/api/xaudio2/nn-xaudio2-ixaudio2) del objeto que administra todos los Estados del motor de audio, el subproceso de procesamiento de audio, el gráfico de voz, etc.
* Una vez creadas las instancias de los motores, use [IXAudio2:: CreateMasteringVoice](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2-createmasteringvoice) para crear una voz de registro para cada uno de los objetos del motor de sonido.

Para obtener más información, vaya a [Cómo: inicializar XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2).

### <a name="audiocreatedeviceindependentresources-method"></a>Audio:: CreateDeviceIndependentResources (método)

```cppwinrt
void Audio::CreateDeviceIndependentResources()
{
    UINT32 flags = 0;

    winrt::check_hresult(
        XAudio2Create(m_musicEngine.put(), flags)
        );

    HRESULT hr = m_musicEngine->CreateMasteringVoice(&m_musicMasteringVoice);
    if (FAILED(hr))
    {
        // Unable to create an audio device
        m_audioAvailable = false;
        return;
    }

    winrt::check_hresult(
        XAudio2Create(m_soundEffectEngine.put(), flags)
        );

    winrt::check_hresult(
        m_soundEffectEngine->CreateMasteringVoice(&m_soundEffectMasteringVoice)
        );

    m_audioAvailable = true;
}
```

## <a name="load-audio-file"></a>Cargar archivo de audio

En el juego de ejemplo, el código para leer archivos de formato de audio se define en [MediaReader. h](#mediareaderh)/cpp__.  Para leer un archivo de audio. wav codificado, llame a [MediaReader:: LoadMedia](#mediareaderloadmedia-method), pasando el nombre del archivo. wav como parámetro de entrada.

### <a name="mediareaderloadmedia-method"></a>MediaReader:: LoadMedia (método)

Este método usa las API de [Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) para leer en los archivos de audio .wav como un búfer de modulación por impulsos codificados (PCM, del inglés Pulse Code Modulation).

#### <a name="set-up-the-source-reader"></a>Configuración del lector de origen

1. Use [MFCreateSourceReaderFromURL](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-mfcreatesourcereaderfromurl) para crear un lector de origen multimedia ([IMFSourceReader](/windows/desktop/api/mfreadwrite/nn-mfreadwrite-imfsourcereader)).
2. Use [MFCreateMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatemediatype) para crear un objeto de tipo de medio ([IMFMediaType](/windows/desktop/api/mfobjects/nn-mfobjects-imfmediatype)) (_mediaType_). Representa una descripción de un formato de medios. 
3. Especifique que la salida descodificada de _mediaType_sea audio PCM, que es un tipo de audio que puede usar __XAudio2__ .
4. Establece el tipo de medio de salida descodificado para el lector de origen mediante una llamada a [IMFSourceReader:: SetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-setcurrentmediatype).

Para obtener más información sobre por qué usamos el lector de origen, vaya a [lector de código fuente](/windows/desktop/medfound/source-reader).

#### <a name="describe-the-data-format-of-the-audio-stream"></a>Describir el formato de datos de la secuencia de audio

1. Use [IMFSourceReader:: GetCurrentMediaType](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getcurrentmediatype) para obtener el tipo de medio actual para la secuencia.
2. Use [IMFMediaType:: MFCreateWaveFormatExFromMFMediaType](/windows/desktop/api/mfapi/nf-mfapi-mfcreatewaveformatexfrommfmediatype) para convertir el tipo de medio de audio actual en un búfer de [WAVEFORMATEX](/windows/desktop/api/mmreg/ns-mmreg-twaveformatex) , utilizando los resultados de la operación anterior como entrada. Esta estructura especifica el formato de datos de la secuencia de audio de onda que se utiliza después de cargar el audio. 

El formato __WAVEFORMATEX__ se puede usar para describir el búfer del PCM. En comparación con la estructura [WAVEFORMATEXTENSIBLE](/windows-hardware/drivers/ddi/content/ksmedia/ns-ksmedia-waveformatextensible) , solo se puede usar para describir un subconjunto de formatos de onda de audio. Para obtener más información sobre las diferencias entre __WAVEFORMATEX__ y __WAVEFORMATEXTENSIBLE__, consulte [descriptores de formato de onda extensible](/windows-hardware/drivers/audio/extensible-wave-format-descriptors).

#### <a name="read-the-audio-stream"></a>Leer la secuencia de audio

1.  Obtiene la duración, en segundos, de la secuencia de audio mediante una llamada a [IMFSourceReader:: GetPresentationAttribute](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-getpresentationattribute) y, a continuación, convierte la duración en bytes.
2.  Lea el archivo de audio en como una secuencia mediante una llamada a [IMFSourceReader:: ReadSample](/windows/desktop/api/mfreadwrite/nf-mfreadwrite-imfsourcereader-readsample). __ReadSample__ lee el siguiente ejemplo del origen multimedia.
3.  Use [IMFSample:: ConvertToContiguousBuffer](/windows/desktop/api/mfobjects/nf-mfobjects-imfsample-converttocontiguousbuffer) para copiar el contenido del búfer de ejemplo de audio (_ejemplo_) en una matriz (_mediaBuffer_).

```cppwinrt
std::vector<byte> MediaReader::LoadMedia(_In_ winrt::hstring const& filename)
{
    winrt::check_hresult(
        MFStartup(MF_VERSION)
        );

    // Creates a media source reader.
    winrt::com_ptr<IMFSourceReader> reader;
    winrt::check_hresult(
        MFCreateSourceReaderFromURL(
        (m_installedLocationPath + filename).c_str(),
            nullptr,
            reader.put()
            )
        );

    // Set the decoded output format as PCM.
    // XAudio2 on Windows can process PCM and ADPCM-encoded buffers.
    // When using MediaFoundation, this sample always decodes into PCM.
    winrt::com_ptr<IMFMediaType> mediaType;
    winrt::check_hresult(
        MFCreateMediaType(mediaType.put())
        );

    // Define the major category of the media as audio. For more info about major media types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa367377.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_MAJOR_TYPE, MFMediaType_Audio)
        );

    // Define the sub-type of the media as uncompressed PCM audio. For more info about audio sub-types,
    // go to: https://msdn.microsoft.com/library/windows/desktop/aa372553.aspx
    winrt::check_hresult(
        mediaType->SetGUID(MF_MT_SUBTYPE, MFAudioFormat_PCM)
        );

    // Sets the media type for a stream. This media type defines that format that the Source Reader 
    // produces as output. It can differ from the native format provided by the media source.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/dd374667.aspx
    winrt::check_hresult(
        reader->SetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), 0, mediaType.get())
        );

    // Get the current media type for the stream.
    // For more info, go to:
    // https://msdn.microsoft.com/library/windows/desktop/dd374660.aspx
    winrt::com_ptr<IMFMediaType> outputMediaType;
    winrt::check_hresult(
        reader->GetCurrentMediaType(static_cast<uint32_t>(MF_SOURCE_READER_FIRST_AUDIO_STREAM), outputMediaType.put())
        );

    // Converts the current media type into the WaveFormatEx buffer structure.
    UINT32 size = 0;
    WAVEFORMATEX* waveFormat;
    winrt::check_hresult(
        MFCreateWaveFormatExFromMFMediaType(outputMediaType.get(), &waveFormat, &size)
        );

    // Copies the waveFormat's block of memory to the starting address of the m_waveFormat variable in MediaReader.
    // Then free the waveFormat memory block.
    // For more info, go to https://msdn.microsoft.com/library/windows/desktop/aa366535.aspx and
    // https://msdn.microsoft.com/library/windows/desktop/ms680722.aspx
    CopyMemory(&m_waveFormat, waveFormat, sizeof(m_waveFormat));
    CoTaskMemFree(waveFormat);

    PROPVARIANT propVariant;
    winrt::check_hresult(
        reader->GetPresentationAttribute(static_cast<uint32_t>(MF_SOURCE_READER_MEDIASOURCE), MF_PD_DURATION, &propVariant)
        );

    // 'duration' is in 100ns units; convert to seconds, and round up
    // to the nearest whole byte.
    LONGLONG duration = propVariant.uhVal.QuadPart;
    unsigned int maxStreamLengthInBytes =
        static_cast<unsigned int>(
            ((duration * static_cast<ULONGLONG>(m_waveFormat.nAvgBytesPerSec)) + 10000000) /
            10000000
            );

    std::vector<byte> fileData(maxStreamLengthInBytes);

    winrt::com_ptr<IMFSample> sample;
    winrt::com_ptr<IMFMediaBuffer> mediaBuffer;
    DWORD flags = 0;

    int positionInData = 0;
    bool done = false;
    while (!done)
    {
        // Read audio data.
        ...
    }

    return fileData;
}
```

## <a name="associate-sound-to-object"></a>Asociar sonido a objeto

La Asociación de sonidos al objeto tiene lugar cuando se inicializa el juego, en el método [Simple3DGame:: Initialize](#simple3dgameinitialize-method) .

Resumen:
* En la clase __GameObject__ , hay una propiedad __HitSound__ que se usa para asociar el efecto de sonido al objeto.
* Cree una nueva instancia del objeto de la clase [SoundEffect](#soundeffecth) y asóciela con el objeto Game. Esta clase reproduce un sonido mediante las API de __XAudio2__ .  Usa una voz de masterización proporcionada por la clase [audio](#audioh) . Los datos de sonido se pueden leer desde la ubicación del archivo mediante la clase [MediaReader](#mediareaderh) .

[SoundEffect:: Initialize](#soundeffectinitialize-method) se usa para inicializar la instancia de __SoundEffect__ con los siguientes parámetros de entrada: puntero al objeto de motor de sonido (objetos IXAudio2 creados en el método [audio:: CreateDeviceIndependentResources](#audiocreatedeviceindependentresources-method) ), puntero al formato del archivo. wav mediante __MediaReader:: GetOutputWaveFormatEx__y los datos de sonido cargados mediante el método [MediaReader:: LoadMedia](#mediareaderloadmedia-method) . Durante la inicialización, también se crea la voz de origen del efecto de sonido.

### <a name="soundeffectinitialize-method"></a>SoundEffect:: Initialize (método)

```cppwinrt
void SoundEffect::Initialize(
    _In_ IXAudio2* masteringEngine,
    _In_ WAVEFORMATEX* sourceFormat,
    _In_ std::vector<byte> const& soundData)
{
    m_soundData = soundData;

    if (masteringEngine == nullptr)
    {
        // Audio is not available so just return.
        m_audioAvailable = false;
        return;
    }

    // Create a source voice for this sound effect.
    winrt::check_hresult(
        masteringEngine->CreateSourceVoice(
            &m_sourceVoice,
            sourceFormat
            )
        );
    m_audioAvailable = true;
}
```

## <a name="play-the-sound"></a>Reproducir el sonido

Los desencadenadores para reproducir efectos sonoros se definen en el método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) porque es donde se actualiza el movimiento de los objetos y se determina la colisión entre objetos.

Dado que la interacción entre los objetos difiere en gran medida, en función del juego, no vamos a analizar aquí la dinámica de los objetos de juego. Si le interesa entender su implementación, vaya al método [Simple3DGame:: UpdateDynamics](#simple3dgameupdatedynamics-method) .

En principio, cuando se produce una colisión, desencadena el efecto de sonido que se va a reproducir llamando a **SoundEffect::P laysound**. Este método detiene los efectos sonoros que se reproducen actualmente y pone en cola el búfer en memoria con los datos de sonido deseados. Usa la voz de origen para establecer el volumen, enviar datos de sonido e iniciar la reproducción.

### <a name="soundeffectplaysound-method"></a>SoundEffect::P método laySound

* Usa el objeto de voz **m \_ sourceVoice** de origen para iniciar la reproducción del búfer de datos de sonido **m \_ soundData**
* Crea un [ \_ búfer de XAUDIO2](/windows/desktop/api/xaudio2/ns-xaudio2-xaudio2_buffer), al que proporciona una referencia al búfer de datos de sonido y, a continuación, lo envía con una llamada a [IXAudio2SourceVoice:: SubmitSourceBuffer](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-submitsourcebuffer). 
* Con los datos de sonido en cola, **SoundEffect::P laysound** empieza a reproducirse llamando a [IXAudio2SourceVoice:: Start](/windows/desktop/api/xaudio2/nf-xaudio2-ixaudio2sourcevoice-start).

```cppwinrt
void SoundEffect::PlaySound(_In_ float volume)
{
    XAUDIO2_BUFFER buffer = { 0 };

    if (!m_audioAvailable)
    {
        // Audio is not available so just return.
        return;
    }

    // Interrupt sound effect if it is currently playing.
    winrt::check_hresult(
        m_sourceVoice->Stop()
        );
    winrt::check_hresult(
        m_sourceVoice->FlushSourceBuffers()
        );

    // Queue the memory buffer for playback and start the voice.
    buffer.AudioBytes = (UINT32)m_soundData.size();
    buffer.pAudioData = m_soundData.data();
    buffer.Flags = XAUDIO2_END_OF_STREAM;

    winrt::check_hresult(
        m_sourceVoice->SetVolume(volume)
        );
    winrt::check_hresult(
        m_sourceVoice->SubmitSourceBuffer(&buffer)
        );
    winrt::check_hresult(
        m_sourceVoice->Start()
        );
}
```

### <a name="simple3dgameupdatedynamics-method"></a>Simple3DGame:: UpdateDynamics (método)

El método __Simple3DGame:: UpdateDynamics__ se encarga de la interacción y la colisión entre los objetos de juego. Cuando los objetos entran en conflicto (o forman una intersección), se desencadena el efecto sonoro asociado para reproducirlo.

```cppwinrt
void Simple3DGame::UpdateDynamics()
{
    ...
    // Check for collisions between ammo.
#pragma region inter-ammo collision detection
if (m_ammoCount > 1)
{
    ...
    // Check collision between instances One and Two.
    ...
    if (distanceSquared < (GameConstants::AmmoSize * GameConstants::AmmoSize))
    {
        // The two ammo are intersecting.
        ...
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
        ...
        // Play the sound associated with the Ammo hitting something.
        m_objects[i]->PlaySound(impact, m_player->Position());

        if (m_objects[i]->Target() && !m_objects[i]->Hit())
        {
            // The object is a target and isn't currently hit, so mark
            // it as hit and play the sound associated with the impact.
            m_objects[i]->Hit(true);
            m_objects[i]->HitTime(timeTotal);
            m_totalHits++;

            m_objects[i]->PlaySound(impact, m_player->Position());
        }
        ...
    }
#pragma endregion

#pragma region Apply Gravity and world intersection
            // Apply gravity and check for collision against enclosing volume.
            ...
                if (position.z < limit)
                {
                    // The ammo instance hit the a wall in the min Z direction.
                    // Align the ammo instance to the wall, invert the Z component of the velocity and
                    // play the impact sound.
                    position.z = limit;
                    m_ammo[i]->PlaySound(-velocity.z, m_player->Position());
                    velocity.z = -velocity.z * GameConstants::Physics::GroundRestitution;
                }
                ...
#pragma endregion
}
```

## <a name="next-steps"></a>Pasos siguientes

Hemos tratado el marco de UWP, los gráficos, los controles, la interfaz de usuario y el audio de un juego de Windows 10. La siguiente parte de este tutorial, [ampliando el juego de ejemplo](tutorial-resources.md), explica otras opciones que se pueden usar al desarrollar un juego.

## <a name="audio-concepts"></a>Conceptos de audio

Para el desarrollo de juegos de Windows 10, use XAudio2 versión 2,9. Esta versión se incluye con Windows 10. Para obtener más información, vaya a [las versiones de XAudio2](/windows/desktop/xaudio2/xaudio2-versions).

__AudioX2__ es una API de bajo nivel que proporciona procesamiento de señal y combinación de base. Para obtener más información, consulte [conceptos clave de XAudio2](/windows/desktop/xaudio2/xaudio2-key-concepts).

### <a name="xaudio2-voices"></a>Voces de XAudio2

Hay tres tipos de objetos de voz de XAudio2: voces de origen, submezcla y maestra. Las voces son los objetos que usa XAudio2 para procesar, manipular y reproducir datos de audio. 
* Las voces de origen operan en datos de audio proporcionados por el cliente. 
* Las voces de origen y de submezcla envían su resultado a una o más voces de submezcla o de procesamiento. 
* Las voces de submezcla y de procesamiento mezclan el audio de todas las voces que les alimentan y trabajan con el resultado. 
* Las voces de Master reciben datos de voces de origen y voces de submezcla, y envían los datos al hardware de audio.

Para obtener más información, vaya a [XAudio2 Voices](/windows/desktop/xaudio2/xaudio2-voices).

### <a name="audio-graph"></a>Gráfico de audio

El gráfico de audio es una colección de [voces de XAudio2](/windows/desktop/xaudio2/xaudio2-voices). El audio se inicia en un lado de un gráfico de audio en las voces de origen y, opcionalmente, pasa a través de una o más voces de la mezcla y finaliza en una voz de la maestra. Un gráfico de audio contendrá una voz de origen para cada sonido que se reproduce actualmente, cero o más voces de submezcla y una voz de maestro. El gráfico de audio más sencillo, y el mínimo necesario para crear un ruido en XAudio2, es una voz de origen única que se envía directamente a una voz de mastering. Para obtener más información, vaya a [gráficos de audio](/windows/desktop/xaudio2/audio-graphs).

### <a name="additional-reading"></a>Lecturas adicionales

* [Cómo: inicializar XAudio2](/windows/desktop/xaudio2/how-to--initialize-xaudio2)
* [Cómo: cargar archivos de datos de audio en XAudio2](/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2)
* [Cómo: reproducir un sonido con XAudio2](/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2)

## <a name="key-audio-h-files"></a>Archivos de Key audio. h

### <a name="audioh"></a>Audio.h

```cppwinrt
// Audio:
// This class uses XAudio2 to provide sound output. It creates two
// engines - one for music and the other for sound effects - each as
// a separate mastering voice.
// The SuspendAudio and ResumeAudio methods can be used to stop
// and start all audio playback.

class Audio
{
public:
    Audio();

    void Initialize();
    void CreateDeviceIndependentResources();
    IXAudio2* MusicEngine();
    IXAudio2* SoundEffectEngine();
    void SuspendAudio();
    void ResumeAudio();

private:
    ...
};
```

### <a name="mediareaderh"></a>MediaReader. h

```cppwinrt
// MediaReader:
// This is a helper class for the SoundEffect class. It reads small audio files
// synchronously from the package installed folder and returns sound data as a
// vector of bytes.

class MediaReader
{
public:
    MediaReader();

    std::vector<byte> LoadMedia(_In_ winrt::hstring const& filename);
    WAVEFORMATEX* GetOutputWaveFormatEx();

private:
    winrt::Windows::Storage::StorageFolder  m_installedLocation{ nullptr };
    winrt::hstring                          m_installedLocationPath;
    WAVEFORMATEX                            m_waveFormat;
};
```

### <a name="soundeffecth"></a>SoundEffect.h

```cppwinrt
// SoundEffect:
// This class plays a sound using XAudio2. It uses a mastering voice provided
// from the Audio class. The sound data can be read from disk using the MediaReader
// class.

class SoundEffect
{
public:
    SoundEffect();

    void Initialize(
        _In_ IXAudio2* masteringEngine,
        _In_ WAVEFORMATEX* sourceFormat,
        _In_ std::vector<byte> const& soundData
        );

    void PlaySound(_In_ float volume);

private:
    ...
};
```
