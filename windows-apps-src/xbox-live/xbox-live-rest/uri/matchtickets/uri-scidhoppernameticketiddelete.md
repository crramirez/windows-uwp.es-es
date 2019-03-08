---
title: DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})
assetID: d9ff3f21-aa70-af41-afa1-9a9244fcdb95
permalink: en-us/docs/xboxlive/rest/uri-scidhoppernameticketiddelete.html
description: " DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fdd28cb94b31102d9af98aa95afde45424dadce9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589670"
---
# <a name="delete-serviceconfigsscidhoppershoppernameticketsticketid"></a>DELETE (/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid})

Quita un vale de coincidencia.

> [!IMPORTANT]
> Este método está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4E2)
  * [Autorización](#ID4EGB)
  * [Códigos de estado HTTP](#ID4EOC)
  * [Cuerpo de la solicitud](#ID4EXC)
  * [Cuerpo de respuesta](#ID4ECD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST elimina el Id. de vale especificado de la hopper con nombre en la configuración del servicio de nivel de Id. (¿SCID). Este método se puede ajustar mediante **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.DeleteMatchTicketAsync**.  
<a id="ID4E2"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| name| string| El nombre de la hopper.|
| ticketId| GUID| El Id. de vale.|

<a id="ID4EGB"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (user ID)| sí| El usuario que realiza la solicitud debe ser un miembro de la sesión de vale que hace referencia el vale.| 403|
| Privilegios y el tipo de dispositivo| sí| Cuando se establece el tipo de dispositivo del usuario a la consola, se permiten solo los usuarios con el privilegio para varios jugadores en sus notificaciones para realizar llamadas para el servicio.| 403|

<a id="ID4EOC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EXC"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4ECD"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ningún objeto que se envía en el cuerpo de la respuesta.

<a id="ID4EPD"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ERD"></a>


##### <a name="parent"></a>Primario  

[/serviceconfigs/{scid}/hoppers/{hoppername}/tickets/{ticketid}](uri-scidhoppernameticketid.md)
