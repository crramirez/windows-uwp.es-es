---
title: Directorio de sesión multijugador de Xbox One
description: Obtenga información sobre cómo crear sesiones de varios jugadores mediante el servicio de Xbox Live Mutliplayer sesión directorio (MPSD).
ms.assetid: 70da1be3-5f39-4eed-b62d-9cdd47e413d2
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 52b363492d1cd17ae54ae5d23d04371c5adf73ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606820"
---
# <a name="xbox-one-multiplayer-session-directory"></a>Directorio de sesión multijugador de Xbox One

En este tema se proporciona información general de creación de sesiones de varios jugadores con el nuevo servicio Xbox uno multijugador sesión directorio (MPSD). El documento está dirigido principalmente a desarrolladores título Xbox One que envían sus plantillas de sesión directamente a Xbox desarrollo Portal (XDP). El servicio MPSD también se puede configurar con el centro de partners, pero no se centra en este artículo. Se está diseñado para que se familiarice con los términos y conceptos asociados con la configuración de MPSD, uso y solución de problemas de sesiones de varios jugadores.

## <a name="revision-summary"></a>Resumen de la revisión

Las bibliotecas de cliente XSAPI (API de servicios de Xbox) usan actualmente la versión del contrato 104 al comunicarse con los servicios MPSD. Esta versión del documento actualiza el esquema de plantilla de sesión para el contrato versión 107, que es la última versión de contrato que se admiten los servicios MPSD. Los cambios entre la versión 104 y 107 se resumen en la sección titulada [actualización del esquema de contrato](#_Contract_schema_update).

Tenga en cuenta que las plantillas que se han escrito para la versión del contrato 104 deberán actualizarse si se vuelven a publicar como 107. Sin embargo, los servicios son compatibles con versiones anteriores para que pueda continuar usar las bibliotecas actuales, que se actualizarán en futuras versiones XDK.

La revisión anterior de este documento había actualizado información sobre las sesiones del servidor y agregó una sección nueva acerca de la API de servicio de Xbox Live y las llamadas al servicio RESTful. Además, se actualizó la tabla que se encuentra en la consulta para la sección de información de estado de sesión y se ha revisado la sección de plantillas de calidad de servicio (QoS).

## <a name="introduction"></a>Introducción

En Xbox One, una sesión de varios jugadores es un documento protegido que se encuentra en la nube en el directorio de sesión de varios jugadores (MPSD) y, en este documento representa un grupo de personas un juego. En concreto, sesiones de varios jugadores tienen las características siguientes:

-   Cada sesión tiene un URI único.

-   El documento de la sesión contiene a los miembros actuales, configuración del juego, datos e información de servidor de juegos de arranque.

-   Los títulos de creación y administración sus propias sesiones.

-   Una sesión permite la conectividad entre sus miembros.

El directorio de sesión de varios jugadores centraliza los metadatos de la sesión de juego en todos los clientes. Proporciona la información básica necesaria sobre una sesión para ayudarle a configurar las asociaciones de dispositivo segura entre los clientes. También proporciona metadatos básicos de primer arranque para un cliente se conecte al juego antes de que comience a pasar datos del juego más específicas. Con administración de vigencia de procesos (PLM) y la naturaleza de la conmutación de tareas de aplicaciones en Xbox One, el MPSD garantiza que los clientes la información correcta para ponerse en contacto con colegas y los servidores que se identifican como parte de la sesión activa de juegos y las coordenadas con el sistema operativo shell y la consola para reservar, activar y administrar la vigencia del Reproductor para una sesión de juego.

## <a name="terminology-used-in-this-document"></a>Terminología usada en este documento

| Término                 | Definición                                                                                                                                                                                                                                                                                  |
|----------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Sesión de varios jugadores  | Un documento protegido que se encuentra en la nube de Xbox Live y representa un grupo de usuarios (o será) conectadas entre sí mientras se reproduce un título en Xbox One. Todos los aspectos de varios jugadores, como contactos, entidades, combinación en curso y así sucesivamente, aproveche la sesión de varios jugadores. |
| Sesión de juego         | Se trata de la sesión de juego real, expuesta en el MPSD, en el que los usuarios están jugando a juntos. Todos los escenarios multijugador en última instancia, terminan en una sesión de juego.                                                                                                                               |
| Coincide con la sesión de vale | Se trata de una sesión que se usa para realizar un seguimiento de enviar una solicitud de coincidencia durante los contactos.                                                                                                                                                                                                                 |
| Reproductor inactivo      | Un reproductor que se ha establecido en el estado inactivo dentro de la sesión. El título establece un usuario en el inactivo estado cuando el juego está restringido, suspendido, o en caso contrario, inactivas como se define en el título.                                                                                     |

## <a name="the-multiplayer-session-directory"></a>El directorio de sesión de varios jugadores

El MPSD facilita y ayuda a los títulos de coordinar la información de sesión entre los jugadores en línea. Puede haber diferentes tipos de sesiones creadas para realizar diferentes tareas de jugadores. En la tabla siguiente presenta las diferencias en cómo se realizaron tareas en la consola Xbox 360 frente a cómo se consigue en Xbox One.

| Tarea o función                     | Xbox 360                                                                                                        | Xbox One                                                                                                   |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
| **Obtener información de sesión de juego**     | **XSessionGetDetails**, **XSessionSearchByID**, o realizar un seguimiento usted mismo.                                              | Solicitar la información de sesión del servicio.                                                          |
| **Migración de host**                     | Cuando es necesario, el título se llama a **XSessionMigrateHost.**                                                           | No es necesario crear una nueva sesión, basta con asignar un nuevo host de SessionID.                                  |
| **Administrar varias sesiones de Reproductor**  | Difícil de controlar más de una sesión a la vez. Por ejemplo, **XNetReplaceKey** frente a **XNetUnregisterKey**. | Sesión basada en servicios facilita la administración de una sesión limpiador y facilita la tarea controlar varias sesiones.    |
| **Controlar la configuración de inicio de sesión y se desconecta** | Tiene que controlar se desconecta y cierre de sesión de manera diferente, con **XCloseHandle** o **XSessionDelete**, respectivamente. | Servicio centralizado simplifica la configuración de inicio de sesión y desconectar el control y los tiempos de espera se establecen en la configuración del juego. |

### <a name="session-patterns"></a>Patrones de sesión

-   Sesiones de juego

    -   Sesión con jugadores XUIDs, datos de dirección de dispositivo seguro y Estados de propiedad. Esto se considera como la sesión de juego real.

    -   Puede ser peer-to-peer, del mismo nivel para el host, del mismo nivel a servidor o híbrida.

-   Coincide con las sesiones de vale

    -   Una sesión que se envía a los contactos para buscar otros jugadores que jugar con. Tenga en cuenta que una sesión también puede ser una sesión de vale, si está buscando el juego para varios jugadores.

-   Sesiones del servidor

    -   Las sesiones de juego se crean y se utilizan a través del procesamiento de Xbox Live Compute.

Figura 1 ilustra los usos de sesiones MPSD, donde los cuadros azules representan sesiones MPSD y los cuadros rojos son los servicios que las utilizan.

Figura 1. Uso de la sesión MPSD.

## <a name="mpsd-sessions"></a>Sesiones MPSD

Las sesiones mantienen dos listas de entidades relacionadas:

1.  Miembros (usuarios) que se han incorporado o ha invitado a una sesión.

2.  Servidores (servidores en la nube o servidores dedicados) que se han unido a la sesión.

Cada entidad tiene:

1.  Establecen valores constantes en tiempo de creación.

2.  Propiedades mutables.

3.  Metadatos de solo lectura.

### <a name="xbox-live-service-apis-and-restful-service-calls"></a>API de servicio de Xbox Live y las llamadas al servicio RESTful

Hay dos formas para interactuar con los servicios de contactos y las sesiones de una Xbox. La primera consiste en utilizar llamadas HTTPS estándar para el URI de servicios de Xbox Live RESTful. Esto permite la flexibilidad de los títulos en una llamada a e interactuar con estos servicios en función de su servidor y las configuraciones de juego. Encontrará una lista de estos URI en el [documentación del Kit de desarrollo uno (XDK) Xbox](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) bajo "Xbox Live servicios RESTful referencia." [1]

El segundo método es usar la Xbox Live Service API, que actúan como contenedores para el URI del servicio RESTful. Permiten un enfoque más tradicional con las API de código sin tener que controlar el tráfico HTTPS para cada llamada. El código fuente para las API de servicio de Xbox Live se distribuye con el Kit de desarrollo de Xbox (XDK) y se puede modificar e incorporar en el título según sea necesario. Los ejemplos están escritos mediante las API de servicios de Xbox Live. Puede encontrar más información acerca de las API de servicios de Xbox Live en Xbox One [documentación XDK](https://developer.xboxlive.com/en-us/platform/development/documentation/software/Pages/home.aspx) bajo "Xbox la referencia de servicios en vivo". [2]

### <a name="mpsd-sessions-and-templates"></a>Las plantillas y las sesiones MPSD

Sesiones MPSD se crean con plantillas que se ingieren a través del Portal de desarrollo de Xbox (XDP). Las plantillas son documentos JSON que definen el marco de trabajo para la sesión que se va a crear. La plantilla proporciona constantes para la nueva sesión.

El siguiente extracto del [código de ejemplo de Reproductor Rendezvous](https://developer.xboxlive.com/en-us/platform/development/education/Pages/Samples.aspx) es un ejemplo de configuración de la plantilla.

```json
// Used for creating the session that you then pass into your match ticket request. This *should* not have any QoS requirements.
MatchTicketSession (Contract Version: 107)
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

// This is the new game session that is returned after you’ve been matched.
// Note: This is used for in-game QoS. For out-of-game QoS, you will need P2P/HTP requirements.
GameSession (Contract Version:107)
{
    "constants": {
        "system": {
            "maxMembersCount": 12,
            "capabilities": {"connectivity": true }
        },
        "memberInitialization": {
         "joinTimeout": 20000,
         "measurementTimeout": 15000,
         "externalEvaluation": false,
         "membersNeededToStart": 4
         },

   "custom": {}
  }
}
```

La sesión de vale de coincidencia se debe usar con una plantilla de sesión de juego configurado con valores de tiempo de espera de QoS en su **memberInitialization** objeto.

Figura 2. Hopper de ejemplo.

![](../../images/whitepapers/mpsd_image1.png)

El fragmento de código que sigue es un ejemplo de una plantilla de punto a punto de partida (administrada por título QoS).

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 2
            }
        },
        "custom": {}
    }
}

```

Esto es un ejemplo de una sesión del mismo nivel a servidor y una plantilla sin procesar.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {}
        },
        "custom": {}
    }
}
```

El código siguiente muestra un ejemplo de una plantilla de sesión de vale de coincidencia, que se utiliza para coincidir con Smart.

```
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 10,
            "visibility": "private",
            "capabilities": {},
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            }
        },
        "custom": {}
    }
}

```

### <a name="checking-which-templates-are-configured-for-your-scid"></a>Comprobación de las plantillas que están configuradas para su ¿SCID

#### <a name="session-templates"></a>Plantillas de sesión

La lista de plantillas de sesión que existen dentro de un ¿SCID, así como los detalles de una plantilla de sesión específico, se pueden recuperar de MPSD:

```
GET /serviceconfigs/{scid}/sessiontemplates

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}
```

#### <a name="query-for-session-state-information"></a>Consultar información de estado de sesión

Las sesiones se pueden consultar en los niveles de plantilla de sesión y la configuración de servicio:

```
GET /serviceconfigs/{scid}/sessions

GET /serviceconfigs/{scid}/sessiontemplates/{session-template-name}/sessions
```

De forma predeterminada, esto devolverá un máximo de 100 sesiones no privados. La consulta se puede modificar mediante estos parámetros de cadena de consulta:

| Cadena de consulta             | Efecto                                                                                                         | Descripción                                                                                         |
|--------------------------|----------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| keyword=foo              | Incluir solo las sesiones que tienen la palabra clave "foo".                                                             |                                                                                                     |
| XUID=123                 | Incluir solo las sesiones que se encuentra el usuario "123" en.                                                               | De forma predeterminada, el usuario debe ser activo en la sesión que se incluya.                                  |
| *reservations*=**true** | Incluyen las sesiones para los que el usuario se establece como un reproductor reservado pero no se ha unido para convertirse en un Reproductor de activo. | Solo al consultar sus propias sesiones o al consultar las sesiones de para servidores un usuario específico. |
| *inactive*=**true**      | Incluyen las sesiones que el usuario ha aceptado, pero no se reproduce activamente.                                     | Las sesiones en que el usuario es "listo" pero no "activo" cuentan como inactivas.                           |
| *private*=**true**       | Incluir sesiones privadas.                                                                                      | Solo al consultar sus propias sesiones o al consultar el servidor a servidor.                            |
| *visibilidad*= abierta        | Incluir solo las sesiones que están "abiertas".                                                                         | Si establece en "private", el "privada = true" también se debe establecer la directiva.                                 |
| *take*=5                 | Devolver un máximo de cinco sesiones.                                                                                    | Debe estar entre 0 y 100.                                                                          |

El resultado es una matriz JSON de referencias de la sesión. Algunos datos de la sesión están incluido en línea.

**Tenga en cuenta** cada consulta debe incluir un filtro de palabras clave, un filtro XUID o ambos.

Configuración *privada* (que devuelve las sesiones privadas) o *reservas* (que devuelve las sesiones de usuario no se ha unido) a **true** requiere que el llamador tiene para que coincida con el filtro XUID en el URI de notificación de nivel de servidor de acceso a la sesión, o para XUID del llamador. En caso contrario, se devuelve 403 Forbidden (si existen o no las sesiones de este tipo realmente).

El siguiente fragmento de código muestra un ejemplo de una respuesta de consulta.

```
{
"results": [ {
"xuid": "9876",  // If the session was found from a xuid, that xuid.
"startTime": "2009-06-15T13:45:30.0900000Z",
"sessionRef": {
  "scid": "foo",
  "templateName": "bar",
  "name": "session-seven"
},
"accepted": 4,        // Approximate number of non-reserved members.
"status": "active",   // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
"visibility": "open", // or "private", "visible", or "full"
"myTurn": true,       // Not present is the same as 'false'. Only present if the session was found using a xuid.
"keywords": [ "one", "two" ]
} ]
}

```

## <a name="session-template-attributes"></a>Atributos de la plantilla de sesión

<div id="_Contract_schema_update"/>

## <a name="contract-schema-update"></a>Actualización del esquema de contrato

Como mencioné al principio de este documento, la versión más reciente de contrato de plantilla de sesión es 107, lo que introduce algunos cambios en el esquema de la versión anterior de 104. Las plantillas que se han escrito para la versión del contrato 104 deberá actualizarse si se vuelven a publicar como 107. El siguiente es un resumen de los cambios.

-   **/Constants/System/managedInitialization** ha cambiado a **/constants/system/memberInitialization**.

    -   El **autoEvaluate** campo ha cambiado a **externalEvaluation** y sus cambios polaridad, excepto en que **false** sigue siendo el predeterminado.

    -   El valor predeterminado de **membersNeededToStart** cambia de 2 a 1.

    -   El valor predeterminado de **joinTimeout** cambia de 5 segundos a 10 segundos.

    -   El **measurementTimeout** predeterminado cambia de 5 segundos a 30 segundos.

-   **/Constants/System/Timeouts** se quita, y los tiempos de espera se cambia el nombre y se reubica en **/constantes/sistema**.

    -   El **reservada** se convierte en tiempo de espera **reservedRemovalTimeout**.

    -   El **inactiva** se convierte en tiempo de espera **inactiveRemovalTimeout** y su nuevo valor predeterminado es 0 en lugar de 2 horas.

    -   El **listo** se convierte en tiempo de espera **readyRemovalTimeout**.

    -   El **sessionEmpty** se convierte en tiempo de espera **sessionEmptyTimeout**.

-   **/Constants/System/Capabilities/gameplay** debe especificarse como **true** en las sesiones que representan el juego real (en lugar de sesiones de aplicación auxiliar como coincidencia e introductoria sesiones) para permitir que los jugadores recientes y reputación de informes.

### <a name="system-objects"></a>Objetos del sistema

Cada uno de los objetos del sistema en el documento de la sesión tiene un esquema fijo que se aplican y se interpretan MPSD.

Dentro del cuerpo de las solicitudes PUT, se validan los objetos del sistema y se combinan al igual que los objetos personalizados. Pero a diferencia de los objetos personalizados, una vez que se combinan el sistema de los objetos más se validan y actuar en función de estos esquemas.

**/constants/system**

```json
{
"version": 1,  //Document Version (FAL release version 1, service contract 107)
"maxMembersCount": 100,  // Defaults to 100 if not set on creation. Must be between 1 and 100.
"visibility": "private",  // Or "visible" or "open", defaults to "open" if not set on creation.

"initiators": [ "1234" ],  // If specified on a new session, the creators xuid must be in the list (or the creator must be a server).
"inviteProtocol": "party",  // Optional URI scheme of the launch URI for invite toasts.

"reservedRemovalTimeout": 10000, // Default is 30 seconds. Member is removed from the session.
"inactiveRemovalTimeout": 0, // Default is zero. Member is removed from the session.
"readyRemovalTimeout": 60000, // Default is three minutes. Member is removed from the session.
"sessionEmptyTimeout": 0, // Default is zero. Session is deleted.

// Capabilities are boolean values that are optionally set in the session template. If no capabilities are needed, an empty "capabilities" object should be in the template in order to prevent capabilities from being specified on session creation, unless the title desires dynamic session capabilities.
"capabilities": {
"clientMatchmaking": true,
"connectivity": true, // Cannot be set if ‘large’ is specified.
     "suppressPresenceActivityCheck": false,
     "gameplay": false,
     "large": false
},
/* If a "memberInitialization" object is set, the session expects the client system or title to perform initialization following session creation and/or as new members join the session. The timeouts and initialization stages are automatically tracked by the session, including QoS measurements if any metrics are set. These timeouts override the session's reservation and ready timeouts for members that have 'initializationEpisode' set. */
"memberInitialization": {
"joinTimeout": 20000,  // Milliseconds. Unspecified counts as 10 seconds.
"externalEvaluation": false,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 00 and the session is created with no members to initialize, episode 1 is skipped..
},
```

**/properties/system**

```
{
// Optional array of case-insensitive strings. Cannot be set if the session's visibility is "private".
"keywords": [ "hello" ],

// Array of integer indices of members whose turn it is. Defaults to empty. Can't be set (and doesn't appear) on large sessions.
"turn": [ 0 ],

/* Restricts who can join "open" sessions. (Has no effect on reservations, which means it has no impact on "private" and "visible" sessions.) Defaults to "none". On large sessions, "none" is the only valid value.
If "local", only users whose token's DeviceId matches someone else already in the session and "active": true.
If "followed", only local users (as defined above) and users who are followed by an existing (not reserved) member of the session can join without a reservation. */
"joinRestriction": "none",

// Device token of the host. Must match the "deviceToken" of at least one member, otherwise this field is deleted.
// If "peerToHostRequirements" is set and "host" is set, the measurement stage assumes the given host is the correct host and only measures metrics to that host.
// Can't be set on large sessions.
"host": "99e4c701",

// Can only be set while "initializing/stage" is "evaluating". True indicates success, and false indicates failure. Once set, "initializing/stage" is immediately updated, and this field is removed.
"initializationSucceeded": true,

/* The ordered list of case-insensitive connection strings that the session could use to connect to a game server. Generally titles should use the first on the list, but sophisticated titles could use a custom mechanism (e.g. Thunderhead) for choosing one of the others (e.g. based on load). */
"serverConnectionStringCandidates": [ "datacenter b", "serverfarm a" ],

"matchmaking": {
    "targetSessionConstants": { },
    // Force a specific connection string to be used (useful in preserveSession=always cases).
    "serverConnectionString": "datacenter b",
},

// True if the match that was found didn't work out and needs to be resubmitted. Set to false to signal that the match did work, and the matchmaking service can release the session.
"matchmakingResubmit": true
}

```

### <a name="timeouts"></a>Timeouts

Las sesiones se pueden cambiar por los temporizadores y otros eventos externos. El **tiempos de espera** objeto de MPSD proporciona un mecanismo básico para administrar la duración de la sesión y los miembros.

El **nextTimer** campo de la sesión proporciona la hora de la siguiente temporizador. Los clientes pueden usar esta información para elegir un buen intervalo para el siguiente sondeo. Este valor también se devuelve en el **Expires** encabezado HTTP.

Se especifican los tiempos de espera en milisegundos. Cero está permitido, lo que significa que el tiempo de espera debe ser inmediata. Si no se especifica un tiempo de espera especificado, se considera infinito. Dado que los tiempos de espera tienen valores predeterminados, la plantilla de sesión debe especificar explícitamente "null" para un tiempo de espera infinito.

#### <a name="sessionemptytimeout"></a>SessionEmptyTimeout

El **/constants/system/sessionEmptyTimeout** valor configura el número de milisegundos después de una sesión se queda vacía que se eliminarán. El valor predeterminado es cero, lo que significa que la sesión se eliminarán inmediatamente. Si el valor no se especifica, las sesiones vacías no caducarán nunca.

#### <a name="member-timeouts"></a>Tiempos de espera de miembro

Los tres otros tiempos de espera en **/constantes/sistema** controlar la cantidad de tiempo puede permanecer un miembro en un estado determinado:

-   **reservedRemovalTimeout**

    -   Cuando expira el tiempo de espera, se elimina la reserva. El valor predeterminado es 30 segundos.

-   **inactiveRemovalTimeout**

    -   Un miembro inactivo se quita de la sesión después de dos horas de forma predeterminada.

-   **readyRemovalTimeout**

    -   Los miembros que son "ready" reversión al estado inactivo después de tres minutos de forma predeterminada.

<div id="_Member_initialization_in"/>

## <a name="member-initialization-in-session-documents"></a>Inicialización de miembro en documentos de la sesión

### <a name="member-initialization"></a>Inicialización de miembro


El **memberInitialization** objeto controla las etapas de la inicialización después de crear la sesión o como miembros nuevos unirse a la sesión. Automáticamente se realiza el seguimiento de las fases de inicialización y tiempos de espera de la sesión, incluidas las medidas de calidad de servicio si se establecen todas las métricas.

Estos tiempos de espera invalidación reserva y listo tiempos de espera de la sesión de los miembros que tienen el **initializationEpisode** conjunto de propiedades.

Por ejemplo:

```
"memberInitialization": {
        "joinTimeout": 5000,
        "measurementTimeout": 5000,
        "evaluationTimeout": 5000,  // only specify for external evaluation
        "externalEvaluation": true,
       "membersNeededToStart": 2
    },
```


![](../../images/whitepapers/mpsd_image2.png)

**Figura 3. Flujo de inicialización de miembro.**

Cada una de las tres fases de inicialización de miembros de tiempo de espera puede:

1.  **joiningTimeout**

    -   La cantidad de tiempo, en milisegundos, que los usuarios tienen que unirse a la sesión. Se quitan las reservas de direcciones de los miembros que no se pueden combinar.

2.  **measuringTimeout**

    -   La cantidad de tiempo que los miembros que se tienen que cargar sus medidas. Los miembros que no se pudo cargar las medidas se marcan con un *failureReason* de "tiempo de espera".

3.  **evaluationTimeout**

    -   La cantidad de tiempo para un miembro realizar y cargar la decisión de evaluación. Si no se recibe ninguna decisión, cuenta como un error.

**externalEvaluation**

-   MPSD puede hacer una evaluación automática de QoS basada en requisitos se proporcionan en la plantilla de sesión. La evaluación se realiza cuando externalEvaluation está establecida en false. **externalEvaluation** debe ser **true** cuando un **evaluationTimeout** está establecido. Si hay dos requisitos de punto a punto o del mismo nivel para el host, debe establecer **externalEvaluation** a **false** para disponer de la sesión de finalizar la inicialización.

**membersNeededToStart**

-   Este es el número mínimo de las reservas de miembros que deben existir con "initialize": "true" y pase la calidad de servicio para la evaluación automática se realice correctamente.

### <a name="initialization-episodes"></a>Episodios de inicialización


Cuando el **memberInitialization** se establece el objeto, MPSD progresan las fases de inicialización por episodio. El primer episodio se inicia cuando la sesión se crea e incluirá todas las fases definidas en la plantilla.

Los nuevos miembros invitados o Unidos mientras ya se está ejecutando un episodio se marcará para el próximo episodio. El estado de un episodio o **memberInitialization** en general, se puede recuperar desde el **inicializar** objeto de la sesión.

**Tenga en cuenta** tenga en cuenta, este objeto se quita una vez completada la inicialización.

Por ejemplo:

```
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

El escenario va de unirse a medir a evaluar. Si se produce un error en un episodio, está preparada *no se pudo* y no se puede inicializar la sesión. En caso contrario, cuando se completa un episodio de inicialización, el **inicializar** se quita el objeto.

También se realiza un seguimiento de los errores de inicialización para cada miembro. Se establecen al realizar la transición fuera de la fase de medición o de unión si no se supera este miembro.

Por ejemplo:
```
"initializationFailure": "latency",
```
En el orden de prioridad, el valor de este atributo puede establecerse en *tiempo de espera, latencia, bandwidthdown, bandwidthup, red y grupo*, o *episodio*. El valor de la red significa que la configuración de red o condiciones (como la traducción de direcciones de red en conflicto \[NAT\]) impide que se va a medir las métricas de calidad de servicio. Es el único valor posible al final de la unión *grupo*. (En tiempo de espera de unión, se quita la reserva).

Si **memberInitialization** está establecida y se agregó el miembro con "initialize": true, se establece en el episodio de inicialización que se va a participar en el miembro. Un valor de 1 se utiliza para los miembros agregados a una nueva sesión en el momento de crearlo, y se quita cuando finaliza el episodio de inicialización.
```
"initializationEpisode": 1,
```

## <a name="match-ticket-session"></a>Coincide con la sesión de vale

Cuando se utiliza una sesión MPSD como una sesión de vale de coincidencia, se utilizan algunas propiedades de la sesión especial y constantes.

**/members/{index}/constants/system**

```json
{

  {
  "xuid": "12345678",

  "initialize": "false", // Run initialization for this user (if “memberInitialization” is set). Defaults to false.

```

Cuando el servicio agrega a los usuarios a una sesión, proporciona un contexto en torno a cómo y por qué se coincide en la sesión, en el **matchmakingResult** campo.

```
"matchmakingResult": {
```

Se trata de una copia de un usuario **serverMeasurements** de la sesión de contactos.

```json
"serverMeasurements": {
    "east.thunderhead.azure.com": {
        "latency": 233  // Milliseconds
      }
    }
  }
}

```

## <a name="quality-of-service-qos-templates"></a>Calidad de las plantillas de servicio (QoS)

Para las plantillas de la sesión de juego, se pueden agregar valores que le informan MPSD coordinarse con el nivel de red y la plataforma social de consola. El propósito de esta coordinación es validar la calidad del estado de conexión antes de que se crea una sesión y antes de que se informa de que el juego está preparado para unirse a un usuario.

El siguiente fragmento de código es un ejemplo de una plantilla de sesión de juego al host del mismo nivel con QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToHostRequirements": {
                "latencyMaximum": 350,
                "bandwidthDownMinimum": 1000,
                "bandwidthUpMinimum": 100,
                "hostSelectionMetric": "latency"
            }
        },
        "custom": {}
    }
}
```

Este fragmento de código es un ejemplo de una plantilla de punto a punto de partida con QoS:

```json
{
    "constants": {
        "system": {
            "version": 1,
            "maxMembersCount": 20,
            "visibility": "private",
            "capabilities": {
                "connectivity": true
            },
            "memberInitialization": {
                "joinTimeout": 20000,
                "externalEvaluation": false,
                "membersNeededToStart": 1
            },
            "peerToPeerRequirements": {
                "latencyMaximum": 250,
                "bandwidthMinimum": 10000
            }
        },
        "custom": {}
    }
}

```

### <a name="qos-session-template-attributes"></a>Atributos de plantilla de sesión de QoS

Si un **memberInitialization** se establece el objeto, la sesión espera que el sistema cliente o el título a realizar la inicialización después de la creación de la sesión o como miembros nuevos unirse a la sesión.

Automáticamente se realiza el seguimiento de las fases de inicialización y tiempos de espera de la sesión, incluidas las medidas de calidad de servicio si se establecen todas las métricas.

Estos tiempos de espera invalidación reserva y listo tiempos de espera de la sesión de los miembros que tienen el **initializationEpisode** conjunto de propiedades.

```json
"memberInitialization": {
"joinTimeout": 5000,  // Milliseconds. Unspecified counts as 10 seconds.
"measurementTimeout": 5000,  // Milliseconds. Unspecified counts as 30 seconds.
"evaluationTimeout": 5000,  // Milliseconds. Can only be set if 'externalEvaluation' is true. Unspecified counts as 5 seconds.
"externalEvaluation": true,
"membersNeededToStart": 2  // Unspecified counts as 1. Must be between 0 and maxMembersCount. Only applies to episode 1. If 0 and the session is created with no members to initialize, episode 1 is skipped.
},

```

Una sesión de juego con QoS requiere funcionalidad de conectividad. Si no hay métricas se especifican, el valor predeterminado lo que se necesitan para cumplir los requisitos de calidad de servicio. Si se especifican, deben ser suficientes para cumplir los requisitos de calidad de servicio.

```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true

```

Los umbrales siguientes se aplican a cada conexión en pares de todos los miembros en una sesión:

```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthMinimum": 10000  // Kilobits per second
},

```

Los umbrales siguientes se aplican a cada conexión de un candidato de host:

```json
"peerToHostRequirements": {
    "latencyMaximum": 250,  // Milliseconds
    "bandwidthDownMinimum": 100000,  // Kilobits per second
    "bandwidthUpMinimum": 1000,  // Kilobits per second
    "hostSelectionMetric": "bandwidthup"  // Or "bandwidthdown" or "latency". Not specified is the same as "latency".
},

```

La siguiente conexión de servidor posibles las cadenas deben ser evaluado (tenga en cuenta que las cadenas de conexión deben estar en minúsculas):

```json
"measurementServerAddresses": {
        "east.thunderhead.azure.com": {
            "secureDeviceAddress": "r5Y="  // Base-64 encoded secure-device-address
        },
        "west.thunderhead.azure.com": {
            "secureDeviceAddress": "rwY="
        }
    }
}

```

**members/{index}/properties/system**

Estos marcadores controlan el estado de miembro y **activeTitle**, y son mutuamente excluyentes (es un error para establecer ambos en **true**). Para cada uno, **false** es igual que "no presente". El estado predeterminado es "inactivo", es decir, que ninguno está presente.

```json
"ready": true,
"active": false,

// Base-64 blob, or not present. Empty-string is the same as not present.
// 'capabilities/connectivity' must be enabled in order for this to be set.
"secureDeviceAddress": "ryY=",

// List of members in my group, by index. Each element must be an integer 0 <= n < 'membersInfo/next'.  
// During member initialization, if any members in the list fail, this member will also fail.
"group": [ 5 ],

// QoS measurements by lower-case device token.
// Like all fields, "measurements" must be updated as a whole. It should be set once when measurement is complete, not incrementally.
// Metrics can me omitted if they weren't successfully measured, i.e. the peer is unreachable.
// If a "measurements" object is set, it can't contain an entry for the member's own address.
"measurements": {
"e69c43a8": {
"latency": 5953,  // Milliseconds
"bandwidthDown": 19342,  // Kilobits per second
"bandwidthUp": 944,  // Kilobits per second
"custom": { }
     }
},

// QoS measurements by game-server connection string. Like all fields, "serverMeasurements" must be updated as a whole, so it should be set once when measurement is complete.
// If empty, it means that none of the measurements completed within the "serverMeasurementTimeout".
    "serverMeasurements": {
        "east.thunderhead.azure.com": {
            "latency": 233  // Milliseconds
        }
    },

// Subscriptions for shoulder taps on session changes. The 'profile' indicates which session changes to tap as well as other properties of the registration like the min time between taps.
// The subscription is named with a client-generated GUID that is also sent back with the tap as a context ID.
// Subscriptions can be added and removed individually, without affecting other subscriptions in the "subscriptions" object.
// To remove a subscription, set its context ID to null.
// (Like the "ready" and "active" flags, the "subscriptions" data is copied out and maintained internally, so the normal replace-all rule on system fields doesn't apply to "subscriptions".)
"subscriptions": {
"961dc162-3a8c-4982-b58b-0347ed086bc9": {
"profile": "party",  // Or "matchmaking", "initialization", "roster", "queueHost", or "queue"
"onBehalfOfTitleId": "3948320593",  // Optional decimal title ID of the registered channel.  If not set the title ID is taken from the token.
},
"709fef70-4638-4b94-905b-24cb02706eb5": null
}
}

```

## <a name="qos-phase-and-session-initialization-details"></a>Detalles de inicialización de sesión y la fase de QoS

MPSD realizará el seguimiento y notificar los resultados de calidad de servicio para la creación de juegos después de la plantilla ha completado la inicialización de miembro. El progreso de esta operación se representarán mediante un *inicializar* objeto en el documento de la sesión, como se describe en el [inicialización de miembro](#_Member_initialization_in) sección anterior. El *inicializar* objeto tiene un *fase* atributo, que representa la fase actual de la inicialización. Progreso de la fase de *unir* a *medir* a *evaluar*.

-   Si se inicializa el episodio \#1 produce un error, está preparada *no se pudo* y no se puede inicializar la sesión. En caso contrario, cuando se completa un episodio de inicialización, se quita el objeto "inicialización". Si **externalEvaluation** está establecido en **false**, la fase de evaluación se ha omitido. Si no **métricas** ni **measurementServerAddresses** está establecido, se omite la fase de medición.

```json
"initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
},

```

-   Los candidatos de host son tokens de dispositivo enumerados en orden de preferencia. Se establecen después de la *medir* fase del episodio de inicialización \#1 si **peerToHostRequirements** está establecida y **/properties/system/host** no se ha establecido. Se borran tras una **/properties/system/host** se establece el objeto.

```json
"hostCandidates": [ "ab90a362", "99582e67" ],

"constants": { /* Property Bag */ },
"properties": { /* Property Bag */ },
"members": {
    "1": {
        "constants": { /* Property Bag */ },
        "properties": { /* Property Bag */ },

```

-   El *nombre de jugador* atributo se establecerá sólo si ha aceptado el miembro y si se encuentra una notificación de nombre de jugador para ese miembro.

```json
"gamertag": "stacy",
```

-   El *deviceToken* atributo se establece cuando el miembro carga una dirección de dispositivo segura. Es una cadena de mayúsculas y minúsculas que se puede usar para comparaciones de igualdad.

```json
"deviceToken": "9f4032ba7",
```

-   El *reservada* se quita el valor después de que el usuario ejecuta su propia solicitud PUT primero para el documento de la sesión. Cuando se reservan los jugadores, que significa que se les ha invitado a la sesión de juego pero no han aceptado ni tenían evalúa sus conexiones.

```json
"reserved": true,
```

-   Si el miembro está activo, *activeTitleId* es el título en el que el miembro está activo, en formato decimal.

```json
"activeTitleId": "8397267",
```

-   Este atributo hace referencia a la vez que el usuario ha unido a la sesión. Si *reservada* es **true**, a continuación, *joinTime* será la hora en que se estableció la reserva.

```json
"joinTime": "2009-06-15T13:45:30.0900000Z",
```

-   Presentar si este miembro de la matriz de propiedades/sistema/a su vez, en caso contrario, no es.

```json
"turn": true,
```

-   El *initializationFailure* atributo se establece en el objeto de miembro al realizar la transición fuera de la unión o medir la etapa si el miembro no ha pasado correctamente la fase. En el orden de prioridad, se podría establecer en *tiempo de espera*, *latencia*, *bandwidthdown*, *bandwidthup*, *red* , *grupo*, o *episodio*.
    El *red* valor significa que la configuración de red o condiciones (como las traducciones de direcciones de red en conflicto \[NAT\]) impide que se va a medir las métricas de calidad de servicio. Es el único valor posible al final de la unión *grupo*. (En tiempo de espera de unión, se quita la reserva). El *episodio* valor se establece después de una fase con errores "evaluación" en todos los miembros del episodio de inicialización que no se produjeron errores durante la combinación o medición.

```json
"initializationFailure": "latency",
```

-   Si **memberInitialization** está establecida y se agregó el miembro con "initialize": true, se establece en el episodio de inicialización que se va a participar en el miembro. Se usa un valor de 1 para los miembros agregados a una nueva sesión en tiempo de creación. Se quitan cuando finaliza el episodio de inicialización.

```json
"initializationEpisode": 1,
```

-   El *siguiente* atributo representa el valor de índice del siguiente miembro de la sesión. Será el mismo valor que el *siguiente* atributo en el **membersInfo** objeto a continuación si no hay ningún miembro para agregar más.

```json
            "next": 4
        },
        "4": { "next": 5 /* etc */ }
    },
    "membersInfo": {
        "first": 1,  // The first member's index.
        "next": 5,  // The index that the next member added will get.
        "count": 2,  // The number of members.
        "accepted": 1  // The number of members that are no longer 'pending'.
    },
    "servers": {
        "name": {
            "constants": { /* Property Bag */ },
            "properties": { /* Property Bag */ }
        }
    }
}

```

## <a name="xbox-cloud-compute-session"></a>Sesión de proceso de Xbox en la nube

Una sesión de proceso de Xbox en la nube contiene la lista ordenada de cadenas de conexión de mayúsculas y minúsculas que la sesión se podría usar para conectarse a un servidor de juegos. Por lo general, títulos deben usar la primera cadena en la lista, pero los títulos de sofisticadas podrían usar un mecanismo personalizado (por ejemplo, Xbox Live Compute) para elegir uno de los otros (por ejemplo, en función de carga).

```json
    "serverConnectionStringCandidates": [ "west.thunderhead.azure.com", "east.thunderhead.azure.com" ],

    "matchmaking": {
        "clientResult": {  // Requires the clientMatchmaking property.
            "status": "searching",  // Or "expired", "found", "failed", or "canceled".
            "statusDetails": "Description",  // Default is empty string.
            "typicalWait": 30,  // The expected number of seconds waiting as a non-negative integer.
            "targetSessionRef": {
                "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
                "templateName": "capture-the-flag",
                "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
            }
        },

        "targetSessionConstants": { },

        // Force a specific connection string to be used (useful in preserveSession=always cases).
        "serverConnectionString": "west.thunderhead.azure.com",

        // True if the match that was found didn't work out and needs to be resubmitted. Set to false
        // to signal that the match did work, and the matchmaking service can release the session.
        "resubmit": true
    }
}

```

**/servers/{server-name}/properties/system **

```json
{
    "lockId": "opaque56789",  // If set, a matchmaking service is servicing this session.
    "status": "searching",  // Or "expired", "found", "failed", or "canceled". Optional.
    "statusDetails": "Description",  // Optional free-form text. Default is empty string.
    "typicalWait": 30,  // Optional. The expected number of seconds waiting as a non-negative integer.
    "targetSessionRef": {  // Optional.
        "scid": "1ECFDB89-36EB-4E59-8901-11F7393689AE",
        "templateName": "capture-the-flag",
        "name": "2D58F65F-0E3C-4F1F-8277-2BC9873FDB23"
    }
}

```

## <a name="raw-session-and-custom-title-properties"></a>Sesión sin formato y las propiedades del título personalizado

Una sesión puede utilizarse para almacenar información de mantenimiento personalizado (metadatos) en torno a un juego multijugador. Debe almacenarse datos juegos o información que se guarda en el TMS ++.

### <a name="property-bags"></a>Contenedores de propiedades

Cada uno de los objetos anteriores marcado como un contenedor de propiedades consta de dos objetos internos opcionales, del sistema y personalizadas.

Los objetos personalizados pueden contener cualquier JSON.

```json
"custom": {
    "myField1": true,
    "myField2": "string",
    "myField3": 5.5,
    "myField4": { "myObject": null },
    "myField5": [ "my", "array" ]
}

```

## <a name="active-member-decay"></a>Decadencia de miembro activo

Miembros activos se marcan automáticamente inactivos cuando MPSD detecta que el usuario ya no está ocupado en el título. Esto puede ocurrir, por ejemplo, si la presencia agota el registro de usuario. Este mecanismo es simplemente un mecanismo de seguridad; es decir, los títulos siguen siendo necesarios para proactivamente marcar el miembro como inactiva (o quitarlas de la sesión) cuando los miembros dejen o conmutador lejos del título, se convierten en desacoplar sign out, o en caso contrario.

## <a name="faq-and-troubleshooting"></a>Preguntas más frecuentes y solución de problemas

### <a name="how-do-i-call-mpsd-"></a>¿Cómo se puede llamar MPSD?

Mediante la autenticación de certificado: sessiondirectory.xboxlive.com de cliente

Por ejemplo:

```
PUT https://client-sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

Mediante la autenticación de XToken: sessiondirectory.xboxlive.com

Por ejemplo:

```
PUT https://sessiondirectory-stress.xboxlive.com/serviceconfigs/8cvda84-2606-4bab-8eda-d12313e65143/sessiontemplates/teamDeathmatch/sessions/3baafc35-816d-49cd-9656-5772506c988a
```

### <a name="how-do-i-find-out-what-scid-session-template-and-sandbox-to-use"></a>¿Cómo averiguo qué ¿SCID, plantilla de sesión y espacio aislado para usar?

Esta información puede encontrarse en el sitio XDP del título. Si aún no tiene acceso a XDP póngase en contacto con su administrador de cuentas para desarrolladores, que puede ayudarle a obtener la información para usted.

### <a name="is-there-an-example-of-a-request-body-that-i-can-compare-my-own-to"></a>¿Hay un ejemplo de un cuerpo de solicitud que puedo comparar mis propios a?


### <a name="i-am-getting-a-404-error-when-calling-mpsd"></a>Obtengo un error 404 al llamar a MPSD.

Recopilar seguimientos de Fiddler para ayudar a obtener más información y, a continuación, haga lo siguiente:

1.  El mensaje devuelto como parte del cuerpo de los 404 mensajes comunes HttpResponse, consulte:

    -   *La configuración de servicio solicitado no es válido o no está configurada para sesiones*. Compruebe que el ¿SCID que se va a usar es correcta.

    -   *No se encontró la sesión solicitada*. Compruebe que se crea la sesión y la plantilla de sesión es correcta antes de recuperarlos. Puede crear una sesión con una llamada PUT.

2.  Compruebe el URI que se usa.

3.  Reiniciar la consola o vuelva a intentarlo con un nuevo usuario.

4.  Búsqueda del [foros para desarrolladores de entretenimiento](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) para el código de error o de otras posibles soluciones.

### <a name="i-am-getting-a-403-error-when-calling-mpsd"></a>Obtengo un error 403 al llamar a MPSD.

Esto suele ser un permisos o problema de ámbito. Recopilar seguimientos de Fiddler para ayudar a obtener más información y, a continuación, haga lo siguiente:

1.  Comprobar los mensajes se devuelven como parte del cuerpo de los mensajes comunes 403 HttpResponse:

-   * La configuración de servicio solicitada no se puede acceder. *

    -   Compruebe que está utilizando una cuenta que tenga acceso al espacio aislado.

    -   Compruebe que está en el espacio aislado correcto.

    -   Si está usando la autenticación de certificado y recibe este error, póngase en contacto con su madre.

-   *No se puede tener acceso a la sesión solicitada. Sesiones privada solo puede leerse los miembros de la sesión.*

    -   Está intentando obtener acceso a una sesión que tenga una visibilidad de "privado". Solo los miembros dentro de la sesión pueden leer el documento de la sesión.

-   *El cuerpo de solicitud no puede contener referencias de miembro existentes a menos que la autenticación principal incluye un servidor.*

    -   No puede unirse a otro usuario a una sesión en el nombre de un usuario. Solo puede invitar a. Establece el índice en "reservar\_&lt;número&gt;" para invitar a un reproductor.

### <a name="i-am-getting-a-412-precondition-failed-error"></a>Obtengo un error 412 Error de condición previa.

Esto devolverá 412 Precondition Failed si la sesión ya existe:

> PUT serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/rápido/sesiones/foo HTTP/1.1 Content-Type: application/json If-None-Match: \*

Esto devolverá 412 Precondition Failed si la etag de la sesión no coincide con el encabezado If-Match:

> PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/foo HTTP/1.1 Content-Type: application/json If--Match: 9555A7DE-8B91-40E4-8CFB-0629312C9C7D

### <a name="i-am-getting-errors-such-as-405-409-503-and-400when-calling-mpsd"></a>Obtengo errores como 405, 409, 503 y 400when llamada MPSD.

Recopilar seguimientos de Fiddler para ayudar a obtener más información y, a continuación, compruebe los mensajes se devuelven como parte del cuerpo HttpResponse. Esto debería proporcionarle información suficiente para identificar y corregir el error o para buscar el [foros para desarrolladores de entretenimiento](https://developer.xboxlive.com/en-us/platform/community/forums/Pages/home.aspx) posibles soluciones.

También puede obtener el cuerpo de respuesta si usa las API de servicio de Xbox Live estableciendo el **DiagnosticsTraceLevel** propiedad Error, lo cual la información de la salida de depuración de salida, o puede usar el  **XboxLiveContextSettings.ServiceCallRouted** eventos como se muestra en varias muestras para la salida para el título de la interfaz de usuario.

### <a name="what-can-or-should-i-change-in-the-templates-for-my-title"></a>¿Qué puede o debo cambiar en las plantillas para Mi título?

Plantillas de sesión no son valores predeterminados, pero son más de un molde. Sin embargo, no se puede invalidar las constantes que ya está establecidas en las plantillas, aunque puede agregar a ellos.

### <a name="im-getting-an-error-that-is-saying-my-session-isnt-initialized"></a>Obtengo un error que dice que mi sesión no se ha inicializado.

Cuando la inicialización de miembro está presente en la plantilla (juego, fabricantes y los escenarios de vale de coincidencia, normalmente), deberá asegurarse de que "inicializar = true" se establece para la suficiente cantidad de las reservas de miembro (miembros necesarios para iniciar) para pasar de QoS para corregir este problema.

### <a name="my-session-is-not-being-created-and-im-getting-an-http-204-error"></a>No se está creando mi sesión y obtengo un error HTTP 204.

Esto indica que no había ningún usuario agregado a la sesión cuando lo creó. Si no hay ningún usuario para una sesión en el momento de creación, no se creará la sesión. Asegúrese de que agregue al menos uno de los jugadores a una sesión de creación. Para escenarios de servidor dedicado, obtenga un Reproductor de quién está intentando crear a una coincidencia, o quién necesita crear a una coincidencia y hacer a que el usuario que el Reproductor inicial en esa coincidencia. También puede cambiar o quitar el **sessionEmptyTimeout**.

### <a name="when-should-i-poll-the-mpsd"></a>¿Cuándo debo sondear el MPSD?

Debe evitar las sesiones MPSD de sondeo. En un nivel alto, puede hacerlo mediante el diseño de su código de forma que utiliza la sesión MPSD solo para el establecimiento de conectividad de red inicial para cada cliente y para volver a establecer el estado de la red para un cliente o los clientes que se han perdido la conectividad. También es importante aprovechar al máximo basada en ETags del MPSD primitivas de sincronización para eliminar la necesidad de actualizar el estado de sesión para resolver las condiciones de carrera.

Una aplicación común de estos principios es cuando tiene un conjunto de N clientes que necesitan conectarse entre sí y reproducir en una malla punto a punto. En lugar de consultar periódicamente la sesión para los nuevos miembros, cada miembro puede unirse a la sesión, conectarse a los miembros ya está en la sesión y se supone que los usuarios posteriores que se unen hará lo mismo. Consulte los ejemplos de Chat y Rendezvous Reproductor para obtener ejemplos de cómo realizar la implementación.

Hay algunos casos excepcionales, donde el sondeo puede ser necesario durante un breve período; Si cree que necesita hacer esto, póngase en contacto con su administrador de cuentas de desarrollador.
