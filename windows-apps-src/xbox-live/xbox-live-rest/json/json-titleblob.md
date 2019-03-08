---
title: TitleBlob (JSON)
assetID: fd1c904d-e8d0-f61f-e403-40b25bd4ac14
permalink: en-us/docs/xboxlive/rest/json-titleblob.html
description: " TitleBlob (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 51a0b17a46d1c71ffdf9098d4637ca59d840c90a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612590"
---
# <a name="titleblob-json"></a>TitleBlob (JSON)
Contiene información sobre un título de almacenamiento. 
<a id="ID4EP"></a>

 
## <a name="titleblob"></a>TitleBlob
 
El objeto TitleBlob tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| clientFileTime| DateTime| [opcional] Fecha y hora de la última carga del archivo.| 
| displayName| string| [opcional] Nombre del archivo que se muestra al usuario.| 
| etag| string| Etiqueta para el archivo usado en descargar y cargar las solicitudes.| 
| fileName| string| Nombre del archivo.| 
| size| entero con signo de 64 bits| Tamaño del archivo en bytes.| 
| smartBlobType| string| [opcional] Tipo de datos. Los valores posibles son: config, json, binario.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "fileName":"foo\bar\blob.txt,binary",
    "clientFileTime":"2012-01-01T01:02:03.1234567Z",
    "displayName":"Friendly Name",
    "size":12,
    "etag":"0x8CEB3E4F8F3A5BF",
    "smartBlobType":"binary"
}
      
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   