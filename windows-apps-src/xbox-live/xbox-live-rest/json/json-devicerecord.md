---
title: DeviceRecord (JSON)
assetID: aca4f4d3-f9b4-8919-5b6d-5ae0fe11e162
permalink: en-us/docs/xboxlive/rest/json-devicerecord.html
description: " DeviceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9746706c00a09cd8b64913b4ae8b5c3426551e48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646150"
---
# <a name="devicerecord-json"></a>DeviceRecord (JSON)
Información acerca de un dispositivo, incluido su tipo y los títulos activos en él. 
<a id="ID4EN"></a>

 
## <a name="devicerecord"></a>DeviceRecord
 
El objeto DeviceRecord tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| type| string| El tipo de dispositivo del dispositivo. Las posibilidades incluyen "D", "Xbox360", "MoLIVE" (Windows), "Windows Phone", "WindowsPhone7" y "PC" (G4WL). Si el tipo es desconocido (por ejemplo iOS, Android o un título incrustado en un explorador web), se devuelve "Web".| 
| titles| matriz de [TitleRecord](json-titlerecord.md)| La lista de títulos activos en este dispositivo.| 
  
<a id="ID4EWB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
[{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  }]
    
```

  
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ENC"></a>

 
##### <a name="reference"></a>Referencia   