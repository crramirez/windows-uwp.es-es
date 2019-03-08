---
title: POST (/users/xuid({xuid})/feedback)
assetID: 48a7a510-a658-f2a3-c6bc-28a3610006e7
permalink: en-us/docs/xboxlive/rest/uri-reputationusersxuidfeedbackpost.html
description: " POST (/users/xuid({xuid})/feedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d8a6e118811203fd34c310840e8688c2255c6beb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660050"
---
# <a name="post-usersxuidxuidfeedback"></a>POST (/users/xuid({xuid})/feedback)
Usar desde el título si así lo desea agregar una opción de comentarios en el juego, en lugar de usar el shell. Es el dominio para estos URI `reputation.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EZ)
  * [Encabezados de solicitud necesarios](#ID4EEB)
  * [Cuerpo de la solicitud](#ID4ENC)
  * [Encabezados obligatorios](#ID4EDE)
  * [Autorización](#ID4EXF)
  * [Códigos de estado HTTP](#ID4EEG)
  * [Cuerpo de respuesta](#ID4EZH)
 
<a id="ID4EZ"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| ulong| Id. de usuario de Xbox (XUID) del usuario que se va a notificar.| 
  
<a id="ID4EEB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 101.| 
  
<a id="ID4ENC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
<a id="ID4EVC"></a>

 
### <a name="required-members"></a>Miembros requeridos 
 
La solicitud debe contener un [comentarios](../../json/json-feedback.md) objeto. 
  
<a id="ID4EED"></a>

 
### <a name="prohibited-members"></a>Miembros prohibidos 
 
Todos los demás miembros están prohibidas en una solicitud.
  
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo 
 

```cpp
{
    "sessionRef":
    {
        "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
        "templateName": "CaptureFlag5",
        "name": "Halo556932",
    },
    "feedbackType": "CommsAbusiveVoice",
    "textReason": "He called me a doodoo-head!",
    "voiceReasonId": null,
    "evidenceId": null
}

      
```

   
<a id="ID4EDE"></a>

 
## <a name="required-headers"></a>Encabezados obligatorios
 
Los encabezados siguientes son necesarios al realizar una solicitud de servicios de Xbox Live.
 
| <b>Header</b>| <b>Valor</b>| <b>Deacription</b>| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación del STS. STSTokenString se sustituye por el token devuelto por la solicitud de autenticación.| 
Content-Type| 
application/json| 
Tipo de datos que se está enviadas.| 
  
<a id="ID4EXF"></a>

 
## <a name="authorization"></a>Autorización
 
La solicitud debe incluir un encabezado de autorización válido de Xbox Live. Si el llamador no tiene permiso para tener acceso a este recurso, el servicio devuelve un código 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devuelve un código 401 no autorizado.
  
<a id="ID4EEG"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| Sin contenido| La solicitud se ha completado, pero no tiene contenido para devolver.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.| 
| 409| Conflicto| Token de continuación ya no son válido.| 
  
<a id="ID4EZH"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta 
 
Si la llamada se realiza correctamente, se devuelve ningún objeto de esta respuesta. En caso contrario, el servicio devuelve un [depuracuión puede contener](../../json/json-serviceerror.md) objeto.
  
<a id="ID4EOAAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQAAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

  
<a id="ID4E3AAC"></a>

 
##### <a name="reference"></a>Referencia 

[Comentarios (JSON)](../../json/json-feedback.md)

 [Depuracuión puede contener (JSON)](../../json/json-serviceerror.md)

   