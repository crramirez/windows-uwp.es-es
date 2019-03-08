---
title: GET (/users/xuid({xuid})/achievements)
assetID: 381d49d1-7a4b-4a1e-1baf-cf674f7e0d54
permalink: en-us/docs/xboxlive/rest/uri-achievementsusersxuidachievementsgetv2.html
description: " GET (/users/xuid({xuid})/achievements)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 720170e88808db7ef95b88896fbca4f1cda4a091
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655270"
---
# <a name="get-usersxuidxuidachievements"></a>GET (/users/xuid({xuid})/achievements)
Obtiene la lista de los logros definidos en el título, los desbloqueado por el usuario o los que el usuario tiene en curso. Es el dominio para estos URI `achievements.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Parámetros de cadena de consulta](#ID4ECB)
  * [Autorización](#ID4ENF)
  * [Encabezados de solicitud necesarios](#ID4ESG)
  * [Encabezados de solicitud opcionales](#ID4ESH)
  * [Cuerpo de la solicitud](#ID4EIBAC)
  * [Cuerpo de respuesta](#ID4ETBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario está accediendo a cuyo (recurso). Debe coincidir con el XUID del usuario autenticado.| 
  
<a id="ID4ECB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Requerido| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| <b>skipItems</b>| No| entero de 32 bits con signo| Devolver elementos a partir de después del número especificado de elementos. Por ejemplo, <b>skipItems = "3"</b> recuperará los elementos recuperados a partir del cuarto elemento. | 
| <b>continuationToken</b>| No| string| Devolver los elementos comenzando en el token de continuación determinado. | 
| <b>maxItems</b>| No| entero de 32 bits con signo| Número máximo de elementos que se va a devolver de la colección, que se puede combinar con <b>skipItems</b> y <b>continuationToken</b> para devolver un intervalo de elementos. El servicio puede proporcionar un valor predeterminado si <b>maxItems</b> no está presente y es posible que devuelva menos de <b>maxItems</b>, incluso si aún no se ha devuelto la última página de resultados. | 
| <b>titleId</b>| No| string| Un filtro para los resultados devueltos. Acepta uno o varios identificadores de título decimal, delimitada por comas.| 
| <b>unlockedOnly</b>| No| Valor booleano| Filtrar los resultados devueltos. Si establece en <b>true</b>, le devuelven solo los logros desbloqueados para el usuario. El valor predeterminado es <b>false</b>.| 
| <b>possibleOnly</b>| No| Valor booleano| Filtrar los resultados devueltos. Si establece en <b>true</b>, devolverá todos los resultados posibles, pero no desbloqueada de sólo metadatos: la información de logro de XMS. El valor predeterminado es <b>false</b>.| 
| <b>types</b>| No| string| Un filtro para los resultados devueltos. Puede ser "Persistent" o "Desafío". Valor predeterminado es todos los tipos admitidos.| 
| <b>orderBy</b>| No| string| Especifica el orden en que se va a devolver los resultados. Puede ser "Desordenados", "Title", "UnlockTime" o "EndingSoon". El valor predeterminado es "Desordenados".| 
  
<a id="ID4ENF"></a>

 
## <a name="authorization"></a>Autorización
 
| Notificación| ¿Obligatorio?| Descripción| Comportamiento si falta| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Usuario| Autor de llamada es un usuario autorizado de Xbox LIVE.| El llamador debe ser un usuario válido en Xbox LIVE.| 403 Prohibido| 
  
<a id="ID4ESG"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4ESH"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor predeterminado: 1.| 
| <b>x-xbl-contract-version</b>| entero de 32 bits sin signo| Si está presente y establecido en 2, se usará la versión V2 de esta API. En caso contrario, V1.| 
| <b>Accept-Language</b>| string| Lista de configuraciones regionales deseadas y reservas (p. ej., fr-FR, fr, en-GB, en WW, en-US). El servicio logros funcionará a través de la lista hasta que encuentra una coincidencia de cadenas localizadas. Si se encuentra ninguna, intenta hacer coincidir la ubicación definida en el token de usuario, que procede de la dirección IP del usuario. Si todavía no hay cadenas localizadas coincidentes se encuentran, usa las cadenas predeterminadas proporcionadas por el desarrollador/Editor de título. | 
  
<a id="ID4EIBAC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4ETBAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si la llamada se realiza correctamente, el servicio devuelve una matriz de [mención especial (JSON)](../../json/json-achievementv2.md) objetos y una [PagingInfo (JSON)](../../json/json-paginginfo.md) objeto.
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "achievements":
    [{
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
                "achievementState":"Achieved",
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
        }],
        "pagingInfo":
        {
                "continuationToken":null,
                "totalRecords":1
        }
}
         
```

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/achievements](uri-achievementsusersxuidachievementsv2.md)

  
<a id="ID4E2CAC"></a>

 
##### <a name="reference"></a>Referencia 

[Mención especial (JSON)](../../json/json-achievementv2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Parámetros de paginación](../../additional/pagingparameters.md)

   