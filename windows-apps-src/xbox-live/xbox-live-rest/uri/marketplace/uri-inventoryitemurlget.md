---
title: GET (/inventory/ {itemID})
assetID: d3ca14a5-0214-ef42-091e-3f05f2a3482d
permalink: en-us/docs/xboxlive/rest/uri-inventoryitemurlget.html
description: " GET (/inventory/ {itemID})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 446197eb20820304088ddac4a6379fa3b2510873
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606700"
---
# <a name="get-inventoryitemid"></a>GET (/inventory/ {itemID})
Proporciona el conjunto completo de detalles para un elemento de inventario concreto. Es el dominio para estos URI `inventory.xboxlive.com`.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EAB)
  * [Cuerpo de respuesta](#ID4ELB)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
No hay ninguna directiva comprueba, cumplimiento, o el filtrado se producirá como parte de esta llamada.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| itemID| string| el identificador único para cada usuario para un elemento de inventario singular| 
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4ERB"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 
La respuesta a una solicitud GET, suponiendo que pasa la autenticación y se asigna el contexto de autorización adecuado, es un elemento de inventario único con el conjunto completo de propiedades de elementos.
 

```cpp
{inventoryItem}
         
```

   
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Primario 

[GET (/inventory/{itemID})](uri-inventoryget.md)

  
<a id="ID4EJC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   