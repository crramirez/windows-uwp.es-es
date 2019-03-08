---
title: LastSeenRecord (JSON)
assetID: 6a93202c-801c-03c6-8386-6acd0f366780
permalink: en-us/docs/xboxlive/rest/json-lastseenrecord.html
description: " LastSeenRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e06de31cabaedb68ed57d3d4f2ff30614ceb6317
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613380"
---
# <a name="lastseenrecord-json"></a>LastSeenRecord (JSON)
Información acerca de cuándo el sistema vio por última vez un usuario, está disponible cuando el usuario no tiene ningún DeviceRecord válido. 
<a id="ID4EN"></a>

 
## <a name="lastseenrecord"></a>LastSeenRecord
 
El objeto LastSeenRecord tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| deviceType| string| El tipo del dispositivo en el que el usuario estaba presente último.| 
| titleId| entero de 32 bits sin signo| El identificador del título en el que el usuario estaba presente último.| 
| titleName| string| El nombre del título en el que el usuario estaba presente último.| 
| marca de tiempo| DateTime| Marca de tiempo UTC que indica cuando el usuario estaba presente último.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
  deviceType:W8,    
  titleId:"23452345",
  titleName:"My Awesome Game",
  timestamp:"2012-09-17T07:15:23.4930000"
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E5C"></a>

 
##### <a name="reference"></a>Referencia   