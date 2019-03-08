---
title: POST (/serviceconfigs/{scid}/batch)
assetID: b821a6eb-1add-ef91-bdf5-10e107082197
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidbatchpost.html
description: " POST (/serviceconfigs/{scid}/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e718fda26264667a7bf08c9254a462eb268dcd74
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603450"
---
# <a name="post-serviceconfigsscidbatch"></a>POST (/serviceconfigs/{scid}/batch)
Crea una consulta por lotes varios Xbox los identificadores de usuario para la configuración del servicio.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4ELB)
  * [Parámetros de cadena de consulta](#ID4EVB)
  * [Códigos de estado HTTP](#ID4EGF)
  * [Cuerpo de la solicitud](#ID4ENF)
  * [Cuerpo de respuesta](#ID4EWF)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST crea consultas por lotes varios Xbox los identificadores de usuario en la configuración del servicio de nivel de Id. (¿SCID). Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync**.

Para varios jugadores de 2015, puede combinar muchas consultas en una sola llamada si todas las consultas son el mismo pero trabaja con distintos identificadores de usuario de Xbox (XUIDs), como se representa en el *xuid* parámetro de cadena de consulta. Tenga en cuenta que los parámetros de cadena de consulta y las respuestas, son los mismos para las consultas normales y consultas por lotes.

Para una consulta por lotes, las sesiones que pertenecen a cada XUID se escriben en la respuesta en el mismo orden que el *xuid* parámetros se presentan en la solicitud. Es posible que la misma sesión aparezca varias veces en la respuesta, una vez para cada *xuid* que coincida con.

<a id="ID4ELB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.|

<a id="ID4EVB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

Una consulta puede modificarse mediante los parámetros de cadena de consulta en la tabla siguiente.

| <b>Parámetro</b>| <b>Tipo</b>| <b>Descripción</b>|
| --- | --- | --- | --- | --- | --- | --- |
| Palabra clave| string| Una palabra clave, por ejemplo, "foo", que se debe encontrar en las sesiones o plantillas si van a recuperar. |
| xuid| entero sin signo de 64 bits| El identificador de usuario de Xbox, por ejemplo, "123", para las sesiones incluir en la consulta. De forma predeterminada, el usuario debe ser activo en la sesión para que esta se incluyera. |
| reservas de direcciones| boolean| True para incluir las sesiones para el que el usuario se establece como un reproductor reservado pero no se ha unido para convertirse en un Reproductor de active. Este parámetro solo se usa al consultar sus propias sesiones, o cuando se consultan las sesiones de para servidores un usuario específico. |
| inactivo| boolean| True para incluir las sesiones que el usuario ha aceptado, pero no se reproduce activamente. Las sesiones en que el usuario es "listo" pero no "activo" cuentan como inactivas. |
| privado| boolean| True para incluir las sesiones privadas. Este parámetro solo se usa al consultar sus propias sesiones, o cuando se consultan las sesiones de para servidores un usuario específico. |
| visibility| string| Estado de visibilidad para las sesiones. Los valores posibles se definen mediante la <b>MultiplayerSessionVisibility</b>. Si este parámetro se establece en "open", la consulta debe incluir las sesiones abiertas solo. Si se establece en "private", la <i>privada</i> parámetro debe establecerse en true. |
| version| entero de 32 bits con signo| La versión máxima de la sesión que se debe incluir. Por ejemplo, un valor de 2 especifica que solo sesiones con una versión de sesión principal de 2 o menos se debe incluir. El número de versión debe ser menor o igual que la versión mod 100 de la solicitud contrato. |
| Take| entero de 32 bits con signo| El número máximo de sesiones para recuperar. Este número debe estar entre 0 y 100. |


Configuración *privada* o *reservas* a true requiere acceso de nivel de servidor a la sesión. Como alternativa, esta configuración requiere XUID del llamador de notificaciones para que coincida con el filtro XUID en el URI. En caso contrario, se devuelve el código de estado HTTP/403, si las sesiones de este tipo realmente existen o no.

<a id="ID4EGF"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4ENF"></a>


## <a name="request-body"></a>Cuerpo de la solicitud


```cpp
{ "xuids": [ "1234", "5678" ] }

```


<a id="ID4EWF"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

El valor devuelto de este método es una matriz JSON de referencias de la sesión, con algunos datos incluidos de sesión en línea.


```cpp
{
    "results": [ {
      "xuid": "9876",    // If the session was found from a xuid, that xuid.
      "startTime": "2009-06-15T13:45:30.0900000Z",
      "sessionRef": {
        "scid": "foo",
        "templateName": "bar",
        "name": "session-seven"
      },
      "accepted": 4,    // Approximate number of non-reserved members.
      "status": "active",    // or "reserved" or "inactive". This is the state of the user in the session, not the session itself. Only present if the session was found using a xuid.
      "visibility": "open",    // or "private", "visible", or "full"
      "joinRestriction": "local",    // or "followed". Only present if 'visibility' is "open" or "full" and the session has a join restriction.
      "myTurn": true,    // Not present is the same as 'false'. Only present if the session was found using a xuid.
      "keywords": [ "one", "two" ]
    }
  ]
}

```


<a id="ID4EDG"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EFG"></a>


##### <a name="parent"></a>Primario

[/serviceconfigs/{scid}/batch](uri-serviceconfigsscidbatch.md)
