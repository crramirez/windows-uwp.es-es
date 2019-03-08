---
title: POST (/users/batchfeedback)
assetID: f94dcf19-a4e3-5bd0-5276-23e43bdcae52
permalink: en-us/docs/xboxlive/rest/uri-reputationusersbatchfeedbackpost.html
description: " POST (/users/batchfeedback)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0906d32a0e15b2eaaf9c33e7f658e9e9f0cd5124
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622730"
---
# <a name="post-usersbatchfeedback"></a>POST (/users/batchfeedback)
Servicio de su título usa para enviar comentarios con formato por lotes fuera de la interfaz de su título. Es el dominio para estos URI `reputation.xboxlive.com`.
 
  * [Cuerpo de la solicitud](#ID4EX)
  * [Encabezados obligatorios](#ID4E3E)
  * [Códigos de estado HTTP](#ID4EWG)
  * [Cuerpo de respuesta](#ID4EDAAC)
 
<a id="ID4EX"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
Los autores de llamadas deben incluir su certificado de notificaciones en la sección ClientCertificates de su objeto de solicitud web.
 
<a id="ID4EBB"></a>

 
### <a name="required-members"></a>Miembros requeridos 
 
La solicitud debe contener una matriz de **BatchFeedback** objetos. 
  
<a id="ID4EPB"></a>

 
### <a name="prohibited-members"></a>Miembros prohibidos 
 
Todos los demás miembros están prohibidas en una solicitud.
  
<a id="ID4E3B"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo 
 

```cpp
{
    "items" :
    [
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayKillsTeammates",
            "textReason": "Killed 19 team members in a single session",
            "evidenceId": null
        },
        {
            "targetXuid": "33445566778899",
            "titleId": "6487",
            "sessionRef":
            {
                "scid": "372D829B-FA8E-471F-B696-07B61F09EC20",
                "templateName": "CaptureFlag5",
                "name": "Halo556932",
            },
            "feedbackType": "FairPlayQuitter",
            "textReason": "Quit 6 times from 9 sessions",
            "evidenceId": null
        }
    ]
}

      
```

 
| <b>Field</b>| <b>Tipo JSON</b>| <b>Descripción</b>| 
| --- | --- | --- | 
| items| array| Una colección de documentos JSON de comentarios.| 
| targetXuid| string| XUID del usuario de destino| 
| titleId| string| El título que estos comentarios se envían desde, o NULL.| 
| sessionRef| object| Un objeto que describe la sesión MPSD estos comentarios se relaciona con, o NULL.| 
| feedbackType| string| Versión de cadena de un valor de la enumeración FeedbackType.| 
| textReason| string| Texto proporcionado por el socio que puede agregar el remitente para dar más detalles sobre los comentarios que se envió.| 
| evidenceId| string| El identificador de un recurso que puede usarse como prueba de los comentarios que se está enviando. Por ejemplo, el identificador de un archivo de vídeo.| 
   
<a id="ID4E3E"></a>

 
## <a name="required-headers"></a>Encabezados obligatorios
 
Los encabezados siguientes son necesarios al realizar una solicitud de servicios de Xbox Live. 

> [!NOTE] 
> , Se debe enviar un certificado de notificaciones asociado con la solicitud para enviar comentarios por lotes. 


 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 101| Versión del contrato de API.| 
| Content-Type| application/json| Tipo de datos que se está enviadas.| 
| Autorización| "XBL3.0 x =&lt;userhash >;&lt; token > "| Credenciales de autenticación para la autenticación HTTP.| 
| X-RequestedServiceVersion| 101| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente.| 
  
<a id="ID4EWG"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EDAAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta 
 
Si la llamada se realiza correctamente, se devuelve ningún objeto de esta respuesta. En caso contrario, el servicio devuelve un [depuracuión puede contener](../../json/json-serviceerror.md) objeto.
  
<a id="ID4EXAAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EZAAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

  
<a id="ID4EFBAC"></a>

 
##### <a name="reference"></a>Referencia 

[Comentarios (JSON)](../../json/json-feedback.md)

 [Depuracuión puede contener (JSON)](../../json/json-serviceerror.md)

   