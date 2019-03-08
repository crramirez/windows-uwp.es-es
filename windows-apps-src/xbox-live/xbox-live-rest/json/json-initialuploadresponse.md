---
title: InitialUploadResponse (JSON)
assetID: 6abb7d37-2c35-2cc3-d9e5-eff695235262
permalink: en-us/docs/xboxlive/rest/json-initialuploadresponse.html
description: " InitialUploadResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: dab59fefb389cf550a1bc4fc6429f6b0970f50ab
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589840"
---
# <a name="initialuploadresponse-json"></a>InitialUploadResponse (JSON)
 
<a id="ID4EO"></a>

 
## <a name="initialuploadresponse"></a>InitialUploadResponse
 
El objeto InitialUploadResponse tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| <b>gameClipId</b>| string| Id. asignado para la solicitud de datos de carga.| 
| <b>uploadUri</b>| URI| Ubicación a la que se debe cargar el clip de juego.| 
| <b>largeThumbnailUri</b>| URI| Opcional. Ubicación a la que se debe cargar la vista en miniatura grande. La presencia de este campo viene determinada por la [ThumbnailSource enumeración](../enums/gvr-enum-thumbnailsource.md) valor en el <b>InitialUploadRequest</b> (estará presente cuando se especifica la carga).| 
| <b>smallThumbnailUri</b>| URI| Opcional. Ubicación a la que se debe cargar la miniatura pequeña. La presencia de este campo viene determinada por la [ThumbnailSource enumeración](../enums/gvr-enum-thumbnailsource.md) valor en el <b>InitialUploadRequest</b> (estará presente cuando se especifica la carga).| 
  
<a id="ID4EYC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "gameClipId": "6b364924-5650-480f-86a7-fc002a1ee752"  ,  
   "uploadUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container",
   "largeThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/large",
   "smallThumbnailUri": "https://gameclips.xbox.live/upload/xuid(2716903703773872)/6b364924-5650-480f-86a7-fc002a1ee752/container/thumbnails/small"
 }
    
```

  
<a id="ID4EBD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EDD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   