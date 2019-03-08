---
title: MediaAsset (JSON)
assetID: 858c720a-1648-738b-bb43-f050e7cac19e
permalink: en-us/docs/xboxlive/rest/json-mediaasset.html
description: " MediaAsset (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 308a65b7c049a6aba0405865bab63fb9d28b8506
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623480"
---
# <a name="mediaasset-json"></a>MediaAsset (JSON)
Los recursos multimedia asociados con el cumplimiento o sus recompensas.
<a id="ID4EN"></a>


## <a name="mediaasset"></a>MediaAsset

El objeto MediaAsset tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| name| string| El nombre de la MediaAsset, por ejemplo, "tile01".|
| type| MediaAssetType enumeration| El tipo del recurso multimedia: <ul><li>icon (0): El icono de logro.</li><li>art (1): El recurso de imágenes digitales.</li></ul> | 
| url| string| La dirección URL de la MediaAsset.|

<a id="ID4EFC"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
  "name":"Icon Name",
  "type":"Icon",
  "url":"https://www.xbox.com"
}

```


<a id="ID4EOC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EQC"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)
