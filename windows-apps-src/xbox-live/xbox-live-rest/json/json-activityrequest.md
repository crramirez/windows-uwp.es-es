---
title: ActivityRequest (JSON)
assetID: 9eb03ab7-352c-4465-ec86-d544e76f63f9
permalink: en-us/docs/xboxlive/rest/json-activityrequest.html
description: " ActivityRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 791a566d278b92aeb34ab36d38719b44e9cc6c8f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628160"
---
# <a name="activityrequest-json"></a>ActivityRequest (JSON)
Una solicitud para obtener información acerca de la presencia enriquecida de uno o varios usuarios. 
<a id="ID4EN"></a>

 
## <a name="activityrequest"></a>ActivityRequest
 
El objeto ActivityRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| richPresence| [RichPresenceRequest](json-richpresencerequest.md)| El nombre descriptivo de la cadena de presencia enriquecida que se debe usar.| 
| Media| MediaRequest| Información multimedia para lo que el usuario está viendo o escucha.| 
  
<a id="ID4EVB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    richPresence:
    {
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f"
    }
  }
    
```

  
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   