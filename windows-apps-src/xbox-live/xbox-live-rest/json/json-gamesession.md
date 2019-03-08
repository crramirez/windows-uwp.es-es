---
title: GameSession (JSON)
assetID: 325af9ae-a9a9-5b36-8342-1aff5aff01d1
permalink: en-us/docs/xboxlive/rest/json-gamesession.html
description: " GameSession (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ca7276ccdc13d896d19873811b4fa9df9a831cd1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646520"
---
# <a name="gamesession-json"></a>GameSession (JSON)
Un objeto JSON que representa los datos del juego para una sesión de varios jugadores. 
<a id="ID4ER"></a>

  
 
El objeto GameSession JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| creationTime| DateTime| La fecha y hora cuando se creó la sesión, en UTC. | 
| customData| matriz de enteros de 8 bits sin signo| 1024 bytes de datos de la sesión de juegos específicos. Este valor es opaco para el servidor. | 
| displayName| string| La sesión en el quid de la presentación, con una longitud máxima de 128 caracteres. Este valor es opaco para el servidor. | 
| hasEnded| Valor booleano| True si la sesión ha finalizado y false en caso contrario. Establecer este campo en true se marca la sesión de juego como de solo lectura, impide que más datos que se envían a la sesión. | 
| isClosed| Valor booleano| True si se cierra la sesión y no hay más jugadores pueden ser agregadas y false en caso contrario. Si este valor es true, se rechazarán las solicitudes para unirse a la sesión. | 
| maxPlayers| entero de 32 bits con signo| Número máximo de reproductores que puede estar en la sesión al mismo tiempo. El intervalo de valores es 2 y 16. Una vez que la sesión contiene el número máximo de jugadores, más se rechazan las solicitudes para unirse a la sesión. | 
| playersCanBeRemovedBy| PlayerAcl| Un valor que indica el jugador que se puede quitar otros jugadores de la sesión. Los valores posibles son NoOne, Self y AnyPlayer. | 
| lista.| matriz de objetos del Reproductor| Una matriz de los jugadores en la sesión. La lista contiene los participantes actuales y los jugadores que estaban previamente en la sesión, pero han dejado. El orden de los jugadores en la lista no cambia nunca. Nuevos jugadores que se agregan al final de la matriz. | 
| seatsAvailable| entero de 32 bits con signo| El número de jugadores que todavía puede unirse a la sesión antes de alcanza el número máximo de los jugadores. Este valor es de solo lectura y siempre es menor que el valor del campo maxPlayers. | 
| sessionId| string| El identificador de sesión asignado por el MPSD cuando se crea la sesión. Este valor normalmente se incluye en el URI al acceder a la información almacenada en una sesión.| 
| titleId| entero de 32 bits sin signo| El identificador del título de la creación de la sesión de juego.| 
| Variant| entero de 32 bits con signo| La variante de juego. Este valor es opaco para el servidor.| 
| visibility| VisibilityLevel| Un valor que indica la visibilidad de la sesión. Los valores posibles son: PlayersCurrentlyInSession, PlayersEverInSession y todo el mundo.| 
  
<a id="ID4EEF"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "maxPlayers": 4,
    "seatsAvailable": 4,
    "isClosed": false,
    "hasEnded": false,
    "roster": [],
    "visibility": "PlayersCurrentlyInSession",
    "playersCanBeRemovedBy": "Self",
 }
    
```

  
<a id="ID4ENF"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EPF"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EZF"></a>

 
##### <a name="reference"></a>Referencia 

[GameMessage (JSON)](json-gamemessage.md)

 [GameSessionSummary (JSON)](json-gamesessionsummary.md)

 [Player (JSON)](json-player.md)

   