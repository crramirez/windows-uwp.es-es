---
title: Enviar invitaciones de juego
description: Aprenda cómo usar el Administrador de varios jugadores Xbox Live para permitir que un reproductor enviar invitaciones de juego.
ms.assetid: 8b9a98af-fb78-431b-9a2a-876168e2fd76
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores, manager multijugador, flowchart, juegos invitación
ms.localizationpriority: medium
ms.openlocfilehash: 85aa45558d1443638ba7dd50dbea8923125ef664
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646000"
---
# <a name="send-game-invites"></a>Enviar invitaciones de juego

Uno de los escenarios multijugador más sencillos es permiten que el jugador reproducir su juego en línea con amigos. Este escenario tratan los pasos que necesarios para enviar invitaciones a otro reproductor para unir su juego.

Una vez que haya [inicializado el Administrador de varios jugadores](play-multiplayer-with-friends.md)y tienen [crea una sesión introductoria agregando usuarios locales](play-multiplayer-with-friends.md), debe esperar hasta que reciba el `user_added` eventos antes de empezar a enviar invitaciones.

Hay dos maneras de enviar una invitación:

1. [Invitación de la plataforma Xbox TCUI](#xbox-platform-invite-tcui)
2. [Título implementa la interfaz de usuario personalizada](#title-implemented-custom-ui)

Puede ver un diagrama de flujo del proceso aquí: [Diagrama de flujo: enviar una invitación a otro reproductor](mpm-flowcharts/mpm-send-invites.md).

### <a name="1-xbox-platform-invite-tcui-a-namexbox-platform-invite-tcui"></a>1) invitación de la plataforma Xbox TCUI <a name="xbox-platform-invite-tcui">

| Método | Evento desencadenado |
| -----|----------------|
| `multiplayer_lobby_session::invite_friends()` | `invite_sent` |

Una llamada a `invite_friends()` se abrirá el estándar Xbox UI para invitar a amigos. Esto muestra una interfaz de usuario que permite que el Reproductor seleccione a amigos o jugadores recientes a invitar al juego. Una vez que confirmación las coincidencias del Reproductor, participan varios jugadores Manager envía las invitaciones a los jugadores seleccionados.

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

### <a name="2-title-implemented-custom-uia-nametitle-implemented-custom-ui"></a>2) título implementa la interfaz de usuario personalizada<a name="title-implemented-custom-ui">

| Método | Evento desencadenado |
|-----|----------------|
| `multiplayer_lobby_session::invite_users()` | `invite_sent` |

El título puede implementar un TCUI personalizado para ver los amigos en línea y que se les invitará. Pueden utilizar juegos el `invite_users()` método envíe invita a un conjunto de personas que se define por sus identificadores de usuario de Xbox Live. Esto es útil si prefiere usar su propia interfaz de usuario en el juego en lugar de las existencias Xbox UI.

**Ejemplo:**

```cpp
std::vector<string_t>& xboxUserIds;
xboxUserIds.push_back();  // Add xbox_user_ids from your in-game roster list

auto result = mpInstance->lobby_session()->invite_users(xboxUserIds);
if (result.err())
{
  // handle error
}
```

**Funciones que se realiza mediante el Administrador de varios jugadores**

* Invitar envía directamente a los participantes seleccionados
