---
title: Migración de juegos charla 2
description: Obtenga información sobre cómo migrar código existente de Chat de juego para utilizar juego de charla 2.
ms.date: 05/02/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, chat juego 2, chat de juegos, comunicación por voz
ms.localizationpriority: medium
ms.openlocfilehash: e963210091694a07114f10d5a3dc531a353621df
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653590"
---
# <a name="migration-from-game-chat-to-game-chat-2"></a>Migración de juegos Chat a juegos Chat 2

Este documento detalla las similitudes entre el Chat de juego y 2 de Chat de juego y cómo migrar de Chat de juego al 2 de Chat de juego. Por lo tanto, es para los títulos que tienen una implementación existente de Chat de juego que desea migrar a 2 de Chat de juego. Si aún no tiene una implementación de Chat de juego, el punto de partida sugerido es [con juego de charla 2](using-game-chat-2.md). 

## <a name="preface"></a>Prefacio

La API de Chat de juego original es una API de WinRT que expone un concepto de usuarios de chat y los canales de voz para ayudar en la implementación de escenarios de chat de voz en el juego de Xbox Live. La API de Chat de juego se basa en la API de Chat, que a su vez es una API de WinRT que expone un concepto de usuarios de chat y canales de voz durante la necesidad de bajo nivel administración de dispositivos de audio. Juego de Chat de 2 es el sucesor de las API de Chat y Chat de juego original: se ha diseñado para ser una API más sencilla para escenarios de chat básica, como la comunicación de equipo, al tiempo que ofrece más flexibilidad para escenarios de chat avanzadas, como el estilo de emisión la comunicación y la manipulación de audio en tiempo real. Chat de juego y juego 2 Chat rellenar el nicho de la mismo: las API proporcionan un método práctico para la integración de Xbox Live habilitado, el chat de voz en el juego, mientras que el título proporciona una capa de transporte para transmitir paquetes de datos hacia y desde instancias remotas de juegos de Chat o juego Chat de 2.

La API del 2 de Chat de juego tiene muchas ventajas respecto a la charla de juego original y API de Chat. Algunos aspectos destacados incluyen:
* Una flexible orientado al usuario API que no está restringido al modelo de "canales".
* Mejoras en el ancho de banda debido al filtrado de paquetes por orígenes de datos y receptores de datos.
* Una restricción interna a 2 subprocesos de larga duración con la afinidad de la aplicación configurable.
* Una API disponible en los formularios de C++ y WinRT.
* Modelo de consumo simplificada alinear con un patrón de "trabajo".
* Enlaces de memoria para redirigir las asignaciones de juego Chat de 2 a través de un asignador personalizado.
* UWP idéntico + encabezados de la aplicación de recurso exclusivo (ERA) para una experiencia de desarrollo de cross-plat más cómodo.

Este documento detalla las similitudes entre el Chat de juego y 2 de Chat de juego y cómo migrar de Chat de juego a la API de C++ de juego Chat 2. Si está interesado en la migración de Chat de juego a la API de WinRT 2 de Chat de juego, se recomienda que lea este documento para entender cómo se asignan conceptos de Chat de juego al 2 de Chat de juego y, a continuación, vea [utilizando juego Chat 2 WinRT proyecciones](using-game-chat-2-winrt.md) para el patrones específicos de WinRT. El código de ejemplo para el Chat de juego original en este documento se usará C++ / c++ / CX.

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

La interfaz de juego Chat 2 no requiere un proyecto para elegir entre compilar con C++ / c++ / CX y C++ tradicionales; se puede usar con cualquiera. La implementación no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumirlos fácilmente desde proyectos sin excepciones si lo prefiere. La implementación, sin embargo, las excepciones como medio de informes de error irrecuperable (consulte [modelo error](#failure-model-and-debugging) para obtener más detalles).

## <a name="initialization"></a>Inicialización

### <a name="game-chat"></a>Chat de juegos

Interactuar con el Chat de juego original se realiza a través de la `ChatManager` clase. El ejemplo siguiente muestra cómo construir un `ChatManager` con los parámetros predeterminados de instancia:

```cpp
auto chatManager = ref new ChatManager();
```

### <a name="game-chat-2"></a>Chat de juego 2

Toda la interacción con el juego de charla 2 se realiza a través de juego Chat 2 `chat_manager` singleton. El singleton debe inicializarse antes de que comience cualquier interacción significativa con la biblioteca. El singleton requiere que se especifique el número máximo de usuarios simultáneos chat locales y remotas en tiempo de inicialización; Esto es porque juego Chat 2 asigna memoria proporcional al número esperado de los usuarios con antelación. El ejemplo siguiente muestra cómo inicializar la instancia de singleton cuando el número máximo de usuarios simultáneos chat locales y remotos será cuatro:

```cpp
chat_manager::singleton_instance().initialize(4);
```

Hay varios parámetros opcionales que se pueden especificar durante la inicialización, como el valor predeterminado representar el volumen de los usuarios de la conversación remota y si debe realizar la conversión automática de texto a voz 2 de Chat de juego. Para obtener más información acerca de estos parámetros, consulte la documentación de `chat_manager::initialize()`.

## <a name="configuring-users"></a>Configuración de usuarios

### <a name="game-chat"></a>Chat de juegos

Adición de usuarios locales a la API de Chat de juego original se realiza a través de `ChatManager::AddLocalUserToChatChannelAsync()`. Agregar el usuario a varios canales de chat requiere varias llamadas, cada uno especifica un canal diferente. El ejemplo siguiente muestra cómo agregar el usuario con xuid "myXuid" para 0 del canal:

```cpp
auto asyncOperation = chatManager->AddLocalUserToChatChannelAsync(0, L"myXuid");
```

Los usuarios remotos no se agregan directamente a la instancia. Cuando un dispositivo remoto aparece en la red del título, el título crea un objeto que representa el dispositivo remoto e informa al juego de Chat del nuevo dispositivo a través de `ChatManager::HandleNewRemoteConsole()`. El título también debe proporcionar un método de comparación de los objetos que representan los dispositivos remotos para charlar juego implementando `CompareUniqueConsoleIdentifiersHandler`. El ejemplo siguiente muestra cómo proporcionar este delegado juego chat, suponiendo que `Platform::String` objetos se utilizan para representar los dispositivos remotos y un nuevo dispositivo representado por la cadena `L"1"` se unió a red del título:

```cpp
auto token = chatManager->OnCompareUniqueConsoleIdentifiers +=
    ref new CompareUniqueConsoleIdentifiersHandler(
        [this](Platform::Object^ obj1, Platform::Object^ obj2)
    {
        return (static_cast<Platform::String^>(obj1)->Equals(static_cast<Platform::String^>(obj2)));
    });

...

Platform::String^ newDeviceIdentifier = ref new Platform::String(L"1");
chatManager->HandleNewRemoteConsole(newDeviceIdentifier);
```

Una vez que el juego de charla se haya informado de este dispositivo, generará los paquetes que contienen información acerca de los usuarios y proporcionar estos paquetes en el título para la transmisión a la instancia de Chat de juego en el dispositivo remoto. De forma similar, la instancia de Chat de juego en el dispositivo remoto generará los paquetes que contienen información acerca de los usuarios en ese dispositivo remoto que se va a transmitir el título a la instancia local de Chat de juego. Cuando la instancia local recibe paquetes que contienen información acerca de nuevos usuarios remotos, los usuarios remotos nuevos se agregan a la lista de la instancia local de usuarios, disponibles a través de `ChatManager::GetUsers()`.

Quitando usuarios de la instancia de Chat de juego se realiza a través de llamadas similar a `ChatManager::RemoveLocalUserFromChatChannelAsync()` y `ChatManager::RemoveRemoteConsoleAsync()`

### <a name="game-chat-2"></a>Chat de juego 2

Adición de usuarios locales al juego Chat 2 se realiza de forma sincrónica a través `chat_manager::add_local_user()`. En este ejemplo, el usuario A representará un usuario local con el Id. de usuario de Xbox `L"myLocalXboxUserId"`:

```cpp
chat_user* chatUserA = chat_manager::singleton_instance().add_local_user(L"myLocalXboxUserId");
```

Tenga en cuenta que el usuario no es agregado a un canal determinado - 2 de Chat de juego se usa un concepto de "una relación de comunicación" en lugar de los canales de administrar si los usuarios pueden hablar y escuchar entre sí. El método de configurar una relación de comunicación se abordará más adelante en esta sección.

Similares a local, los usuarios remotos se agregan usuarios sincrónicamente a la charla de juego local instancia 2. Los usuarios remotos simultáneamente deben asociarse con los identificadores que se usará para representar el dispositivo remoto. Juego 2 Chat hace referencia a una instancia de la aplicación se ejecuta en un dispositivo remoto como un "punto de conexión". En este ejemplo, el usuario B será un usuario con el Id. de usuario de Xbox `L"remoteXboxUserId"` en un punto de conexión representado por el entero `1`.

```cpp
chat_user* chatUserB = chat_manager::singleton_instance().add_remote_user(L"remoteXboxUserId", 1);
```

Una vez que los usuarios se agregaron a la instancia, debe configurar la relación de comunicación de"" entre cada usuario remoto y cada usuario local. En este ejemplo, suponga que el usuario A y usuario B están en el mismo equipo y se permite la comunicación bidireccional. `c_communicationRelationshiSendAndReceiveAll` es una constante definida en GameChat2.h para representar la comunicación bidireccional. La relación se puede configurar con:

```cpp
chatUserA->local()->set_communication_relationship(chatUserB, c_communicationRelationshipSendAndReceiveAll);
```

Por último, suponga que el usuario B ha abandonado el juego y debe quitarse de la instancia local de Chat de juego. Que se realiza sincrónicamente con la siguiente llamada:

```cpp
chat_manager::singleton_instance().remove_user(chatUserD);
```

Consulte [utilizando juego Chat 2: configurar los usuarios](using-game-chat-2.md#configuring-users) para obtener más ejemplos o la referencia para los métodos de API individuales para obtener más información.

## <a name="processing-data"></a>Procesar datos

### <a name="game-chat"></a>Chat de juegos

Chat de juego no tiene su propia capa de transporte; Esto se debe proporcionar la aplicación. Los paquetes salientes se controlan mediante la suscripción a la `OnOutgoingChatPacketReady` eventos e inspeccionar los argumentos para determinar los requisitos de transporte y de destino del paquete. El ejemplo siguiente muestra cómo suscribirse al evento y reenviar los argumentos de la `HandleOutgoingPacket()` método implementado por el título:

```cpp
auto token = chatManager->OnOutgoingChatPacketReady +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::ChatPacketEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::ChatPacketEventArgs^ args)
    {
        this->HandleOutgoingPacket(args);
    });
```

Los paquetes entrantes se envían a una charla de juego a través de `ChatManager::ProcessingIncomingChatMessage()`. Deben proporcionarse el búfer sin formato de paquete y el identificador de dispositivo remoto. El ejemplo siguiente muestra cómo enviar un paquete que se almacena en el equipo local `packetBuffer` y el identificador de dispositivo remoto almacenada en la variable local `remoteIdentifier`.

```cpp
Platform::String^ remoteIdentifier = /* The identifier associated with the device that generated this packet */
Windows::Storage::Streams::IBuffer^ packetBuffer = /* The incoming chat packet */

chatManager->ProcessIncomingChatMessage(packetBuffer, remoteIdentifier);
```

### <a name="game-chat-2"></a>Chat de juego 2

De forma similar, 2 de Chat de juego no tiene su propia capa de transporte; Esto se debe proporcionar la aplicación. Los paquetes salientes se controlan mediante llamadas normales, con frecuencia de la aplicación a la `chat_manager::start_processing_data_frames()` y `chat_manager::finish_processing_data_frames()` par de métodos. Estos métodos son cómo juego Chat 2 proporciona los datos salientes a la aplicación. Están diseñados para funcionar rápidamente que puede ser sondea con frecuencia en un subproceso dedicado de red. Esto proporciona un lugar conveniente para recuperar todos los datos en cola sin preocuparse por la imprecisión del tiempo de la red o la complejidad de devolución de llamada multiproceso.

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

Datos entrantes se envían a 2 de Chat de juego a través de `chat_manager::processing_incoming_data()`. Deben proporcionarse el búfer de datos y el identificador del extremo remoto. El ejemplo siguiente muestra cómo enviar un paquete de datos que se almacena en la variable local `dataFrame` y el identificador de punto de conexión remota almacenada en la variable local `remoteEndpointIdentifier`:

```cpp
uin64_t remoteEndpointIdentier = /* The identifier associated with the endpoint that generated this packet */
uint32_t dataFrameSize = /* The size of the incoming data frame, in bytes */
uint8_t* dataFrame = /* A pointer to the buffer containing the incoming data */

chatManager::singleton_instance().process_incoming_data(remoteEndpointIdentifier, dataFrameSize, dataFrame);
```

## <a name="processing-events"></a>Procesamiento de eventos

### <a name="game-chat"></a>Chat de juegos

Chat de juego utiliza un modelo de eventos para informar a la aplicación cuando ocurre algo interesante: por ejemplo, la recepción de un mensaje de texto o la modificación de la preferencia de accesibilidad de un usuario. La aplicación debe suscribirse a e implementar un controlador para cada evento de interés. En este ejemplo se muestra cómo suscribirse a la `OnTextMessageReceived` evento y reenviar los argumentos para el `OnTextChatReceived()` o `OnTranscribedChatReceived()` métodos implementados por la aplicación:

```cpp
auto token = chatManager->OnTextMessageReceived +=
    ref new Windows::Foundation::EventHandler<Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^>(
        [this](Platform::Object^, Microsoft::Xbox::GameChat::TextMessageReceivedEventArgs^ args)
    {
        if (args->ChatTextMessageType != ChatTextMessageType::TranscribedSpeechMessage)
        {
            this->OnTextChatReceived(args);
        }
        else
        {
            this->OnTranscribedChatReceived(args);
        }
    });
```

### <a name="game-chat-2"></a>Chat de juego 2

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

### <a name="game-chat"></a>Chat de juegos

Para enviar texto chat con Chat de juego, `GameChatUser::GenerateTextMessage()` se puede usar. El ejemplo siguiente muestra cómo enviar un mensaje de texto de chat con un usuario de charla local representado por la `chatUser` variable:

```cpp
chatUser->GenerateTextMessage(L"Hello", false);
```

El segundo parámetro booleano controla la conversión de texto a voz. Para obtener más información, consulte [accesibilidad](#accessibility). Chat de juegos, a continuación, genera un paquete de chat que contiene este mensaje. Las instancias remotas de Chat de juego se notificará el mensaje de texto a través de la `OnTextMessageReceived` eventos.

### <a name="game-chat-2"></a>Chat de juego 2

Para enviar el chat de texto con 2 de Chat de juego, utilice `chat_user::chat_user_local::send_chat_text()`. En el ejemplo siguiente se muestra cómo enviar un mensaje de texto de chat con un usuario de charla local representado por la `chatUser` variable:

```cpp
chatUser->local()->send_chat_text(L"Hello");
```

2 de Chat de juego, se generará una trama de datos que contiene este mensaje; los puntos de conexión de destino de la trama de datos serán las asociadas a los usuarios que se han configurado para recibir el texto del usuario local. Cuando los datos se procesan mediante los puntos de conexión remotas, el mensaje se expondrá a través de un `game_chat_text_chat_received_state_change`. Al igual que con chat de voz, se respetan las restricciones de privacidad y con privilegios de chat de texto. Si un par de usuarios se han configurado para permitir el chat de texto, pero las restricciones de privilegios o la privacidad no permitir que la comunicación, se eliminarán en modo silencioso el mensaje de texto.

Compatibilidad con entrada de chat de texto y la presentación es necesario para mejorar la accesibilidad (consulte [accesibilidad](#accessibility) para obtener más detalles).

## <a name="accessibility"></a>Accesibilidad

Compatibilidad con chat de texto de entrada y la presentación es necesaria para Chat de juego y 2 de Chat de juego. Entrada de texto es necesario porque, incluso en plataformas o géneros de juegos que históricamente no tuvieron la generalizada teclado físico usar, los usuarios pueden configurar el sistema para usar las tecnologías de asistencia texto a voz. De forma similar, la presentación del texto es necesaria porque los usuarios pueden configurar el sistema para usar texto a voz. Chat de juego y de juego Chat 2 proporcionan métodos de detección y respetar las preferencias de accesibilidad de un usuario; es posible que desea habilitar condicionalmente basados en la configuración de mecanismos de texto.

### <a name="text-to-speech---game-chat"></a>Texto a voz-juegos de Chat

Cuando un usuario tiene texto a voz habilitada, `GameChatUser::HasRequestedSynthesizedAudio()` devolverá true. Cuando se detecta este estado, `GameChatUser::GenerateTextMessage()` además generará texto a voz de audio que se inserta en la secuencia de audio asociada con el usuario local. El ejemplo siguiente muestra cómo enviar un mensaje de texto de chat con un usuario local representado por la `chatUser` variable:

```cpp
chatUser->GenerateTextMessage(L"Hello", true);
```

El segundo parámetro booleano se usa para permitir que la aplicación invalidar la preferencia de texto a voz: 'true' indica que el Chat de juego debe generar texto a voz de audio cuando se ha habilitado la preferencia del usuario texto a voz, mientras que 'false' indica que juego Chat nunca debe generar texto a voz de audio del mensaje.

### <a name="text-to-speech---game-chat-2"></a>Texto a voz-Chat juego 2

Cuando un usuario tiene texto a voz habilitada, `chat_user::chat_user_local::text_to_speech_conversion_preference_enabled()` devuelve 'true'. Cuando se detecta este estado, la aplicación debe proporcionar un método de entrada de texto. Una vez que la entrada de texto que se proporcionó mediante un teclado real o virtual, pase la cadena para el `chat_user::chat_user_local::synthesize_text_to_speech()` método. Juego 2 Chat detectará y síntesis de datos de audio en función de la cadena y la preferencia del usuario accesible de voz. Por ejemplo:

```cpp
chat_userA->local()->synthesize_text_to_speech(L"Hello");
```

El audio sintetizado como parte de esta operación se transportarán a todos los usuarios que se han configurado para recibir audio de este usuario local. Si `chat_user::chat_user_local::synthesize_text_to_speech()` se llama en un usuario que no tengan texto a voz habilitada 2 de Chat de juego no realiza ninguna acción.

### <a name="speech-to-text---game-chat"></a>Voz a texto - juegos de Chat

Cuando un usuario tiene habilitada, de voz a texto `GameChatUser::HasRequestSynthesizedAudio()` devuelve 'true'. Cuando se detecta este estado, Chat de juego transcribir el audio de audio de cada usuario remoto automáticamente y exponerlo a través de la `OnTextMessageReceived` eventos. Cuando el `OnTextMessageReceived` desencadena el evento debido a la recepción de un mensaje de transcripción, el `TextMessageReceivedEventArgs` indicará un mensaje de tipo `ChatTextMessageType::TranscribedSpeechMessage`.

### <a name="speech-to-text---game-chat-2"></a>Voz a texto - Chat juego 2

Cuando un usuario tiene habilitada, de voz a texto `chat_user::chat_user_local::speech_to_text_conversion_preference_enabled()` devolverá true. Cuando se detecta este estado, la aplicación debe estar preparada para proporcionar la que interfaz de usuario asociadas con mensajes de chat de transcripción. Juego 2 Chat automáticamente transcribir audio de cada usuario remoto y exponerlo a través de un `game_chat_transcribed_chat_received_state_change`.

> `Windows::Xbox::UI::Accessibility` es una clase de Xbox One específicamente diseñada para proporcionar una representación sencilla de chat de texto en el juego con un enfoque en las tecnologías de voz a texto.

Consulte [con juego de charla 2 - consideraciones de rendimiento de voz a texto](using-game-chat-2.md#speech-to-text-performance-considerations) para obtener más detalles sobre las consideraciones de rendimiento de voz a texto.

## <a name="ui"></a>Interfaz de usuario

Se recomienda que en cualquier parte los jugadores se muestran, especialmente en una lista de nombres de jugadores, como un panel de resultados, también mostrar iconos de Silenciar de habla como comentarios del usuario. Juego 2 Chat presenta un indicador combinado que proporciona una manera más sencilla de determinar los elementos correspondientes de la interfaz de usuario para mostrar.

### <a name="game-chat"></a>Chat de juegos

Un `GameChatUser` tiene tres propiedades que normalmente se inspeccionan al determinar los elementos correspondientes de la interfaz de usuario - `GameChatUser::TalkingMode`, `GameChatUser::IsMuted`, y `GameChatUser::RestrictionMode`. En el ejemplo siguiente se muestra una posible heurística para determinar un valor constante de icono determinado para asignar a un `iconToShow` variable desde un `GameChatUser` objeto apunta a la variable 'chatUser'.

```cpp
if (chatUser->RestrictionMode != None)
{
    if (!chatUser->IsMuted)
    {
        if (chatUser->TalkingMode != NotTalking)
        {
            iconToShow = Icon_ActiveSpeaker;
        }
        else
        {
            iconToShow = Icon_InactiveSpeaker;
        }
    }
    else
    {
        iconToShow = Icon_MutedSpeaker;
    }
}
else
{
    iconToShow = Icon_RestrictedSpeaker;
}
```

### <a name="game-chat-2"></a>Chat de juego 2

Juego de Chat de 2 tiene un fusionados `game_chat_user_chat_indicator` utilizados para representar el estado actual, la instantáneo de chat para un reproductor. Este valor se recupera mediante una llamada a `chat_user::chat_indicator()`. El ejemplo siguiente muestra el valor de indicador de un `chat_user` objeto apunta a la variable 'chatUser' para determinar un valor constante de icono determinado para asignar a una variable 'iconToShow':

```cpp
switch (chatUser->chat_indicator())
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

## <a name="muting"></a>Silenciar

## <a name="game-chat"></a>Chat de juegos

Silenciar o reactivación de un usuario de una conversación de juego se realiza a través de una llamada a `ChatManager::MuteUserFromAllChannels()` o `ChatManager::UnMuteUIserFromAllChannels()`, respectivamente. Se puede recuperar el estado del audio de un usuario mediante la inspección `GameChatUser::IsMuted` o `GameChatUser::IsLocalUserMuted`.

## <a name="game-chat-2"></a>Chat de juego 2

El `chat_user::chat_user_local::set_microphone_muted()` método puede utilizarse para alternar el estado del audio del micrófono de un usuario local. Cuando el micrófono está silenciado, audio desde ese micrófono no se capturará. Si el usuario está en un dispositivo compartido, como la Kinect, el estado del audio se aplica a todos los usuarios.

El `chat_user::chat_user_local::microphone_muted()` método puede utilizarse para recuperar el estado del audio del micrófono de un usuario local. Este método refleja solo si se desactivó el micrófono del usuario local en el software a través de una llamada a `chat_user::chat_user_local::set_microphone_muted()`; este método no refleja la controla un silencio de hardware, por ejemplo, mediante un botón de auriculares del usuario. No hay ningún método para recuperar el estado del audio de hardware de dispositivo de audio de un usuario a través del 2 de Chat de juego.

El `chat_user::chat_user_local::set_remote_user_muted()` método puede utilizarse para alternar el estado del audio de un usuario remoto con respecto a un determinado usuario local. Cuando el usuario remoto está desactivado, el usuario local no oír el audio o recibir cualquier mensaje de texto del usuario remoto.

## <a name="bad-reputation-auto-mute"></a>Mala fama de silencio en automático

Normalmente, los usuarios remotos se iniciará reactivó el micrófono. Chat de juego y de juego Chat 2 tienen una característica de "mala reputación automática silencio". Esto significa que los usuarios comenzarán en un estado desactivado cuando (1) el usuario remoto no amigos con un usuario local, y (2) el usuario remoto tiene una marca de mala reputación. 2 de Chat de juego ofrece comentarios cuando un usuario está silenciado debido a esta característica. Consulte [con juego de charla 2 - mala reputación automática silencio](using-game-chat-2.md#bad-reputation-auto-mute) para obtener más información.

## <a name="privilege-and-privacy"></a>Privacidad y con privilegios

Chat de juego y de juego Chat 2 aplican Xbox Live con privilegios y privacidad las restricciones sobre las relaciones de comunicación o canal administradas por la aplicación. 2 de Chat de juego además proporciona información de diagnóstico para determinar exactamente cómo la restricción afecta a la dirección de audio (por ejemplo, si una restricción de audio audio es unidireccionales o bidireccionales).

### <a name="game-chat"></a>Chat de juegos

Chat de juego expone información de privacidad y con privilegios a través de la `RestrictionMode` propiedad. Se puede recuperar mediante la inspección `GameChatUser::RestrictionMode`.

### <a name="game-chat-2"></a>Chat de juego 2

Juego 2 Chat realiza búsquedas de restricción de privilegios y privacidad cuando se agrega un usuario por primera vez; el usuario `chat_user::chat_indicator()` siempre devolverá `game_chat_user_chat_indicator::silent` hasta que se completen las operaciones. Si la comunicación con un usuario se ve afectada por un privilegio o la privacidad restricción, el usuario `chat_user::chat_indicator()` devolverá `game_chat_user_chat_indicator::platform_restricted`. Aplican restricciones de comunicación de la plataforma a la charla de voz y texto; nunca habrá una instancia de chat de texto está bloqueado por una restricción de la plataforma donde pero chat de voz es no, o viceversa.

`chat_user::chat_user_local::get_effective_communication_relationship()` puede usarse para ayudar a distinguir cuando los usuarios no pueden comunicarse debido a privilegios incompleta y las operaciones de privacidad. Devuelve la relación de comunicación que se aplica en forma de juego Chat 2 `game_chat_communication_relationship_flags` y la razón por la relación no puede ser igual a la relación configurada en forma de `game_chat_communication_relationship_adjuster`. Por ejemplo, si las operaciones de búsqueda están aún en curso, el `game_chat_communication_relationship_adjuster` será `game_chat_communication_relationship_adjuster::intializing`. Se espera que este método para usarse en escenarios de desarrollo y depuración; no debe usarse para influir en la interfaz de usuario (consulte [UI](#UI)).

## <a name="cleanup"></a>Limpieza

Del original juego Chat `ChatManager` es una clase en tiempo de ejecución de WinRT - interfaz contada de referencia. Por lo tanto, los recursos de memoria del que se recuperan cuando el último recuento de referencias llega a cero y se limpia. Interacción con la API de C++ del 2 de Chat de juego se realiza a través de una instancia de singleton. Cuando la aplicación ya no necesita las comunicaciones a través de juego Chat 2, debe llamar a `chat_manager::cleanup()`. Esto permite juego Chat 2 liberar inmediatamente todos los recursos asignados para administrar las comunicaciones. Para obtener más información sobre la API de WinRT y administración de recursos de juego Chat 2, consulte [utilizando juego Chat 2 WinRT proyecciones](using-game-chat-2-winrt.md#cleanup).

## <a name="failure-model-and-debugging"></a>Modelo de error y la depuración

El Chat de juego original es una API de WinRT; por lo tanto, notifican errores a través de las excepciones. No se ha producido errores o advertencias se notifican a través de una devolución de llamada de depuración opcionales. Juego de C++ API 2 de charla no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumirlos fácilmente desde proyectos sin excepciones si lo prefiere. 2 de Chat de juegos, sin embargo, las excepciones para informar sobre errores irrecuperables. Estos errores son el resultado del uso incorrecto de la API, como agregar un usuario a la instancia de Chat de juego antes de inicializar la instancia o a un objeto de usuario después de que se ha quitado de la instancia de Chat de juego. Estos errores deben capturarse en etapas tempranas del desarrollo y se pueden corregir modificando el modelo utilizado para interactuar con Game Chat 2. Cuando se produce un error de ese tipo, se imprime una sugerencia sobre el motivo del error al depurador antes de que se produce la excepción. API de WinRT juego Chat del 2 sigue el patrón de WinRT de informar de errores a través de las excepciones.

## <a name="how-to-configure-popular-scenarios"></a>Cómo configurar escenarios populares

Consulte [con 2 de la conversación de juego - cómo configurar escenarios populares](using-game-chat-2.md#how-to-configure-popular-scenarios) para obtener ejemplos sobre cómo configurar escenarios populares como hablar de inserción, los equipos y escenarios de comunicación de estilo de emisión.

## <a name="pre-encode-and-post-decode-audio-manipulation"></a>Previamente codificar y descodificar posteriores a la manipulación de audio

El Chat de juego original permitido para la manipulación de audio, permitiendo el acceso al audio del micrófono sin procesar a través de la `OnPreEncodeAudioBuffer` y `OnPostDecodeAudioBuffers` eventos. Juego 2 Chat expone esta característica a través de un modelo de sondeo. Para obtener más información, consulte [manipulación de audio en tiempo real](real-time-audio-manipulation.md).
