---
title: GET (/users/{ownerId}/summary)
assetID: 754190c9-b15d-f34b-1dca-5c92f6f67d12
permalink: en-us/docs/xboxlive/rest/uri-usersowneridsummaryget.html
description: " GET (/users/{ownerId}/summary)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3b228adab7b035ec8f4e65fc8b7458228a677987
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614000"
---
# <a name="get-usersowneridsummary"></a>GET (/users/{ownerId}/summary)
Obtiene los datos de resumen sobre el propietario desde la perspectiva del llamador.

  * [Parámetros de URI](#ID4EQ)
  * [Autorización](#ID4E2)
  * [Encabezados de solicitud necesarios](#ID4EBC)
  * [Encabezados de solicitud opcionales](#ID4EHD)
  * [Cuerpo de la solicitud](#ID4EXE)
  * [Códigos de estado HTTP](#ID4ECF)
  * [Encabezados de respuesta necesaria](#ID4EZG)
  * [Cuerpo de respuesta](#ID4EGAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}). Valores de ejemplo: <code>me</code>, <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>|

<a id="ID4E2"></a>


## <a name="authorization"></a>Autorización

| <b>Nombre</b>| <b>Tipo</b>| <b>Descripción</b>|
| --- | --- | --- | --- | --- | --- |
| xuid| entero sin signo de 64 bits| Obligatorio. identificador de usuario del llamador. Valor de ejemplo: 2533274790395904|

<a id="ID4EBC"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Datos de autorización de. Esto suele ser un token XSTS cifrado. Valor de ejemplo: <b>XBL3.0 x = [hash]; [token] </b>.|

<a id="ID4EHD"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| x-xbl-contract-version| string| Nombre o número del servicio al que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valores de ejemplo: 1|
| Aceptar| string| Tipos de contenido que son aceptables. Todas las respuestas serán <code>application/json</code>.|

<a id="ID4EXE"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4ECF"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 400| solicitud incorrecta| Los identificadores de usuario estaban mal formados.|
| 403| prohibido| No se pudo analizar la notificación XUID del encabezado de autorización.|

<a id="ID4EZG"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: 232.|
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Debe ser <b>application/json</b>.|

<a id="ID4EGAAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Consulte [PersonSummary (JSON)](../../json/json-personsummary.md).

<a id="ID4ESAAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}

```


<a id="ID4E3AAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E5AAC"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/summary](uri-usersowneridsummary.md)
