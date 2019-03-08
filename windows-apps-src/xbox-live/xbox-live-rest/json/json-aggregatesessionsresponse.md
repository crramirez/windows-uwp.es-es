---
title: AggregateSessionsResponse (JSON)
assetID: 020ee9b2-c96c-2e65-4e6d-f9f4bd25a374
permalink: en-us/docs/xboxlive/rest/json-aggregatesessionsresponse.html
description: " AggregateSessionsResponse (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ef026fd5096d047b2014faaf95a667c69827e043
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608310"
---
# <a name="aggregatesessionsresponse-json"></a>AggregateSessionsResponse (JSON)
Contiene los datos agregados para las sesiones de idoneidad de un usuario. 
<a id="ID4EN"></a>

 
## <a name="aggregatesessionsresponse"></a>AggregateSessionsResponse
 
El objeto AggregateSessionsResponse tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| totalDurationInSeconds| entero con signo de 64 bits| Duración total de sesiones en segundos durante el período de agregación.| 
| totalJoules| entero con signo de 64 bits| Total de energía quemada — en julios, durante el período de agregación. | 
| totalSessions| entero con signo de 64 bits| Número total de sesiones durante el período de agregación.| 
| weightedAverageMets| número de punto flotante de precisión sencilla | Equivalente a metabólica Media ponderada del valor de la tarea (MÉ) durante el período de agregación. El valor de trato es la proporción del metabolismo de una persona durante una actividad con respecto al metabolismo del individuo en reposo. Porque el metabolismo para colocar es 1.0 independientemente del peso de un individuo y son valores MÉ con respecto a un individuo descansando metabolismo, que pueden utilizarse para comparar la intensidad de una actividad que está realizarla las personas de distintas ponderaciones.| 
  
<a id="ID4ESC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "totalSessions" : 300,
   "totalDurationInSeconds" : 1240,
   "totalJoules" :  21600,
   "weightedAvgMet" : 21,
}

    
```

  
<a id="ID4E2C"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E4C"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   