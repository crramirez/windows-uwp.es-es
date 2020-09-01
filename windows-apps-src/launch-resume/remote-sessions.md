---
title: Conectar dispositivos mediante sesiones remotas
description: Crea experiencias compartidas entre varios dispositivos uniéndolos en una sesión remota.
ms.assetid: 1c8dba9f-c933-4e85-829e-13ad784dd3e2
ms.date: 06/28/2017
ms.topic: article
keywords: Windows 10, UWP, dispositivos conectados, sistemas remotos, Roma, proyecto Roma
ms.localizationpriority: medium
ms.openlocfilehash: f88f44d26c3a6f4971422074e855ffca53935c7f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164829"
---
# <a name="connect-devices-through-remote-sessions"></a>Conectar dispositivos mediante sesiones remotas

La característica de sesiones remotas permite a una aplicación conectarse a otros dispositivos a través de una sesión, ya sea para la mensajería de aplicación explícita o para el intercambio asincrónico de datos administrados por el sistema, como **[SpatialEntityStore](/uwp/api/windows.perception.spatial.spatialentitystore)** para el uso compartido de Holographic entre dispositivos Windows Holographic.

Las sesiones remotas se pueden crear en cualquier dispositivo de Windows y cualquier dispositivo de Windows puede solicitar unirse (aunque las sesiones pueden tener visibilidad solo de invitación), incluidos los dispositivos que han iniciado sesión otros usuarios. En esta guía se proporciona código de ejemplo básico para todos los escenarios principales que hacen uso de sesiones remotas. Este código se puede incorporar a un proyecto de aplicación existente y modificar según sea necesario. Para una implementación de un extremo a otro, consulte la [aplicación de ejemplo juego de quiz](https://github.com/microsoft/Windows-appsample-remote-system-sessions)).

## <a name="preliminary-setup"></a>Configuración preliminar

### <a name="add-the-remotesystem-capability"></a>Agregar la funcionalidad remoteSystem

Para que la aplicación pueda iniciar una aplicación en un dispositivo remoto, debes agregar la funcionalidad `remoteSystem` al manifiesto del paquete de la aplicación. Puede usar el diseñador de manifiestos de paquete para agregarlo seleccionando **sistema remoto** en la pestaña **capacidades** , o puede Agregar manualmente la siguiente línea al archivo _Package. appxmanifest_ del proyecto.

``` xml
<Capabilities>
   <uap3:Capability Name="remoteSystem"/>
</Capabilities>
```

### <a name="enable-cross-user-discovering-on-the-device"></a>Habilitar la detección de usuarios cruzados en el dispositivo
Las sesiones remotas están orientadas a la conexión de varios usuarios diferentes, por lo que los dispositivos implicados deberán tener habilitado el uso compartido entre usuarios. Se trata de una configuración del sistema que se puede consultar con un método estático en la clase **RemoteSystem** :

```csharp
if (!RemoteSystem.IsAuthorizationKindEnabled(RemoteSystemAuthorizationKind.Anonymous)) {
    // The system is not authorized to connect to cross-user devices. 
    // Inform the user that they can discover more devices if they
    // update the setting to "Everyone nearby".
}
```

Para cambiar esta configuración, el usuario debe abrir la aplicación de **configuración** . En el **System**  >  menú recursos**compartidos de experiencias compartidas**en  >  **varios dispositivos** , hay un cuadro desplegable en el que el usuario puede especificar los dispositivos con los que el sistema puede compartir.

![Página de configuración de experiencias compartidas](images/shared-experiences-settings.png)

### <a name="include-the-necessary-namespaces"></a>Incluir los espacios de nombres necesarios
Para usar todos los fragmentos de código de esta guía, necesitará las siguientes `using` instrucciones en los archivos de clase.

```csharp
using System.Runtime.Serialization.Json;
using Windows.Foundation.Collections;
using Windows.System.RemoteSystems;
```

## <a name="create-a-remote-session"></a>Crear una sesión remota

Para crear una instancia de sesión remota, debe empezar con un objeto **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** . Use el siguiente marco de trabajo para crear una nueva sesión y controlar las solicitudes de combinación desde otros dispositivos.

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

### <a name="make-a-remote-session-invite-only"></a>Hacer una sesión remota de solo invitación

Si quiere evitar que la sesión remota se pueda detectar de forma pública, puede hacer que sea de solo invitación. Solo los dispositivos que reciben una invitación podrán enviar solicitudes de combinación. 

El procedimiento es básicamente el mismo que el anterior, pero al construir la instancia de **[RemoteSystemSessionController](/uwp/api/windows.system.remotesystems.remotesystemsessioncontroller)** , pasará un objeto **[RemoteSystemSessionOptions](/uwp/api/windows.system.remotesystems.RemoteSystemSessionOptions)** configurado.

```csharp
// define the session options with the invite-only designation
RemoteSystemSessionOptions sessionOptions = new RemoteSystemSessionOptions();
sessionOptions.IsInviteOnly = true;

// create the session controller
RemoteSystemSessionController manager = new RemoteSystemSessionController("Bob's Minecraft game", sessionOptions);

//...
```

Para enviar una invitación, debe tener una referencia al sistema remoto receptor (adquirido a través de la detección de sistemas remotos normal). Simplemente pase esta referencia en el método **[SendInvitationAsync](/uwp/api/windows.system.remotesystems.remotesystemsession.sendinvitationasync)** del objeto de sesión. Todos los participantes de una sesión tienen una referencia a la sesión remota (consulte la siguiente sección), por lo que cualquier participante puede enviar una invitación.

```csharp
// "currentSession" is a reference to a RemoteSystemSession.
// "guestSystem" is a previously discovered RemoteSystem instance
currentSession.SendInvitationAsync(guestSystem); 
```

## <a name="discover-and-join-a-remote-session"></a>Detectar y unirse a una sesión remota

El proceso de detección de sesiones remotas se controla mediante la clase **[RemoteSystemSessionWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionwatcher)** y es similar a la detección de sistemas remotos individuales.

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

Cuando se obtiene una instancia de **[RemoteSystemSessionInfo](/uwp/api/windows.system.remotesystems.remotesystemsessioninfo)** , se puede usar para emitir una solicitud de combinación al dispositivo que controla la sesión correspondiente. Una solicitud de combinación aceptada devolverá de forma asincrónica un objeto **[RemoteSystemSessionJoinResult](/uwp/api/windows.system.remotesystems.remotesystemsessionjoinresult)** que contenga una referencia a la sesión combinada.

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

Un dispositivo puede unirse a varias sesiones al mismo tiempo. Por esta razón, puede ser conveniente separar la funcionalidad de combinación de la interacción real con cada sesión. Siempre que se mantenga una referencia a la instancia de **[RemoteSystemSession](/uwp/api/windows.system.remotesystems.remotesystemsession)** en la aplicación, se puede intentar la comunicación en esa sesión.

## <a name="share-messages-and-data-through-a-remote-session"></a>Compartir mensajes y datos a través de una sesión remota

### <a name="receive-messages"></a>Recepción de mensajes

Puede intercambiar mensajes y datos con otros dispositivos participantes en la sesión mediante una instancia de **[RemoteSystemSessionMessageChannel](/uwp/api/windows.system.remotesystems.remotesystemsessionmessagechannel)** , que representa un canal de comunicación de una sola sesión. En cuanto se inicializa, empieza a escuchar los mensajes entrantes.

>[!NOTE]
>Los mensajes se deben serializar y deserializar desde las matrices de bytes tras el envío y la recepción. Esta funcionalidad se incluye en los ejemplos siguientes, pero se puede implementar por separado para mejorar la modularidad del código. Consulte la [aplicación de ejemplo](https://github.com/microsoft/Windows-appsample-remote-system-sessions)) para obtener un ejemplo de esto.

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

### <a name="send-messages"></a>Envío de mensajes

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

Para enviar un mensaje solo a determinados participantes, primero debe iniciar un proceso de detección para adquirir las referencias a los sistemas remotos que participan en la sesión. Esto es similar al proceso de detección de sistemas remotos fuera de una sesión. Use una instancia de **[RemoteSystemSessionParticipantWatcher](/uwp/api/windows.system.remotesystems.remotesystemsessionparticipantwatcher)** para buscar los dispositivos de participante de una sesión.

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

Cuando se obtiene una lista de referencias a los participantes de la sesión, puede enviar un mensaje a cualquier conjunto de ellos.

Para enviar un mensaje a un solo participante (lo ideal es que el usuario lo seleccione en pantalla), simplemente pase la referencia a un método como el siguiente.

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

Para enviar un mensaje a varios participantes (idealmente, seleccionado en pantalla por el usuario), agréguelos a un objeto de lista y pase la lista a un método similar al siguiente.

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
* [Aplicaciones y dispositivos conectados (Proyecto Roma)](connected-apps-and-devices.md)
* [Referencia de API de sistemas remotos](/uwp/api/Windows.System.RemoteSystems)