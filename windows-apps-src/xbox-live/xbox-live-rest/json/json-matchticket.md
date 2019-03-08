---
title: MatchTicket (JSON)
assetID: 12617677-47f2-e517-af53-5ab9687eea2a
permalink: en-us/docs/xboxlive/rest/json-matchticket.html
description: " MatchTicket (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4bc638dfe7735856295ed92f35e244213be7bc1e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608340"
---
# <a name="matchticket-json"></a>MatchTicket (JSON)
Un objeto JSON que representa un vale de coincidencia, utilizado los reproductores para buscar otros jugadores a través del directorio de sesión de varios jugadores (MPSD). 
<a id="ID4EN"></a>

  
 
El objeto MatchTicket JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| serviceConfig| GUID| Identificador de configuración de servicio (¿SCID) para la sesión.| 
| hopperName| string| Nombre de la hopper en el que se debe colocar este vale.| 
| giveUpDuration| entero de 32 bits con signo| Tiempo de espera máximo (número entero de segundos).| 
| preserveSession| enumeración| Un valor que indica si se debe reutilizar la sesión como la sesión en la que buscar la coincidencia. Los valores posibles son "siempre" o "nunca". | 
| ticketSessionRef| MultiplayerSessionReference| <b>MultiplayerSessionReference</b> objeto para la sesión en el que el Reproductor o el grupo se está reproduciendo. Este miembro siempre es necesario. | 
| ticketAttributes| matriz de objetos| Colección de atributos proporcionada por el usuario y valores sobre los vales para los jugadores.| 
| jugadores| matriz de objetos| Colección de objetos de jugador, cada uno con un contenedor de propiedades de atributos proporcionada por el usuario. | 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
        "serviceConfig": "07617C5B-3423-4505-B6C6-10A16E1E5DDB",
        "hopperName": "TestHopper",
        "giveUpDuration": 10,
        "preserveSession": "never",
        "ticketSessionRef": {
        "scid": "AFFEABDF-0000-0000-0000-000000000001",
        "templateName": "TestTemplate",
        "sessionName": "5E551041-0000-0000-0000-000000000001"
    },
    "ticketAttributes": {
        "desiredMap": "Test Map",
        "desiredGameType": "Test GameType"
    },
    "players": [
        {
            "xuid": 123412345123,
            "playerAttributes": {
                "skill": 5,
                "ageRange": "teenager"
            }
        },
        {
          "xuid": 123412345124,
          "playerAttributes": {
              "skill": 15,
              "ageRange": "twenty-something"
          }
        }
      ]
    }
  
    
```

  
<a id="ID4EEB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EGB"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   