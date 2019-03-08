---
title: Procedimientos para varios jugadores
description: Describe cómo se implementan tareas comunes en 2015 de varios jugadores de Xbox Live.
ms.assetid: 99c5b7c4-018c-4f7a-b2c9-0deed0e34097
ms.date: 08/29/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, 2015 multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 20bf3486491e8173ce946a3a5dc2e4bc83305c83
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648640"
---
# <a name="multiplayer-how-tos"></a>Procedimientos multijugador

Este tema contiene información sobre cómo implementar tareas específicas relacionadas con el uso de varios jugadores 2015.

* Suscríbase para recibir notificaciones de cambio de sesión MPSD
* Crear una sesión MPSD
* Establecer a un árbitro para una sesión MPSD
* Administrar la activación de título
* Hacer que el usuario puede unir
* Enviar invitaciones de juego
* Unirse a una sesión desde una sesión introductoria
* Unirse a una sesión MPSD de activación de un título
* Establece la actividad del usuario actual
* Actualizar una sesión MPSD
* Deje una sesión MPSD
* Rellenar las ranuras de la sesión abierta durante contactos
* Cree una incidencia de coincidencia
* Obtener el estado de vale de coincidencia

## <a name="subscribe-for-mpsd-session-change-notifications"></a>Suscríbase para recibir notificaciones de cambio de sesión MPSD

| Nota                                                                                                                                                                                                                                                                                                                                    |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Suscribirse a los cambios realizados en una sesión de requiere que el Reproductor asociado que estén activas en la sesión. El campo connectionRequiredForActiveMembers también debe establecerse en true en el objeto /constants/system/capabilities para la sesión. Normalmente, este campo se establece en la plantilla de sesión. Consulte [MPSD sesión plantillas](multiplayer-session-directory.md). |



Para recibir notificaciones de cambio de sesión MPSD, el título puede seguir el procedimiento siguiente.

1.  Usar el mismo **XboxLiveContext clase** objeto para todas las llamadas al mismo usuario. Las suscripciones están asociadas a la duración de este objeto. Si hay varios usuarios locales, use otro **XboxLiveContext** objeto para cada usuario.
2.  Implementar controladores de eventos para el **RealTimeActivityService.MultiplayerSessionChanged eventos** y **RealTimeActivityService.MultiplayerSubscriptionsLost eventos**.
3.  Si se suscribe a los cambios de más de un usuario, agregue código a su **MultiplayerSessionChanged** controlador de eventos para evitar el trabajo innecesario. Use la **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad** y **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad**. Uso de estas propiedades permite el seguimiento del último cambio visto y se omite de los cambios anteriores.
4.  Llame a la **MultiplayerService.EnableMultiplayerSubscriptions método** para permitir que las suscripciones.
5.  Crear un objeto de sesión local y la combinación esa sesión como activa.
6.  Realizar llamadas para cada usuario para el **MultiplayerSession.SetSessionChangeSubscription método**, pasando la sesión de cambiar el tipo que se va a recibir una notificación.
7.  Escribir ahora la sesión en MPSD como se describe en **Cómo: Actualizar una sesión de varios jugadores**.

El siguiente diagrama de flujo ilustra cómo iniciar Multplayer suscribiéndose a los eventos descritos en este procedimiento.

![](../../images/multiplayer/Multiplayer_2015_Start_Multiplayer.png)


### <a name="parsing-duplicate-session-change-notifications"></a>Análisis de las notificaciones de cambio de sesión duplicado

Cuando hay varios usuarios suscritos a las notificaciones para la misma sesión, cada cambio en esa sesión desencadenará una derivación de hombro para cada usuario. Todos excepto uno de ellos será duplicados. Mientras se sigue recomendando que un título de suscribirse a todos los usuarios en una sesión para las notificaciones, un título debe omitir los cambios que ya se ha recibido una notificación de; Puede hacerlo mediante las propiedades de la rama y ChangeNumber.

Para detectar varios derivaciones hombro, un título debe:

-   Para cada **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad** valor visto, el almacén de la última **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad**.
-   Si tiene un valor superior una derivación de hombros **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad** que el último visto para que **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad** , procesarlo y actualizar la versión más reciente **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad**.
-   Si no tiene una mayor una derivación de hombros **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad** para que **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad**, omitirla. Ya se ha controlado ese cambio.

| Nota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Propiedad RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber** valores se deben realizar un seguimiento de **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad**, no por sesión. Es posible que el **RealTimeActivityMultiplayerSessionChangeEventArgs.Branch propiedad** cambiar valor (y el **RealTimeActivityMultiplayerSessionChangeEventArgs.ChangeNumber propiedad**restablecer) dentro de la duración de una sesión. |

## <a name="create-an-mpsd-session"></a>Crear una sesión MPSD


| Nota                                                                                                                                                                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| De forma predeterminada, se crea una sesión MPSD cuando el primer miembro une a él. Si la lógica de título espera el título a existe o no existe en el momento de combinación, puede pasar un valor del modo de escritura adecuados para el método de escritura durante la actualización de la sesión. |



El título debe hacer lo siguiente para crear una nueva sesión:

1.  Cree un nuevo **XboxLiveContext clase** objeto. El título de la crea este objeto una vez, lo almacena y lo reutiliza según sea necesario en todo el código fuente. Especialmente cuando se trabaja con las suscripciones de la sesión, es necesario usar exactamente el mismo contexto.
2.  Cree un nuevo **MultiplayerSession clase** objeto para preparar todos los datos de sesión que el MPSD necesita para crear una nueva sesión.
3.  Realice los cambios necesarios antes de escribir la sesión en MPSD. Por ejemplo, al unirse a un miembro a la sesión con una llamada a **MultiplayerSession.Join método**, el cliente agrega los datos de solicitud local oculto que indica a MPSD para unir tras la llamada para actualizar la sesión.
4.  Cuando termine de realizar los cambios locales, escribirlos en MPSD como se describe en **Cómo: Actualizar una sesión de varios jugadores**.
5.  Recibir el nuevo **MultiplayerSession** objeto desde MPSD, con muchos campos cumplimentados.
6.  Use el nuevo objeto de sesión a partir de ahora y descartar la copia antigua, que contiene una solicitud oculta para crear una nueva sesión.

### <a name="example"></a>Ejemplo

    void Example_MultiplayerService_CreateSession()
    {
      XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

      // Values found in Xbox Developer Portal(XDP) or Partner Center configuration
      MultiplayerSessionReference^ multiplayerSessionReference = ref new MultiplayerSessionReference(
        "c83c597b-7377-4886-99e3-2b5818fa5e4f", // serviceConfigurationId
        "team-deathmatch", // sessionTemplateName
        "mySession" // sessionName
        );

      MultiplayerSession^ multiplayerSession = ref new MultiplayerSession(
        xboxLiveContext,
        multiplayerSessionReference
        );

      auto asyncOp = xboxLiveContext->MultiplayerService->WriteSessionAsync(
        multiplayerSession,
        MultiplayerSessionWriteMode::CreateNew
        );

      create_task(asyncOp)
      .then([](MultiplayerSession^ newMultiplayerSession)
      {
        // Throw away stale multiplayerSession, and use newMultiplayerSession from now on
      });
    }


## <a name="set-an-arbiter-for-an-mpsd-session"></a>Establecer a un árbitro para una sesión MPSD




El título utiliza el procedimiento siguiente para establecer a un árbitro para una sesión que ya se ha creado.

| Nota                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------|
| Los tokens de dispositivo para los miembros (posibles hosts) no están disponibles hasta que han unido a la sesión y se incluyen sus direcciones de dispositivo segura los miembros. |

1.  Recuperar los tokens de dispositivo para los candidatos de host desde el MPSD mediante el **MultiplayerSession.Members propiedad**.

| Nota                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Si ha creado la sesión SmartMatch contactos, pueden usar los clientes de los candidatos de host disponibles en MPSD a través de la **MultiplayerSession.HostCandidates propiedad**. |

2.  Seleccione el host necesario en la lista de candidatos de host.
3.  Llame a la **MultiplayerSession.SetHostDeviceToken método** para establecer el token del dispositivo en la caché local de la MPSD. Si la llamada para establecer el host de token del dispositivo se realiza correctamente, el token del dispositivo local reemplaza el token del host.
4.  Si se recibe un código de estado HTTP/412 al intentar establecer el host de token del dispositivo, los datos de sesión de consulta y ver si el token del dispositivo host para la consola local. Si no es de la consola local, otra consola se ha designado como el árbitro.

| Nota                                                                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| El cliente debe controlar el código de estado HTTP/412 por separado de los demás códigos HTTP, dado que HTTP/412 no indica un error estándar. Para obtener más información sobre el código de estado, consulte [códigos de estado de sesión de varios jugadores](multiplayer-session-status-codes.md). |

5.  Actualice la sesión en MPSD, como se describe en **Cómo: Actualizar una sesión de varios jugadores**.

| Nota                                                                                                                                                                                                                                           |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si no dispone de ningún algoritmo mejor, el cliente puede implementar un algoritmo voraz en que cada candidato host intenta establecido como el host si nadie ha hecho aún. Para obtener más información, consulte [sesión árbitro](mpsd-session-details.md). |

## <a name="manage-title-activation"></a>Administrar la activación de título

Se desencadena Xbox One el **CoreApplicationView.Activated eventos** durante la activación de protocolos. En el contexto de la API de varios jugadores, este evento se desencadena cuando un usuario acepta una invitación, o une a otro usuario. Estas acciones desencadenan una activación que el título debe reaccionar a llevando el usuario de unión en un juego con el usuario de destino.

| Nota                                                                                       |
|---------------------------------------------------------------------------------------------------------|
| El título debe esperar nuevos argumentos de activación en cualquier momento y nunca debe codificarse en longitud. |

El título debe realizar los siguientes pasos principales para controlar la activación del título.

1.  Configurar un controlador de eventos para el **CoreApplicationView.Activated eventos**. Este controlador se desencadena cada vez que se produce la activación de protocolo, incluso si ya se está ejecutando el título.
2.  En la activación de título, iniciar una sesión y suscríbase para recibir notificaciones de cambio de sesión. Vea **Cómo: Suscríbase para recibir notificaciones de cambio de sesión MPSD**.
3.  Unir el usuario a la sesión como activa. Vea **Cómo: Unirse a una sesión MPSD de activación de un título**.
4.  Establecer la sesión introductoria como la sesión de actividad, que se exponen a través de la interfaz de usuario del perfil. Vea **Cómo: Establece la actividad del usuario actual**.
5.  Unir el usuario a la sesión de juego como activa. Ahora el usuario puede conectarse a los elementos del mismo nivel y escribir un juego o el área de recepción.

El diagrama de flujo siguiente muestra cómo administrar la activación del título.

![](../../images/multiplayer/Multiplayer_2015_OnActivation.png)

## <a name="make-the-user-joinable"></a>Hacer que el usuario puede unir

Para hacer que el usuario puede unir, el título debe hacer lo siguiente:

1.  Crear un objeto de sesión y modificar los atributos según sea necesario.
2.  Unir el usuario a la sesión como activa. Vea **Cómo: Unirse a una sesión MPSD de activación de un título**.
3.  Determinar si el usuario se ha designado como el árbitro de sesión.
4.  Si el usuario no es el árbitro, vaya al paso 7.
5.  Si el usuario es el árbitro, llame a la **MultiplayerSession.SetHostDeviceToken método**.
6.  Intente escribir la sesión mediante una llamada a la **MultiplayerService.TryWriteSessionAsync método**.
7.  Establecer la sesión como la sesión activa. Vea **Cómo: Establece la actividad del usuario actual**.

El siguiente diagrama de flujo ilustra los pasos necesarios para permitir que un usuario a otros jugadores combinarse durante un juego.

![](../../images/multiplayer/Multiplayer_2015_Become_Joinable.png)

## <a name="send-game-invites"></a>Enviar invitaciones de juego

El título puede permitir que un reproductor enviar invitaciones de juego de las maneras siguientes:

-   Enviar las invitaciones para la sesión introductoria.
-   Enviar las invitaciones con la plataforma Xbox genérica invitar a la interfaz de usuario con la referencia de la sesión de juego.

Para enviar invitaciones de juego para un Reproductor de, el título debe hacer lo siguiente:

1.  Hacer que el Reproductor de juego que invita puede unir. Vea **Cómo: Hacer que el usuario puede unir**.
2.  Determinar si son las invitaciones para enviarse a través de la sesión introductoria o mediante la interfaz de usuario de la invitación.
3.  Si usa la sesión introductoria, enviar las invitaciones mediante una llamada a **MultiplayerService.SendInvitesAsync método**. Este método puede requerir la creación de una lista de la interfaz de usuario en el juego con la **SystemUI.ShowPeoplePickerAsync método** o **PartyChat.GetPartyChatViewAsync método**.
4.  Si usa la interfaz de usuario de la invitación, llame a la **SystemUI.ShowSendGameInvitesAsync método** para mostrar la interfaz de usuario de la invitación.
5.  Controlar la **RealTimeActivityService.MultiplayerSessionChanged evento** para el Reproductor local después de las combinaciones de Reproductor remoto.
6.  Para el Reproductor remoto, implemente el código de activación de título. Vea **Cómo: Administrar la activación de título**.

El diagrama de flujo siguiente se muestra cómo enviar invitaciones.

![](../../images/multiplayer/Multiplayer_2015_Send_Invites.png)

## <a name="join-a-game-session-from-a-lobby-session"></a>Unirse a una sesión desde una sesión introductoria

Las sesiones de juego en dispositivos Windows 10 deben tener la `userAuthorizationStyle` funcionalidad establezca en **true** si no son las sesiones de gran tamaño. Esto significa que el `joinRestriction` propiedad no puede ser "none", lo que significa que la sesión no se puede unir públicamente directamente.

Un escenario común es crear una sesión introductoria para recopilar a los jugadores y, a continuación, mueva estos agentes en un juego de sesión o contactos. Pero si la sesión de juego no es públicamente joinable, los clientes de juego no podrá unirse a la sesión de juego a menos que cumplan la `joinRestriction` configuración, que es demasiado restrictivo en la mayoría de los casos para este escenario.

La solución es utilizar un identificador de transferencia para vincular la sesión introductoria y la sesión de juego.  El título puede hacerlo haciendo lo siguiente:

1. Cuando se crea la sesión de juego, utilice el `multiplayer_service::set_transfer_handle(gameSessionRef, lobbySessionRef)` API para crear un identificador de transferencia que vincula la sesión introductoria y la sesión de juego.
2. Store el identificador GUID de transferencia en la sesión introductoria en lugar de referencia de la sesión de la sesión de juego.
3. Cuando el título desea mover los miembros de la sesión introductoria a la sesión de juego, cada cliente usaría el identificador de transferencia de la sesión introductoria para unirse a la sesión de juego mediante el uso de la `multiplayer_service::write_session_by_handle(multiplayerSession, multiplayerSessionWriteMode, handleId)` API.
4. Busca la sesión introductoria para comprobar que los miembros que intenta unirse a la sesión de juego mediante el controlador de transferencia también están en la sesión introductoria MPSD.
5. Si los miembros se encuentran en la sesión introductoria, podrán tener acceso a la sesión de juego.

## <a name="join-an-mpsd-session-from-a-title-activation"></a>Unirse a una sesión MPSD de activación de un título

Cuando un usuario elige unir la actividad de un amigo o Aceptar una invitación mediante Xbox el shell de interfaz de usuario, el título se activa con los parámetros que indican qué sesión el usuario desea unirse. El título debe controlar esta activación y agregue el usuario a la sesión correspondiente.

Estos son los pasos que debe seguir el título:

1.  Implementar un controlador de eventos para el **CoreApplicationView.Activated eventos**. Este evento le notifica de activaciones del título.
2.  Cuando se activa el controlador, examine el **IActivatedEventArgs.Kind propiedad**. Si se establece en el protocolo, convertir los argumentos de evento **ProtocolActivatedEventArgs clase**.
3.  Examine el **ProtocolActivatedEventArgs** objeto. Si el URI indicado en el **ProtocolActivatedEventArgs.Uri propiedad** coincide con cualquier inviteHandleAccept (correspondiente a una invitación aceptada) o activityHandleJoin (que corresponde a una combinación a través del shell de interfaz de usuario), analizar la cadena de consulta del URI, que se da formato como una cadena de consulta URI normal con pares clave/valor, extraer los campos siguientes:
    -   Para obtener una invitación aceptada:
        1.  identificador
        2.  invitedXuid
        3.  senderXuid
    -   Para una combinación:
        1.  identificador
        2.  joinerXuid
        3.  joineeXuid

4.  Iniciar el código para varios jugadores del título, que debe incluir la llamada la **MultiplayerService.EnableMultiplayerSubscriptions método**.
5.  Crear una variable local **MultiplayerSession clase** objeto utilizando el **MultiplayerSession Constructor (XboxLiveContext)**.
6.  Llame a la **MultiplayerSession.Join método (String, Boolean, Boolean)** unirse a la sesión. Use los siguientes valores de parámetro para que la combinación se establece como activa:
    -   *memberCustomConstantsJson* = null
    -   *initializeRequested* = false
    -   *joinWithActiveStatus* = true

7.  Llame a la **MultiplayerSession.SetSessionChangeSubscription método** hombro-puntear cuando cambia la sesión después de unirse.
8.  Llame a la **MultiplayerService.WriteSessionByHandleAsync método**, utilizando el identificador obtenido a como se describe en el paso 3. Ahora el usuario es miembro de la sesión y puede usar datos en la sesión para conectar con el juego.

## <a name="set-the-users-current-activity"></a>Establece la actividad del usuario actual

La actividad del usuario actual se muestra en las experiencias de usuario del panel de Xbox para el título. Actividad de un usuario se puede establecer a través de una sesión o a través de la activación del título. En el último caso, el usuario introduce una sesión a través de los contactos o iniciar un juego.

| Nota                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Se puede eliminar actividad establece a través de una sesión con una llamada a la **MultiplayerService.ClearActivityAsync método**. |

Para establecer una sesión como la actividad del usuario actual, las llamadas de título del **MultiplayerService.SetActivityAsync método**, pasando la referencia de la sesión para la sesión.

Para establecer la actividad del usuario actual mediante la activación de título, consulte **Cómo: Unirse a una sesión MPSD de activación de un título**.

## <a name="update-an-mpsd-session"></a>Actualizar una sesión MPSD

| Nota                                                                                                                                                 |
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Cuando actualiza el título de una sesión existente mediante la API de varios jugadores, recuerde que está trabajando con una copia local, hasta que realiza una llamada a la sesión de escritura. |

Para actualizar una sesión existente, el título debe:

1.  Realizar cambios a la sesión actual según sea necesario, por ejemplo, mediante una llamada a la **MultiplayerSession.Leave método**.
2.  Cuando todos los cambios se realizan, escribir los cambios locales en MPSD, mediante cualquiera de estos métodos:

    -   **Método MultiplayerService.WriteSessionAsync**
    -   **Método MultiplayerService.WriteSessionByHandleAsync**.
    -   **Método MultiplayerService.TryWriteSessionAsync**
    -   **Método MultiplayerService.TryWriteSessionByHandleAsync**

    Establezca el modo de escritura en **SynchronizedUpdate** si escribir en una parte compartida que otros títulos también se puede modificar. Consulte [sincronización de actualizaciones de la sesión](multiplayer-session-directory.md) para obtener más información.

    El método write escribe la combinación en el servidor y obtiene la sesión más reciente, en el que se va a detectar otros miembros de la sesión y las direcciones de dispositivo segura (SDAs) de consolas. Para obtener más información acerca de cómo establecer una conexión de red entre estas consolas, consulte **Introducción a Winsock en Xbox One**.

3.  Descartar el objeto de sesión local antiguo y usar el objeto de sesión recién recuperados para que las acciones futuras se basan en el estado de sesión conocida más reciente.

## <a name="leave-an-mpsd-session"></a>Deje una sesión MPSD

Para permitir que un usuario deje una sesión, el título debe hacer lo siguiente.

1.  Llame a la **MultiplayerSession.Leave método** para la sesión de juego.
2.  Actualice la sesión de juego en MPSD, como se describe en **Cómo: Actualizar una sesión de varios jugadores**.
3.  Si es necesario, llame a **deje** método para la sesión introductoria y actualizar esa sesión.
4.  Si es necesario para la sesión introductoria, apague la API de varios jugadores anulando la **RealTimeActivityService.MultiplayerSubscriptionsLost eventos** y  **Evento RealTimeActivityService.MultiplayerSessionChanged**.

El siguiente diagrama de flujo ilustra cómo abandonar la sesión y apagar.

![](../../images/multiplayer/Multiplayer_2015_Shut_Down.png)

## <a name="fill-open-session-slots-during-matchmaking"></a>Rellenar las ranuras de la sesión abierta durante contactos

Para rellenar espacios abiertos en una sesión de vale durante contactos, el título debe seguir pasos similares a lo siguiente:

1.  El estado de sesión más reciente de acceso para la sesión de vale creado durante los contactos.
2.  Agregue los jugadores disponibles para el juego de la sesión introductoria.
3.  Determinar si la sesión de vale está lleno.
4.  Si la sesión está lleno, continuar con el juego.
5.  Si la sesión aún no está lleno, crear el vale de coincidencia como se describe en **Cómo: Cree una incidencia de coincidencia**. Asegúrese de crear el vale con el *preserveSession* parámetro se establece en Always.
6.  Continúe con los contactos. Consulte [con contactos SmartMatch](using-smartmatch-matchmaking.md).

El diagrama de flujo siguiente muestra cómo rellenar las ranuras de la sesión abierta durante los contactos.

![](../../images/multiplayer/Multiplayer_2015_Fill_Open_Slots.png)

## <a name="create-a-match-ticket"></a>Cree una incidencia de coincidencia

Para crear un vale de coincidencia, se debe el scout contactos:

1.  Llame a la **MatchmakingService.CreateMatchTicketAsync método**, pasando una referencia a la sesión de vale. El método lee la sesión de vale MPSD y contactos para los usuarios inicia en la sesión. Las llamadas al método internamente el **POST (/hoppers/ /serviceconfigs/ {¿scid} {hoppername})**.
2.  Establecer el *preserveSession* parámetro nunca si es el servicio para que coincida con los miembros de la sesión en una nueva sesión o en otra sesión existente. Establecer el *preserveSession* parámetro Always para permitir que el título a reutilizar una sesión de juegos existente como una sesión de vale para continuar el juego. El servicio, a continuación, puede asegurarse de que se conserva la sesión enviada y los reproductores coincidentes se agregan a esa sesión.

3.  Use la **CreateMatchTicketResponse.EstimatedWaitTime propiedad** devuelto en el **CreateMatchTicketResponse clase** objeto para establecer las expectativas del usuario de tiempo de contactos.
4.  Use la **CreateMatchTicketResponse.MatchTicketId propiedad** devuelto en el objeto de respuesta para cancelar los contactos de la sesión si es necesario, eliminando el vale. Vale de eliminación usa la **MatchmakingService.DeleteMatchTicketAsync método**.

## <a name="get-match-ticket-status"></a>Obtener el estado de vale de coincidencia

El título debe hacer lo siguiente para recuperar el estado de la incidencia de coincidencia:

1.  Obtener el **MultiplayerSession clase** objeto para la sesión de vale.
2.  Use la **MultiplayerSession.MatchmakingServer propiedad** para tener acceso a la **MatchmakingServer clase** objeto usado en contactos.
3.  Compruebe el **MatchmakingServer** objeto para determinar el estado de este proceso de contactos, el tiempo de espera típica para la sesión y la referencia de la sesión de destino, si se ha encontrado una coincidencia.
