---
title: PUT (/{uri})
assetID: 24a24c93-f43b-017e-91e0-29e190fec8a8
permalink: en-us/docs/xboxlive/rest/uri-uriput.html
description: " PUT (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 862b956e29222bb9d28f98510f13d42fd1a51b6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57633120"
---
# <a name="put-uri"></a>PUT (/{uri})
Cargar los datos de clip de juego.
Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.

  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EQB)
  * [Parámetros de cadena de consulta](#ID4ERC)
  * [Encabezados de solicitud necesarios](#ID4EBE)
  * [Encabezados de solicitud opcionales](#ID4ENG)
  * [Cuerpo de la solicitud](#ID4EWH)
  * [Códigos de estado HTTP](#ID4ECAAC)
  * [Encabezados de respuesta necesaria](#ID4EYEAC)
  * [Encabezados de respuesta opcional](#ID4ELHAC)
  * [Cuerpo de respuesta](#ID4ELIAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Observaciones

Después de la **InitialUploadResponse** se devuelve, la carga se realiza a través de la **uploadUri** devuelve en ese objeto. El cliente debe dividir el archivo en **expectedBlocks** bloques secuenciales, no puede superar 2 MB. Se pueden cargar en paralelo.

Si va a cargar el archivo en bloques, el servidor devolverá un código de estado HTTP aceptado (202) para cada solicitud, hasta que haya recibido esperados todos los bloques, en cuyo caso se confirma todos los bloques como un archivo, devolver Created (201). En estos casos, la respuesta no contiene un objeto y el servidor puede programar el procesamiento adicional. En caso de error, un **ServiceErrorResponse** se puede devolver el objeto junto con un código de estado HTTP adecuado.

En un código de error recuperable, el cliente debería reintentar mediante un mecanismo de reintento de retroceso estándar.

> [!NOTE] 
> Incluso si una carga finalice correctamente, su posterior procesamiento se producirá que puede complementar rechazar el clip por motivos no relacionados con la carga o los metadatos de proceso.


<a id="ID4EQB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| <b>uri</b>| string| El <b>uploadUri</b> campo dentro de la <b>InitialUploadResponse</b> objeto.|

<a id="ID4ERC"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- |
| <b>blockNum</b>| entero de 32 bits sin signo| Es necesario si <b>expectedBlocks</b> está establecido. Número de bloque indizados con cero determinar el orden de bloque de archivo. Por ejemplo, si <b>expectedBlocks</b> es 7, a continuación, <b>blockNum</b> puede estar comprendido entre 0 y 6. |
| <b>uploadId</b>| string| Obligatorio. Identificador opaco en <b>GameClipsServiceUploadResponse</b> objeto.|

<a id="ID4EBE"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>|
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.|
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.|
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.|
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.|

<a id="ID4ENG"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.|
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".|

<a id="ID4EWH"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4ECAAC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 301| Movido permanentemente| El servicio se ha movido a otro URI.|
| 307| Redirección temporal| El servicio se ha movido a otro URI.|
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|
| 401| Sin autorización| La solicitud requiere autenticación del usuario.|
| 403| prohibido| No se permite la solicitud para el usuario o servicio.|
| 404| No encontrado| No se encontró el recurso especificado.|
| 406| No es aceptable| No se admite la versión del recurso.|
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.|
| 410| Pasado| El recurso solicitado ya no está disponible.|

<a id="ID4EYEAC"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.|
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.|
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.|
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.|
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible.|
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores.|

<a id="ID4ELHAC"></a>


## <a name="optional-response-headers"></a>Encabezados de respuesta opcional

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Se usa para la optimización de memoria caché. Por ejemplo: "686897696a7c876b7e".|

<a id="ID4ELIAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ningún objeto que se envía en el cuerpo de la respuesta.

<a id="ID4EWIAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EYIAC"></a>


##### <a name="parent"></a>Primario

[/{uri}](uri-uri.md)
