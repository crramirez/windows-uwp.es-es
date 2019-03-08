---
title: SessionEntry (JSON)
assetID: b5cf5c3d-83b8-635f-d1a5-0be5d9434ea5
permalink: en-us/docs/xboxlive/rest/json-sessionentry.html
description: " SessionEntry (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 73133f898ff219477cb60f54798cbd81acb87ebe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589740"
---
# <a name="sessionentry-json"></a>SessionEntry (JSON)
Contiene los datos de una sesión de idoneidad. 
<a id="ID4EN"></a>

 
## <a name="sessionentry"></a>SessionEntry
 
El objeto SessionEntry tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| durationInSeconds| entero de 32 bits con signo | Duración, en segundos, de la sesión. | 
| julios| entero de 32 bits con signo | Energía — en julios: grabado en la sesión. | 
| cumplen| número de punto flotante de precisión sencilla| Valor cumple medio a través de la duración de la sesión. El valor de trato es la proporción del metabolismo de una persona durante una actividad con respecto al metabolismo del individuo en reposo. Porque el metabolismo para colocar es 1.0 independientemente del peso de un individuo y son valores MÉ con respecto a un individuo descansando metabolismo, que pueden utilizarse para comparar la intensidad de una actividad que está realizarla las personas de distintas ponderaciones.| 
| serverTimestamp| DateTime| Tiempo, según la hora UTC, se ha introducido en el servidor. | 
| Código fuente| entero de 8 bits sin signo| Origen de la sesión.| 
| marca de tiempo| DateTime| Tiempo, basándose en la hora Universal coordinada (UTC): se creó la entrada en el cliente. | 
| titleId| entero sin signo de 64 bits| Título: en formato decimal, que crea la entrada.| 
  
<a id="ID4EFE"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "titleId" : "1234567",
   "timestamp" : "2011-11-18T08:08:46Z",
   "serverTimestamp" : "2011-11-20T04:04:23Z",
   "durationInSeconds" : 240,
   "joules" :  1600,
   "met" :  "124"
   "source" :  "1"
}
    
```

  
<a id="ID4EOE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQE"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   