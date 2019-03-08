---
title: Directorio de sesión de varios jugadores
description: Obtenga información sobre el directorio de sesión de varios jugadores en vivo de Xbox (MPSD).
ms.assetid: a9b029ea-4cc1-485a-8253-e1c74184f35e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, mpsd, directorio de sesión de varios jugadores.
ms.localizationpriority: medium
ms.openlocfilehash: 1681fe59533ebaf0db197efb95ca46b828846f5d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662550"
---
# <a name="multiplayer-session-directory-mpsd"></a>Directorio de sesión multijugador (MPSD)

Este artículo contiene los siguientes apartados:

* Información general MPSD
* MPSD Cambiar control de notificación y detección de la desconexión
* MPSD identificadores de sesiones
* Sincronización de actualizaciones de la sesión
* Una llamada a MPSD
* Plantillas de sesión MPSD
* Explorador de la sesión de varios jugadores

## <a name="session-overview"></a>Información general de la sesión

### <a name="what-is-mpsd"></a>¿Qué es MPSD?

Directorio de sesión de varios jugadores (MPSD) es un servicio que funcione en la nube de Xbox Live que centraliza la creación de metadatos de un juego multijugador sistema entre varios clientes. Está incluido en el **MultiplayerService clase**.

MPSD permite títulos compartir la información básica necesaria para conectarse a un grupo de usuarios. Garantiza que la funcionalidad de la sesión sea coherente y sincronizados. Coordina con el sistema operativo shell y la consola en envío/aceptar invitaciones y en la que se combina a través de la tarjeta de jugador.


### <a name="mpsd-sessions"></a>Sesiones MPSD

Una sesión MPSD está representada por la **MultiplayerSession clase** como el escenario en el que uno o más empleados usan un juego. MPSD almacena una sesión como un documento JSON seguro que se encuentran en la nube de Xbox Live. En concreto, una sesión MPSD tiene las siguientes características:   Se crea y administra los títulos.

-   Tiene un URI único. Para obtener más información, consulte **URI del directorio de sesión**.
-   Permite la conectividad entre usuarios, denominados a miembros de la sesión.
-   Almacena los datos útiles para permitir juego reproducción, por ejemplo, los atributos y los miembros, configuración del juego, información y servidor de juegos de arranque.

MPSD admite distintas variaciones de sesión para su uso en la configuración de juegos multijugador. Todas las sesiones contiene identificadores de usuario de Xbox de jugadores (XUIDs) y proteger los datos de dirección de asociación de dispositivo.

-   Sesión de juego, usada como patrón para el juego. Una sesión puede ser peer-to-peer, del mismo nivel para el host, del mismo nivel a servidor o una combinación de estos tipos.
-   Sesión de vale, una sesión de auxiliar que se usa para realizar el seguimiento del estado de una coincidencia durante contactos. A menudo también es una sesión introductoria y, a veces puede ser una sesión de juego. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).
-   Sesión de destino, una sesión de aplicación auxiliar creada durante los contactos para representar el juego coincidente. Casi siempre es también una sesión de juego. Consulte [SmartMatch contactos](smartmatch-matchmaking.md).
-   Sesión introductoria, utilizada para dar cabida a los jugadores invitados que están esperando para unirse a una sesión de una sesión de aplicación auxiliar. Muchos títulos crearán una sesión introductoria y una sesión de juego. Para obtener más información, consulte **encargados de administrar en su título**.

## <a name="mpsd-change-notification-handling-and-disconnect-detection"></a>MPSD Cambiar control de notificación y detección de la desconexión

MPSD permite a los clientes para conectarse a él utilizando el socket web de servicio de actividad en tiempo real. Se utiliza para la conexión:

-   Envío de notificaciones breves (derivaciones hombro) cuando se producen cambios en la sesión, en función de las suscripciones de eventos que inician los títulos.
-   Detectar las desconexiones de usuario.
-   Conjunto de usuarios como inactiva y quitarlas posteriormente en la sesión, en función al desconectarse de detección.


### <a name="making-user-connections"></a>Las conexiones de usuario

La biblioteca XSAPI administra la conexión entre el cliente y MPSD. La primera llama a título el **MultiplayerService.EnableMultiplayerSubscriptions método**. Este método indica XSAPI que el cliente intenta usar una conexión de la actividad en tiempo real para fines de varios jugadores. A continuación, cuando el título realiza su primera llamada a la **MultiplayerService.WriteSessionAsync método** o **MultiplayerService.WriteSessionByHandleAsync método**, con el usuario actual establecido en activo estado de una conexión se crea y se enlazó a MPSD.

| Nota                                                                                                                         |
|-------------------------------------------------------------------------------------------------------------------------------------------|
| Para habilitar las notificaciones de la sesión y detectar las desconexiones de que la plantilla de sesión tiene que establecer el connectionRequiredForActiveMembers en true. |

| Nota                                                                                                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| En versiones anteriores de XSAPI, llaman a los títulos de los **RealTimeActivityService.ConnectAsync método** para crear conexiones de usuario con el servicio de la actividad en tiempo real. Para el participan varios jugadores de 2015, este método no hace nada y se crea la conexión a petición. |

### <a name="subscribing-for-session-changes"></a>Suscribirse a los cambios de la sesión

MPSD usa una derivación de"consentimiento temporal" como una notificación ligera que ha cambiado algo de interés. El título debe recuperar el recurso modificado para determinar la naturaleza exacta del cambio. Con las suscripciones habilitadas, el título puede suscribirse para hombro pulsa en cambios de la sesión con una llamada a la **MultiplayerSession.SetSessionChangeSubscription método**. Vea [Cómo: Suscríbase para recibir notificaciones de cambio de sesión MPSD](multiplayer-how-tos.md).


### <a name="handling-shoulder-taps"></a>Controlar el hombro pulsa

Cuando un cambio en una sesión coincide con la suscripción del título para la sesión, MPSD notifica el título del cambio mediante la **RealTimeActivityService.MultiplayerSessionChanged eventos**. El título debe recuperar la sesión y comparar la versión recuperada de la sesión con la anterior vista almacenada en caché y realizar acciones de forma adecuada.


### <a name="handling-notifications-of-connection-state-changes"></a>Controlar las notificaciones de cambios de estado de conexión

El título también puede recibir notificaciones sobre cambios en el estado de la conexión a MPSD. Estos cambios de la señal de dos eventos: ** RealTimeActivityService.MultiplayerSubscriptionsLost evento **: se desencadena cuando se pierde la conexión del título a MPSD mediante el servicio de la actividad en tiempo real. Cuando se produce este evento, el título debe apagar el participan varios jugadores.
-   ** Evento RealTimeActivityService.ConnectionStateChanged **: desencadenadas tras un cambio temporal en el estado de conexión del título para el servicio de la actividad en tiempo real. El título no es necesario realizar ninguna acción después de recibir este evento, pero puede ser útil utilizar el evento con fines de diagnóstico.


### <a name="disconnecting-clients"></a>Se desconectan los clientes

Los clientes para el título de desconexión MPSD cuando el título deshabilita las notificaciones con una llamada a la **RealTimeActivityService.DisableMultiplayerSubscriptions método**. Poco después de esta llamada, el **MultiplayerSubscriptionsLost** desencadena el evento, que indica que un cliente se desconectó MPSD.

| Nota                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| En versiones anteriores de varios jugadores, denominan títulos la ** método RealTimeActivityService.Disconnect ** para desconectarse del servicio de la actividad en tiempo real. Para el participan varios jugadores de 2015, este método no hace nada. Se produce la desconexión después automáticamente **DisableMultiplayerSubscriptions** se llama, si no hay ningún usuario de la conexión de socket web, por ejemplo, una suscripción de servicio de la actividad en tiempo real de presencia. |


### <a name="disconnect-detection"></a>Detección de la desconexión

MPSD usa su desconectar la característica de detección para averiguar rápidamente cuando un usuario se desconecta de manera inadecuada. Podría producirse una desconexión incorrectos cuando se produce un error en la red de un jugador, o cuando se bloquea un título. MPSD cambia estado desconectado del Reproductor de activo a inactivo y notifica a los otros miembros de la sesión del cambio según corresponda, en función de las suscripciones de los miembros a la sesión.

## <a name="mpsd-handles-to-sessions"></a>MPSD identificadores de sesiones

Un identificador de sesión MPSD es una referencia abstracta e inmutable a una sesión que también puede contener datos con tipo adicionales. Es similar a un identificador de archivo. Todos los identificadores de tengan un identificador (GUID) identificador y una referencia de la sesión completa que consta de la configuración del servicio de Id. (¿SCID), plantilla de sesión y nombre de la sesión. No se puede actualizar un controlador, pero puede crear, leer y eliminar.

| Nota                                                                                                                                   |
|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| Puede señalar un identificador a una sesión que no existe. Creación de un identificador con un nombre de sesión inexistente no provoca una nueva sesión que se va a crear. |


### <a name="handle-types"></a>Tipos de identificador

Varios jugadores 2015 admite los tipos de identificador se describe en esta sección.

#### <a name="invite-handle"></a>Identificador de invitación

Un identificador de invitación representa una invitación (invitación) a una persona específica. Datos específicos del tipo incluyen la persona de origen, la persona de destino y cadena de contexto que describe la invitación, por ejemplo, un modo de juego específico.

Un identificador de invitación concede acceso de lectura y escritura a una sesión abierta. Si se cierra la sesión, el identificador concede acceso a la sesión de solo lectura.

| Nota                                                |
|------------------------------------------------------------------|
| MPSD puede crear una invitación, incluso si la sesión está completa o cerrada. |


#### <a name="activity-handle"></a>Identificador de actividad

Un identificador de actividad indica lo que hace un usuario en este momento. La actividad del usuario representada por la **MultiplayerActivityDetails clase**.

| Nota                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Un identificador de actividad puede también se elimine explícitamente, pero solo el título del propietario de un usuario específico. Esta eliminación se realiza mediante el **MultiplayerService.ClearActivityAsync método**. |


### <a name="creating-an-invite-handle"></a>Creación de un identificador de invitación

Para crear un identificador de invitación, las llamadas de título del **MultiplayerService.SendInvitesAsync método**. El método envía una invitación a los usuarios especificados en el formulario de una interfaz de usuario del sistema que los destinatarios pueden actuar en para aceptar la invitación.


### <a name="creating-an-activity-handle"></a>Creación de un identificador de actividad

Para crear un identificador de actividad, las llamadas de título del **MultiplayerService.SetActivityAsync método**. MPSD establece el Id. del identificador nuevo como actividad dependiente del miembro de la sesión. Si hubiera una actividad enlazada anterior, MPSD elimina el identificador correspondiente. Cuando el miembro activo pasa a estar inactivo o abandona la sesión, MPSD elimina el identificador de actividad dependiente.


### <a name="using-handles"></a>Uso de controladores

El título utiliza identificadores cuando un usuario acepta una invitación (identificador de invitación) y cuando un usuario une a la actividad actual de un amigo (identificador de actividad). En ambos casos, el título debe:

1.  Obtenga el Id. del identificador el título de los parámetros de activación.
2.  Crear un objeto de sesión local MPSD y unirla como activa.
3.  Escribir la sesión, pasando el identificador apropiado.

## <a name="synchronization-of-session-updates"></a>Sincronización de actualizaciones de la sesión

Dado que una sesión es un recurso compartido que se puede crear o actualizar cualquiera de sus miembros, es posible para las escrituras en conflicto. Esto puede provocar resultados inesperados, por ejemplo, un título puede sobrescribir inadvertidamente los cambios realizados por otro título. Enfoque del MPSD para resolver estos conflictos es admitir la simultaneidad optimista y un patrón de lectura-modificación-escritura.

Sincronización de actualizaciones de la sesión por MPSD usar dos modelos de implementación de alto nivel relacionada:

-   Arbiter actualiza compartidas partes de la sesión. Si la implementación implica a un árbitro único, puede evitar usar actualizaciones sincronizadas para la mayoría de las operaciones de escritura. El título puede evitar la sincronización para (1) cualquier actualización que realiza el árbitro a compartido partes de la sesión, a menos que están relacionadas a la comunicación de identidad de arbiter y (2) todas las actualizaciones que realiza un título al área de miembro dentro de la sesión.

| Nota                                                                                                                                                                                                                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Aunque los tipos de actualización anterior no necesitan sincronización, es importante sincronizar las actualizaciones de la ** MultiplayerSessionProperties.HostDeviceToken propiedad **. Esa propiedad se utiliza para comunicar la identidad de arbiter, por ejemplo, como parte de la migración de arbiter. |

-   Todos los clientes actualizan compartidas partes de la sesión. En este caso, todas las actualizaciones a compartido partes de la sesión deben estar sincronizadas. Sin embargo, los títulos todavía pueden escribir en sus áreas de miembro sin sincronización.


### <a name="session-update-synchronization-using-the-multiplayer-winrt-api"></a>Sincronización de actualizaciones de sesión mediante la API de WinRT multijugador

Los siguientes métodos de API de WinRT multijugador implementan simultaneidad optimista:

-   **Método MultiplayerService.WriteSessionAsync**
-   **Método MultiplayerService.WriteSessionByHandleAsync**
-   **Método MultiplayerService.TryWriteSessionAsync**
-   **Método MultiplayerService.TryWriteSessionByHandleAsync**

Cada método de escritura acepta un **MultiplayerSessionWriteMode enumeración** valor. Pasar el valor SynchronizedUpdate hace uso de simultaneidad optimista para actualizaciones.

Otros valores de la enumeración ayudan a resolver los conflictos potenciales tras la creación inicial de una sesión. Cualquier escritura a una parte de la sesión MPSD que potencialmente puede escribirse en otro título debe usar una actualización sincronizada. Sin embargo, no todas las escrituras se deben proteger.

Si el título intenta escribir el objeto de sesión local en MPSD mediante uno de los métodos de la sesión de escritura y recibe un código de estado 412/HTTP, debe actualizar la copia local mediante la emisión de un **MultiplayerService.GetCurrentSessionAsync método**llamada para obtener la última versión del servidor de la sesión antes de volver a intentar la operación de escritura. En caso contrario, continúa el documento de sesión local para que contenga los datos incorrectos y las llamadas a escribir la sesión seguirán produciendo errores.

| Nota                                                                                                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si el título está usando uno de los **TryWrite\***  métodos, se devuelve la sesión actualizada en el caso de un código de estado HTTP/412. Este comportamiento evita la necesidad de llamar a **GetCurrentSessionAsync**. |

Cuando el título en el que se llama a uno de los métodos de la sesión de escritura, se podría devolver una versión actualizada de la sesión. Si es así, el título debe cambiar su copia en caché local a esta nueva versión de una manera segura para subprocesos.


### <a name="session-update-synchronization-using-the-multiplayer-rest-api"></a>Sincronización de actualizaciones de sesión mediante la API de REST para varios jugadores

MPSD admite simultaneidad optimista en actualizaciones de la sesión a través de la funcionalidad REST mediante el encabezado "if-match" de HTTP con la configuración de ETag y el patrón de lectura-modificación-escritura. El valor de ETag que se pasan en la solicitud de escritura debe ser lo que devuelve el MPSD con la solicitud de lectura anterior.

## <a name="calling-mpsd"></a>Una llamada a MPSD

Hay dos maneras para el título tener acceso a MPSD para poder usar el sistema de varios jugadores y contactos:
-   Se recomienda. Utilice la API de WinRT para varios jugadores, que contiene clases que actúan como contenedores para la funcionalidad de RESTful. Consulte **Microsoft.Xbox.Services.Multiplayer Namespace**. Para contactos SmartMatch, utilice los contactos API de WinRT, representado por la **Microsoft.Xbox.Services.Matchmaking Namespace**.
-   Usar llamadas directas de HTTP estándares a los varios jugadores y contactos las API de REST, incluido en el **Xbox Live servicios RESTful referencia**. Se describen los URI aplicables en **URI del directorio de sesión** (para varios jugadores) y **contactos URI** (para contactos). Los objetos relacionados de JSON se describen en la **referencia de objeto de JavaScript Object Notation (JSON)**.


### <a name="using-the-multiplayer-winrt-api-to-call-mpsd"></a>Mediante la API de WinRT multijugador para llamar a MPSD

La manera recomendada para llamar a MPSD es usar la API de WinRT para varios jugadores y la API de WinRT de contactos.

| Nota                                                                                             |
|---------------------------------------------------------------------------------------------------------------|
| Los ejemplos XDK están escritos usando la participan varios jugadores y WinRT APIs de contactos y los demás elementos de XSAPI. |

Uso de código de contenedor para la funcionalidad subyacente de REST permite adoptar un enfoque más tradicional a métodos de API del lado cliente sin tener que controlar el tráfico HTTP para cada llamada. Un archivo binario y un origen para XSAPI se suministran con el XDK. Puede usar el archivo binario directamente, o modifique el origen y se incorporará el título según sea necesario.


### <a name="using-the-multiplayer-rest-api-to-interact-with-mpsd"></a>Uso de la API de REST de varios jugadores para interactuar con MPSD

El título o su servicio, puede usar las llamadas HTTP estándar para la API de REST para varios jugadores y la API de REST de contactos. Cuando se usa la funcionalidad REST directamente, los problemas del autor de la llamada DELETE, PUT, POST y obtención llamadas en el URI del directorio de sesión para la mayoría de las acciones. En una solicitud PUT, el cuerpo de solicitud se combina con la sesión existente. Si no hay ninguna sesión existente, el cuerpo de solicitud se usa para crear una nueva sesión, junto con la plantilla de sesión almacenada en [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard). Todos los campos son opcionales y diferencias solo deben especificarse. Por lo tanto, {} es una solicitud PUT válida con deltas de cero.

Para realizar una solicitud PUT hipotética que devuelve el resultado de la fusión mediante combinación sin que afecte a la copia del servidor oficial de la sesión, puede anexar la cadena de consulta "? nocommit = true" a la solicitud PUT.

Las solicitudes y respuestas para los métodos de API de REST de contactos y varios jugadores son documentos JSON. Una estructura de solicitud de sesión de varios jugadores, consulte **MultiplayerSessionRequest (JSON)**. Se muestra una estructura de la respuesta asociada en **MultiplayerSession (JSON)**. La estructura de la respuesta los miembros de la sesión de marcos como una lista vinculada y rellena de otras propiedades de solo lectura de la sesión y sus miembros.


### <a name="querying-for-sessions-and-session-templates-rest"></a>Consulta de las sesiones y plantillas de sesión (REST)

Los títulos pueden consultar información de sesión en la configuración del servicio y los niveles de la plantilla de sesión. Este tema describen las consultas que utilizan la API de REST para varios jugadores.


#### <a name="query-for-basic-session-information"></a>Consultar información de sesión básica

Puede configurar consultas para obtener información de sesión básica con el directorio de sesión y el URI de contactos. El resultado de una consulta es una matriz JSON de la sesión de referencias, con algunos datos de sesión incluyen en línea. De forma predeterminada, una consulta recupera hasta 100 sesiones no privados.

| Nota                                                          |
|----------------------------------------------------------------------------|
| Cada consulta debe incluir un filtro de palabras clave, un filtro XUID o ambos. |


#### <a name="query-for-session-templates"></a>Consulta para plantillas de sesión

Para recuperar la lista de plantillas de sesión para el ¿SCID, así como los detalles de una plantilla de sesión específico, use el método GET para uno de los URI siguientes:
-   **/serviceconfigs/{scid}/sessiontemplates**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}**


#### <a name="query-for-session-state"></a>Consultar el estado de sesión

Para consultar el estado de sesión, use el método GET para uno de estos URI:
-   **/serviceconfigs/{scid}/sessions**
-   **/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions**

## <a name="multiplayer-session-explorer"></a>Explorador de la sesión de varios jugadores

Explorador de sesión de varios jugadores es una herramienta integrada en MPSD para examinar las sesiones, plantillas de sesión y las cadenas de localización. La herramienta está diseñada únicamente para su uso para los espacios aislados de desarrollo.


### <a name="accessing-multiplayer-session-explorer"></a>Acceso a la sesión de varios jugadores Explorer

| Nota                                                                                                      |
|------------------------------------------------------------------------------------------------------------------------|
| Para usar la herramienta, debe iniciar sesión. La exploración se limita a las sesiones que tienen el usuario que inició sesión como miembro. |

Para obtener acceso a varios jugadores sesión de explorador, abra Internet Explorer en Xbox One, presione el **vista** botón y escriba <https://sessiondirectory.xboxlive.com/debug> en el **dirección** campo.

| Nota                                                                                                                                                                                |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Recibirá un código de estado 404 de HTTP o si se intenta acceder a la herramienta en el espacio aislado de venta directa. Para obtener más información sobre este código, consulte [códigos de estado de sesión de varios jugadores](multiplayer-session-status-codes.md). |


### <a name="using-multiplayer-session-explorer"></a>Mediante el Explorador de la sesión de varios jugadores


#### <a name="open-the-main-page"></a>Abra principal página

1.  Obtener acceso a la página principal de la herramienta. Muestra el contexto de seguridad (usuario con sesión iniciada y espacio aislado) y una lista de identificadores (SCIDs) configuración del servicio en el espacio aislado.
2.  Presione el **menú** botón Anclar esta página a la página principal para que no tiene que escribir el URI de cada vez.


#### <a name="display-available-sessions-and-templates"></a>Mostrar las plantillas y las sesiones disponibles

1.  Haga clic en un ¿SCID en la herramienta para mostrar una lista de sesiones en ese ¿SCID del que el usuario que inició sesión es un miembro.
2.  En esta misma página, puede haga clic en el ¿SCID y mostrar las cadenas de localización y plantillas de sesión en la configuración del servicio para el ¿SCID. Estos elementos se introducen a través de [XDP](https://xdp.xboxlive.com) o [centro de partners](https://partner.microsoft.com/dashboard).


#### <a name="display-the-full-contents-of-a-session"></a>Mostrar todo el contenido de una sesión

En el Explorador de la sesión de varios jugadores, haga clic en un nombre de sesión para mostrar todo el contenido de la sesión correspondiente.

La sesión, como se muestra en MPSD puede diferir de la respuesta a un método GET estándar para el URI de la sesión por diversas razones:

-   La llamada GET podría estar usando una versión anterior del contrato en el encabezado X-Xbl-contrato-Version. Explorador de sesión siempre muestra la sesión con la versión más actualizada del contrato.
-   Cuando una sesión se solicita normalmente mediante GET, transformaciones y efectos pueden activarse, por ejemplo, tiempos de espera ha expirado. Explorador de sesión muestra una instantánea de la sesión cuando se almacenan, sin ejecutar cualquier lógica, transformaciones o efectos secundarios.
-   Puesto que el campo de objeto JSON nextTimer se calcula al mismo tiempo como los efectos secundarios, no está presente en las sesiones MPSD.

## <a name="see-also"></a>Consulte también

[Información general de la sesión](mpsd-session-details.md)

[Códigos de estado de sesión de varios jugadores](multiplayer-session-status-codes.md)

[Cómo: Actualizar una sesión de varios jugadores](multiplayer-how-tos.md)

[Cómo: Unirse a una sesión MPSD de activación de un título](multiplayer-how-tos.md)

[Cómo: Suscríbase para recibir notificaciones de cambio de sesión MPSD](multiplayer-how-tos.md)

[SmartMatch contactos](smartmatch-matchmaking.md)