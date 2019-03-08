---
title: GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
assetID: dbd60c93-9d8e-609b-0ae3-b3f7ee26ba2d
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipidget.html
description: " GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5d2858b11bf8fb902ea07a016c8f41db375013f9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640960"
---
# <a name="get-usersowneridscidsscidclipsgameclipid"></a>GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})
Obtener un único juego clip del sistema si se conocen todos los identificadores para buscarlo. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EVB)
  * [Autorización](#ID4EAC)
  * [Encabezados de solicitud necesarios](#ID4EUH)
  * [Encabezados de solicitud opcionales](#ID4EBCAC)
  * [Cuerpo de la solicitud](#ID4ETDAC)
  * [Códigos de estado HTTP](#ID4E5DAC)
  * [Encabezados de respuesta necesaria](#ID4EQIAC)
  * [Encabezados de respuesta opcional](#ID4EJLAC)
  * [Cuerpo de respuesta](#ID4EJMAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
Todos los datos para consultar el clip está disponible en el **clip de juego** objetos que se devuelve desde cualquier consulta de metadatos. **XUID**, **ServiceConfigId**, **GameClipId** y **SandboxId** en las notificaciones de la solicitud se requieren para obtener un único juego clip a través de esta API. Si el clip de juego se ha marcado para la aplicación, o determinar que el usuario no tiene permiso para obtener el clip juegos específico de comprobaciones de aislamiento del contenido o la privacidad, la API devolverá un código de estado HTTP de 404 (no encontrado).
 
**SandboxId** se recuperan de la notificación en el XToken ahora y se aplican. Si el **SandboxId** no está presente, a continuación, servicios de detección de entretenimiento (EDS) producirá un error 400 Solicitud incorrecta.
 
También se debe utilizar esta API para actualizar URI expiradas. Una vez completada la consulta, cualquier URI caducadas para el clip de juego se actualizará según corresponda. 

> [!NOTE] 
> Una actualización URI puede tardar hasta 30-40 segundos después de realiza esta solicitud. Si ha expirado el URI, intenta usarlo de inmediato para las operaciones de transmisión por secuencias obtendrá un código de estado HTTP 500 desde los servidores de IIS Smooth Streaming. Estamos trabajando en formas de reducir esto, y esta nota se actualizará según corresponda como ese trabajo progresa. 


  
<a id="ID4EVB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| ownerId| string| Identidad de usuario del usuario cuyo recurso se está obteniendo acceso. Formatos admitidos: "me" o "xuid(123456789)". Longitud máxima: 16.| 
| scid| string| Id. de configuración de servicio del recurso al que se tiene acceso. Debe coincidir con el ¿SCID del usuario autenticado.| 
| gameClipId| string| Clip de juego el identificador del recurso que está accediendo.| 
  
<a id="ID4EAC"></a>

 
## <a name="authorization"></a>Autorización
 
Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo| Observaciones| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Xuid| entero con signo de 64 bits| sí| 1234567890|  | 
| TitleId| entero con signo de 64 bits| sí| 1234567890| Utilizado para <b>contenido aislamiento</b> comprobar.| 
| SandboxId| binario hexadecimal| sí|  | Indica al sistema que el área adecuada para las búsquedas y se utiliza para <b>contenido aislamiento</b> comprobar.| 
  
Efecto de la configuración de privacidad en recurso | Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Como se describe.| 
| Friend| Todo el mundo| Está prohibido.| 
| Friend| solo los amigos| Está prohibido.| 
| Friend| bloqueado| Está prohibido.| 
| usuario de no confianza| Todo el mundo| Está prohibido.| 
| usuario de no confianza| solo los amigos| Está prohibido.| 
| usuario de no confianza| bloqueado| Está prohibido.| 
| sitio de terceros| Todo el mundo| Está prohibido.| 
| sitio de terceros| solo los amigos| Está prohibido.| 
| sitio de terceros| bloqueado| Está prohibido.| 
 
<a id="ID4EUH"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
  
<a id="ID4EBCAC"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.| 
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".| 
| Intervalo| string|  | 
  
<a id="ID4ETDAC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4E5DAC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 301| Movido permanentemente|  | 
| 307| Redirección temporal|  | 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.| 
| 410| Pasado| El recurso solicitado ya no está disponible.| 
  
<a id="ID4EQIAC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible. Ejemplo: <b>application/json</b>.| 
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores. Ejemplo: <b>application/json</b>.| 
  
<a id="ID4EJLAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4EJMAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EPMAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
 "gameClip": {
   "xuid": "2716903703773872",
   "clipName": "1234567890",
   "titleName": "",
   "gameClipId": "cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
   "state": "Published",
   "dateRecorded": "2013-05-08T21:32:17.4201279Z",
   "lastModified": "2013-05-08T21:34:48.8117829Z",
   "userCaption": "",
   "type": "DeveloperInitiated",
   "source": "Console",
   "visibility": "Public",
   "durationInSeconds": 30,
   "scid": "00000000-0000-0012-0023-000000000070",
   "titleId": 0,
   "rating": 0,
   "ratingCount": 0,
   "views": 0,
   "titleData": "",
   "systemProperties": "",
   "savedByUser": false,
   "thumbnails": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/large",
       "fileSize": 0,
       "thumbnailType": "Large"
     },
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000\/thumbnails\/small",
       "fileSize": 0,
       "thumbnailType": "Small"
     }
   ],
   "gameClipUris": [
     {
       "uri": "http:\/\/localhost\/users\/xuid(2716903703773872)\/scids\/00000000-0000-0012-0023-000000000070\/clips\/cd42452a-8ec0-4289-9e9e-e4cd89d7d674-000",
       "fileSize": 9332015,
       "uriType": "Download",
       "expiration": "9999-12-31T23:59:59.9999999"
     }
   ]
 }
}
         
```

   
<a id="ID4EZMAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E2MAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

  
<a id="ID4EFNAC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   