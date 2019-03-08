---
title: PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
assetID: ed037b64-6525-99a2-b7f6-050fdf345de8
permalink: en-us/docs/xboxlive/rest/uri-trustedplatformusersxuidscidssciddatapathandfilenametype-put.html
description: " PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f04deef3a7c9eba494fdce8e7df16d9815cfc1ba
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594130"
---
# <a name="put-trustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>PUT (/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type})
Carga un archivo. Los datos se pueden cargar en una carga completa en la que se envían los datos y metadatos en un único mensaje, o como una carga de varios bloque en el que se envían los datos y metadatos en una serie de bloques más pequeños. Solo los archivos que tengan menos de 4 megabytes pueden enviarse como un solo mensaje. No se admite la carga de varios bloque para los datos de tipo json. Es el dominio para estos URI `titlestorage.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Autorización](#ID4EEB)
  * [Parámetros de cadena de consulta opcional](#ID4ERB)
  * [Encabezados de solicitud necesarios](#ID4EQE)
  * [Encabezados de solicitud opcionales](#ID4EZF)
  * [Cuerpo de la solicitud](#ID4E3G)
  * [Códigos de estado HTTP](#ID4EHH)
  * [Cuerpo de respuesta](#ID4E1EAC)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero de 64 bits sin signo| El usuario Xbox ID (XUID) del Reproductor que realiza la solicitud.| 
| scid| guid| el identificador de la configuración de servicio para buscar.| 
| pathAndFileName| string| Ruta de acceso y nombre del elemento que se va a obtenerse. Los caracteres válidos para la parte de la ruta de acceso (hasta e incluida la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_) y la barra diagonal (/). La parte de la ruta de acceso puede estar vacía. Los caracteres válidos para la parte del nombre de archivo (todos los elementos después de la barra diagonal final) incluyen letras mayúsculas (A-z), letras minúsculas (a-z), números (0-9), un carácter de subrayado (_), punto (.) y guión (-). El nombre de archivo no puede estar vacío, terminar con un punto ni contener dos puntos consecutivos.| 
| type| string| El formato de los datos. Los valores posibles son binario o json.| 
  
<a id="ID4EEB"></a>

 
## <a name="authorization"></a>Autorización 
 
La solicitud debe incluir un encabezado de autorización válido de Xbox LIVE. Si el llamador no puede acceder a este recurso, el servicio devolverá una respuesta 403 Prohibido. Si el encabezado está ausente o no válido, el servicio devolverá una respuesta 401 no autorizado. 
  
<a id="ID4ERB"></a>

 
## <a name="optional-query-string-parameters"></a>Parámetros de cadena de consulta opcional 
 
Para cargas de mensaje único, los parámetros de cadena de consulta son:
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Fecha y hora del archivo en cualquier cliente cargó por última vez el archivo.| 
| displayName| string| Nombre del archivo que se debe mostrar al usuario.| 
 
Para cargas de varios bloques, los parámetros de cadena de consulta son:
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| clientFileTime| DateTime| Fecha y hora del archivo en cualquier cliente cargó por última vez el archivo.| 
| displayName| string| Nombre del archivo que se debe mostrar al usuario.| 
| continuationToken| string| El token de continuación de la respuesta de la solicitud de carga anterior. Si se trata del primer bloque, esto no se debe especificar. | 
| finalBlock| bool| Establecido en true para el último bloque del archivo. Se establece en false para todos los demás bloques.| 
  
<a id="ID4EQE"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
| Autorización| XBL3.0 x = [hash]; [token]| Token de autenticación del STS. STSTokenString se sustituye por el token devuelto por la solicitud de autenticación. Para obtener más información sobre cómo recuperar un token STS y crear un encabezado de autorización, consulte autenticación y autorización de Xbox LIVE las solicitudes de servicios.| 
  
<a id="ID4EZF"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-Match| Especifica un valor ETag que debe coincidir con un elemento existente para completar la operación.| 
| If-None-Match| Especifica un valor ETag que no debe coincidir con ningún elemento existente para completar la operación.| 
  
<a id="ID4E3G"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
El cuerpo de solicitud contiene el contenido del archivo que se va a cargar. Para cargas de mensaje único, el cuerpo es el contenido completo del archivo. Para cargas de varios bloques, el cuerpo es la parte del archivo especificado en los parámetros de cadena de consulta. 
  
<a id="ID4EHH"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E1EAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta 
 
Si la llamada es una solicitud de varios bloque y es correcta, el servicio devolverá un token continution que se pasan con el siguiente bloque.
 
<a id="ID4EGFAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "continuationToken":"abcd1234-1111-2222-3333-abcd12345678-1"
}
         
```

   
<a id="ID4ESFAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EUFAC"></a>

 
##### <a name="parent"></a>Primario  

[/trustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}](uri-trustedplatformusersxuidscidssciddatapathandfilenametype.md)

   