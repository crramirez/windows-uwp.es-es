---
title: Reproducir un juego multijugador con SmartMatch
description: Obtenga información sobre cómo usar el Administrador de varios jugadores Xbox Live para permitir que un Reproductor de encontrar un juego multijugador mediante SmartMatch.
ms.assetid: f9001364-214f-4ba0-a0a6-0f3be0b2f523
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores, Administrador de varios jugadores, flowchart, smartmatch
ms.localizationpriority: medium
ms.openlocfilehash: 0c0ba897f23eb690c3044b00cdb4ce3bea975e71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655960"
---
# <a name="find-a-multiplayer-game-by-using-smartmatch"></a>Busque un juego multijugador utilizando SmartMatch

En ocasiones, un jugador podría no tener suficiente amigos en línea cuando desean jugar a un juego o desean jugar contra usuarios aleatorios en línea. Puede usar el servicio de Xbox Live SmartMatch para buscar otros jugadores de Xbox Live

Esta página trata los pasos básicos que necesita para implementar SmartMatch contactos mediante el Administrador de varios jugadores.

## <a name="find-a-match"></a>Buscar una coincidencia

Hay cuatro pasos implicados cuando se usa el Administrador de varios jugadores para enviar una invitación a amigo del usuario y, a continuación, para amigos a unirse al juego en curso:

1. [Inicializar el Administrador de varios jugadores](#initialize-multiplayer-manager)
2. [Crear la sesión introductoria mediante la adición de usuarios locales](#create-lobby)
3. [Enviar invitaciones a amigos](#send-invites)
4. [Aceptar invitaciones](#accept-invites)
5. [Buscar una coincidencia](#find-match)

Los pasos 1, 2, 3 y 5 se realizan en el dispositivo que realiza la invitación.  Normalmente se podría iniciar paso 4 en la máquina del invitado después de iniciar la aplicación a través de la activación de protocolos.

Puede ver un diagrama de flujo del proceso aquí: [Diagrama de flujo: reproducir un juego multijugador mediante el uso de contactos SmartMatch](mpm-flowcharts/mpm-play-with-smartmatch-matchmaking.md).

### <a name="1-initialize-multiplayer-manager-a-nameinitialize-multiplayer-manager"></a>(1) iniciar el Administrador de varios jugadores <a name="initialize-multiplayer-manager">

| Conferencia | Evento desencadenado |
|-----|----------------|
| `multiplayer_manager::initialize(lobbySessionTemplateName)` | N/D |

El objeto de sesión introductoria se crea automáticamente al inicializar el Administrador de varios jugadores, suponiendo que se especifica un nombre de plantilla de sesión válido (configurado en la configuración del servicio). Tenga en cuenta que esto no crea la instancia de la sesión introductoria en el servicio.

**Ejemplo:**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

mpInstance->initialize(lobbySessionTemplateName);
```


### <a name="2-create-the-lobby-session-by-adding-local-usersa-namecreate-lobby"></a>(2) crear la sesión introductoria mediante la adición de usuarios locales<a name="create-lobby">

| Método | Evento desencadenado |
|-----|----------------|
| `multiplayer_lobby_session::add_local_user()` | `user_added_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

En este caso, agrega firmado localmente en Xbox Live, los usuarios a la sesión introductoria. Esto hospeda una sala de nuevo cuando se agrega el primer usuario. Para todos los demás usuarios se agregarán a la sala existente como usuarios secundaria. Esta API también anunciará el vestíbulo del shell para sus amigos a participar. Puede enviar invitaciones, establecer las propiedades introductoria, tener acceso a miembros de lobby mediante lobby() solo una vez que haya agregado el usuario local.

Después de unirse a la sala, Microsoft recomienda establecer la dirección de conexión del miembro local, así como todas las propiedades personalizadas del miembro.

Debe repetir este proceso para todos los iniciado localmente en los usuarios.

**Ejemplo: (usuario local única)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();

auto result = mpInstance->lobby_session()->add_local_user(xboxLiveContext->user());
if (result.err())
{
  // handle error
}

// Set member connection address
string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

// Set custom member properties
mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
```

**Ejemplo: (locales varios usuarios)**

```cpp
auto mpInstance = multiplayer_manager::get_singleton_instance();
string_t connectionAddress = L"1.1.1.1";

for (User^ user : User::Users)
{
  if( user->IsSignedIn )
  {
    auto result = mpInstance.lobby_session()->add_local_user(user);
    if (result.err())
    {
      // handle error
    }

    // Set member connection address
    mpInstance->lobby_session()->set_local_member_connection_address(
        xboxLiveContext->user(), connectionAddress);

    // Set custom member properties
    mpInstance->lobby_session()->set_local_member_properties(xboxLivecontext->user(), ..., ...)
  }
}

```


Los cambios se procesan por lotes en la siguiente `do_work()` llamar.  
Se desencadena manager multijugador un `user_added` evento cada vez que un usuario se agrega a la sesión introductoria. Se recomienda que inspeccionar el código de error del evento para ver si ese usuario se agregó correctamente. En caso de error, se proporcionará un mensaje de error que se detallan los motivos del error.

**Funciones que se realiza mediante el Administrador de varios jugadores**

* Registrar la actividad en tiempo Real y las suscripciones de varios jugadores con el servicio de varios jugadores de Xbox Live
* Crear sesión introductoria
* Únase a todos los jugadores locales como activa
* Cargar SDA
* Establecer propiedades de miembro
* Registrarse para eventos de cambio de sesión
* Establecer la sesión introductoria como sesión activa

### <a name="3-send-invites-to-friends-optional-a-namesend-invites"></a>(3) enviar invitaciones a amigos (opcionales) <a name="send-invites">

| Método | Evento desencadenado |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

A continuación, querrá poner la UI de Xbox estándar para invitar a amigos. Esto muestra una interfaz de usuario que permite que el Reproductor seleccione a amigos o jugadores recientes a invitar al juego. Una vez que confirmación las coincidencias del Reproductor, participan varios jugadores Manager envía las invitaciones a los jugadores seleccionados.

También pueden utilizar juegos el `invite_users()` método envíe invita a un conjunto de personas que se define por sus identificadores de usuario de Xbox Live. Esto es útil si prefiere usar su propia interfaz de usuario en el juego en lugar de las existencias Xbox UI.

**Ejemplo:**

```cpp
auto result = mpInstance->lobby_session()->invite_friends(xboxLiveContext);
if (result.err())
{
  // handle error
}
```

**Funciones que se realiza mediante el Administrador de varios jugadores**

* Aporta seguridad de la consola Xbox stock título de la interfaz de usuario que se puede llamar (TCUI)
* Invitar envía directamente a los participantes seleccionados

### <a name="4-accept-invites-optional-a-nameaccept-invites"></a>(4) aceptar invitaciones (opcionales) <a name="accept-invites">

| Método | Evento desencadenado |
| -----|----------------|
| `multiplayer_manager::join_lobby(Windows::ApplicationModel::Activation::IProtocolActivatedEventArgs^ args)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Cuando un reproductor invitado acepta una invitación de juego o Unidos a un juego de amigos a través de un shell de interfaz de usuario, el juego se inicia en su dispositivo mediante la activación de protocolo. Una vez se inicia el juego, multijugador Manager puede usar los argumentos de evento de protocolo activado para unirse a la sala. Opcionalmente, si no ha agregado los usuarios locales a través de lobby_session()::add_local_user(), puede pasar en la lista de usuarios a través de la API join_lobby(). Si el usuario invitado no se agrega a través de lobby_session()::add_local_user() o a través de join_lobby(), a continuación, se producirá un error y se proporcionará el invited_xbox_user_id() que se envió la invitación como parte de la join_lobby_completed_event_args() join_lobby().

Después de unirse a la sala, Microsoft recomienda establecer la dirección de conexión del miembro local, así como todas las propiedades personalizadas del miembro. También puede establecer el host a través de set_synchronized_host si no existe.

Por último, el Administrador de varios jugadores se automáticamente el usuario en la sesión de juego de combinación si un juego ya está en curso y que tiene espacio para el invitado. El título se notificará a través del evento join_game_completed que proporciona un mensaje y el código de error adecuado.

**Ejemplo:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(), connectionAddress);

```

Error o éxito se controla a través de la `join_lobby_completed` eventos

**Funciones que se realiza mediante el Administrador de varios jugadores**

* Registrar ATR & suscripciones multijugador
* Unirse a sesión introductoria
 * Limpieza de estado introductoria existente
 * Únase a todos los jugadores locales como activa
 * Cargar SDA
 * Establecer propiedades de miembro
* Registrarse para eventos de cambio de sesión
* Establecer la sesión introductoria como sesión activa
* Únase a la sesión de juego (si existe)
 * Identificador de transferencia de usos


### <a name="5-find-match-a-namefind-match"></a>(5) Buscar coincidencia <a name="find-match">

| Conferencia | Evento desencadenado |
|-----|----------------|
| `multiplayer_manager::find_match()` | Muchos eventos, como se describe en ```match_status``` (p. ej.: ```searching```, ```found```, ```measuring```, etcetera) |

Después de invitaciones, si existe, se han aceptado y el host está listo para empezar a reproducir el juego, puede usar SmartMatch para cualquier búsqueda de un juego existente que tiene suficiente Reproductor abierto ranuras para todos los miembros de la sesión introductoria mediante una llamada a `find_match()`, o cree un nuevo juego se ssion que incluye todos los miembros de la sesión introductoria y relleno manchas de abierta con otras personas que busca una coincidencia del mismo tipo de juego, mediante una llamada a `join_game_from_lobby()` seguido con `set_auto_fill_members_during_matchmaking()`.

Antes de llamar `find_match()`, primero debe configurar hoppers en la configuración del servicio. Un hopper define las reglas que SmartMatch se usa para hacer coincidir los jugadores.

**Ejemplo:**

```cpp
auto result = mpInstance.find_match(HOPPER_NAME);
if (result.err())
{
  // handle error
}
```

**Funciones que se realiza mediante el Administrador de varios jugadores**

* Cree una incidencia de coincidencia
* Controlar todas las fases de QoS
* Controlar los cambios de la lista.
 * Volver a enviar (si es necesario)
* Sesión de juego de destino de combinaciones
* Anunciar el juego a través de la sesión introductoria
