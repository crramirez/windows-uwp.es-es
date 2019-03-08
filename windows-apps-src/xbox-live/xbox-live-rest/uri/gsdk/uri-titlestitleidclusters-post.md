---
title: POST (/titles/{titleId}/clusters)
assetID: 0977b0b0-872d-f7ad-9ba0-30d56cff4912
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidclusters-post.html
description: " POST (/titles/{titleId}/clusters)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 91d7c49628914f887c5d2243942e10e47d47b095
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608910"
---
# <a name="post-titlestitleidclusters"></a>POST (/titles/{titleId}/clusters)
URI que permite que un cliente crear una instancia de servidor de Xbox Live Compute. Es el dominio para estos URI `gameserverms.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Encabezados de solicitud necesarios](#ID4EGB)
  * [Autorización](#ID4ELD)
  * [Cuerpo de la solicitud](#ID4EWD)
  * [Encabezados de respuesta necesaria](#ID4EZE)
  * [Cuerpo de respuesta](#ID4E5G)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Descripción| 
| --- | --- | 
| titleId| Id. del título que debe operar la solicitud.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nombre de host

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
Al realizar una solicitud, los encabezados que se muestra en la tabla siguiente son necesarios.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | 
| User-Agent|  | Información sobre el agente de usuario que realiza la solicitud.| 
| Content-Type| application/json| Tipo de datos que se está enviadas.| 
| Host| gameserverms.xboxlive.com|  | 
| Content-Length|  | Longitud del objeto de solicitud.| 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación.| 
  
<a id="ID4ELD"></a>

 
## <a name="authorization"></a>Autorización
 
La solicitud debe incluir un encabezado de autorización válido de Xbox Live. Si el llamador no tiene permiso para tener acceso a este recurso, el servicio devuelve 403 Prohibido en respuesta. Si el encabezado está ausente o no válido, el servicio devuelve 401 no autorizado en la respuesta.
  
<a id="ID4EWD"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
La solicitud debe contener un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Identificador de sesión desde el MPSD.| 
| abortIfQueued| Parámetro opcional, que cuando establece en true indica GSMS no poner en cola esta sesión para un recurso si no puede cumplirse inmediatamente. Si la solicitud se ha anulado porque este valor es true, el objeto de respuesta contendrá <code>"fulfillmentState" : "Aborted"</code>. | 
 
<a id="ID4ERE"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
  "sessionId" : "/serviceconfigs/00000000-0000-0000-0000-000000000000/sessiontemplates/quick/session/scott1",
  "abortIfQueued" : "true"
}

      
```

   
<a id="ID4EZE"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
Una respuesta siempre incluirá los encabezados que se muestra en la tabla siguiente.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Cache-Control|  | Directivas que deben obedecer todos los mecanismos de almacenamiento en caché a lo largo de la cadena de solicitud/respuesta.| 
| Content-Type| application/json| Tipo de datos en la respuesta.| 
| Content-Length|  | Longitud del cuerpo de respuesta.| 
| X-Content-Type-Options|  |  | 
| X-XblCorrelationId|  | El tipo mime del cuerpo de respuesta.| 
| Fecha|  |  | 
  
<a id="ID4E5G"></a>

 
## <a name="response-body"></a>Cuerpo de respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| pollIntervalMilliseconds| Intervalo en milisegundos para sondear la finalización recomendado. Tenga en cuenta que esto no es una estimación de cuando el clúster estará listo, pero en su lugar una recomendación para con qué frecuencia el llamador debe sondear en busca de una actualización de estado según el grupo actual de las suscripciones y las tasas de solicitud y el cumplimiento.| 
| fulfillmentState| Indica si la sesión proporcionada se ha asignado un recurso inmediatamente, "Cumplen", agrega a la cola para la disponibilidad de un recurso futuras, "en cola", o puede ser aborted, "Anuladas", debido a la incapacidad para satisfacer la solicitud inmediatamente cuando la solicitud abortIfQueued especificado como "true". | 
 
<a id="ID4EWH"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
  "pollIntervalMilliseconds" : "1000",
  "fulfillmentState" : "Fulfilled" | "Queued" | "Aborted"
}
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Observaciones
 
Un título sólo debe reintentar la llamada al servicio cuando se reciben los siguientes códigos de respuesta:
 
   * 408: tiempo de espera del servidor
   * 429: demasiadas solicitudes
   * 500: Error de servidor
   * 502: puerta de enlace incorrecta
   * 503: servicio no disponible
   * 504: tiempo de espera de puerta de enlace
   
<a id="ID4EFBAC"></a>

 
## <a name="see-also"></a>Consulte también
 [/titles/{titleId}/clusters](uri-titlestitleidclusters.md)

  