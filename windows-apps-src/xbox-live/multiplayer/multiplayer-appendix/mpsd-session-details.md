---
title: Detalles de la sesión de varios jugadores
description: Obtenga información sobre los detalles de las sesiones de varios jugadores de Xbox Live.
ms.assetid: 5aeae339-4a97-45f4-b0e7-e669c994f249
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, varios jugadores 2015, sesión, mpsd
ms.localizationpriority: medium
ms.openlocfilehash: 3eb92af2d478fa41150f375dd19ef347d2b6f2e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616090"
---
# <a name="mpsd-session-details"></a>Detalles de la sesión MPSD

Este artículo contiene los siguientes apartados:
* Información general de la sesión
* Capacidades de la sesión
* Tamaño de la sesión
* Estados de sesión de usuario
* Visibilidad y Joinability
* Tiempos de espera de sesión
* Varios usuarios con sesión iniciada en una única consola
* Administración de ciclo de vida del proceso
* Limpieza de sesiones inactivas
* Sesión Arbiter

## <a name="session-overview"></a>Información general de la sesión

Una sesión de directorio de sesión de varios jugadores (MPSD) tiene un nombre de sesión y se identifica como una instancia de una plantilla de sesión, que es un documento JSON que proporciona una configuración predeterminada para la sesión. La plantilla es parte de una configuración de servicio con un identificador de configuración de servicio (¿SCID), que es un GUID. Esta plantilla se puede encontrar en [Xbox Developer Portal (XDP)](https://xdp.xboxlive.com) y [centro de partners](https://partner.microsoft.com/dashboard). Configuraciones de servicio son los recursos de desarrollador orientado usados para la ingesta, administración y la directiva de seguridad. Cuando se tiene acceso a una sesión a través de MPSD, la autorización de la entidad se realiza en la configuración del servicio según las directivas de acceso establecidas por el desarrollador a través de XDP o centro de partners. Cuando se carga la sesión después de que se autoriza el acceso a la configuración del servicio, se realizan comprobaciones de acceso secundaria, como la validación de la pertenencia de sesión, en el nivel de sesión.

En este tema se da por supuesto que la plantilla utiliza la versión de contrato 107, que es la versión utilizada por el actual MPSD para Xbox One. Si se han definido plantillas basadas en la versión del contrato 105 (idéntico a 104), debe cambiar estos elementos para admitir la versión 107. Para obtener instrucciones, consulte [problemas de migración comunes 2015 participan varios jugadores](common-issues-when-adapting-multiplayer.md).

| Importante                                                                                                                                                                                                                                                      |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| La funcionalidad que se establece a través de una plantilla no se puede cambiar a través de operaciones de escritura a la MPSD. Para cambiar los valores, debe crear y enviar una nueva plantilla con los cambios necesarios. Todos los elementos que no se establecen a través de una plantilla pueden cambiarse a través de operaciones de escritura a MPSD. |

### <a name="session-reference"></a>Referencia de la sesión

Cada sesión MPSD forma única se denomina mediante una referencia de la sesión, representada en la API de WinRT para varios jugadores por la **MultiplayerSessionReference clase**. La referencia de la sesión contiene estos valores de cadena:

-   Identificador de configuración de servicio (¿SCID)
-   Nombre de la plantilla de sesión
-   Nombre de la sesión

La referencia de la sesión se asigna en el URI para identificar las sesiones, tal como se muestra a continuación. En la siguiente asignación de ejemplo, "autoridad" es sessiondirectory.xboxlive.com.

```HTTP
https://{authority}/serviceconfigs/{service-config-id}/sessiontemplates/{session-template-name}/sessions/{session-name}
```

### <a name="elements-of-a-session"></a>Elementos de una sesión

Cada sesión contiene grupos de elementos que aplican la mutabilidad y reglas de seguridad, que varían según el elemento de la sesión, junto con información de mantenimiento de solo lectura (metadatos). En esta sección se describe los grupos de elementos de sesión incluidos en los archivos JSON para configurar la sesión y en el archivo JSON de la plantilla que elija.

| Nota                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Si usa contenedores de WinRT para una implementación de HTTP/REST, la sesión y la plantilla deben definir los objetos JSON que reflejan con precisión la funcionalidad de WinRT. |

Dentro de cada uno de los grupos de elementos, hay dos objetos internos:

-   Objetos del sistema, estos objetos tienen un esquema fijo que se aplican y se interpretan MPSD. Se valida y se combinan. Dado que MPSD define y sabe lo que significan, puede actuar en ellos. Para obtener la definición completa de cada uno de los objetos del sistema, consulte las referencias para el **MultiplayerSession clase** y las referencias de la **URI del directorio de sesión**.
-   Objetos personalizados--estos objetos son opcionales y no tienen esquema. Se utilizan para almacenar los metadatos de un juego multijugador. Puesto que MPSD no puede interpretar estos datos, no se actúa. Debe almacenarse datos juegos o información que se guarda en almacenamiento administrado por el título (TMS). Para obtener más información acerca de TMS, consulte **almacenamiento de Xbox Live título**.

Este es un ejemplo de un objeto JSON personalizado:
```JSON
    "custom": {
      "myField1": true,
      "myField2": "string",
      "myField3": 5.5,
      "myField4": { "myObject": null },
      "myField5": [ "my", "array" ]
    }
```

#### <a name="session-constants"></a>Constantes de sesión

Constantes de la sesión se establecen solo en tiempo de creación, el creador o la plantilla de sesión. El objeto /constants/system se utiliza para definir constantes para el sistema de varios jugadores, como se conoce a través de la MPSD. El contenedor de WinRT asociado a este objeto se representa mediante el **MultiplayerSessionConstants clase**.

El objeto /constants/system puede definir un número de elementos, incluido un objeto de funcionalidad, un objeto de métricas, un managedInitialization (versión de contrato de plantilla 104/105) o el objeto memberInitialization (versión de contrato 107), un peerToPeerRequirements objeto, un objeto peerToHostRequirements y un objeto measurementsServerAddresses.


#### <a name="session-properties"></a>Propiedades de la sesión

Utilice el objeto /properties/system para definir las propiedades de sesión para MPSD. El contenedor de WinRT asociado a este objeto es el **MultiplayerSessionProperties clase**. Propiedades de la sesión son miembros de la sesión que puede escribir en cualquier momento. Ejemplos de propiedades de la sesión en formato JSON son: joinRestriction, initializationSucceeded y el objeto de contactos. Para obtener un ejemplo del uso de este grupo de elementos, vea [inicialización de la sesión de destino y QoS](smartmatch-matchmaking.md).


#### <a name="member-constants"></a>Constantes de miembro

Establezca al miembro constantes en tiempo de combinación para cada miembro de la sesión. El objeto JSON es /members/ {index} / constantes/sistema. La clase de contenedor de WinRT que representa un miembro de la sesión es el **MultiplayerSessionMember clase**.


## <a name="member-properties"></a>Propiedades de miembro

Las propiedades de miembro son modificables sólo por un miembro de la sesión. Se establecen en el objeto de sistema/propiedades//members/ {index} y reflejar los elementos de la **MultiplayerSessionMember clase**. He aquí un ejemplo:

```
    {
      // These flags control the member status and "activeTitle", and are mutually exclusive (it's an error to set both to true).
      // For each, false is the same as not present. The default status is "inactive", i.e. neither present.
      "ready": true,
      "active": false,

      // Base-64 blob, or not present. Empty string is the same as not present.
      "secureDeviceAddress": "ryY=",

      // During member initialization, if any members in the list fail, this member will also fail.
      // Can't be set on large sessions.
      "initializationGroup": [ 5 ],

      // List of the groups I'm in and the encounters I just had.
      // An encounter is a brief interaction with a group. When an encounter is reported, it counts as retroactively joining the group 30 seconds ago and just now leaving.
      // Group names use the session name validation rules (e.g. case-insensitive).
      // On large sessions, groups are used to report who played with who (rather than just session membership). Members
      // that are active in at least one group together at the same time are counted as playing together.
      // Empty lists are the same as no value specified.
      // The set of encounters is a point-in-time property, so it's immediately consumed and will never appear on a response.
      "groups": [ "team-buzz", "posse.99" ],
      "encounters": [ "CoffeeShop-757093D8-E41F-49D0-BB13-17A49B20C6B9" ],

      // Optional list of role preferences the player has specified for role-based game modes.
      // All role names have to match across all members in the session. Role weights are
      // defined from 0-100.
      "RolePreference": { "medic": 75, "sniper": 25, "assault": 50, "support": 100 },

      // QoS measurements by lower-case device token.
      // Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
      // Metrics can be omitted if they weren't successfully measured, i.e. the peer is unreachable.
      // If a "measurements" object is set, it can't contain an entry for the member's own address.
      "measurements": {
        "e69c43a8": {
          "bandwidthDown": 19342,  // Kilobits per second
          "bandwidthUp": 944,  // Kilobits per second
          "custom": { }
        }

      // QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
      // If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
      "serverMeasurements": {
        "server farm a": {
          "latency": 233  // Milliseconds
        }
      },

      // Subscriptions for shoulder taps on session changes. The "profile" indicates which session changes to tap as well as other properties of the registration like the min time between taps.
      // The subscription is named with a title-generated GUID that is also sent back with the tap as a context ID.
      // Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
      // To remove a subscription, set its context ID to null.
      // (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
      // Can't be set on large sessions.
      "subscriptions": {
        "961dc162-3a8c-4982-b58b-0347ed086bc9": {
          "profile": "party",  // Or "matchmaking", "initialization", "roster", "queuehost", or "queue"
          "onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
        },
        "709fef70-4638-4b94-905b-24cb02706eb5": null
      }
    }
```

#### <a name="server-elements"></a>Elementos de servidor

Los servidores son que no sean usuarios que han incorporado o ha invitado a una sesión. Los objetos JSON asociados son/Servers / {nombre del servidor} / constantes/sistema y/Servers / {nombre del servidor} / propiedades /. Estos objetos son sólo los servidores que puede escribir.

| Nota                                                         |
|---------------------------------------------------------------------------|
| El objeto de sistema/Servers / {nombre del servidor} / constantes/no se usa actualmente. |


### <a name="session-configuration"></a>Configuración de sesión

Hay dos maneras de controlar la configuración de sesiones:

-   Usar plantillas de sesión ingeridas mediante XDP o centro de partners.
-   Utilizar llamadas a los varios jugadores y WinRT APIs o las API de REST de contactos. Todavía debe usar una plantilla, pero no tiene la plantilla que contiene los valores que desea configurar. Tenga en cuenta que el título no puede invalidar las constantes que ya está establecidas en la plantilla.

Se proporciona un documento independiente de JSON para definir la propia sesión. Además, el desarrollador debe implementar cualquier funcionalidad de contenedor de WinRT requerida para un determinado título. El contenido de los documentos JSON y cualquier código de contenedor de WinRT debe reflejar entre sí con precisión y debe reflejar la versión más reciente de contrato de plantilla.

El esquema para una sesión tiene una versión con la versión de la sesión (versión principal) y la revisión del protocolo (versión secundaria). Las versiones se combinan en el encabezado X-Xbl-contrato-Version como "100 \* principal + secundaria". Por ejemplo, un título v1.7 incluye el encabezado siguiente en cada solicitud REST, suponiendo que la última versión de contrato de la plantilla de 107: X-Xbl-Contract-Version: 107.

| Nota                                                                                              |
|----------------------------------------------------------------------------------------------------------------|
| Se recomienda para la mayoría de títulos (mediante XSAPI) para usar la versión del contrato 105 y versión de la plantilla de sesión 107. |


### <a name="session-templates"></a>Plantillas de sesión

Cada plantilla de sesión es un documento JSON, parte de la configuración del servicio, que define el marco de trabajo de la sesión que se está creando y proporciona constantes para la nueva sesión. Para obtener más información, consulte [MPSD sesión plantillas](../service-configuration/session-templates.md).

## <a name="session-capabilities"></a>Capacidades de la sesión

Capacidades son constantes en la sesión MPSD que configuración el comportamiento que debe aplicarse el MPSD a esa sesión. Utilice con más frecuencia XDP y centro de partners para definir las capacidades de la plantilla de sesión. Se establecen en el objeto /constants/system/capabilities. Si no hay capacidades son necesarias, usar un objeto vacío de capacidades.

| Nota                                                                                                       |
|-------------------------------------------------------------------------------------------------------------------------|
| Títulos casi nunca cambie o tener acceso a las capacidades de la sesión mediante la API de WinRT para varios jugadores o la API de WinRT de contactos. |

Capacidades de la sesión se representan mediante el **MultiplayerSessionCapabilities clase**. Son valores booleanos que indican lo que puede admitir la sesión:

-   Conectividad
-   Juego
-   Tamaño grande
-   Conexión necesaria para miembros activos

El **MultiplayerSessionConstants clase** define las siguientes propiedades que se refieren a las capacidades de sesión:

-   **CapabilitiesConnectivity**
-   **CapabilitiesGameplay**
-   **CapabilitiesLarge**

| Nota                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------|
| Si el título define una funcionalidad de sesión dinámica, la propiedad correspondiente se establece en true para las constantes de la sesión. |

## <a name="session-size"></a>Tamaño de la sesión

El tamaño de una sesión MPSD viene determinada por el número de miembros en esa sesión.


### <a name="maximum-session-size"></a>Tamaño máximo de la sesión

El tamaño máximo de una sesión es el número máximo de miembros de la sesión que y puede acomodar. Se representa mediante el **MultiplayerSessionConstants.MaxMembersInSession propiedad**. El tamaño máximo de miembros se establece en el objeto /constants/system.

El tamaño máximo de la sesión está entre 1 y 100 miembros de sesión y el valor predeterminado es 100 si no se establece en la creación. Si el tamaño necesario es superior a 100, la sesión se denomina una sesión "grande" y se establece de manera especial.

Establecer un tamaño máximo para una sesión puede provocar una ranura abierta aparezca como completa durante determinadas desconectar escenarios. Por ejemplo, si un jugador se desconecta como resultado un error de red o de alimentación, el retraso no se reflejan inmediatamente en la sesión. El miembro se establece en inactivo utilizando la característica de detección de desconexión descrita en [desconectar detección y control de notificación de cambio de MPSD](multiplayer-session-directory.md).

En comparación, una malla del mismo nivel que usa un latido para detectar una desconexión suele ser consciente de una desconexión en dos o tres segundos y puede abrir la ranura player inmediatamente. Sin embargo, el árbitro no puede quitar a otros miembros.

### <a name="large-sessions"></a>Sesiones de gran tamaño

Una sesión MPSD grande puede tener hasta 1000 miembros, pero tiene algunas características de sesión deshabilitados, por ejemplo, obtener una lista de todos los miembros. Largeness sesión está representado por la **MultiplayerSessionCapabilities.Large propiedad**. Esta propiedad se establece en true para indicar una sesión de gran tamaño y la capacidad de "grande" se indica en el objeto /constants/system/capabilities. 

## <a name="session-user-states"></a>Estados de sesión de usuario



MPSD define un estado de usuario como el estado de un usuario que se ha agregado a una sesión. Estados de usuario posible se definen mediante la **MultiplayerSessionStatus enumeración**. El usuario también se considera que tienen un estado de "disponible" antes de que se agrega a una sesión.

El **MultiplayerSession.SetCurrentUserStatus método** puede usarse para cambiar el estado de sesión de usuario. Este cambio se puede realizar para REST /members/ {index} / propiedades/sistema de configuración correctamente en el documento JSON de la sesión de juego.


### <a name="reserved-user-state"></a>Estado de usuario reservado

El usuario se encuentra en el estado de usuario reservado cuando el árbitro ha seleccionado el usuario para rellenar uno de los espacios abiertos dentro de la sesión. En este estado, el usuario aún no oficialmente aceptado la invitación a la sesión o unido a la sesión para iniciar la conexión con elementos del mismo nivel.


### <a name="active-user-state"></a>Estado de usuario activo

Cuando un usuario está en estado activo, el título ha unido a la sesión en nombre del usuario y el usuario está participando activamente en la sesión. El usuario continúa en este estado, siempre se ejecuta el juego.

Cuando se inició por primera vez un título, debe comprobar para ver si el usuario ya es miembro de las sesiones, normalmente comprobando el estado de sesión. Si el usuario es un miembro de la sesión, el título puede ir directamente a la partida y establecer a cualquier participantes miembros locales en el estado de usuario activo.

Un usuario debe permanecer en estado activo mientras se reproduce en la sesión. Si un usuario abandona la sesión a través de la interfaz de usuario en el juego, el usuario debe quitarse de la sesión con la **MultiplayerSession.Leave método**. Si el usuario es sólo temporalmente fuera del juego, como cuando está restringido el título, el título debe tener el usuario en el estado activo para un período de tiempo razonable. Es adecuado cambiar el estado de usuario en inactivo si el usuario no ha devuelto tras un período de tiempo especificado por el título.


### <a name="inactive-user-state"></a>Estado de usuario inactivo

En el estado inactivo, el usuario no esté implicado actualmente con el juego, pero aún tiene una ranura de guardado en la sesión. En otras palabras, el usuario es "no activo".

Es la consola del usuario que tiene la responsabilidad de establecer ese estado inactivo de un usuario a otro en la sesión. El árbitro no puede realizar esta acción. Escenarios de ejemplo en el que un usuario se coloca en el estado inactivo se incluyen:

-   El título recibe un evento de suspender.
-   El usuario ha estado inactivo (sin respuesta de entrada o controlador) durante un período de tiempo definido por el título. Se recomiendan dos minutos para red competitivo.
-   El título ha sido en modo restringido durante más de dos minutos, o durante un período definido por el título. Este período de tiempo de espera del modo restringido es la cantidad de tiempo para el que un usuario podría estar alejado el título con otras experiencias relacionadas con el título o una aplicación relacionada con el esperado.
-   El usuario se desconectó brusco de la sesión. Consulte [MPSD Cambiar control de notificación y detección de la desconexión](multiplayer-session-directory.md).

Si se inicia el título y el estado de usuario para un miembro de la sesión determinada se establece en inactivo, se ha suspendido el título o el usuario ha estado inactivo durante demasiado tiempo en la sesión. Dado que el título es iniciar de nuevo, la indicación es el usuario que desea continuar con la sesión de juego al que pertenece. Si el estado del usuario está activo en el inicio de título, esta situación es probablemente debido a una desconexión de red u otro escenario donde el título no pudo establecer el usuario a inactivo antes de interrumpirla. En ambos casos, el título debe intentan volver a conectar el usuario con el juego y los otros usuarios para seguir reproduciendo o quite el usuario de la sesión.

### <a name="user-state-when-the-session-is-over"></a>Es el estado cuando la sesión de usuario a través de

Cuando finaliza la sesión, se ha suprimido el juego. El título debe permitir a los usuarios quitar a sí mismas con la **MultiplayerSession.Leave método**. Las actividades de sesión asociadas a los usuarios se borran automáticamente cuando abandonan la sesión.

## <a name="visibility-and-joinability"></a>Visibilidad y Joinability

Acceso a la sesión se controla en el nivel de MPSD dos opciones de configuración: joinability de sesión y visibilidad de la sesión. Las recomendaciones de visibilidad y joinability en este tema se aplican para los escenarios más comunes de título. Si es posible, títulos deben seguir estos valores, y deben usar lógica en el título para tomar una decisión sobre si un nuevo jugador se admitió en una sesión autoritativo, final.


### <a name="session-visibility"></a>Visibilidad de la sesión

Visibilidad de la sesión se representa mediante una constante que se establece al crear la sesión. Normalmente se define en la plantilla de sesión y determina qué tipos de usuarios han leído y acceso de escritura a una sesión. Los valores posibles para la visibilidad de la sesión se definen mediante la **MultiplayerSessionVisibility enumeración**. Los valores permitidos para la constante de visibilidad de un archivo JSON están abiertos, visibles y privada.


#### <a name="recommended-game-session-visibility-open"></a>Recomienda la visibilidad de la sesión de juego: abrir

Las sesiones abiertas de juego no requieren las reservas del Reproductor, lo que simplifica el proceso de invitación. El árbitro no reservar los jugadores en MPSD después de que se ha enviado una invitación, pero sólo realiza un seguimiento invitados reproductores localmente. Por lo tanto, reproductores inmediatamente pueden conectar con el árbitro y determinar si debe participar en una sesión, se rechazan o debe esperar (si se admiten los jugadores en espera). El árbitro es la máxima autoridad y responde e indica el nuevo miembro a permanecer en o dejar la sesión.

Visibilidad de la sesión abierta de juego requiere que el Reproductor de invitado iniciar un título y conectar con el árbitro antes de que se ha tomado la decisión final. Es aceptable para mostrar un mensaje de error al usuario si se completa una sesión o si se ha rechazado una invitación.

Para establecer una conexión con el árbitro, se requiere una dirección de dispositivo segura. El **MultiplayerSessionProperties.HostDeviceToken propiedad** es utilizado para averiguar qué miembros de sesión es el árbitro actual de una sesión, y que proteger la dirección del dispositivo debe usar un Reproductor de invitado para la conexión.

### <a name="session-joinability"></a>Sesión Joinability

Joinability sesión determina qué tipos de los usuarios pueden participar en una sesión. Se puede establecer dinámicamente durante una sesión. Los valores posibles para joinability sesión son:

-   Ninguno (valor predeterminado), no existen restricciones sobre quién puede unirse a la sesión.
-   Locales, solo los usuarios locales pueden unirse a la sesión.
-   Seguido--sólo usuarios locales y los usuarios que van seguidos por otros miembros de la sesión pueden unirse a la sesión sin una reserva.

Un árbitro sesión puede crear una sesión privada a través de la configuración de joinability. Realizar joinability local o seguidos restringe el acceso a la sesión y lo convierte en privado.

Además, el árbitro debe realizar un seguimiento de sesión joinability para que se invita una sesión anterior se pueden rechazar en el nivel de host, si es necesario. Por ejemplo, si no han llegado los reproductores de invitado para participar en una sesión hasta que la sesión ya está completa, el árbitro puede indicar a los encargados de unión que se ha bloqueado la sesión y que necesitan para abandonar la sesión automáticamente.

## <a name="session-timeouts"></a>Tiempos de espera de sesión

Las sesiones se pueden cambiar por los temporizadores y otros eventos externos. Los tiempos de espera de sesión definen los períodos en que los miembros de la sesión pueden permanecer en Estados concretos antes de que se inactiva o se quita de la sesión automáticamente. MPSD también admite tiempos de espera para administrar la duración de la sesión.

| Nota                                                                                                                                                                                                                                                           |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Configuración de tiempo de espera se realiza en /constants/system/timeouts o dentro del objeto de inicialización administrada, versión de la plantilla contrato 104/105. Para la versión 107 o versiones posterior, la configuración se realiza individualmente en/constantes/sistema o dentro del objeto de inicialización administrada. |

Cuando expira un temporizador, MPSD actualizar la sesión no automáticamente y se notificará al árbitro en ese instante cualquier cambio. Los Estados de sesión y tiempo de espera solo se actualizan inmediatamente antes de una lectura o escritura se envía la solicitud. Actualización inmediata, se garantiza que los datos devueltos están la más actualizada.

| Nota                                                                                                          |
|----------------------------------------------------------------------------------------------------------------------------|
| Los tiempos de espera de sesión no se apilan, y se aplica solo uno para una transición de estado con cada miembro de la sesión en una actualización. |


### <a name="currently-defined-timeouts"></a>Tiempos de espera definido actualmente

Esta sección describen los tiempos de espera MPSD definidas actualmente. Se especifican todos los tiempos de espera en milisegundos. Un valor de 0 se permite e indica un tiempo de espera de inmediato. Se considera que un tiempo de espera sin un valor infinito. Dado que los tiempos de espera tienen valores predeterminados, debe especificar explícitamente null para un tiempo de espera infinito.
#### <a name="evaluationtimeout"></a>evaluationTimeout

Este tiempo de espera indica la cantidad de tiempo para un miembro de la sesión realizar y cargar la decisión de evaluación. Si no se recibe ninguna decisión, la Decisión se considera un error. Este tiempo de espera se coloca en el objeto de inicialización administrada.


#### <a name="inactiveremovaltimeout"></a>inactiveRemovalTimeout

Este tiempo de espera se establece para un miembro de la sesión que se ha unido a una sesión, pero no participa en el juego. El miembro se quita de la sesión después de 2 horas, de forma predeterminada.

| Nota                                                                      |
|----------------------------------------------------------------------------------------|
| Este tiempo de espera se designa el tiempo de espera inactivo para la versión de contrato de plantilla 104/105. |

En muchos casos, se recomienda establecer el tiempo de espera inactivo en 0, haciendo que cualquier usuario que se establece en el estado inactivo que se quitará inmediatamente de la sesión y la ranura correspondiente se borra. Este comportamiento es deseable para juegos multijugador más competitivas que, si un usuario ha quedado inactiva o se alcanza un estado inactivo, se puede agregar un nuevo reproductor rápidamente. Co-op o otros diseños de varios jugadores, podría el título para que los usuarios más tiempo para volver a conectarse si se desconecta o no participa en el título durante períodos de tiempo. Tenga en cuenta que no hay ninguna solución única se adapte a todos los escenarios de diseño.

#### <a name="jointimeout"></a>joinTimeout

Este tiempo de espera indica el número de milisegundos que un usuario tiene que unirse a la sesión. Se quitan las reservas de direcciones de los usuarios que no se pueden combinar. Este tiempo de espera se coloca en el objeto de inicialización administrada.


#### <a name="measurementtimeout"></a>measurementTimeout

Este tiempo de espera indica la cantidad de tiempo que un miembro de la sesión tiene que cargar las medidas. Un miembro que se produce un error al cargar las medidas se marca con un motivo del error de "tiempo de espera". Este tiempo de espera se coloca en el objeto de inicialización administrada.

| Nota                                                                                                                                                                              |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Durante los contactos, se aplica un tiempo de espera de 45 segundos para las mediciones de calidad de servicio. Por lo tanto, se recomienda el uso de un tiempo de espera de la medida que sea menor o igual a 30 segundos durante los contactos. |


#### <a name="readyremovaltimeout"></a>readyRemovalTimeout

Este tiempo de espera se establece para un miembro de la sesión que se ha unido a la sesión y está intentando obtener en el juego. Esto suele significar que el shell se ha unido el usuario en nombre del título y el título que se va a iniciar. El miembro es quitar de la sesión y se coloca en el estado inactivo después de 3 minutos, de forma predeterminada.

| Nota                                                          |
|----------------------------------------------------------------------------|
| Este tiempo de espera se designa el tiempo de espera listo para la versión de contrato 104/105. |


#### <a name="reservedremovaltimeout"></a>reservedRemovalTimeout

Este tiempo de espera se establece para un miembro de la sesión que se ha agregado a la sesión por otra persona, pero no se ha unido todavía la sesión. Se elimina la reserva y el miembro se considera inactivo cuando expira el tiempo de espera. El valor predeterminado es 30 segundos.

| Nota                                                             |
|-------------------------------------------------------------------------------|
| Este tiempo de espera se designa el tiempo de espera reservado para la versión de contrato 104/105. |


#### <a name="sessionemptytimeout"></a>sessionEmptyTimeout

Este tiempo de espera indica el número de milisegundos después de una sesión se queda vacía cuando se elimina. El valor predeterminado es 0.

| Nota                                                                 |
|-----------------------------------------------------------------------------------|
| Este tiempo de espera se designa el tiempo de espera sessionEmpty versión 104/105 del contrato. |


### <a name="session-timeout-example"></a>Ejemplo de tiempo de espera de sesión

1.  Se inicia una sesión con cuatro jugadores.
2.  Dos jugadores, A y B, se desconectan debido a un corte del suministro eléctrico. Su estado en el juego permanece activo.
3.  Los otros dos jugadores, C y D, salga correctamente con el **MultiplayerSession.Leave método**.
4.  La sesión permanece abierto, con jugadores A y B desconectado pero aún están en estado activo.
5.  Unos días más tarde, un reproductor devuelve e inicia el juego.
6.  Comprueba sesiones que A es miembro del juego del Reproductor (realiza una lectura) y busca la sesión huérfana desde hace unos días.
7.  La sesión realiza una comprobación de presencia en los dos jugadores que siguen en la sesión (A y B).
    1.  Dado que un reproductor se está ejecutando el título, se realiza correctamente la comprobación de presencia en un reproductor y estado activo del jugador en la coincidencia sigue siendo el mismo.
    2.  Reproductor B no se está ejecutando el título. Por lo tanto, la comprobación de la presencia de Reproductor B se produce un error y el servicio establece el estado del Reproductor B en inactivo. En este momento, se inicia el tiempo de espera inactivo para el Reproductor B.

8.  Un Reproductor cierra la sesión correctamente con el **deje** método.
9.  Expira el tiempo de espera inactivo para el Reproductor de B, que se quita de la sesión en la siguiente operación de lectura o escritura por cualquier usuario.
10. La sesión ahora tiene cero miembros y se quita del servicio.

Si el tiempo de espera inactivo de la sesión de ejemplo se establece en 0, B reproductor agote inmediatamente después de la comprobación de presencia en el paso 7a y probablemente sea quitado por la escritura de la sesión. En este caso, el cierre de sesión sin necesidad de más lee o escribe en la sesión.


## <a name="multiple-signed-in-users-on-a-single-console"></a>Varios usuarios con sesión iniciada en una única consola


Cuando varios usuarios inician sesión en la misma consola, es posible que algunos usuarios para estar en una sesión de juego, mientras que otros usuarios no están en la sesión o no están activa en el título de la actual. También se pueden recibir invitaciones de juego y aceptados para varios usuarios, tener un impacto en la pertenencia a la sesión de juego. Un título debe tener en cuenta estos puntos para poder controlar correctamente todos los escenarios de pertenencia de la sesión.

En un escenario común, un nuevo reproductor inicia sesión, se activa en el juego y debe agregarse a una sesión existente de juego. Como con la creación de una nueva sesión de juego, un título sólo debe agregar un usuario cuándo es adecuado durante el juego.

Con varios usuarios con sesión iniciada, uno o varios usuarios también pueden recibir invitaciones a otra sesión de juego. Los títulos no es necesario administrar estos escenarios de cualquier manera específica. Eventos de estado y miembro de la sesión notifican el título de las actualizaciones a la pertenencia de usuario y la sesión de juego.

Para controlar varios usuarios con sesión iniciada para una sesión en línea, el título se suscribe para hombro pulsa para todos los usuarios, con otro **XboxLiveContext clase** objeto para cada usuario. El título se usa el **MultiplayerSession.ChangeNumber propiedad** determinar determinados cambios en la sesión y omitir el hombro duplicado pulsa.


## <a name="process-lifecycle-management"></a>Administración de ciclo de vida del proceso


Al igual que un título que no sea de varios jugadores, puede producirse un título en una sesión de varios jugadores suspensión de título y la terminación de eventos de ciclo de vida del proceso. El árbitro de sesión, por tanto, debe guardar periódicamente el estado de sesión. En caso de que se suspende el árbitro, el título debe intentar árbitro migración y guardar el estado del juego según corresponda, para que un árbitro nuevo puede restaurar el estado de sesión. A continuación, es posible para una sesión completa multijugador suspender y reanudar más adelante, siempre y cuando la sesión sigue siendo válida en MPSD. Solo un par designado, normalmente el anfitrión del juego, debe actualizar el estado global del juego.


### <a name="storage-of-game-metadata"></a>Almacenamiento de metadatos de juegos

Un título almacena los metadatos de juegos en la sesión MPSD. Metadatos de juegos son la información necesaria para mostrar datos de la sesión y habilitar el título buscar y unirse a la sesión de juego. El título almacena metadatos específicos del Reproductor en la sección de propiedades personalizadas para la sesión miembro, por ejemplo, color del Reproductor, arma Reproductor preferido para la sesión, etcetera. Metadatos de nivel de sesión, por ejemplo, la asignación actual, se almacenan en la sección global de las propiedades personalizadas de la sesión MPSD.


### <a name="storage-of-game-state"></a>Almacenamiento del estado del juego

Estado del juego se almacena en almacenamiento administrado por el título (TMS), mediante el **servicio de almacenamiento del título**. Con esta ubicación de almacenamiento permite un título al migrar al árbitro sin problemas de permisos. Consulte [migrar un árbitro](migrating-an-arbiter.md).

| Nota                                                                                                               |
|---------------------------------------------------------------------------------------------------------------------------------|
| El título no debe intentar guardar estado del juego en TMS con más frecuencia que una vez cada 5 minutos, a menos que se ha suspendido. |

## <a name="cleanup-of-inactive-sessions"></a>Limpieza de sesiones inactivas

Si el sessionEmptyTimeout se establece en 0, una sesión MPSD se elimina automáticamente cuando el Reproductor último sale de la sesión. Para obtener información sobre cómo impedir que una sesión sin usar que contiene los jugadores tras bloqueo o la desconexión, consulte [desconectar detección y control de notificación de cambio de MPSD.](multiplayer-session-directory.md). El tratamiento incorrecto de las sesiones inactivas tras bloqueo o desconectar puede causar problemas cuando un título está consultando las sesiones para un reproductor.

Es la manera recomendada para limpiar las sesiones inactivas que todas las sesiones para un usuario determinado de la consulta de título mediante una llamada a la **MultiplayerService.GetSessionsAsync método** y, a continuación, evaluar las sesiones. Cuando encuentra una sesión obsoleta, el título se llama a la **MultiplayerSession.Leave método** para todos los reproductores locales en la sesión. Esta llamada quita finalmente el número de miembros en 0 y limpia las sesiones.

## <a name="session-arbiter"></a>Sesión Arbiter


Algunos métodos multijugador solo deben llamarse por un cliente dentro de una sesión de juego. Este cliente es una de las consolas que participan en la sesión, se llama el árbitro, o el host. Si al menos un miembro de la sesión está en un juego, la sesión debe tener un árbitro para supervisar las combinaciones en curso.


### <a name="setting-the-arbiter"></a>Establecer el árbitro

Cuando crea una sesión, el cliente designa una única consola como el árbitro. Vea [Cómo: Establecer un árbitro para una sesión MPSD](multiplayer-how-tos.md).


### <a name="saving-session-state"></a>Guardar el estado de sesión

Como se describe en **administración del ciclo de vida del proceso**, el árbitro debe guardar el estado de sesión periódicamente. Un árbitro nuevo debe ser capaz de restaurar el estado de sesión en el caso de migración de arbiter el título. Para obtener más información, consulte [migrar un árbitro](multiplayer-how-tos.md).


### <a name="managing-game-session-members-and-joins-in-progress"></a>Administración de miembros de la sesión de juego y combinaciones en curso

Es el rol más importante del árbitro de sesión administrar los usuarios que entran en la sesión de juego para reproducir. Esto incluye controlar juegos invitaciones, notificar a los jugadores de espera y trabajar con los jugadores que sale del juego.


#### <a name="receiving-notifications"></a>Recibir notificaciones

Debe escuchar el árbitro nuevos jugadores que deseen unirse a la sesión de juego con la **RealTimeActivityService.MultiplayerSessionChanged eventos**.


#### <a name="finding-players-to-fill-empty-game-session-slots"></a>Buscar los reproductores para rellenar las ranuras de la sesión de juego vacío

El árbitro busca los reproductores para rellenar las ranuras de la sesión de juego vacío por una de estas operaciones:   Si el título de la utiliza una sesión introductoria u otro mecanismo para permitir combinaciones diferidas, buscar a miembros de la nueva sesión con dicho mecanismo.
-   Cree otra sesión de vale de coincidencia.

Vea también [Cómo: Rellenar las ranuras de la sesión abierta durante contactos](multiplayer-how-tos.md).


#### <a name="handling-invited-session-members"></a>Control de los miembros de la sesión de invitado

El árbitro debe supervisar a los miembros de la sesión invitado y aplicar un intervalo mínimo entre invitaciones a un único usuario. Vea también [Cómo: Enviar invitaciones juego](multiplayer-how-tos.md).
