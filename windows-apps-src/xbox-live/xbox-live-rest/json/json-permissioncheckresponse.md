---
title: PermissionCheckResponse (JSON)
assetID: 7a749001-b569-b0e0-2a35-f299474c8710
permalink: en-us/docs/xboxlive/rest/json-permissioncheckresponse.html
description: " PermissionCheckResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7623a45fa21e30a015bd5c6a7c1f5add19cc189b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623370"
---
# <a name="permissioncheckresponse-json"></a>PermissionCheckResponse (JSON)
Los resultados de una comprobación de un único usuario en una configuración de permisos solo con un usuario de destino único. 
<a id="ID4EN"></a>

 
## <a name="permissioncheckresponse"></a>PermissionCheckResponse
 
El objeto PermissionCheckResponse tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| IsAllowed| Valor booleano| Obligatorio. Este miembro es <b>true</b> si el usuario solicitante puede realizar la acción solicitada con el usuario de destino.| 
| Results| Matriz de [PermissionCheckResult (JSON)](json-permissioncheckresult.md)| Opcional. Si <b>IsAllowed</b> fuese falsa y la comprobación se denegó por algo relacionado con el solicitante, indica por qué se denegó el permiso.| 
  
<a id="ID4E3B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "isAllowed": false,
    "reasons":
    [
        {"reason": "BlockedByRequestor"},
        {"reason": "MissingPrivilege", "restrictedSetting": "VideoCommunications"}
    ]
}
    
```

  
<a id="ID4EFC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EHC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   