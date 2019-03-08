---
title: DELETE (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 486fac60-6884-2e3f-9ef8-8de5da0ad8af
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipiddelete.html
description: " DELETE (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 391e4d79a389c358dea83509b52782d086201ffc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622840"
---
# <a name="delete-usersmescidsscidclipsgameclipid"></a>DELETE (/users/me/scids/{scid}/clips/{gameClipId})
Clip de juego de eliminar los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4ECB)
  * [Autorización](#ID4ENB)
  * [Encabezados de solicitud necesarios](#ID4EYB)
  * [Encabezados de solicitud opcionales](#ID4EEE)
  * [Cuerpo de la solicitud](#ID4ENF)
  * [Códigos de estado HTTP](#ID4EYF)
  * [Encabezados de respuesta necesaria](#ID4EIAAC)
  * [Encabezados de respuesta opcional](#ID4E2CAC)
  * [Cuerpo de respuesta](#ID4E2DAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
Proporciona un mecanismo para eliminar el vídeo del usuario desde el servicio GameClips. Tras la eliminación, se quitan todos los metadatos y los recursos de vídeo reales (generados y originales) del sistema. Se trata de una operación permanente. 

> [!NOTE] 
> El identificador de propietario especificado debe coincidir con el autor de llamada en el token de autorización para la solicitud de eliminación se realice correctamente. 


  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| scid| string| Id. de configuración de servicio del recurso al que se tiene acceso. Debe coincidir con el ¿SCID del usuario autenticado.| 
| gameClipId| string| Clip de juego el identificador del recurso que está accediendo.| 
  
<a id="ID4ENB"></a>

 
## <a name="authorization"></a>Autorización
 
Solo la notificación Xuid es necesaria para este método.
  
<a id="ID4EYB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
  
<a id="ID4EEE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.| 
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EYF"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 204| Aceptar| Eliminación correcta del clip.| 
| 401| Sin autorización| Hay un problema con el formato de token de autenticación en la solicitud.| 
| 403| prohibido| Algunos necesarios faltan las notificaciones.| 
| 404| No encontrado| El clip especificado en la dirección URL no estaba presente (o eliminó una segunda vez).| 
| 503| No es aceptable| El servicio o algunas dependencias de nivel inferiores están inactivos. Vuelva a intentar con el comportamiento estándar de retroceso.| 
  
<a id="ID4EIAAC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible.| 
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores.| 
  
<a id="ID4E2CAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Se usa para la optimización de memoria caché. Por ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4E2DAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
El servicio responderá con un código de estado HTTP de 204 (sin contenido) cuando se realiza correctamente. Intentando eliminar el mismo objeto o un objeto inexistente devolverá 404.
 
En el caso de errores, una **ServiceErrorResponse** se devolverá el objeto.
  
<a id="ID4EJEAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ELEAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   