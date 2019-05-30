---
title: Conectar dispositivos a través de sesiones remotas
description: Cree experiencias compartidas en varios dispositivos uniéndolos en una sesión remota.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: dispositivos Windows 10, uwp, conectados, los sistemas remotos, Roma, proyecto Roma
ms.localizationpriority: medium
ms.openlocfilehash: 4787b6c14408dc8ee35e26764caafc5b6e7fbdc9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371887"
---
# <a name="connect-devices-through-remote-sessions"></a>Conectar dispositivos a través de sesiones remotas

La característica Sesiones remotas permite que una aplicación se conecte con otros dispositivos a través de una sesión, ya sea para mensajes de la aplicación explícitos o para intercambio asincrónico de datos administrados por el sistema, como la **[SpatialEntityStore](https://docs.microsoft.com/uwp/api/windows.perception.spatial.spatialentitystore)** para uso compartido holográfico entre dispositivos Windows Holographic.

Las sesiones remotas se pueden crear por cualquier dispositivo Windows y cualquier dispositivo Windows puede solicitar unirse (aunque las sesiones pueden tener visibilidad de solo invitación), incluidos los dispositivos en los que otros usuarios han iniciado sesión. Esta guía proporciona código de ejemplo básico para todos los escenarios principales que usan sesiones remotas. Este código se puede incorporar en un proyecto de aplicación existente y modificarse según sea necesario. Para una implementación integral, consulta la [aplicación de ejemplo de juego de preguntas](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

## <a name="preliminary-setup"></a>Configuración preliminar

### <a name="add-the-remotesystem-capability"></a>Agregar la funcionalidad remoteSystem

Para que la aplicación pueda iniciar una aplicación en un dispositivo remoto, debes agregar la funcionalidad `remoteSystem` al manifiesto del paquete de la aplicación. Si quieres usar el diseñador de manifiestos del paquete para agregarla, selecciona **Sistema remoto** en la pestaña **Funcionalidades**, o bien puedes agregar manualmente la siguiente línea al archivo _Package.appxmanifest_ del proyecto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Habilitar la detección entre usuarios en el dispositivo
Sesiones remotas está orientada a la conexión de varios usuarios diferentes, por lo que los dispositivos implicados deberán tener la configuración de uso compartido entre usuarios. Se trata de una configuración del sistema que se puede consultar con un método estático en la clase **RemoteSystem**:

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Para cambiar esta configuración, el usuario debe abrir la aplicación **Configuración**. En el menú **Sistema** > **Experiencias compartidas** > **Compartir entre dispositivos**, hay un cuadro desplegable donde el usuario puede especificar con qué dispositivos puede compartir su sistema.

![página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Incluir los espacios de nombres necesarios
Para usar todos los fragmentos de código de esta guía, necesitarás las siguientes instrucciones `using` en los archivos de clase.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Crear una sesión remota

Para crear una instancia de sesión remota, debes comenzar con un objeto **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** . Usa el marco siguiente para crear una nueva sesión y controlar solicitudes de unión de otros dispositivos.

```csharp
public async void CreateSession() {
    
    // create a session controller
    RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob’s Minecraft game");
    
    // register the following code to handle the JoinRequested event
    manager.JoinRequested += async (sender, args) => {
        // Get the deferral
        var deferral = args.GetDeferral();
        
        // display the participant (args.JoinRequest.Participant) on UI, giving the 
        // user an opportunity to respond
        // ...
        
        // If the user chooses "accept", accept this remote system as a participant
        args.JoinRequest.Accept();
    };
    
    // create and start the session
    RemoteSystemSessionCreationResult createResult = await manager.CreateSessionAsync();
    
    // handle the creation result
    if (createResult.Status == RemoteSystemSessionCreationStatus.Success) {
        // creation was successful, get a reference to the session
        RemoteSystemSession currentSession = createResult.Session;
        
        // optionally subscribe to the disconnection event
        currentSession.Disconnected += async (sender, args) => {
            // update the UI, using args.Reason
            //...
        };
    
        // Use session (see later section)
        //...
    
    } else if (createResult.Status == RemoteSystemSessionCreationStatus.SessionLimitsExceeded) {
        // creation failed. Optionally update UI to indicate that there are too many sessions in progress
    } else {
        // creation failed for an unknown reason. Optionally update UI
    }
}
```

### <a name="make-a-remote-session-invite-only"></a>Crear una sesión remota solo de invitación

Si deseas que la sesión remota se detecte públicamente, puedes hacerla solo de invitación. Solo los dispositivos que reciben una invitación podrán enviar solicitudes de unión. 

El procedimiento es básicamente el mismo que el anterior, pero al construir la instancia **[RemoteSystemSessionController](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** , pasarás un objeto **[RemoteSystemSessionOptions](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** configurado.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Para enviar una invitación, debes tener una referencia para el sistema remoto receptor (adquirida a través de la detección del sistema remoto normal). Solo tienes que pasar esta referencia al método **[SendInvitationAsync](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** del objeto de la sesión. Todos los participantes de una sesión tienen una referencia a la sesión remota (consulta la sección siguiente), por lo que cualquier participante puede enviar una invitación.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Detectar y unirse a una sesión remota

El proceso de detección de sesiones remotas se controla mediante la clase **[RemoteSystemSessionWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** y es similar a detectar sistemas remotos individuales.

```csharp
public void DiscoverSessions() {
    
    // create a watcher for remote system sessions
    RemoteSystemSessionWatcher sessionWatcher = RemoteSystemSession.CreateWatcher();
    
    // register a handler for the "added" event
    sessionWatcher.Added += async (sender, args) => {
        
        // get a reference to the info about the discovered session
        RemoteSystemSessionInfo sessionInfo = args.SessionInfo;
        
        // Optionally update the UI with the sessionInfo.DisplayName and 
        // sessionInfo.ControllerDisplayName strings. 
        // Save a reference to this RemoteSystemSessionInfo to use when the
        // user selects this session from the UI
        //...
    };
    
    // Begin watching
    sessionWatcher.Start();
}
```

Cuando se obtiene una instancia **[RemoteSystemSessionInfo](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** , se puede usar para emitir una solicitud de unión al dispositivo que controla la sesión correspondiente. Una solicitud de unión aceptada devolverá asincrónicamente un objeto **[RemoteSystemSessionJoinResult](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** que contiene una referencia a la sesión unida.

```csharp
public async void JoinSession(RemoteSystemSessionInfo sessionInfo) {

    // issue a join request and wait for result.
    RemoteSystemSessionJoinResult joinResult = await sessionInfo.JoinAsync();
    if (joinResult.Status == RemoteSystemSessionJoinStatus.Success) {
        // Join request was approved

        // RemoteSystemSession instance "currentSession" was declared at class level.
        // Assign the value obtained from the join result.
        currentSession = joinResult.Session;
        
        // note connection and register to handle disconnection event
        bool isConnected = true;
        currentSession.Disconnected += async (sender, args) => {
            isConnected = false;

            // update the UI with args.Reason value
        };
        
        if (isConnected) {
            // optionally use the session here (see next section)
            //...
        }
    } else {
        // Join was unsuccessful.
        // Update the UI, using joinResult.Status value to show cause of failure.
    }
}
```

Un dispositivo se puede unir a varias sesiones al mismo tiempo. Por este motivo, puede que sea adecuado separar la funcionalidad de unión de la interacción real con cada sesión. Siempre que se mantenga una referencia a la instancia **[RemoteSystemSession](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsession)** en la aplicación, se puede intentar la comunicación a través de esa sesión.

## <a name="share-messages-and-data-through-a-remote-session"></a>Compartir mensajes y datos a través de una sesión remota

### <a name="receive-messages"></a>Recibir mensajes

Puede intercambiar mensajes y datos con otros dispositivos participantes en la sesión mediante el uso de una instancia de **[RemoteSystemSessionMessageChannel](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** , que representa un canal de comunicación único para toda la sesión. En cuanto se inicializa, empieza a escuchar mensajes entrantes.

>[!NOTE]
>Los mensajes se deben serializar y deserializar de matrices de bytes tras el envío y la recepción. Esta funcionalidad se incluye en los siguientes ejemplos, pero se puede implementar por separado para lograr una mejor modularidad de código. Consulta la [aplicación de muestra](https://github.com/microsoft/Windows-appsample-remote-system-sessions) para ver un ejemplo de esto.

```csharp
public async void StartReceivingMessages() {
    
    // Initialize. The channel name must be known by all participant devices 
    // that will communicate over it.
    RemoteSystemSessionMessageChannel messageChannel = new RemoteSystemSessionMessageChannel(currentSession, 
        "Everyone in Bob's Minecraft game", 
        RemoteSystemSessionMessageChannelReliability.Reliable);
    
    // write the handler for incoming messages on this channel
    messageChannel.ValueSetReceived += async (sender, args) => {
        
        // Update UI: a message was received from the participant args.Sender
        
        // Deserialize the message 
        // (this app must know what key to use and what object type the value is expected to be)
        ValueSet receivedMessage = args.Message;
        object rawData = receivedMessage["appKey"]);
        object value = new ExpectedType(); // this must be whatever type is expected

        using (var stream = new MemoryStream((byte[])rawData)) {
            value = new DataContractJsonSerializer(value.GetType()).ReadObject(stream);
        }
        
        // do something with the "value" object
        //...
    };
}
```

### <a name="send-messages"></a>Enviar mensajes

Cuando se establece el canal, el envío de un mensaje a todos los participantes de la sesión es sencillo.

```csharp
public async void SendMessageToAllParticipantsAsync(RemoteSystemSessionMessageChannel messageChannel, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }
    
    // Send message to all participants. Ordering is not guaranteed.
    await messageChannel.BroadcastValueSetAsync(message);
}
```

Para enviar un mensaje únicamente a determinados participantes, debe iniciar primero un proceso de detección para adquirir referencias para los sistemas remotos que participan en la sesión. Esto es similar al proceso de detectar sistemas remotos fuera de una sesión. Usa una instancia **[RemoteSystemSessionParticipantWatcher](https://docs.microsoft.com/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** para buscar los dispositivos participantes de una sesión.

```csharp
public void WatchForParticipants() {
    // "currentSession" is a reference to a RemoteSystemSession.
    RemoteSystemSessionParticipantWatcher watcher = currentSession.CreateParticipantWatcher();

    watcher.Added += (sender, participant) => {
        // save a reference to "participant"
        // optionally update UI
    };   

    watcher.Removed += (sender, participant) => {
        // remove reference to "participant"
        // optionally update UI
    };

    watcher.EnumerationCompleted += (sender, args) => {
        // Apps can delay data model render up until this point if they wish.
    };

    // Begin watching for session participants
    watcher.Start();
}
```

Cuando se obtiene una lista de referencias a los participantes de una sesión, puedes enviar un mensaje a un conjunto de ellos.

Para enviar un mensaje a un único participante (idealmente seleccionado en la pantalla por el usuario), solo tienes que pasar la referencia a un método similar al siguiente.

```csharp
public async void SendMessageToParticipantAsync(RemoteSystemSessionMessageChannel messageChannel, RemoteSystemSessionParticipant participant, object value) {
    
    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to the participant
    await messageChannel.SendValueSetAsync(message,participant);
}
```

Para enviar un mensaje a varios participantes (idealmente seleccionado en la pantalla por el usuario), agrégalos a un objeto de lista y pasa la lista en un método similar al siguiente.

```csharp
public async void SendMessageToListAsync(RemoteSystemSessionMessageChannel messageChannel, IReadOnlyList<RemoteSystemSessionParticipant> myTeam, object value){

    // define a ValueSet message to send
    ValueSet message = new ValueSet();
    
    // serialize the "value" object to send
    using (var stream = new MemoryStream()){
        new DataContractJsonSerializer(value.GetType()).WriteObject(stream, value);
        byte[] rawData = stream.ToArray();
            message["appKey"] = rawData;
    }

    // Send message to specific participants. Ordering is not guaranteed.
    await messageChannel.SendValueSetToParticipantsAsync(message, myTeam);   
}
```

## <a name="related-topics"></a>Temas relacionados
* [Las aplicaciones conectadas y los dispositivos (proyecto Roma)](connected-apps-and-devices.md)
* [Referencia de API de sistemas remoto](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)
