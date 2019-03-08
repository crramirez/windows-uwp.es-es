---
title: UserSettings (JSON)
assetID: 17c030cb-05e0-f78e-5ab1-cdbd8b801ceb
permalink: en-us/docs/xboxlive/rest/json-usersettings.html
description: " UserSettings (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5451c59ab608105677a657ade41154bd2b622f5e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655050"
---
# <a name="usersettings-json"></a>UserSettings (JSON)
Devuelve la configuración de usuario autenticado actual. 
<a id="ID4EN"></a>

 
## <a name="usersettings"></a>UserSettings
 
El objeto UserSettings tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| id| entero de 32 bits sin signo| El identificador de la configuración.| 
| Código fuente| entero de 32 bits sin signo| Representa el origen de la configuración. | 
| titleId| entero de 32 bits sin signo| El identificador del título asociado con la configuración. | 
| value| matriz de enteros de 8 bits sin signo| Representa el valor de la configuración. Recuperando configuración de clientes deben comprender el formato de representación para poder leer los datos. | 
  
<a id="ID4EJC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "id":268697600,
   "source":1,
   "titleId":1297287259,
   "value":"CAAAAA=="
}
    
```

  
<a id="ID4ESC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EUC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   