---
title: PermissionCheckBatchRequest (JSON)
assetID: 3100b17c-1b60-fdf2-3af9-7e424f1903ee
permalink: en-us/docs/xboxlive/rest/json-permissioncheckbatchrequest.html
description: " PermissionCheckBatchRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 538547c85648970ab3e9fe3ae413e8a03df814ad
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623510"
---
# <a name="permissioncheckbatchrequest-json"></a>PermissionCheckBatchRequest (JSON)
Colección de objetos PermissionCheckBatchRequest. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckbatchrequest"></a>PermissionCheckBatchRequest
 
El objeto PermissionCheckBatchRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| Usuarios| Matriz de los usuarios| Obligatorio. Matriz de destinos que se va a comprobar el permiso contra. Cada entrada de esta matriz es un Id. de usuario de Xbox (XUID) o un usuario anónimo de fuera de la red para escenarios entre redes ("UsuariosAnónimos": "allUsers"). | 
| Permisos| Matriz de [PermissionId (enumeración)](../enums/privacy-enum-permissionid.md)| Obligatorio. Los permisos para comprobar el valor de cada usuario.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "users":
    [
        {"xuid":"12345"},
        {"xuid":"54321"}
    ],
    "permissions":
    [
        "ShareFriendList",
        "ShareGameHistory"
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   