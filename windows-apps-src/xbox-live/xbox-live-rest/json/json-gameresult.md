---
title: GameResult (JSON)
assetID: 43d863c0-2179-ae46-5d4a-2f08cd44b667
permalink: en-us/docs/xboxlive/rest/json-gameresult.html
description: " GameResult (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b408b1aaae5e6f54958a016575c4a2c37765f1e9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57602340"
---
# <a name="gameresult-json"></a>GameResult (JSON)
Un objeto JSON que representa los datos que se describen los resultados de una sesión de juego. 
<a id="ID4EN"></a>

  
 
El objeto GameResult JSON tiene los siguientes miembros.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| blob| matriz de enteros de 8 bits sin signo| Datos de resultado específico del título personalizado.| 
| Resultado| string| El resultado de la participación del jugador en la sesión de juego. Los valores válidos son "Win", "Pérdida" o "Enlazar". | 
| puntuación| entero con signo de 64 bits| La puntuación que recibió el Reproductor en la sesión de juego.| 
| time| entero con signo de 64 bits| Hora del Reproductor para la sesión de juego.| 
| xuid| entero sin signo de 64 bits| El identificador de usuario de Xbox del Reproductor a los que se aplican los resultados.| 
  
<a id="ID4EPC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "xuid": 2533274790412952,
   "outcome": "Win",
   "score": 100
}
    
```

  
<a id="ID4EYC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E1C"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   