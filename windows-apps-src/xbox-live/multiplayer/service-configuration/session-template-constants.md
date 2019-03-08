---
title: Constantes de plantilla de sesión
description: Describe las constantes de sistema definidas en las plantillas de sesión de varios jugadores de Xbox Live.
ms.assetid: d51b2f12-1c56-4261-8692-8f73459dc462
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox one, plantilla de sesión para varios jugadores,
ms.localizationpriority: medium
ms.openlocfilehash: 5bb4c6f39bab8779b5675a7b9c81b883965676c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646910"
---
# <a name="session-template-constants"></a>Constantes de plantilla de sesión

Las tablas siguientes describen los elementos de una plantilla de sesión de varios jugadores, usando la versión de plantilla de sesión 107 predefinidos.

## <a name="system"></a>Sistema

constante del sistema  | Descripción | Valores válidos | valor predeterminado
--|-- | -- | --
version | La versión de la plantilla de sesión. | 1 - n | ninguno
maxMembersCount | El número de sesiones totales miembro ranuras compatibles con la actividad de varios jugadores. | 1 - 100 para una sesión normal, 101 + para una sesión de gran tamaño | 100
visibility | El estado de visibilidad de la sesión, lo que indica si otros usuarios pueden ver o participar en la sesión. | Private, visible, abra | abrir
inviteProtocol | Establecer esta constante para "game" permite a los invitados recibir una notificación cuando se les invita a la sesión. | juego, tournamentgame, chat, gameparty | ninguno
reservedRemovalTimeout  | El tiempo de espera para una reserva de miembro, en milisegundos. Un valor de 0 indica un tiempo de espera de inmediato. Si el tiempo de espera es null, se considera infinito. | 0 - n, null | 30000
inactiveRemovalTimeout  | El tiempo de espera para un miembro se considere inactiva, en milisegundos. Un valor de 0 indica un tiempo de espera de inmediato. Si el tiempo de espera es null, se considera infinito. | 0 - n, null | 0
readyRemovalTimeout | El tiempo de espera para un miembro para considerarse esté listo, en milisegundos. Un valor de 0 indica un tiempo de espera de inmediato. Si el tiempo de espera es null, se considera infinito. | 0 - n, null | 180000
sessionEmptyTimeout | El tiempo de espera para una sesión vacía, en milisegundos. Un valor de 0 indica un tiempo de espera de inmediato. Si el tiempo de espera es null, se considera infinito. | 0 - n, null | 0
[**Capacidades**](#capabilities) | Especifica las capacidades de la sesión. Consulte la sección de capacidades más adelante. | n/d | n/d
[**Métricas**](#metrics) | Especifica un conjunto de título definida requisitos de calidad de servicio, como la velocidad de ancho de banda y latencia, que deben satisfacer los miembros de la sesión.  | n/d | n/d
[**memberInitialization**](#memberInitialization) | Especifica los requisitos de inicialización y tiempos de espera que se aplican cuando la sesión se unen a nuevos miembros. Consulte la siguiente sección de inicialización de miembro. | n/d | n/d
[**peerToPeerRequirements**](#peerToPeerRequirements) | Especifica la red requisitos de calidad de servicio para las conexiones de la malla del mismo nivel del mismo nivel. Consulte la siguiente sección de requisitos de punto a punto. |n/d | n/d
[**peerToHostRequirements**](#peerToHostRequirements) | Especifica la calidad de requerimientos de servicio del mismo nivel para hospedar las conexiones de red. Consulte la siguiente sección de requisitos de host del mismo nivel. | n/d | n/d
[**measurementServerAddresses**](#measurementserveraddresses) | Especifica una colección de los centros de datos posibles que se usan para determinar las medidas de calidad de servicio. Consulte la sección measurementServerAddresses más adelante. | n/d | n/d
[**cloudComputePackage**](#cloudComputePackage) | ? | n/d | n/d
[**Arbitraje**](#arbitration) | Especifica los tiempos de espera para los miembros enviar los resultados de arbitraje en torneos. Consulte la sección cloudComputePackage más adelante. | n/d | n/d
[**broadcastViewerTitleIds**](#broadcastViewerTitleIds) | Especifica una lista de identificadores que siempre debe tener acceso de lectura a la sesión de título. Consulte la sección broadcastViewerTitleIds más adelante. | n/d | n/d
[**ownershipPolicies**](#ownershipPolicies) | Especifica las directivas relativas a la propiedad de sesión. Consulte la sección OwnershipPolicies más adelante. | n/d | n/d


## <a name="capabilities"></a>capabilities
Capacidades son valores booleanos que opcionalmente se establecen en la plantilla de sesión. Si no hay capacidades son necesarias, debe ser un objeto vacío "capabilities" en la plantilla con el fin de impedir que las capacidades se especifican en la creación de la sesión, a menos que el título desea capacidades dinámicas de sesión.

Capacidad |  description | Valores válidos | valor predeterminado
-- | -- | -- | -- |
de red | Indica si la sesión admite la conectividad del mismo nivel. Si este valor es false, a continuación, la sesión no puede habilitar todas las métricas y los miembros de la sesión no pueden establecer sus SecureDeviceAddress. No se puede establecer en las sesiones de gran tamaño. | es true, false | falso
suppressPresenceActivityCheck | Si es true, desactiva las comprobaciones de presencia. | es true, false | falso
jugar a un juego | Indica si la sesión representa el juego real, a diferencia de tiempo o el menú de configuración como una feria o contactos. Si es true, la sesión está en modo de juego. | es true, false | falso
grande | Indica si la sesión es una sesión de gran tamaño (más de 100 miembros). No se admiten las sesiones de gran tamaño para su uso con el Administrador de varios jugadores. | es true, false | falso
connectionRequiredForActiveMembers | Indica si se necesita una conexión en el orden de un miembro a ser activo. | es true, false | falso
cloudCompute | Permite a los clientes solicitar que se puede asignar una instancia de proceso en la nube en el nombre de la sesión. | es true, false | falso
autoPopulateServerCandidates | Calcule y establecer 'serverConnectionStringCandidates' desde 'serverMeasurements' automáticamente. Esta funcionalidad no se puede establecer en las sesiones de gran tamaño. | es true, false | falso
userAuthorizationStyle | Indica si la sesión admite llamadas desde las plataformas sin identidad de fuerte de título. Esta funcionalidad no se puede establecer en las sesiones de gran tamaño.</br></br>Establecer el `userAuthorizationStyle` capacidad `true` los valores predeterminados del `readRestriction` y `joinRestriction`de la sesión para `local` en lugar de `none`. Esto significa que los títulos deben usar identificadores de búsqueda o transferir identificadores para participar en una sesión de juego.| es true, false | falso
crossplay | Indica que la sesión admite el juego cruzado entre los dispositivos de PC y Xbox One. | es true, false | falso
Difusión | Indica que la sesión representa una difusión. El nombre de la sesión debe ser el xuid del emisor. Requiere la capacidad de "grande". | es true, false | falso
Equipo | Indica que la sesión representa un equipo de torneo. Esta funcionalidad no se puede establecer en "grandes" o "juego" sesiones. | es true, false | falso
Arbitraje | Indica que la sesión debe crearse mediante una entidad de servicio que agrega la entrada del servidor "arbitraje". No se puede establecer en las sesiones "grandes", pero requiere "juego". | es true, false | falso
hasOwners | Indica que la sesión tiene una directiva de seguridad basada en determinados miembros que se va a los propietarios. | es true, false | falso
que se pueden buscar | Indica que la sesión puede ser una sesión de destino de un identificador de búsqueda. Si se establece la capacidad de 'userAuthorizationStyle', la capacidad de "permitebúsqueda" no puede establecerse si no se establece la capacidad de 'hasOwners'. | es true, false | falso

Por ejemplo:

```json
"capabilities": {
    "connectivity": true,  
    "suppressPresenceActivityCheck": true,
    "gameplay": true,
    "large": true,
    "connectionRequiredForActiveMembers": true,
    "cloudCompute": true,
    "autoPopulateServerCandidates": true,
    "userAuthorizationStyle": true,
    "crossPlay": true,  
    "broadcast": true,  
    "team": true,   
    "arbitration": true,   
    "hasOwners": true,   
    "searchable": true  
},
```

## <a name="metrics"></a>Métricas
Si el `metrics` propiedades no se especifican, su valor predeterminado para los valores que son necesarios para satisfacer los requisitos de calidad de servicio.  
Si se especifican, los valores deben ser suficientes para satisfacer los requisitos de calidad de servicio.
Este elemento solo es válida si la sesión tiene el `connectivity` conjunto de funcionalidad.

Métrica | Descripción | Valores válidos | valor predeterminado
-- | -- | -- | --
latencia | | es true, false | Consulte la descripción
bandwidthDown | | es true, false | Consulte la descripción
bandwidthUp | | es true, false | Consulte la descripción
Personalizado | | es true, false | Consulte la descripción

Por ejemplo:
```json
"metrics": {
    "latency": true,
    "bandwidthDown": true,
    "bandwidthUp": true,
    "custom": true
},
```

## <a name="memberinitialization"></a>memberInitialization
Si un `memberInitialization` se establece el objeto, la sesión espera que el sistema cliente o el título a realizar la inicialización después de la creación de la sesión o como miembros nuevos unirse a la sesión.  
Automáticamente se realiza el seguimiento de las fases de inicialización y tiempos de espera de la sesión, incluidas las medidas de calidad de servicio si se establecen todas las métricas.  
Estos tiempos de espera invalidación reserva y listo tiempos de espera de la sesión de los miembros que tengan 'initializationEpisode' establecido.  
No se puede especificar en las sesiones de gran tamaño.

Elemento  | Descripción | Valores válidos | valor predeterminado
-- | -- | -- | --
joinTimeout | Indica el número de milisegundos que tiene un miembro a unirse a la sesión. Se quitan las reservas de direcciones de los usuarios que no se pueden combinar.</br>**Nota:** La duración predeterminada es suficiente para la ejecución normal del título, pero puede dar lugar a unir los tiempos de espera si se está depurando un título durante el flujo MPSD. Para estos escenarios invalidar y aumentar este valor predeterminado para la sesión.| 0 - n | 10000
measurementTimeout | Indica el número de milisegundos que un miembro de la sesión tiene que cargar las medidas. Un miembro que se produce un error al cargar las medidas se marca con un motivo del error de "tiempo de espera".  | 0 - n | 30000
evaluationTimeout | Indica el número de milisegundos que una evaluación externa tiene que cargar las medidas. | 0 - n | 5000
externalEvaluation | Si es true, indica que el código de título realiza la evaluación de que una combinación basada en las mediciones de calidad de servicio. El servicio de varios jugadores no realiza ninguna lógica de calidad de servicio y el título es responsable de avanzar a la fase de inicialización. Los títulos no normalmente necesitan esta regla. | es true, false | falso
membersNeededToStart | El número de miembros que sean necesarios para iniciar la sesión para el episodio de inicialización cero solo. | 1 - maxMembersCount | 1

Por ejemplo:
```json
"memberInitialization": {
    "joinTimeout": 10000,  
    "measurementTimeout": 30000,  
    "evaluationTimeout": 5000,
    "externalEvaluation": false,
    "membersNeededToStart": 1
},
```


## <a name="peertopeerrequirements"></a>peerToPeerRequirements

requisitos de red punto a punto | Descripción | valor predeterminado
-- | -- |--
latencyMaximum | La latencia máxima, en milisegundos, entre los dos clientes. | 250
bandwidthMinimum | El ancho de banda mínimo en kilobits por segundo entre los dos clientes. | 10000

Por ejemplo:
```json
"peerToPeerRequirements": {
    "latencyMaximum": 250,  
    "bandwidthMinimum": 10000
},
```


## <a name="peertohostrequirements"></a>peerToHostRequirements

requisitos de red de host de mismo nivel | Descripción | Valores válidos | valor predeterminado
-- | -- | -- | --
latencyMaximum | La latencia máxima, en milisegundos, para la conexión con el host del mismo nivel. | | 250
bandwidthDownMinimum | El ancho de banda mínimo en kilobits por segundo para la información enviada desde el host al mismo nivel. | | 100000
bandwidthUpMinimum | El ancho de banda mínimo en kilobits por segundo para la información enviada desde el mismo nivel en el host. | | 1000
hostSelectionMetric | Indica qué métrica se utiliza para seleccionar el host. | bandwidthup, bandwidthdown, ancho de banda y latencia | latencia

Por ejemplo:
```json
"peerToHostRequirements": {
    "latencyMaximum": 250,
    "bandwidthDownMinimum": 100000,
    "bandwidthUpMinimum": 1000,  
    "hostSelectionMetric": "bandwidthup"
},
```

## <a name="measurementserveraddresses"></a>measurementServerAddresses
El conjunto de posibles cadenas de conexión de servidor que se debe evaluar. Las cadenas de conexión deben estar en minúsculas.
No se puede especificar en las sesiones de gran tamaño.

Las cadenas de conexión se definen en el formato siguiente:

`"<server name>" : {deviceAddress}`

Donde la dirección del dispositivo se describe a continuación:

cadena de conexión de servidor | Descripción
-- | --
secureDeviceAddress | Base 64 codificado de dirección de dispositivo de seguridad del servidor

Por ejemplo:
```json
"measurementServerAddresses": {
    "server farm a": {
        "secureDeviceAddress": "r5Y="
    },
    "datacenter b": {
        "secureDeviceAddress": "rwY="
    }
},
```

## <a name="cloudcomputepackage"></a>cloudComputePackage
Especifica el que paquete para asignar de proceso de las propiedades de la nube. Requiere que el `cloudCompute` capacidad está establecida.

propiedad de proceso en la nube | Descripción
-- | -- | -- | --
titleId | Indica el título del paquete para asignar de proceso de Id. de la nube.
gsiSet | Indica el conjunto GSI del paquete de proceso en la nube para asignar.
Variant | Indica la variante del paquete de proceso en la nube para asignar.

Por ejemplo:
```json
"cloudComputePackage": {
    "titleId": "4567",
    "gsiSet": "128ce92a-45d0-4319-8a7e-bd8e940114ec",
    "vaiant": "30ebca60-d96e-4629-930b-6957aa6bfbfa"
},
```

## <a name="arbitration"></a>Arbitraje
Especifica los tiempos de espera para el proceso de arbitraje. Requiere que el `arbitration` capacidad está establecida. La hora de inicio de arbitraje se define en una sesión en el */servers/arbitration/constants/system/startTime* elemento.

timeout | Descripción | Valores válidos | valor predeterminado
-- | -- | -- | --
forfeitTimeout | Indica el tiempo, en milisegundos desde la hora de inicio de arbitraje que un TBD | 0 - n | 60000
arbitrationTimeout | Indica el tiempo, en milisegundos desde la hora de inicio de arbitraje, que el resultado de arbitraje agota el tiempo. Este valor no puede ser menor que el `forfeitTimeout` valor | 0 - n | 300000

Por ejemplo:
```json
"arbitration": {
    "forfeitTimeout": 60000,
    "arbitrationTimeout": 300000
},
```

## <a name="broadcastviewertitleids"></a>broadcastViewerTitleIds

Especifica una matriz de identificadores de los títulos que siempre deben tener acceso de lectura a la sesión de difusión de título.

Por ejemplo:
```json
"broadcastViewerTitleIds" : ["34567", "8910"],
```

## <a name="ownershippolicies"></a>ownershipPolicies
Especifica cómo controlar una sesión cuando sale el último propietario de la sesión. Requiere que el `hasOwners` capacidad está establecida.

Directiva de la propiedad | Descripción | Valores válidos | valor predeterminado
-- | -- | -- | --
Migración | Indica el comportamiento que se produce cuando el propietario de la última abandona la sesión. Si la directiva de migración se establece en "endsession", expire la sesión. Si la directiva de migración se establece en "más antiguo", seleccione el miembro con el tiempo más antiguo de combinación para convertirse en el nuevo propietario de la sesión. | "más antiguo", "endsession" | "endsession"

Por ejemplo:
```json
"ownershipPolicies": {
     "migration": "oldest"
}
```
