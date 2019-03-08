---
title: POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
assetID: fb4cff17-2721-89c5-6646-5ab76952b411
permalink: en-us/docs/xboxlive/rest/uri-jsonusersbatchscidssciddatapathandfilenametype-post.html
description: " POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cfe20136ad1320966fc851cf9dd6de765871c957
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623300"
---
# <a name="post-jsonusersbatchscidssciddatapathandfilenamejson"></a>POST (/json/users/batch/scids/{scid}/data/{pathAndFileName},json)
Descarga varios archivos de varios usuarios con el mismo nombre de archivo. Descarga el archivo viene determinada por el URI de la solicitud. El cuerpo de la solicitud contiene la lista de XUIDs de los usuarios cuyos archivos para descargar. El cuerpo de la respuesta será un mensaje MIME de varias partes, con cada elemento que representa un archivo para un usuario determinado con su propio conjunto de encabezados. Es posible que las partes de la respuesta a ser una combinación de operaciones correctas y erróneas. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Autorización](#ID4ECB)
  * [Cuerpo de la solicitud](#ID4EPB)
  * [Códigos de estado HTTP](#ID4E3C)
  * [Encabezados de respuesta necesaria](#ID4EPAAC)
  * [Encabezados de respuesta opcional](#ID4ESBAC)
  * [Cuerpo de respuesta](#ID4E3CAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
  
<a id="ID4ECB"></a>

 
## <a name="authorization"></a>Autorización 
 
La solicitud debe incluir un encabezado de autorización válido de Xbox LIVE. Si el llamador no puede acceder a este recurso, el servicio devolverá una respuesta 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devolverá una respuesta 401 no autorizado. 
  
<a id="ID4EPB"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
| Propiedad| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| xuids| matriz de enteros de 64 bits sin signo| La lista de XUIDs para que se va a descargar archivos.| 
 
<a id="ID4EQC"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
    "xuids" : 
    [
      12345,
      45678,
      78901
    ]
}
      
```

   
<a id="ID4E3C"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPAAC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Disposition| Describe el contenido del elemento. Las partes "name" y "filename" del encabezado son el XUID del usuario que pertenece este archivo.| 
| HttpStatusCode| El código de estado HTTP relacionadas con la recuperación de este archivo en particular.| 
  
<a id="ID4ESBAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| ETag es un identificador opaco asignado por un servidor web a una versión específica de un recurso que se encuentra en una dirección URL. Si el contenido de los recursos en esa dirección URL cambia en algún momento, se asigna un valor de ETag nueva y diferente.| 
| Content-Type| Si el archivo se ha recuperado correctamente, se trata el tipo de contenido del archivo.| 
| Encabezado Content-Range| Si el archivo se ha recuperado correctamente y es una descarga parcial, se trata del intervalo de bytes del archivo de contenido en la respuesta. | 
  
<a id="ID4E3CAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá el contenido de los archivos solicitados en una respuesta de varias partes.
 
<a id="ID4EGDAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo 
 

```cpp
HTTP/1.1 200 OK
Transfer-Encoding: chunked
Content-Type: multipart/form-data; boundary=c0a9fd75-d036-4294-8b7b-85fea15a31bb

228
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="123"; filename="123"
HttpStatusCode: 200
ETag: 0x8CF327717411C31
Content-Type: application/octet-stream

asdf123
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="456"; filename="456"
HttpStatusCode: 200
ETag: 0x8CF32771E954BB8
Content-Type: application/octet-stream

asdf456
--c0a9fd75-d036-4294-8b7b-85fea15a31bb
Content-Disposition: binary; name="789"; filename="789"
HttpStatusCode: 404


--c0a9fd75-d036-4294-8b7b-85fea15a31bb--

0

```

   
<a id="ID4EUDAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EWDAC"></a>

 
##### <a name="parent"></a>Primario 

[/json/users/batch/scids/{scid}/data/{pathAndFileName},json](uri-jsonusersbatchscidssciddatapathandfilenametype.md)

   