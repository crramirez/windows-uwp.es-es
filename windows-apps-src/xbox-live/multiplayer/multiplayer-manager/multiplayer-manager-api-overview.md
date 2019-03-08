---
title: Información general de la API del administrador multijugador
description: Obtenga información acerca de la API de Xbox Live Manager participan varios jugadores.
ms.assetid: 658babf5-d43e-4f5d-a5c5-18c08fe69a66
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores, Administrador de varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: 7838de6845bc6c49acf649c0e859cd0d7020490f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628030"
---
# <a name="multiplayer-manager-api-overview"></a>Introducción a la API del Administrador de varios jugadores

Esta página describe una amplia visión general de la API del Administrador de varios jugadores y cómo se usan en un juego. Llama a las clases más importantes y los métodos de la API. Para obtener información detallada de API, consulte la documentación de referencia. Para obtener ejemplos de cómo usar estas API en una aplicación, vea el ejemplo de varios jugadores.

## <a name="namespace"></a>Espacio de nombres
Las clases de administrador de varios jugadores se incluyen el espacio de nombres siguiente:

| language | espacio de nombres |
| --- | --- |
| C++/CX | Microsoft::Xbox::Services::Multiplayer::Manager |
| C++ | xbox::services::multiplayer::manager |

Hay las siguientes listas se describen las clases principales que debe conocer:

* [Clase de administrador de varios jugadores](#multiplayer-manager-class)
* [Clase de eventos de varios jugadores](#multiplayer-event-class)
* [Clase de miembro de varios jugadores](#multiplayer-member-class)
* [Clase de sesión introductoria de varios jugadores](#multiplayer-lobby-session-class)
* [Clase de sesión de juego multijugador](#multiplayer-game-session-class)

## <a name="multiplayer-manager-class-a-namemultiplayer-manager-class"></a>Clase de administrador de varios jugadores <a name="multiplayer-manager-class">

| language | clase |
| --- | --- |
| C++/CX | MultiplayerManager |
| C++ | multiplayer_manager |

Esta clase es la clase principal para interactuar con el Administrador de varios jugadores. Es una clase singleton, por lo que solo se necesita crear e inicializar la clase una vez.
Esta clase contiene un objeto de sesión introductoria único y un objeto de sesión único de juego.

Como mínimo, debe llamar a la `initialize()` y `do_work()` métodos en esta clase en orden para el Administrador de varios jugadores a función.

Las tablas siguientes se describen algunas, pero no todos, de los más comúnmente utilizan métodos y propiedades para esta clase. Para obtener una lista descriptiva completa de los miembros de clase, consulte la referencia.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| Initialize() | initialize() | Inicializa el Administrador de varios jugadores. Debe llamar a este método antes de usar el Administrador de varios jugadores. |
| DoWork() | do_work() | Actualiza los Estados de sesión visible de la aplicación. Debe llamar a este método al menos una vez por fotograma, y su juego debe controlar los eventos de varios jugadores devueltos por el método. |
| JoinLobby() | join_lobby() | Proporciona una manera de combinaciones una sesión introductoria de amigo a través de un handleId que identifica el área de recepción que desea combinar o cuando el usuario acepta una invitación que hace que el título obtener el protocolo activado. |
| JoinGameFromLobby() | join_game_from_lobby() | Únase a la partida de la sala si existe y si hay espacio. Si la sesión no existe, crea una nueva sesión de juego con los miembros existentes de la sala. Esto no migrar propiedades de la sesión introductoria existente a la sesión de juego. Después de unirse, puede establecer las propiedades o el host a través de `game_session()::set_synchronized_*` API. El título es necesario para llamar a esta API en todos los clientes que deseen unirse a la sesión de juego.|
| JoinGame() | join_game() | Se une a una sesión existente de juego recibe un nombre de sesión único, que normalmente se encuentran a través de servicios de contactos de terceros. Puede pasar en la lista de xbox de identificadores de usuario que desea que forme parte del juego.|
| FindMatch() | find_match() | Usar contactos de Xbox Live para buscar y unirse a un juego. |
| LeaveGame() | leave_game() | Deja un juego y devuelve el miembro y todos los miembros locales a la sala. |
| **Propiedades** | | |
| LobbySession | lobby_session() | Un identificador para un objeto que representa la sesión introductoria. |
| GameSession |  game_session() | Un identificador para un objeto que representa la sesión de juego. |

## <a name="multiplayer-event-class-a-namemultiplayer-event-class"></a>Clase de eventos de varios jugadores <a name="multiplayer-event-class">

| language | clase |
| --- | --- |
| C++/CX | MultiplayerEvent |
| C++ | multiplayer_event |

Cuando se llama a `do_work()`, Administrador de varios jugadores devuelve una lista de eventos que representan los cambios a las sesiones desde que se llame por última vez `do_work()`. Estos eventos incluyen cambios, como un miembro ha unido a una sesión, un miembro ha dejado una sesión, han cambiado las propiedades de miembro, el cliente de host ha cambiado, etcetera.

Para obtener una lista de todos los tipos de eventos posibles, vea el `multiplayer_event_type` enumeración.

Cada uno devuelve `multiplayer_event` incluye un `event_args`, que debe convertirse en la clase event_args adecuado para el tipo de evento. Por ejemplo, si la `event_type` es `member_joined`, a continuación, convertiría la `event_args` hacen referencia a un `member_joined_event_args` clase.

El juego debe controlar cada uno de los eventos según sea necesario después de llamar a `do_work()`.

## <a name="multiplayer-member-class-a-namemultiplayer-member-class"></a>Clase de miembro de varios jugadores <a name="multiplayer-member-class">

| language | clase |
| --- | --- |
| C++/CX | MultiplayerMember |
| C++ | multiplayer_member |

Esta clase representa un reproductor en una feria o una sesión de juego. La clase contiene propiedades sobre el miembro, incluido el XboxUserID para el Reproductor, la dirección de conexión de red para el Reproductor, propiedades personalizadas para cada jugador y mucho más.

## <a name="multiplayer-lobby-session-class-a-namemultiplayer-lobby-session-class"></a>Clase de sesión introductoria de varios jugadores <a name="multiplayer-lobby-session-class">

| language | clase |
| --- | --- |
| C++/CX | MultiplayerLobbySession |
| C++ | multiplayer_lobby_session |

Una sesión persistente que se usa para administrar los usuarios que son locales para este dispositivo y sus amigos invitados que deseen reproducir juntos. Una sesión introductoria debe contener a al menos un miembro para varios jugadores Manager realizar ninguna acción para varios jugadores. Inicialmente, puede crear una nueva sesión introductoria mediante una llamada a la `add_local_user()` método.

Las tablas siguientes se describen algunas, pero no todos, de los más comúnmente utilizan métodos y propiedades para esta clase. Para obtener una lista descriptiva completa de los miembros de clase, consulte la referencia.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| AddLocalUser() | add_local_user() | Agrega un usuario local (es decir, un jugador que ha iniciado sesión en el dispositivo local) a la sesión introductoria. Si este es el primer miembro agregado a una sesión introductoria, a continuación, crea una nueva sesión introductoria. |
| RemoveLocalUser() | remove_local_user() | Quita a un miembro especificado de la sesión de juego e introductoria. |
| InviteFriends() | invite_friends() | Se abre una consola Xbox Live interfaz de usuario estándar que permite que el Reproductor seleccionar personas de su lista de amigos y, a continuación, invita a los jugadores del juego. |
| InviteUsers() | invite_users() | Invita a los usuarios de Xbox Live sepcified al juego. |
| SetLocalMemberConnectionAddress() | set_local_member_connection_address() | Establece la dirección de red para el miembro local. Juegos pueden usar esta dirección de red para establecer comunicaciones de red entre los miembros. |
| SetLocalMemberProperties() | set_local_member_properties() | Establece una propiedad personalizada para un miembro local. La propiedad se almacena en una cadena JSON. |
| DeleteLocalMemberProperties() | delete_local_member_properties() | Quita una propiedad personalizada para un miembro local. |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Establece una propiedad personalizada de la sesión introductoria. La propiedad se almacena en una cadena JSON. Si la propiedad se comparte entre los dispositivos y pueden actualizar varios dispositivos al mismo tiempo, use la versión sincronizada del método. |
| IsHost() | is_host() | Indica si el dispositivo actual está actuando como el host introductoria. |
| SetSynchronizedHost() | set_synchronized_host() | Establece el host de la sala. |
| **Propiedades** | | |
| LocalMembers | local_members() | La colección de miembros que han iniciado sesión en el dispositivo local. |
| Miembros | members() | La colección de miembros que se encuentran en la sesión introductoria. |
| Propiedades | properties() | Un objeto JSON que representa una colección de propiedades para la sesión introductoria. |
| Host | host() | El miembro de host para el área de recepción. |


## <a name="multiplayer-game-session-class-a-namemultiplayer-game-session-class"></a>Clase de sesión de juego multijugador <a name="multiplayer-game-session-class">

| language | clase |
| --- | --- |
| C++/CX | MultiplayerGameSession |
| C++ | multiplayer_game_session |

La sesión de juego representa el grupo de miembros de Xbox Live que forman parte de una instancia de un juego real. Esto puede incluir los jugadores que se han asociado a través de servicios de contactos.

Para iniciar una nueva sesión de juego que incluye miembros de la `lobby_session`, puede llamar a `multiplayer_manager::join_game_from_lobby()`. Si desea usar contactos de Xbox Live, puede llamar a `multiplayer_manager::find_match()`. Si usa un servicio de emparejamiento de terceros, puede llamar a `multiplayer_manager::join_game()`.

Las tablas siguientes se describen algunas, pero no todos, de los más comúnmente utilizan métodos y propiedades para esta clase. Para obtener una lista descriptiva completa de los miembros de clase, consulte la referencia.

| C++/CX | C++ | description |
| --- | --- | --- |
| **Métodos** | | |
| SetProperties() / SetSynchronizedProperties() | set_properties() / set_synchronized_properties() | Establece una propiedad personalizada para la sesión de juego. La propiedad se almacena en una cadena JSON. Si la propiedad se comparte entre los dispositivos y pueden actualizar varios dispositivos al mismo tiempo, use la versión sincronizada del método. |
| IsHost() | is_host() | Indica si el dispositivo actual está actuando como el anfitrión del juego. |
| SetSynchronizedHost() | set_synchronized_host() | Establece el host del juego. |
| **Propiedades** | | |
| Miembros | members() | La colección de miembros que se encuentran en la sesión de juego. |
| Propiedades | properties() | Un objeto JSON que representa una colección de propiedades para la sesión de juego. |
| Host | host() | El miembro de host para el juego. |
