---
title: quotaInfo (JSON)
assetID: 3147ab78-e671-e142-0a3a-688dc4079451
permalink: en-us/docs/xboxlive/rest/json-quota.html
description: " quotaInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f3499fdba972d6e953813fc490d080910921698e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654120"
---
# <a name="quotainfo-json"></a>quotaInfo (JSON)
Contiene información de cuota de un grupo de título. 
<a id="ID4EN"></a>

 
## <a name="quotainfo"></a>quotaInfo
 
El objeto quotaInfo tiene las siguientes especificaciones.
 
Para el almacenamiento global
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| quotaBytes| entero de 32 bits con signo | Número máximo de bytes que puede usar el título.| 
| usedBytes| entero de 32 bits con signo | Número de bytes utilizados por el título.| 
  
<a id="ID4EXB"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 
El ejemplo siguiente muestra la respuesta de almacenamiento global:
 

```json
{
    "quotaInfo":
    {
        "usedBytes":4194304,
        "quotaBytes":536870912
    }
}
      
```

  
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   