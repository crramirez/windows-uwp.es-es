---
title: PermissionCheckResult (JSON)
assetID: 1cf147fa-4ff1-3299-0822-0fc1726d1600
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresult.html
description: " PermissionCheckResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dc13826be1b6f81201d069f5ade7ea5ba6668cd0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598450"
---
# <a name="permissioncheckresult-json"></a>PermissionCheckResult (JSON)
Los resultados de una comprobación de un único usuario en una configuración de permisos solo con un usuario de destino único. 
<a id="ID4EP"></a>

 
## <a name="permissioncheckresult"></a>PermissionCheckResult
 
El objeto PermissionCheckResult tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| reason| string| Opcional. Un <b>PermissionResultCode</b> valor que indica por qué se denegó el permiso si <b>IsAllowed</b> era false.| 
| restrictedSetting| string| Opcional. Si el <b>PermissionResultCode</b> valor en el <b>motivo</b> miembro indica que una comprobación de privilegios para el solicitante no se pudo, esto indica qué privilegios no se pudo.| 
  
<a id="ID4E6B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "reason": "MissingPrivilege",
    "restrictedSetting": "VideoCommunications"
}
    
```

  
<a id="ID4EIC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EKC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   