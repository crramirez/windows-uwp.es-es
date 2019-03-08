---
title: GET (/users/xuid({xuid})/scids/{scid}/stats)
assetID: af117e87-6f1d-6448-9adf-7cf890d1380f
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstatsget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: baf965dcbd23bf00d7d0953726f9f20852324e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662390"
---
# <a name="get-usersxuidxuidscidsscidstats"></a>GET (/users/xuid({xuid})/scids/{scid}/stats)
Obtiene una configuración de servicio con ámbito de una lista delimitada por comas de nombres de estadísticas de usuario en nombre del usuario especificado.
Es el dominio para estos URI `userstats.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EEB)
  * [Parámetros de cadena de consulta](#ID4EPB)
  * [Autorización](#ID4EUC)
  * [Encabezados de solicitud necesarios](#ID4EPD)
  * [Encabezados de solicitud opcionales](#ID4EYE)
  * [Cuerpo de la solicitud](#ID4E3F)
  * [Códigos de estado HTTP](#ID4EHG)
  * [Cuerpo de respuesta](#ID4E5BAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

los clientes necesitan una manera de leer y escribir las estadísticas de título en nombre de los jugadores en nuestro nuevo sistema de las estadísticas del Reproductor.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| GUID| Id. de usuario de Xbox (XUID) del usuario en cuyo nombre para tener acceso a la configuración del servicio.|
| scid| GUID| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.|

<a id="ID4EPB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| statNames| string| El único parámetro de cadena de consulta es el nombre URI delimitados por comas usuario estadística nombre. Por ejemplo, el siguiente URI se informa al servicio que se solicitan las estadísticas de cuatro en nombre de la Id. de usuario especificado en el URI. `https://userstats.xboxlive.com/users/xuid({xuid})/scids/{scid}/stats/wins,kills,kdratio,headshots`Habrá un límite en el número de estadísticas que se pueden solicitar en una sola llamada, y ese límite cuidadosamente considerará "idóneo" para desarrolladores vs comodidad. Viabilidad de longitud URI. Por ejemplo, el límite puede determinarse mediante cualquier 600 caracteres vale la pena de texto de nombre de estadística (incluidas las comas) o 10 estadísticas máximas. Habilitar una operación GET sencilla como esta permite el almacenamiento en caché de HTTP para las estadísticas solicitadas habitualmente, lo que reduce el volumen de llamadas de los clientes generen mucha conversación. |

<a id="ID4EUC"></a>


## <a name="authorization"></a>Autorización

No hay lógica de autorización que se implementa para escenarios de aislamiento de contenido y control de acceso.

   * Las estadísticas de tablas de clasificación y usuario se pueden leer desde los clientes en cualquier plataforma, siempre que el llamador envía un token XSTS válido con la solicitud. Escrituras son obviamente se limita a los clientes compatibles con la plataforma de datos.
   * Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.

Pseudocódigo para la comprobación tiene este aspecto:


```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.

```


<a id="ID4EPD"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|

<a id="ID4EYE"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nombre o número del servicio al que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones del token de autenticación y así sucesivamente. Valor predeterminado: 1.|

<a id="ID4E3F"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4EHG"></a>


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

<a id="ID4E5BAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EECAC"></a>


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
                "value": 40
            },
            {
                "statname": "Kills",
                "type": "Integer",
                "value": 700
            },
            {
                "statname": "KDRatio",
                "type": "Double",
                "value": 2.23
            },
            {
                "statname": "Headshots",
                "type": "Integer",
                "value": 173
            }
        ],
    }
}

```


<a id="ID4EOCAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EQCAC"></a>


##### <a name="parent"></a>Primario

[/users/xuid({xuid})/scids/{scid}/stats](uri-usersxuidscidsscidstats.md)
