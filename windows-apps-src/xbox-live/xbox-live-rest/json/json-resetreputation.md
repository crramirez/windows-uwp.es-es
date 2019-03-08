---
title: ResetReputation (JSON)
assetID: 15edb5e7-a00b-4188-9b49-9db5774c4a10
permalink: en-us/docs/xboxlive/rest/json-resetreputation.html
description: " ResetReputation (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d09c8bbc1130f91dfea3d4c35e391dcf9adcf127
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649420"
---
# <a name="resetreputation-json"></a>ResetReputation (JSON)
Contiene las puntuaciones de reputación bases nueva a la que se deben cambiar las puntuaciones de un usuario existente. 
<a id="ID4EN"></a>

 
## <a name="resetreputation"></a>ResetReputation
 
El objeto ResetReputation tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| fairplayReputation| número| Deseado nuevo base calificación de reputación de Fairplay para el usuario (intervalo válido de 0 a 75).| 
| commsReputation| número| Deseado nuevo base calificación de reputación de comunicaciones para el usuario (intervalo válido de 0 a 75).| 
| userContentReputation| número| Deseado nuevo base puntuación UserContent reputación del usuario (intervalo válido de 0 a 75).| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   