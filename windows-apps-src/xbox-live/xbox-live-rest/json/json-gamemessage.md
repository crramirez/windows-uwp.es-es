---
title: GameMessage (JSON)
assetID: c11606e6-701f-5807-4aef-5608c98ad831
permalink: en-us/docs/xboxlive/rest/json-gamemessage.html
description: " GameMessage (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a2bddd9e26b4716fd1e33c4b5bbde56672b5d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651300"
---
# <a name="gamemessage-json"></a>GameMessage (JSON)
Un objeto JSON que define los datos para un mensaje en cola de mensajes de la sesión de juego. 
<a id="ID4EN"></a>

  
 
El objeto GameMessage JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| datos| matriz de enteros de 8 bits sin signo| Los datos codificados en Base64 que el cliente de juego quiere enviar a los clientes de juegos. Este valor es opaco para el servidor. | 
| senderXuid| entero sin signo de 64 bits| El identificador de usuario de Xbox del Reproductor envía el mensaje. | 
| sequenceNumber| entero de 32 bits con signo| El número de secuencia del mensaje de juego. Este valor se asigna el servidor. Se garantiza que los números de secuencia aumenten de forma uniforme, pero podría no ser consecutivos. Números de secuencia son únicos dentro de una cola de mensajes, pero no entre las colas de mensajes. | 
| queueIndex| entero de 32 bits con signo| El índice de la cola de mensajes de sesión para el mensaje. Los valores posibles son 0-3.| 
| timeStamp| DateTime| Tiempo de cuando se creó el mensaje de juegos en la cola por el servidor, en formato UTC. | 
  
<a id="ID4ERC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "queueIndex": 0,
    "sequenceNumber": 5,
    "senderXuid": 65536,
    "timestamp": "2011-06-23T18:49:50Z",
    "data": null
}
    
```

  
<a id="ID4E1C"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E3C"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EGD"></a>

 
##### <a name="reference"></a>Referencia 

[GameSession (JSON)](json-gamesession.md)

   