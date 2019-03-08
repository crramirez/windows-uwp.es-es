---
title: MultiplayerActivityDetails (JSON)
assetID: f982aa5e-2694-4ef9-bc55-6c099a3cf9ec
permalink: en-us/docs/xboxlive/rest/json-multiplayeractivitydetails.html
description: " MultiplayerActivityDetails (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 188bcebb8d6bff879f30dcc83d7039fbcbfae0b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658110"
---
# <a name="multiplayeractivitydetails-json"></a>MultiplayerActivityDetails (JSON)
Un objeto JSON que representa el **Microsoft.Xbox.Services.Multiplayer.MultiplayerActivityDetails**. 

> [!NOTE] 
> Este objeto es implementado por el multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Se está diseñado para su uso con el contrato de la plantilla 104/105 o posterior.  

 
<a id="ID4ES"></a>

  
 
El objeto MultiplayerActivityDetails JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| SessionReference| MultiplayerSessionReference| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference</b> objeto que representa la información de identificación de la sesión.| 
| HandleId| entero sin signo de 64 bits| El Id. del identificador correspondiente a la actividad.| 
| TitleId| entero de 32 bits sin signo| El identificador del título que se debe iniciar con el fin de unirse a la actividad.| 
| Visibilidad| MultiplayerSessionVisibility| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionVisibility</b> valor que indica el estado de visibilidad de la sesión.| 
| JoinRestriction| MultiplayerSessionJoinRestriction| Un <b>Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionJoinRestriction</b> valor que indica la restricción de combinación para la sesión. Esta restricción se aplica si el campo de visibilidad se establece en "open".| 
| Cerrado| Valor booleano| True si la sesión se ha cerrado temporalmente para unir y false en caso contrario.| 
| OwnerXboxUserId| entero sin signo de 64 bits| Xbox Id. de usuario del miembro que es propietario de la actividad.| 
| MaxMembersCount| entero de 32 bits sin signo| Número de ranuras en total.| 
| MembersCount| entero de 32 bits sin signo| Número de ranuras de ocupado.| 
  
<a id="ID4E3D"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
  "results": [{
    "id": "11111111-ebe0-42da-885f-033860a818f6",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "party",
      "name": "e3a836aeac6f4cbe9bcab985494d3175"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": true,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  },
  {
    "id": "11111111-ebe0-42da-885f-033860a818f7",
    "type": "activity",
    "version": 1,
    "sessionRef": {
      "scid": "8dfb0100-ebe0-42da-885f-033860a818f6",
      "templateName": "TitleStorageTestDefault",
      "name": "795fcaa7-8377-4281-bd7e-e86c12843632"
    },
    "titleId": "1234567",
    "ownerXuid": "3212",

    // Only if ?include=relatedInfo
    "relatedInfo": {
      "visibility": "open",
      "joinRestriction": "followed",
      "closed": false,
      "maxMembersCount": 8,
      "membersCount": 4,
    }
  }]
}
    
```

  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   