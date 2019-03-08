---
title: Tratar la activación de protocolos
description: Obtenga información sobre cómo usar el Administrador de varios jugadores Xbox Live para controlar la activación de protocolos.
ms.assetid: e514bcb8-4302-4eeb-8c5b-176e23f3929f
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, el Administrador de varios jugadores, activación de protocolos
ms.localizationpriority: medium
ms.openlocfilehash: 0b5dead742e18bbf5f3e9c271109352ae48e8fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655840"
---
# <a name="handle-protocol-activation"></a>Tratar la activación de protocolos

Activación de protocolos es cuando el sistema inicia automáticamente un juego en respuesta a otra acción, normalmente cuando un reproductor acepta una invitación de juegos desde otro reproductor.

El título puede obtener el protocolo activado a través de las maneras siguientes:

* Cuando un usuario acepta una invitación de juegos
* Cuando un usuario selecciona "Unir Game" desde gamercard un jugador.

Este escenario describe cómo controlar la activación de protocolos cuando se inicia el título y unir el vestíbulo y el juego (si existe).

Puede ver un diagrama de flujo del proceso aquí: [Diagrama de flujo: Reproductor de activación de controlador de protocolo](mpm-flowcharts/mpm-on-protocol-activation.md).

| Método | Evento desencadenado |
| -----|----------------|
| `multiplayer_manager::join_lobby(IProtocolActivatedEventArgs^ args, User^)` | `join_lobby_completed_event` |
| `multiplayer_lobby_session::set_local_member_connection_address()` | `local_member_connection_address_write_completed ` |
| `multiplayer_lobby_session::set_local_member_properties()` | `member_property_changed` |

Cuando un reproductor acepta una invitación de juego o une el juego de un amigo a través de gamercard del jugador, el juego se inicia en su dispositivo mediante la activación de protocolo. Una vez se inicia el juego, multijugador Manager puede usar los argumentos de evento de protocolo activado para unirse a la sala. Opcionalmente, si aún no ha agregado los usuarios locales a través de `lobby_session()::add_local_user()`, puede pasar en la lista de usuarios a través de la `join_lobby()` API. Si no se agrega el usuario invitado, o si se tratase la invitación de otro usuario distinto del que se ha agregado, `join_lobby()` provocarán errores y proporcionar la `invited_xbox_user_id()` que se envió la invitación como parte de la `join_lobby_completed_event_args`.

Después de unirse a la sala, Microsoft recomienda establecer la dirección de conexión del miembro local, así como todas las propiedades personalizadas del miembro. También puede establecer el host a través de `set_synchronized_host` si no existe.

Por último, el Administrador de varios jugadores se automáticamente el usuario en la sesión de juego de combinación si un juego ya está en curso y que tiene espacio para el invitado. El título se notificará a través del `join_game_completed` que proporciona un código de error y un mensaje de evento.

**Ejemplo:**

```cpp
auto result = mpInstance().join_lobby(IProtocolActivatedEventArgs^ args, users);
if (result.err())
{
  // handle error
}

string_t connectionAddress = L"1.1.1.1";
mpInstance->lobby_session()->set_local_member_connection_address(
    xboxLiveContext->user(),
    connectionAddress);
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
