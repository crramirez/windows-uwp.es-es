---
title: GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
assetID: 27318886-f084-d6a8-e582-3eb070ccbc38
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementidget.html
description: " GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 19083937d48d8c312215f1734513d83c59f52f0d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627510"
---
# <a name="get-usersxuidxuidachievementsscidachievementid"></a>GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})
Obtiene los detalles de la realización. Es el dominio para estos URI `achievements.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
  * [Autorización](#ID4EAB)
  * [Efecto de la configuración de privacidad en recurso](#ID4E4C)
  * [Encabezados de solicitud necesarios](#ID4EPG)
  * [Encabezados de solicitud opcionales](#ID4EPH)
  * [Cuerpo de la solicitud](#ID4ECBAC)
  * [Códigos de estado HTTP](#ID4ENBAC)
  * [Cuerpo de respuesta](#ID4EBGAC)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el XUID del usuario autenticado.| 
| scid| GUID| Identificador único de la configuración del servicio cuyo cumplimiento se tiene acceso.| 
| achievementid| entero de 32 bits sin signo| Identificador único (dentro de la ¿SCID especificado) del logro que está accediendo.| 
  
<a id="ID4EAB"></a>

 
## <a name="authorization"></a>Autorización
 
Notificaciones de autorización usa | Notificación| ¿Obligatorio?| Descripción| Comportamiento si falta| 
| --- | --- | --- | --- | --- | --- | --- | 
| Usuario| Sí| Un usuario válido en Xbox LIVE para los que se está realizando la solicitud.| 403 Prohibido| 
| Título| No| El título que realiza la llamada.| Depende de AuthZ. A partir del 1 de mayo de 2013, AuthZ no proporciona una notificación cuando faltan y, por tanto, se denegará el acceso a cualquier SCIDs no marcados como públicos.| 
| Espacio aislado| No| El recinto de seguridad desde el que se deben recuperar los resultados.| Depende de AuthZ. A partir del 1 de mayo de 2013, AuthZ no proporciona una notificación de forma predeterminada cuando no tiene.| 
  
<a id="ID4E4C"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso
 
Efecto de la configuración de privacidad en recurso | Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Como se describe.| 
| Friend| Todo el mundo| Aceptar| 
| Friend| solo los amigos| Aceptar| 
| Friend| bloqueado| Está prohibido.| 
| usuario de no confianza| Todo el mundo| Aceptar| 
| usuario de no confianza| solo los amigos| Está prohibido.| 
| usuario de no confianza| bloqueado| Está prohibido.| 
| sitio de terceros| Todo el mundo| Aceptar| 
| sitio de terceros| solo los amigos| Está prohibido.| 
| sitio de terceros| bloqueado| Está prohibido.| 
  
<a id="ID4EPG"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4EPH"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.| 
| x-xbl-contract-version| string| El valor predeterminado es V1.| 
| Aceptar idioma| string| Lista de configuraciones regionales deseadas y reservas (p. ej., fr-FR, fr, en-GB, en WW, en-US). El servicio logros funcionará a través de la lista hasta que encuentra una coincidencia de cadenas localizadas. Si se encuentra ninguna, intenta hacer coincidir la ubicación definida en el token de usuario, que procede de la dirección IP del usuario. Si todavía no hay cadenas localizadas coincidentes se encuentran, usa las cadenas predeterminadas proporcionadas por el desarrollador/Editor de título. | 
  
<a id="ID4ECBAC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4ENBAC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 301| Movido permanentemente| El servicio se ha movido a otro URI.| 
| 307| Redirección temporal| Se cambió temporalmente el URI para este recurso.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso; se debe rechazar el nivel de MVC.| 
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.| 
| 410| Pasado| El recurso solicitado ya no está disponible.| 
  
<a id="ID4EBGAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EHGAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "achievement":
    {
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
                "requirements":null,
                "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }
}
         
```

   
<a id="ID4ERGAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ETGAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/achievements/{scid}/{achievementid}](uri-usersxuidachievementsscidachievementid.md)

   