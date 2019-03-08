---
title: GET (/users/{ownerId}/people/mute)
assetID: 49b6c830-95f7-3200-0e46-0a1af573971c
permalink: en-us/docs/xboxlive/rest/uri-privacyusersowneridpeoplemuteget.html
description: " GET (/users/{ownerId}/people/mute)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 94e2bf4d04619ffa3348ae08fc37964cdc58e7b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661590"
---
# <a name="get-usersowneridpeoplemute"></a>GET (/users/{ownerId}/people/mute)
Obtiene la lista de silencio de un usuario.

  * [Comentarios](#ID4EQ)
  * [Parámetros de URI](#ID4EZ)
  * [Efecto de la configuración de privacidad en recurso](#ID4EEB)
  * [Autorización](#ID4ENB)
  * [Encabezados de solicitud necesarios](#ID4ESC)
  * [Cuerpo de la solicitud](#ID4EPE)
  * [Códigos de estado HTTP](#ID4E1E)
  * [Encabezados de respuesta necesaria](#ID4E3G)
  * [Cuerpo de respuesta](#ID4ETAAC)

<a id="ID4EQ"></a>


## <a name="remarks"></a>Observaciones

Si se especifica un destino, este URI devuelve solo ese usuario si el usuario está en la lista de silencio, o vacío si el usuario no está.

<a id="ID4EZ"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Obligatorio. Identificador del usuario cuyo recurso se está obteniendo acceso. Los valores posibles son "me" <code>xuid({xuid})</code>, o gt({gamertag}). Debe ser el usuario autenticado. Valores de ejemplo: <code>xuid(2603643534573581)</code>, <code>gt(SomeGamertag)</code>. Tamaño máximo: ninguno. |

<a id="ID4EEB"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Ninguno.

<a id="ID4ENB"></a>


## <a name="authorization"></a>Autorización

Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo|
| --- | --- | --- | --- | --- | --- | --- |
| Xuid| entero con signo de 64 bits| sí| 1234567890|

<a id="ID4ESC"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización | string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: <code>Xauth=&lt;authtoken></code>. Tamaño máximo: ninguno.|
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autorización y así sucesivamente. Valores de ejemplo: <code>1</code>, <code>vnext</code>. Tamaño máximo: ninguno.|
| Aceptar| string| Tipos de contenido que son aceptables. Valor de ejemplo: <code>application/json</code>. Tamaño máximo: ninguno.|

<a id="ID4EPE"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E1E"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| Solicitud correcta de la lista de silencio.|
| 400| solicitud incorrecta| El identificador de destino especificado en el URI no es válido.|
| 403| prohibido| El propietario especificado en el URI no es el usuario autenticado.|
| 404| No encontrado| El propietario especificado en el URI no existe.|

<a id="ID4E3G"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| El tipo MIME del cuerpo de la solicitud. Valor de ejemplo: <code>application/json</code>|
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: 34|
| Cache-Control| string| Solicitud normal desde el servidor para especificar el comportamiento de almacenamiento en caché. Ejemplo: <code>no-cache, no-store</code>|

<a id="ID4ETAAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EZAAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo

Consulte [UserList](../../json/json-userlist.md).


```cpp
{
    "users":
    [
        { "xuid":"12345" },
        { "xuid":"23456" }
    ]
}

```


<a id="ID4EJBAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ELBAC"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/people/mute](uri-privacyusersowneridpeoplemute.md)
