---
title: PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
assetID: e3e4f164-ac5e-cbd9-8c05-2e1ac00dc55e
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.html
description: " PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d35b3f89f8b866a5236e8f5ac91eb37d9a82d306
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598560"
---
# <a name="put-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname"></a>PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})
Crea, actualiza o se une a una sesión.

> [!IMPORTANT]
> Este método URI requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EYB)
  * [Códigos de estado HTTP](#ID4EFC)
  * [Cuerpo de la solicitud](#ID4EOC)
  * [Cuerpo de respuesta](#ID4E4C)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST combinaciones, que crea o actualiza una sesión, dependiendo de qué subconjunto de la misma plantilla de cuerpo de solicitud JSON se envía. Si se ejecuta correctamente, devuelve un **MultiplayerSession** objeto que contiene la respuesta devuelta desde el servidor. Los atributos pueden ser diferentes de los atributos en el pasado **MultiplayerSession** objeto. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionAsync**.

Las operaciones de creación y actualización de sesión use PUT con un cuerpo de la aplicación/json que representa que se apliquen los cambios. Las operaciones son idempotentes, es decir, varias aplicaciones de los mismos cambios no tienen ningún efecto adicional.

El cuerpo de solicitud JSON refleja la estructura de datos de sesión. Todos los campos y subcampos son opcionales.

El formato para el método PUT crear la sesión o unirse a modo se muestra a continuación.

> [!NOTE]
> Tenga cuidado con este patrón. Actualiza se aplican a ciegas, independientemente de su estado actual de la sesión.



```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



A continuación, se muestra el formato para el modo de actualización de sesión del método PUT.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001 HTTP/1.1
         Content-Type: application/json

```



El formato de conexión para el método PUT actualizar las propiedades de la sesión se muestra a continuación. Es equivalente a una operación PUT a la sesión de URI con un cuerpo no dejando nada, pero el objeto debajo como propiedades. La diferencia es que esta operación devuelve el código de error 404 no encontrado si la sesión no existe. Esta operación es compatible con el encabezado If-Match.

```cpp
PUT /serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/sessions/00000000-0000-0000-0000-000000000001/properties HTTP/1.1
         Content-Type: application/json

         { "system": { }, "custom": { } }

```



<a id="ID4EYB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.|
| sessionTemplateName| string| Nombre de la instancia actual de la plantilla de sesión. Parte 2 del identificador de sesión.|
| sessionName| GUID| Identificador único de la sesión. Parte 3 del identificador de sesión.|

<a id="ID4EFC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EOC"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

A continuación es un cuerpo de solicitud de ejemplo para crear o unirse a una sesión. Los siguientes miembros de cuerpo de la solicitud son opcionales. Otros posibles miembros están prohibidas en una solicitud.

| Miembro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Constantes| object| Configuración de solo lectura que se combina con la plantilla de sesión para generar las constantes para la sesión. |
| propiedades | object | Cambios que se combinarán en las propiedades de sesión.|
| members.me | object| Constantes y las propiedades que trabajan en forma muy como a sus homólogos de nivel superior. Cualquier método PUT requiere que el usuario sea miembro de la sesión y agrega el usuario si es necesario. Si "" se especifica como null, se quita el miembro que realiza la solicitud de la sesión. |
| miembros | object| Otros objetos que representan a los usuarios agregar a la sesión, ordenados por un índice de base cero. El número de miembros en una solicitud siempre comienza con 0, incluso si la sesión ya contiene a miembros. Los miembros se agregan a la sesión en el orden en que aparecen en la solicitud. Las propiedades de miembro solo pueden establecerse por el usuario a los que pertenecen. |
| servidores | object| Los valores que indican las actualizaciones y adiciones a la sesión del conjunto de participantes de servidor asociado. Si un servidor se especifica como null, se quita esa entrada del servidor de la sesión. |



```cpp
{
  "properties": {
    "custom": {"KANWE": "MGMSY"},
    "system": {}
  },
  "constants": {
    "custom": {},
    "system": {"visibility": "open"}
  },
  "members": {
    "reserve_0": {
    "constants": {
      "custom": {"type": "leader"},
      "system": {"xuid": "5500461"} }}
   }
}

```


<a id="ID4E4C"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Cuerpo de respuesta de ejemplo para crear o unirse a una sesión:


```cpp
{
  "contractVersion": 104,
  "correlationId": "0FE81338-EE96-46E3-A3B5-2DBBD6C41C3B",
  "nextTimer": "2009-06-15T13:45:30.0900000Z",

  "initializing": {
    "stage": "measuring",
    "stageStartTime": "2009-06-15T13:45:30.0900000Z",
    "episode": 1
  },

  "hostCandidates": [ "ab90a362", "99582e67" ],

  "constants": {
    "system": {"visibility": "open"},
    "custom": {}
  },

  "properties": {
     "system": { "turn": [] },
     "custom": { "myProperty": "myValue" }
  },

  "members": {
      "1": {
        "properties": {
        "system": { },
        "custom": { }
      },

      "constants": {
        "system": { "xuid": "5500461" },
        "custom": { }
      }

      "gamertag": "stacy",
      "deviceToken": "9f4032ba7",
      "reserved": true,
      "activeTitleId": "8397267",
      "joinTime": "2009-06-15T13:45:30.0900000Z",
      "turn": true,
      "initializationFailure": "latency",
      "initializationEpisode": 1,
      "next": 4
    },
  },

  "membersInfo": {
      "first": 1,
      "next": 4,
      "count": 1,
      "accepted": 0
  },

  "servers": {
      "name": {
        "constants": { },
        "properties": { }
      }
  }
}

```


<a id="ID4EID"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EKD"></a>


##### <a name="parent"></a>Primario

[/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName}](uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionname.md)
