---
title: GET (/sessions/{sessionId}/scids/{scid}/data/{path})
assetID: 007821b8-16f0-2fe1-5196-890743d77775
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapath-get.html
description: " GET (/sessions/{sessionId}/scids/{scid}/data/{path})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 28a279347fa5a463c0d482a624af831c0cdb0fba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623720"
---
# <a name="get-sessionssessionidscidssciddatapath"></a>GET (/sessions/{sessionId}/scids/{scid}/data/{path})
Muestra información de archivo en una ruta de acceso especificada. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Parámetros de cadena de consulta opcional](#ID4ECB)
  * [Autorización](#ID4EWC)
  * [Encabezados de solicitud necesarios](#ID4EDD)
  * [Cuerpo de la solicitud](#ID4EME)
  * [Códigos de estado HTTP](#ID4EZE)
  * [Cuerpo de respuesta](#ID4EUBAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| sessionId| string| el identificador de la sesión para buscar.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| ruta de acceso| string| La ruta de acceso a los elementos de datos para devolver. Se devuelven todos los directorios y subdirectorios de búsqueda de coincidencias. Caracteres válidos son letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y barra diagonal (/). Puede estar vacío. Longitud máxima de 256.| 
  
<a id="ID4ECB"></a>

 
## <a name="optional-query-string-parameters"></a>Parámetros de cadena de consulta opcional 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| skipItems| entero| Devuelve los elementos que empieza por N + 1 en la colección, por ejemplo, omitir N elementos.| 
| continuationToken| string| Devolver los elementos comenzando en el token de continuación determinado. El parámetro continuationToken tiene prioridad sobre skipItems si se especifican ambas. En otras palabras, el parámetro skipItems se omite si el parámetro continuationToken está presente.| 
| maxItems| entero| Número máximo de elementos que se va a devolver de la colección, que se puede combinar con skipItems y continuationToken para devolver un intervalo de elementos. El servicio puede proporcionar un valor predeterminado si maxItems no está presente y es posible que devuelva menos de maxItems, incluso si aún no se ha devuelto la última página de resultados. | 
  
<a id="ID4EWC"></a>

 
## <a name="authorization"></a>Autorización 
 
La solicitud debe incluir un encabezado de autorización válido de Xbox LIVE. Si el llamador no puede acceder a este recurso, el servicio devolverá una respuesta 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devolverá una respuesta 401 no autorizado. 
  
<a id="ID4EDD"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación del STS. STSTokenString se sustituye por el token devuelto por la solicitud de autenticación. Para obtener más información sobre cómo recuperar un token STS y crear un encabezado de autorización, consulte autenticación y autorización de Xbox LIVE las solicitudes de servicios.| 
  
<a id="ID4EME"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EZE"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La solicitud es correcta.| 
| 201| Creación| Se creó la entidad.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| La solicitud tardó demasiada en completarse.| 
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EUBAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá una matriz de [TitleBlob](../../json/json-titleblob.md) objetos. 
 
<a id="ID4ECCAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
"blobs":
[
    {
        "fileName":"foo\bar\blob.txt,binary",
        "clientFileTime":"2012-01-01T01:02:03.1234567Z",
        "displayName":"Friendly Name",
        "size":12,
        "etag":"0x8CEB3E4F8F3A5BF"
    },
    {
        "fileName":"foo\bar\blob2.txt,binary",
        "displayName":"Blob 2",
        "size":4,
        "etag":"0x8CEB3FE57F1A142"
    },
    {
        "fileName":"foo\jsonblob.txt,json",
        "size":15,
        "etag":"0x8CEB40152B4A6F8"
    }
],
"pagingInfo":
    {
        "continuationToken":"54",
    }
}
         
```

   
<a id="ID4EOCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQCAC"></a>

 
##### <a name="parent"></a>Primario  

[/sessions/{sessionId}/scids/{scid}/data/{path}](uri-sessionssessionidscidssciddatapath.md)

  
<a id="ID4E3CAC"></a>

 
##### <a name="reference--titleblob-jsonjsonjson-titleblobmd"></a>Referencia [TitleBlob (JSON)](../../json/json-titleblob.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

   