---
title: Reservas de XIM fuera de banda
description: Describe cómo usar Xbox integrado participan varios jugadores (XIM) como una solución de chat dedicado a través de las reservas de direcciones fuera de banda.
ms.assetid: 0ed26d19-defb-414d-a414-c4877bd0ed37
ms.date: 01/28/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores integrada de xbox, xim, chat
ms.localizationpriority: medium
ms.openlocfilehash: 82b38b8d0d94e4cccf501a101faee0e28a6b43c0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660840"
---
# <a name="using-xim-as-a-dedicated-chat-solution-via-out-of-band-reservations"></a>Usar XIM como una solución de chat dedicada a través de las reservas fuera de banda

La mayoría de las aplicaciones usan XIM para controlar todos los aspectos de unión de jugadores. Después de todo, el enfoque de ensamblar todas las funcionalidades necesarias para admitir escenarios multijugador comunes de forma integral es el motivo por el que se denomina "Xbox Integrated Multiplayer". Pero algunas aplicaciones que implementan sus propias soluciones de varios jugadores usa servidores dedicados de Internet también desea las ventajas del establecimiento de confianza, baja latencia y comunicación de bajo costo chat de punto a punto. XIM reconoce esta necesidad y admite un modo de extensión que aprovecha las ventajas de comunicación de simplificada del mismo nivel del XIM para aumentar la administración de jugadores externo que ocurre fuera de la API XIM. En lugar de mover a una red XIM a través de medios sociales o contactos, reproductores de mover con "reservas", garantizados marcadores de posición para usuarios concretos que intercambian "fuera de banda" a través del mecanismo de rendezvous Reproductor externo de la aplicación.

Aparte del proceso de mover, redes XIM administradas utilizando las reservas de direcciones fuera de banda eficazmente son los mismos que ninguna otra red XIM. Todas las funciones de comunicación funcionan de forma idéntica. Sin embargo, contactos y métodos de la API de detección de redes sociales necesariamente están deshabilitados para redes XIM administradas utilizando las reservas de direcciones fuera de banda, ya que entraría en conflicto con la implementación de la aplicación externa. No se puede enviar invitaciones de dicha red XIM, por ejemplo.

>XIM está optimizado para proporcionar una solución sencilla de extremo a otro. Por lo tanto, no todas las topologías complejas o escenarios pueden ser ideales para las reservas de direcciones fuera de banda. Si tiene preguntas sobre si o cómo aprovechar las características de comunicación del XIM, póngase en contacto con su representante de Microsoft.

Los temas siguientes describen cómo aprovechar las reservas de fuera de banda en XIM. Debido a las relativamente pocas diferencias de uso XIM "estándar" se describe en las secciones anteriores, se abrevia una explicación. Familiaridad con [XIM utilizando](using-xim.md) se recomienda.


## <a name="moving-to-a-new-out-of-band-reservation-xim-network"></a>Mover a una nueva red XIM de reserva fuera de banda

Para empezar a usar las reservas de direcciones fuera de banda, uno de sus participantes recopilados debe mover a una nueva red XIM creada en este modo. La selección de qué del mismo nivel que participan dispositivo depende de usted. Es posible que ya tiene un concepto de juegos host o servidor, que es una opción natural para iniciar el proceso, pero esto no es necesario. Se recomienda elegir un dispositivo que informa de un tipo de acceso de red de "Abrir" para lograr el menor tiempo de configuración de conectividad. Consulte la `Windows::Networking::XboxLive` documentación de la plataforma para obtener más información.

Mover a un XIM administrada a través de las reservas de direcciones fuera de banda de red se realiza al inicializar XIM y declarando los identificadores de usuario de Xbox local previsto tal como se muestra en el tutorial de uso XIM estándar, pero en lugar de llamar a un método como `xim::move_to_new_network()`, llame a `xim::move_to_network_using_out_of_band_reservation()` con un cadena de reserva null. Por ejemplo:

```cpp
 xim::singleton_instance().initialize(myServiceConfigurationId, myTitleId);
 xim::singleton_instance().set_intended_local_xbox_user_ids(1, &myXuid);
 xim::singleton_instance().move_to_network_using_out_of_band_reservation(nullptr);
```

El estándar `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, y `xim_move_to_network_succeeded_state_change` , a continuación, se proporcionarán con el tiempo durante el procesamiento de los cambios de estado en la típica `xim::start_processing_state_changes()` y `xim::finish_processing_state_changes()` bucle. Por ejemplo:

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
             MyHandlePlayerJoined(static_cast<const xim_player_joined_state_change *>(stateChange));
             break;
         }

         case xim_state_change_type::player_left:
         {
             MyHandlePlayerLeft(static_cast<const xim_player_left_state_change *>(stateChange));*
             break;
         }

         ...
     }
 }
 xim::singleton_instance().finish_processing_state_changes(stateChanges);
```

Una vez que el dispositivo inicial ha procesado los cambios de estado y tenía su player(s) movido correctamente a la red XIM, deben crear reservas de direcciones para los usuarios adicionales.

## <a name="adding-players-to-a-xim-network-managed-using-out-of-band-reservations"></a>Adición de los jugadores a una red XIM administrado utilizando las reservas de direcciones fuera de banda

Informe de redes XIM que se administran con las reservas de direcciones fuera de banda siempre un valor de `xim_allowed_player_joins::out_of_band_reservation` desde el `xim::allowed_player_joins()` método; se está cerrado todos los jugadores excepto aquellos con las zonas reservadas para sus identificadores de usuario de Xbox mediante una llamada a `xim::create_out_of_band_reservation()`. `xim::create_out_of_band_reservation()` toma una matriz de los usuarios, para que pueda crear tales reservas para los jugadores externamente recopilados a la vez o con el tiempo. Además, se omiten los usuarios que ya tienen los jugadores que participan en la red XIM, por lo que también puede proporcionar los identificadores de usuario Xbox adicionales como un conjunto de reemplazo completo o los cambios delta, lo que sea conveniente. En el siguiente ejemplo se asume que ya el conjunto totalmente recopilado de los punteros de cadena de los identificadores de usuario de Xbox en una matriz variable 'xboxUserIds' con 'xboxUserIdCount' número de elementos:

```cpp
 xim::singleton_instance().create_out_of_band_reservation(xboxUserIdCount, xboxUserIds);
```

Esto comenzará el proceso asincrónico de creación de un reservas para los identificadores de usuario especificados de Xbox. Cuando finalice la operación, XIM proporcionará un `xim_create_out_of_band_reservation_completed_state_change` que informa del éxito o error. Si es correcto, una cadena de reserva estará disponible para su sistema proporcionar a los identificadores de usuario de Xbox proporcionado a la operación. Las cadenas de reserva que se creó correctamente son válidas para sólo una determinada cantidad de tiempo. Se devuelve ese tiempo en `xim_create_out_of_band_reservation_completed_state_change`.

Con una cadena válida de reserva, el mecanismo externo "fuera de banda" utilizado para recopilar a los encargados de XIM puede utilizarse para distribuir la cadena a los enumerados. Por ejemplo, si usa el servicio de directorio de sesión de varios jugadores (MPSD) Xbox Live, puede escribir esta cadena como una propiedad de la sesión compartida personalizada en el documento de la sesión (Nota: la cadena de reserva siempre contendrá sólo un conjunto limitado de caracteres seguro para subprocesos para su uso en JSON sin necesidad de secuencias de escape o codificación Base64).

Una vez que los demás usuarios recuperar sus cadenas de reserva de la propiedad de documento de la sesión compartida, puede empezar a continuación, pasar a la red XIM usarlo como parámetro para `xim::move_to_network_using_out_of_band_reservation()`. En el siguiente ejemplo se da por supuesto que se ha extraído la cadena de reserva a una variable denominada 'reservationString'.

```cpp
xim::singleton_instance().move_to_network_using_out_of_band_reservation(reservationString);

```

Las operaciones de movimiento ejecuta de forma asincrónica al igual que para el dispositivo inicial que se especifica un puntero nulo para la cadena de reserva. Cambios de estado `xim_move_to_network_starting_state_change`, `xim_player_joined_state_change`, y `xim_move_to_network_succeeded_state_change` generará el movimiento. Cuando el movimiento se realiza correctamente, se agregarán los jugadores locales y remotos. Los dispositivos existentes en la red XIM se proporcionará un `xim_player_joined_state_change` para estos nuevos jugadores. En este momento, la comunicación de chat de voz y texto se habilita automáticamente entre los jugadores en distintos dispositivos en esta red XIM (donde permitan directiva y privacidad).

Si la operación falla debido a una cadena de reserva no es válido, XIM devolverá un `xim_network_exited_state_change` con el motivo `xim_network_exit_reason::invalid_reservation`. Esto puede ocurrir por diversos motivos:

1. El título intenta llamar a `xim::move_to_network_using_out_of_band_reservation()` en el dispositivo remoto antes de la `xim_create_out_of_band_reservation_completed_state_change` se devuelve en el dispositivo de host
1. El título intenta llamar a `xim::move_to_network_using_out_of_band_reservation()` en el dispositivo remoto después de la reserva.
1. El título intenta llamar a `xim::move_to_network_using_out_of_band_reservation()` en el dispositivo remoto sólo llamar varias veces mientras `xim::create_out_of_band_reservation()` una vez.

Independientemente del éxito o error, las reservas se consumen las operaciones de movimiento que usan. Por lo tanto, después de cada intento de uso, el host debe generar una nueva cadena de reserva mediante una llamada a `xim::create_out_of_band_reservation()` nuevo.

## <a name="configuring-chat-targets-in-a-xim-network-managed-using-out-of-band-reservations"></a>Configuración de destinos de chat en una red XIM administrado utilizando las reservas de direcciones fuera de banda

Redes XIM que se administran con las reservas de direcciones fuera de banda se comportan exactamente igual a las redes XIM tradicionales con respecto a la comunicación de chat. Para admitir competitivo escenarios, que se acaba de crear redes XIM se configuran automáticamente para admitir únicamente la charla con otros jugadores que son miembros del mismo equipo; es decir, el valor configurado notificados por `xim::chat_targets()` de forma predeterminada es `xim_chat_targets::same_team_index_only`. Esto se puede cambiar para permitir a todos para comunicarse con todos los demás (donde permitan privacidad y la directiva) mediante una llamada a `xim::set_chat_targets()`. En el ejemplo siguiente, se empieza a configurar todos los usuarios en la red XIM para utilizar un `xim_chat_targets::all_players` valor:

```cpp
xim::singleton_instance().set_chat_targets(xim_chat_targets::all_players);
```

Todos los participantes se les informa de que un destino nueva configuración surta efecto cuando les proporciona un `xim_chat_targets_changed_state_change`.

Cada player en redes XIM administradas utilizando las reservas de direcciones fuera de banda es inicialmente configurado con el mismo valor de índice del equipo, cero. Esto significa que una configuración de `xim_chat_targets::same_team_index_only` es indistinguible de `xim_chat_targets::all_players` de forma predeterminada. Sin embargo, puede cambiar el índice del equipo de un jugador local en cualquier momento mediante una llamada a `xim_player::xim_local::set_team_index()`. El ejemplo siguiente configura un puntero de Reproductor 'localPlayer' para tener un nuevo valor de índice del equipo de uno:

```cpp
 localPlayer->local()->set_team_index(1);
```

Todos los dispositivos se informaren de que el jugador tiene un nuevo valor de índice del equipo en vigor cuando les proporciona un xim_player_team_index_changed_state_change para que el Reproductor. Si la configuración de destino de chat está actualmente xim_chat_targets::same_team_index_only, otros jugadores con ese mismo índice nuevo equipo comenzará voz tener noticias suyas y se proporciona el chat de texto (privacidad y la directiva que permita) desde el Reproductor y viceversa cambiante. Los reproductores con el índice antiguo equipo dejará de intercambiar dicha comunicación chat. Si la configuración de destino de chat está actualmente xim_chat_targets::all_players, índice del equipo no tiene ningún impacto en la que pueden comunicarse con los que.

## <a name="removing-players-from-a-xim-network-managed-using-out-of-band-reservations"></a>Quitando los jugadores desde una red XIM administrado utilizando las reservas de direcciones fuera de banda

Administre la lista de jugadores externamente cuando se usa fuera de banda de reservas, naturalmente que deberá quitar los reproductores de la red XIM. La forma habitual es para que las aplicaciones aprovechar el mismo host de juego que se usó para crear la red XIM y posteriores reserva originalmente para también administrar la eliminación del Reproductor y hacerlo mediante una llamada a `xim::kick_player()`. Esto quita el Reproductor especificado y todos los jugadores en el mismo dispositivo de la red XIM. En el siguiente ejemplo se da por supuesto que la aplicación ha determinado que desea quitar el `xim_player` objeto apunta a la variable 'playerToRemove':

```cpp
xim::singleton_instance().kick_player(playerToRemove);
```

El Reproductor aplicable (o reproductores) se proporcionará cualquier necesaria `xim_player_left_state_change` para cada jugador y un `xim_network_exited_state_change` que indica que han tomado de la red. Como alternativa, puede llamar cada jugador `xim::leave_network()` para salir en su propias para el mismo efecto.

Tenga en cuenta que la comunicación de peer-to-peer XIM puede realizar su propia determinación en cualquier momento en que un jugador ha dejado la red XIM debido a problemas del entorno. La aplicación debe estar preparada para controlar un "no solicitado" `xim_player_left_state_change` y reconciliar cualquier discrepancia entre el estado del XIM y el esquema de administración externa del Reproductor de forma adecuada para su aplicación concreta.

## <a name="cleaning-up-a-xim-network-managed-using-out-of-band-reservations"></a>Limpiar una red XIM administrado utilizando las reservas de direcciones fuera de banda

Los reproductores que ya no se ha iniciado desde el XIM de red y desea volver al estado como si solo xim::initialize() y `xim::set_intended_local_xbox_user_ids()` hubiera llamado, puede empezar a hacerlo mediante la `xim::leave_network()` método:

```cpp
xim::singleton_instance().leave_network();
```

Esto inicia asincrónicamente desconectando los demás participantes. Esto hará que los dispositivos remotos que se proporcione un `xim_player_left_state_change` para el player(s) local y el dispositivo local se ofrecerán un `xim_player_left_state_change` por cada jugador, local o remoto. Cuando ha terminado todas desconexión estable, final `xim_network_exited_state_change` se proporcionará. A continuación, puede llamar la aplicación `xim::cleanup()` para liberar todos los recursos y volver al estado sin inicializar:

```cpp
 xim::singleton_instance().cleanup();
```

Invocar `xim::leave_network()` y esperando la `xim_network_exited_state_change` para salir de un XIM red correctamente siempre recomienda cuándo un `xim_network_exited_state_change` no se ha proporcionado. Una llamada a `xim::cleanup()` directamente puede causar problemas de rendimiento de comunicación para los demás participantes mientras obliga a los mensajes de tiempo de espera para el dispositivo que simplemente "desaparecido".
