---
title: GET (/users/xuid({xuid})/inbox)
assetID: c603910d-b430-f157-2634-ceddea89f2bd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxget.html
description: " GET (/users/xuid({xuid})/inbox)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 05f75510f15f6e6c5f1b1673673428c00f7a6c16
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632250"
---
# <a name="get-usersxuidxuidinbox"></a>GET (/users/xuid({xuid})/inbox)
Recupera un número especificado de los resúmenes de mensaje de usuario desde el servicio.
Es el dominio para estos URI `msg.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EEB)
  * [Parámetros de cadena de consulta](#ID4EIC)
  * [Autorización](#ID4EGE)
  * [Efecto de la configuración de privacidad en recurso](#ID4ETE)
  * [Códigos de estado HTTP](#ID4E5E)
  * [Respuesta de objeto de JavaScript Notation (JSON)](#ID4EMH)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Un resumen de mensajes de usuario contiene a solo el asunto del mensaje. Los mensajes generados por el usuario, actualmente es los 20 primeros caracteres del texto del mensaje. Los mensajes del sistema pueden proporcionar a un sujeto alternativo, como "System LIVE".

Los mensajes se devuelven en el orden inverso al que se enviaron; es decir, se devuelven mensajes más recientes en primer lugar.

El tipo de contenido solo es compatible con esta API es "application/json", que es necesario en los encabezados HTTP de cada llamada.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| entero de 64 bits sin signo| La consola Xbox usuario ID (XUID) del Reproductor que realiza la solicitud.|

<a id="ID4EIC"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- | --- | --- | --- |
| maxItems| entero| 100| Devuelve el número máximo de mensajes.|
| continuationToken| string|  | Cadena devuelta en una llamada anterior de la enumeración; se usa para continuar la enumeración.|
| skipItems| entero| 100| Número de mensajes que se va a omitir; se omite si continuationToken está presente.|

<a id="ID4EGE"></a>


## <a name="authorization"></a>Autorización

Debe tener su propio usuario recuperar un resumen de usuario de mensaje de notificación.

<a id="ID4ETE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Solo puede enumerar sus propios mensajes de usuario.

<a id="ID4E5E"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| La solicitud es correcta.|
| 400| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|
| 403| No se permite la solicitud para el usuario o servicio.|
| 404| Falta un XUID válido en el URI.|
| 409| La colección subyacente cambia según el token de continuación que se pasó.|
| 416| El número de elementos para omitir es mayor que el número de elementos disponibles.|
| 500| Error general al servidor.|

<a id="ID4EMH"></a>


## <a name="javascript-object-notation-json-response"></a>Respuesta de objeto de JavaScript Notation (JSON)

Si se llama correctamente, el servicio devuelve los datos de resultados en formato JSON.

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| Resultados| Message[]| 100| Matriz de mensajes de usuario|
| pagingInfo| PagingInfo|  | Información de paginación para el conjunto actual de resultados|

#### <a name="message"></a>Mensaje

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| header| Encabezado|  | Encabezado del mensaje de usuario|
| messageSummary| string| 20| UTF-8; Normalmente, los 20 primeros caracteres del mensaje|

#### <a name="header"></a>Encabezado

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| id| string| 50| Identificador de mensaje, utilizado para recuperar los detalles del mensaje o eliminación de mensajes.|
| isRead| bool|  | Marca que indica que el usuario ya ha leído los detalles del mensaje.|
| Enviado| DateTime|  | Fecha y hora UTC que se envió el mensaje. (Proporcionado por el servicio).|
| expiración| DateTime|  | Fecha y hora UTC que expira el mensaje. (Todos los mensajes tienen una duración máxima, determinarse en el futuro).|
| messageType| string| 50| Tipos de mensajes: Usuario, sistema, FriendRequest, vídeo, QuickChat, VideoChat, PartyChat, título, GameInvite.|
| senderXuid| ulong|  | XUID del remitente.|
| remitente| string| 15| Nombre de jugador del remitente.|
| hasAudio| bool|  | Si el mensaje tiene datos adjuntos de audio (voz).|
| hasPhoto| bool|  | Si el mensaje tiene un adjunto de foto.|
| hasText| bool|  | Si el mensaje contiene texto.|

#### <a name="paging-info"></a>Información de paginación

| Propiedad| Tipo| Longitud máxima| Observaciones|
| --- | --- | --- | --- |
| continuationToken| string| 100| Opcionalmente, devuelto por servidor. Permite las llamadas posteriores a continuar la enumeración.|
| totalItems| entero|  | Número total de mensajes en la Bandeja de entrada.|

#### <a name="sample-response"></a>Respuesta de ejemplo

```cpp
{
          "results":
          [
            {
              "header":
              {
                "expiration":"2011-10-11T23:59:59.9999999",
                "id":"opaqueBlobOfText",
                "messageType":"User",
                "isRead":false,
                "senderXuid":"123456789",
                "sender":"Striker",
                "sent":"2011-05-08T17:30:00Z",
                "hasAudio":false,
                "hasPhoto":false,
                "hasText":true
              },
            "messageSummary":"first 20 chars"
          },
          ...
        ],
        "pagingInfo":
          {
          "continuationToken":"opaqueBlobOfText"
          "totalItems":5,
          }
        }

```

#### <a name="error-response"></a>Respuesta de error

En el caso de error, el servicio puede devolver un objeto de errorResponse, que puede contener valores desde el entorno del servicio.

| Propiedad| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| errorSource| string| Una indicación de dónde se originó el error.|
| errorCode| entero| Código numérico asociado con el error (puede ser null).|
| errorMessage| string| Detalles del error si ha configurado para mostrar los detalles.|

<a id="ID4EIKAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EKKAC"></a>


##### <a name="parent"></a>Primario  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)


<a id="ID4EWKAC"></a>


##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referencia [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md)
