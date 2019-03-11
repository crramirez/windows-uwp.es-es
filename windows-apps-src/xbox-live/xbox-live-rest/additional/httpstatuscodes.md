---
title: Códigos de estado HTTP estándar
assetID: 7a19de56-7acd-ad2b-e8e6-53126991093b
permalink: en-us/docs/xboxlive/rest/httpstatuscodes.html
description: " Códigos de estado HTTP estándar"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c77972e88dbf4950f716594ee61220d1734a7fef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635560"
---
# <a name="standard-http-status-codes"></a>Códigos de estado HTTP estándar
 
El protocolo de transporte de hipertexto (HTTP) estándar describe una serie de códigos de estado devueltos por el servidor en respuesta a una solicitud de cliente. Métodos de servicios de Xbox Live devuelven códigos de estado compatible con el protocolo HTTP para describir el estado de la solicitud.
 
Esta es una lista de códigos de estado devueltos por los servicios de Xbox Live y sus significados típicos.
 
<a id="ID4EAB"></a>

 
## <a name="xbox-live-services-status-codes"></a>Códigos de estado de servicios de Xbox Live
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | 
| 200| Aceptar| La solicitud es correcta.| 
| 201| Creación| Se creó la entidad.| 
| 202| Aceptado| La solicitud se aceptó, pero no se ha completado todavía.| 
| 204| Sin contenido| La solicitud se ha completado, pero no tiene contenido para devolver.| 
| 301| Movido permanentemente| El servicio se ha movido a otro URI.| 
| 302| Se encontró| El recurso solicitado reside temporalmente en un URI diferente.| 
| 307| Redirección temporal| Se cambió temporalmente el URI para este recurso.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.| 
| 409| Conflicto| No se completó la solicitud debido a un conflicto con el estado actual del recurso.| 
| 410| Pasado| El recurso solicitado ya no está disponible.| 
| 412| Error de condición previa| El servidor no cumple una de las condiciones previas que el solicitante ha establecido en la solicitud.| 
| 416| No se puede satisfacer el intervalo solicitado| El intervalo solicitado no está disponible.| 
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 501| No implementado| El servidor no admite la funcionalidad necesaria para satisfacer la solicitud.| 
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
 

> [!NOTE] 
> Algunos recursos y los métodos proporcionan información específica sobre el significado de los códigos de estado determinado dentro del contexto de ese recurso o el método. Para obtener más información, consulte la documentación para los recursos o los métodos que está usando. 

  
<a id="ID4E3BAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E5BAC"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EKCAC"></a>

 
##### <a name="reference--universal-resource-identifier-uri-referenceuriatoc-xboxlivews-reference-urismd"></a>Referencia [referencia de identificador (URI) de recursos Universal](../uri/atoc-xboxlivews-reference-uris.md)

 [Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4EZCAC"></a>

 
##### <a name="external-links--w3org-http11-status-code-definitionshttpswwww3orgprotocolsrfc2616rfc2616-sec10htmlsec10"></a>Vínculos externos [W3.org: Definiciones de código de estado HTTP/1.1](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10)

   