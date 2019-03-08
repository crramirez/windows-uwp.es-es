---
title: El uso de proyecciones de Chat juego WinRT 2
description: Aprenda a usar Xbox Live Game Chat 2 con proyecciones de WinRT para agregar la comunicación por voz a su juego.
ms.date: 04/11/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, chat juego 2, chat de juegos, comunicación por voz
ms.localizationpriority: medium
ms.openlocfilehash: 1cb0578151d4262d61f5fbc078bebab721fb3bfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639170"
---
# <a name="using-game-chat-2-winrt-projections"></a>Uso de juegos Chat 2 (proyecciones de WinRT)

Se trata de un breve tutorial sobre el uso de juego Chat 2 C# API. Deberían ver los desarrolladores de juegos que desean tener acceso a 2 de Chat de juego mediante C++ [con juego de charla 2](using-game-chat-2.md).

## <a name="prerequisites-a-nameprereq"></a>Requisitos previos <a name="prereq">

Para consumir 2 de Chat de juego, se debe incluir el [paquete de nuget Microsoft.Xbox.Services.GameChat2](https://www.nuget.org/packages/Microsoft.Xbox.Game.Chat.2.WinRT.UWP/).

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

## <a name="initialization-a-nameinit"></a>Inicialización <a name="init">

Empezar a interactuar con la biblioteca de creación de instancias de un `GameChat2ChatManager` espera agregarse a la instancia de objeto con el número máximo de usuarios simultáneos de chat. Para cambiar este valor, el `GameChat2ChatManager` objeto debe ser eliminado y vuelto a crear con el valor deseado. Solo puede tener un `GameChat2ChatManager` crea una instancia a la vez.

```cs
GameChat2ChatManager myGameChat2ChatManager = new GameChat2ChatManager(<maxChatUserCount>);
```

La `GameChat2ChatManager` objeto tiene propiedades adicionales y opcionales que se pueden configurar en cualquier momento. Ejemplo de código siguiente se da por supuesto que las variables le asignará un valor antes de que se usan para establecer las propiedades en el `myGameChat2ChatManager` objeto.

```cs
GameChat2SpeechToTextConversionMode mySpeechToTextConversionMode;
GameChat2SharedDeviceCommunicationRelationshipResolutionMode mySharedDeviceResolutionMode;
GameChat2CommunicationRelationship myDefaultLocalToRemoteCommunicationRelationship;
float myDefaultAudioRenderVolume;
GameChat2AudioEncodingTypeAndBitrate myAudioEncodingTypeAndBitRate;

...

myGameChat2ChatManager.SpeechToTextConversionMode = mySpeechToTextConversionMode;
myGameChat2ChatManager.SharedDeviceResolutionMode = mySharedDeviceResolutionMode;
myGameChat2ChatManager.DefaultLocalToRemoteCommunicationRelationship = myDefaultLocalToRemoteCommunicationRelationship;
myGameChat2ChatManager.DefaultAudioRenderVolume = myDefaultAudioRenderVolume;
myGameChat2ChatManager.AudioEncodingTypeAndBitRate = myAudioEncodingTypeAndBitRate;
```

## <a name="configuring-users-a-nameconfig"></a>Configuración de usuarios <a name="config">

Una vez que se inicializa la instancia, debe agregar los usuarios locales a la instancia de GC2. En este ejemplo, el usuario A representará un usuario local.

```cs
string userAXuid;

...

IGameChat2ChatUser userA = myGameChat2ChatManager.AddLocalUser(userAXuid);
```

A continuación, debe agregar los usuarios remotos y los identificadores que se usará para representar los extremos"remotos" que se encuentra en cada usuario remoto. "Puntos de conexión" se representan mediante objetos que pertenecen a la aplicación que implementan la `IGameChat2Endpoint` interfaz. El ejemplo siguiente muestra una implementación de ejemplo `MyEndpoint` clase que implementa el `IGameChat2Endpoint`.

```cs
class MyEndpoint : IGameChat2Endpoint
{
    private uint endpointIdentifier;
    public MyEndpoint(uint identifier)
    {
        endpointIdentifier = identifier;
    }

    // Implementing IsSameEndpointAs, the only method on the IGameChat2Endpoint interface
    public bool IsSameEndpointAs(IGameChat2Endpoint gameChat2Endpoint)
    {
        return endpointIdentifier == ((MyEndpoint)gameChat2Endpoint).endpointIdentifier;
    }
}
```

En el ejemplo de código siguiente, los usuarios B, C y D son los usuarios remotos que se va a agregar. El usuario B está en un dispositivo remoto y los usuarios de C y D en otro dispositivo remoto. En este ejemplo se da por supuesto que se establecerán las variables y que `myGameChat2ChatManager` es una instancia de `GameChat2ChatManager` y que "MyEndpoint" es una clase que implementa `IGameChat2Endpoint`.

```cs
string userBXuid;
string userCXuid;
string userDXuid;
MyEndpoint myRemoteEndpointOne;
MyEndpoint myRemoteEndpointTwo;

...

IGameChat2ChatUser remoteUserB = myGameChat2ChatManager.AddRemoteUser(userBXuid, myRemoteEndpointOne);
IGameChat2ChatUser remoteUserC = myGameChat2ChatManager.AddRemoteUser(userCXuid, myRemoteEndpointTwo);
IGameChat2ChatUser remoteUserD = myGameChat2ChatManager.AddRemoteUser(userDXuid, myRemoteEndpointTwo);
```

Ahora configura la relación de comunicación entre cada usuario remoto y el usuario local. En este ejemplo, suponga que el usuario A y usuario B están en el mismo equipo y se permite la comunicación bidireccional. `GameChat2CommunicationRelationship.SendAndReceiveAll` se define para representar la comunicación bidireccional. Establecer la relación del usuario al usuario B con:

```cs
GameChat2ChatUserLocal localUserA = userA as GameChat2ChatUserLocal;
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

Ahora suponga que los usuarios de C y D son 'espectadores' y se deben permitir a escuchar al usuario A, pero no habla. `GameChat2CommunicationRelationship.ReceiveAll` es un definido este tipo de comunicación unidireccional. Establecer las relaciones con:

```cs
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.ReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.ReceiveAll);
```

Si en cualquier momento hay usuarios remotos que se han agregado a la `GameChat2ChatManager` instancia pero aún no ha configurado para comunicarse con los usuarios - que bien! Esto puede darse en escenarios donde los usuarios determinan los equipos o cambiar de canal habla arbitrariamente. Juego 2 Chat solo almacenará en caché información (por ejemplo, las relaciones de privacidad y reputación) para los usuarios que se han agregado a la instancia, por lo que es útil informar al 2 de Chat de juego de todos los posibles usuarios incluso si no se hablan a los usuarios locales en un momento determinado en el tiempo.

Por último, suponga que usuario D ha abandonado el juego y debe quitarse de la instancia local de juego 2 Chat. Que se puede hacer con la siguiente llamada:

```cs
remoteUserD.Remove();
```

El objeto de usuario se invalida inmediatamente cuando `Remove()` se llama. El último estado del usuario se almacena en caché cuando se elimina. Ningún método informativo que se llama después de que se invalida el objeto de usuario reflejará el estado del usuario antes de se ha quitado. Otros métodos producirán un error cuando se llama.

## <a name="processing-data-frames-a-namedata"></a>Procesamiento de tramas de datos <a name="data">

2 de Chat de juego no tiene su propia capa de transporte; Esto se debe proporcionar la aplicación. Este complemento se administra mediante llamadas normales, con frecuencia de la aplicación a la `GameChat2ChatManager.GetDataFrames()`. Este método es la forma juego Chat 2 proporciona los datos salientes a la aplicación. Está diseñado para funcionar rápidamente que puede ser sondea con frecuencia en un subproceso dedicado de red. Esto proporciona un lugar conveniente para recuperar todos los datos en cola sin preocuparse por la imprecisión del tiempo de la red o la complejidad de devolución de llamada multiproceso.

Cuando `GameChat2ChatManager.GetDataFrames()` se llama, todos los datos en cola se notifican en una lista de `IGameChat2DataFrame` objetos. Las aplicaciones deben recorrer en iteración la lista, inspeccionar la `TargetEndpointIdentifiers`y usar capa de red de la aplicación para entregar los datos a las instancias de aplicación remota adecuada. En este ejemplo, `HandleOutgoingDataFrame` es una función que envía los datos en el `Buffer` a cada usuario en cada extremo de"" especificado en el `TargetEndpointIdentifiers` según el `TransportRequirement`.

```cs
IReadOnlyList<IGameChat2DataFrame> frames = myGameChat2ChatManager.GetDataFrames();
foreach (IGameChat2DataFrame dataFrame in frames)
{
    HandleOutgoingDataFrame(
        dataFrame.Buffer,
        dataFrame.TargetEndpointIdentifiers,
        dataFrame.TransportRequirement
        );
}
```

Con más frecuencia se procesan las tramas de datos, menor será la latencia de audio evidente para el usuario final. El audio se combinan en tramas de datos de 40 ms; Este es el período de sondeo sugeridos.

## <a name="processing-state-changes-a-namestate"></a>Cambios de estado de procesamiento <a name="state">

Juego 2 Chat proporciona actualizaciones para la aplicación, como mensajes de texto recibidos a través de llamadas de la aplicación normal, con frecuencia a la `GameChat2ChatManager.GetStateChanges()` método. Se ha diseñado para funcionar rápidamente, que puede llamarse cada fotograma de gráficos en el bucle de representación de la interfaz de usuario. Esto proporciona un lugar conveniente para recuperar todos los cambios de la cola sin preocuparse por la imprecisión del tiempo de la red o la complejidad de devolución de llamada multiproceso.

Cuando `GameChat2ChatManager.GetStateChanges()` es llamado, se notifican en una lista de todas las actualizaciones en cola `IGameChat2StateChange` objetos. Las aplicaciones deben:
1. Recorrer en iteración la lista
2. Inspeccionar la estructura de base de su tipo más específico
3. Convierte la estructura de base correspondiente más detallada de tipo
4. Controlar esa actualización según corresponda. 

```cs
IReadOnlyList<IGameChat2StateChange> stateChanges = myGameChat2ChatManager.GetStateChanges();
foreach (IGameChat2StateChange stateChange in stateChanges)
{
    switch (stateChange.Type)
    {
        case GameChat2StateChangeType.CommunicationRelationshipAdjusterChanged:
        {
            MyHandleCommunicationRelationshipAdjusterChanged(stateChange as GameChat2CommunicationRelationshipAdjusterChangedStateChange);
            break;
        }
        case GameChat2StateChangeType.TextChatReceived:
        {
            MyHandleTextChatReceived(stateChange as GameChat2TextChatReceivedStateChange);
            break;
        }

        ...
    }
}
```

## <a name="text-chat-a-nametext"></a>Chat de texto <a name="text">

Para enviar el chat de texto, use `GameChat2ChatUserLocal.SendChatText()`. Por ejemplo:

```cs
localUserA.SendChatText("Hello");
```

2 de Chat de juego, se generará una trama de datos que contiene este mensaje; los puntos de conexión de destino de la trama de datos serán las asociadas a los usuarios que se han configurado para recibir el texto del usuario local. Cuando los datos se procesan mediante los puntos de conexión remotas, el mensaje se expondrá a través de un `GameChat2TextChatReceivedStateChange`. Al igual que con chat de voz, se respetan las restricciones de privacidad y con privilegios de chat de texto. Si un par de usuarios se han configurado para permitir el chat de texto, pero las restricciones de privilegios o la privacidad no permitir que la comunicación, se quitará el mensaje de texto.

Compatibilidad con entrada de chat de texto y la presentación es necesario para mejorar la accesibilidad (consulte [accesibilidad](#access) para obtener más detalles).

## <a name="accessibility-a-nameaccess"></a>Accesibilidad <a name="access">

Se requiere compatibilidad con chat de texto de entrada y la presentación. Entrada de texto es necesario porque, incluso en plataformas o géneros de juegos que históricamente no tuvieron la generalizada teclado físico usar, los usuarios pueden configurar el sistema para usar las tecnologías de asistencia texto a voz. De forma similar, la presentación del texto es necesaria porque los usuarios pueden configurar el sistema para usar texto a voz. Estas preferencias se pueden detectar en usuarios locales mediante la comprobación de la `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` y `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` propiedades respectivamente, y puede que desee habilitar condicionalmente los mecanismos de texto.

### <a name="text-to-speech"></a>Texto a voz

Cuando un usuario tiene texto a voz habilitada, `GameChat2ChatUserLocal.TextToSpeechConversionPreferenceEnabled` será 'true'. Cuando se detecta este estado, la aplicación debe proporcionar un método de entrada de texto. Una vez que la entrada de texto que se proporcionó mediante un teclado real o virtual, pase la cadena para el `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` método. Juego 2 Chat detectará y síntesis de datos de audio en función de la cadena y la preferencia del usuario accesible de voz. Por ejemplo:

```cs
localUserA.SynthesizeTextToSpeech("Hello");
```

El audio sintetizado como parte de esta operación se transportarán a todos los usuarios que se han configurado para recibir audio de este usuario local. Si `GameChat2ChatUserLocal.SynthesizeTextToSpeech()` se llama en un usuario que no tengan texto a voz habilitada 2 de Chat de juego no realiza ninguna acción.

### <a name="speech-to-text"></a>Voz a texto

Cuando un usuario tiene habilitada, de voz a texto `GameChat2ChatUserLocal.SpeechToTextConversionPreferenceEnabled` será true. Cuando se detecta este estado, la aplicación debe estar preparada para proporcionar la que interfaz de usuario asociadas con mensajes de chat de transcripción. GC automáticamente transcribir audio de cada usuario remoto y exponerlo a través de un `GameChat2TranscribedChatReceivedStateChange`.

### <a name="speech-to-text-performance-considerations"></a>Consideraciones de rendimiento de voz a texto

Cuando está habilitada voz a texto, la instancia 2 de Chat de juego en cada dispositivo remoto inicia una conexión de socket web con el punto de conexión de servicios de voz. Cada cliente remoto de juego Chat 2 carga audio en el punto de conexión de servicios de voz a través de dicho websocket; en ocasiones, el punto de conexión de servicios de voz devuelve un mensaje de transcripción al dispositivo remoto. El dispositivo remoto, a continuación, envía el mensaje de transcripción (es decir, un mensaje de texto) en el dispositivo local, donde se asigna al mensaje transcrito juego Chat de 2 a la aplicación para representar.

Por lo tanto, el costo de rendimiento principal de voz a texto es el uso de la red. La mayoría del tráfico de red es la carga de audio codificado. El socket Web carga audio que ya se ha codificado por 2 de Chat de juego en la ruta de acceso de chat de voz "normal"; la aplicación tiene control sobre la velocidad de bits a través de `GameChat2ChatManager.AudioEncodingTypeAndBitrate`.

## <a name="ui-a-nameui"></a>INTERFAZ DE USUARIO <a name="UI">

Se recomienda que en cualquier parte los jugadores se muestran, especialmente en una lista de nombres de jugadores, como un panel de resultados, también mostrar iconos de Silenciar de habla como comentarios del usuario. El `IGameChat2ChatUser.ChatIndicator` propiedad representa el estado actual, la instantáneo de chat para que el Reproductor. El ejemplo siguiente muestra el valor de indicador de un `IGameChat2ChatUser` objeto apunta a la variable 'userA' para determinar un valor constante de icono determinado para asignar a una variable 'iconToShow':

```cs
switch (userA.ChatIndicator)
{
    case GameChat2UserChatIndicator.Silent:
    {
        iconToShow = Icon_InactiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.Talking:
    {
        iconToShow = Icon_ActiveSpeaker;
        break;
    }
    case GameChat2UserChatIndicator.LocalMicrophoneMuted:
    {
        iconToShow = Icon_MutedSpeaker;
        break;
    }
    
    ...
}
```

El valor de `IGameChat2ChatUser.ChatIndicator` es previsible que cambie con frecuencia como reproductores start y stop hablar, por ejemplo. Está diseñado para admitir aplicaciones de sondeo cada fotograma de la interfaz de usuario como resultado.

## <a name="muting-a-namemute"></a>Silenciar <a name="mute">

El `GameChat2ChatUserLocal.MicrophoneMuted` propiedad puede usarse para alternar el estado del audio del micrófono de un usuario local. Cuando el micrófono está silenciado, audio desde ese micrófono no se capturará. Si el usuario está en un dispositivo compartido, como la Kinect, el estado del audio se aplica a todos los usuarios. Este método no refleja un control de silencio de hardware (por ejemplo, un botón de auriculares del usuario). No hay ningún método para recuperar el estado del audio de hardware de dispositivo de audio de un usuario a través del 2 de Chat de juego.

El `GameChat2ChatUserLocal.SetRemoteUserMuted()` método puede utilizarse para alternar el estado del audio de un usuario remoto con respecto a un determinado usuario local. Cuando el usuario remoto está desactivado, el usuario local no oír el audio o recibir cualquier mensaje de texto del usuario remoto.

## <a name="bad-reputation-auto-mute-a-nameautomute"></a>Mala fama de silencio en automático <a name="automute">

Normalmente, los usuarios remotos se iniciará reactivó el micrófono. 2 de Chat de juego se iniciará a los usuarios en un estado desactivado cuando (1) el usuario remoto no amigos con el usuario local, y (2) el usuario remoto tiene una marca de mala reputación. Cuando los usuarios están en silencio debido a esta operación, `IGameChat2ChatUser.ChatIndicator` devolverá `GameChat2UserChatIndicator.ReputationRestricted`. Este estado se reemplazará por la primera llamada a `GameChat2ChatUserLocal.SetRemoteUserMuted()` que incluye el usuario remoto como el usuario de destino.

## <a name="privilege-and-privacy-a-namepriv"></a>Privacidad y con privilegios <a name="priv">

Encima de la relación de comunicación configurada por el juego, juego 2 Chat aplica las restricciones de privacidad y con privilegios. Juego 2 Chat realiza búsquedas de restricción de privilegios y privacidad cuando se agrega un usuario por primera vez; el usuario `IGameChat2ChatUser.ChatIndicator` siempre devolverá `GameChat2UserChatIndicator.Silent` hasta que se completen las operaciones. Si la comunicación con un usuario se ve afectada por un privilegio o la privacidad restricción, el usuario `IGameChat2ChatUser.ChatIndicator` devolverá `GameChat2UserChatIndicator.PlatformRestricted`. Aplican restricciones de comunicación de la plataforma a la charla de voz y texto; nunca habrá una instancia de chat de texto está bloqueado por una restricción de la plataforma donde pero chat de voz es no, o viceversa.

`GameChat2ChatUserLocal.GetEffectiveCommunicationRelationship()` puede usarse para ayudar a distinguir cuando los usuarios no pueden comunicarse debido a privilegios incompleta y las operaciones de privacidad. Devuelve la relación de comunicación que se aplica en forma de juego Chat 2 `GameChat2CommunicationRelationship` y la razón por la relación no puede ser igual a la relación configurada en forma de `GameChat2CommunicationRelationshipAdjuster`. Por ejemplo, si las operaciones de búsqueda están aún en curso, el `GameChat2CommunicationRelationshipAdjuster` será `GameChat2CommunicationRelationshipAdjuster.Initializing`. Se espera que este método para usarse en escenarios de desarrollo y depuración; no debe usarse para influir en la interfaz de usuario (consulte [UI](#UI)).

## <a name="cleanup-a-namecleanup"></a>Limpieza <a name="cleanup">

Cuando la aplicación ya no necesita las comunicaciones a través de juego Chat 2, debe llamar a `GameChat2ChatManager.Dispose()`. Esto permite juego Chat 2 reclamar recursos asignados para administrar las comunicaciones.

## <a name="how-to-configure-popular-scenarios"></a>Cómo configurar escenarios populares

### <a name="push-to-talk"></a>Hablar de inserción

Hablar de inserción debe implementarse con `GameChat2ChatUserLocal.MicrophoneMuted`. Establecer `MicrophoneMuted` en false para permitir voz y `MicrophoneMuted` es verdadera para restringirla. Este cambio de propiedad proporcionará la respuesta de latencia más baja del 2 de Chat de juego.

### <a name="teams"></a>Equipos

Suponga que el usuario A y usuario B están en azul de equipo y usuario C y d. de usuario se encuentran en el equipo rojo. Cada usuario está en una instancia única de la aplicación.

En el dispositivo del usuario:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

En el dispositivo del usuario B:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.SendAndReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

En el dispositivo del usuario C:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

En el dispositivo del usuario D:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAndReceiveAll);
```

### <a name="broadcast"></a>Transmisión

Suponga que el usuario A es el líder de pedidos y solo pueden escuchar a los usuarios B, C y D. Cada player se encuentra en un único dispositivo.

En el dispositivo del usuario:

```cs
localUserA.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.SendAll);
localUserA.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.SendAll);
```

En el dispositivo del usuario B:

```cs
localUserB.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserB.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
localUserB.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

En el dispositivo del usuario C:

```cs
localUserC.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserC.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserC.SetCommunicationRelationship(remoteUserD, GameChat2CommunicationRelationship.None);
```

En el dispositivo del usuario D:

```cs
localUserD.SetCommunicationRelationship(remoteUserA, GameChat2CommunicationRelationship.ReceiveAll);
localUserD.SetCommunicationRelationship(remoteUserB, GameChat2CommunicationRelationship.None);
localUserD.SetCommunicationRelationship(remoteUserC, GameChat2CommunicationRelationship.None);
```
