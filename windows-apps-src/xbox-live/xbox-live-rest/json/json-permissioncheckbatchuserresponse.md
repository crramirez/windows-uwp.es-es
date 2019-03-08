---
title: PermissionCheckBatchUserResponse (JSON)
assetID: c587dbc1-9436-4d55-afcb-deb47e3c2664
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchuserresponse.html
description: " PermissionCheckBatchUserResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c9e20cc195ad737a7e847a8ad41b76247220adfe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628360"
---
# <a name="permissioncheckbatchuserresponse-json"></a>PermissionCheckBatchUserResponse (JSON)
Comprobación los motivos de un permiso de lote para la lista de valores de permiso para un usuario de destino único. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckbatchuserresponse"></a>PermissionCheckBatchUserResponse
 
El objeto PermissionCheckBatchUserResponse tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| Usuario| string| Obligatorio. Este miembro es <b>true</b> si el usuario solicitante puede realizar la acción solicitada con el usuario de destino.| 
| Permisos| Matriz de [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md)| Obligatorio. Un [PermissionCheckResponse (JSON)](json-permissioncheckresponse.md) para cada permiso que se solicitó en la solicitud original, en el mismo orden que en la solicitud.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "User": {"Xuid": "12345"},
    "Permissions":
    [
        {
            "isAllowed": true
        },
        {
            "isAllowed": false
        },
        {
            "isAllowed": false,
            "reasons":
            [
                {"reason": "BlockedByRequestor"},
                {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
            ]
        }
    ]
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   