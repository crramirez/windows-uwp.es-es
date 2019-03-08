---
title: Manipulación de audio en tiempo real
description: Obtenga información sobre cómo manipular y procesar el audio de chat capturado por 2 de Chat de juego.
ms.date: 05/10/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, chat juego 2, chat de juegos, comunicación por voz, manipulación del búfer, manipulación de audio
ms.localizationpriority: medium
ms.openlocfilehash: 7746080ea8a9698993a679b425f41442e4a6d943
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643900"
---
# <a name="real-time-audio-manipulation"></a>Manipulación de audio en tiempo real

Juego de Chat de 2 ofrece a los desarrolladores la opción para insertar a sí mismos en la canalización de audio de chat para inspeccionar y manipular los datos de audio de chat de los jugadores. Esto puede resultar útil para aplicar interesantes a efectos de audio a las voces de jugadores en juego. Canalización del juego Chat 2 audio manipulación es interactuar con objetos de secuencia de audio que puede sondearse para datos de audio. En lugar de usar devoluciones de llamada, este modelo permite a los desarrolladores inspeccionar o manipular audio en cualquier subproceso de procesamiento es más conveniente para ellos.

Un breve tutorial mediante la manipulación de audio en tiempo real se incluye a continuación, que contiene los temas siguientes:

1. [Inicializando la canalización de manipulación de audio](#initializing-the-audio-manipulation-pipeline)
2. [Cambios de estado de procesamiento de secuencias de audio](#processing-audio-stream-state-changes)
3. [Previamente se manipular codificar el audio de chat](#manipulating-pre-encode-chat-audio-data)
4. [Posterior a la manipulación de descodificar el audio de chat](#manipulating-post-decode-chat-audio-data)
5. [Duraciones de usuarios de chat](#chat-user-lifetimes)

## <a name="initializing-the-audio-manipulation-pipeline"></a>Inicializando la canalización de manipulación de audio

Juego 2 de Chat de forma predeterminada no se habilitará la manipulación de audio en tiempo real. Para habilitar la manipulación de audio en tiempo real, la aplicación debe especificar qué formas de manipulación de audio desea que habilita en `chat_manager::initialize()` estableciendo el parámetro audioManipulationMode.

Actualmente, se admiten los siguientes formatos de audio de manipulación:

* `game_chat_audio_manipulation_mode_flags::none` -Deshabilita la manipulación de audio. Es la configuración predeterminada. En este modo, el audio de chat se propagará sin interrupciones.
* `game_chat_audio_manipulation_mode_flags::pre_encode_stream_manipulation` -Permite codifica previamente la manipulación de audio. En este modo, se introducirán todo el audio chat generados por los usuarios locales a través de la canalización de audio manipulación antes de se codifica. Incluso si la aplicación es solo inspeccionar los datos de audio de chat y no trabaja con ella, todavía es responsabilidad de la aplicación para enviar los búferes de audio sin modificaciones al juego Chat 2 para que se pueden transmitir y codificados.
* `game_chat_audio_manipulation_mode_flags::post_decode_stream_manipulation` -Habilita descodifica posteriores a la manipulación de audio. En este modo, se introducirán todo el audio chat recibidos de los usuarios remotos a través de la canalización de manipulación de audio después su descodificación por parte del receptor, pero antes de representarse. Incluso si la aplicación es solo inspeccionar los datos de audio de chat y no trabaja con ella, todavía es responsabilidad de la aplicación para mezclar y enviar los búferes de audio sin modificaciones al juego Chat 2 para que puedan representarse.

## <a name="processing-audio-stream-state-changes"></a>Cambios de estado de procesamiento de secuencias de audio

Juego 2 Chat proporciona actualizaciones para el estado de secuencias de audio a través de `game_chat_stream_state_change` estructuras. Estas actualizaciones almacenan información acerca de qué se ha actualizado la secuencia y cómo se ha actualizado. Estas actualizaciones pueden sondearse para a través de las llamadas a la `chat_manager::start_processing_stream_state_changes()` y `chat_manager::finish_processing_stream_state_changes()` par de métodos. Este par de métodos proporciona toda la secuencia de audio más reciente, en cola las actualizaciones de estado como una matriz de `game_chat_stream_state_change` estructura punteros. Las aplicaciones deben recorrer en iteración la matriz y controlar correctamente cada actualización. Una vez disponibles `game_chat_stream_state_change` han sido controladas o actualizaciones, se debe pasar esa matriz al juego de Chat de 2 a través de `chat_manager::finish_processing_stream_state_changes()`. Por ejemplo:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            HandlePreEncodeAudioStreamCreated(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
```

## <a name="manipulating-pre-encode-chat-audio-data"></a>Manipular codificar previamente los datos de audio de chat

Juego de Chat de 2 proporciona acceso para codificar previamente los datos de audio de chat para los usuarios locales a través de la `pre_encode_audio_stream` clase.

### <a name="stream-lifetime"></a>Duración de Stream
Cuando un nuevo `pre_encode_audio_stream` instancia está listo para usar la aplicación, se entregarán a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::pre_encode_audio_stream_created`. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la secuencia de audio pasará a estar disponible para codificar previamente la manipulación de audio.

Cuando una existente `pre_encode_audio_stream` se convierte en disponible que se usará para la manipulación de audio, la aplicación se notificará a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::pre_encode_audio_stream_closed`. Esta es la oportunidad de la aplicación para iniciar la limpieza de los recursos asociados con esta secuencia de audio. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la secuencia de audio dejarán de estar disponible para codificar previamente la manipulación de audio.

Cuando un cerrado `pre_encode_audio_stream` todos sus recursos volvió, la secuencia se destruirá y se notificará la aplicación a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed`. Referencias o punteros a esta secuencia se deben limpiar. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la memoria de la secuencia de audio dejará de ser válida.

### <a name="stream-users"></a>Usuarios de Stream
La lista de los usuarios asociados con una secuencia se puede inspeccionar mediante `pre_encode_audio_stream::get_users()`.

### <a name="audio-formats"></a>Formatos de audio
El formato de audio de los búferes de la aplicación se recupera de juego Chat 2 se puede inspeccionar mediante `pre_encode_audio_stream::get_pre_processed_format()`. El formato de audio procesado previamente siempre será mono. La aplicación debe esperar a controlar los datos representados como valores de punto flotante de 32 bits, los enteros de 16 bits y enteros de 32 bits.

La aplicación debe informar al juego Chat 2 del formato de audio de los búferes manipulados que se están enviando a él para la codificación y transmisión utilizando `pre_encode_audio_stream::set_processed_format()`. Previamente, formatos procesados para codifican audio secuencias deben cumplir estas condiciones previas:

* El formato debe ser mono.
* El formato debe ser float de 32 bits PCM, PCM de entero de 32 bits o formatos PCM del entero de 16 bits.
* Frecuencia de muestreo del formato debe seguir las condiciones previas en función de la plataforma. Xbox One ERA admite velocidades de muestreo de 8kHz, 12kHz, 24kHz y 16kHz. Es compatible con UWP en Xbox One y PC 8kHz, 12kHz, kHz 16, 24kHz, 32kHz, 44,1 kHz y velocidades de muestreo de 48 kHz.

### <a name="retrieving-and-submitting-audio"></a>Recuperar y enviar Audio
Las aplicaciones pueden consultar previamente codificar secuencias de audio para el número de búferes disponibles para que procese utilizando `pre_encode_audio_stream::get_available_buffer_count()`. Esta información puede usarse si desea que la aplicación retrasar el procesamiento de audio hasta que haya un número mínimo de búferes disponible. Sólo 10 búferes se pondrá en cola en cada codificar previamente la secuencia de audio y audio retrasos introducirán latencia en la canalización de audio, por lo que se recomienda que las aplicaciones de agotar su previamente codificar secuencias de audio antes de poner en cola más de 4 búferes.

Las aplicaciones pueden recuperar los búferes de audio desde un codificar previamente utilizando la secuencia de audio `pre_encode_audio_stream::get_next_buffer()`. Búferes de audio nuevo estará disponibles en promedio, una vez cada 40 ms. Los búferes que devuelve este método deben liberarse a `pre_encode_audio_stream::return_buffer()` cuando hayan terminado que se va a usar. En cola un máximo de 10 o búferes de elementos que pueden existir en un momento dado para una secuencia de audio de codificar previamente. Una vez que se alcanza este límite, se eliminarán nuevos búferes capturados desde el origen de audio del Reproductor hasta que se devuelven a algunos de los búferes pendientes.

Las aplicaciones pueden enviar sus búferes inspeccionados y manipulados al juego Chat 2 para la codificación y transmisión utilizando `pre_encode_audio_stream::submit_buffer()`. Juego 2 Chat admite la manipulación de audio en contexto y fuera de lugar, por lo que los búferes se envían a `pre_encode_audio_stream::submit_buffer()` no tiene necesariamente que ser el mismos búferes recuperados `pre_encode_audio_stream::get_next_buffer()`. Privacidad/privilegio para estos búferes enviados se aplicará en función de los usuarios asociados con esta secuencia. Cada 40 MS, los 40 MS siguiente de esta secuencia de audio se codifican y se transmiten. Para evitar interrupciones de audio, se deben enviar los búferes de audio que debería ser escuchada continuamente en esta secuencia a una velocidad constante.

### <a name="stream-contexts"></a>Contextos de Stream
Las aplicaciones pueden administrar los valores de contexto personalizado dimensionado por puntero en codificar previamente secuencias de audio con `pre_encode_audio_stream::set_custom_stream_context()` y `pre_encode_audio_stream::custom_stream_context()`. Estos contextos de secuencia personalizada son útiles para crear asignaciones entre los de juegos Chat 2 secuencias de audio y datos auxiliar: hacer streaming a los metadatos, estado del juego, etcetera.

### <a name="example"></a>Ejemplo
Este es un ejemplo simplificado de-to-end para saber cómo usar previamente codificar secuencias de audio en el marco de procesamiento de audio uno:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::pre_encode_audio_stream_created:
        {
            pre_encode_audio_stream* stream = streamStateChanges[streamStateChangeIndex]->pre_encode_audio_stream;
            stream->set_processed_audio_format(...);
            stream->set_custom_stream_context(...);
            HandlePreEncodeAudioStreamCreated(stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            HandlePreEncodeAudioStreamClosed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        case game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            HandlePreEncodeAudioStreamDestroyed(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t preEncodeAudioStreamCount;
pre_encode_audio_stream_array preEncodeAudioStreams;
chat_manager::singleton_instance().get_pre_encode_audio_streams(&preEncodeAudioStreamCount, &preEncodeAudioStreams);
for (uint32_t preEncodeAudioStreamIndex = 0; preEncodeAudioStreamIndex < preEncodeAudioStreamCount; ++preEncodeAudioStreamIndex)
{
    pre_encode_audio_stream* stream = preEncodeAudioStreams[preEncodeAudioStreamIndex];
    StreamContext* context = reinterpret_cast<StreamContext*>(stream->custom_stream_context());

    game_chat_audio_format audio_format = stream->get_pre_processed_format();

    uint32_t preProcessedBufferByteCount;
    void* preProcessedBuffer;
    stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);

    while (preProcessedBuffer != nullptr)
    {
        void* processedBuffer = nullptr;
        switch (audio_format.bits_per_sample)
        {
            case 16:
            {
                assert (audio_format.sample_type == game_chat_sample_type::integer);
                processedBuffer = ManipulateChatBuffer<int16_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                break;
            }

            case 32:
            {
                switch (audio_format.sample_type)
                {
                    case game_chat_sample_type::integer:
                    {
                        processedBuffer = ManipulateChatBuffer<int32_t>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    case game_chat_sample_type::ieee_float:
                    {
                        processedBuffer = ManipulateChatBuffer<float>(preProcessedBufferByteCount, preProcessedBuffer, context);
                        break;
                    }

                    default:
                    {
                        assert(false);
                        break;
                    }
                }
                break;
            }

            default:
            {
                assert(false);
                break;
            }
        }
        // processedBuffer can be the same as preProcessedBuffer (in-place manipulation) or it can be a buffer of
        // memory not managed by Game Chat 2 (out-of-place manipulation).
        stream->submit_buffer(processedBuffer);
        // Only return buffers retrieved from Game Chat 2. Do not return foreign memory to return_buffer.
        stream->return_buffer(preProcessedBuffer);
        stream->get_next_buffer(&preProcessedBufferByteCount, &preProcessedBuffer);
    }
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="manipulating-post-decode-chat-audio-data"></a>Posterior a la manipulación de descodificar los datos de audio de chat

Juego de Chat de 2 proporciona acceso a los posteriores al descodificar datos de audio de chat a través de la `post_decode_audio_source_stream` y `post_decode_audio_sink_stream` clases, para que los usuarios pueden manipular audio de los usuarios remotos de forma única para cada receptor local de audio de chat.

### <a name="sources-and-sinks"></a>Orígenes y receptores
A diferencia de la canalización pre-encode, el modelo para tratar con posteriores al descodificar datos de audio se divide en dos clases: `post_decode_audio_source_stream` y `post_decode_audio_sink_stream`. Audio descodificado de los usuarios remotos puede obtenerse de `post_decode_audio_source_stream` objetos, manipular y envían a `post_decode_audio_sink_stream` objetos para la representación. Esto permite para la integración entre el juego de charla 2 descodificar posteriores a la canalización de procesamiento de audio y de middleware útil de audio.

### <a name="stream-lifetime"></a>Duración de Stream
Cuando un nuevo `post_decode_audio_source_stream` o `post_decode_audio_sink_stream` instancia está listo para usar la aplicación, se entregarán a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::post_decode_audio_source_stream_created` o `game_chat_stream_state_change_type::post_decode_audio_sink_stream_created`, respectivamente. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la secuencia de audio pasará a estar disponible para descodificar posteriores a la manipulación de audio.

Cuando una existente `post_decode_audio_source_stream` o `post_decode_audio_sink_stream` se convierte en disponible que se usará para la manipulación de audio, la aplicación se notificará a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::post_decode_audio_source_stream_closed` o `game_chat_stream_state_change_type::post_decode_audio_sink_stream`, respectivamente. Esta es la oportunidad de la aplicación para iniciar la limpieza de los recursos asociados con esta secuencia de audio. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la secuencia de audio dejarán de estar disponible para descodificar posteriores a la manipulación de audio. Para las secuencias de origen, esto significa que no hay más búferes se pondrá en cola para la manipulación. Para las secuencias de receptor, esto significa que envía los búferes ya no se representará.

Cuando un cerrado `post_decode_audio_source_stream` o `post_decode_audio_sink_stream` todos sus recursos volvió, la secuencia se destruirá y se notificará la aplicación a través de un `game_chat_stream_state_change` estructura con su `state_change_type` campo establecido en `game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed` o `game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed`, respectivamente. Referencias o punteros a esta secuencia se deben limpiar. Una vez que este cambio de estado de la secuencia se devuelve al 2 de Chat de juego, la memoria de la secuencia de audio dejará de ser válida.

### <a name="stream-users"></a>Usuarios de Stream
La lista de los usuarios remotos asociados con una secuencia de origen post-decode puede comprobarse con `post_decode_audio_source_stream::get_users()`. La lista de usuarios locales asociada a una secuencia de receptor post-decode puede comprobarse con `post_decode_audio_sink_stream::get_users()`.

### <a name="audio-formats"></a>Formatos de audio
El formato de audio de los búferes de la aplicación se recupera de juego Chat 2 se puede inspeccionar mediante `post_decode_audio_source_stream::get_pre_processed_format()`. El formato de audio procesado previamente siempre será mono, 16 bits entero PCM.

La aplicación debe informar al juego Chat 2 del formato de audio de los búferes manipulados que se están enviando a él para la representación mediante `post_decode_audio_sink_stream::set_processed_format()`. Formatos procesados para descodificación posteriores al audio receptor secuencias deben cumplir estas condiciones previas:

* El formato debe tener menos de 64 canales.
* El formato debe ser entero de 16 bits PCM (óptimo), 20 bits entero PCM (en un contenedor de 24 bits), 24 bits entero PCM, PCM de entero de 32 bits o float PCM (formato preferido después de entero de 16 bits PCM) de 32 bits. 
* Frecuencia de muestreo del formato debe ser entre 1000 y 200000 muestras por segundo.

### <a name="retrieving-and-submitting-audio"></a>Recuperar y enviar Audio
Las aplicaciones pueden consultar posteriores al descodificar secuencias de origen de audio para el número de búferes disponibles para que procese utilizando `post_decode_audio_source_stream::get_available_buffer_count()`. Esta información puede usarse si desea que la aplicación retrasar el procesamiento de audio hasta que haya un número mínimo de búferes disponible. Sólo 10 búferes se pondrá en cola en cada posteriores al descodificar la secuencia de origen de audio y audio retrasos introducirán latencia en la canalización de audio, por lo que se recomienda que las aplicaciones de agotar su posterior al descodificar secuencias de audio antes de poner en cola más de 4 búferes.

Las aplicaciones pueden recuperar los búferes de audio desde un posteriores al descodificar el flujo de origen de audio mediante `post_decode_audio_source_stream::get_next_buffer()`. Búferes de audio nuevo estará disponibles en promedio, una vez cada 40 ms. Los búferes que devuelve este método deben liberarse a `post_decode_audio_source_stream::return_buffer()` cuando hayan terminado que se va a usar. En cola un máximo de 10 o búferes de elementos que pueden existir en un momento dado para un posterior al descodificar la secuencia de origen de audio. Una vez que se alcanza este límite, se eliminarán nuevos búferes descodificados desde el Reproductor remoto hasta que se devuelven a algunos de los búferes pendientes.

Las aplicaciones pueden enviar sus búferes inspeccionados y manipulados al juego de Chat de 2 a través de posteriores al descodifican secuencias de audio del receptor para la representación mediante `post_decode_audio_sink_stream::submit_mixed_buffer()`. Juego 2 Chat admite la manipulación de audio en contexto y fuera de lugar, por lo que los búferes se envían a `post_decode_audio_sink_stream::submit_mixed_buffer()` no tiene necesariamente que ser el mismos búferes recuperados `post_decode_audio_source_stream::get_next_buffer()`. Cada 40 MS, se representará el 40 MS siguiente de esta secuencia de audio. Para evitar interrupciones de audio, se deben enviar los búferes de audio que debería ser escuchada continuamente en esta secuencia a una velocidad constante.

### <a name="privacy-and-mixing"></a>Privacidad y mezcla
Debido a modelo de origen y receptor de la canalización post-decode, es responsabilidad de la aplicación para mezclar los búferes de recuperarse `post_decode_audio_source_stream` objetos y enviar los búferes mixtos a `post_decode_audio_sink_stream` objetos para la representación. Esto también significa que es responsabilidad de la aplicación para realizar la combinación con la privacidad adecuada y privilegios que se aplican. Juego 2 Chat ofrece `post_decode_audio_sink_stream::can_receive_audio_from_source_stream()` para simplificar las consultas de esta información sencilla y eficaz.

### <a name="chat-indicators"></a>Indicadores de chat

Posterior al descodificar audio manipulación no tendrá efecto el estado del indicador de chat para cada usuario. Por ejemplo, cuando un usuario remoto está desactivado, el audio se proporcionará a la aplicación, pero todavía se indicará desactivado el indicador de chat para que el usuario remoto. Cuando está hablando con un usuario remoto, se proporcionará el audio, pero el indicador de chat indicará hablando con independencia de si la aplicación proporciona una mezcla de audio que contiene audio de dicho usuario. Para obtener más información sobre la interfaz de usuario y el indicador de chat, consulte [con juego Chat 2](using-game-chat-2.md#ui). Si las restricciones adicionales específicos de la aplicación se usan para determinar qué usuarios están presentes en una mezcla de audio, es responsabilidad de la aplicación a tener en cuenta las mismas restricciones al leer los indicadores de chat proporcionados por 2 de Chat de juego.

### <a name="stream-contexts"></a>Contextos de Stream
Las aplicaciones pueden administrar los valores de contexto personalizado dimensionado por puntero en posteriores al descodificar secuencias de audio con la `set_custom_stream_context()` y `custom_stream_context()` métodos. Estos contextos de secuencia personalizada son útiles para crear asignaciones entre los de juegos Chat 2 secuencias de audio y datos auxiliar: hacer streaming a los metadatos, estado del juego, etcetera.

### <a name="example"></a>Ejemplo
Este es un ejemplo simplificado de-to-end para saber cómo usar posteriores al descodificar secuencias de audio en el marco de procesamiento de audio uno:

```cpp
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);

for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        case game_chat_stream_state_change_type::post_decode_audio_source_stream_created:
        {
            post_decode_audio_source_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_source_stream;
            stream->set_custom_stream_context(...);
            HandlePostDecodeAudioSourceStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_closed:
        {
            HandlePostDecodeAudioSourceStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_source_stream_destroyed:
        {
            HandlePostDecodeAudioSourceStreamDestroyed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_created:
        {
            post_decode_audio_sink_stream* stream = streamStateChanges[streamStateChangeIndex]->post_decode_audio_sink_stream;
            stream->set_custom_stream_context(...);
            stream->set_processed_format(...);
            HandlePostDecodeAudioSinkStreamCreated(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_closed:
        {
            HandlePostDecodeAudioSinkStreamClosed(stream);
            break;
        }

        case game_chat_stream_state_change_type::post_decode_audio_sink_stream_destroyed:
        {
            HandlePostDecodeAudioSinkStreamDestroyed(stream);
            break;
        }

        ...
    }
}

chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

uint32_t sourceStreamCount;
post_decode_audio_source_stream_array sourceStreams;
chatManager::singleton_instance().get_post_decode_audio_source_streams(&sourceStreamCount, &sourceStreams);

uint32_t sinkStreamCount;
post_decode_audio_sink_stream_array sinkStreams;
chatManager::singleton_instance().get_post_decode_audio_sink_streams(&sinkStreamCount, &sinkStreams);

//
// MixBuffer is a custom type defined as:
// struct MixBuffer
// {
//     uint32_t bufferByteCount;
//     void* buffer;
// };
//
std::vector<std::pair<post_decode_audio_source_stream*, MixBuffer>> cachedSourceBuffers;

for (uint32_t sourceStreamIndex = 0; sourceStreamIndex < sourceStreamCount; ++sourceStreamIndex)
{
    post_decode_audio_source_stream* sourceStream = sourceStreams[sourceStreamIndex];

    MixBuffer mixBuffer;
    sourceStream->get_next_buffer(&mixBuffer.bufferByteCount, &mixBuffer.buffer);
    if (buffer != nullptr)
    {
        // Stash the buffer to return after we are done with mixing. If this program was using audio middleware, now
        // would be an appropriate time to plumb the buffer through the middleware
        cachedSourceBuffer.push_back(std::pair<post_decode_audio_source_stream*, MixBuffer>{sourceStream, mixBuffer});
    }
}

// Loop over each sink stream, perform mixing, and submit
for (uint32_t sinkStreamIndex = 0; sinkStreamIndex < sinkStreamCount; ++sinkStreamIndex)
{
    post_decode_audio_sink_stream* sinkStream = sinkStreams[sinkStreamIndex];

    if (sinkStream->is_open())
    {
        std::vector<std::pair<MixBuffer, float>> buffersToMixForThisStream;

        for (const std::pair<post_decode_audio_source_stream, MixBuffer>& sourceBufferPair : cachedSourceBuffers)
        {
            float volume;
            if (sinkStream->can_receive_audio_from_source_stream(sourceBufferPair.first, &volume))
            {
                buffersToMixForThisStream.push_back(std::pair<MixBuffer, float>{sourceBufferPair.second, volume});
            }
        }

        if (buffersToMixForThisStream.size() > 0)
        {
            uint32_t mixedBufferByteCount;
            uint8_t* mixedBuffer;
            MixPostDecodeBuffers(buffersToMixForThisStream, &mixedBufferByteCount, &mixedBuffer);
            sinkStream->submit_mixed_buffer(mixedBufferByteCount, mixedBuffer);
        }
    }
}

// Return buffers after mix and submission
for (const std::pair<post_decode_audio_source_stream*, MixBuffer>& cachedSourceBuffer : cachedSourceBuffers)
{
    post_decode_audio_source_stream* sourceStream = cachedSourceBuffer.first;
    void* bufferToReturn = cachedSourceBuffer.second.buffer;
    sourceStream->return_buffer(bufferToReturn);
}

Sleep(audioProcessingPeriodInMilliseconds);
```

## <a name="chat-user-lifetimes"></a>Duraciones de usuarios de chat

Habilitación de manipulación de audio en tiempo real, afectará a las duraciones de usuarios de charla. Si `chat_manager::remove_user(chatUserX)` se denomina el chat_user objeto al que señala chatUserX serán válida hasta que se hayan destruido todas las secuencias de audio que hacen referencia a chatUserX. Considere el siguiente escenario:

```cpp
// At somepoint a chat user, chatUserX, leaves the game session.
chat_manager::singleton_instance().remove_user(chatUserX);

// chatUserX is still valid, but to avoid further synchronization, prevent non-audio-stream use of chatUserX
chatUserX = nullptr;

// On the audio processing thread...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        // All of the streams associated with chatUserX will close.
        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_closed:
        {
            CleanupPreEncodeAudioStreamResources(streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream);
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);

// The next time the app processes stream state changes...
uint32_t streamStateChangeCount;
game_chat_stream_state_change_array streamStateChanges;
chat_manager::singleton_instance().start_processing_stream_state_changes(&streamStateChangeCount, &streamStateChanges);
for (uint32_t streamStateChangeIndex = 0; streamStateChangeIndex < streamStateChangeCount; ++streamStateChangeIndex)
{
    switch (streamStateChanges[streamStateChangeIndex]->state_change_type)
    {
        ...

        case Xs::game_chat_2::game_chat_stream_state_change_type::pre_encode_audio_stream_destroyed:
        {
            uint32_t chatUserCount;
            Xs::game_chat_2::chat_user_array chatUsers;
            streamStateChanges[streamStateChangeIndex].pre_encode_audio_stream->get_users(&chatUserCount, &chatUsers);
            assert(chatUserCount != 0);
            for (uint32_t chatUserIndex = 0; chatUserIndex < chatUserCount; ++chatUserIndex)
            {
                // chat_user objects such as chatUserX will still be valid while the destroyed state change is being processed.
                Log(chatUsers[chatUserIndex]->xbox_user_id());
            }
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_stream_state_changes(streamStateChanges);
// Once the all destroyed state changes have been processed for all streams associated with chatUserX, it's memory will be invalidated.
// Do not call methods on chatUserX, e.g. chatUserX->xbox_user_id()
```
