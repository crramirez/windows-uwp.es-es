---
title: Uso de juegos Chat 2
description: Aprenda a usar Xbox Live Game Chat 2 para agregar la comunicación por voz a su juego.
ms.date: 03/20/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, chat juego 2, chat de juegos, comunicación por voz
ms.localizationpriority: medium
ms.openlocfilehash: 43fbd7cec037df735686aa60bc6cd57217875e33
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639260"
---
# <a name="using-game-chat-2-c"></a>Uso de juegos Chat 2 (C++)

Se trata de un breve tutorial sobre el uso de API de C++ del 2 de Chat de juego. Los desarrolladores que desean tener acceso a 2 de Chat de juego a través de juegos C# debería ver [uso juego Chat 2 WinRT proyecciones](using-game-chat-2-winrt.md).

1. [Requisitos previos](#prerequisites)
2. [Inicialización](#initialization)
3. [Configuración de usuarios](#configuring-users)
4. [Procesamiento de tramas de datos](#processing-data-frames)
5. [Cambios de estado de procesamiento](#processing-state-changes)
6. [Chat de texto](#text-chat)
7. [Accesibilidad](#accessibility)
8. [UI](#ui)
9. [Silenciar](#muting)
10. [Mala fama de silencio en automático](#bad-reputation-auto-mute)
11. [Privacidad y con privilegios](#privilege-and-privacy)
12. [Cleanup](#cleanup)
13. [Modelo de error](#failure-model)
14. [Cómo configurar escenarios populares](#how-to-configure-popular-scenarios)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a codificar con el juego de charla 2, debe haber configurado AppXManifest la aplicación para declarar la funcionalidad del dispositivo "micrófono". AppXManifest funcionalidades se describen con más detalle en las secciones respectivas de la documentación de platform; el fragmento de código siguiente muestra el nodo de capacidad de dispositivo "micrófono" que debe existir en el nodo de paquete/capacidades, o bien se bloquearán chat:

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <Package ...>
   <Identity ... />
   ...
   <Capabilities>
     <DeviceCapability Name="microphone" />
   </Capabilities>
 </Package>
```

Compilar juegos Chat 2 requiere, incluido el encabezado GameChat2.h principal. Para vincular correctamente, el proyecto debe incluir también GameChat2Impl.h en al menos una unidad de compilación (se recomienda un encabezado precompilado común ya que estas implementaciones de la función de código auxiliar son pequeño y fácil para que el compilador genere como "inline").

La interfaz de juego Chat 2 no requiere un proyecto para elegir entre compilar con C++ / c++ / CX y C++ tradicionales; se puede usar con cualquiera. La implementación no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumirlos fácilmente desde proyectos sin excepciones si lo prefiere. La implementación, sin embargo, las excepciones como medio de informes de error irrecuperable (consulte [modelo error](#failure) para obtener más detalles).

## <a name="initialization"></a>Inicialización

Empezar a interactuar con la biblioteca mediante la inicialización de la instancia de singleton de juego 2 de Chat con parámetros que se aplican a la duración de la inicialización del singleton.

```cpp
chat_manager::singleton_instance().initialize(...);
```

## <a name="configuring-users"></a>Configuración de usuarios

Una vez que se inicializa la instancia, debe agregar los usuarios locales a la instancia 2 de Chat de juego. En este ejemplo, el usuario A representará un usuario local.

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(<user_a_xuid>);
```

También debe agregar los usuarios remotos y los identificadores que se usará para representar el "punto de conexión remoto" que se encuentra el usuario. "Extremo" es una instancia de la aplicación se ejecuta en un dispositivo remoto. En este ejemplo, el usuario B se encuentra en X del punto de conexión, los usuarios de C y D son en X de punto de conexión de punto de conexión Y. arbitrariamente se le asigna el identificador "1" y punto de conexión Y se asigna arbitrariamente identificador "2". Informar al 2 de Chat del juego de los usuarios remotos con estas llamadas:

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(<user_b_xuid>, 1);
chat_user* chatUserC = chat_manager::singleton_instance().add_remote_user(<user_c_xuid>, 2);
chat_user* chatUserD = chat_manager::singleton_instance().add_remote_user(<user_d_xuid>, 2);
```

Ahora configura la relación de comunicación entre cada usuario remoto y el usuario local. En este ejemplo, suponga que el usuario A y usuario B están en el mismo equipo y se permite la comunicación bidireccional. `c_communicationRelationshipSendAndReceiveAll` es una constante definida en GameChat2.h para representar la comunicación bidireccional. Establecer la relación del usuario al usuario B con:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Ahora suponga que los usuarios de C y D son 'espectadores' y se deben permitir a escuchar al usuario A, pero no habla. `c_communicationRelationshipSendAll` es una constante definida en GameChat2.h para representar esta comunicación unidireccional. Establecer las relaciones con:

```cpp
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

Si en cualquier momento hay usuarios remotos que se han agregado a la instancia de singleton, pero aún no ha configurado para comunicarse con los usuarios - eso está bien! Esto puede darse en escenarios donde los usuarios determinan los equipos o cambiar de canal habla arbitrariamente. Juego 2 Chat solo almacenará en caché información (por ejemplo, las relaciones de privacidad y reputación) para los usuarios que se han agregado a la instancia, por lo que es útil informar al 2 de Chat de juego de todos los posibles usuarios incluso si no se hablan a los usuarios locales en un momento determinado en el tiempo.

Por último, suponga que usuario D ha abandonado el juego y debe quitarse de la instancia local de juego 2 Chat. Que se puede hacer con la siguiente llamada:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Una llamada a `chat_manager::remove_user()` puede invalidar el objeto de usuario. Si usas [manipulación de audio en tiempo real](real-time-audio-manipulation.md), consulte el [duraciones de usuarios de Chat](real-time-audio-manipulation.md#chat-user-lifetimes) documentación para obtener más información. En caso contrario, el objeto de usuario se invalida inmediatamente cuando `chat_manager::remove_user()` se llama. Una restricción sutil sobre cuándo se pueden quitar los usuarios se detalla en [los cambios de estado de procesamiento](#state).

## <a name="processing-data-frames"></a>Procesamiento de tramas de datos

2 de Chat de juego no tiene su propia capa de transporte; Esto se debe proporcionar la aplicación. Este complemento se administra mediante llamadas normales, con frecuencia de la aplicación a la `chat_manager::start_processing_data_frames()` y `chat_manager::finish_processing_data_frames()` par de métodos. Estos métodos son cómo juego Chat 2 proporciona los datos salientes a la aplicación. Están diseñados para funcionar rápidamente que puede ser sondea con frecuencia en un subproceso dedicado de red. Esto proporciona un lugar conveniente para recuperar todos los datos en cola sin preocuparse por la imprecisión del tiempo de la red o la complejidad de devolución de llamada multiproceso.

Cuando `chat_manager::start_processing_data_frames()` se llama, todos los datos en cola se notifican en una matriz de `game_chat_data_frame` estructura punteros. Las aplicaciones deben recorrer en iteración la matriz, inspeccionar el destino "puntos de conexión" y usar capa de red de la aplicación para entregar los datos a las instancias de aplicación remota adecuada. Una vez finalizado con todos los `game_chat_data_frame` estructuras, la matriz debe pasarse al 2 de Chat de juego para liberar los recursos mediante una llamada a `chat_manager:finish_processing_data_frames()`. Por ejemplo:

```cpp
uint32_t dataFrameCount;
game_chat_data_frame_array dataFrames;
chat_manager::singleton_instance().start_processing_data_frames(&dataFrameCount, &dataFrames);
for (uint32_t dataFrameIndex = 0; dataFrameIndex < dataFrameCount; ++dataFrameIndex)
{
    game_chat_data_frame const* dataFrame = dataFrames[dataFrameIndex];
    HandleOutgoingDataFrame(
        dataFrame->packet_byte_count,
        dataFrame->packet_buffer,
        dataFrame->target_endpoint_identifier_count,
        dataFrame->target_endpoint_identifiers,
        dataFrame->transport_requirement
        );
}
chat_manager::singleton_instance().finish_processing_data_frames(dataFrames);
```

Con más frecuencia se procesan las tramas de datos, menor será la latencia de audio evidente para el usuario final. El audio se combinan en tramas de datos de 40 ms; Este es el período de sondeo sugeridos.

## <a name="processing-state-changes"></a>Cambios de estado de procesamiento

Juego 2 Chat proporciona actualizaciones para la aplicación, como mensajes de texto recibidos a través de llamadas de la aplicación normal, con frecuencia a la `chat_manager::start_processing_state_changes()` y `chat_manager::finish_processing_state_changes()` par de métodos. Están diseñados para funcionar rápidamente, que se les pueden llamar cada fotograma de gráficos en el bucle de representación de la interfaz de usuario. Esto proporciona un lugar conveniente para recuperar todos los cambios de la cola sin preocuparse por la imprecisión del tiempo de la red o la complejidad de devolución de llamada multiproceso.

Cuando `chat_manager::start_processing_state_changes()` es llamado, se notifican en una matriz de todas las actualizaciones en cola `game_chat_state_change` estructura punteros. Las aplicaciones deben recorrer en iteración la matriz, inspeccionar la estructura de base de su tipo más específico, convierta la estructura de base correspondiente más detallada escriba y, a continuación, controlan esa actualización según corresponda. Una vez finalizado con todos los `game_chat_state_change` objetos disponibles actualmente, se debe pasar esa matriz al 2 de Chat de juego para liberar los recursos mediante una llamada a `chat_manager::finish_processing_state_changes()`. Por ejemplo:

```cpp
uint32_t stateChangeCount;
game_chat_state_change_array gameChatStateChanges;
chat_manager::singleton_instance().start_processing_state_changes(&stateChangeCount, &gameChatStateChanges);

for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; ++stateChangeIndex)
{
    switch (gameChatStateChanges[stateChangeIndex]->state_change_type)
    {
        case game_chat_state_change_type::text_chat_received:
        {
            HandleTextChatReceived(static_cast<const game_chat_text_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        case Xs::game_chat_2::game_chat_state_change_type::transcribed_chat_received:
        {
            HandleTranscribedChatReceived(static_cast<const Xs::game_chat_2::game_chat_transcribed_chat_received_state_change*>(gameChatStateChanges[stateChangeIndex]));
            break;
        }

        ...
    }
}
chat_manager::singleton_instance().finish_processing_state_changes(gameChatStateChanges);
```

Dado que `chat_manager::remove_user()` invalida inmediatamente la memoria asociada a un objeto de usuario, y los cambios de estado pueden contener punteros a objetos de usuario, `chat_manager::remove_user()` no debe llamarse al procesar los cambios de estado.

## <a name="text-chat"></a>Chat de texto

Para enviar el chat de texto, use `chat_user::chat_user_local::send_chat_text()`. Por ejemplo:

```cpp
chatUserA->local()->send_chat_text(L"Hello");
```

2 de Chat de juego, se generará una trama de datos que contiene este mensaje; los puntos de conexión de destino de la trama de datos serán las asociadas a los usuarios que se han configurado para recibir el texto del usuario local. Cuando los datos se procesan mediante los puntos de conexión remotas, el mensaje se expondrá a través de un `game_chat_text_chat_received_state_change`. Al igual que con chat de voz, se respetan las restricciones de privacidad y con privilegios de chat de texto. Si un par de usuarios se han configurado para permitir el chat de texto, pero las restricciones de privilegios o la privacidad no permitir que la comunicación, se quitará el mensaje de texto.

Compatibilidad con entrada de chat de texto y la presentación es necesario para mejorar la accesibilidad (consulte [accesibilidad](#access) para obtener más detalles).

## <a name="accessibility"></a>Accesibilidad

Se requiere compatibilidad con chat de texto de entrada y la presentación. Entrada de texto es necesario porque, incluso en plataformas o géneros de juegos que históricamente no tuvieron la generalizada teclado físico usar, los usuarios pueden configurar el sistema para usar las tecnologías de asistencia texto a voz. De forma similar, la presentación del texto es necesaria porque los usuarios pueden configurar el sistema para usar texto a voz. Estas preferencias se pueden detectar en usuarios locales mediante una llamada a la `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` y `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` métodos respectivamente, y puede que desee habilitar condicionalmente los mecanismos de texto.

### <a name="text-to-speech"></a>Texto a voz

Cuando un usuario tiene texto a voz habilitada, `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` devuelve 'true'. Cuando se detecta este estado, la aplicación debe proporcionar un método de entrada de texto. Una vez que la entrada de texto que se proporcionó mediante un teclado real o virtual, pase la cadena para el `chat_user::chat_user_local::synthesize_text_to_speech()` método. Juego 2 Chat detectará y síntesis de datos de audio en función de la cadena y la preferencia del usuario accesible de voz. Por ejemplo:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

El audio sintetizado como parte de esta operación se transportarán a todos los usuarios que se han configurado para recibir audio de este usuario local. Si `chat_user::chat_user_local::synthesize_text_to_speech()` se llama en un usuario que no tengan texto a voz habilitada 2 de Chat de juego no realiza ninguna acción.

### <a name="speech-to-text"></a>Voz a texto

Cuando un usuario tiene habilitada, de voz a texto `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` devolverá true. Cuando se detecta este estado, la aplicación debe estar preparada para proporcionar la que interfaz de usuario asociadas con mensajes de chat de transcripción. Juego 2 Chat automáticamente transcribir audio de cada usuario remoto y exponerlo a través de un `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` es una clase de Xbox One específicamente diseñada para proporcionar una representación sencilla de chat de texto en el juego con un enfoque en las tecnologías de voz a texto.

### <a name="speech-to-text-performance-considerations"></a>Consideraciones de rendimiento de voz a texto

Cuando está habilitada voz a texto, la instancia 2 de Chat de juego en cada dispositivo remoto inicia una conexión de socket web con el punto de conexión de servicios de voz. Cada cliente remoto de juego Chat 2 carga audio en el punto de conexión de servicios de voz a través de dicho websocket; en ocasiones, el punto de conexión de servicios de voz devuelve un mensaje de transcripción al dispositivo remoto. El dispositivo remoto, a continuación, envía el mensaje de transcripción (es decir, un mensaje de texto) en el dispositivo local, donde se asigna al mensaje transcrito juego Chat de 2 a la aplicación para representar.

Por lo tanto, el costo de rendimiento principal de voz a texto es el uso de la red. La mayoría del tráfico de red es la carga de audio codificado. El socket Web carga audio que ya se ha codificado por 2 de Chat de juego en la ruta de acceso de chat de voz "normal"; la aplicación tiene control sobre la velocidad de bits a través de `chat_manager::set_audio_encoding_type_and_bitrate`.

## <a name="ui"></a>Interfaz de usuario

Se recomienda que en cualquier parte los jugadores se muestran, especialmente en una lista de nombres de jugadores, como un panel de resultados, también mostrar iconos de Silenciar de habla como comentarios del usuario. Esto se realiza mediante una llamada a `chat_user::chat_indicator()` para recuperar un `game_chat_user_chat_indicator` que representa el estado actual, la instantáneo de chat para que el Reproductor. El ejemplo siguiente muestra el valor de indicador de un `chat_user` objeto apunta a la variable 'chatUserA' para determinar un valor constante de icono determinado para asignar a una variable 'iconToShow':

```cpp
switch (chatUserA->chat_indicator())
{
   case game_chat_user_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case game_chat_user_chat_indicator::local_microphone_muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

El valor indicado por `xim_player::chat_indicator()` se espera que cambie con frecuencia como reproductores start y stop hablar, por ejemplo. Está diseñado para admitir aplicaciones de sondeo cada fotograma de la interfaz de usuario como resultado.

## <a name="muting"></a>Silenciar

El `chat_user::chat_user_local::set_microphone_muted()` método puede utilizarse para alternar el estado del audio del micrófono de un usuario local. Cuando el micrófono está silenciado, audio desde ese micrófono no se capturará. Si el usuario está en un dispositivo compartido, como la Kinect, el estado del audio se aplica a todos los usuarios.

El `chat_user::chat_user_local::microphone_muted()` método puede utilizarse para recuperar el estado del audio del micrófono de un usuario local. Este método refleja solo si se desactivó el micrófono del usuario local en el software a través de una llamada a `chat_user::chat_user_local::set_microphone_muted()`; este método no refleja la controla un silencio de hardware, por ejemplo, mediante un botón de auriculares del usuario. No hay ningún método para recuperar el estado del audio de hardware de dispositivo de audio de un usuario a través del 2 de Chat de juego.

El `chat_user::chat_user_local::set_remote_user_muted()` método puede utilizarse para alternar el estado del audio de un usuario remoto con respecto a un determinado usuario local. Cuando el usuario remoto está desactivado, el usuario local no oír el audio o recibir cualquier mensaje de texto del usuario remoto.

## <a name="bad-reputation-auto-mute"></a>Mala fama de silencio en automático

Normalmente, los usuarios remotos se iniciará reactivó el micrófono. 2 de Chat de juego se iniciará a los usuarios en un estado desactivado cuando (1) el usuario remoto no amigos con el usuario local, y (2) el usuario remoto tiene una marca de mala reputación. Cuando los usuarios están en silencio debido a esta operación, `chat_user::chat_indicator()` devolverá `game_chat_user_chat_indicator::reputation_restricted`. Este estado se reemplazará por la primera llamada a `chat_user::chat_user_local::set_remote_user_muted()` que incluye el usuario remoto como el usuario de destino.

## <a name="privilege-and-privacy"></a>Privacidad y con privilegios

Encima de la relación de comunicación configurada por el juego, juego 2 Chat aplica las restricciones de privacidad y con privilegios. Juego 2 Chat realiza búsquedas de restricción de privilegios y privacidad cuando se agrega un usuario por primera vez; el usuario `chat_user::chat_indicator()` siempre devolverá `game_chat_user_chat_indicator::silent` hasta que se completen las operaciones. Si la comunicación con un usuario se ve afectada por un privilegio o la privacidad restricción, el usuario `chat_user::chat_indicator()` devolverá `game_chat_user_chat_indicator::platform_restricted`. Aplican restricciones de comunicación de la plataforma a la charla de voz y texto; nunca habrá una instancia de chat de texto está bloqueado por una restricción de la plataforma donde pero chat de voz es no, o viceversa.

`chat_user::chat_user_local::get_effective_communication_relationship()` puede usarse para ayudar a distinguir cuando los usuarios no pueden comunicarse debido a privilegios incompleta y las operaciones de privacidad. Devuelve la relación de comunicación que se aplica en forma de juego Chat 2 `game_chat_communication_relationship_flags` y la razón por la relación no puede ser igual a la relación configurada en forma de `game_chat_communication_relationship_adjuster`. Por ejemplo, si las operaciones de búsqueda están aún en curso, el `game_chat_communication_relationship_adjuster` será `game_chat_communication_relationship_adjuster::intializing`. Se espera que este método para usarse en escenarios de desarrollo y depuración; no debe usarse para influir en la interfaz de usuario (consulte [UI](#UI)).

## <a name="cleanup"></a>Limpieza

Cuando la aplicación ya no necesita las comunicaciones a través de juego Chat 2, debe llamar a `chat_manager::cleanup()`. Esto permite juego Chat 2 reclamar recursos asignados para administrar las comunicaciones.

## <a name="failure-model"></a>Modelo de error

La implementación de juego Chat 2 no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumirlos fácilmente desde proyectos sin excepciones si lo prefiere. 2 de Chat de juegos, sin embargo, las excepciones para informar sobre errores irrecuperables. Estos errores son el resultado del uso incorrecto de la API, como agregar un usuario a la instancia de Chat de juego antes de inicializar la instancia o a un objeto de usuario después de que se ha quitado de la instancia 2 de Chat de juego. Estos errores deben capturarse en etapas tempranas del desarrollo y se pueden corregir modificando el modelo utilizado para interactuar con Game Chat 2. Cuando se produce un error de ese tipo, se imprime una sugerencia sobre el motivo del error al depurador antes de que se produce la excepción.

## <a name="how-to-configure-popular-scenarios"></a>Cómo configurar escenarios populares

### <a name="push-to-talk"></a>Hablar de inserción

Hablar de inserción debe implementarse con `chat_user::chat_user_local::set_microphone_muted()`. Llame a `set_microphone_muted(false)` para permitir voz y `set_microphone_muted(true)` restringirla. Este método proporcionará la respuesta de latencia más baja del 2 de Chat de juego.

### <a name="teams"></a>Equipos

Suponga que el usuario A y usuario B están en azul de equipo y usuario C y d. de usuario se encuentran en el equipo rojo. Cada usuario está en una instancia única de la aplicación.

En el dispositivo del usuario:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
chatUserA->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserA->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

En el dispositivo del usuario B:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipSendAndReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

En el dispositivo del usuario C:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAndReceiveAll);
```

En el dispositivo del usuario D:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAndReceiveAll);
```

### <a name="broadcast"></a>Transmisión

Suponga que el usuario A es el líder de pedidos y solo pueden escuchar a los usuarios B, C y D. Cada player se encuentra en un único dispositivo.

En el dispositivo del usuario:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserC, c_communicationRelationshipSendAll);
chatUserA->local()->set_communication_relationship(chatUserD, c_communicationRelationshipSendAll);
```

En el dispositivo del usuario B:

```cpp
chatUserB->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserB->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
chatUserB->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

En el dispositivo del usuario C:

```cpp
chatUserC->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserC->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserC->local()->set_communication_relationship(chatUserD, game_chat_communication_relationship_flags::none);
```

En el dispositivo del usuario D:

```cpp
chatUserD->local()->set_communication_relationship(chatUserA, c_communicationRelationshipReceiveAll);
chatUserD->local()->set_communication_relationship(chatUserB, game_chat_communication_relationship_flags::none);
chatUserD->local()->set_communication_relationship(chatUserC, game_chat_communication_relationship_flags::none);
```
