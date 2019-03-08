---
title: PUT (/users/xuid(xuid)/lists/PINS/{listname})
assetID: f7964d2e-a8c8-2caa-ca97-e0d76ef586f4
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameput.html
description: " PUT (/users/xuid(xuid)/lists/PINS/{listname})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 35527df28c2ca482d0c5ae2fe25637b3bc29f6ca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622930"
---
# <a name="put-usersxuidxuidlistspinslistname"></a>PUT (/users/xuid(xuid)/lists/PINS/{listname})
Actualiza los elementos en una lista en función de los índices especificados para cada elemento en el cuerpo de solicitud. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E1B)
  * [Autorización](#ID4EFC)
  * [Códigos de estado HTTP](#ID4ESC)
  * [Encabezados de solicitud necesarios](#ID4EPH)
  * [Cuerpo de la solicitud](#ID4EGBAC)
  * [Cuerpo de respuesta](#ID4EWBAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Esta llamada actualizará los elementos de una lista en función de los índices especificados para cada elemento en el cuerpo de solicitud. Esta llamada no insertará los elementos en la lista y, si no existen elementos en los índices especificados, la llamada devolverá un estado 400 Solicitud incorrecta. Se pueden actualizar varios elementos en una sola llamada, pero debe existir en la lista actual. Es decir, se actualizan todos o ninguno se actualizan.
 
Esta llamada permitirá que el elemento que se actualizará para especificar el **itemId** en lugar de **índice**. Para ello, simplemente use "-1" para el índice en el **IndexedItems** estructura que se envía al servicio. Obviamente, en este caso, el **itemId** no se puede cambiar como parte de la actualización, por lo que solo funcionará para los cambios a otros campos de metadatos. El **proveedor**/**providerId** combinado se puede usar en lugar de **itemId** para identificar el elemento. Internamente, el servicio busca en la lista de estos elementos y las cifras de los índices adecuados para actualizar. Si no se encuentra el elemento o elementos, a continuación, se devolverá un estado 400 Solicitud incorrecta y no se actualizará ningún elemento.
 
Esta llamada requiere un **si-coincidencia: versionNumber** encabezado que se incluirán en la solicitud si la utilización de índices para identificar los elementos. Si usando identificadores para identificar los elementos de elemento (y la lista no permite duplicados), la **If-Match** encabezado es opcional. Si está presente, el **if-Match** encabezado siempre se validará. En el encabezado, el **versionNumber** es el número de versión actual de la lista. Si no está incluido (y necesario), o no se devolverá el número de versión actual de lista y, a continuación, un código de estado HTTP 412 Precondition error de coincidencia y el cuerpo de la respuesta contendrá los metadatos de la lista más reciente incluye el número de versión actual. Esto es para protegerse frente a las actualizaciones de distintos clientes trampling entre sí.
  
<a id="ID4E1B"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID).| 
| ListType| string| Tipo de la lista (cómo se usa y cómo actúa). Siempre "ANCLA" para estos métodos relacionados.| 
| nombre de lista| string| Nombre de la lista (la lista de un determinado listtype para actuar sobre). Siempre "XBLPins" para los elementos de PIN.| 
  
<a id="ID4EFC"></a>

 
## <a name="authorization"></a>Autorización
 
Esta llamada espera un token SAML XSTS en el **autorización** encabezado. Una notificación Xuid debe existir dentro de ese token SAML para identificar el llamador. Este valor se utiliza para determinar si el llamador tiene los derechos de acceso a los datos de lista en cuestión. La propia lista se identifica mediante el Xuid así y se incluirá en el URI de la lista. Con esto, se puede en el futuro soporte compartido acceso a listas, pero no es una característica en este momento. Actualmente, todas las listas que un usuario accede a será sus propia y no hay ningún acceso compartido. Por lo tanto el Xuid en el URI debe coincidir con el Xuid en el token de notificaciones SAML. 

> [!NOTE] 
> En la actualidad se admiten XBL Auth 2.0 y 3.0 tokens. 


  
<a id="ID4ESC"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
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
  
<a id="ID4EPH"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Contiene el token de STS que se usa para autenticar y autorizar la solicitud. Debe ser un token de servicio XSTS e incluyen la XUID como una de las notificaciones. | 
| X-XBL-Contract-Version| Especifica qué versión de API se va a solicitado (enteros positivos). Versión de es compatible con los PIN 2. Si falta este encabezado o el valor no se admite el servicio se devolverá un 400 – Solicitud incorrecta con "encabezado de versión de contrato no compatible o que faltan" en la descripción del estado.| 
| Content-Type| Especifica si el contenido de los cuerpos de solicitud/respuesta estará en json o xml. Los valores admitidos son "application/json" y "application/xml"| 
| If-Match| Este encabezado debe contener el número de versión actual de una lista al realizar solicitudes de modificación. Uso de las solicitudes PUT, POST, de modificar o eliminar los verbos. Si la versión en el encabezado "If-Match" no coincide con la versión actual de la lista, se rechazará la solicitud con un HTTP 412: código de retorno de error de condición previa. (opcional) También puede utilizarse para funciones Get, si existe y la versión pasada coincide con la versión actual de la lista, a continuación, no hay datos de lista y un 304 HTTP, se devolverá el código no modificado. | 
  
<a id="ID4EGBAC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4EMBAC"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{"IndexedItems":
 [{ "Index": 0, 
     "Item": 
     {
    "ContentType": "Movie",
    "ItemId": "3a5095a5-eac3-4215-944d-27bc051faa47",
    "ProviderId": null,
    "Provider": null,
    "ImageUrl": "https://www.bing.com/thumb/...",
    "Title": "The Dark Knight",
    "SubTitle":null, 
    "Locale": "en-us",
    "DeviceType": "XboxOne"
}
}]}      
      
```

   
<a id="ID4EWBAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4E3BAC"></a>

 
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

   
<a id="ID4EGCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EICAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

   