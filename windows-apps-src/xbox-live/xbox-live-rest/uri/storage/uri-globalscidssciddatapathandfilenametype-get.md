---
title: GET (/global/scids/{scid}/data/{pathAndFileName},{type})
assetID: 5ca8e0dd-3c45-1b7b-022e-d5d61414fd7d
permalink: en-us/docs/xboxlive/rest/uri-globalscidssciddatapathandfilenametype-get.html
description: " GET (/global/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c7d83d6ee4b0b3787fe86c3ff21e64e6524d2a25
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628010"
---
# <a name="get-globalscidssciddatapathandfilenametype"></a>GET (/global/scids/{scid}/data/{pathAndFileName},{type})
Descarga un archivo. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Autorización](#ID4ECB)
  * [Parámetros de cadena de consulta opcional](#ID4EPB)
  * [Encabezados de solicitud necesarios](#ID4EZC)
  * [Encabezados de solicitud opcionales](#ID4ECE)
  * [Cuerpo de la solicitud](#ID4EMF)
  * [Códigos de estado HTTP](#ID4EZF)
  * [Encabezados de respuesta](#ID4EMDAC)
  * [Cuerpo de respuesta](#ID4EPEAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
| type| string| El formato de los datos. Los valores posibles son: binario, config o json.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorización 
 
La solicitud debe incluir un encabezado de autorización válido de Xbox LIVE. Si el llamador no puede acceder a este recurso, el servicio devolverá una respuesta 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devolverá una respuesta 401 no autorizado. 
  
<a id="ID4EPB"></a>

 
## <a name="optional-query-string-parameters"></a>Parámetros de cadena de consulta opcional 
 
Varía según el tipo de blob. Blobs binarios no admiten parámetros de consulta.
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| estable| string| Sólo se pueden usar cuando el tipo es json. Especifica que la respuesta sólo debe contener un determinado propiedad/valor de JSON, según lo determinado por este parámetro. Use un "punto" (.) para especificar subpropiedades y los corchetes ('[' y ']') para especificar los índices de matriz. Por ejemplo, "array1 .prop2 [4]" especifica la propiedad "prop2" del índice 4 de la matriz de "matriz 1".| 
| customSelector| string| Para los archivos del tipo de configuración. Indica qué nodos virtuales personalizados para incluir. Para obtener más información sobre los archivos de tipo de configuración, consulte almacenamiento de título.| 
  
<a id="ID4EZC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación del STS. STSTokenString se sustituye por el token devuelto por la solicitud de autenticación. Para obtener más información sobre cómo recuperar un token STS y crear un encabezado de autorización, consulte autenticación y autorización de Xbox LIVE las solicitudes de servicios.| 
  
<a id="ID4ECE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Especifica un valor ETag que debe coincidir con un elemento existente para completar la operación.| 
| If-None-Match| Especifica un valor ETag que no debe coincidir con ningún elemento existente para completar la operación.| 
| Intervalo| Especifica el intervalo de bytes que se descargue. Sigue el formato de encabezado de intervalo HTTP estándar.| 
  
<a id="ID4EMF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EZF"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar | La solicitud es correcta.| 
| 201| Creación | Se creó la entidad.| 
| 400| solicitud incorrecta | Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización | La solicitud requiere autenticación del usuario.| 
| 403| prohibido | No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado | No se encontró el recurso especificado.| 
| 406| No es aceptable | No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud | La solicitud tardó demasiada en completarse.| 
| 500| error interno del servidor | El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible | Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EMDAC"></a>

 
## <a name="response-headers"></a>Encabezados de respuesta
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag es un identificador opaco asignado por un servidor web a una versión específica de un recurso que se encuentra en una dirección URL. Si el contenido de los recursos en esa dirección URL cambia en algún momento, se asigna un valor de ETag nueva y diferente.| 
| Encabezado Content-Range| Si se trata de una descarga parcial, este encabezado especifica el intervalo de bytes descargados.| 
  
<a id="ID4EPEAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
Si la llamada se realiza correctamente, el servicio devolverá el contenido del archivo.  
<a id="ID4EYEAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E1EAC"></a>

 
##### <a name="parent"></a>Primario  

[/global/scids/{scid}/data/{pathAndFileName},{type}](uri-globalscidssciddatapathandfilenametype.md)

  
<a id="ID4EGFAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Referencia [TitleBlob (JSON)](../../json/json-titleblob.md)

   