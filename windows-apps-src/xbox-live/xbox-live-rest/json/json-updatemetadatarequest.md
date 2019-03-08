---
title: UpdateMetadataRequest (JSON)
assetID: 0bc210e3-c1dc-9267-e322-aadb9f0a074a
permalink: en-us/docs/xboxlive/rest/json-updatemetadatarequest.html
description: " UpdateMetadataRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a76e4b12e0ffadb112913775b500ac0d39d413d5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597770"
---
# <a name="updatemetadatarequest-json"></a>UpdateMetadataRequest (JSON)
Los metadatos que se deben actualizar de un clip. 
<a id="ID4EN"></a>

 
## <a name="updatemetadatarequest"></a>UpdateMetadataRequest
 
El objeto UpdateMetadataRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| userCaption| string| Cambia la cadena no localizada del usuario ha especificado para el clip de juego.| 
| visibility| [Enumeración GameClipVisibility](../enums/gvr-enum-gameclipvisibility.md)| Cambia la visibilidad del clip juegos según se publican en el sistema.| 
| titleData| string| La bolsa de propiedades de título específico. Tamaño máximo: 10 KB.| 
  
<a id="ID4EBC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 
Cambiar el nombre de usuario Clip y visibilidad:
 

```json
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Cambiar solo las propiedades de título (Esto es solo un ejemplo, puesto que el esquema de este campo es hasta el llamador):
 

```json
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EQC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ESC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E3C"></a>

 
##### <a name="reference"></a>Referencia 

[POST (/users/me/scids/{scid}/clips/{gameClipId})](../uri/dvr/uri-usersmescidclipsgameclipidpost.md)

   