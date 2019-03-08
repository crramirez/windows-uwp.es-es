---
title: GET (/untrustedplatform/users/xuid({xuid})/scids/{scid})
assetID: ef295295-fee1-b247-2a45-3accf2816fd2
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidsscid-get.html
description: " GET (/untrustedplatform/users/xuid({xuid})/scids/{scid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 50b91334b184114cd86e07574142768d63f747f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618130"
---
# <a name="get-untrustedplatformusersxuidxuidscidsscid"></a>GET (/untrustedplatform/users/xuid({xuid})/scids/{scid})
Recupera información de cuota de este tipo de almacenamiento. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Autorización](#ID4ECB)
  * [Encabezados de solicitud necesarios](#ID4ENB)
  * [Cuerpo de la solicitud](#ID4EWC)
  * [Códigos de estado HTTP](#ID4EBD)
  * [Cuerpo de respuesta](#ID4EUAAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero de 64 bits sin signo| El usuario Xbox ID (XUID) del Reproductor que realiza la solicitud.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorización
 
La solicitud debe incluir un encabezado de autorización válido de Xbox LIVE. Si el llamador no puede acceder a este recurso, el servicio devolverá una respuesta 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devolverá una respuesta 401 no autorizado. 
  
<a id="ID4ENB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación del STS. STSTokenString se sustituye por el token devuelto por la solicitud de autenticación. Para obtener más información sobre cómo recuperar un token STS y crear un encabezado de autorización, consulte autenticación y autorización de Xbox LIVE las solicitudes de servicios.| 
  
<a id="ID4EWC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EBD"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar | La solicitud es correcta.| 
| 201| Creación | Se creó la entidad.| 
| 400| solicitud incorrecta | Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización | La solicitud requiere autenticación del usuario.| 
| 403| prohibido | No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado | No se encontró el recurso especificado.| 
| 406| No es aceptable | No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud | La solicitud tardó demasiada en completarse.| 
| 500| error interno del servidor | El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible | Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EUAAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si la llamada es correcta, el servicio devolverá una [quotaInfo](../../json/json-quota.md) objeto.
 
<a id="ID4ECBAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
  "quotaInfo" :
  {
    "usedBytes" : 619,
    "quotaBytes" : 16777216
  }
}
         
```

   
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Primario 

[/untrustedplatform/users/xuid({xuid})/scids/{scid}](uri-untrustedplatformusersxuidscidsscid.md)

  
<a id="ID4E1BAC"></a>

 
##### <a name="reference"></a>Referencia 

[quotaInfo (JSON)](../../json/json-quota.md)

   