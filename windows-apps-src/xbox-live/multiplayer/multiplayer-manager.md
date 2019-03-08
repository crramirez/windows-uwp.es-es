---
title: Administrador de varios jugadores
description: Obtenga información sobre Administrador multijugador Xbox Live, una API de alto nivel diseñado para que sea más fácil de implementar varios jugadores.
ms.assetid: f3a6c8bc-4f73-4b99-ac51-aadee73c8cfa
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 99159b5d126c671b07d37e20f1bcd61452c7d670
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594720"
---
# <a name="multiplayer-manager"></a>Administrador de varios jugadores

Xbox Live proporciona una amplia compatibilidad para agregar funcionalidad multijugador a los títulos, lo que permite su juego para conectarse a los miembros de Xbox Live en todo el mundo.  Esto incluye escenarios de contactos enriquecido, la posibilidad de que un reproductor para unirse a un juego de un amigo en curso y mucho más. Sin embargo, la implementación de Xbox Live multijugador directamente mediante las API de varios jugadores 2015 puede ser una tarea compleja, que requieren un alto grado de diseño y pruebas para comprobar que está siguiendo los procedimientos recomendados y se cumplen los requisitos de certificación.

Varios jugadores Manager facilita la tarea Agregar funcionalidad multijugador a su juego mediante la administración de sesiones y contactos y proporcionando un estado y modelo de programación basada en eventos. Es un conjunto de API diseñados para que sea fácil de implementar escenarios multijugador para de Xbox Live game. Proporciona una API que se orienta en torno a escenarios multijugador comunes como jugar a juegos multijugador con amigos, invita a control de juego, control de combinación en curso, contactos y mucho más. Admite varios usuarios locales y resulta más fácil para el título de la integración con el directorio de sesión de varios jugadores si usas un servicio de emparejamiento de terceros. Muchos de estos escenarios pueden realizarse con unas pocas llamadas de API.

## <a name="key-features"></a>Principales características
Estas son las principales características de la API del Administrador de varios jugadores:

* Administración de la sesión fácil y contactos de Xbox Live
* Estado y eventos según el modelo de programación.
* Garantiza prácticas recomendadas de Xbox Live, junto con la que se va a varios jugadores XR conforme.
* Es compatible con los títulos de Xbox uno XDK y UWP.
* Implementa [diagramas de flujo de varios jugadores 2015](https://developer.xboxlive.com/en-us/platform/development/education/Documents/Xbox%20One%20Multiplayer%202015%20Developer%20Flowcharts.aspx).
* Funciona junto con las API de 2015 tradicional de varios jugadores.

>**Importante** -su juego aún debe implementar los eventos necesarios para varios jugadores en línea para conseguir la certificación.

## <a name="overview"></a>Introducción
El Administrador de varios jugadores se orienta en torno a unos cuantos conceptos clave:
* `lobby_session` : Una sesión persistente que se usa para administrar los usuarios que son locales para este dispositivo y sus amigos invitados que deseen reproducir juntos. El grupo puede desempeñar varias rondas, mapas, niveles, etc., del juego, y la sesión introductoria realiza un seguimiento de este grupo de amigos (incluidos los reproductores locales en el dispositivo). Normalmente, este grupo está formado mientras el host puede navegar a través de los menús, charlar con los miembros del grupo para decidir qué modo de juego que deseen jugar.

* `game_session` : Jugadores de pistas que están reproduciendo una instancia específica del juego. Por ejemplo, una carrera, mapa o nivel. Puede crear una nueva sesión de juego a través de `join_game_from_lobby` que incluye los miembros de la sesión introductoria.  Cuando un miembro acepta una invitación, se agregarán a la sala de espera y la sesión de juego (si hay espacio). Los jugadores adicionales se pueden agregar a la sesión de juego si contactos está habilitado, pero no se agregan estos agentes adicionales a la sesión introductoria. Esto significa que, una vez que el juego se termina, los participantes en la sesión introductoria siguen estando juntos, mientras que los jugadores de contactos adicionales no están.

* `multiplayer_member` : Representa un usuario individual que inician sesión en un dispositivo local o remoto.

* `do_work` : Garantiza que se mantienen las actualizaciones de estado del juego adecuado entre el título y el servicio de varios jugadores de Xbox Live. Para garantizar un rendimiento óptimo, debe llamarse a do_work() con frecuencia, por ejemplo, una vez por fotograma. Proporciona una lista de `multiplayer_event` para el juego controlar los eventos de devolución de llamada.

## <a name="state-machine"></a>Equipo de estado
El `do_work()` llamada es necesaria para garantizar su estado se mantiene actualizado.  Para varios jugadores Manager para realizar su trabajo, deben llamar los desarrolladores la `do_work()` método con regularidad. La forma más fiable de hacerlo es llamar al menos una vez por fotograma. `do_work()` Devuelve rápidamente cuando no tiene ningún trabajo que hacer, así que no hay ningún motivo de preocupación sobre llamarlo con demasiada frecuencia.

Todos los objetos devueltos por la API del Administrador de varios jugadores no deben considerarse segura para subprocesos. Sin embargo, le ofrece control de sincronización de subprocesos si está llamando desde varios subprocesos. La biblioteca tiene protección multithreading interno, pero todavía deberá implementar su propio bloqueo si necesita un subproceso tenga acceso a los valores: por ejemplo, recorrer la lista de members() mientras otro subproceso podría ser invocar `do_work()`.

Administrador de varios jugadores mantiene un modelo en función del estado actualizando las sesiones en segundo plano como reproductores de join, dejen o cuando se actualizan las sesiones. Con el fin de ayudar a evitar problemas de sincronización de subprocesos entre el subproceso de interfaz de usuario y el subproceso de juego, el Administrador de varios jugadores no actualiza el estado de visibilidad de la aplicación de las sesiones hasta que llame a la `do_work()` método. Tradicionalmente podría recibir notificaciones sobre eventos, como cambios de la sesión en un subproceso en segundo plano y, a continuación, debe sincronizarlo con un subproceso de interfaz de usuario para mostrar estos cambios. Con el Administrador de varios jugadores, este trabajo se realiza automáticamente en segundo plano.  Puede llamar a `do_work()` en el subproceso principal en el momento de su elección, para obtener la instantánea del estado más reciente que participan varios jugadores Manager almacena en búfer automáticamente en segundo plano.

## <a name="events-and-notifications"></a>Eventos y notificaciones
El Administrador de varios jugadores define un conjunto de eventos importantes (consulte `multiplayer_event_type`) y notifica el título a través de la `do_work()` método cuando se produzcan. Eventos, como reproductores remotos o la baja, cambiar las propiedades de miembro, o cambiar el estado de sesión. Todas las API de varios jugadores Manager son sincrónicos. El `do_work()` método devuelve una lista de eventos cuando se completan estas operaciones asincrónicas. El título debe controlar estos eventos según sea necesario. Consulte la `multiplayer_event` documentación para obtener más detalles de la clase.

Para cada evento, según el tipo de evento, primero debe convertir el `event_args` a la clase de evento adecuado arg para obtener las propiedades de evento. El ejemplo siguiente se muestra cómo utilizar `do_work()` para controlar los eventos:

```cpp
auto eventQueue = mpInstance.do_work();
for (auto& event : eventQueue)
{
    switch (event.event_type())
    {
      case multiplayer_event_type::member_joined:
      {
        auto memberJoinedArgs =  std::dynamic_pointer_cast<member_joined_event_args>(event.event_args());
        HandleMemberJoined(memberJoinedArgs);
        break;
      }
      case multiplayer_event_type::session_property_changed:
      {
        auto sessionPropertyChangedArgs =  std::dynamic_pointer_cast<session_property_changed_event_args>(event.event_args());
        HandlePropertiesChanged(sessionPropertyChangedArgs);
        break;
      }
      . . .
    }
}

```

## <a name="scenarios"></a>Escenarios

En esta sección vamos a través de algunos escenarios comunes y las API que se llamaría a en cada escenario.  También se proporciona información sobre lo que hace el Administrador de varios jugadores en segundo plano.

* [Jugar con amigos](multiplayer-manager/play-multiplayer-with-friends.md)
* [Buscar una coincidencia](multiplayer-manager/play-multiplayer-with-matchmaking.md)
* [Enviar invitaciones de juegos](multiplayer-manager/send-game-invites.md)
* [Identificador de activación de protocolos](multiplayer-manager/handle-protocol-activation.md)

Encontrará una introducción de alto nivel de la API en [Introducción a la API del Administrador de varios jugadores](multiplayer-manager/multiplayer-manager-api-overview.md).

## <a name="what-multiplayer-manager-does-not-do"></a>¿Qué hacer el Administrador de varios jugadores
Mientras que participan varios jugadores Manager hace que sea mucho más fácil de implementar escenarios multijugador y abstrae algunos de los datos del desarrollador, hay algunas cosas no controla o no es ideal para.

* Juegos de servidor en línea persistente, como MMOs u otros tipos de juegos que requieren sesiones grandes (más de 100 jugadores en una sesión).
* Servidor de administración de sesiones de servidor

>Administrador de varios jugadores no está asociado a ninguna tecnología de red específica y deben funcionar con cualquier nivel de red.

## <a name="next-steps"></a>Pasos siguientes

Consulte el C++ o WinRT *participan varios jugadores* ejemplo para obtener un ejemplo de la API.

La documentación de API puede encontrarse en las guías de C++ o WinRT en el espacio de nombres Microsoft::Xbox::Services::Multiplayer::Manager.  También puede ver el `multiplayer_manager.h` encabezado.

Si tiene alguna pregunta, comentarios, o surge algún problema con el Administrador de varios jugadores, póngase en contacto con su madre o publicar un subproceso de soporte técnico en los foros en [ https://forums.xboxlive.com ](https://forums.xboxlive.com).
