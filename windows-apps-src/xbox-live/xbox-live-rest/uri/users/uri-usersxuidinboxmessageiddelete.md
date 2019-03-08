---
title: DELETE (/users/xuid({xuid})/inbox/{messageId})
assetID: c54eede3-3e3b-2cbe-1be9-8bf3a48171bc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinboxmessageiddelete.html
description: " DELETE (/users/xuid({xuid})/inbox/{messageId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 80ec2a462648177cc6bfc846b9c84278821b0e5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594110"
---
# <a name="delete-usersxuidxuidinboxmessageid"></a>DELETE (/users/xuid({xuid})/inbox/{messageId})
Elimina un mensaje de usuario en la Bandeja de entrada. Es el dominio para estos URI `msg.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4ECB)
  * [Autorización](#ID4EPB)
  * [Cuerpo de la solicitud](#ID4E1B)
  * [Códigos de estado HTTP](#ID4EHC)
  * [Respuesta de objeto de JavaScript Notation (JSON)](#ID4EAE)
  * [Efecto de la configuración de privacidad en recurso](#ID4EYF)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones 
 
La operación de eliminación es idempotente.
 
El tipo de contenido solo es compatible con esta API es "application/json", que es necesario en los encabezados HTTP de cada llamada. 
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid | entero de 64 bits sin signo | La consola Xbox usuario ID (XUID) del Reproductor que realiza la solicitud. | 
| messageId | string[50] | Identificador del mensaje que se va a recuperar o eliminar. | 
  
<a id="ID4EPB"></a>

 
## <a name="authorization"></a>Autorización 
 
Debe tener su propio usuario eliminar un usuario de mensaje de notificación.
  
<a id="ID4E1B"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EHC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Descripción| 
| --- | --- | --- | --- | --- | 
| 204| Proceso completado correctamente.| 
| 403| No se puede convertir el XUID o no se encuentra una notificación XUID válida.| 
| 404| Id. de mensaje en el URI no se puede analizar o falta una XUID en el URI.| 
| 500| Error general al servidor.| 
  
<a id="ID4EAE"></a>

 
## <a name="javascript-object-notation-json-response"></a>Respuesta de objeto de JavaScript Notation (JSON) 
 
En el caso de error, el servicio puede devolver un objeto de errorResponse, que puede contener valores desde el entorno del servicio.
 
| Propiedad| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| errorSource| string| Una indicación de dónde se originó el error.| 
| errorCode| entero| Código numérico asociado con el error (puede ser null).| 
| errorMessage| string| Detalles del error si ha configurado para mostrar los detalles.| 
  
<a id="ID4EYF"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso 
 
Solo puede eliminar sus propios mensajes de usuario. 
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Primario  

[/users/xuid({xuid})/inbox](uri-usersxuidinbox.md)

  
<a id="ID4ETG"></a>

 
##### <a name="reference--standard-http-status-codesadditionalhttpstatuscodesmd"></a>Referencia [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md)

   