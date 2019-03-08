---
title: Uso de XIM (C++)
description: Obtenga información sobre cómo usar Xbox integrado participan varios jugadores (XIM) con C++.
ms.date: 04/24/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, xbox uno, varios jugadores integrada de xbox
ms.localizationpriority: medium
ms.openlocfilehash: 798d1a1bc738cbdc7bb2b3fb34076f0897fc76ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644060"
---
# <a name="using-xim-c"></a>Uso de XIM (C++)

> [!div class="op_single_selector" title1="Language"]
> - [C++](using-xim.md)
> - [C#](using-xim-cs.md)

Se trata de un breve tutorial sobre el uso de API de C++ de XIM. Los desarrolladores que desean tener acceso a XIM a través de juegos C# debería ver [utilizando XIM (C#)](using-xim-cs.md).

- [Uso de XIM (C++)](#using-xim-c)
    - [Requisitos previos](#prerequisites)
    - [Inicialización e inicio](#initialization-and-startup)
    - [Las operaciones asincrónicas y cambios de estado de procesamiento](#asynchronous-operations-and-processing-state-changes)
    - [Objetos de control de XIM Reproductor](#handling-xim-player-objects)
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

Compilación XIM requiere incluida la principal `XboxIntegratedMultiplayer.h` encabezado. Para vincular correctamente, también debe incluir el proyecto `XboxIntegratedMultiplayerImpl.h` en unidad de compilación al menos un (se recomienda un encabezado precompilado común ya que estas implementaciones de la función de código auxiliar son pequeño y fácil para que el compilador genere como "inline").

Como se mencionó en el [XIM Introducción](../xbox-integrated-multiplayer.md), la interfaz XIM no requiere un proyecto para elegir entre compilar con C++ / c++ / CX y C++ tradicionales; se puede usar con cualquiera. La implementación no produce excepciones como medio de error no irrecuperable reporting por lo que puede consumir fácilmente desde proyectos sin excepciones, si lo prefiere.

## <a name="initialization-and-startup"></a>Inicialización e inicio

Empezar a interactuar con la biblioteca al inicializar el singleton del objeto XIM con su Xbox Live servicio configuración título y la cadena de Id. número de ID. Si también usa la API de servicios de Xbox, es posible que resulte conveniente recuperar estos de `xbox::services::xbox_live_app_config`. En el siguiente ejemplo se da por supuesto que los valores ya residen en las variables 'myServiceConfigurationId' y 'myTitleId' respectivamente:

```cpp
xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
```

Una vez inicializado, la aplicación debe recuperar las cadenas de identificador de usuario de Xbox para todos los usuarios actualmente en el dispositivo local que participan y pasarlos a un `xim::set_intended_local_xbox_user_ids()` llamar. El código de ejemplo siguiente se da por supuesto que un usuario ha presionado un botón del dispositivo de expresar la intención para reproducir y ya se ha recuperado la cadena de Id. de usuario de Xbox asociada con el usuario en una variable 'myXuid':

```cpp
xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
```

Una llamada a `xim::set_intended_local_xbox_user_ids()` establece inmediatamente los identificadores de usuario de Xbox asociado a los usuarios locales que se deben agregar a la red XIM. Esta lista de identificadores de usuario de Xbox que se usará en todas las operaciones de la futura red hasta que cambie la lista a través de otra llamada a `xim::set_intended_local_xbox_user_ids()`.

En el caso donde no hay ninguna red XIM nada todavía, el primer paso es mover a una red XIM para que el proceso iniciado. Si el usuario no tiene ya una red XIM específica en mente, el procedimiento recomendado consiste en mover simplemente a una red nueva y vacía. Esto permite que sus amigos del usuario para unirse a la red como una especie de "introductoria" desde el que pueden colaborar para seleccionar su actividad multijugador siguiente (por ejemplo, introducir los contactos).

El siguiente es un ejemplo de cómo mover los usuarios locales se agregó anteriormente con `xim::set_intended_local_xbox_user_ids()` a una red XIM vacía con espacio para un total de hasta 8 reproductores:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_only_local_players);
```

La llamada a `xim::move_to_new_network()` inicia asincrónico la operación de mover que concluye con un evento de cambio de estado que los desarrolladores deben procesar periódicamente para.

## <a name="asynchronous-operations-and-processing-state-changes"></a>Las operaciones asincrónicas y cambios de estado de procesamiento

El corazón de XIM es la aplicación regular, frecuentes llamadas a la `xim::start_processing_state_changes()` y `xim::finish_processing_state_changes()` par de métodos. Estos métodos son cómo XIM se informa de que la aplicación está preparada para controlar las actualizaciones de estado de varios jugadores y cómo XIM proporciona esas actualizaciones. Están diseñados para funcionar rápidamente, que se les pueden llamar cada fotograma de gráficos en el bucle de representación de la interfaz de usuario. Esto proporciona un lugar conveniente para recuperar todos los cambios de la cola sin preocuparse por la imprecisión de intervalos de red o la complejidad de la devolución de llamada multiproceso. La API XIM está optimizada para este patrón de un único subproceso. Garantiza que su estado permanecerá sin cambios fuera de estas dos funciones, por lo que puede usar directamente y eficaz.

Para la misma razón, todos los objetos devueltos por la API XIM deben *no* se consideran seguras para subprocesos. La biblioteca tiene protección multithreading interno, pero todavía deberá implementar su propio bloqueo si necesita un subproceso arbitrario para tener acceso a cualquiera de los valores de XIM. Por ejemplo, en el caso de tener un subproceso recorrer el `xim::players()` lista mientras otro subproceso podría invocar cualquiera `xim::start_processing_state_changes()` o `xim::finish_processing_state_changes()` y modificación de la memoria asociada con esa lista del Reproductor.

Cuando `xim::start_processing_state_changes()` es llamado, se notifican en una matriz de todas las actualizaciones en cola `xim_state_change` estructura punteros.

Las aplicaciones deben:

1. Recorre en iteración la matriz.
1. Inspeccionar la estructura de base de su tipo más específico.
1. Convertir el tipo de estructura base correspondiente más detallada de tipo.
1. Controlar esa actualización según corresponda.

Una vez finalizado con todos los `xim_state_change` estructuras disponibles actualmente, se debe pasar esa matriz a XIM para liberar los recursos mediante una llamada a `xim::finish_processing_state_changes()`. Por ejemplo:

```cpp
uint32_t stateChangeCount;
xim_state_change_array stateChanges;
xim::singleton_instance().start_processing_state_changes(&stateChangeCount, &stateChanges);
for (uint32_t stateChangeIndex = 0; stateChangeIndex < stateChangeCount; stateChangeIndex++)
{
   const xim_state_change * stateChange = stateChanges[stateChangeIndex];
   switch (stateChange->state_change_type)
   {
       case xim_state_change_type::player_joined:
       {
           MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change*>(stateChange));
           break;
        }

       case xim_state_change_type::player_left:
       {
           MyHandlePlayerLeft(static_cast<const xim_player_left_state_change*>(stateChange));
           break;
       }

       ...
    }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Ahora que tiene el bucle de procesamiento básico, puede controlar los cambios de estado asociados con la inicial `xim::move_to_new_network()` operación. Cada operación de movimiento de red XIM comenzará con una `xim_move_to_network_starting_state_change`. Si el movimiento falla por alguna razón, la aplicación se proporcionará un `xim_network_exited_state_change`, que es el mecanismo para cualquier error asincrónico que no le permitirá mover a una red XIM o lo desconecta de la red XIM actual de control de error comunes. En caso contrario, el movimiento se completará con un `xim_move_to_network_succeeded_state_change` después de que todos los Estados se ha finalizado y todos los jugadores se han agregado correctamente a la red XIM.

Cuando un usuario no tiene el privilegio de plataforma para reproducir con otros usuarios en una sesión de varios jugadores, XIM enviará a un dispositivo al intentar crear o unirse a una red XIM un `xim_network_exited_state_change` con el motivo `xim_network_exit_reason::user_multiplayer_restricted`. Esto sucede cuando se establezca cualquiera de los usuarios locales con `xim::set_intended_local_xbox_user_ids()` tiene una restricción de varios jugadores de Xbox Live. O bien deben llamar aplicaciones de Xbox ERA con XIM `Store::Product::CheckPrivilegeAsync` reproductores local antes de intentar moverlos a una red XIM con attemptResolution establecido en true, o hacerlo en respuesta a la recepción `xim_network_exit_reason::user_multiplayer_restricted` como un motivo de un `xim_network_exited_state_change`.

## <a name="handling-xim-player-objects"></a>Objetos de control de XIM Reproductor

Suponiendo que el ejemplo de cómo mover un único usuario local a una nueva red XIM se realizó correctamente, la aplicación, se proporcionará un `xim_player_joined_state_change` para una variable local `xim_player` objeto. Este puntero de objeto seguirá siendo válido para siempre y cuando la propia instancia del Reproductor es válida. Instancia del encargado de deja de ser válida cuando la correspondiente `xim_player_left_state_change` para que el Reproductor ha devuelto mediante una llamada a la instancia `xim::finish_processing_state_changes()`. La aplicación siempre se proporcionará un `xim_player_left_state_change` para cada `xim_player_joined_state_change`.

También puede recuperar una matriz de todos los `xim_player` objetos en el XIM de red en cualquier momento mediante el uso de `xim::get_players()`.

El `xim_player` objeto tiene muchos métodos útiles, como `xim_player::gamertag()` para recuperar la cadena de nombre de jugador de Xbox Live actual asociado con el Reproductor para fines de presentación. Si el `xim_player` es local en el dispositivo, entonces también notificará que no sean null `xim_player::xim_local` puntero de objeto de `xim_player::local()`, que tiene métodos adicionales disponibles solo a los jugadores locales.

Por supuesto, el estado más importante de los jugadores no es la información común que XIM sabe, pero lo que la aplicación específica que desee realizar un seguimiento. Puesto que es probable que tenga su propia construcción para esa información de seguimiento, conviene vincular el `xim_player` objeto a su propio objeto de construcción del Reproductor para que cada vez que los informes XIM un `xim_player`, puede recuperar rápidamente el estado sin tener que rellenar y buscar un puntero de contexto de Reproductor personalizado. En el siguiente ejemplo se da por supuesto que es un puntero a su estado privado en la variable 'myPlayerStateObject' y se ha agregado recientemente `xim_player` objeto está en la variable 'newXimPlayer':

```cpp
newXimPlayer->set_custom_player_context(myPlayerStateObject);
```

Esto guarda el valor de puntero especificado con el objeto player localmente (nunca transferidos a través de la red a los dispositivos remotos donde la memoria no sería válida). A continuación, podrá siempre regresar a su objeto recuperando el contexto personalizado y convertirlo a su objeto similar al ejemplo siguiente:

```cpp
myPlayerStateObject = reinterpret_cast<MyPlayerState *>(newXimPlayer->custom_player_context());
```

Puede cambiar el puntero de contexto de Reproductor personalizado asignado a un `xim_player` en cualquier momento.

## <a name="enabling-friends-to-join-and-inviting-them"></a>Habilitación de sus amigos a participar y que se les invitará

Privacidad y seguridad, de todas las redes XIM nuevo se configuran automáticamente de manera predeterminada sólo se puede unir reproductores locales y depende de la aplicación para permitir explícitamente que otros usuarios una vez que esté listo. El ejemplo siguiente muestra cómo usar `xim::network_configuration()` para recuperar la configuración de red actual y actualizar mediante joinability `xim::set_network_configuration()` para empezar a permitir que los nuevos usuarios locales a unirse como reproductores, así como otros usuarios que han sido invitados o que se va a " seguido de"(una Xbox Live sociales relación de) los jugadores ya está en la red XIM:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance.network_configuration();
networkConfiguration.allowed_player_joins = xim_allowed_player_joins::local | xim_allowed_player_joins::invited | xim_allowed_player_joins::followed;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

`xim::set_network_configuration()` ejecuta asynchronoulsy. Una vez que se complete la llamada de ejemplo de código anterior, un `xim_network_configuration_changed_state_change` se proporciona para notificar a la aplicación que ha cambiado el valor joinability a su valor predeterminado de `xim_allowed_player_joins::none`. A continuación, puede consultar el nuevo valor activando el `allowed_player_joins` propiedad de la `xim_network_configuration` devuelto por `xim::network_configuration()`. 

El `allowed_player_joins` se puede comprobar mientras el dispositivo está en una red XIM para determinar el joinability de la red.

Debe uno de los jugadores locales desea enviar invitaciones a los usuarios remotos a incorporarse a esta red XIM, la aplicación puede llamar a `xim_player::xim_local::show_invite_ui()` para iniciar la interfaz de usuario de invitación del sistema. En este caso, el usuario local puede seleccionar las personas que desean invitar y enviar invitaciones. El ejemplo siguiente se muestra cómo funciona y se da por supuesto que la variable 'ximPlayer' señala a una variable local válida `xim_player`:

```cpp
ximPlayer->local()->show_invite_ui();
```

Una vez ejecutada la instrucción anterior, se mostrará la interfaz de usuario la invitación del sistema. Una vez que el usuario ha enviado las invitaciones (o en caso contrario, se descarta la interfaz de usuario), un `xim_show_invite_ui_completed_state_change` se proporcionará en la siguiente llamada a `xim::start_processing_state_changes()`. Como alternativa, la aplicación puede enviar las invitaciones directamente mediante `xim_player::xim_local::invite_users()` sin que se muestre la interfaz de usuario de invitación del sistema.

En cualquier caso, los usuarios remotos recibirán mensajes de invitación de Xbox Live siempre que haya iniciado sesión, que puede elegir Aceptar o rechazar. Si los acepta, una activación de protocolos iniciará la aplicación en dichos dispositivos si no se está ejecutando y la activación de protocolo se enviarán a la aplicación con sus argumentos de evento que se puede usar para mover a la red XIM correspondiente.

Consulte la documentación de plataforma para obtener más información sobre la activación de protocolo.

El ejemplo siguiente muestra cómo una llamada a `xim::extract_protocol_activation_information()` determina si los argumentos de evento son aplicables a XIM. Esto se da por supuesto que ha recuperado la cadena sin formato del URI de `Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs` a una variable 'uriString':

```cpp
xim_protocol_activation_information activationInfo;
bool isXimActivation;
isXimActivation = xim::singleton_instance().extract_protocol_activation_information(uriString, &activationInfo);
```

Si es una activación XIM, desea asegurarse de que el usuario local identificado en el campo 'local_xbox_user_id' de ha llenado `xim_protocol_activation_information` estructura se ha iniciado sesión y se encuentra entre los usuarios especificados a `xim::set_intended_local_xbox_user_ids()`. A continuación, puede iniciar mover a la red XIM especificada con una llamada a `xim::move_to_network_using_protocol_activated_event_args()` utilizando la misma cadena URI. Por ejemplo:

```cpp
xim::singleton_instance().move_to_network_using_protocol_activated_event_args(uriString);
```

También tenga en cuenta que los usuarios remotos "seguir" puede navegar a la tarjeta del Reproductor del usuario local en el sistema de la interfaz de usuario e iniciar un intento de unir a sí mismos sin invitación (suponiendo que ha permitido dichas combinaciones Reproductor como se muestra arriba). Esta acción también se protocolo activar la aplicación tal como invitaciones y se controlan de la misma manera.

Mover a una red XIM mediante la activación de protocolos es idéntico a moverse a una nueva red XIM tal como se muestra en [inicio y la inicialización](#initialization-and-startup). La única diferencia es que cuando el movimiento se realiza correctamente, el dispositivo móvil se proporcionará el Reproductor local y remoto `xim_player_joined_state_change` estructuras que representan todos los jugadores aplicable. Naturalmente, no se puede mover el dispositivo original que ya estaba en la red XIM, pero verá que los usuarios del dispositivo nuevo se agrega como jugadores a través de otras `xim_player_joined_state_change` estructuras.

Una vez que se unen los jugadores en diferentes dispositivos en una red XIM, pueden iniciar escenarios multijugador, automáticamente se comunican a través de voz y texto y enviar mensajes específicos de la aplicación.

## <a name="basic-matchmaking-and-moving-to-another-xim-network-with-others"></a>Contactos básica y mover a otra red XIM con otros usuarios

Puede expandir la experiencia de red para un grupo de amigos moviendo los jugadores a una red XIM que también tenga extraños. Extraños son oponentes de todo el mundo que se incluyan entre sí mediante según los mismos intereses el servicio Xbox Live.

Es una manera sencilla de iniciar contactos básica con XIM mediante una llamada a `xim::move_to_network_using_matchmaking()` en uno de los dispositivos con un rellenado `xim_matchmaking_configuration` estructura, teniendo encargados de la red XIM actual junto con ella.

El ejemplo siguiente inicia un movimiento mediante una configuración de contactos que se configura para encontrar un total de 8 jugadores una coincidencia de fiesta inolvidable no equipos. Si no se encuentran 8 jugadores en total, el sistema podría coincidir los reproductores de 2 a 7 juntos. Este ejemplo utiliza una constante definida en la aplicación, de tipo uint64_t, denominado MYGAMEMODE_DEATHMATCH, que representa el modo de juego para filtrar fuera de. Contactos del XIM sólo coincidirá con esta red con otros jugadores especificando el mismo valor y lleva a lo largo de todo social unido reproductores desde la red XIM actual al proporcionar el segundo parámetro `xim_players_to_move::bring_existing_social_players` a `xim::move_to_network_using_matchmaking()`:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Similar a movimientos anteriores, este proceso contactos proporcionará un inicial `xim_move_to_network_starting_state_change` en todos los dispositivos y un `xim_move_to_network_succeeded_state_change` una vez finalizado el movimiento. Puesto que esto es un movimiento de una red XIM a otra, una diferencia es que ya existen `xim_player` agregado objetos para los usuarios locales y remotos, y seguirán estando en todos los jugadores que pasan a la nueva red XIM juntos. Comunicación de chat y los datos entre los jugadores seguirán funcionando sin interrupciones mientras contactos está en curso.

Contactos pueden ser un proceso largo según el número de jugadores potenciales en el grupo de contactos que también han llamado `xim::move_to_network_using_matchmaking()`.

Un `xim_matchmaking_progress_updated_state_change` se proporcionará periódicamente a lo largo de la operación para que usted y a los usuarios informados del estado actual. Cuando se ha encontrado la coincidencia, los participantes adicionales se agregan a la red XIM con el típico `xim_player_joined_state_change` y se completa el movimiento.

Una vez haya terminado la experiencia de varios jugadores con este conjunto de jugadores "matchmade", puede repetir el proceso de mover a otra red XIM con otra ronda de contactos. Verá cada jugador que combinen mediante previo `xim::move_to_network_using_matchmaking()` operación proporcionan un `xim_player_left_state_change` para indicar que su `xim_player` objetos ya no están en la misma red XIM.

Si cuando decide iniciar contactos utilizando de nuevo `xim::move_to_network_using_matchmaking()`, especifique `xim_players_to_move::bring_existing_social_players`, los jugadores que combinen mediante puntos de entrada social (`xim::move_to_network_using_protocol_activated_event_args()` o `xim::move_to_network_using_joinable_xbox_user_id()`) permanecerá mientras lleva a los nuevos contactos. También puede especificar `xim_players_to_move::bring_only_local_players` desconectará social conectados reproductores remotos, dejando sólo los jugadores locales para que sigan siendo. En ambos casos, se agregará un conjunto diferente de extraños cuando se completa la segunda operación de movimiento.

También tiene la opción de mover los jugadores no matchmade (o reproductores solo locales) a una red XIM completamente nueva al decidir si la siguiente actividad multijugador/configuración de contactos. El ejemplo siguiente muestra cómo podría llamar un dispositivo `xim::move_to_new_network()` para una red XIM con un máximo de 8 jugadores, pero esta vez teniendo el social unido los reproductores existentes, así:

```cpp
xim::singleton_instance().move_to_new_network(8, xim_players_to_move::bring_existing_social_players);
```

Un `xim_move_to_network_starting_state_change` y `xim_move_to_network_succeeded_state_change` se proporcionará a todos los dispositivos que participan, junto con un `xim_player_left_state_change` para los jugadores matchmade que quedaron. En dichos dispositivos, del mismo modo verá un `xim_player_left_state_change` para cada jugador que se mueve fuera de la red.

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

Cuando se realizan los usuarios locales que participan en una red XIM, a menudo que simplemente moverá a una nueva red XIM que permite a los usuarios locales, usuarios invitados y "seguido de" los usuarios se unan para que puedan coordinar con sus amigos en la búsqueda de la siguiente actividad. Pero si el usuario se realiza por completo con todas las experiencias multijugador, la aplicación que desee empezar dejando la XIM de red por completo y volver al estado como si solo `xim::initialize()` y `xim::set_intended_local_xbox_user_ids()` hubiera llamado. Esto se realiza mediante el `xim::leave_network()` método:

```cpp
xim::singleton_instance().leave_network();
```

Este método comienza el proceso de forma asincrónica desconectando los demás participantes correctamente. Esto hará que los dispositivos remotos que se proporcione un `xim_player_left_state_change` para cada jugador local que desconectado. El dispositivo local se también se proporcionará un `xim_player_left_state_change` para cada jugador local y remoto que se ha desconectado. Cuando se desconectan todas las operaciones han finalizado, un final `xim_network_exited_state_change` se proporcionará. A continuación, puede llamar la aplicación ' xim::cleanup()'' para liberar todos los recursos y volver al estado sin inicializar:

```cpp
xim::singleton_instance().cleanup();
```

Invocar `xim::leave_network()` y esperando la `xim_network_exited_state_change` para salir de un XIM red correctamente siempre recomienda cuándo un `xim_network_exited_state_change` no se ha proporcionado.

Si `xim::cleanup()` se denomina directamente sin `xim::leave_network()` llama, a continuación, se producirán problemas de rendimiento de comunicación para los demás participantes desde que se verán obligados a enviar mensajes sin entregar al dispositivo que simplemente "desapareció" una llamada a `xim::cleanup()`.

## <a name="sending-and-receiving-messages"></a>Enviar y recibir mensajes

XIM y sus componentes subyacentes hacen todo el trabajo tedioso de establecimiento de los canales de comunicación segura a través de Internet para que no tenga que preocuparse sobre problemas de conectividad o pueden llegar a algunos pero no todos los jugadores. Si hay algún problema de conectividad de punto a punto fundamental, mover a una red XIM no se realizará correctamente.

Cuando está formada y confirme con una red XIM `xim_move_to_network_succeeded_state_change`, se garantiza que todas las instancias de la aplicación entre todos los dispositivos Unidos para mantenerse informado sobre cada `xim_player` conectado a la red XIM y puede enviar mensajes a cualquiera de ellas.

Vamos a demostrar cómo enviar mensajes a través de la red XIM. En el siguiente ejemplo se da por supuesto que la variable 'sendingPlayer' es un puntero a un objeto player local válido. invoca 'sendingPlayer' `local()` y llama a `send_data_to_other_players()` para enviar a la estructura del mensaje 'msgData' a todos los jugadores (locales y remotos) con entrega garantizada y secuencial. Tenga en cuenta que va a todos los reproductores porque una lista de jugadores específicos no se pasó al método:

```cpp
sendingPlayer->local()->send_data_to_other_players(sizeof(msgData), &msgData, 0, nullptr, xim_send_type::guaranteed_and_sequential);
```

Se pueden entregar mensajes a todos los reproductores con `send_data_to_other_players()` al no pasar una matriz de reproductores específicos al método.

Todos los destinatarios del mensaje se proporcionan un `xim_player_to_player_data_received_state_change` que incluye un puntero a una copia de los datos, así como punteros a la correspondiente xim_player objeto que envían y reciben localmente.

Por supuesto, es conveniente entrega garantizada, secuencial, pero también puede ser un tipo de envío ineficaz, ya que XIM debe retransmitir o retrasar los mensajes si los paquetes se quiten o se están ordenados por Internet. Asegúrese de considerar el uso de los otros tipos de envío para que la aplicación puede tolerar perder o tener los mensajes llegan sin orden. Puesto que los datos del mensaje proceden de un equipo remoto, el procedimiento recomendado es tener claramente definidos los formatos de datos, como el empaquetado de los valores de varios bytes en un orden determinado byte ("modos endian"). La aplicación también debe validar los datos antes de actuar en él.

XIM proporciona seguridad de red, por lo que no es necesario para las combinaciones de cifrado o firma adicionales. Sin embargo, siempre se recomienda emplear "defensa en profundidad" con el fin de protegerse frente a errores de la aplicación accidental y ser capaz de controlar distintas versiones de la coexistencia de protocolo de aplicación correctamente (durante el desarrollo, las actualizaciones de contenido, etcetera.). Conexiones de Internet de los usuarios pueden ser recursos limitados y cambiantes. Se recomienda encarecidamente el uso de formatos de datos de mensajes eficiente y evitar diseños que envían cada fotograma de la interfaz de usuario.

## <a name="assessing-data-pathway-quality"></a>Evaluación de la calidad de la ruta de datos

Para obtener información sobre la calidad de la ruta de datos entre dos jugadores actual, llame a la `xim_player::network_path_information()` método. El ejemplo siguiente recupera un puntero a la `xim_network_path_information` de estructura para un `xim_player` puntero incluido en la variable 'remotePlayer':

```cpp
 const xim_network_path_information * networkPathInfo = remotePlayer->network_path_information();
```

La estructura devuelta incluye la latencia de ida y vuelta estimado y cuántos mensajes se siguen en la cola local para la transmisión en el caso de que la conexión no admite la transmisión de más datos para ese momento.

Si ve que las colas de una copia de seguridad para un determinado 'xim_player', debe reducir la velocidad a la que va a enviar datos.

El `xim_network_path_information::round_trip_latency_in_milliseconds` campo representa la latencia de la red subyacente y la latencia estimada del XIM sin puesta en cola. Los aumentos de latencia efectiva como `xim_network_path_information::send_queue_size_in_messages` crece y XIM funciona a través de la cola.

Elija un punto razonable para iniciar la limitación de las llamadas a `send_data_to_other_players` según los requisitos y el uso del juego. Todos los mensajes de la cola de envío representa un aumento de la latencia de red eficaz.

Un valor que se aproxima el límite máximo del XIM (actualmente 3500 mensajes) es demasiado alto para la mayoría de los juegos y es probable que representa varios segundos de datos en espera de envío según la velocidad de la llamada a `send_data_to_other_players` y es el tamaño de cada carga de datos. En su lugar, elija un número que tiene en cuenta los requisitos de latencia del juego junto con cómo nerviosos juego `send_data_to_other_players` es el patrón de llamada.

## <a name="sharing-data-using-player-custom-properties"></a>Uso compartido de datos mediante las propiedades personalizadas del Reproductor

Mayoría intercambios de datos de aplicación que se producen con el `xim_player::xim_local::send_data_to_other_players()` método, ya que permite el máximo control sobre quién recibe datos, al que deben recibir esos datos y cómo debe tratar el sistema de pérdida de paquetes. Sin embargo, hay veces cuando sería bueno para reproductores compartir el estado básico, casi no presentan cambio sobre sí mismos con otras personas sin mayores inconvenientes. Por ejemplo, cada jugador podría tener una cadena fija que representa el modelo de caracteres seleccionado antes de entrar en varios jugadores que todos los reproductores de usan para representar su representación en el juego.

Para los datos que no cambian muy a menudo sobre un Reproductor, XIM proporciona a Reproductor propiedades personalizadas. Estas propiedades constan de un nombre definido por la aplicación y un valor, que son pares de cadena terminada en null que se puede aplicar al Reproductor local y que se propagan automáticamente a todos los dispositivos cada vez que se cambian.

Las propiedades de Reproductor personalizado tienen sincronización interna más sobrecarga que el `xim_player::xim_local::send_data_to_other_players()` método, por lo que para cambiar rápidamente los datos (es decir, la posición del jugador), debería usar aún envía directa.

Pares de clave-valor de propiedades personalizadas de Reproductor tienen sus valores actuales que se proporciona automáticamente a nuevos dispositivos participantes al unirse a una red XIM y vea el Reproductor agregarlo estos dispositivos. Se pueden establecer valores mediante una llamada a `xim_player::xim_local::set_player_custom_property()` con cadenas de nombre y valor. En el ejemplo siguiente, se establece una propiedad denominada "model" para que el valor "fuerza bruta" en una variable local `xim_player` objeto apunta a la variable 'localPlayer':

```cpp
localPlayer->local()->set_player_custom_property(L"model", L"brute");
```

Los cambios en las propiedades del Reproductor provocará un `xim_player_custom_properties_changed_state_change` proporcionarse a todos los dispositivos, les alertan a los nombres de propiedades que han cambiado. El valor de un nombre determinado se puede recuperar en cualquier reproductor, local o remoto, con `xim_player::get_player_custom_property()`. El ejemplo siguiente recupera el valor para una propiedad denominada "model" de un `xim_player` apunta a la variable 'ximPlayer':

```cpp
PCWSTR modelName = ximPlayer->get_player_custom_property(L"model");
```

Establecer un nuevo valor para un nombre de propiedad dado, se reemplazará cualquier valor existente. Establecer un nombre de propiedad en un valor de un puntero de cadena del valor null se trata igual que una cadena de un valor vacío, que es el mismo que la propiedad no tener se ha especificado aún. Los nombres y valores no se interpretan XIM, por lo tanto, se deja en la aplicación para validar el contenido de cadena según sea necesario.

Las propiedades de Reproductor personalizado siempre se restablecen al pasar de una red XIM a otro. Sin embargo, los nuevos jugadores que se unen a la red XIM recibirán propiedades actuales de Reproductor personalizado para todos los jugadores que tienen un conjunto.

## <a name="sharing-data-using-network-custom-properties"></a>Uso compartido de datos mediante las propiedades personalizadas de red

Propiedades personalizadas de red están diseñadas para su comodidad sincronizar datos específicos de la red XIM que no cambia con frecuencia. Trabajo de las propiedades personalizadas de red de forma idéntica a [propiedades personalizadas del Reproductor](#sharing-data-using-player-custom-properties), excepto en están establecidas en el objeto singleton XIM con `xim::set_network_custom_property()`. El ejemplo siguiente establece una propiedad de "mapa" para que el valor "stronghold":

```cpp
xim::singleton_instance().set_network_custom_property(L"map", L"stronghold");
```

Los cambios en las propiedades de red provocará un `xim_network_custom_properties_changed_state_change` proporcionarse a todos los dispositivos, les alertan a los nombres de propiedades que han cambiado. El valor de un nombre determinado se puede recuperar con `xim::get_network_custom_property()`, como en el ejemplo siguiente, que recupera el valor de una propiedad con nombre "asignación":

```cpp
PCWSTR mapName = xim::singleton_instance().get_network_custom_property(L"map");
```

Al igual que [propiedades personalizadas del Reproductor](#sharing-data-using-player-custom-properties), establecer un valor nuevo para un nombre de propiedad dado reemplazará cualquier valor existente. Establecer un nombre de propiedad en un valor de un puntero de cadena del valor null se trata igual que una cadena de un valor vacío, que es el mismo que la propiedad no tener se ha especificado aún. Los nombres y valores no se interpretan XIM, por lo tanto, se deja en la aplicación para validar el contenido de cadena según sea necesario.

Recién creado XIM redes siempre empiezan con ningún conjunto de propiedades personalizadas de la red. Sin embargo, los nuevos jugadores que se unen a una red existente de XIM recibirán los valores actuales de las propiedades personalizadas de red establecida para esa red XIM.

## <a name="matchmaking-using-per-player-skill"></a>Uso por Reproductor aptitud de contactos

Búsqueda de coincidencias reproductores interés común en un determinado modo de juego especificado a la aplicación es una buena estrategia de base. A medida que crece el grupo de jugadores disponibles, considere la posibilidad de coincidencia también reproductores según sus conocimientos o experiencia con su juego para que los jugadores veteranos pueden disfrutar el desafío de sana competición con otros veteranos, mientras que los reproductores más recientes pueden crecer por compite con otros usuarios con funcionalidades similares.

Para ello, inicie proporcionando el nivel de aptitud para todas las compañías locales en sus roles de por el Reproductor y la estructura de configuración de aptitud especificadas en llamadas a `xim_player::xim_local::set_roles_and_skill_configuration()` antes de comenzar a mover a un XIM de red con contactos. Nivel de dificultad es un concepto específico de la aplicación y el número no interpreta XIM, excepto en que se primero intenta encontrar los reproductores con el mismo valor de cualificación contactos y, a continuación, periódicamente ampliar su búsqueda en incrementos de +/-10 para intentar encontrar otros jugadores declarar habilidades valores dentro de un intervalo en torno a dicha aptitud. En el siguiente ejemplo se da por supuesto que el equipo local `xim_player` objeto, cuyo puntero se encuentra 'localPlayer', tiene un valor de cualificación de asociado uint32_t específico de la aplicación recuperado de almacenamiento local o Xbox Live en una variable denominada 'playerSkillValue':

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.skill = playerSkillValue;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Cuando se complete, todos los participantes se proporcionará un `xim_player_roles_and_skill_configuration_changed_state_change` que indica esto `xim_player` ha cambiado su roles por Reproductor y la configuración de la aptitud. El nuevo valor se puede recuperar mediante una llamada a `xim_player::roles_and_skill_configuration()`. Cuando todos los jugadores tener roles distintos de null y aplique la configuración de conocimientos, puede mover a una red XIM con contactos con un valor True para el `require_player_roles_and_skill_configuration` campo de la `xim_matchmaking_configuration` estructura especificada a `xim::move_to_network_using_matchmaking()`. En el ejemplo siguiente se rellena una configuración de contactos que encontrará un total de los jugadores del 2 al 8 para una fiesta inolvidable no equipos, mediante un uint64_t constante de modo de juego específicos de la aplicación definidas el valor MYGAMEMODE_DEATHMATCH que sólo coincidirá con otros jugadores Especifica que mismo valor y que requiere roles por Reproductor y la configuración de aptitud:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 2;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 8;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Cuando se proporciona esta estructura para `xim::move_to_network_using_matchmaking()`, la operación de movimiento se iniciará con normalidad, siempre que han llamado a reproductores mover `xim_player::xim_local::set_roles_and_skill_configuration()` con un valor no null `xim_player_roles_and_skill_configuration` puntero. Si cualquier jugador no ha hecho, a continuación, se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `xim_matchmaking_progress_updated_state_change` con un `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valor. Esto incluye los jugadores que se unen posteriormente join la red XIM a través de una invitación enviada anteriormente o a través de otros medios (por ejemplo, una llamada a `xim::move_to_network_using_joinable_xbox_user_id()`) antes de contactos han finalizado. Una vez que han proporcionado todos los jugadores sus `xim_player_roles_and_skill_configuration` estructuras, se reanudarán en contactos.

Contactos con conocimientos por el Reproductor también pueden combinarse con rol de contactos usuario por el Reproductor, como se explica en la sección siguiente. Si sólo uno es el deseado, puede especificar un valor de 0 para los demás. Esto es porque todos los reproductores de declarar tienen un `xim_player_roles_and_skill_configuration` valor aptitud 0 siempre coinciden entre sí.

Una vez el `xim::move_to_network_using_matchmaking()` o cualquier otra red XIM mover la operación ha finalizado, todos los jugadores `xim_player_roles_and_skill_configuration` estructuras se borrará automáticamente a un puntero null (con una que lo acompañan `xim_player_roles_and_skill_configuration_changed_state_change` notificación). Si va a mover a otra red XIM con contactos que requiera una configuración por el Reproductor, deberá llamar a `xim_player::xim_local::set_roles_and_skill_configuration()` nuevo con un nuevo puntero de estructura que contiene la información más actualizada.

## <a name="matchmaking-using-per-player-role"></a>Uso de rol por el Reproductor de contactos

Otro método del uso de roles por Reproductor y la configuración de aptitud para mejorar la experiencia de contactos de los usuarios es mediante el uso de roles necesarios del Reproductor. Esto se adapta mejor a los juegos que proporcionan los tipos de caracteres seleccionable que fomentan estilos diferentes play cooperativa; es decir, tipos que simplemente no alteran la representación gráfica del juego, pero puede controlan complementarios, impactantes atributos como defensivas "healers" frente a ofensa cerrar sesión "melee" frente a "range" distante atacan de soporte técnico. Personalidades usuarios significan que quizás prefiera jugar como una especialización determinada. Pero si su juego está diseñado para que funcionalmente no es posible completar los objetivos sin al menos una persona de cumplimiento de cada rol, a veces es mejor para que coincida con estos reproductores primero de que coinciden con los reproductores juntos, a continuación, necesario volver a negociar una vez recopilan los estilos de Play entre sí. Puede hacerlo mediante la primera definición de un indicador de bits única que represente cada rol que se especifique en un reproductor determinado `xim_player_roles_and_skill_configuration` estructura.

El ejemplo siguiente establece un valor específico de la aplicación MYROLEBITFLAG_HEALER uint8_t rol del objeto local xim_player, cuyo puntero se encuentra 'localPlayer':

```cpp
xim_player_roles_and_skill_configuration playerRolesAndSkillConfiguration = { 0 };
playerRolesAndSkillConfiguration.roles = MYROLEBITFLAG_HEALER;

localPlayer->local()->set_roles_and_skill_configuration(&playerRolesAndSkillConfiguration);
```

Cuando se complete, todos los participantes se proporcionará un `xim_player_roles_and_skill_configuration_changed_state_change` que indica esto `xim_player` ha cambiado su configuración de rol por el Reproductor. El nuevo valor se puede recuperar mediante una llamada a `xim_player::roles_and_skill_configuration()`.

Global `xim_matchmaking_configuration` estructura especificada a `xim::move_to_network_using_matchmaking()` deben haber todas las marcas de roles necesarios combinar con OR bit a bit y un valor true para el campo require_player_matchmaking_configuration.

El ejemplo siguiente rellena una configuración de contactos que encontrará un total de 3 reproductores para una fiesta de inolvidable no equipos. Además, en este ejemplo se utiliza una constante definida en la aplicación, que es de tipo uint64_t y denominado MYGAMEMODE_COOPERATIVE, que representa el modo de juego para filtrar fuera de. Además, la configuración está establecida para requerir roles por Reproductor y la configuración de aptitud donde al menos uno de los jugadores cumple cada roles específicos de la aplicación uint8_t que estaban OR bit a bit tenían juntas y se coloca en la configuración (MYROLEBITFLAG_HEALER, MYROLEBITFLAG_ MELEE y MYROLEBITFLAG_RANGE):

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 1;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 3;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 3;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_COOPERATIVE;
matchmakingConfiguration.required_roles = MYROLEBITFLAG_HEALER | MYROLEBITFLAG_MELEE | MYROLEBITFLAG_RANGE;
matchmakingConfiguration.require_player_roles_and_skill_configuration = true;
```

Cuando se proporciona esta estructura para `xim::move_to_network_using_matchmaking()`, se iniciará la operación de movimiento como se describió anteriormente.

Contactos con rol por el Reproductor también pueden combinarse con la habilidad de contactos usuario por el Reproductor. Si sólo uno es el deseado, especifique un valor de 0 para los demás. Esto es porque todos los reproductores de declarar tienen un `xim_player_roles_and_skill_configuration` valor aptitud 0 siempre coinciden entre sí; y, si todos los bits son cero en el `xim_matchmaking_configuration` required_roles campo, a continuación, ningún bit de rol es necesarios para que coincida.

Una vez el `xim::move_to_network_using_matchmaking()` o cualquier otra red XIM mover la operación ha finalizado, todos los jugadores `xim_player_roles_and_skill_configuration` estructuras se borrará automáticamente a un puntero null (con una que lo acompañan `xim_player_roles_and_skill_configuration_changed_state_change` notificación). Si va a mover a otra red XIM con contactos que requiera una configuración por el Reproductor, deberá llamar a `xim_player::xim_local::set_roles_and_skill_configuration()` nuevo con un nuevo puntero de estructura que contiene la información más actualizada.

## <a name="how-xim-works-with-player-teams"></a>Cómo funciona XIM con los equipos del Reproductor

Varios jugadores de juegos a menudo implica a personas organizadas en opuestos a los equipos. Hace XIM que resulte fácil asignar equipos al contactos estableciendo `xim_team_configuration`. En el ejemplo siguiente se inicia un movimiento con contactos configurada para buscar un total de 8 jugadores para colocar en dos equipos iguales de 4 (aunque si no se encuentran 4, 1-3 reproductores también son aceptables), mediante un uint64_t constante de modo de juego específicos de la aplicación definida por el valor MYGAMEMODE_CAPTURETHEFLAG que sólo coincidirá con otros jugadores especificar ese mismo valor y pone todos los jugadores unido social desde la red XIM actual:

```cpp
xim_matchmaking_configuration matchmakingConfiguration = { 0 };
matchmakingConfiguration.team_configuration.team_count = 2;
matchmakingConfiguration.team_configuration.min_player_count_per_team = 1;
matchmakingConfiguration.team_configuration.max_player_count_per_team = 4;
matchmakingConfiguration.custom_game_mode = MYGAMEMODE_CAPTURETHEFLAG;

xim::singleton_instance().move_to_network_using_matchmaking(matchmakingConfiguration, xim_players_to_move::bring_existing_social_players);
```

Cuando se completa este tipo una operación de movimiento XIM red, se asignará los encargados de un equipo valor de índice 1 a través de {n} correspondiente a los equipos de {n} solicitados. El verdadero significado de cualquier valor de índice de equipo determinado depende de la aplicación. Valor de índice del equipo de un jugador se recupera mediante `xim_player::team_index()`. Cuando se usa un `xim_team_configuration` con dos o más equipos, los jugadores nunca se asignará un valor de índice del equipo de cero por la llamada a `xim::move_to_network_using_matchmaking()`. Esto es en contraste a reproductores que se agregan a la red XIM con cualquier otra configuración o tipo de operación de movimiento (como a través de una activación de protocolos resultante de aceptar la invitación), que siempre tendrá un índice cero del equipo. Puede ser útil tratar el índice 0 de equipo como un equipo especial "sin asignar".

El ejemplo siguiente recupera el índice del equipo para un objeto xim_player cuyo puntero se encuentra en la variable 'ximPlayer':

```cpp
uint8_t playerTeamIndex = ximPlayer->team_index();
```

Para la experiencia de usuario preferidos (no a mencionar que reduce la oportunidad de comportamiento de los jugadores negativo), el servicio Xbox Live nunca será reproductores de división que se están trasladando a una red XIM juntas en distintos equipos.

El valor de índice de equipo asignado inicialmente por contactos es sólo una recomendación y la aplicación puede cambiar para reproductores locales en cualquier momento mediante `xim_player::xim_local::set_team_index()`. Esto también se puede llamar en las redes XIM que no usan en todos los contactos. El ejemplo siguiente configura un puntero de Reproductor 'localPlayer' para tener un nuevo valor de índice del equipo de uno:

```cpp
localPlayer->local()->set_team_index(1);
```

Todos los dispositivos se les informa de que el jugador tiene un nuevo valor de índice del equipo en vigor cuando les proporciona un `xim_player_team_index_changed_state_change` para que el Reproductor. Puede llamar a `xim_player::xim_local::set_team_index()` en cualquier momento.

Contactos se evalúa como los roles necesarios por Reproductor independientemente de los equipos. Por lo tanto, no se recomienda utilizar los equipos y roles necesarios como criterios de la configuración de contactos simultáneas porque los equipos se equilibrará por recuento de Reproductor, no por funciones del Reproductor que se cumple.

## <a name="working-with-chat"></a>Trabajar con chat

Comunicación de chat de voz y texto se habilitan automáticamente entre los jugadores en una red XIM. XIM controla interactuar con todo voz auriculares con micrófono el hardware y para usted. La aplicación no necesitará hacer mucho para chat, pero tiene un requisito con respecto a la charla de texto: compatibilidad con entrada y la presentación. Entrada de texto es necesario porque, incluso en plataformas o géneros de juegos que históricamente no tuvieron la generalizada teclado físico usar, reproductores pueden configurar el sistema para usar las tecnologías de asistencia texto a voz. De forma similar, la presentación del texto es necesaria porque los jugadores pueden configurar el sistema para usar texto a voz.

Estas preferencias se pueden detectar reproductores local mediante una llamada a `xim_player::xim_local::chat_text_to_speech_conversion_preference_enabled()` y `xim_player::xim_local::chat_speech_to_text_conversion_preference_enabled()`, y puede que desee habilitar condicionalmente los mecanismos de texto. Sin embargo, se recomienda que considere la posibilidad de realizar el texto de entrada y mostrar las opciones que están siempre disponibles.

 `Windows::Xbox::UI::Accessability` es una clase de Xbox One específicamente diseñada para proporcionar una representación sencilla de chat de texto en el juego con un enfoque en las tecnologías de voz a texto.

Una vez que tenga la entrada de texto que se proporcionó mediante un teclado real o virtual, pase la cadena para el `xim_player::xim_local::send_chat_text()` método. El código siguiente muestra el envío de un ejemplo de cadena codificados de forma rígida de una variable local `xim_player` objeto apunta a la variable 'localPlayer':

```cpp
localPlayer->local()->send_chat_text(L"Example chat text");
```

Este texto de la conversación se entrega a todos los jugadores en la red XIM que pueden recibir comunicación chat desde el Reproductor local original. Se podría sintetizarlo audio de voz y se podría ofrecer como un `xim_chat_text_received_state_change`.

La aplicación debe realizar una copia de cualquier cadena de texto recibido y mostrarlo junto con algunos identificación del Reproductor original una cantidad adecuada de tiempo (o en una ventana desplazable).

También hay algunas prácticas recomendadas con respecto a la conversación. Se recomienda que en cualquier parte los jugadores se muestran, especialmente en una lista de nombres de jugadores, como un panel de resultados, también mostrar iconos de Silenciar de habla como comentarios del usuario. Esto se realiza mediante una llamada a `xim_player::chat_indicator()` para recuperar un `xim_player_chat_indicator` que representa el estado actual, la instantáneo de chat para que el Reproductor. El ejemplo siguiente muestra el valor de indicador de un `xim_player` objeto apunta a la variable 'ximPlayer' para determinar un valor constante de icono determinado para asignar a una variable 'iconToShow':

```cpp
switch (ximPlayer->chat_indicator())
{
   case xim_player_chat_indicator::silent:
   {
       iconToShow = Icon_InactiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::talking:
   {
       iconToShow = Icon_ActiveSpeaker;
       break;
   }

   case xim_player_chat_indicator::muted:
   {
       iconToShow = Icon_MutedSpeaker;
       break;
   }
   ...
}
```

El valor indicado por `xim_player::chat_indicator()` se espera que cambie con frecuencia como reproductores start y stop hablar, por ejemplo. Está diseñado para admitir aplicaciones de sondeo cada fotograma de la interfaz de usuario como resultado.

> [!NOTE]
> Si un usuario local no tiene suficientes privilegios de las comunicaciones debido a la configuración del dispositivo, `xim_player::chat_indicator()` devolverá `xim_player_chat_indicator::platform_restricted`. La expectativa para cumplir los requisitos para la plataforma es que la aplicación muestran iconografía que indica una restricción de la plataforma de chat de voz o de mensajería y un mensaje al usuario que indica el problema. Se recomienda un mensaje de ejemplo es: "Lo sentimos, no puede hablar ahora."

## <a name="muting-players"></a>Silenciar el audio de los jugadores

Otra práctica recomendada es compatible con los reproductores silenciar el audio. XIM controla automáticamente el sistema silenciamiento iniciadas por los usuarios a través de las tarjetas del Reproductor, pero las aplicaciones deben admitir juegos específicos transitorio de silencio que pueden realizarse dentro de la interfaz de usuario de juego a través de la `xim_player::set_chat_muted()` método. El ejemplo siguiente comienza silenciamiento remoto `xim_player` objeto apunta a la variable 'remotePlayer' para que no se escucha ningún chat de voz y no se recibe ningún chat de texto de él:

```cpp
remotePlayer->set_chat_muted(true);
```

El modo silencioso surte efecto inmediatamente y no hay ningún `xim_state_change` asociados con él. Se pueden deshacer llamando `xim_player::set_chat_muted()` nuevo con el valor false. En el ejemplo siguiente, se reactiva un elemento remoto `xim_player` objeto apunta a la variable 'remotePlayer':

```cpp
remotePlayer->set_chat_muted(false);
```

Silencia siguen en vigor para siempre y cuando el `xim_player` existe, como cuando se mueve a una nueva red XIM con el Reproductor. No se conserva si el jugador sale y vuelve a unir el mismo usuario (como un nuevo `xim_player` instancia).

Normalmente, los jugadores comienzan en el estado reactivó el micrófono. Si desea que la aplicación iniciar un reproductor en el estado desactivado por motivos de juego, puede llamar a `xim_player::set_chat_muted()` en el `xim_player` objeto antes de finalizar el procesamiento asociado `xim_player_joined_state_change`, y XIM garantizará que no habrá ningún período de tiempo donde el audio de voz desde el Reproductor pueda ser escuchado.

Una comprobación de silencio automática basada en la reputación del Reproductor se produce cuando un jugador remoto une a la red XIM. Si el jugador tiene una marca de mala reputación, el Reproductor se desactiva automáticamente. Silenciar solo afecta al estado local y continúa, por tanto, si un jugador se mueve a través de redes. Se realiza una vez y no vuelve a evaluar nuevo para la comprobación automática de silencio reputación siempre y cuando el `xim_player` sigue siendo válida.

## <a name="configuring-chat-targets-using-player-teams"></a>Configuración de destinos de chat con los equipos del Reproductor

Como se indica en la [XIM cómo funciona con los equipos del Reproductor](#how-xim-works-with-player-teams) sección de este documento, el verdadero significado de cualquier valor de índice de equipo determinado depende de la aplicación. XIM no interpretarlos excepto para las comparaciones de igualdad con respecto a la configuración del destino de conversación. Si informa de la configuración de destino de chat `xim::chat_targets()` está actualmente `xim_chat_targets::same_team_index_only`, a continuación, cualquier reproductor determinado solo intercambiará comunicación de chat con otro si los dos tienen el mismo valor notificado por `xim_player::team_index()` (y privacidad / directiva también permite).

Para ser conservador y compatibilidad con escenarios competitivos, recién creado redes XIM automáticamente son configurado como valor predeterminado `xim_chat_targets::same_team_index_only`. Sin embargo, puede ser deseable, por ejemplo, en un juego posterior a la "introductoria" charlar con vanquished oponentes en otro equipo. Puede indicar a XIM para permitir que todo el mundo para comunicarse con todos los demás (donde permitan privacidad y la directiva) mediante una llamada a `xim::set_chat_targets()`. El ejemplo siguiente comienza a configurar todos los participantes en la red XIM para usar un `xim_chat_targets::all_players` valor:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Todos los participantes se informaren de que una nueva configuración de destino está en vigor cuando les proporciona un `xim_chat_targets_changed_state_change`.

Como se indicó anteriormente, tipos de movimiento de red XIM mayoría inicialmente asignará a todos los jugadores el valor de índice del equipo de cero. Esto significa que una configuración de `xim_chat_targets::same_team_index_only` es probable que indistinguible de `xim_chat_targets::all_players` de forma predeterminada. Sin embargo, reproductores que mover a una red XIM con contactos tendrán distintos valores de índice del equipo si la configuración de contactos `xim_team_matchmaking_mode` valor declara dos o más equipos. También puede llamar a `xim_player::xim_local::set_team_index()` en cualquier momento, como se indicó anteriormente. Si su aplicación usa los valores de índice de equipo distinto de cero a través de cualquiera de estos métodos, no olvide administrar los destinos de chat actual establecer de forma adecuada.

## <a name="automatic-background-filling-of-player-slots-backfill-matchmaking"></a>Rellenar automáticamente en segundo plano de ranuras del Reproductor ("reposición" contactos)

Grupos distintos de reproductores de una llamada a `xim::move_to_network_using_matchmaking()` ofrece el servicio Xbox Live la máxima flexibilidad para organizarlos en nuevas redes XIM óptimo rápidamente al mismo tiempo. Sin embargo, algunos escenarios de juego le gustaría mantener una red determinada de XIM intacta y sólo entablar relación adicionales reproductores sólo para llenar las ranuras de vacantes Reproductor. Admite la XIM configuración contactos para que funcione en automáticamente en segundo plano rellenar el modo o "reposición", mediante una llamada a `xim::set_network_configuration()` con un `xim_network_configuration` cuya `xim_allowed_player_joins::matchmade` marcador establecido en su `xim_network_configuration::allowed_player_joins` propiedad.

El ejemplo siguiente configura la reposición contactos para intentar localizar un total de 8 jugadores para una fiesta de inolvidable no equipos (aunque si no se encuentran 8, reproductores de 2 a 7 también son aceptables), mediante un uint64_t constante de modo de juego específicos de la aplicación definida por el valor MYGAMEMODE_ DEATHMATCH que sólo coincidirá con otros jugadores especificar ese mismo valor:

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::matchmade;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 2;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = MYGAMEMODE_DEATHMATCH;

xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Esto hace que el XIM existente de red disponible para dispositivos de una llamada a `xim::move_to_network_using_matchmaking()` de la manera normal. Esos dispositivos no ver ningún comportamiento cambian. Los participantes en la red de reposición XIM no se moverá, pero se proporcionará un `xim_network_configuration_changed_state_change` lo que significa reposición activar, así como varios `xim_matchmaking_progress_updated_state_change` notificaciones cuando sea aplicable. Se agregarán a la red XIM con el valor normal a cualquier reproductor matchmade `xim_player_joined_state_change`.

De forma predeterminada, los contactos de reposición permanecen en progreso indefinidamente, aunque no intente agregar los jugadores si la red XIM ya tiene el número máximo de jugadores especificado por el valor de xim_team_configuration. La reposición se puede deshabilitar mediante la opción xim_allowed_player_joins para no permitir matchmade. El ejemplo siguiente deshabilita la reposición desactivando la marca xim_allowed_player_joins::matchmade conservando todas las otras marcas existentes y opciones de configuración de red.

```cpp
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins &= ~xim_allowed_player_joins::matchmade;
xim::singleton_instance().set_network_configuration(&networkConfiguration);
```

Correspondiente `xim_network_configuration_changed_state_change` se proporcionará a todos los dispositivos, y cuando haya completado este proceso asincrónico, un final `xim_matchmaking_progress_updated_state_change` le proporcionará `xim_matchmaking_status::none` para indicar que no se agregará a la red XIM ningún reproductores matchmade adicionales.

Al habilitar la reposición contactos con un `xim_team_configuration` establecer que declara dos o más equipos, todos los reproductores existentes deben tener un índice de equipo válido comprendido entre 1 y el número de equipos. Esto incluye los jugadores que han llamado `xim_player::xim_local::set_team_index()` para especificar un valor personalizado o que se han unido a mediante una invitación o a través de otro social significa (p. ej., una llamada a `xim::move_to_network_using_joinable_xbox_user_id`) y se han agregado con un valor de índice del equipo predeterminado de 0. Si cualquier jugador no tiene un índice válido de equipo, entonces se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `xim_matchmaking_progress_updated_state_change` con un `xim_matchmaking_status::waiting_for_player_team_index` valor. Una vez que todos los jugadores han proporcionado o corregir sus valores de índice del equipo con `xim_player::xim_local::set_team_index()`, contactos de reposición se reanudarán. Puede encontrar más información en el [XIM cómo funciona con los equipos del Reproductor](#how-xim-works-with-player-teams) sección de este documento.

De forma similar, cuando se habilita la reposición contactos con un `xim_network_configuration` estructura con la `require_player_roles_and_skill_configuration` campo establecido en true para roles o aptitud, a continuación, todos los jugadores deben haber especificado una configuración de contactos por Reproductor distinto de null. Si cualquier jugador no ha hecho, a continuación, se pondrá en pausa el proceso de contactos y todos los participantes se proporcionará un `xim_matchmaking_progress_updated_state_change` con un `xim_matchmaking_status::waiting_for_player_roles_and_skill_configuration` valor. Una vez que han proporcionado todos los jugadores sus `xim_player_roles_and_skill_configuration` estructuras, contactos de reposición se reanudarán. Puede encontrar más información en el [contactos con conocimientos por Reproductor](#matchmaking-using-per-player-skill) y [contactos con rol por el Reproductor](#matchmaking-using-per-player-role) secciones de este documento.

## <a name="querying-joinable-networks"></a>Consulta de redes puede unir

Aunque contactos es una excelente manera de conectar los jugadores rápidamente, a veces es mejor permitir que los jugadores a detectar redes joinable con criterios de búsqueda personalizada y seleccione la red que desean unirse a. Esto puede ser muy útil cuando una sesión de juego podría tener un gran conjunto de reglas configurables de juegos y las preferencias del Reproductor. Para ello, una red existente en primer lugar se debe realizar consultable habilitando `xim_allowed_player_joins::queried` joinability y configuración de la información de red disponible para otros usuarios fuera de la red mediante una llamada a `xim::set_network_configuration()`.

En el ejemplo siguiente se habilita `xim_allowed_player_joins::queried` joinability, red de conjuntos de configuración con una configuración de equipo que permite un total de 1-8 jugadores juntos en 1 equipo, un uint64_t constante de modo de juego específicos de la aplicación definida por el valor GAME_MODE_BRAWL, una descripción "gato y conversión boxing oveja coincide con", un uint32_t constante de índice de asignación específicos de la aplicación definida por el valor MAP_KITCHEN e incluye etiquetas "chatrequired", "fácil", "spectatorallowed":

```cpp
PCWSTR tags[] = { L"chatrequired", L"easy", L"spectatorallowed" };
xim_network_configuration networkConfiguration = *xim::singleton_instance().network_configuration();
networkConfiguration.allowed_player_joins |= xim_allowed_player_joins::queried;
networkConfiguration.team_configuration.team_count = 1;
networkConfiguration.team_configuration.min_player_count_per_team = 1;
networkConfiguration.team_configuration.max_player_count_per_team = 8;
networkConfiguration.custom_game_mode = GAME_MODE_BRAWL;
networkConfiguration.description = L"cat and sheep's boxing match";
networkConfiguration.map_index = MAP_KITCHEN;
networkConfiguration.tag_count = _countof(tags);
networkConfiguration.tags = tags;

xim::set_network_configuration(&networkConfiguration);
```

Otros jugadores fuera de la red, a continuación, pueden encontrar la red mediante una llamada a `xim::start_joinable_network_query()` con un conjunto de filtros que coinciden con la información de red en el anterior `xim::set_network_configuration()` llamar. El ejemplo siguiente inicia una consulta de la red puede unir con la opción de filtro de modo de juego que consultará sólo para redes que utilizan el modo de juego específicos de la aplicación definido por el valor GAME_MODE_BRAWL:

```cpp
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;

xim::start_joinable_network_query(queryFilters);
```

Este es otro ejemplo que usa la opción de filtros de etiqueta para consultar las redes que tienen la etiqueta "fácil" y "spectatorallowed" en su configuración consultable pública:

```cpp
PCWSTR tagFilters[] = { L"easy", L"spectatorallowed" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

También se pueden combinar las opciones de filtro diferentes. El siguiente ejemplo que usa la opción de filtro de modo de juego y la etiqueta de opción para iniciar una consulta para las redes que tienen la constante de modo de juego específicos de la aplicación GAME_MODE_BRAWL y la etiqueta "fácil" de filtro:

```cpp
PCWSTR tagFilters[] = { L"easy" };
xim_joinable_network_query_filters queryFilters = { 0 };
queryFilters.custom_game_mode_filter = GAME_MODE_BRAWL;
queryFilters.tag_filter_count = _countof(tagFilters);
queryFilters.tag_filters = tagFilters;

xim::start_joinable_network_query(queryFilters);
```

Si la operación de consulta se realiza correctamente, la aplicación recibirá una `xim_start_joinable_network_query_completed_state_change` desde el que la aplicación puede recuperar una lista de redes puede unir. La aplicación recibirá también continuamente `xim_joinable_network_query_updated_state_change` para redes joinable adicionales o los cambios que se producen en la lista de redes puede unir hasta que se detenga de forma manual o automática. Se puede detener manualmente la consulta en curso mediante una llamada a `xim::stop_joinable_network_query()`. Se detiene automáticamente cuando se llama a `xim::start_joinable_network_query()` para iniciar una nueva consulta.

La aplicación puede intentar unirse a una red en la lista de redes puede unir mediante una llamada a `xim::move_to_network_using_joinable_network_information()`. En el siguiente ejemplo se da por supuesto que está intentando unirse a un `xim_joinable_network_information` apunta el puntero 'selectedNetwork' no está protegido mediante un código de acceso (por lo que estamos pasando nullptr para el segundo parámetro):

```cpp
xim::move_to_network_using_joinable_network_information(selectedNetwork, nullptr);
```

Al habilitar la consulta de la red con un xim_team_configuration que declara dos o más equipos, los jugadores unido mediante una llamada a `xim::move_to_network_using_joinable_network_information()` tendrá un valor de índice del equipo predeterminado de 0.

> [!NOTE]
> Si la aplicación ha especificado varias usuarios locales y se une a una red tiene menos espacio que el número de usuarios locales, la combinación se puede autenticar correctamente. Pero solo el número permitido de usuarios locales puede unirse a la red.