---
title: POST (/users/xuid({xuid})/devices/current/titles/current)
assetID: d5e2d12d-ba75-2d04-2805-c69a4c143f73
permalink: en-us/docs/xboxlive/rest/uri-usersxuiddevicescurrenttitlescurrentpost.html
description: " POST (/users/xuid({xuid})/devices/current/titles/current)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b29a0bc796712d7b7c44a1fe8512f99bf09eb4fc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649530"
---
# <a name="post-usersxuidxuiddevicescurrenttitlescurrent"></a>POST (/users/xuid({xuid})/devices/current/titles/current)
Actualizar un título con la presencia de un usuario. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EEB)
  * [Autorización](#ID4EPB)
  * [Encabezados de solicitud necesarios](#ID4ENE)
  * [Encabezados de solicitud opcionales](#ID4ERG)
  * [Cuerpo de la solicitud](#ID4ERH)
  * [Cuerpo de respuesta](#ID4EKAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Este URI puede utilizarse por todos los títulos en plataformas no-console para agregar y actualizar la presencia, presencia enriquecida y datos de la presencia de medios para un título.
 
**SandboxId** se recuperan de la notificación en el XToken ahora y se aplican. Si el **SandboxId** no está presente, a continuación, servicios de detección de entretenimiento (EDS) producirá un error 400 Solicitud incorrecta.
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario de destino.| 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorización
 
| Tipo| Requerido| Descripción| Si falta de respuesta| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Sí| Id. de usuario de Xbox (XUID) del llamador| 403 Prohibido| 
| titleId| Sí| titleId del título| 403 Prohibido| 
| deviceId| Sí, para todos, excepto Windows y Web| deviceId del llamador| 403 Prohibido| 
| deviceType| Sí, para todo excepto Web| tipo de dispositivo del llamador| 403 Prohibido| 
| sandboxId| Sí para las llamadas procedentes de | espacio aislado del llamador| 403 Prohibido| 
  
<a id="ID4ENE"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.| 
| Content-Type| string| El tipo mime del cuerpo de la solicitud de valor de ejemplo: application/json.| 
| Content-Length| string| Longitud del cuerpo de la solicitud. Valor de ejemplo: 312.| 
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: presencebeta.xboxlive.com.| 
  
<a id="ID4ERG"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.| 
  
<a id="ID4ERH"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
El objeto de solicitud es un [TitleRequest](../../json/json-titlerequest.md). Se actualizan las propiedades realmente presentes en el cuerpo. Las propiedades que son no forma parte del cuerpo, pero existe en el servidor no se modificará.
 
<a id="ID4EAAAC"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
  id:"12341234",
  placement:"snapped",
  state:"active"
}
      
```

   
<a id="ID4EKAAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
En caso de éxito, un código de estado HTTP 200 o 201 creado se devuelve, según corresponda.
 
Si se produce un error (HTTP 4xx o 5xx), se devuelve información de error adecuada en el cuerpo de respuesta.
  
<a id="ID4EVAAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EXAAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

  
<a id="ID4EBBAC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   