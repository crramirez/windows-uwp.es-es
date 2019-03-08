---
title: Progresión (JSON)
assetID: cdff6415-f12b-0a45-61f2-26dbf47b1b56
permalink: en-us/docs/xboxlive/rest/json-progression.html
description: " Progresión (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dda124e5be9a4d21a1ee5b9d6130290207e31921
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593830"
---
# <a name="progression-json"></a>Progresión (JSON)
Progresión del usuario a desbloquear el logro. 
<a id="ID4EN"></a>

 
## <a name="progression"></a>Progresión
 
El objeto de progresión tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| requisitos| matriz de requisito| Los requisitos para obtener el logro y cuánto ha avanzado el usuario es hacia desbloquearla.| 
| timeUnlocked| DateTime| El tiempo que el logro en primer lugar se ha desbloqueado.| 
  
<a id="ID4ETB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
  "requirements":
  [{
    "id":"12345678-1234-1234-1234-123456789012",
    "current":null,
    "target":"100"
  }],
  "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
}
    
```

  
<a id="ID4E3B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E5B"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   