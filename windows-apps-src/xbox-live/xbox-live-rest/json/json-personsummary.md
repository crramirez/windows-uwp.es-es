---
title: PersonSummary (JSON)
assetID: 22fedb5f-5602-98d8-04a6-786fe3905921
permalink: en-us/docs/xboxlive/rest/json-personsummary.html
description: " PersonSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a787992507405a70185140e879be731d72806eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612630"
---
# <a name="personsummary-json"></a>PersonSummary (JSON)
Colección de [persona (JSON)](json-person.md) objetos. 
<a id="ID4ER"></a>

 
## <a name="personsummary"></a>PersonSummary
 
El objeto PersonSummary tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| hasCallerMarkedTargetAsFavorite| Valor booleano| Si el llamador ha marcado el destino como favorito. Valores de ejemplo: true| 
| hasCallerMarkedTargetAsKnown| Valor booleano| Si el llamador ha marcado el destino como conocidas. Valores de ejemplo: true| 
| isCallerFollowingTarget| Valor booleano| Si el llamador está siguiendo el destino. Valores de ejemplo: true| 
| isTargetFollowingCaller| Valor booleano| Si el destino está siguiendo el llamador. Valores de ejemplo: true| 
| legacyFriendStatus| string| Estado heredado amigo del destino, tal como se muestra por el llamador. Puede ser "", "MutuallyAccepted", "OutgoingRequest" o "IncomingRequest". Valores de ejemplo: "MutuallyAccepted"| 
| recentChangeCount| entero de 32 bits sin signo| Opcional. Número de cambios recientes en el grafo social del destino. Este valor solo está presente cuando un usuario está viendo su propio resumen. Valores de ejemplo: 5| 
| targetFollowerCount| > entero de 32 bits sin signo| Número de personas que siguen el destino. Valores de ejemplo: 1308| 
| targetFollowingCount| entero de 32 bits sin signo| Número de personas que sigue el destino. Valores de ejemplo: 112| 
| Marca de agua| string| Opcional. Marca de agua de cambio de reciente para el destino. Este valor solo está presente cuando un usuario está viendo su propio resumen. Valores de ejemplo: 5| 
  
<a id="ID4E4D"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "targetFollowingCount": 87,
    "targetFollowerCount": 19,
    "isCallerFollowingTarget": true,
    "isTargetFollowingCaller": false,
    "hasCallerMarkedTargetAsFavorite": true,
    "hasCallerMarkedTargetAsKnown": true,
    "legacyFriendStatus": "None",
    "recentChangeCount": 5,
    "watermark": "5246775845000319351"
}
    
```

  
<a id="ID4EGE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIE"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   