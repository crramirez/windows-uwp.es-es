---
title: GET (/users/xuid({xuid})/inbox/{messageId})
assetID: d76563d0-2c74-0308-054b-762c80392a02
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageidget.html
description: " GET (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 29b4c57468148a431a10e0d74f85d360ff0992b3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618060"
---
# <a name="get-usersxuidxuidinboxmessageid"></a>GET (/users/xuid({xuid})/inbox/{messageId})
Recupera el texto del mensaje detallado para un mensaje de usuario en particular, marcándolo como de lectura en el servicio.
Es el dominio para estos URI `msg.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EEB)
  * [Autorización](#ID4ERB)
  * [Cuerpo de la solicitud](#ID4E3B)
  * [Efecto de la configuración de privacidad en recurso](#ID4EJC)
  * [Códigos de estado HTTP](#ID4EUC)
  * [Respuesta de objeto de JavaScript Notation (JSON)](#ID4EUE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Solo puede realizarse la operación get en los tipos de mensajes de usuario, sistema y FriendRequest.

Este URI requiere una actualización en Xbox.com. Actualmente, la consola Xbox 360 no se actualizará el estado leído o hasta que un usuario cierre la sesión y vuelva a intentarlo.

El tipo de contenido solo es compatible con esta API es "application/json", que es necesario en los encabezados HTTP de cada llamada.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid | entero de 64 bits sin signo | La consola Xbox usuario ID (XUID) del Reproductor que realiza la solicitud. |
| messageId | string[50] | Identificador del mensaje que se va a recuperar o eliminar. |

<a id="ID4ERB"></a>


## <a name="authorization"></a>Autorización

Debe tener su propio usuario de notificación recuperar un mensaje de usuario.

<a id="ID4E3B"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4EJC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Solo puede recuperar sus propios mensajes de usuario.

<a id="ID4EUC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Descripción|
| --- | --- | --- | --- | --- |
| 200| Proceso completado correctamente.|
| 400| No se puede convertir el XUID correctamente.|
| 403| No se puede convertir el XUID o no se encuentra una notificación XUID válida.|
| 404| Falta un XUID válido, o el identificador del mensaje no se encuentra o no se analiza correctamente.|
| 500| Error general de servidor o el tipo de mensaje no es válido para GET.|

<a id="ID4EUE"></a>


## <a name="javascript-object-notation-json-response"></a>Respuesta de objeto de JavaScript Notation (JSON)

Si se llama correctamente, el servicio devuelve los datos de resultados en formato JSON. El objeto raíz es un objeto UserMessageHeader.

#### <a name="usermessageheader"></a>UserMessageHeader

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| header| Encabezado|  | Objeto JSON|
| messageText| string| 256| UTF-8|

#### <a name="header"></a>Encabezado

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| Enviado| DateTime|  | Fecha y hora en que se envió el mensaje. (Proporcionado por el servicio).|
| expiración| DateTime|  | Fecha y hora en que expira el mensaje. (Todos los mensajes tienen una duración máxima, determinarse en el futuro).|
| messageType| string| 13| Tipos de mensajes: Usuario, sistema, FriendRequest.|
| senderXuid| ulong|  | XUID del remitente.|
| remitente| string| 15| Nombre de jugador del remitente.|
| hasAudio| bool|  | Si el mensaje tiene datos adjuntos de audio (voz).|
| hasPhoto| bool|  | Si el mensaje tiene un adjunto de foto.|
| hasText| bool|  | Si el mensaje contiene texto.|

#### <a name="sample-response"></a>Respuesta de ejemplo

```cpp
{
          "header":
          {
            "expiration":"2011-10-11T23:59:59.9999999",
            "messageType":"User",
            "senderXuid":"123456789",
            "sender":"Striker",
            "sent":"2011-05-08T17:30:00Z",
            "hasAudio":false,
            "hasPhoto":false,
            "hasText":true
          },
        "messageText":"random user text up to 256 characters"
        }

```

#### <a name="error-response"></a>Respuesta de error

En el caso de error, el servicio puede devolver un objeto de errorResponse, que puede contener valores desde el entorno del servicio.

| Propiedad| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| Una indicación de dónde se originó el error.|
| errorCode| entero| Código numérico asociado con el error (puede ser null).|
| errorMessage| string| Detalles del error si ha configurado para mostrar los detalles.|

<a id="ID4E3DAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E5DAC"></a>


##### <a name="parent"></a>Primario  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EMEAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referencia [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md)
