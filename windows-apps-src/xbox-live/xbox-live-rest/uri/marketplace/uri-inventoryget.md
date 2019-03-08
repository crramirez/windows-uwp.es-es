---
title: GET (/users/me/inventory)
assetID: 7b74dd08-2854-319d-3ed0-ddee75d922b9
permalink: en-us/docs/xboxlive/rest/uri-inventoryget.html
description: " GET (/users/me/inventory)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 31c787108fad84f06b02ded3958f9d2f99727cbe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632210"
---
# <a name="get-usersmeinventory"></a>GET (/users/me/inventory)
Proporciona el conjunto de inventario actualmente asociado con el usuario proporcionado al llamador.
Es el dominio para estos URI `inventory.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de cadena de consulta](#ID4EHB)
  * [Solicitud de ejemplo](#ID4EDE)
  * [Cuerpo de respuesta](#ID4ERE)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

No hay ninguna directiva comprueba, cumplimiento, o el filtrado se producirá como parte de esta llamada. Los llamadores tienen la opción de pasar los parámetros de consulta con el fin de restringir el ámbito de los resultados devueltos.

Los autores de llamadas pueden navegar por los resultados con un token de continuación a través de una respuesta anterior: **/users/me/inventory? continuationToken = continuationTokenString**.

El llamador puede realizar una llamada a los detalles de la API con una dirección URL del elemento específico con el fin de ver información sobre un elemento específico.

<a id="ID4EHB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| disponibilidad| string| La disponibilidad actual de elementos que se va a devolver. Valor predeterminado es "Disponible", que devuelve los elementos para los que la fecha actual se encuentra entre la fecha de inicio y final de intervalo de fechas. Otros valores son "All", que devuelve todos los elementos y "No disponible" que devuelve los elementos para los que la fecha actual no entra en el intervalo de fechas de inicio final y de fecha y, por tanto, no está disponible actualmente. |
| Contenedor| string| Opcional. Si establece el valor para el identificador de producto de un juego, los resultados del inventario de incluirán solo los elementos relacionados con este juego. Esto es especialmente útil cuando se llama a inventario desde su servidor para filtrar los resultados hacia abajo para productos de un juego específico.|
| expandSatisfyingEntitlements| string| Una marca que indica si la respuesta incluye todos los derechos satisfactorios que el usuario ha dentro de los resultados devueltos. El valor predeterminado es "false". Cuando este parámetro se usa con un valor "true", los productos que se conceden al usuario a través de satisfacer elementos agrupados como de los derechos, las compras de Xbox 360 se migran a Xbox One, beneficios de suscripción, etc. se agregan a los resultados. Cuando este valor es "false", solo los elementos primarios como ProductID de la agrupación se devuelven en los resultados y no incluye elementos individuales. **Nota:** Solo se admite el uso de este parámetro con valor "true" si el parámetro de tipo de elemento no se incluye en el URI, en caso contrario, recibirá un error HTTP 400. |  
  | productIds | string |  Una colección de ProductID que desea recuperar específicamente de inventario del usuario, separados por ','.  Si el usuario no tiene un ProductID proporcionado en los resultados de inventario, ese elemento no aparecerá en los resultados de la llamada de API. Si se pasa en el productID de un paquete junto con el parámetro expandSatisfyingEntitlements establecido en true, se devuelven todos los elementos incluidos en el paquete en los resultados de la llamada (si se ha especificado sus productIds en la cadena de consulta o no).   |
  | Estado | string | El estado de los elementos que se va a devolver. El valor predeterminado es "all", que devuelve todos los elementos. Otros valores son "habilitados", que indica que se habilitan ese itemsthat solo debe devolverse, "Suspendido", lo que indica que solo los elementos que se suspenden deben devolverse, "Expirada", que indica que se deben devolver solo los elementos que han expirado, " Cancelar", lo que indica que se deben devolver solo los elementos que se cancelan y"Renovación", lo que indica que se deben devolver solo los elementos que se han renovado.  |

Además, el recurso es compatible con la mecánica de paginación estándar.

<a id="ID4EDE"></a>


## <a name="sample-request"></a>Solicitud de ejemplo

Es el nombre de dominio completo para este URI (método) `https://inventory.xboxlive.com/users/me/inventory.
         `

> [!NOTE] 
> Los usuarios que se consideran depende el token proporcionado, que puede incluir varios usuarios. Si desea que el inventario de un usuario único, también debe proporcionar el valor de hash de usuario para el usuario específico que desee considerar exclusivamente.

.

<a id="ID4ERE"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Si la llamada se realiza correctamente, el servicio devuelve una matriz de elementos del inventario. Consulte [inventoryItem (JSON)](../../json/json-inventoryitem.md).

<a id="ID4E4E"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
  "pagingInfo": {
    "continuationToken": string,
    "totalItems": int
  },
  "items":
  {
    "url": string,
    "itemType": "Music",
    "titleId": string,
    "containers": string,
    "obtained": DateTime,
    "startDate": DateTime,
    "endDate": DateTime,
    "state": "Enabled"  
}

```


<a id="ID4EHF"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EJF"></a>


##### <a name="parent"></a>Primario

[/users/me/inventory](uri-inventory.md)


<a id="ID4ETF"></a>


##### <a name="further-information"></a>Para obtener más información

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)
