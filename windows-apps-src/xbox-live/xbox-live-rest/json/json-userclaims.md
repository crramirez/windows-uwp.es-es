---
title: UserClaims (JSON)
assetID: f88d5ee0-2875-fcfb-3098-3cd6afce8748
permalink: en-us/docs/xboxlive/rest/json-userclaims.html
description: " UserClaims (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 21b4322d002747145c3b09e0f3cd7eb03874380b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623350"
---
# <a name="userclaims-json"></a>UserClaims (JSON)
Devuelve información sobre el usuario autenticado actual. 
<a id="ID4EN"></a>

 
## <a name="userclaims"></a>UserClaims
 
El objeto de UserClaims tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| gamertag| string| nombre de jugador del usuario.| 
| xuid| entero sin signo de 64 bits| La consola Xbox usuario ID (XUID) del usuario.| 
  
<a id="ID4EZB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "xuid" : 2533274790412952,
   "gamertag" : "gamer"

}
    
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   