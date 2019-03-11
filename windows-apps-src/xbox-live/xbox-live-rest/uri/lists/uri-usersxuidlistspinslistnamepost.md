---
title: POST (/users/xuid(xuid)/lists/PINS/{listname})
assetID: 813c0bd2-aca9-a1f6-9e81-a84a4c897b1e
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnamepost.html
description: " POST (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d30e5407be128032947f3f701f59ef25a16daaea
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589980"
---
# <a name="post-usersxuidxuidlistspinslistname"></a>POST (/users/xuid(xuid)/lists/PINS/{listname})
Inserta elementos en la lista en el índice basado en el parámetro de cadena de consulta **insertIndex**. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EY)
  * [Parámetros de URI](#ID4ETB)
  * [Parámetros de cadena de consulta](#ID4E5B)
  * [Autorización](#ID4EZC)
  * [Códigos de estado HTTP](#ID4EGD)
  * [Encabezados de solicitud necesarios](#ID4EEAAC)
  * [Solicitud de ejemplo](#ID4E1BAC)
  * [Cuerpo de respuesta](#ID4EPCAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Observaciones
 
Esta llamada insertará los elementos en la lista en el índice basado en el parámetro de cadena de consulta **insertIndex** (valor predeterminado es 0 o al principio de la lista). Todos los elementos en el cuerpo de solicitud se insertará en ese momento en la lista. Si el **insertIndex** es mayor que el número de elementos en la lista existente, se insertarán los nuevos elementos al final.
 
Los elementos que va a insertar deben tener los campos obligatorios se indican en la especificación funcional; en caso contrario, se devolverá un HTTP 400. De forma similar, si el resultado de la instrucción insert supera el tamaño máximo de la lista (definido por el tipo de lista), a continuación, se devolverá un HTTP 400 y nada se van a insertar.
 
Si el elemento es *no* va a insertar al principio o al final de la lista, el **si-coincidencia: versionNumber** encabezado es necesario que se incluirán en la solicitud. El encabezado es opcional si la inserción es para el principio o al final. Si está presente, el encabezado se validarán independientemente de la ubicación de inserción. En el encabezado **VersionNumber** es el número de versión actual de la lista. Si no está incluido y es necesario, o no coincide con el número de versión actual de lista y, a continuación, un HTTP 412 (error de condición previa) se devolverá el código de estado y el cuerpo de la respuesta contendrá los metadatos de la lista que incluye el número actual de la versión más reciente. Esto es para protegerse frente a las actualizaciones de distintos clientes trampling entre sí.
 
Tenga en cuenta que esta llamada no es idempotente; las llamadas repetidas, con los mismos datos podrían producir varias inserciones. Sin embargo, puesto que ninguna lista actualmente admite duplicados, llamadas repetidas superará probablemente se producirá en HTTP 400 códigos que se devuelven.
  
<a id="ID4ETB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID).| 
| ListType| string| Tipo de la lista (cómo se usa y cómo actúa). Siempre "ANCLA" para estos métodos relacionados.| 
| nombre de lista| string| Nombre de la lista (la lista de un determinado listtype para actuar sobre). Siempre "XBLPins" para los elementos de PIN.| 
  
<a id="ID4E5B"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| insertIndex| string| Opcional. Define dónde se va a insertar elementos. Valores admitidos: 0, números enteros positivos y "end". Cualquier valor de índice mayor que el número de elementos de lista agregará el nuevo elemento en la parte inferior de la lista y no insertará espacio "blank" en la lista. Valor predeterminado: 0.| 
  
<a id="ID4EZC"></a>

 
## <a name="authorization"></a>Autorización
 
Esta llamada espera un token SAML XSTS en el **autorización** encabezado. Una notificación Xuid debe existir dentro de ese token SAML para identificar el llamador. Este valor se utiliza para determinar si el llamador tiene los derechos de acceso a los datos de lista en cuestión. La propia lista se identifica mediante el Xuid así y se incluirá en el URI de la lista. Con esto, se puede en el futuro soporte compartido acceso a listas, pero no es una característica en este momento. Actualmente, todas las listas que un usuario accede a será sus propia y no hay ningún acceso compartido. Por lo tanto el Xuid en el URI debe coincidir con el Xuid en el token de notificaciones SAML. 

> [!NOTE] 
> En la actualidad se admiten XBL Auth 2.0 y 3.0 tokens. 


  
<a id="ID4EGD"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EEAAC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado.| 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4E1BAC"></a>

 
## <a name="sample-request"></a>Solicitud de ejemplo
 
**ContentType**, **ItemId** o **ProviderId**, **proveedor** y **configuración regional** son obligatorios.
 

```cpp
{"Items":
  [{
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": "",
    "Provider": "",
    "ImageUrl": "https://www.bing.com/thumb/get?bid=Gw%2fsjCGSS4kAAQ584x800&bn=SANGAM&fbid=7wIR63+Clmj+0A&fbn=CC", 
    "AltImageUrl": null, 
    "Title": "The Dark Knight", 
    "SubTitle": null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
  }]}
      
```

  
<a id="ID4EPCAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EVCAC"></a>

 
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

   
<a id="ID4E6CAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBDAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   