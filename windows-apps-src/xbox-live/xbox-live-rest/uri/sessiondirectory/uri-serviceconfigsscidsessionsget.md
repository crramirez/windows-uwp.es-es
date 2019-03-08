---
title: GET (/serviceconfigs/{scid}/sessions)
assetID: adc65d0b-58dd-bfb9-54c8-9bc9d02e68ec
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidsessionsget.html
description: " GET (/serviceconfigs/{scid}/sessions)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e54c9cd68a899cfd040bc3e16a05f6deb2daa7c3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614110"
---
# <a name="get-serviceconfigsscidsessions"></a>GET (/serviceconfigs/{scid}/sessions)
Recupera información de la sesión el especificado.

> [!IMPORTANT]
> Ahora, este método requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EKB)
  * [Códigos de estado HTTP](#ID4EXB)
  * [Cuerpo de la solicitud](#ID4EAC)
  * [Cuerpo de respuesta](#ID4ELC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST recupera información de sesión para los filtros proporcionados. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsAsync**.


> [!NOTE] 
> Para varios jugadores de 2015, se llama a este método <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetSessionsForUsersFilterAsync</b>.  



> [!NOTE] 
> Cada llamada a este método debe incluir una palabra clave, un filtro de Id. de usuario de Xbox o ambos. Si el llamador no tiene los permisos correctos para el <i>privada</i> y <i>reservas</i> parámetros, el método devuelve un código de error de 403 Prohibido, si las sesiones de este tipo realmente existen o no.  


<a id="ID4EKB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| scid| GUID| Identificador de configuración de servicio (¿SCID). Identificador de la parte 1 de la sesión.|
| Palabra clave| string| Una palabra clave utilizada para filtrar los resultados a solo las sesiones identificados con esa cadena.|
| xuid| GUID| Identificadores de usuario de Xbox para los usuarios para que se van a recuperar las sesiones. Los usuarios deben estar activos en las sesiones.|
| reservas de direcciones| string| Valor que indica si la lista de sesiones incluye aquellas que los usuarios no han aceptado. Este parámetro solo se puede establecer en true. Esta configuración requiere que el llamador tenga acceso de nivel de servidor a la sesión, o puede solicitar XUID del llamador para que coincida con el filtro de Id. de usuario de Xbox. |
| inactivo| string| Valor que indica si la lista de sesiones incluye aquellas que los usuarios han aceptado, pero no se reproducción activamente. Este parámetro solo se puede establecer en true.|
| privado| string| Valor que indica si la lista de sesiones incluye sesiones privadas. Este parámetro solo se puede establecer en true. Es válido solo cuando se consultan sus propias sesiones o al consultar el servidor a servidor. Si establece este parámetro en true requiere que el llamador tener acceso de nivel de servidor a la sesión, o puede solicitar XUID del llamador para que coincida con el filtro de Id. de usuario de Xbox. |
| visibility| string| Valor de enumeración que indica el estado de visibilidad que se usa en el filtrado de resultados. Actualmente este parámetro solo puede establecerse a Open para incluir las sesiones abiertas. Consulte <b>MultiplayerSessionVisibility</b>.|
| version| string| Un entero positivo que indica la versión de sesión principal o inferior de las sesiones que incluyen. El valor debe ser menor o igual a la versión de contrato de la solicitud de módulo 100.|
| Take| string| Un entero positivo que indica el número máximo de sesiones para recuperar.|

<a id="ID4EXB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EAC"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4ELC"></a>


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


<a id="ID4EWC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EYC"></a>


##### <a name="parent"></a>Primario

[/serviceconfigs/{scid}/sessions](uri-serviceconfigsscidsessions.md)
