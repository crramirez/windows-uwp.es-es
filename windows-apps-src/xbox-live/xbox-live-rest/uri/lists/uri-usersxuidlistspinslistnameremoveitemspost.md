---
title: POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
assetID: f7235c68-3214-db10-52ad-c3665b31b8cd
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameremoveitemspost.html
description: " POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5e62d978e94286c815f2274c56684e4fd6bbf2d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637300"
---
# <a name="post-usersxuidxuidlistspinslistnameremoveitems"></a>POST /users/xuid(xuid)/lists/PINS/{listname}/RemoveItems
Quita elementos de una lista por Id. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EFB)
  * [Parámetros de cadena de consulta](#ID4EOC)
  * [Cuerpo de la solicitud](#ID4EZC)
  * [Códigos de estado HTTP](#ID4EED)
  * [Encabezados de solicitud necesarios](#ID4E1AAC)
  * [Cuerpo de respuesta](#ID4EQCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones 
 
Quitar elementos de la lista mediante la especificación de los identificadores de elemento en lugar de los índices. Se permiten solo 100 elementos que se quitarán en una sola llamada **si no hay elementos se pasan a continuación, se eliminará toda la lista (lista permanecerá pero está vacío, seguirá incrementar el número de versión).** Una vez que se quitan los elementos, la lista se "compacta" que no hay huecos se mantienen en el orden de los índices. 
 
Un "si-coincidencia: versionNumber" encabezado es opcional para esta llamada. Si se incluye se validarán. El número de versión es el número de versión actual del archivo. Si se incluye y no coincide con la actual se devolverá la lista de versiones de número y, a continuación, un HTTP 412 – Error de condición previa código de estado de error y el cuerpo de la respuesta contendrá los metadatos de la lista que incluye el número actual de la versión más reciente. Esto se hace para protegerse frente a las actualizaciones de distintos clientes trampling entre sí. 
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| XUID| string| XUID del usuario.| 
| nombre de lista| string| Nombre de la lista para manipular.| 
  
<a id="ID4EOC"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta 
 
No se admiten parámetros de consulta.
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud 
 

```cpp
{
   "Items":
   [{"ItemId":  "ed591a0c-dde3-563f-99af-530278a3c402",
      "ProviderId":  null,
      "Provider":  null
   }]
}

    
```

  
<a id="ID4EED"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP 
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4E1AAC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado. | 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4EQCAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta 
 
Si la llamada se realiza correctamente, el servicio devuelve los metadatos más reciente de la lista. 
 
<a id="ID4E1CAC"></a>

 
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

   
<a id="ID4EGDAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIDAC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   