---
title: GET (/users/xuid({xuid})/groups/{moniker} )
assetID: 63aa7e5d-0599-5850-756d-3079c0772238
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerget.html
description: " GET (/users/xuid({xuid})/groups/{moniker} )"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: df967677ce779fc128a8956f137a027a108d313d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623850"
---
# <a name="get-usersxuidxuidgroupsmoniker-"></a>GET (/users/xuid({xuid})/groups/{moniker} )
Obtiene el PresenceRecord para un grupo. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E5)
  * [Parámetros de cadena de consulta](#ID4EJB)
  * [Autorización](#ID4EKC)
  * [Efecto de la configuración de privacidad en recurso](#ID4EQD)
  * [Encabezados de solicitud necesarios](#ID4EEH)
  * [Encabezados de solicitud opcionales](#ID4EMBAC)
  * [Cuerpo de la solicitud](#ID4EMCAC)
  * [Códigos de estado HTTP](#ID4EXCAC)
  * [Encabezados de respuesta necesaria](#ID4E3GAC)
  * [Encabezados de respuesta opcional](#ID4EMJAC)
  * [Cuerpo de respuesta](#ID4E5KAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Recupera los usuarios en el grupo especificado por el moniker relacionados con el usuario en el URI y devuelve el PresenceRecord para esos usuarios. Simplemente no se devolverán datos que viene determinado por la privacidad o el aislamiento del contenido.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID) del usuario relacionada con el XUIDs en el grupo.| 
| moniker| string| Cadena que define el grupo de usuarios. El moniker solo aceptado en la actualidad es "People", con una letra mayúscula "P".| 
  
<a id="ID4EJB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| level| string| Devuelve el nivel de detalle especificado por esta cadena de consulta. Las opciones válidas son "user", "dispositivo", "title" y "all". El nivel "user" es el objeto PresenceRecord sin el objeto anidado DeviceRecord. El nivel "dispositivo" es los objetos PresenceRecord y DeviceRecord sin TitleRecord objeto anidado. El nivel "title" es el objetos PresenceRecord, DeviceRecord y TitleRecord sin ActivityRecord objeto anidado. El nivel "all" es el registro completo, incluidos todos los objetos anidados. Si no se proporciona este parámetro, el servicio tiene como valor predeterminado del nivel title (es decir, devuelve la presencia de este usuario hasta los detalles de título).| 
  
<a id="ID4EKC"></a>

 
## <a name="authorization"></a>Autorización
 
Notificaciones de autorización usa | Notificación| Tipo| ¿Obligatorio?| Valor de ejemplo| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>Xuid</b>| entero con signo de 64 bits| sí| 1234567890| 
  
<a id="ID4EQD"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso
 
El servicio siempre devolverá 200 OK si la solicitud está bien formada. Sin embargo, filtrará la información de la respuesta al no pasar comprobaciones de privacidad.
 
Efecto de la configuración de privacidad en recurso | Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Me| -| Como se describe.| 
| Friend| Todo el mundo| Como se describe.| 
| Friend| solo los amigos| Como se describe.| 
| Friend| bloqueado| Como se describe: el servicio filtrará los datos.| 
| usuario de no confianza| Todo el mundo| Como se describe.| 
| usuario de no confianza| solo los amigos| Como se describe: el servicio filtrará los datos.| 
| usuario de no confianza| bloqueado| Como se describe: el servicio filtrará los datos.| 
| sitio de terceros| Todo el mundo| Como se describe: el servicio filtrará los datos.| 
| sitio de terceros| solo los amigos| Como se describe: el servicio filtrará los datos.| 
| sitio de terceros| bloqueado| Como se describe: el servicio filtrará los datos.| 
  
<a id="ID4EEH"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.| 
| Aceptar| string| Tipos de contenido que son aceptables. La sola admite presencia es <b>application/json</b>, pero todavía se debe especificar en el encabezado.| 
| Aceptar idioma| string| Configuración regional aceptable para las cadenas en la respuesta. Valor de ejemplo: en-US.| 
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: userpresence.xboxlive.com.| 
  
<a id="ID4EMBAC"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.| 
  
<a id="ID4EMCAC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EXCAC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 405| Método no permitido| Método HTTP se utilizó en un tipo de contenido no admitido.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 500| Tiempo de espera de solicitud| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 503| Tiempo de espera de solicitud| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
  
<a id="ID4E3GAC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 1 vnext.| 
| Content-Type| string| El tipo mime del cuerpo de la solicitud. Valor de ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché. Valores de ejemplo: "no-cache".| 
| X-XblCorrelationId| string| Valor generado por el servicio para correlacionar el servidor devuelve y lo que es recibido por el cliente. Valor de ejemplo: "4106d0bc-1cb3-47bd-83fd-57d041c6febe".| 
| X-Content-Type-Option| string| Devuelve el valor compatible con SDL. Valor de ejemplo: "nosniff".| 
| Fecha| string| La fecha y hora en que se envió el mensaje. Valor de ejemplo: "Tue, 17 Nov 2012 10:33:31 GMT".| 
  
<a id="ID4EMJAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Retry-After| string| Se devuelve sobre los errores HTTP 503. Permite el cliente sabe cuánto tiempo debe para esperar antes de reintentar la llamada. Valores de ejemplo: "120".| 
| Content-Length| string| Longitud del cuerpo de respuesta. Valor de ejemplo: "527".| 
| Codificación de contenido| string| Tipo de codificación de la respuesta. Valor de ejemplo: "gzip".| 
  
<a id="ID4E5KAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Esta API devuelve una matriz de objetos de PresenceRecord, uno para cada uno de los XUIDs de la solicitud.
 
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
[
     {
         xuid:"0123456789",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341234",
                         name:"Contoso 5",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement:"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Playing on Nirvana"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456780",
         state:"online",
         devices:
         [
             {
                 type:"D",
                 titles:
                 [
                     {
                         id:"12341235",
                         name:"Contoso Waypoint",
                         lastModified:"2012-09-17T07:15:23.4930000",
                         placement;"full",
                         state:"active",
                         activity:
                         {
                             richPresence:"Viewing stats"
                         },
                     }
                 ]
             }
         ]
     },
     {
         xuid:"0123456781",
         state:"online",
         devices:
         [
             {
                 type:"Win8",
                 titles:
                 [
                     {
                         id:"23452345",
                         name:"Contoso Gamehelp",
                         state:"active",
                         timestamp:"2012-09-17T07:15:23.4930000",
                         activity:
                         {
                             richPresence:"Viewing help"
                         }
                     }
                 ]
             }
         ]
     }
 ]
         
```

   
<a id="ID4EQLAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ESLAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

   