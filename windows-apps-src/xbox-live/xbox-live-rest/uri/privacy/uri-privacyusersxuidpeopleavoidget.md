---
title: GET (/users/{ownerId}/people/avoid)
assetID: e3420658-4738-8e80-44da-8281726fce01
permalink: en-us/docs/xboxlive/rest/uri-privacyusersxuidpeopleavoidget.html
description: " GET (/users/{ownerId}/people/avoid)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 745893b4b975b5fbf64fe76591ec15d18af59d73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641620"
---
# <a name="get-usersowneridpeopleavoid"></a>GET (/users/{ownerId}/people/avoid)
Obtiene la lista evite para un usuario.

  * [Comentarios](#ID4EQ)
  * [Parámetros de URI](#ID4EZ)
  * [Autorización](#ID4EEB)
  * [Encabezados de solicitud necesarios](#ID4EJC)
  * [Códigos de estado HTTP](#ID4EYD)
  * [Encabezados de respuesta necesaria](#ID4E1F)
  * [Cuerpo de respuesta](#ID4ESH)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Observaciones

Si se especifica un destino, devuelve ese usuario solo si están en la lista de bloqueo, o vacío si no están.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Obligatorio. Identificador del usuario cuyo recurso se está obteniendo acceso. Los valores posibles son <code>xuid({xuid})</code>. Debe ser el usuario autenticado. Valor de ejemplo: <code>xuid(2603643534573581)</code>. Tamaño máximo: ninguno. |

<a id="ID4EEB"></a>


## <a name="authorization"></a>Autorización

Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entero con signo de 64 bits| sí| 1234567890|

<a id="ID4EJC"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización | string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: <code>Xauth=&lt;authtoken></code>. Tamaño máximo: ninguno.|
| Aceptar| string| Tipos de contenido que son aceptables. Valor de ejemplo: <code>application/json</code>. Tamaño máximo: ninguno.|

<a id="ID4EYD"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 400| solicitud incorrecta| El identificador de destino especificado en el URI no es válido.|
| 403| prohibido| El propietario especificado en el URI no es el usuario autenticado.|
| 404| No encontrado| El propietario especificado en el URI no existe.|

<a id="ID4E1F"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| El tipo MIME del cuerpo de la solicitud. Valor de ejemplo: <code>application/json</code>. Tamaño máximo: ninguno.|
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: 34. Tamaño máximo: ninguno.|
| Cache-Control| string| Solicitud normal desde el servidor para especificar el comportamiento de almacenamiento en caché. Valor de ejemplo: <code>application/json</code>. Tamaño máximo: ninguno.|

<a id="ID4ESH"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EYH"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EDAAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EFAAC"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/people/avoid](uri-privacyusersxuidpeopleavoid.md)
