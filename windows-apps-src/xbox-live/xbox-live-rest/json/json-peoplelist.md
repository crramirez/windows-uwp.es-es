---
title: PeopleList (JSON)
assetID: ac538652-c10c-44e5-c1e3-5314ebe8ba83
permalink: en-us/docs/xboxlive/rest/json-peoplelist.html
description: " PeopleList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f9ab412088707752d62cc20fd54da2639f26ddc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613330"
---
# <a name="peoplelist-json"></a>PeopleList (JSON)
Colección de [persona](json-person.md) objetos. 
<a id="ID4ER"></a>

 
## <a name="peoplelist"></a>PeopleList
 
El objeto PeopleList tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| Contactos| matriz de [persona](json-person.md)| El [persona](json-person.md) objetos que componen la lista de personas.| 
| totalCount| entero de 32 bits sin signo| Número total de [persona](json-person.md) disponible en el conjunto de objetos. Este valor puede usarse por los clientes para la paginación, ya que representa el tamaño de todo el conjunto, no solo la respuesta más reciente. Valor de ejemplo: 680.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVC"></a>

 
##### <a name="reference"></a>Referencia 

[GET (/users/{ownerId}/people)](../uri/people/uri-usersowneridpeopleget.md)

 [POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   