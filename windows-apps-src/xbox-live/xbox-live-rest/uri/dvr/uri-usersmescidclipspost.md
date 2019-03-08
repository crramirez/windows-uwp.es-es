---
title: POST (/users/me/scids/{scid}/clips)
assetID: 44535926-9fb8-5498-b1c8-514c0763e6c9
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipspost.html
description: " POST (/users/me/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a8973390ccbf5dd9980410f60f03a7edd78c134
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608790"
---
# <a name="post-usersmescidsscidclips"></a>POST (/users/me/scids/{scid}/clips)
Realice una solicitud de carga inicial. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EFB)
  * [Autorización](#ID4EQB)
  * [Encabezados de solicitud necesarios](#ID4EKC)
  * [Encabezados de solicitud opcionales](#ID4ENE)
  * [Cuerpo de la solicitud](#ID4ENF)
  * [Solicitud de ejemplo](#ID4E1F)
  * [Códigos de estado HTTP](#ID4EDG)
  * [Cuerpo de respuesta](#ID4EVAAC)
  * [Respuesta de ejemplo](#ID4EFBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
Se trata de la primera parte del proceso de carga de clip de juego. Tras la captura de un vídeo, se recomienda llamar al servicio GameClips inmediatamente para obtener el identificador y el URI para la carga de los bits, incluso si la carga no está programada para comenzar de inmediato. Esta llamada llevará a cabo comprobaciones de cuota de usuario y otras comprobaciones mediante aislamiento de contenido, privacidad, y así sucesivamente para ver si un vídeo incluso se debe programar para la carga por el cliente. Una respuesta positiva de esta llamada indica que el servicio está dispuesto a aceptar el clip de vídeo para la carga. Todos los clips cargados deben estar asociados con un título específico (a través de un ¿SCID) que se admitirán en el sistema.
 
Esta llamada no es idempotente; hará que las llamadas subsiguientes diferentes identificadores y los identificadores URI que se emitan. Reintentos en caso de error deben seguir el estándar comportamiento de retroceso del lado cliente.
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| string| Id. de configuración de servicio del recurso al que se tiene acceso. Debe coincidir con el ¿SCID del usuario autenticado.| 
  
<a id="ID4EQB"></a>

 
## <a name="authorization"></a>Autorización
 
Las siguientes notificaciones son necesarias para este método:
 
   * Xuid
   * Tipo de dispositivo: debe ser el dispositivo para cargar
   * DeviceId
   * TitleId
   * TitleSandboxId
   
<a id="ID4EKC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
  
<a id="ID4ENE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.| 
  
<a id="ID4ENF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
El cuerpo de la solicitud debe ser un [InitialUploadRequest](../../json/json-initialuploadrequest.md) objeto en formato JSON.
  
<a id="ID4E1F"></a>

 
## <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
   "clipNameStringId": "1193045",
   "userCaption": "OMG Look at this!",
   "sessionRef": "4587552a-a5ad-4c4c-a787-5bc5af70e4c9",
   "dateRecorded": "2012-12-23T11:08:08Z",
   "durationInSeconds": 27,
   "expectedBlocks": 7,
   "fileSize": 1234567,
   "type": "MagicMoment, Achievement",
   "source": "Console",
   "visibility": "Default",
   "titleData": "{ 'Boss': 'The Invincible' }",
   "systemProperties": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }",
   "thumbnailSource": "Offset",
   "thumbnailOffsetMillseconds": 20000,
   "savedByUser": false
 }
      
```

  
<a id="ID4EDG"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 400| solicitud incorrecta| Se produjo un error en el cuerpo de solicitud o el usuario es a través de su cuota.| 
| 401| Sin autorización| Hay un problema con el formato de token de autenticación en la solicitud.| 
| 403| prohibido| Algunos necesarios faltan las notificaciones, o no es de tipo de dispositivo.| 
| 503| No es aceptable| El servicio o algunas dependencias de nivel inferiores están inactivos. Vuelva a intentar con el comportamiento estándar de retroceso.| 
  
<a id="ID4EVAAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
La respuesta puede ser un [InitialUploadResponse](../../json/json-initialuploadresponse.md) objeto o un [ServiceErrorResponse](../../json/json-serviceerrorresponse.md) objeto en formato JSON.
  
<a id="ID4EFBAC"></a>

 
## <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
   "clipName": "ClipName",
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752",  
   "titleName": "TitleName",
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
         
```

  
<a id="ID4EOBAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQBAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

   