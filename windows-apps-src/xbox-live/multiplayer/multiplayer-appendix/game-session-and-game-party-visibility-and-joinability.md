---
title: Joinability y visibilidad de la sesión de juego
description: Describe la sesión de juego de Xbox Live y visibilidad de fabricantes de juegos y joinability.
ms.assetid: 39b6dac1-0c6b-4dc1-9fe0-3cb7c471fbab
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 65da0d60080cd790b84ce46f8598c2f6f638ff05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619720"
---
# <a name="game-session-and-game-party-visibility-and-joinability"></a>Sesión de juego y visibilidad de fabricantes de juegos y joinability

En Xbox One, el *visibilidad* y *joinability* configuración para las sesiones de juego y fabricantes de juegos, respectivamente, controlar acceso a las experiencias de varios jugadores. Para proporcionar una experiencia de usuario excelente para unirse a la sesión y para reproductores que invita a entidades y las sesiones de juego, los desarrolladores de título deben comprender estos valores. En este artículo Revisa las diferencias entre visibilidad y joinability, y describe la configuración específica, se recomienda que los títulos de usar para proporcionar a los consumidores el flujo de mejor multijugador usuario.

## <a name="game-session-visibility"></a>Visibilidad de la sesión de juego


Acceso de directorio de sesión de varios jugadores (MPSD) sesión viene determinado por dos configuraciones: sesión *visibilidad* y sesión *joinability*. Esta configuración se puede usar para limitar el acceso de la sesión en el nivel de MPSD.

El primer aspecto del control de acceso es la visibilidad de la sesión. Visibilidad de la sesión es una constante que se establece en el momento de crear la sesión. Normalmente se define en la plantilla de sesión y determina qué tipos de usuarios han leído y acceso de escritura a una sesión. Los valores de la configuración de visibilidad son:

-   *Abierto:* todos los usuarios pueden leer y escribir en la sesión.

-   *Visible:* todos los usuarios pueden leer, pero solo unirse y reservado jugadores pueden escribir a la sesión.

-   *Privado:* solo unirse y reservado reproductores pueden leer o escribir en la sesión.

> **Nota:** Establecer la visibilidad a privada puede necesitar reintentos en el **join()** llamar el Reproductor de invitado. Si se envía una invitación a través de la plataforma de interfaz de usuario, puede recibir otro reproductor antes de que el Reproductor se ha reservado en la sesión. Esta condición de carrera puede provocar un **join()** llamar para que un jugador invitado producirá un error porque las sesiones privadas o visibles requieren una reserva para el Reproductor de unión.
>
> Si no existe ninguna reserva, se produce un error de acceso en la sesión (HTTP/403). El Reproductor invitado, a continuación, tendría que volver a intentar la **join()** operación pero reintentos no deben realizarse con más frecuencia que cada cinco segundos. Se recomienda que los títulos de establecer la visibilidad de la sesión para abrir para evitar condiciones de carrera durante el flujo de combinación para un reproductor.

## <a name="game-session-joinability"></a>Joinability de sesión de juego


El segundo aspecto que el control de acceso es joinability. Se puede establecer dinámicamente durante la duración de la sesión y determina qué tipos de los usuarios pueden participar en una sesión. Los valores de joinability sesión son:

-   *Ninguno* (valor predeterminado), no existen restricciones sobre quién puede unirse a la sesión.

-   *Local*: solo los usuarios locales pueden unirse a la sesión.

-   *Después,* solo usuarios locales y los usuarios que van seguidos por otros miembros de la sesión pueden unirse a la sesión sin una reserva.

Un host de sesión o un árbitro puede crear una sesión privada a través de la configuración joinability: realizar joinability local o seguidos se restringir el acceso a una sesión de juego y hacerla privada.

Además, el host de sesión o el árbitro debe realizar un seguimiento de la joinability de una sesión, para que la sesión anterior invitaciones se pueden rechazar a nivel de host si es necesario. Por ejemplo, si no han llegado los reproductores de invitado para unirse a una sesión o la entidad hasta que la sesión ya está completa, el host o un árbitro puede indicar a los reproductores de unión que se ha bloqueado la sesión y que necesitan para abandonar la sesión o de terceros automáticamente.

**Nota:** Joinability de sesión de juego no se refleja en la interfaz de usuario de aplicación de terceros. Incluso si joinability está restringido, la aplicación de terceros invitar a la interfaz de usuario y **ShowSendInvitesAsync** permitirá invitaciones de la sesión de juego.

## <a name="game-party-joinability"></a>Joinability fabricantes de juegos


Además de controlar la sesión MPSD, el título también tiene control sobre el estado de joinability de fabricantes de juegos. Esta configuración puede utilizarse para limitar el acceso de terceros de juego en el nivel de entidad.

Joinability de entidad se puede cambiar dinámicamente una vez creada la entidad de juego. El estado de la joinability de una entidad de juego se refleja en la interfaz de usuario de invitar a la entidad. Se puede establecer en:

-   *Sólo por invitación,* esta configuración requiere una invitación para unirse a la entidad de juego.

-   *Puede unir tus amigos* (valor predeterminado)*:* esta configuración requiere una relación de confianza para que un jugador a la fiesta de juego (para unirse a una entidad, un miembro de entidad tiene que seguir el Reproductor de unión).

Joinability puede utilizarse para crear una entidad de juego sólo por invitación. Para restringir el acceso a una entidad y requieren que los jugadores han recibido una invitación para unirse, joinability debe establecerse en "sólo por invitación".

**Nota:** Joinability de entidad no influye en joinability de sesión, pero la joinability de entidad se refleja en la interfaz de usuario de aplicación de terceros. Los jugadores pueden cambiar el joinability de entidad en la aplicación de terceros manualmente fuera de un título.

## <a name="recommendations"></a>Recomendaciones


Las siguientes recomendaciones sirven para los escenarios más comunes de título. Si es posible, títulos deben seguir estos valores, y deben usar lógica en el título para tomar una decisión sobre si un nuevo jugador se admitió en la sesión o no autoritativo, final.

### <a name="recommended-game-session-visibility-open"></a>Recomienda la visibilidad de la sesión de juego: abrir

Las sesiones abiertas de juego no requieren las reservas del Reproductor, invitaciones la forma que se simplifica el flujo. El host de sesión o un árbitro no reservar los jugadores en el documento de la sesión después de que se ha enviado una invitación. En su lugar, el árbitro o host de sesión solo realiza un seguimiento de los jugadores invitados localmente. Esto permite que los reproductores para conectarse al host inmediatamente y determinar si debe participar en una sesión, se rechazan o debe esperar (si se admiten los jugadores en espera). El host o el árbitro es la máxima autoridad y responde e indica el nuevo miembro a permanecer en o dejar la sesión.

**Nota:** Esto requerirá que el Reproductor de invitado iniciar un título y conectar con el host o el árbitro antes de que se ha tomado la decisión final. Es aceptable para mostrar un mensaje de error al usuario si una sesión esté lleno o se ha rechazado una invitación.

> **Nota:** Para establecer una conexión con el host o de arbiter, se requiere una dirección de dispositivo segura. El **HostDeviceToken** en la sesión se deben utilizar para indicar qué miembro de la sesión es el host actual o el árbitro de una sesión y dirección del dispositivo seguro debe conectar un Reproductor de invitado.

### <a name="recommended-game-session-joinability-default-not-set"></a>Recomienda joinability sesión juegos: predeterminado (*no establece*)

Actualmente joinability partida no influye en la interfaz de usuario de aplicación de terceros y no se expone al usuario. Para simplificar el flujo de título, joinability juego debe permanecer en el valor predeterminado, y toda la autoridad de combinación debe controlarse a través de arbiter o host.

### <a name="recommended-game-party-joinability"></a>Recomienda joinability de fabricantes de juegos

Joinability fabricantes de juegos es depende del tipo de sesión que se trata de un título para proporcionar a un usuario. Los dos escenarios son:

-   Abra el juego, para un juego abierto, se debe dejar el joinability fabricantes de juegos en el valor predeterminado: Puede unir tus amigos. Esto permite que sus amigos a unirse al juego de entidad (y por extensión de la sesión de juego) sin una invitación.

-   Juego privado, para un juego privado o cerrado, debe establecerse el joinability fabricantes de juegos para invitar a solo. Esto restringe otros jugadores de unirse a la entidad (y por extensión de la sesión de juego) sin una invitación.

**Nota**: Los jugadores pueden cambiar manualmente el joinability de una sesión de juego a través de la aplicación de terceros.

Para ambos tipos de juegos, el host o un árbitro debe permanecer la máxima autoridad para determinar quién está aceptado en una sesión. Esto aborda las condiciones de carrera que pueden producirse de cambio de flujo de juego de open a privada. Si el host de arbiter rechaza un jugador, la instancia de título rechazadas debe quitar el encargado de la sesión y mostrar una interfaz de usuario en el juego para también permitir que un reproductor dejar la entidad.

### <a name="maximum-session-size"></a>Tamaño máximo de la sesión

El tamaño máximo de miembros para una sesión puede utilizarse para limitar el número de jugadores participar en una sesión de juego. Sin embargo, este límite puede provocar una zona abierta a seguirá apareciendo como completa durante determinadas desconectar escenarios. Por ejemplo, si un jugador se desconecta como resultado un error de red o de alimentación, el retraso no se reflejan inmediatamente en la sesión. El miembro se establecerá en inactivo tan pronto como el servicio de presencia detecta la desconexión (hasta 20 segundos) y, a continuación, se quitará el Reproductor haya transcurrido el tiempo de espera inactivo.

En comparación, una malla del mismo nivel que usa un latido para detectar una desconexión a menudo es consciente de una desconexión en dos o tres segundos y se pudo abrir la ranura player inmediatamente. Sin embargo, el árbitro o el host no se puede quitar a otros miembros.

Para solucionar este problema, el tamaño máximo de miembros de una sesión debería establecerse siempre mayor que el número máximo de reproductores que realmente admite un título. Por ejemplo, si el número de Reproductor es 8, el título debe establecer el tamaño máximo de la sesión a las 12. De esta manera pueden unir nuevos jugadores que incluso si los reproductores anteriores no han agotado todavía. El host o un árbitro determina si la sesión está lleno y se establece una propiedad de sesión personalizada que determina si pueden unirse nuevos jugadores (**IsFull** : "true"). Esto permite una comprobación rápida de unirse a los jugadores si la sesión se puede unir.

El árbitro o el host también debe mantener una propiedad personalizada que indica qué miembros, por índice, se han agotado (por ejemplo, **TimedOutPlayers** : “3, 5, 9”). Esto permite que los jugadores nuevos identificar correctamente los miembros de la sesión actual.

### <a name="notifying-waiting-players"></a>Notificar a los jugadores en espera

Un título puede admitir los reproductores de espera debido a que administra una lista del Reproductor en cola además de la entidad de juego. Cuando un jugador se conecta al host y la sesión de juego está lleno, el Reproductor se agrega a la lista de espera interno en el host. El Reproductor en cola no combina con la sesión de juego, pero permanece en la parte del juego. Para minimizar el tráfico de red, el Reproductor espera desconecta el host en este momento.

Cuando se abre una ranura en la sesión de juego para que el Reproductor de espera, el árbitro o el host agrega una reserva para el Reproductor mediante una llamada a la **AddMemberReservation** método. Todavía en este momento el Reproductor de espera no es consciente de esta reserva, por lo tanto, el árbitro o el host tiene que llamar a la **PullReservedPlayersAsync** método. Esto hace que una interfaz de usuario del sistema o **GameSessionReady** notificación para todos los reproductores reservados, según la configuración de notificación del sistema y el estado del título de foco. El nuevo jugador se vuelve a conectar al host cuando esta notificación se recibe y une a la sesión.

Como alternativa, un Reproductor puede permanecer conectado al host y unirse a la sesión cuando el host de alertas del Reproductor que está disponible una ranura. Sin embargo, en este flujo no se puede mostrar una notificación del sistema de la interfaz de usuario fuera el título.

**Nota:** Los jugadores se quitarán automáticamente parte del juego si el título se suspende o finaliza y ninguna otra recibirán notificaciones.

<a name="client-multiplayer-flow"></a>Flujo de varios jugadores de cliente
-----------------------
![](../../images/whitepapers/gamesessionvisibility_image1.png)
![](../../images/whitepapers/gamesessionvisibility_image2.png)
