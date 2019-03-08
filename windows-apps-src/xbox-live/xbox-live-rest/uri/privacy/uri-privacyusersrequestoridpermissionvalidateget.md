---
title: GET (/users/{requestorId}/permission/validate)
assetID: 8d22c668-af9a-1d24-8d65-830c2ce913d7
permalink: en-us/docs/xboxlive/rest/uri-privacyusersrequestoridpermissionvalidateget.html
description: " GET (/users/{requestorId}/permission/validate)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 07ccac63b3e6681ea35405314b0cd8b93aa60f9a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594760"
---
# <a name="get-usersrequestoridpermissionvalidate"></a>GET (/users/{requestorId}/permission/validate)
Obtiene una respuesta Sí o no acerca de si el usuario puede realizar la acción especificada con un usuario de destino.

  * [Parámetros de URI](#ID4EQ)
  * [Parámetros de cadena de consulta](#ID4E2)
  * [Autorización](#ID4EDC)
  * [Encabezados de solicitud necesarios](#ID4EID)
  * [Cuerpo de la solicitud](#ID4ETE)
  * [Códigos de estado HTTP](#ID4E5E)
  * [Encabezados de respuesta necesaria](#ID4ETG)
  * [Cuerpo de respuesta](#ID4EKAAC)

<a id="ID4EQ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| requestorId| string| Obligatorio. Identificador del usuario que realiza la acción. Los valores posibles son <code>xuid({xuid})</code> y <code>me</code>. Esto debe ser un usuario ha iniciado sesión. Valor de ejemplo: <code>xuid(0987654321)</code>.|

<a id="ID4E2"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| Configuración de| enumeración de cadena| El valor PermissionId para comprobarla. Valor de ejemplo: "CommunicateUsingText".|
| target| string| Identificador de usuario en los que se realizará la acción. Los valores posibles son <code>xuid({xuid})</code>. Valores de ejemplo: <code>xuid(0987654321)</code>|

<a id="ID4EDC"></a>


## <a name="authorization"></a>Autorización

Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Xuid| entero con signo de 64 bits| sí| 1234567890|

<a id="ID4EID"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <code>XBL3.0 x=&lt;userhash>;&lt;token></code>|
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor de ejemplo: 1.|

<a id="ID4ETE"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 400| La solicitud no es válida.| Ejemplos: los identificadores de configuración incorrecta, los URI incorrecto, etcetera.|
| 404| El usuario especificado en el URI no existe.| No se encontró el recurso especificado.|

<a id="ID4ETG"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| El tipo MIME del cuerpo de la solicitud. Valor de ejemplo: <code>application/json</code>|
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: 34|
| Cache-Control| string| Solicitud normal desde el servidor para especificar el comportamiento de almacenamiento en caché. Ejemplo: <code>no-cache, no-store</code>|

<a id="ID4EKAAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Consulte [PermissionCheckResponse (JSON)](../../json/json-permissioncheckresponse.md).

<a id="ID4EWAAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}

```


<a id="ID4EABAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ECBAC"></a>


##### <a name="parent"></a>Primario

[/users/{requestorId}/permission/validate](uri-privacyusersrequestoridpermissionvalidate.md)

 [Enumeración PermissionId](../../enums/privacy-enum-permissionid.md)
