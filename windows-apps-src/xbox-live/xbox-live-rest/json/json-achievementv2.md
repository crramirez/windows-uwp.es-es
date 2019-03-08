---
title: Logro (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Logro (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628480"
---
# <a name="achievement-json"></a>Logro (JSON)
Un objeto de logro (versión 2).
<a id="ID4EN"></a>


## <a name="achievement"></a>Logro

El objeto de logro tiene la especificación siguiente. Se requieren todos los miembros.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| id| string| Identificador de recursos.|
| serviceConfigId| string| ¿SCID para este recurso. Identifica los títulos que está relacionado con este logro. |
| name| string| El nombre localizado de logro.|
| titleAssociations| matriz de [TitleAssociation](json-titleassociation.md)| Una matriz de TitleAssociation.|
| progressState| **ProgressState** (enumeración)| El estado de progresión: <ul><li>no es válido (0): La progresión de logro está en un estado desconocido.</li><li>consigue (1): El logro ha desbloqueado.</li><li>inProgress (2): Se bloquea la consecución pero el usuario realizó progreso para desbloquearla.</li><li>notStarted (3): El logro está bloqueado y el usuario no ha realizado ningún progreso para desbloquearla.</li></ul> | 
| Progresión| [Progresión](json-progression.md)| Progresión del usuario dentro de la realización.|
| mediaAssets| matriz de [MediaAsset](json-mediaasset.md)| Los recursos multimedia asociados con la realización, como los identificadores de imagen. |
| Plataforma| string| Se ha obtenido la mención especial de la plataforma en.|
| isSecret| Valor booleano| Si es o no la consecución secreta.|
| description| string| La descripción de la realización cuando desbloquea.|
| lockedDescription| string| La descripción de la realización antes de que está desbloqueada.|
| productId| string| Se lanzó la consecución de ProductId con.|
| achievementType| **AchievementType** (enumeración)| El tipo de logro (no es el mismo como el tipo anterior en logros heredadas): <ul><li>no es válido (0): Un tipo de logro desconocido y no admitidos.</li><li>persistente (1): Un logro que no tiene ninguna fecha de finalización y se puede desbloquear en cualquier momento.</li><li>desafío de (2): Una mención especial de que tiene un período de tiempo específico durante el cual puede ser desbloqueado.</li></ul> |
| participationType| **ParticipationType** (enumeración)| El tipo de participación de la realización. Los valores válidos son el individuo o grupo.|
| timeWindow| TimeWindow| El período de tiempo durante el cual puede ser desbloqueado el logro. Solo se admite para los desafíos.|
| recompensas| matriz de [recompensa](json-reward.md)| La colección de recompensas acumulado cuando desbloquea.|
| estimatedTime| TimeSpan| El tiempo estimado que tardará la consecución ganar.|
| vínculo profundo| string| Un vínculo profundo en el título.|
| isRevoked| Valor booleano| Si se revoca la consecución por aplicación.|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)
