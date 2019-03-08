---
title: MultiplayerSessionReference (JSON)
assetID: 6e03e060-8c9b-b394-415f-af7e85be569f
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionreference.html
description: " MultiplayerSessionReference (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5986079e1cae3338d8cc24a9e85f6941cf4fbec4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651250"
---
# <a name="multiplayersessionreference-json"></a>MultiplayerSessionReference (JSON)
Un objeto JSON que representa el **MultiplayerSessionReference**. 
<a id="ID4EQ"></a>

  
 
El objeto MultiplayerSessionReference JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| GUID| Identificador de configuración de servicio (¿SCID). Parte 1 de identificador de sesión.| 
| templateName | string | Nombre de la instancia actual de la plantilla de sesión. Parte 2 del identificador de sesión. | 
| name | string | Nombre de la sesión. Parte 3 del identificador de sesión. | 
  
<a id="ID4EZ"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo 
 

```json
{
  "scid": "8d050174-412b-4d51-a29b-d55a34edfdb7",
  "templateName": "integration",
  "name": "19de0095d8bb41048f19edbbb6bc6b04"
}
  
    
```

  
<a id="ID4EJB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ELB"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EVB"></a>

 
##### <a name="reference"></a>Referencia 

[MultiplayerSession (JSON)](json-multiplayersession.md)

   