---
title: Persona (JSON)
assetID: b49234b1-03cd-f16e-c293-c74174382167
permalink: en-us/docs/xboxlive/rest/json-person.html
description: " Persona (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 175d66ffc7744ca8203fe7681fcb0167e150f012
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640870"
---
# <a name="person-json"></a>Persona (JSON)
Metadatos sobre una sola persona en el sistema de personas. 
<a id="ID4EN"></a>

 
## <a name="person"></a>Person
 
El objeto de persona tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Obligatorio. Id. de usuario de Xbox (XUID) en formato decimal. Valor de ejemplo: 2603643534573573.| 
| isFavorite| Valor booleano| Obligatorio. Si esta persona es aquella que interesan al usuario más. Dado que los usuarios pueden tener un gran número de personas en su lista de personas, seres queridos deben ser tenga prioridad en las experiencias y se muestra antes que otras que no son los favoritos.| 
| isFollowingCaller| Valor booleano| Opcional. Si esta persona está siguiendo el usuario en cuyo nombre se realizó la llamada API.| 
| socialNetworks| matriz de cadena| Opcional. Dentro de las redes externas al usuario y esta persona tienen una relación.| 
  
<a id="ID4EHC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
    
```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Referencia 

[/users/{ownerId}/people/{targetid}](../uri/people/uri-usersowneridpeopletargetid.md)

 [/users/{ownerId}/people/xuids](../uri/people/uri-usersowneridpeoplexuids.md)

   