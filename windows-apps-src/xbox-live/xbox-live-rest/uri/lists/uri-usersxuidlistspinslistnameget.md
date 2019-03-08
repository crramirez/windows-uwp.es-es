---
title: GET (/users/xuid(xuid)/lists/PINS/{listname})
assetID: a63f595a-61dd-5885-c405-9833230abb94
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameget.html
description: " GET (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a31e6a6b541276d3191ba5d40767a1929c70168
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641630"
---
# <a name="get-usersxuidxuidlistspinslistname"></a>GET (/users/xuid(xuid)/lists/PINS/{listname})
Devuelve el contenido de una lista. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EIB)
  * [Parámetros de cadena de consulta](#ID4ETB)
  * [Autorización](#ID4ESD)
  * [Encabezados de solicitud necesarios](#ID4E6D)
  * [Cuerpo de la solicitud](#ID4EVF)
  * [Códigos de estado HTTP](#ID4EAG)
  * [Cuerpo de respuesta](#ID4E5CAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
El **listCount** campo en los datos devueltos indica cuántos elementos se encuentran en la lista total mantenida por el servicio, por lo tanto, se puede usar para determinar dónde está al final de la lista, y esto es potencialmente un número diferente de cuántos se devolvieron elementos específicos por la solicitud.
 
Si aún no existe la lista y, después, los resultados no contendrán ningún elemento de lista, el **listCount** será cero y el **listVersion** será igual cero.
  
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
| skipItems| entero de 32 bits con signo| Opcional. Número de elementos que se omiten en la enumeración antes de devolver resultados. Valor predeterminado: 0.| 
| maxItems| entero de 32 bits con signo| Opcional. Número máximo de elementos que se va a devolver. El valor predeterminado es 25 elementos si no se especifica ningún máximo en la solicitud. El servicio no impone un máximo de este valor; Si el valor es mayor que el número de elementos de la lista, se devolverán todos los elementos con ningún error.| 
| filterItemId| string| Opcional. Especifica el elemento que se va a buscar en la lista. Devuelve todas las instancias del elemento de la lista. Permite al cliente determinar rápidamente si y cuando un elemento está en una lista. Es útil para las listas grandes determinar todas las instancias de un elemento sin recorrer en iteración toda la lista. Valor predeterminado: null.| 
| filterContentType| string| Opcional. Especifica una lista separada por comas de los tipos de contenido para devolver (no se devolverán tipos no está en la lista). Se usa para obtener solo determinados tipos de contenido de una lista. Se usa una lista separada por comas de los tipos de contenido para este filtro. (Varios tipos de contenido se pueden consultar en una llamada). Tipos de contenido admitidos incluyen todos los tipos de medios definidos por servicios de detección de entretenimiento (EDS). Valor predeterminado: null (todos los tipos de contenido).| 
| filterDeviceType| string| Opcional. Especifica una lista separada por comas de los tipos de dispositivo para devolver (no se devolverán tipos no está en la lista). Filtra el conjunto devuelto para devolver solo los elementos que se han insertado de un conjunto específico de tipos de dispositivo. Se usa una lista separada por comas de tipos de dispositivo para este filtro (se pueden consultar varios tipos de dispositivo en una llamada). Posibles valores: XboxOne, MCapensis, Windows Phone, WindowsPhone7, Web, PC, MoLive. Valor predeterminado: null (todos los tipos de contenido).| 
  
<a id="ID4ESD"></a>

 
## <a name="authorization"></a>Autorización
 
Esta llamada espera un token SAML XSTS en el **autorización** encabezado. Una notificación Xuid debe existir dentro de ese token SAML para identificar el llamador. Este valor se utiliza para determinar si el llamador tiene los derechos de acceso a los datos de lista en cuestión. La propia lista se identifica mediante el Xuid así y se incluirá en el URI de la lista. Con esto, se puede en el futuro soporte compartido acceso a listas, pero no es una característica en este momento. Actualmente, todas las listas que un usuario accede a será sus propia y no hay ningún acceso compartido. Por lo tanto el Xuid en el URI debe coincidir con el Xuid en el token de notificaciones SAML. 

> [!NOTE] 
> En la actualidad se admiten XBL Auth 2.0 y 3.0 tokens. 


  
<a id="ID4E6D"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado.| 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4EVF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EAG"></a>

 
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
  
<a id="ID4E5CAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EEDAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{ 
"ListMetadata":
  {"ListVersion": 12,
   "ListCount": 6,
   "MaxListSize": 200,
   "AccessSetting": "OwnerOnly",
   "AllowDuplicates": true
  },
"ListItems":
  [{ 
   "Index": 0,
   "DateAdded": "\/Date(1198908717056)/",
   "DateModified": "\/Date(1198908717056)/",
   "HydrationResult": "Indeterminate",
   "HydratedItem": null

   "Item":
   {
     "ContentType": "Movie",
     "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
     "ProviderId": null,
     "Provider": null,
     "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC",
     "Title": "The Dark Knight",
     "SubTitle": null,
     "Locale": "en-us",
     "AltImageUrl": null,
     "DeviceType": "XboxOne"
    }
  }]
}
         
```

   
<a id="ID4EODAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQDAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

  
<a id="ID4E1DAC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../marketplace/atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   