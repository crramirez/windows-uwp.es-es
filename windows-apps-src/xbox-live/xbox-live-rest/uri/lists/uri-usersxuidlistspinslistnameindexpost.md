---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: df61be42-c229-7408-5e4c-dbf4ae95b52b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindexpost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7711beee6551c40afe1afcb031278484a3dc5820
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627470"
---
# <a name="post-usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Mueve un elemento en una lista a una posición diferente en la lista. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EEB)
  * [Parámetros de cadena de consulta](#ID4EWC)
  * [Cuerpo de la solicitud](#ID4EVD)
  * [Códigos de estado HTTP](#ID4EEE)
  * [Encabezados de solicitud necesarios](#ID4E1BAC)
  * [Cuerpo de respuesta](#ID4EQDAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones 
 
Esta llamada se proporciona para permitir que el cliente puede mover fácilmente un elemento a otro índice dentro de la lista en una sola operación. Solo un elemento se puede mover a la vez. Si no existe el índice del elemento que se va a mover, a continuación, se devolverá un HTTP 400. El índice del punto de inserción sigue las mismas reglas que una inserción estándar. De forma predeterminada en 0: el principio de la lista y cualquier número mayor que el número de elementos en la lista de volver a insertará el elemento al final de la lista. De forma similar al final de la lista puede indicarse pasando "end" para insertIndex. 
 
Esta llamada también permite identificar el elemento que se va a mover el itemId (o proveedor/providerId combinado). En este caso, se debe pasar el identificador de elemento en el cuerpo de la solicitud y debe coincidir con un elemento existente en la lista. Si no es así, se devolverá un error HTTP 400 con IndexOutOfRange en la descripción; para esta versión especial de la llamada, use "-1" como el índice del elemento va a mover. 
 
Esta llamada requiere un "si-coincidencia: versionNumber" encabezado que se incluirán en la solicitud si se especifica el elemento por índice. Si usa itemId para indicar qué elemento va a mover, el encabezado "If-Match" es opcional. Si está presente, siempre se validará el encabezado "If-Match". En el encabezado "if-Match", el número de versión es el número de versión actual de la lista. Si no está incluido (y necesario), o no coincide con la actual se devolverá la lista de versiones de número y, a continuación, un HTTP 412 – Error de condición previa código de estado de error y el cuerpo de la respuesta contendrá los metadatos de la lista que incluye el número actual de la versión más reciente . Esto se hace para protegerse frente a las actualizaciones de distintos clientes trampling entre sí. 
  
<a id="ID4EEB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| XUID| string| XUID del usuario.| 
| nombre de lista| string| Nombre de la lista para manipular.| 
| index| string| Especifica el índice actual del elemento que se va a mover. Si el valor de índice es cero o un entero positivo, esto se hace referencia al índice del elemento actual y el cuerpo de solicitud de la llamada debe estar vacío. Sin embargo, si el valor de índice es "-1", debe especificarse el elemento que se va a mover ItemId o proveedor/ProviderID en el cuerpo de solicitud de la llamada.| 
  
<a id="ID4EWC"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| Especifica la ubicación de la lista para insertar el elemento. Valores permitidos son "end", números enteros positivos y cero. "final" coloca el elemento al final de la lista actual. Si el valor especificado está al final de la lista, el elemento se inserta al final de la lista. | 
  
<a id="ID4EVD"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 
Un cuerpo de solicitud se envía únicamente cuando se especifica el elemento al mover itemId o proveedor/ProviderId.
 
<a id="ID4E6D"></a>

  
Solicitud de ejemplo 

```cpp
{
  "Item":
  {
    "ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
    "ProviderId":  null,
    "Provider": null
  }
}
    
```

  
<a id="ID4EEE"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar | La solicitud se completó correctamente. El cuerpo de respuesta debe contener el recurso solicitado (para una operación GET). POST y PUT solicitudes recibirán los metadatos de la lista actualizada (versión de la lista, recuento, etcetera.).| 
| 201| Creación | Se ha creado una nueva lista. Se devuelve en la inserción inicial en una lista. La respuesta incluye los metadatos actualizados en la lista y el encabezado de ubicación contiene el URI de la lista.| 
| 304| No modificado| Devuelto en obtiene. Indica que el cliente tiene la versión más reciente de la lista. El servicio compara el valor de la <b>If-Match</b> encabezado a la versión de la lista. Si son iguales, a continuación, se devuelve sin datos 304. | 
| 400| solicitud incorrecta | La solicitud era incorrecta. Podría ser un valor no válido o un tipo para un parámetro de cadena de consulta o URI. También puede ser el resultado de un parámetro obligatorio que falta o valor de datos o la solicitud indica una versión no válida o ausente de la API. Consulte la <b>X-XBL-contrato-Version</b> encabezado. | 
| 401| Sin autorización | La solicitud requiere autenticación del usuario.| 
| 403| prohibido | No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado | El llamador no tiene acceso al recurso. Esto indica que no se ha creado ninguna lista de este tipo.| 
| 412| Error de condición previa | Indica que ha cambiado la versión de la lista y error en una solicitud de modificar. La solicitud de modificación es la inserción, actualización o eliminación. El servicio comprueba la <b>If-Match</b> encabezado para la versión de la lista. Si no coincide con la versión actual de la lista, se produce un error en la operación y se devuelve junto con los metadatos de lista actual (que incluyen la versión actual). | 
| 500| error interno del servidor | El servicio rechaza la solicitud debido a un error en el servidor.| 
| 501| No implementado | El llamador solicita un URI que no se ha implementado en el servidor. (Actualmente solo se usa cuando una solicitud se realiza para un nombre de la lista que no está permitido.)| 
| 503| servicio no disponible | El servidor rechaza la solicitud, normalmente debido a una carga excesiva. Después de un retraso (consulte la <b>Retry-after</b> encabezado), se puede volver a intentar la solicitud. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado.| 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4EQDAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta 
 
Si la llamada se realiza correctamente, el servicio devuelve los metadatos más reciente de la lista. 
 
<a id="ID4E1DAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo 
 

```cpp
{ 
  "ListVersion":  1,
  "ListCount":  1,
  "MaxListSize": 200,
  "AllowDuplicates": "true",
  "AccessSetting":  "OwnerOnly"
}

      
```

   
<a id="ID4EIEAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EKEAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

   