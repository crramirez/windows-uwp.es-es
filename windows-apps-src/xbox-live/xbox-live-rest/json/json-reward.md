---
title: Recompensa (JSON)
assetID: d1c92e8a-afbc-22c5-c0b5-6063963f8c4d
permalink: en-us/docs/xboxlive/rest/json-reward.html
description: " Recompensa (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1d58d34263e7e0e90091c41c1df4fd5e078f5055
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607500"
---
# <a name="reward-json"></a>Recompensa (JSON)
La recompensa asociada con la realización.
<a id="ID4EN"></a>


## <a name="reward"></a>Recompensa

El objeto de recompensa tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| name| string| El nombre de usuario de la recompensa.|
| description| string| La descripción de cara al usuario de la recompensa.|
| value| string| Valor de la recompensa.|
| type| Enumeración RewardType| El tipo de recompensa: <ul><li>no es válido (0): Se ha configurado un tipo de recompensa desconocido y no admitidos.</li><li>Puntuación (1): La recompensa agrega puntos a la puntuación del jugador.</li><li>inApp (2): La recompensa se define y entregan el título.</li><li>Art (3): La recompensa es un activo digital.</li></ul> | 
| valueType| Enumeración ProgressValueDataType| El tipo de valor. Consulte [requisito (JSON)](json-requirement.md) para obtener más información.|

<a id="ID4EBD"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
  "name":null,
  "description":null,
  "value":"10",
  "type":"Gamerscore",
  "valueType":"Int"
}

```


<a id="ID4EKD"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EMD"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)
