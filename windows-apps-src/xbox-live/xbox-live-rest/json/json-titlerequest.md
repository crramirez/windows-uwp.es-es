---
title: TitleRequest (JSON)
assetID: 43aeb6f9-726d-9260-e2ba-f005ea688bf1
permalink: en-us/docs/xboxlive/rest/json-titlerequest.html
description: " TitleRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a90f42c2f830ba6f04f77a1acaba067a2746a062
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593800"
---
# <a name="titlerequest-json"></a>TitleRequest (JSON)
Solicitud para obtener información sobre un título. 
<a id="ID4EN"></a>

 
## <a name="titlerequest"></a>TitleRequest
 
El objeto TitleRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| id| entero de 32 bits sin signo| Identificador del título.| 
| actividad| [ActivityRequest](json-activityrequest.md)| Obtener información, incluida la información de presencia y medios enriquecida, si está disponible en el título.| 
| Estado| string| Si un usuario está activo o no. Este campo es necesario para marcar un usuario como inactiva. El valor predeterminado es "activo".| 
| selección de ubicación| string| El modo de colocación del título. Valores posibles incluyen "completo", "rellenar", "acopladas," o "background". El valor predeterminado es "full".| 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
  id:"12341234",
  placement:"snapped",
  state:"active",
  activity:
  {
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"82b11353-08ba-48ca-9f1a-21627b189b0f"
    }
  }
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Referencia 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   