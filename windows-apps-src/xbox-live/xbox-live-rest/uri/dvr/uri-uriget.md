---
title: GET (/{uri})
assetID: a67a3288-88f9-c504-5fa8-8fd06055d079
permalink: en-us/docs/xboxlive/rest/uri-uriget.html
description: " GET (/{uri})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 757d84c9ad5a005e042b42d699ada08504dc57ff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650620"
---
# <a name="get-uri"></a>GET (/{uri})
Descargue el clip de juego. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EDB)
  * [Encabezados de solicitud necesarios](#ID4EEC)
  * [Encabezados de solicitud opcionales](#ID4EQE)
  * [Cuerpo de la solicitud](#ID4EZF)
  * [Encabezados de respuesta necesaria](#ID4EEG)
  * [Códigos de estado HTTP](#ID4EYAAC)
  * [Encabezados de respuesta opcional](#ID4EOFAC)
  * [Cuerpo de respuesta](#ID4EOGAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
El cliente puede descargar cualquier clip o la vista en miniatura que se ha alcanzado el estado publicado y es del tipo descargable, como se especifica en un **GameClipUri** objeto. El URI para solicitar el archivo se incluye en el cuerpo de respuesta al recuperar una lista de clips públicas o de usuario.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| <b>uri</b>| string| El <b>uri</b> campo dentro de la <b>GameClipUri</b> objeto.| 
  
<a id="ID4EEC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
  
<a id="ID4EQE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.| 
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4EZF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EEG"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible.| 
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores.| 
  
<a id="ID4EYAAC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EOFAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Se usa para la optimización de memoria caché. Por ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4EOGAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EUGAC"></a>

  
 
Si se realiza correctamente, el servidor devolverá el clip de vídeo, posiblemente truncado según el encabezado de solicitud de intervalo. Para un clip truncado, la respuesta será contenido parcial (206). Si el servidor devuelve todo el archivo, responderá OK (200). En caso de error, un **GameClipsServiceErrorResponse** se puede devolver el objeto junto con un código de estado HTTP adecuado (p. ej., 416, solicita intervalo no se puede satisfacer).
   
<a id="ID4E4GAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E6GAC"></a>

 
##### <a name="parent"></a>Primario 

[/{uri}](uri-uri.md)

   