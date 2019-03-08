---
title: GET (/users/{ownerId}/people)
assetID: c948d031-ec19-7571-31ef-23cb9c5ebfaf
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopleget.html
description: " GET (/users/{ownerId}/people)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6c8672188a93b2e8d27a081ae068387e7ee7aa42
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623770"
---
# <a name="get-usersowneridpeople"></a>GET (/users/{ownerId}/people)
Obtiene colección de personas de la persona que llama.
Es el dominio para estos URI `social.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E5)
  * [Parámetros de cadena de consulta](#ID4EJB)
  * [Autorización](#ID4ERD)
  * [Encabezados de solicitud necesarios](#ID4EZE)
  * [Encabezados de solicitud opcionales](#ID4EYF)
  * [Cuerpo de la solicitud](#ID4E5G)
  * [Códigos de estado HTTP](#ID4EJH)
  * [Encabezados de respuesta necesaria](#ID4EBBAC)
  * [Cuerpo de respuesta](#ID4ENCAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Las operaciones GET no modificación los recursos, por lo que esto producirá los mismos resultados si ejecuta una o varias veces.

<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).|

<a id="ID4EJB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| vista| string| Devolver la persona asociada a una vista. El valor predeterminado es "all". Los valores posibles son: <ul><li><b>Todos los</b> -devuelve todas las personas en la lista de personas del usuario. Este es el valor predeterminado.</li><li><b>Favoritos</b> -devuelve todas las personas en la lista del usuario las personas que tienen el atributo favorito.</li><li><b>LegacyXboxLiveFriends</b> -devuelve todas las personas en la lista del usuario las personas que también son heredados amigos de Xbox LIVE.</li></br>**Nota:**  Solo el **todas** se admite el valor si el usuario que realiza la llamada es diferente al usuario propietario.|
| startIndex| entero de 32 bits sin signo| Devolver los elementos a partir del índice especificado.  
| maxItems| entero de 32 bits sin signo| Número máximo de usuarios para devolver de la colección comenzando desde el índice inicial. El servicio puede proporcionar un valor predeterminado si <b>maxItems</b> no está presente y es posible que devuelva menos de <b>maxItems</b> (incluso si la última página de resultados todavía no se han devuelto).|

<a id="ID4ERD"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| sí| Autor de llamada tiene el Id. de usuario de Xbox (XUID del usuario).| 401 no autorizado|

<a id="ID4EZE"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| Cadena. Datos de autorización de Xbox LIVE. Esto suele ser un token XSTS cifrado. Valor de ejemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>.|

<a id="ID4EYF"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor predeterminado: 1.|
| Aceptar| Cadena. Tipos de contenido que acepta el llamador en la respuesta. Todas las respuestas son <b>application/json</b>.|

<a id="ID4E5G"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4EJH"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| Proceso completado correctamente.|
| 400| solicitud incorrecta| Parámetros de consulta o los identificadores de usuario eran tiene un formato incorrecto.|
| 403| prohibido| No se pudo analizar la notificación XUID del encabezado de autorización.|

<a id="ID4EBBAC"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| entero de 32 bits sin signo| Longitud, en bytes, del cuerpo de respuesta. Valor de ejemplo: 22.|
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Esto siempre será <b>application/json</b>.|

<a id="ID4ENCAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Si la llamada se realiza correctamente, el servicio devuelve el número total de personas en la colección de las personas del llamador y una matriz que contiene la colección de las personas del llamador. Consulte [PeopleList (JSON)](../../json/json-peoplelist.md).

<a id="ID4EZCAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFollowingCaller": false,
            "isFavorite": false
        },
    ],
    "totalCount": 3
}

```


<a id="ID4EDDAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EFDAC"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/people](uri-usersowneridpeople.md)
