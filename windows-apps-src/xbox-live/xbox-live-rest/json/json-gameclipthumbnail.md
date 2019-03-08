---
title: Miniatura de GameClip (JSON)
assetID: 3ed87fc1-734c-d8b5-d908-0ae3359769ed
permalink: en-us/docs/xboxlive/rest/json-gameclipthumbnail.html
description: " Miniatura de GameClip (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ad2d35431cb4c40690978f4f3920f2e47f2b9bc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606570"
---
# <a name="gameclipthumbnail-json"></a>Miniatura de GameClip (JSON)
Contiene la información relacionada con una vista en miniatura individual. Puede haber varios tamaños de cada clip, y se deja el cliente para seleccionar el adecuado para su presentación. 
<a id="ID4EN"></a>

 
## <a name="gameclipthumbnail"></a>GameClipThumbnail
 
El objeto GameClipThumbnail tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| <b>uri</b>| string| El URI para la imagen en miniatura.| 
| <b>fileSize</b>| entero de 32 bits sin signo| El tamaño total del archivo de la imagen en miniatura.| 
| <b>thumbnailType</b>| ThumbnailType| El tipo de imagen en miniatura.| 
  
<a id="ID4EAC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
         "uri": "https://gameclips.xbox.com/thumbnails/7ce5c1a7-1255-46d3-a90e-34a0e2dfab06/small.jpg",
         "fileSize": 123,
         "width": 120,
         "height": 250
       }
    
```

  
<a id="ID4EJC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ELC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   