---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632680"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
Obtiene a una persona por Id. de destino de colección de personas de la persona que llama. Es el dominio para estos URI `social.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Encabezados de solicitud necesarios](#ID4ERC)
  * [Encabezados de solicitud opcionales](#ID4EQD)
  * [Cuerpo de la solicitud](#ID4EWE)
  * [Códigos de estado HTTP](#ID4EBF)
  * [Encabezados de respuesta necesaria](#ID4EDH)
  * [Cuerpo de respuesta](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Las operaciones GET no modificación los recursos, por lo que esto producirá los mismos resultados si ejecuta una o varias veces.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).| 
| targetID| string| Identificador del usuario cuyos datos se recuperan de la lista de personas del propietario, un Id. de usuario de Xbox (XUID) o un nombre de jugador. Valores de ejemplo: xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Autorización
 
| Tipo| Requerido| Descripción| Si falta de respuesta| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| sí| Autor de llamada tiene el Id. de usuario de Xbox (XUID del usuario).| 401 no autorizado| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Cadena. Datos de autorización de Xbox LIVE. Esto suele ser un token XSTS cifrado. Valor de ejemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor predeterminado: 1.| 
| Aceptar| Cadena. Tipos de contenido que acepta el llamador en la respuesta. Todas las respuestas son <b>application/json</b>.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| Proceso completado correctamente.| 
| 400| solicitud incorrecta| Los identificadores de usuario estaban mal formados.| 
| 403| prohibido| No se pudo analizar la notificación XUID del encabezado de autorización.| 
| 404| No encontrado| No se encontró el usuario de destino en la lista de personas del propietario.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| entero de 32 bits sin signo| Longitud, en bytes, del cuerpo de respuesta. Valor de ejemplo: 22.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Esto siempre será <b>application/json</b>.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si la llamada se realiza correctamente, el servicio devuelve a la persona de destino. Consulte [persona (JSON)](../../json/json-person.md).
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   