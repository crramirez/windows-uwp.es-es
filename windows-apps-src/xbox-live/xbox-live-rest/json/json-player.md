---
title: Reproductor (JSON)
assetID: eaf6d082-869b-d2d3-d548-5cef65e54541
permalink: en-us/docs/xboxlive/rest/json-player.html
description: " Reproductor (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a5967cbfecd47c5675926bd45939442c45dda7b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622500"
---
# <a name="player-json"></a>Reproductor (JSON)
Contiene los datos de un reproductor en una sesión de juego. 
<a id="ID4EN"></a>

 
## <a name="player"></a>Reproductor
 
El objeto Player tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| customData| matriz de enteros de 8 bits sin signo| 1024 bytes de Base64 habían codificado de datos de los jugadores de juegos específicos. Este valor es opaco para el servidor.| 
| gamertag| string| Nombre de jugador, un máximo de 15 caracteres, del Reproductor. El cliente debería utilizar este valor en la interfaz de usuario para identificar el Reproductor. | 
| isCurrentlyInSession| Valor booleano| Indica si el Reproductor está actualmente en la sesión o abandonado la sesión.| 
| seatIndex| entero de 32 bits con signo| El índice del Reproductor en la sesión.| 
| xuid| entero sin signo de 64 bits| La consola Xbox usuario ID (XUID) del Reproductor.| 
  
<a id="ID4E3C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "xuid": 2533274790412952,
    "gamertag":"MyTestUser",
    "seatindex": 3
    "customData":"AIHJ2?iE?/jiKE!l5S=T..."
    "isCurrentlyInSession":"true"
}
    
```

  
<a id="ID4EFD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EHD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference"></a>Referencia 

[GameSession (JSON)](json-gamesession.md)

   