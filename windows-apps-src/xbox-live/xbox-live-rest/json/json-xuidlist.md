---
title: XuidList (JSON)
assetID: 06938a52-e582-a15b-ec7f-4b053dfc28ad
permalink: en-us/docs/xboxlive/rest/json-xuidlist.html
description: " XuidList (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d8172063d40f8df77827ab845c4dfd0c0799811
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627920"
---
# <a name="xuidlist-json"></a>XuidList (JSON)
Lista de XUIDs en el que se va a realizar una operación. 
<a id="ID4EN"></a>

 
## <a name="xuidlist"></a>XuidList
 
El objeto XuidList tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| xuids| matriz de cadena| Lista de valores de Id. de usuario de Xbox (XUID) en el que debe realizarse una operación o se deben devolver los datos.| 
  
<a id="ID4EMB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
    
```

  
<a id="ID4EVB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EXB"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EBC"></a>

 
##### <a name="reference"></a>Referencia 

[POST (/users/{ownerId}/people/xuids)](../uri/people/uri-usersowneridpeoplexuidspost.md)

   