---
title: DELETE (/users/xuid({xuid})/devices/current/titles/current)
assetID: 3bf75247-0a2a-0e4c-afcc-9e7654a89648
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentdelete.html
description: " DELETE (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dd916fee5455276d45e4437e4ee90cacbde30bf9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604220"
---
# <a name="delete-usersxuidxuiddevicescurrenttitlescurrent"></a>DELETE (/users/xuid({xuid})/devices/current/titles/current)
Eliminar la presencia de un título de cierre, en lugar de esperar el [PresenceRecord](../../json/json-presencerecord.md) a punto de expirar. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EZ)
  * [Autorización](#ID4EEB)
  * [Encabezados de solicitud necesarios](#ID4ERD)
  * [Encabezados de solicitud opcionales](#ID4EVF)
  * [Cuerpo de la solicitud](#ID4EVG)
  * [Cuerpo de respuesta](#ID4EAH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario de destino.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorización
 
| Tipo| Requerido| Descripción| Si falta de respuesta| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Sí| Id. de usuario de Xbox (XUID) del llamador| 403 Prohibido| 
| titleId| Sí| titleId del título| 403 Prohibido| 
| deviceId| Sí, para todos, excepto Windows y Web| deviceId del llamador| 403 Prohibido| 
| deviceType| Sí, para todo excepto Web| tipo de dispositivo del llamador| 403 Prohibido| 
  
<a id="ID4ERD"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.| 
| Content-Type| string| El tipo mime del cuerpo de la solicitud de valor de ejemplo: application/json.| 
| Content-Length| string| Longitud del cuerpo de la solicitud. Valor de ejemplo: 312.| 
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: presencebeta.xboxlive.com.| 
  
<a id="ID4EVF"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.| 
  
<a id="ID4EVG"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EAH"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
En caso de éxito, se devuelve el código de estado HTTP sin cuerpo de respuesta.
 
En el caso de error (HTTP 4xx o 5xx), se devuelve información de error adecuada en el cuerpo de respuesta.
  
<a id="ID4ELH"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ENH"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

   