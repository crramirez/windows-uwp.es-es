---
title: GameSessionSummary (JSON)
assetID: 50cf91ba-29d3-1260-7643-bcb3f8d74fc0
permalink: en-us/docs/xboxlive/rest/json-gamesessionsummary.html
description: " GameSessionSummary (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8dace19404ae7c8b1d1ef296a21c874e4dd14c6f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613390"
---
# <a name="gamesessionsummary-json"></a>GameSessionSummary (JSON)
Un objeto JSON que representa los datos de resumen para una sesión de juego. 
<a id="ID4EN"></a>

  
 
El objeto GameSessionSummary JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| creationTime| DateTime| La fecha y hora cuando se creó la sesión, en UTC. | 
| customData| matriz de enteros de 8 bits sin signo| 1024 bytes de datos de la sesión de juegos específicos. Este valor es opaco para el servidor. | 
| displayName| string| La sesión en el quid de la presentación, con una longitud máxima de 128 caracteres. Este valor es opaco para el servidor. | 
| hasEnded| Valor booleano| True si la sesión ha finalizado y false en caso contrario. Establecer este campo en true se marca la sesión de juego como de solo lectura, impide que más datos que se envían a la sesión. | 
| sessionId| cadena del identificador de sesión. | 
| titleId| entero de 32 bits sin signo| El identificador del título de la creación de la sesión de juego.| 
| Variant| entero de 32 bits con signo| La variante de juego. Este valor es opaco para el servidor.| 
  
<a id="ID4EID"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
    "sessionId": "702e5aaf-e7bd-4a7c-abea-9dd4be10edec",
    "titleId": 1297287259,
    "variant": 1,
    "displayName": "Contoso Cards",
    "creationTime": "2011-06-23T17:13:06Z",
    "customData": null,
    "hasEnded": false,
}
    
```

  
<a id="ID4ERD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ETD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Referencia 

[GameSession (JSON)](json-gamesession.md)

   