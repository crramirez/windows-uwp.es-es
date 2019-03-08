---
title: Preguntas frecuentes y solución de problemas sobre varios jugadores de 2015
description: Preguntas más frecuentes acerca de 2015 de varios jugadores de Xbox Live y solución de problemas.
ms.assetid: 75823f10-b342-4e20-b885-e5ad4392bc3d
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores
ms.localizationpriority: medium
ms.openlocfilehash: 171d80f4fc925d95d80043f40bb387b045a4fe23
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625530"
---
# <a name="multiplayer-2015-faq-and-troubleshooting"></a>Preguntas frecuentes y solución de problemas sobre varios jugadores de 2015

-   Estoy desarrollando un título nuevo. ¿Los elementos de la API de varios jugadores debo usar?
-   ¿Cómo acceder a la nueva API de varios jugadores desde un servicio?
-   ¿Mi título puede suscribirse a los cambios de más de una sesión?
-   ¿Un usuario se quitará inmediatamente si éste pierde la conexión de red o desactiva la consola?
-   ¿Cómo averiguo qué ¿SCID, plantilla de sesión y espacio aislado para usar?
-   ¿Hay un ejemplo de un cuerpo de solicitud que puedo comparar a la para Mi título?
-   Obtengo un código HTTP/403 al llamar a MPSD.
-   Obtengo un código HTTP/404 al llamar a MPSD.
-   ¿Por qué estoy obteniendo un código HTTP/404 al llamar a **MultiplayerService.WriteSessionByHandleAsync** o **MultiplayerService.GetCurrentSessionByHandleAsync**?
-   Obtengo un código HTTP/412 al llamar a MPSD.
-   Obtengo un HTTP/400, 405, 409, 503, código de etc. al llamar a MPSD.
-   ¿Qué puede o debo cambiar en las plantillas de sesión para Mi título?
-   Obtengo un error que dice que mi sesión no se ha inicializado.
-   No se está creando mi sesión y estoy obteniendo un código HTTP/204.
-   ¿Cuándo debo sondear MPSD?
-   ¿Qué ocurre si un jugador que se ha reservado o invitados a la sesión no unirlo?
-   ¿Por qué una sesión creada no se encontraría por contactos?
-   ¿Qué es la diferencia clave entre la forma partes se utilizan correctamente por 2015 participan varios jugadores y la forma en que se usaron en el modo multijugador 2014?
-   Si una sesión de juego tiene ranuras Reproductor abierta y admite la combinación en curso, ¿por qué los usuarios no sería capaz de encontrar la sesión una vez que se ha iniciado?
-   ¿Si está abierta una sesión de juego, puede un usuario que solo se ha unido a un juego simplemente unirse a la sesión y comenzar a jugar sin tener que esperar para la reserva?
-   Cuando las sesiones de juego grandes están reproduciendo en Mi título, ¿por qué todos los miembros de la sesión no ven la notificación del sistema de invitación para jugar?
-   Veo un comportamiento incoherente juegos y ha recibido la información de activación de protocolo que hacen referencia a las características de fabricantes de juegos.
-   ¿Por qué veo la sintaxis para los documentos de la sesión de v105 en mi seguimientos aunque he configurado una plantilla de sesión v107?


### <a name="i-am-developing-a-new-title-which-multiplayer-api-elements-should-i-use"></a>Estoy desarrollando un título nuevo. ¿Los elementos de la API de varios jugadores debo usar?

Funcionalidad multijugador 2014 se seguirá aplicando para títulos existentes, pero los elementos de API asociados que probablemente ser en desuso. Se recomienda encarecidamente el uso de varios jugadores 2015 al preparar a los clientes para su lanzamiento en 2015.


### <a name="how-can-i-access-the-new-multiplayer-api-from-a-service"></a>¿Cómo acceder a la nueva API de varios jugadores desde un servicio?

Consulte [llamada MPSD](multiplayer-session-directory.md).


### <a name="can-my-title-subscribe-to-changes-for-more-than-one-session"></a>¿Mi título puede suscribirse a los cambios de más de una sesión?

Sí, un título puede suscribirse para recibir los cambios en un máximo de 10 sesiones por conexión.


### <a name="will-a-user-be-removed-immediately-if-heshe-loses-a-network-connection-or-turns-off-the-console"></a>¿Un usuario se quitará inmediatamente si éste pierde la conexión de red o desactiva la consola?

La conexión de socket web permite MPSD rápidamente detectar la desconexión del cliente y configurar al cliente en inactivo. Tan pronto como expire el tiempo de espera inactivo de eliminación, se quitan los miembros de la sesión. Para obtener más información, consulte [tiempos de espera de sesión](mpsd-session-details.md).


### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>¿Cómo averiguo qué ¿SCID, plantilla de sesión y espacio aislado para usar?

Si no estaba implicado en el registro inicial de su título, cuando se creó esta información, no tendrá acceso a esta información. Póngase en contacto con su administrador de cuentas de desarrollador, quién puede obtener estos datos para usted.


### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-to-the-one-for-my-title"></a>¿Hay un ejemplo de un cuerpo de solicitud que puedo comparar a la para Mi título?

Ver la estructura de la solicitud en **MultiplayerSessionRequest (JSON)**


### <a name="i-am-getting-an-http403-code-when-calling-mpsd"></a>Obtengo un código HTTP/403 al llamar a MPSD.

Esto suele ser un permisos o problema de ámbito. Recopilar seguimientos de Fiddler para obtener más información y, a continuación, compruebe los mensajes se devuelven como parte del cuerpo de los mensajes comunes 403 HttpResponse:

-   No se puede tener acceso a la configuración de servicio solicitado.
    1.  Confirme que está utilizando una cuenta que tenga acceso al espacio aislado.
    2.  Confirme que está en el espacio aislado correcto.
     3.  Si está usando la autenticación de certificado y recibe este error, póngase en contacto con su administrador de cuentas de desarrollador.   No se puede tener acceso a la sesión solicitada. El usuario que realiza la llamada debe tener privilegios para varios jugadores y sesiones privadas solo se pueden leer los miembros de la sesión.

    No tiene la capacidad para ver la sesión, posiblemente porque está intentando obtener acceso a una sesión que tiene una visibilidad de privado. Si se establece la visibilidad en privado, solo los miembros de esa sesión pueden leer el documento de la sesión.

| Nota                                                                                                                                                  |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Un usuario debe tener una cuenta de oro para registrar una nueva sesión. Sin privilegios de cuenta Gold para un usuario de reproducción, las solicitudes para registrar una nueva sesión devuelven/403 de HTTP. |

-   El cuerpo de solicitud no puede contener referencias de miembro existentes a menos que la autenticación principal incluye un servidor.

    No puede unirse a otro usuario a una sesión en el nombre de un usuario. Solo puede invitar a. Establece el índice en "reservar\_&lt;número&gt;" para invitar a un reproductor.


### <a name="i-am-getting-an-http404-code-when-calling-mpsd"></a>Obtengo un código HTTP/404 al llamar a MPSD.

Recopilar seguimientos de Fiddler para obtener más información y, a continuación, haga lo siguiente:

1.  El mensaje devuelto como parte del cuerpo de los 404 mensajes comunes HttpResponse, consulte:
    -   La configuración de servicio solicitado no es válido o no está configurada para sesiones. Asegúrese de que es correcta la ¿SCID que se va a usar.
    -   No se encontró la sesión solicitada. Asegúrese de que se crea la sesión y que la plantilla de sesión es correcta antes de recuperarlos. Puede crear una sesión con una llamada PUT.

2.  Compruebe el URI que se usa.
3.  Reiniciar la consola o pruebe con un nuevo usuario.
4.  Compruebe los foros para desarrolladores de juegos para el código de error u otras soluciones.
5.  Confirme que la sesión no se ha eliminado como resultado está vacío. Sesiones pueden quedar vacías porque el tiempo de espera de los usuarios. Cuando esto sucede, suele deberse a que todos los miembros de la sesión se encuentran en un estado al que se aplica un tiempo de espera, como "listo" o "inactivo". Consulte [Estados de sesión de usuario](mpsd-session-details.md) para obtener más información de estado de usuario.


### <a name="why-am-i-getting-an-http404-code-when-calling-multiplayerservicewritesessionbyhandleasync-or-multiplayerservicegetcurrentsessionbyhandleasync"></a>¿Por qué estoy obteniendo un código HTTP/404 al llamar a **MultiplayerService.WriteSessionByHandleAsync** o **MultiplayerService.GetCurrentSessionByHandleAsync**?

Si el título tiene acceso a una sesión MPSD por identificador en respuesta a una activación de protocolos de combinación que contiene un Id. del identificador, el Id. del identificador en la activación de protocolo podría ser obsoleto para uno de los siguientes motivos:

-   La vista de la interfaz de usuario en el shell de Xbox desde la que el título inicia la combinación es posible que no estén actualizada. Algunas vistas de interfaz de usuario, por ejemplo, tarjeta de perfil del usuario, no actualizan automáticamente identificadores de combinación mientras están abiertos. Para evitar recibir el código o 404 de HTTP, el título debe cerrar y volver a abrir la vista para actualizar los datos antes de la unión.
-   El usuario que se va a unir el título podría haber cambiado actividad las sesiones después de que el título había seleccionado de la operación de combinación desde la interfaz de usuario del shell de Xbox. Esta razón para el código o 404 de HTTP debe ser poco frecuente.

En cualquiera de estos casos, el código de título debe mostrar al usuario unirse a un mensaje de error que indica que la combinación no se pudo.


### <a name="i-am-getting-an-http412-code-when-calling-mpsd"></a>Obtengo un código HTTP/412 al llamar a MPSD.

La siguiente solicitud devuelve 412/HTTP si la sesión ya existe:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-None-Match: *


La siguiente solicitud devuelve 412/HTTP si el valor de etag de la sesión no coincide con el encabezado If-Match:

    PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1
    Content-Type: application/json
    If-Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D


Consulte [sincronización de actualizaciones de la sesión](multiplayer-session-directory.md) para obtener más información.


### <a name="i-am-getting-an-http400-405-409-503-etc-code-when-calling-mpsd"></a>Obtengo un HTTP/400, 405, 409, 503, código de etc. al llamar a MPSD.

Recopilar seguimientos de Fiddler para obtener más información y, a continuación, comprobar los mensajes se devuelven como parte del cuerpo HttpResponse. Esto debería proporcionarle información suficiente para identificar y corregir el error, o puede buscar los foros para desarrolladores de soluciones.

También puede obtener el cuerpo de respuesta si usas XSAPI, como se describe en [solución de problemas de la API de servicios de Xbox Live](../../using-xbox-live/troubleshooting/troubleshooting-the-xbox-live-services-api.md). Como alternativa, puede usar el código la **XboxLiveContextSettings.EnableServiceCallRoutedEvents** propiedad para enviar la salida para el título de la interfaz de usuario.


### <a name="what-can-or-should-i-change-in-the-session-templates-for-my-title"></a>¿Qué puede o debo cambiar en las plantillas de sesión para Mi título?

Plantillas de sesión son patrones para las sesiones, y no se puede invalidar las constantes que ya está establecidas en las plantillas. Sin embargo, puede agregar nuevas propiedades a las plantillas. Consulte [MPSD sesión plantillas](multiplayer-session-directory.md) para obtener más detalles.


### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Obtengo un error que dice que mi sesión no se ha inicializado.

Error de respuesta de ejemplo:

400 - \[ResponseBody\]: Esta sesión está configurada para la inicialización administrada que se requieren al menos 2 miembros que se inicie.

No se puede crear la sesión porque no hay suficientes reservas de miembros de sesión con el campo "initialize" establecido en true se incluyen en la solicitud. El código puede establecer este campo para un miembro con el *initializeRequested* parámetro para el **MultiplayerSession.AddMemberReservation** método o la **MultiplayerSession.Join** método.

Cuando se especifica la inicialización de la plantilla de sesión, asegúrese de que ese "initialize": "true" se establece para las reservas de miembro de datos lo suficiente para pasar los contactos de QoS. Para obtener más información, consulte [inicialización de la sesión de destino y QoS](smartmatch-matchmaking.md).


### <a name="my-session-is-not-being-created-and-im-getting-an-http204-code"></a>No se está creando mi sesión y estoy obteniendo un código HTTP/204.

Este código de estado indica que no hay usuarios se agregaron a la sesión cuando lo creó. Si no hay ningún usuario en la sesión cuando se crea y el tiempo de espera de sesión vacío es cero (valor predeterminado), no se crea la sesión.

Asegúrese de incluir al menos uno de los jugadores al crear una sesión. Para escenarios de servidor dedicado, obtenga un Reproductor de quién está intentando crear a una coincidencia o que necesita crear a una coincidencia y hacer a que el Reproductor del Reproductor inicial en la coincidencia. Como alternativa, puede cambiar o quitar el tiempo de espera de sesión vacío. Para obtener más información, consulte [tiempos de espera de sesión](mpsd-session-details.md).


### <a name="when-should-i-poll-mpsd"></a>¿Cuándo debo sondear MPSD?

Deben evitar los títulos de sondeo MPSD. Si un título debe detectar los cambios realizados en las sesiones MPSD, debe suscribirse a eventos de cambio de sesión. Para obtener más información, vea [Cómo: Suscríbase para recibir notificaciones de cambio de sesión MPSD](multiplayer-how-tos.md).


### <a name="what-happens-if-a-player-who-was-reserved-or-invited-to-the-session-does-not-join-it"></a>¿Qué ocurre si un jugador que se ha reservado o invitados a la sesión no unirlo?

Depende de si el título se está ejecutando cuando el Reproductor se notifica que la sesión de juego está listo. Si el Reproductor está en el título y no acepta la notificación de partida desde el título de la interfaz de usuario, es hasta el título para quitar el Reproductor de la sesión de juego con la **MultiplayerSession.Leave** método.

Si el título está restringido o no se está ejecutando, el shell proporciona una notificación que informa al Reproductor que está disponible una ranura. Si el Reproductor rechaza o pasa por alto la notificación del sistema, MPSD quita a esa persona de la sesión de juego.


### <a name="why-would-a-created-session-not-be-found-by-matchmaking"></a>¿Por qué una sesión creada no se encontraría por contactos?

En Xbox One, basta con crear una sesión no es suficiente para contactos encontrar la nueva sesión. Debe crear un vale de coincidencia para iniciar el anuncio de la sesión para el servicio. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).


### <a name="what-is-the-key-difference-between-the-way-parties-are-properly-used-by-2015-multiplayer-and-the-way-they-were-used-in-2014-multiplayer"></a>¿Qué es la diferencia clave entre la forma partes se utilizan correctamente por 2015 participan varios jugadores y la forma en que se usaron en el modo multijugador 2014?

En 2015 multijugador, la API de varios jugadores no define ninguna entidad juegos de nivel de sistema, solo las partes de chat. En lugar de usar partes de juego, el título utiliza las sesiones para controlar la unión, invita y características relacionadas con. Para varios jugadores de 2014, la API de varios jugadores en Xbox One forma destacada usa el concepto de fabricantes de juegos (**entidad** clase), que implementa de forma efectiva un vestíbulo de la combinación de nivel de sistema en lugar de juego invita.


### <a name="if-a-game-session-has-open-player-slots-and-supports-join-in-progress-why-would-users-not-be-able-to-find-the-session-once-it-has-started"></a>Si una sesión de juego tiene ranuras Reproductor abierta y admite la combinación en curso, ¿por qué los usuarios no sería capaz de encontrar la sesión una vez que se ha iniciado?

Cuando se inicia una sesión de juego, ya no esté implementado en el servicio. Si el Reproductor las ranuras estén disponibles dentro de la sesión y desea que el árbitro (host) atraer a nuevos actores, el árbitro debe crear un nuevo vale de coincidencia para la sesión en curso con el **MatchmakingService.CreateMatchTicketAsync** método. El vale anuncia la sesión de nuevo y encuentra más jugadores. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).


### <a name="if-a-game-session-is-open-can-a-user-who-has-just-joined-a-game-simply-join-the-session-and-start-playing-without-having-to-wait-for-the-reservation"></a>¿Si está abierta una sesión de juego, puede un usuario que solo se ha unido a un juego simplemente unirse a la sesión y comenzar a jugar sin tener que esperar para la reserva?

Sí. Esto es especialmente útil en casos donde el título usa varias sesiones para realizar el seguimiento de los subgrupos de jugadores dentro de una sesión de juego. El usuario unión puede unirse a la sesión que representa su grupo y, a continuación, debe unir la sesión de juego más grande.


### <a name="when-large-game-sessions-are-playing-in-my-title-why-arent-all-session-members-seeing-the-game-invite-toast"></a>Cuando las sesiones de juego grandes están reproduciendo en Mi título, ¿por qué todos los miembros de la sesión no ven la notificación del sistema de invitación para jugar?

Cuando el título agrega un usuario a la sesión a través de unión, el título siempre establece la **MultiplayerSessionMember.InitializeRequested** propiedad en true. Esto indica a MPSD a esperar el resto de los miembros de la sesión antes de pasar el juego fuera de la fase de inicialización. En caso contrario, los usuarios tienen una ventana en la que se va a unir muy breve y puede perder notificaciones del sistema y notificaciones de cambios de la sesión.


### <a name="i-am-seeing-inconsistent-game-behavior-and-have-received-protocol-activation-information-referencing-game-party-features"></a>Veo un comportamiento incoherente juegos y ha recibido la información de activación de protocolo que hacen referencia a las características de fabricantes de juegos.

Esto indica que se mezcla 2014 participan varios jugadores y la funcionalidad de varios jugadores de 2015. La API para varios jugadores 2015 nunca debe utilizarse con código escrito para 2014 participan varios jugadores.


### <a name="why-am-i-seeing-the-syntax-for-v105-session-documents-in-my-traces-although-i-have-configured-a-v107-session-template"></a>¿Por qué veo la sintaxis para los documentos de la sesión de v105 en mi seguimientos aunque he configurado una plantilla de sesión v107?

El MPSD realiza la conversión automática entre las versiones de documento de sesión basada en la solicitud de cliente. Actualmente, todas las API de servicio de Xbox usar v105 para las solicitudes a la MPSD. Esto puede provocar una sintaxis diferente entre plantillas de sesión v107 y v105 realiza un seguimiento, pero no tiene ningún impacto funcional. Plantillas de sesión deben configurarse como v107.

© 2015 Microsoft Corporation. Todos los derechos reservados.
Enviar comentarios en <https://forums.xboxlive.com/spaces/22/index.html>.
Versión: 2.0.90731.0
