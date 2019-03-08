---
title: GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
assetID: 890e3f93-4fdc-955f-d849-ba9579d5c1eb
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsgetvaluemetadata.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 55ad44d4c29a2d7a43c76c4df2a78e08462fa65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612720"
---
# <a name="get-usersxuidxuidscidsscidstatsincludevaluemetadata"></a>GET (/users/xuid({xuid})/scids/{scid}/stats?include=valuemetadata)
Obtiene una lista de estadísticas especificado, incluidos los metadatos asociados con los valores de estadística para un usuario en una configuración de servicio especificado.
Es el dominio para estos URI `userstats.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EAB)
  * [Parámetros de cadena de consulta](#ID4ELB)
  * [Autorización](#ID4EWC)
  * [Encabezados de solicitud necesarios](#ID4ERD)
  * [Encabezados de solicitud opcionales](#ID4EDF)
  * [Cuerpo de la solicitud](#ID4EHG)
  * [Códigos de estado HTTP](#ID4ESG)
  * [Cuerpo de respuesta](#ID4EJCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

El? incluyen = valuemetadata parámetro de consulta permite la respuesta incluir los metadatos asociados a los valores de stat de usuario, como el modelo y el color de un automóvil que se utiliza para lograr un tiempo en una pista de carreras.

Para incluir los metadatos de valor en la respuesta, la llamada de solicitud también debe establecer el valor del encabezado X-Xbl-contrato-Version a 3.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| GUID| Id. de usuario de Xbox (XUID) del usuario en cuyo nombre para tener acceso a la configuración del servicio.|
| scid| GUID| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| statNames| string| Lista de nombres de usuario estadística delimitada por comas. Por ejemplo, el siguiente URI se informa al servicio que se solicitan las estadísticas de cuatro en nombre de la Id. de usuario especificado en el URI. {:: nomakrdown}<br/><br/>`https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots?include=valuemetadata`| 
| include=valuemetadata| string| Indica que la respuesta incluye los metadatos de valor asociado con los valores de stat use.|

<a id="ID4EWC"></a>


## <a name="authorization"></a>Autorización

No hay lógica de autorización que se implementa para escenarios de aislamiento de contenido y control de acceso.

   * Las estadísticas de tablas de clasificación y usuario se pueden leer desde los clientes en cualquier plataforma, siempre que el llamador envía un token XSTS válido con la solicitud. Operaciones de escritura se limitan a los clientes compatibles con la plataforma de datos.
   * Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.

Pseudocódigo para la comprobación tiene este aspecto:


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4ERD"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| X-Xbl-Contract-Version| string| Indica qué versión de la API para usar. Este valor debe establecerse en "3" con el fin de incluir los metadatos de valor en la respuesta.|

<a id="ID4EDF"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nombre o número del servicio al que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones del token de autenticación y así sucesivamente. Valor predeterminado: 1.|

<a id="ID4EHG"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4ESG"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 304| No modificado| Recursos no ha sido modificado desde la última solicitado.|
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|
| 401| Sin autorización| La solicitud requiere autenticación del usuario.|
| 403| prohibido| No se permite la solicitud para el usuario o servicio.|
| 404| No encontrado| No se encontró el recurso especificado.|
| 406| No es aceptable| No se admite la versión del recurso.|
| 408| Tiempo de espera de solicitud| No se admite la versión del recurso; se debe rechazar el nivel de MVC.|

<a id="ID4EJCAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EPCAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
  "user": {
    "xuid": "123456789",
    "gamertag": "WarriorSaint",
    "stats": [
      {
        "statname": "Wins",
        "type": "Integer",
        "value": 40,
        "valuemetadata" : "{\"region\" : \"EU\", \"isRanked\" : true}"
      },
      {
        "statname": "Kills",
        "type": "Integer",
        "value": 700,
        "valuemetadata" : "{\"longestKillStreak" : 15, \"favoriteTarget\" : \"CrazyPigeon\"}"
      },
      {
        "statname": "KDRatio",
        "type": "Double",
        "value": 2.23,
        "valuemetadata" : "{\"totalKills\" : 700, \"totalDeaths\" : 314}"
      },
      {
        "statname": "Headshots",
        "type": "Integer",
        "value": 173,
        "valuemetadata" : ""
      }
    ],
  }
}

```


<a id="ID4EZCAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E2CAC"></a>


##### <a name="parent"></a>Primario

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
