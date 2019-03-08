---
title: DELETE (/users/xuid(xuid)/lists/PINS/{listname})
assetID: b43e3faa-7791-8bcb-3aec-7bdad8ffbebf
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamedelete.html
description: " DELETE (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eed1d73917be450038fdf098b802d0d7c1c44a7b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603350"
---
# <a name="delete-usersxuidxuidlistspinslistname"></a>DELETE (/users/xuid(xuid)/lists/PINS/{listname})
Quita elementos de una lista. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EIB)
  * [Parámetros de cadena de consulta](#ID4ETB)
  * [Autorización](#ID4ETC)
  * [Encabezados de solicitud necesarios](#ID4EAD)
  * [Cuerpo de la solicitud](#ID4EWE)
  * [Códigos de estado HTTP](#ID4EBF)
  * [Cuerpo de respuesta](#ID4E6BAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Eliminación de elementos se indican mediante su índice en la lista y se identifican en el parámetro de cadena de consulta **índices**. La lista de índices debe estar delimitado por comas y se pueden pasar solo 100 índices en una sola llamada. Sin embargo, si no hay índices se transfieren (parámetro de cadena de consulta vacía) se eliminarán el contenido de toda la lista (seguirá siendo una lista vacía y se seguirá incrementar el número de versión). Una vez que se quitan los elementos, la lista se "compacta" que no hay huecos se mantienen en el orden de los índices. Por lo tanto, esta llamada no es idempotente.
 
Esta llamada requiere un **si-coincidencia: versionNumber** encabezado que se incluirán en la solicitud, donde **versionNumber** es el número de versión actual del archivo. Si no se incluye o no coincide con el número de versión actual de lista y, a continuación, un HTTP 412 (error de condición previa) se devolverá el código de estado y el cuerpo de la respuesta contendrá los metadatos de la lista que incluye el número actual de la versión más reciente. Esto se hace para protegerse frente a las actualizaciones de distintos clientes trampling entre sí.
  
<a id="ID4EIB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID).| 
| ListType| string| Tipo de la lista (cómo se usa y cómo actúa). Siempre "ANCLA" para estos métodos relacionados.| 
| nombre de lista| string| Nombre de la lista (la lista de un determinado listtype para actuar sobre). Siempre "XBLPins" para los elementos de PIN.| 
  
<a id="ID4ETB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Índices| string| Opcional. Especifica dónde se debe quitar los elementos. Valores admitidos: 0, números enteros positivos y "end". Valor predeterminado: 0.| 
 
Ejemplo: **índices = 1, 8** quita los elementos en los índices 1 y 8. Los índices deben ser únicos. Si no se proporciona ningún índice, se borrará toda la lista.
  
<a id="ID4ETC"></a>

 
## <a name="authorization"></a>Autorización
 
Esta llamada espera un token SAML XSTS en el **autorización** encabezado. Una notificación Xuid debe existir dentro de ese token SAML para identificar el llamador. Este valor se utiliza para determinar si el llamador tiene los derechos de acceso a los datos de lista en cuestión. La propia lista se identifica mediante el Xuid así y se incluirá en el URI de la lista. Con esto, se puede en el futuro soporte compartido acceso a listas, pero no es una característica en este momento. Actualmente, todas las listas que un usuario accede a será sus propia y no hay ningún acceso compartido. Por lo tanto el Xuid en el URI debe coincidir con el Xuid en el token de notificaciones SAML. 

> [!NOTE] 
> En la actualidad se admiten XBL Auth 2.0 y 3.0 tokens. 


  
<a id="ID4EAD"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado.| 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La solicitud se completó correctamente. El cuerpo de respuesta debe contener el recurso solicitado (para una operación GET). POST y PUT solicitudes recibirán los metadatos de la lista actualizada (versión de la lista, recuento, etcetera.).| 
| 201| Creación| Se ha creado una nueva lista. Se devuelve en la inserción inicial en una lista. La respuesta incluye los metadatos actualizados en la lista y el encabezado de ubicación contiene el URI de la lista.| 
| 304| No modificado| Devuelto en obtiene. Indica que el cliente tiene la versión más reciente de la lista. El servicio compara el valor de la <b>If-Match</b> encabezado a la versión de la lista. Si son iguales, a continuación, se devuelve sin datos 304.| 
| 400| solicitud incorrecta| La solicitud era incorrecta. Podría ser un valor no válido o un tipo para un parámetro de cadena de consulta o URI. También puede ser el resultado de un parámetro obligatorio que falta o valor de datos o la solicitud indica una versión no válida o ausente de la API. Consulte la <b>X-XBL-contrato-Version</b> encabezado.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| El llamador no tiene acceso al recurso. Esto indica que no se ha creado ninguna lista de este tipo.| 
| 412| Error de condición previa| Indica que ha cambiado la versión de la lista y error en una solicitud de modificar. La solicitud de modificación es la inserción, actualización o eliminación. El servicio comprueba la <b>If-Match</b> encabezado para la versión de la lista. Si no coincide con la versión actual de la lista, se produce un error en la operación y se devuelve junto con los metadatos de lista actual (que incluyen la versión actual).| 
| 500| error interno del servidor| El servicio rechaza la solicitud debido a un error en el servidor.| 
| 501| No implementado| El llamador solicita un URI que no se ha implementado en el servidor. (Actualmente solo se usa cuando una solicitud se realiza para un nombre de la lista que no está permitido.)| 
| 503| servicio no disponible| El servidor rechaza la solicitud, normalmente debido a una carga excesiva. Después de un retraso (consulte la <b>Retry-after</b> encabezado), se puede volver a intentar la solicitud.| 
  
<a id="ID4E6BAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EFCAC"></a>

 
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

   
<a id="ID4EPCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERCAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   