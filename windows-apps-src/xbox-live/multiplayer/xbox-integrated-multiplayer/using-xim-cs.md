---
title: Uso de XIM (C#)
description: Aprenda a usar Xbox integrado participan varios jugadores (XIM) con C#.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox integrado multijugador
ms.localizationpriority: medium
ms.openlocfilehash: 92ac7b9897b57de42fa56126b477f4db5b9b74dd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619440"
---
# <a name="using-xim-c"></a>Uso de XIM (C#)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Este es un tutorial breve sobre el uso de XIM C# API. Deberían ver los desarrolladores de juegos que desean tener acceso a XIM mediante C++ [XIM Using (C++)](using-xim.md).
- [Uso de XIM (C#)](#using-xim-c)
    - [Requisitos previos](#prerequisites)
    - [Inicialización e inicio](#initialization-and-startup)
    - [Las operaciones asincrónicas y cambios de estado de procesamiento](#asynchronous-operations-and-processing-state-changes)
    - [Tratamiento de IXimPlayer básico](#basic-iximplayer-handling)
    - [Habilitación de sus amigos a participar y que se les invitará](#enabling-friends-to-join-and-inviting-them)
    - [Contactos básica y mover a otra red XIM con otros usuarios](#basic-matchmaking-and-moving-to-another-xim-network-with-others)
    - [Deshabilitando la regla NAT de contactos con fines de depuración](#disabling-matchmaking-nat-rule-for-debugging-purposes)
    - [Salir de una red XIM y limpieza](#leaving-a-xim-network-and-cleaning-up)
    - [Enviar y recibir mensajes](#sending-and-receiving-messages)
    - [Evaluación de la calidad de la ruta de datos](#assessing-data-pathway-quality)
    - [Uso compartido de datos mediante las propiedades personalizadas del Reproductor](#sharing-data-using-player-custom-properties)
    - [Uso compartido de datos mediante las propiedades personalizadas de red](#sharing-data-using-network-custom-properties)
    - [Uso por Reproductor aptitud de contactos](#matchmaking-using-per-player-skill)
    - [Uso de rol por el Reproductor de contactos](#matchmaking-using-per-player-role)
    - [Cómo funciona XIM con los equipos del Reproductor](#how-xim-works-with-player-teams)
    - [Trabajar con chat](#working-with-chat)
    - [Silenciar el audio de los jugadores](#muting-players)
    - [Configuración de destinos de chat con los equipos del Reproductor](#configuring-chat-targets-using-player-teams)
    - [Rellenar automáticamente en segundo plano de ranuras del Reproductor ("reposición" contactos)](#automatic-background-filling-of-player-slots-backfill-matchmaking)
    - [Consulta de redes puede unir](#querying-joinable-networks)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar a codificar con XIM, existen dos requisitos previos.

1. Debe haber configurado el AppXManifest de su aplicación con las capacidades de red multijugador estándares y debe haber configurado la parte de manifiesto de la red para declarar el tráfico necesario plantillas de patrón de XIM.

    AppXManifest capacidades y los manifiestos de red se describen con más detalle en las secciones respectivas de la documentación de platform; se proporciona el XML específico de XIM típico para pegar en [configuración del proyecto XIM](xim-manifest.md).

1. Necesita tener dos fragmentos de información de identidad de aplicación disponible:

    * El título asignado de Xbox Live ID.
    * El identificador de configuración de servicio que se proporciona como parte del aprovisionamiento de la aplicación para el acceso al servicio de Xbox Live.

    Consulte a su representante de Microsoft para obtener más información acerca de cómo obtener estas. Estos fragmentos de información se usará durante la inicialización.

## <a name="initialization-and-startup"></a>Inicialización e inicio

Empezar a interactuar con la biblioteca mediante la inicialización de las propiedades de la clase estática XIM con su cadena de Id. de configuración de servicio de Xbox Live y número de identificación del título. Si también usa la API de servicios de Xbox, es posible que resulte conveniente recuperar estos de `Microsoft.Xbox.Services.XboxLiveAppConfiguration`. En el siguiente ejemplo se da por supuesto que los valores ya residen en las variables 'myServiceConfigurationId' y 'myTitleId' respectivamente:

```cs
XboxIntegratedMultiplayer.ServiceConfigurationId = myServiceConfigurationId;
XboxIntegratedMultiplayer.TitleId = myTitleId;
```

Una vez inicializado, la aplicación debe recuperar las cadenas de identificador de usuario de Xbox para todos los usuarios actualmente en el dispositivo local que participan y pasarlos a un `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` llamar. El código de ejemplo siguiente se da por supuesto que un usuario ha presionado un botón del dispositivo de expresar la intención para reproducir y ya se ha recuperado la cadena de Id. de usuario de Xbox asociada con el usuario en una variable 'myXuid':

```cs
XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds(new List<string>() { myXuid });
```

Una llamada a `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` establece inmediatamente los identificadores de usuario de Xbox asociado a los usuarios locales que se deben agregar a la red XIM. Esta lista de identificadores de usuario de Xbox que se usará en todas las operaciones de la futura red hasta que cambie la lista a través de otra llamada a `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`.

En el caso donde no hay ninguna red XIM nada todavía, el primer paso es mover a una red XIM para que el proceso iniciado. Si el usuario no tiene ya una red XIM específica en mente, el procedimiento recomendado consiste en mover simplemente a una red nueva y vacía. Esto permite que sus amigos del usuario para unirse a la red como una especie de "introductoria" desde el que pueden colaborar para seleccionar su actividad multijugador siguiente (por ejemplo, introducir los contactos).

El siguiente es un ejemplo de cómo mover los usuarios locales se agregó anteriormente con `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` a una red XIM vacía con espacio para un total de hasta 8 reproductores:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringOnlyLocalPlayers);
```

La llamada a `XboxIntegratedMultiplayer.MovetoNewNetwork()` inicia asincrónico la operación de mover que concluye con un evento de cambio de estado que los desarrolladores deben procesar periódicamente para.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Las operaciones asincrónicas y cambios de estado de procesamiento

El corazón de XIM es la aplicación regular, frecuentes llamadas a la `XboxIntegratedMultiplayer.GetStateChanges()` método. Este método es cómo XIM se informa de que la aplicación está preparada para controlar las actualizaciones de estado de varios jugadores y cómo XIM proporciona esas actualizaciones devolviendo un `XimStateChangeCollection` objeto que contiene todas las actualizaciones en cola. `XboxIntegratedMultiplayer.GetStateChanges()` está diseñado para funcionar rápidamente, que puede llamarse cada fotograma de gráficos en el bucle de representación de la interfaz de usuario. Esto proporciona un lugar conveniente para recuperar todos los cambios de la cola sin preocuparse por la imprecisión de intervalos de red o la complejidad de la devolución de llamada multiproceso. La API XIM está optimizada para este patrón de un único subproceso. Garantiza su estado permanecerá sin cambios mientras un `XimStateChangeCollection` objeto es que se están procesando y aún no se ha eliminado.

El `XimStateChangeCollection` es una colección de `IXimStateChange` objetos.
Las aplicaciones deben:

1. Recorre en iteración la matriz.
1. Inspeccionar el `IXimStateChange` para su tipo más específico.
1. Conversión del `IXimStateChange` tipo correspondiente más detallada de tipo.
1. Controlar esa actualización según corresponda.

Una vez finalizado con todos los `IXimStateChange` objetos disponibles actualmente, se debe pasar esa matriz a XIM para liberar los recursos mediante una llamada a `XimStateChangeCollection.Dispose()`. El `using` instrucción se recomienda para que se garantiza que los recursos se elimina después del procesamiento. Por ejemplo:

```cs
using (var stateChanges = XboxIntegratedMultiplayer.GetStateChanges())
{
    foreach (var stateChange in stateChanges)
    {
        switch (stateChange.Type)
        {
            case XimStateChangeType.PlayerJoined:
                HandlePlayerJoined((XimPlayerJoinedStateChange)stateChange);
                break;

            case XimStateChangeType.PlayerLeft:
                HandlePlayerLeft((XimPlayerLeftStateChange)stateChange);
                break;

            ...
        }
    }
}
```

Ahora que tiene el bucle de procesamiento básico, puede controlar los cambios de estado asociados con la inicial `XboxIntegratedMultiplayer.MoveToNewNetwork()` operación. Cada operación de movimiento de red XIM comenzará con una `XimMoveToNetworkStartingStateChange`. Si el movimiento falla por alguna razón, la aplicación se proporcionará un `XimNetworkExitedStateChange`, que es el mecanismo para cualquier error asincrónico que no le permitirá mover a una red XIM o lo desconecta de la red XIM actual de control de error comunes. En caso contrario, el movimiento se completará con un `XimMoveToNetworkSucceededStateChange` después de que todos los Estados se ha finalizado y todos los jugadores se han agregado correctamente a la red XIM.

Cuando un usuario no tiene el privilegio de plataforma para reproducir con otros usuarios en una sesión de varios jugadores, XIM enviará a un dispositivo al intentar crear o unirse a una red XIM un `XimNetworkExitedStateChange` con el motivo `UserMultiplayerRestricted`. Esto sucede cuando se establezca cualquiera de los usuarios locales con `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()` tiene una restricción de varios jugadores de Xbox Live. Por favor, controlar adecuadamente para informar al usuario del problema.

## <a name="basic-iximplayer-handling"></a>Tratamiento de IXimPlayer básico

Suponiendo que el ejemplo de cómo mover un único usuario local a una nueva red XIM se realizó correctamente, la aplicación, se proporcionará un `XimPlayerJoinedStateChange` para una variable local `XimLocalPlayer` objeto. Este objeto serán válido hasta la correspondiente `XimPlayerLeftStateChange` para ha sido proporcionado y devuelve a través de `XimStateChangeCollection.Dispose()`. La aplicación siempre se proporcionará un `XimPlayerLeftStateChange` para cada `XimPlayerJoinedStateChange`.

También puede recuperar una lista de todos los `IXimPlayer` objetos en el XIM de red en cualquier momento mediante el uso de `XboxIntegratedMultiplayer.GetPlayers()`.

El `IXimPlayer` objeto tiene muchas propiedades útiles y métodos, como `IXimPlayer.Gamertag()` para recuperar la cadena de nombre de jugador de Xbox Live actual asociado con el Reproductor para fines de presentación. Si la `IXimPlayer` es local en el dispositivo, IXimPlayer.Local devolverá true. Una variable local `IXimPlayer` puede convertirse en un `XimLocalPlayer`, que tiene métodos adicionales disponibles solo a los jugadores locales.

Por supuesto, el estado más importante de los jugadores no es la información común que XIM sabe, pero lo que la aplicación específica que desee realizar un seguimiento. Puesto que es probable que tenga su propia construcción para esa información de seguimiento, conviene vincular el `IXimPlayer` objeto a su propio objeto de construcción del Reproductor para que cada vez que los informes XIM un `IXimPlayer`, puede recuperar rápidamente el estado sin tener que realizar una búsqueda estableciendo un reproductor personalizado del objeto de contexto. En el siguiente ejemplo se da por supuesto que es un objeto que contiene el estado privado en la variable 'myPlayerStateObject' y se ha agregado recientemente `IXimPlayer` objeto está en la variable 'newXimPlayer':

```cs
newXimPlayer.CustomPlayerContext = myPlayerStateObject;
```

Esto guarda el objeto especificado con el objeto player localmente (nunca transferidos a través de la red a los dispositivos remotos donde la memoria no sería válida). A continuación, podrá siempre regresar a su objeto recuperando el contexto personalizado y convertirlo a su objeto similar al ejemplo siguiente:

```cs
myPlayerStateObject = (MyPlayerState)(newXimPlayer.CustomPlayerContext);
```

Puede cambiar este contexto de Reproductor personalizado en cualquier momento.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Habilitación de sus amigos a participar y que se les invitará

Privacidad y seguridad, de todas las redes XIM nuevo se configuran automáticamente de manera predeterminada sólo se puede unir reproductores locales y depende de la aplicación para permitir explícitamente que otros usuarios una vez que esté listo. El ejemplo siguiente muestra cómo usar `XboxIntegratedMultiplayer.NetworkConfiguration` para recuperar la configuración de red actual y actualizar mediante joinability `XboxIntegratedMultiplayer.SetNetworkConfiguration()` para empezar a permitir que los nuevos usuarios locales a unirse como reproductores, así como otros usuarios que han sido invitados o que se va a " seguido de"(una Xbox Live sociales relación de) los jugadores ya está en la red XIM:

```cs
XimNetworkConfiguration currentConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
currentConfiguration.AllowedPlayerJoins = XimAllowedPlayerJoins.Local | XimAllowedPlayerJoins.Invited | XimAllowedPlayerJoins.Followed;
XboxIntegratedMultiplayer.SetNetworkConfiguration(currentConfiguration);
```

`XboxIntegratedMultiplayer.SetNetworkConfiguration()` ejecuta de forma asincrónica. Una vez que se complete la llamada de ejemplo de código anterior, un `XimNetworkConfigurationChangedStateChange` se proporciona para notificar a la aplicación que ha cambiado el valor joinability a su valor predeterminado de `XimAllowedPlayerJoins.None`. A continuación, puede consultar el nuevo valor comprobando el valor de `XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins`.

`XboxIntegratedMultiplayer.NetworkConfiguration.AllowedPlayerJoins` se puede comprobar mientras el dispositivo está en una red XIM para determinar el joinability de la red.

Debe uno de los jugadores locales desea enviar invitaciones a los usuarios remotos a incorporarse a esta red XIM, la aplicación puede llamar a `XimLocalPlayer.ShowInviteUI()` para iniciar la interfaz de usuario de invitación del sistema. En este caso, el usuario local puede seleccionar las personas que desean invitar y enviar invitaciones.

```cs
XimLocalPlayer.ShowInviteUI();
```

Una vez ejecutada la instrucción anterior, se mostrará la interfaz de usuario la invitación del sistema. Una vez que el usuario ha enviado las invitaciones (o en caso contrario, se descarta la interfaz de usuario), un `xim_show_invite_ui_completed_state_change` se proporcionará. Como alternativa, la aplicación puede enviar las invitaciones directamente mediante `XimLocalPlayer.InviteUsers()` sin que se muestre la interfaz de usuario de invitación del sistema.

En cualquier caso, los usuarios remotos recibirán un mensaje de invitación de Xbox Live donde haya iniciado sesión y puede aceptar. Se iniciará la aplicación en dichos dispositivos si no se está ejecutando, y "Protocolo activar" con los argumentos de evento que se pueden usar para mover a esta red XIM misma.

Consulte la documentación de plataforma para obtener más información sobre la activación de protocolo.

El ejemplo siguiente muestra cómo una llamada `XboxIntegratedMultiplayer.ExtractProtocolActivationInformation()` determina si los argumentos de evento son aplicables a XIM. Esto se da por supuesto que ha recuperado la cadena sin formato del URI de `Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs` a una variable 'uriString':

```cs
XimProtocolActivationInformation protocolActivationInformation;
XboxIntegratedMultiplayer.ExtractProtocolActivationInformation(uriString, out protocolActivationInformation);
if (protocolActivationInformation != null)
{
    // It is a XIM activation.
}
```

Si es una activación XIM, desea asegurarse de que el usuario local identificado en la propiedad 'LocalXboxUserId' de la `XimProtocolActivationInformation` objeto ha iniciado sesión y se encuentra entre los usuarios especificados a `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds()`. A continuación, puede iniciar mover a la red XIM especificada con una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` utilizando la misma cadena URI. Por ejemplo:

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs(uriString);
```

También tenga en cuenta que los usuarios remotos "seguir" puede navegar a la tarjeta del Reproductor del usuario local en el sistema de la interfaz de usuario e iniciar un intento de unir a sí mismos sin invitación (suponiendo que ha permitido dichas combinaciones Reproductor como se muestra arriba). Esta acción también se protocolo activar la aplicación tal como invitaciones y se controlan de la misma manera.

Mover a una red XIM mediante la activación de protocolos es idéntico a moverse a una nueva red XIM tal como se muestra en [inicio y la inicialización](#initialization-and-startup). La única diferencia es que cuando el movimiento se realiza correctamente, el dispositivo móvil se proporcionará el Reproductor local y remoto `XimPlayerJoinedStateChange` objetos que representan los jugadores aplicable. Naturalmente, no se puede mover el dispositivo original que ya estaba en la red XIM, pero verá que los usuarios del dispositivo nuevo se agrega como jugadores a través de otras `XimPlayerJoinedStateChange` objetos.

Una vez que se unen los jugadores en diferentes dispositivos en una red XIM, pueden iniciar escenarios multijugador, automáticamente se comunican a través de voz y texto y enviar mensajes específicos de la aplicación.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Contactos básica y mover a otra red XIM con otros usuarios

Puede expandir la experiencia de red para un grupo de amigos moviendo los jugadores a una red XIM que también tenga extraños. Extraños son oponentes de todo el mundo que se incluyan entre sí mediante según los mismos intereses el servicio Xbox Live.

Es una manera sencilla de iniciar contactos básica con XIM mediante una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` en uno de los dispositivos con un rellenado `MatchmakingConfiguration` objeto, tomar encargados de la red XIM actual junto con ella.

El ejemplo siguiente inicia un movimiento mediante una configuración de contactos que se configura para encontrar un total de 8 jugadores una coincidencia de fiesta inolvidable no equipos. Si no se encuentran 8 jugadores en total, el sistema podría coincidir los reproductores de 2 a 7 juntos. Este ejemplo utiliza una constante definida en la aplicación, de tipo uint, denominado MYGAMEMODE_DEATHMATCH, que representa el modo de juego para filtrar fuera de. Contactos del XIM sólo coincidirá con esta red con otros jugadores especificando el mismo valor y lleva a lo largo de todo social unido reproductores desde la red XIM actual al proporcionar el segundo parámetro `XimPlayersToMove.BringExistingSocialPlayers` a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Similar a movimientos anteriores, esto le proporcionará una inicial `MoveToNetworkStartingStateChange` en todos los dispositivos y un `MoveToNetworkSucceededStateChange` una vez finalizado el movimiento. Puesto que esto es un movimiento de una red XIM a otra, una diferencia es que ya existen `IXimPlayer` agregado objetos para los usuarios locales y remotos, y seguirán estando en todos los jugadores que pasan a la nueva red XIM juntos. Comunicación de chat y los datos entre los jugadores seguirán funcionando sin interrupciones mientras contactos está en curso.

Contactos pueden ser un proceso largo según el número de jugadores potenciales en el grupo de contactos que también han llamado `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

Un `XimMatchmakingProgressUpdatedStateChange` se proporcionará periódicamente a lo largo de la operación para que usted y a los usuarios informados del estado actual. Cuando se ha encontrado la coincidencia, los participantes adicionales se agregan a la red XIM con el típico `PlayerJoinedStateChange` y se completa el movimiento.

Una vez haya terminado la experiencia de varios jugadores con este conjunto de jugadores "matchmade", puede repetir el proceso de mover a otra red XIM con otra ronda de contactos. Verá cada jugador que combinen mediante previo `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` operación proporcionan un `PlayerLeftStateChange` para indicar que su `IXimPlayer` objetos ya no están en la misma red XIM.

Si cuando decide iniciar contactos utilizando de nuevo `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, especifique `XimPlayersToMove.BringExistingSocialPlayers`, los jugadores que combinen mediante puntos de entrada social (`XboxIntegratedMultiplayer.MoveToNetworkUsingProtocolActivatedEventArgs()` o `xXboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId`) permanecerá mientras lleva a los nuevos contactos. También puede especificar `XimPlayersToMove.BringOnlyLocalPlayers` desconectará social conectados reproductores remotos, dejando sólo los jugadores locales para que sigan siendo. En ambos casos, se agregará un conjunto diferente de extraños cuando se completa la segunda operación de movimiento.

También tiene la opción de mover los jugadores no matchmade (o reproductores solo locales) a una red XIM completamente nueva al decidir si la siguiente actividad multijugador/configuración de contactos.

El ejemplo siguiente muestra cómo podría llamar un dispositivo `XboxIntegratedMultiplayer.MoveToNewNetwork()` para una red XIM con un máximo de 8 jugadores, pero esta vez teniendo el social unido los reproductores existentes, así:

```cs
XboxIntegratedMultiplayer.MovetoNewNetwork(8, XimPlayersToMove.BringExistingSocialPlayers);
```

Un `MoveToNetworkStartingStateChange` y `MoveToNetworkSucceededStateChange` se proporcionará a todos los dispositivos que participan, junto con un `PlayerLeftStateChange` para el matchmade que se han olvidado. En dichos dispositivos, del mismo modo verá un `PlayerLeftStateChange` para cada jugador que se mueve fuera de la red.

Puede mover desde la red XIM a red XIM del modo descrito anteriormente tantas veces como desee.

Para el rendimiento, el servicio Xbox Live no intentará coincidir con los grupos de jugadores en los dispositivos que no están probable que establezcan las conexiones directas de punto a punto. Si está desarrollando en un entorno de red que no está configurado correctamente para admitir el estándar Xbox Live multijugador, el cambio a la nueva red mediante la operación de contactos podría continuar indefinidamente sin coincidencia correcta incluso cuando está seguro de que tiene suficientes jugadores cumplen los criterios de contactos que son todos ellos usando y mueve todos los dispositivos en el mismo entorno local. Asegúrese de ejecutar la prueba de conectividad para varios jugadores en la configuración de red de aplicación de área/Xbox y siga las recomendaciones si informa de problemas, especialmente con respecto a la NAT"Strict".

## <a name="disabling-matchmaking-nat-rule-for-debugging-purposes"></a>Deshabilitando la regla NAT de contactos con fines de depuración

Si no puede realizar los cambios de entorno necesarias para admitir las reglas NAT del XIM el Administrador de red, puede desbloquear los contactos de las pruebas en kits de desarrollo de Xbox One configurando XIM para permitir "Strict NAT" dispositivos sin al menos un "abrir coincidentes Dispositivo NAT". Esto se hace mediante la colocación de un archivo denominado "xim_disable_matchmaking_nat_rule" (no se importa el contenido) en la raíz de la unidad "cero título" en todas las consolas Xbox One. Es una manera de ejemplo para ello, ejecute lo siguiente desde un símbolo del sistema de XDK antes de iniciar la aplicación, reemplazando el marcador de posición "{console_name_or_ip_address}" para cada consola según corresponda:

```bat
echo.>%TEMP%\emptyfile.txt
copy %TEMP%\emptyfile.txt \\{console_name_or_ip_address}\TitleScratch\xim_disable_matchmaking_nat_rule
del %TEMP%\emptyfile.txt
```

> [!NOTE]
> Esta solución de desarrollo actualmente solo está disponible para aplicaciones de recurso exclusivo Xbox One y no se admite para aplicaciones universales de Windows. También tenga en cuenta que las consolas que usan esta configuración nunca coincidirá con los dispositivos que no tienen también este archivo está presente, independientemente del entorno de red, así que asegúrese de agregar y quitar el archivo en todas partes.

## <a name="leaving-a-xim-network-and-cleaning-up"></a>Salir de una red XIM y limpieza

Cuando se realizan los usuarios locales que participan en una red XIM, a menudo simplemente se moverá volver a una nueva red XIM que permite a los usuarios locales, invitaciones y "seguido de" los usuarios para unirse a él para que puedan coordinar con sus amigos para buscar la siguiente actividad. Pero si el usuario se realiza por completo con todas las experiencias multijugador, la aplicación que desee empezar dejando la XIM de red por completo y volver al estado como si solo `XboxIntegratedMultiplayer.SetIntendedLocalXboxUserIds` hubiera llamado. Esto se realiza mediante el `XboxIntegratedMultiplayer.LeaveNetwork()` método:

```cs
 XboxIntegratedMultiplayer.LeaveNetwork();
```

Este método comienza el proceso de forma asincrónica desconectando los demás participantes correctamente. Esto hará que los dispositivos remotos que se proporcione un `PlayerLeftStateChange` para cada jugador local que desconectado. El dispositivo local se también se proporcionará un `PlayerLeftStateChange` para cada jugador local y remoto que se ha desconectado. Cuando se desconectan todas las operaciones han finalizado, un final `NetworkExitedStateChange` se proporcionará.

Invocar `XboxIntegratedMultiplayer.LeaveNetwork()` y esperando la `NetworkExitedStateChange` para salir de un XIM red correctamente siempre recomienda cuándo un `NetworkExitedStateChange` no se ha proporcionado.

## <a name="sending-and-receiving-messages"></a>Enviar y recibir mensajes

XIM y sus componentes subyacentes hacen todo el trabajo tedioso de establecimiento de los canales de comunicación segura a través de Internet para que no tenga que preocuparse sobre problemas de conectividad o pueden llegar a algunos pero no todos los jugadores. Si hay algún problema de conectividad de punto a punto fundamental, mover a una red XIM no se realizará correctamente.

Cuando está formada y confirme con una red XIM `XimMoveToNetworkSucceededStateChange`, se garantiza que todas las instancias de la aplicación entre todos los dispositivos Unidos para mantenerse informado sobre cada `IXimPlayer` conectado a la red XIM y puede enviar mensajes a cualquiera de ellas.

Vamos a demostrar cómo enviar mensajes a través de la red XIM. En el siguiente ejemplo se da por supuesto que la variable 'sendingPlayer' es un objeto player local válido. El ejemplo se utiliza ' sendingPlayer'' método s `SendDataToOtherPlayers()` para enviar una estructura de mensaje que se ha copiado en el 'msgBuffer' todos los jugadores (local o remoto) en la red XIM. Tenga en cuenta que va a todos los reproductores porque una lista de jugadores específicos no se pasó al método:

```cs
SendingPlayer.SendDataToOtherPlayers(msgBuffer, null, XimSendType.GuaranteedAndSequential);
```

Todos los destinatarios del mensaje se proporcionará un `XimPlayertoPlayerDataReceivedStateChange` que incluye una copia de los datos, así como el `IXimPlayer` objeto que envía y una lista de la `IXimPlayer` objetos que están recibiendo localmente.

Por supuesto, es conveniente entrega garantizada, secuencial, pero también puede ser un tipo de envío ineficaz, ya que XIM debe retransmitir o retrasar los mensajes si los paquetes se quiten o se están ordenados por Internet. Asegúrese de considerar el uso de los otros tipos de envío para que la aplicación puede tolerar perder o tener los mensajes llegan sin orden. Puesto que los datos del mensaje proceden de un equipo remoto, el procedimiento recomendado es tener claramente definidos los formatos de datos, como el empaquetado de los valores de varios bytes en un orden determinado byte ("modos endian"). La aplicación también debe validar los datos antes de actuar en él.

XIM proporciona seguridad de red, por lo que no es necesario para las combinaciones de cifrado o firma adicionales. Sin embargo, siempre se recomienda emplear "defensa en profundidad" con el fin de protegerse frente a errores de la aplicación accidental y ser capaz de controlar distintas versiones de la coexistencia de protocolo de aplicación correctamente (durante el desarrollo, las actualizaciones de contenido, etcetera.). Conexiones de Internet de los usuarios pueden ser recursos limitados y cambiantes. Se recomienda encarecidamente el uso de formatos de datos de mensajes eficiente y evitar diseños que envían cada fotograma de la interfaz de usuario.

## <a name="assessing-data-pathway-quality"></a>Evaluación de la calidad de la ruta de datos

Para obtener información sobre la calidad actual de la ruta de datos entre dos jugadores inspeccionando el `XimPlayer.NetworkPathInformation` propiedad. Puede aprender más sobre la calidad actual de la ruta de acceso entre dos jugadores inspeccionando el `XimPlayer.NetworkPathInformation` propiedad. La propiedad incluye la latencia de ida y vuelta estimado y cuántos mensajes se siguen en la cola local para la transmisión en el caso de que la conexión no admite la transmisión de más datos para ese momento.

Si ve que las colas de una copia de seguridad para un determinado 'IXimPlayer', debe reducir la velocidad a la que va a enviar datos.

El `NetworkPathInformation.RoundTripLatency` campo representa la latencia de la red subyacente y la latencia estimada del XIM sin puesta en cola. Los aumentos de latencia efectiva como `XimNetworkPathInformation.SendQueueSizeInMessages` crece y XIM funciona a través de la cola.

Elija un punto razonable para iniciar la limitación de las llamadas a `SendDataToOtherPlayers` según los requisitos y el uso del juego. Todos los mensajes de la cola de envío representa un aumento de la latencia de red eficaz.

Un valor que se aproxima el límite máximo del XIM (actualmente 3500 mensajes) es demasiado alto para la mayoría de los juegos y es probable que representa varios segundos de datos en espera de envío según la velocidad de la llamada a `SendDataToOtherPlayers` y es el tamaño de cada carga de datos. En su lugar, elija un número que tiene en cuenta los requisitos de latencia del juego junto con cómo nerviosos juego `SendDataToOtherPlayers` es el patrón de llamada.

## <a name="sharing-data-using-player-custom-properties"></a>Uso compartido de datos mediante las propiedades personalizadas del Reproductor

Mayoría intercambios de datos de aplicación que se producen con el `XimLocalPlayer.SendDataToOtherPlayers()` método, ya que permite el máximo control sobre quién lo recibe y cuándo, cómo debe tratar con pérdida de paquetes y así sucesivamente. Sin embargo, hay veces cuando sería bueno para reproductores compartir el estado básico, casi no presentan cambio sobre sí mismos con otras personas sin mayores inconvenientes. Por ejemplo, cada jugador podría tener una cadena fija que representa el modelo de caracteres seleccionado antes de entrar en varios jugadores que todos los reproductores de usan para representar su representación en el juego.

Para los datos que no cambian muy a menudo sobre un Reproductor, XIM proporciona a Reproductor propiedades personalizadas. Estas propiedades constan de un nombre definido por la aplicación y un valor, que son pares de cadena terminada en null que se puede aplicar al Reproductor local y que se propagan automáticamente a todos los dispositivos cada vez que se cambian.

Las propiedades de Reproductor personalizado tienen sincronización interna más sobrecarga que el `XimLocalPlayer.SendDataToOtherPlayers()` método, por lo que para cambiar rápidamente los datos (es decir, la posición del jugador), debería usar aún envía directa.

Pares de clave-valor de propiedades personalizadas de Reproductor tienen sus valores actuales que se proporciona automáticamente a nuevos dispositivos participantes al unirse a una red XIM y vea el Reproductor agregarlo estos dispositivos. Se pueden establecer valores mediante una llamada a `XimLocalPlayer.SetPlayerCustomProperty()` con el nombre y valor de cadenas, como en el ejemplo siguiente, que establece una propiedad denominada "model" para que tenga el valor "fuerza bruta" en una variable local `IXIMPlayer` objeto apunta a la variable 'localPlayer':

```cs
localPlayer.SetPlayerCustomProperty("model","brute");
```

Los cambios en las propiedades del Reproductor provocará un `XimPlayerCustomPropertiesChangedStateChange` proporcionarse a todos los dispositivos, les alertan a los nombres de propiedades que han cambiado. El valor de un nombre determinado se puede recuperar en cualquier reproductor, local o remoto, con `IXIMPlayer.GetPlayerCustomProperty()`. El ejemplo siguiente recupera el valor para una propiedad denominada "model" de un `IXIMPlayer` apunta a la variable 'ximPlayer':

```cs
string propertyValue = localPlayer.GetPlayerCustomProperty("model");
```

Establecer un nuevo valor para un nombre de propiedad dado, se reemplazará cualquier valor existente. Establecer un nombre de propiedad en un valor de un puntero de cadena del valor null se trata igual que una cadena de un valor vacío, que es el mismo que la propiedad no tener se ha especificado aún. Los nombres y valores no se interpretan XIM, por lo tanto, se deja en la aplicación para validar el contenido de cadena según sea necesario.

Las propiedades de Reproductor personalizado siempre se restablecen al pasar de una red XIM a otro. Sin embargo, los nuevos jugadores que se unen a la red XIM recibirán propiedades actuales de Reproductor personalizado para todos los jugadores que tienen un conjunto.

## <a name="sharing-data-using-network-custom-properties"></a>Uso compartido de datos mediante las propiedades personalizadas de red

Propiedades personalizadas de red están diseñadas para su comodidad sincronizar datos específicos de la red XIM que no cambia con frecuencia. Trabajo de las propiedades personalizadas de red de forma idéntica a [propiedades personalizadas del Reproductor](#sharing-data-using-player-custom-properties), excepto en están establecidas en el objeto singleton XIM con `XboxIntegratedMultiplayer.SetNetworkCustomProperty()`. El ejemplo siguiente establece una propiedad de "mapa" para que el valor "stronghold":

```cs
XboxIntegratedMultiplayer.SetNetworkCustomProperty("map", "stronghold");
```

Los cambios en las propiedades de red provocará un `NetworkCustomPropertiesChanged` proporcionarse a todos los dispositivos, les alertan a los nombres de propiedades que han cambiado. El valor de un nombre determinado se puede recuperar con `XboxIntegratedMultiplayer.GetNetworkCustomProperty()`, como en el ejemplo siguiente, que recupera el valor de una propiedad con nombre "asignación":

```cs
string property = XboxIntegratedMultiplayer.GetNetworkCustomProperty("map")
```

Al igual que [propiedades personalizadas del Reproductor](#sharing-data-using-player-custom-properties), establecer un valor nuevo para un nombre de propiedad dado reemplazará cualquier valor existente. Establecer un nombre de propiedad en un valor de un puntero de cadena del valor null se trata igual que una cadena de un valor vacío, que es el mismo que la propiedad no tener se ha especificado aún. Los nombres y valores no se interpretan XIM, por lo tanto, se deja en la aplicación para validar el contenido de cadena según sea necesario.

Recién creado XIM redes siempre empiezan con ningún conjunto de propiedades personalizadas de la red. Sin embargo, los nuevos jugadores que se unen a una red existente de XIM recibirán los valores actuales de las propiedades personalizadas de red establecida para esa red XIM.

## <a name="matchmaking-using-per-player-skill"></a>Uso por Reproductor aptitud de contactos

Búsqueda de coincidencias reproductores interés común en un determinado modo de juego especificado a la aplicación es una buena estrategia de base. A medida que crece el grupo de jugadores disponibles, considere la posibilidad de coincidencia también reproductores según sus conocimientos o experiencia con su juego para que los jugadores veteranos pueden disfrutar el desafío de sana competición con otros veteranos, mientras que los reproductores más recientes pueden crecer por compite con otros usuarios con funcionalidades similares.

Para ello, inicie proporcionando el nivel de aptitud para todas las compañías locales en sus roles de por el Reproductor y la estructura de configuración de aptitud especificadas en llamadas a `XimLocalPlayer.SetRolesAndSkillConfiguration()` antes de comenzar a mover a un XIM de red con contactos. Nivel de dificultad es un concepto específico de la aplicación y el número no interpreta XIM, excepto en que se primero intenta encontrar los reproductores con el mismo valor de cualificación contactos y, a continuación, periódicamente ampliar su búsqueda en incrementos de +/-10 para intentar encontrar otros jugadores declarar habilidades valores dentro de un intervalo en torno a dicha aptitud. En el siguiente ejemplo se da por supuesto que el equipo local `XimLocalPlayer` objeto, cuyo variable es 'localPlayer', tiene un valor de cualificación de entero específico de la aplicación asociada recuperado de almacenamiento local o Xbox Live en una variable denominada 'playerSkillValue':

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Skill = playerSkillValue;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Cuando se complete, todos los participantes se proporcionará un `XimPlayerRolesAndSkillConfigurationChangedStateChange` que indica esto `IXIMPlayer` ha cambiado su roles por Reproductor y la configuración de la aptitud. El nuevo valor se puede recuperar mediante una llamada a `IXIMPlayer.RolesAndSkillConfiguration`. Cuando todos los jugadores tienen aplique la configuración de contactos que no son null, puede mover a una red XIM con contactos con un valor True para el `RequirePlayerRolesAndSkillConfiguration` campo de la `XimMatchmakingConfiguration` estructura especificada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`.

El ejemplo siguiente rellena una configuración de contactos para encontrar un total de los jugadores del 2 al 8 de una fiesta de inolvidable no equipos. Además, en este ejemplo se utiliza una constante definida en la aplicación, que es de tipo Uint64 y denominado MYGAMEMODE_DEATHMATCH, que representa el modo de juego para filtrar fuera de. Esto configura los contactos para que coincida con los encargados de la red XIM con otros jugadores especificar esos mismos valores, así como la necesidad de configuración de contactos por el Reproductor.

```cs
XimMatchmakingConfiguration matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 1;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
matchmakingConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.RequirePlayerRolesAndSkillConfiguration = true;
```

Cuando se proporciona esta estructura para `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`, la operación de movimiento se iniciará con normalidad, siempre que han llamado a reproductores mover `XimLocalPlayer.SetRolesAndSkillConfiguration()` con un valor no null `XimPlayerRolesAndSkillConfiguration` objeto. Si cualquier jugador no ha hecho, a continuación, se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `XimMatchmakingProgressUpdatedStateChange` con un `WaitingForPlayerRolesAndSkillConfiguration` valor. Esto incluye los jugadores que se unen posteriormente join la red XIM a través de una invitación enviada anteriormente o a través de otros medios sociales (por ejemplo, una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) antes de contactos han finalizado. Una vez que han proporcionado todos los jugadores sus `XimPlayerRolesAndSkillConfiguration` objetos, se reanudarán contactos.

Contactos con conocimientos por el Reproductor también pueden combinarse con rol de contactos usuario por el Reproductor, como se explica en la sección siguiente. Si sólo uno es el deseado, puede especificar un valor de 0 para los demás. Esto es porque todos los reproductores de declarar tienen un `XimPlayerRolesAndSkillConfiguration` valor aptitud 0 siempre coinciden entre sí.

Una vez el `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` o cualquier otra red XIM mover la operación ha finalizado, todos los jugadores `XimPlayerRolesAndSkillConfiguration` estructuras se borrará automáticamente a un puntero null (con una que lo acompañan `XimPlayerRolesAndSkillConfigurationChangedStateChange` notificación). Si va a mover a otra red XIM con contactos que requiera una configuración por el Reproductor, deberá llamar a `XimLocalPlayer.SetRolesAndSkillConfiguration()` nuevo con un objeto que contiene la información más actualizada.

## <a name="matchmaking-using-per-player-role"></a>Uso de rol por el Reproductor de contactos

Otro método del uso de roles por Reproductor y la configuración de aptitud para mejorar la experiencia de contactos de los usuarios es mediante el uso de roles necesarios del Reproductor. Esto se adapta mejor a los juegos que proporcionan tipos de caracteres seleccionable que fomentan cooperativo diferentes estilos de reproducir. Estos tipos de caracteres son las que no basta con modificar la representación gráfica del juego y, en su lugar, modificar el estilo de juego para que el Reproductor. Los usuarios quizás prefiera jugar como una especialización determinada. Sin embargo, si su juego está diseñado para que funcionalmente no es posible completar los objetivos sin al menos una persona de cumplimiento de cada rol, a veces es mejor para que coincida con estos reproductores primero de que coinciden con los reproductores juntos necesario, a continuación, volver a negociar play estilos entre ellos una vez recopilada. Puede hacerlo mediante la primera definición de un indicador de bits única que represente cada rol que se especifique en un reproductor determinado `XimPlayerRolesAndSkillConfiguration` estructura.

En el ejemplo siguiente se establece un valor específico de la aplicación, que es de tipo byte y MYROLEBITFLAG_HEALER, el nombre para el equipo local `XimLocalPlayer` objeto cuyo puntero se encuentra 'localPlayer':

```cs
var config = new XimPlayerRolesAndSkillConfiguration();
config.Roles = MYROLEBITFLAG_HEALER;
localPlayer.SetRolesAndSkillConfiguration(config);
```

Cuando se complete, todos los participantes se proporcionará un `XimPlayerRolesAndSkillConfigurationChangedStateChange` que indica esto `IXimPlayer` ha cambiado su roles por Reproductor y la configuración de la aptitud. El nuevo valor se puede recuperar mediante una llamada a `IXimPlayer.RolesAndSkillConfiguration`.

Global `XimMatchmakingConfiguration` estructura especificada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` deben, a continuación, todas las marcas de roles necesarios combinaron con OR bit a bit y un valor True para el `RequirePlayerRolesAndSkillConfiguration` campo.

Contactos con rol por el Reproductor también pueden combinarse con la habilidad de contactos usuario por el Reproductor. Si sólo uno es el deseado, especifique un valor de 0 para los demás. Esto es porque todos los reproductores de declarar tienen un `XimPlayerRolesAndSkillConfiguration` valor aptitud 0 siempre coinciden entre sí; y, si todos los bits son cero en el `XimMatchmakingConfiguration.RequiredRoles` campo, a continuación, ningún bit de rol es necesarios para que coincida.

Una vez el `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` o cualquier otra red XIM mover la operación ha finalizado, todos los jugadores `XimPlayerRolesAndSkillConfiguration` objetos se borrarán automáticamente en null (con una que lo acompañan `XimPlayerRolesAndSkillConfigurationChangedStateChange` notificación). Si va a mover a otra red XIM con contactos que requiera una configuración por el Reproductor, deberá llamar a `XimLocalPlayer.SetRolesAndSkillConfiguration()` nuevo con un objeto que contiene la información más actualizada.

## <a name="how-xim-works-with-player-teams"></a>Cómo funciona XIM con los equipos del Reproductor

Varios jugadores de juegos a menudo implica a personas organizadas en opuestos a los equipos. Hace XIM que resulte fácil asignar equipos al contactos estableciendo `XimMatchmakingConfiguration.TeamConfiguration`. El ejemplo siguiente inicia un movimiento con contactos configurada para buscar un total de 8 jugadores para colocar en dos equipos de 4 (aunque si no se encuentran 4, 1-3 reproductores también son aceptables). Además, en este ejemplo se utiliza una constante definida en la aplicación, que es de tipo uint y denominado MYGAMEMODE_CAPTURETHEFLAG, que representa el modo de juego para filtrar fuera de.  Además, la configuración está configurada para llevar todas social unido reproductores desde la red XIM actual:

```cs
var matchmakingConfiguration = new XimMatchmakingConfiguration();
matchmakingConfiguration.TeamConfiguration.TeamCount = 2;
matchmakingConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
matchmakingConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 4;
XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking(matchmakingConfiguration, XimPlayersToMove.BringExistingSocialPlayers);
```

Cuando se completa este tipo una operación de movimiento XIM red, se asignará los encargados de un equipo valor de índice 1 a través de {n} correspondiente a los equipos de {n} solicitados. Valor de índice del equipo de un jugador se recupera mediante `IXIMPlayer.TeamIndex`. Cuando se usa un `XimMatchmakingConfiguration.TeamConfiguration` con dos o más equipos, los jugadores nunca se asignará un valor de índice del equipo de cero por la llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()`. Esto es en contraste a reproductores que se agregan a la red XIM con cualquier otra configuración o tipo de operación de movimiento (como a través de una activación de protocolos resultante de aceptar la invitación), que siempre tendrá un índice cero del equipo. Puede ser útil tratar el índice 0 de equipo como un equipo especial "sin asignar".

El ejemplo siguiente recupera el índice del equipo para un objeto de Reproductor XIM almacenado en la variable 'ximPlayer':

```cs
byte playerIndex = ximPlayer.TeamIndex;
```

Para la experiencia de usuario preferidos (no a mencionar que reduce la oportunidad de comportamiento de los jugadores negativo), el servicio Xbox Live nunca será reproductores de división que se están trasladando a una red XIM juntas en distintos equipos.

El valor de índice de equipo asignado inicialmente por contactos es sólo una recomendación y la aplicación puede cambiar para reproductores locales en cualquier momento mediante `XimLocalPlayer.SetTeamIndex()`. Esto también se puede llamar en las redes XIM que no usan en todos los contactos. El ejemplo siguiente configura un objeto de Reproductor local XIM almacenado en la variable 'ximLocalPlayer' para tener un nuevo valor de índice del equipo de uno:

```cs
ximLocalPlayer.SetTeamIndex(1);
```

Todos los dispositivos se les informa de que el jugador tiene un nuevo valor de índice del equipo en vigor cuando les proporciona un `XimPlayerTeamIndexChangedStateChange` para que el Reproductor. Puede llamar a `XimLocalPlayer.SetTeamIndex()` en cualquier momento.

Contactos se evalúa como los roles necesarios por Reproductor independientemente de los equipos. Por lo tanto, no se recomienda utilizar los equipos y roles necesarios como criterios de la configuración de contactos simultáneas porque los equipos se equilibrará por recuento de Reproductor, no por funciones del Reproductor que se cumple.

## <a name="working-with-chat"></a>Trabajar con chat

Comunicación de chat de voz y texto se habilitan automáticamente entre los jugadores en una red XIM. XIM controla interactuar con todo voz auriculares con micrófono el hardware y para usted. La aplicación no necesitará hacer mucho para chat, pero tiene un requisito con respecto a la charla de texto: compatibilidad con entrada y la presentación. Entrada de texto es necesario porque, incluso en plataformas o géneros de juegos que históricamente no tuvieron la generalizada teclado físico usar, reproductores pueden configurar el sistema para usar las tecnologías de asistencia texto a voz. De forma similar, la presentación del texto es necesaria porque los jugadores pueden configurar el sistema para usar texto a voz.

Si desea habilitar condicionalmente los mecanismos de texto, se pueden detectar estas preferencias reproductores local mediante el acceso a la `XimLocalPlayer.ChatTextToSpeechConversionPreferenceEnabled` y `XimLocalPlayer.ChatSpeechToTextConversionPreferenceEnabled` campos respectivamente, y puede que desee habilitar condicionalmente los mecanismos de texto. Sin embargo, se recomienda que considere la posibilidad de realizar el texto de entrada y mostrar las opciones que están siempre disponibles.

`Windows::Xbox::UI::Accessability` es una clase de Xbox One específicamente diseñada para proporcionar una representación sencilla de chat de texto en el juego con un enfoque en las tecnologías de voz a texto.

Una vez que tenga la entrada de texto que se proporcionó mediante un teclado real o virtual, pase la cadena para el `XimLocalPlayer.SendChatText()` método. El código siguiente muestra el envío de un ejemplo de cadena codificados de forma rígida de una variable local `IXIMPlayer` objeto apunta a la variable 'localPlayer':

```cs
localPlayer.SendChatText(text);
```

Este texto de la conversación se entrega a todos los jugadores en la red XIM que pueden recibir comunicación de chat de Reproductor el origen local como un `XimChatTextReceivedStateChange`. Es posible que sintetizada audio de voz si está habilitada la TTS.

La aplicación debe realizar una copia de cualquier cadena de texto recibido y mostrarlo junto con algunos identificación del Reproductor original una cantidad adecuada de tiempo (o en una ventana desplazable).

También hay algunas prácticas recomendadas con respecto a la conversación. Se recomienda que en cualquier parte los jugadores se muestran, especialmente en una lista de nombres de jugadores, como un panel de resultados, también mostrar iconos de Silenciar de habla como comentarios del usuario. Esto se hace mediante el acceso a `IXimPlayer.ChatIndicator` para recuperar un `XboxIntegratedMultiplayer.XimPlayerChatIndicator` que representa el estado actual, la instantáneo de chat para que el Reproductor.

El valor indicado por `IXimPlayer.ChatIndicator` se espera que cambie con frecuencia como reproductores start y stop hablar, por ejemplo. Está diseñado para admitir aplicaciones de sondeo cada fotograma de la interfaz de usuario como resultado.

> [!NOTE]
> Si un usuario local no tiene suficientes privilegios de las comunicaciones debido a la configuración del dispositivo, `XimLocalPlayer.ChatIndicator` devolverá un `XboxIntegratedMultiplayer.XimPlayerChatIndicator` con valor `XIM_PLAYER_CHAT_INDICATOR_PLATFORM_RESTRICTED`. La expectativa para cumplir los requisitos para la plataforma es que la aplicación muestran iconografía que indica una restricción de la plataforma de chat de voz o de mensajería y un mensaje al usuario que indica el problema. Se recomienda un mensaje de ejemplo es: "Lo sentimos, no puede hablar ahora."

## <a name="muting-players"></a>Silenciar el audio de los jugadores

Otra práctica recomendada es compatible con los reproductores silenciar el audio. XIM controla automáticamente el sistema silenciamiento iniciadas por los usuarios a través de las tarjetas del Reproductor, pero las aplicaciones deben admitir juegos específicos transitorio de silencio que pueden realizarse dentro de la interfaz de usuario de juego a través de la `IXimPlayer.ChatMuted` propiedad. El ejemplo siguiente comienza silenciamiento remoto `IXIMPlayer` objeto apunta a la variable 'remotePlayer' para que no se escucha ningún chat de voz y no se recibe ningún chat de texto de él:

```cs
remotePlayer.ChatMuted = true;
```

El silenciar el audio entra en vigor inmediatamente y no existe no es ningún cambio de estado XIM asociado con él. Puede cambiar el valor de la `IXimPlayer.ChatMuted` propiedad en false. En el ejemplo siguiente, se reactiva un elemento remoto `IXIMPlayer` objeto apunta a la variable 'remotePlayer':

```cs
remotePlayer.ChatMuted = false;
```

Silencia siguen en vigor para siempre y cuando el `IXIMPlayer` existe, como cuando se mueve a una nueva red XIM con el Reproductor. No se conserva si el jugador sale y vuelve a unir el mismo usuario (como un nuevo `IXIMPlayer` instancia).

Normalmente, los jugadores comienzan en el estado reactivó el micrófono. Si desea que la aplicación iniciar un reproductor en el estado desactivado por motivos de juego, se puede establecer `IXIMPlayer.ChatMuted` en el `IXIMPlayer` objeto antes de finalizar el procesamiento asociado `PlayerJoinedStateChange`, y XIM garantizará que no habrá ningún período de tiempo donde de voz de audio el Reproductor pueda ser escuchado.

Una comprobación de silencio automática basada en la reputación del Reproductor se produce cuando un jugador remoto une a la red XIM. Si el jugador tiene una marca de mala reputación, el Reproductor se desactiva automáticamente. Silenciar solo afecta al estado local y continúa, por tanto, si un jugador se mueve a través de redes. Se realiza una vez y no vuelve a evaluar nuevo para la comprobación automática de silencio reputación siempre y cuando el `IXIMPlayer` sigue siendo válida.

## <a name="configuring-chat-targets-using-player-teams"></a>Configuración de destinos de chat con los equipos del Reproductor

Como se indica en la [XIM cómo funciona con los equipos del Reproductor](#how-xim-works-with-player-teams) sección de este documento, el verdadero significado de cualquier valor de índice de equipo determinado depende de la aplicación. XIM no interpretarlos excepto para las comparaciones de igualdad con respecto a la configuración del destino de conversación. Si informa de la configuración de destino de chat `XboxIntegratedMultiplayer.ChatTargets` está actualmente `XimChatTargets.SameTeamIndexOnly`, a continuación, cualquier reproductor determinado solo intercambiará comunicación de chat con otro si los dos tienen el mismo valor notificado por `IXIMPlayer.TeamIndex` (y privacidad / directiva también permite).

Para ser conservador y compatibilidad con escenarios competitivos, recién creado redes XIM automáticamente son configurado como valor predeterminado `XimChatTargets.SameTeamIndexOnly`. Sin embargo, puede ser deseable, por ejemplo, en un juego posterior a la "introductoria" charlar con vanquished oponentes en otro equipo. Puede indicar a XIM para permitir que todo el mundo para comunicarse con todos los demás (donde permitan privacidad y la directiva) mediante una llamada a `XboxIntegratedMultiplayer.SetChatTargets()`. El ejemplo siguiente comienza a configurar todos los participantes en la red XIM para usar un `XimChatTargets.AllPlayers` valor:

```cs
XboxIntegratedMultiplayer.SetChatTargets(XimChatTargets.AllPlayers)
```

Todos los participantes se informaren de que una nueva configuración de destino está en vigor cuando les proporciona un `XimChatTargetsChangedStateChange`.

Como se indicó anteriormente, tipos de movimiento de red XIM mayoría inicialmente asignará a todos los jugadores el valor de índice del equipo de cero. Esto significa que una configuración de `XimChatTargets.SameTeamIndexOnly` es probable que indistinguible de `XimChatTargets.AllPlayers` de forma predeterminada. Sin embargo, reproductores que mover a una red XIM con contactos tendrán distintos valores de índice del equipo si la configuración de contactos `TeamMatchmakingMode` valor declara dos o más equipos. También puede llamar a `XimLocalPlayer.SetTeamIndex()` en cualquier momento, como se indicó anteriormente. Si su aplicación usa los valores de índice de equipo distinto de cero a través de cualquiera de estos métodos, no olvide administrar los destinos de chat actual establecer de forma adecuada.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Rellenar automáticamente en segundo plano de ranuras del Reproductor ("reposición" contactos)

Grupos distintos de reproductores de una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` ofrece el servicio Xbox Live la máxima flexibilidad para organizarlos en nuevas redes XIM óptimo rápidamente al mismo tiempo. Sin embargo, algunos escenarios de juego le gustaría mantener una red determinada de XIM intacta y sólo entablar relación adicionales reproductores sólo para llenar las ranuras de vacantes Reproductor. Admite la XIM configuración contactos para que funcione en automáticamente en segundo plano rellenar el modo o "reposición", mediante una llamada a `XboxIntegratedMultiplayer.SetNetworkConfiguration()` con un `XimNetworkConfiguration` cuya `XimAllowedPlayerJoins.Matchmade` marcador establecido en su `XimNetworkConfiguration.AllowedPlayerJoins` propiedad.

El ejemplo siguiente configura la reposición contactos para intentar localizar un total de 8 jugadores para una fiesta de inolvidable no equipos (aunque si no se encuentran 8, reproductores de 2 a 7 también son aceptables), mediante un uint64_t constante de modo de juego específicos de la aplicación definida por el valor MYGAMEMODE_ DEATHMATCH que sólo coincidirá con otros jugadores especificar ese mismo valor:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Matchmade;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 2;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = MYGAMEMODE_DEATHMATCH;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Esto hace que el XIM existente de red disponible para dispositivos de una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingMatchmaking()` de la manera normal. Esos dispositivos no ver ningún comportamiento cambian. Los participantes en la red de reposición XIM no se moverá, pero se proporcionará un `XimNetworkConfigurationChangedStateChange` lo que significa reposición activar, así como varios `XimMatchmakingProgressUpdatedStateChange` notificaciones cuando sea aplicable. Se agregarán a la red XIM con el valor normal a cualquier reproductor matchmade `XimPlayerJoinedStateChange`.

De forma predeterminada, los contactos de reposición permanecen habilitado indefinidamente, aunque no intente agregar los jugadores si la red XIM ya tiene el número máximo de jugadores especificado por el `XimNetworkConfiguration.TeamConfiguration` configuración. La reposición puede deshabilitarse estableciendo `XimNetworkConfiguration.AllowedPlayerJoins` sin incluir `XimAllowedPlayerJoins.Matchmade`:

```cs
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins &= ~XimAllowedPlayerJoins.Matchmade;
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Correspondiente `XimNetworkConfigurationChangedStateChange` se proporcionará a todos los dispositivos, y cuando haya completado este proceso asincrónico, un final `XimMatchmakingProgressUpdatedStateChange` le proporcionará `MatchmakingStatus.None` para indicar que no se agregará a la red XIM ningún reproductores matchmade adicionales.

Al habilitar la reposición contactos con un `XimNetworkConfiguration.TeamConfiguration` establecer que declara dos o más equipos, todos los reproductores existentes deben tener un índice de equipo válido comprendido entre 1 y el número de equipos. Esto incluye los jugadores que han llamado `XimLocalPlayer.SetTeamIndex()` para especificar un valor personalizado o que se han unido a mediante una invitación o a través de otro social significa (p. ej., una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableXboxUserId()`) y se han agregado con un valor de índice del equipo predeterminado de 0. Si cualquier jugador no tiene un índice válido de equipo, entonces se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `XimMatchmakingProgressUpdatedStateChange` con un `MatchmakingStatus.WaitingForPlayerTeamIndex` valor. Una vez que todos los jugadores han proporcionado o corregir sus valores de índice del equipo con `XimLocalPlayer.SetTeamIndex()`, contactos de reposición se reanudarán. Puede encontrar más información en el [XIM cómo funciona con los equipos del Reproductor](#how-xim-works-with-player-teams) sección de este documento.

De forma similar, cuando se habilita la reposición contactos con un `XimNetworkConfiguration` estructura con la `RequirePlayerRolesAndSkillConfiguration` campo establecido en true, entonces todos los jugadores deben haber especificado una configuración de contactos por Reproductor distinto de null. Si cualquier jugador no ha hecho, a continuación, se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `XimMatchmakingProgressUpdatedStateChange` con un `XimMatchMakingStatus.WaitingForPlayerRolesAndSkillConfiguration` valor. Una vez que han proporcionado todos los jugadores sus `XimPlayerRolesAndSkillConfiguration` objetos, se reanudarán la reposición contactos. Puede encontrar más información en el [contactos con conocimientos por Reproductor](#matchmaking-using-per-player-skill) y [contactos con rol por el Reproductor](#matchmaking-using-per-player-role) secciones de este documento.

## <a name="querying-joinable-networks"></a>Consulta de redes puede unir

Aunque contactos es una excelente manera de conectar los jugadores rápidamente, a veces es mejor permitir que los jugadores a detectar redes joinable con criterios de búsqueda personalizada y seleccione la red que desean unirse a. Esto puede ser muy útil cuando una sesión de juego podría tener un gran conjunto de reglas configurables de juegos y las preferencias del Reproductor. Para ello, una red existente en primer lugar se debe realizar consultable habilitando `XimAllowedPlayerJoins.Queried` joinability y configuración de la información de red disponible para otros usuarios fuera de la red mediante una llamada a `XboxIntegratedMultiplayer.SetNetworkConfiguration()`.

En el ejemplo siguiente se habilita `XimAllowedPlayerJoins.Queried` joinability, red de conjuntos de configuración con una configuración de equipo que permite un total de 1-8 jugadores juntos en 1 equipo, un uint64_t constante de modo de juego específicos de la aplicación definida por el valor GAME_MODE_BRAWL, una descripción "gato y conversión boxing oveja coincide con", un uint32_t constante de índice de asignación específicos de la aplicación definida por el valor MAP_KITCHEN e incluye etiquetas "chatrequired", "fácil", "spectatorallowed":

```cs
string[] tags = { "chatrequired", "easy", "spectatorallowed" };
var networkConfiguration = new XimNetworkConfiguration(XboxIntegratedMultiplayer.NetworkConfiguration);
networkConfiguration.AllowedPlayerJoins |= XimAllowedPlayerJoins.Queried;
networkConfiguration.TeamConfiguration.TeamCount = 1;
networkConfiguration.TeamConfiguration.MinPlayerCountPerTeam = 1;
networkConfiguration.TeamConfiguration.MaxPlayerCountPerTeam = 8;
networkConfiguration.CustomGameMode = GAME_MODE_BRAWL;
networkConfiguration.Description = "Cat and sheep's boxing match";
networkConfiguration.MapIndex = MAP_KITCHEN;
networkConfiguration.SetTags(tags);
XboxIntegratedMultiplayer.SetNetworkConfiguration(networkConfiguration);
```

Otros jugadores fuera de la red, a continuación, pueden encontrar la red mediante una llamada a xim::start_joinable_network_query() con un conjunto de filtros que coinciden con la información de red en la llamada xim::set_network_configuration() anterior. El ejemplo siguiente inicia una consulta de la red puede unir con la opción de filtro de modo de juego que consultará sólo para redes que utilizan el modo de juego específicos de la aplicación definido por el valor GAME_MODE_BRAWL:

```cs
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Este es otro ejemplo que usa la opción de filtros de etiqueta para consultar las redes que tienen la etiqueta "fácil" y "spectatorallowed" en su configuración consultable pública:

```cs
string[] tagFilters = { "easy", "spectatorallowed" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

También se pueden combinar las opciones de filtro diferentes. El siguiente ejemplo que usa la opción de filtro de modo de juego y la etiqueta de opción para iniciar una consulta para las redes que tienen la constante de modo de juego específicos de la aplicación GAME_MODE_BRAWL y la etiqueta "fácil" de filtro:

```cs
string[] tagFilters = { "easy" };
XimJoinableNetworkQueryFilters queryFilters = new XimJoinableNetworkQueryFilters();
queryFilters.CustomGameModeFilter = GAME_MODE_BRAWL;
queryFilters.SetTagFilters(tagFilters);
XboxIntegratedMultiplayer.StartJoinableNetworkQuery(queryFilters);
```

Si la operación de consulta se realiza correctamente, la aplicación recibirá una xim_start_joinable_network_query_completed_state_change desde el que la aplicación puede recuperar una lista de redes puede unir. La aplicación recibirá también continuamente `XimJoinableNetworkUpdatedStateChange` para redes joinable adicionales o los cambios que se producen en la lista de redes puede unir hasta que se detenga de forma manual o automática. Se puede detener manualmente la consulta en curso mediante una llamada a `XboxIntegratedMultiplayer.StopJoinableNetworkQuery()`. Se detiene automáticamente cuando se llama a `XboxIntegratedMultiplayer.StartJoinableNetworkQuery()` para iniciar una nueva consulta.

La aplicación puede intentar unirse a una red en la lista de redes puede unir mediante una llamada a `XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation()`. En el siguiente ejemplo se da por supuesto que está intentando unirse a una red al que hace referencia 'selectedNetwork' no está protegido mediante un código de acceso (por lo que estamos pasando una cadena vacía para el segundo parámetro):

```cs
XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation(selectedNetwork, "");
```

Al habilitar la consulta de la red con un `XimNetworkConfiguration.TeamConfiguration` que declara dos o más equipos, los jugadores Unidos mediante una llamada a voluntad XboxIntegratedMultiplayer.MoveToNetworkUsingJoinableNetworkInformation() tienen un valor de índice del equipo predeterminado de 0.

> [!NOTE]
> Si la aplicación ha especificado varias usuarios locales y se une a una red tiene menos espacio que el número de usuarios locales, la combinación se puede autenticar correctamente. Pero solo el número permitido de usuarios locales puede unirse a la red.