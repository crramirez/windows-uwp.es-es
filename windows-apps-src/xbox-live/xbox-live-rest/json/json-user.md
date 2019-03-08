---
title: Usuario (JSON)
assetID: dbc733e4-0348-0e3d-1f55-17b465e599d6
permalink: en-us/docs/xboxlive/rest/json-user.html
description: " Usuario (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5c1b3a34ef329696d51e615dd79d57783a132d05
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641360"
---
# <a name="user-json"></a>Usuario (JSON)
Contiene los datos de la tabla de clasificación de usuario. 
<a id="ID4EN"></a>

 
## <a name="user"></a>Usuario
 
El objeto de usuario tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| gamertag| string| Nombre de jugador del Reproductor (máximo de 15 caracteres). El cliente debería utilizar este valor en la interfaz de usuario para identificar el Reproductor.| 
| rango| entero de 32 bits con signo| El rango del usuario en relación con el usuario que solicita los datos de marcador.| 
| rating| string| La valoración del usuario.| 
| xuid| entero sin signo de 64 bits| La consola Xbox usuario ID (XUID) del usuario.| 
  
<a id="ID4EMC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{ 
   "gamertag":"TrueBlue402",
   "rank":2,
   "rating":"2:19:21.17",
   "xuid":1234567890123456 
}
    
```

  
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   