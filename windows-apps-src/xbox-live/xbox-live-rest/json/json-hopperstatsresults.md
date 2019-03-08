---
title: HopperStatsResults (JSON)
assetID: 91927da1-2e97-f7bc-ae62-7e0e9966b98e
permalink: en-us/docs/xboxlive/rest/json-hopperstatsresults.html
description: " HopperStatsResults (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 38e345fc20e92cdf6446c6ae1100e347fe634eff
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646210"
---
# <a name="hopperstatsresults-json"></a>HopperStatsResults (JSON)
Un objeto JSON que representa las estadísticas para un hopper. 
<a id="ID4EN"></a>

  
 
El objeto HopperStatsResults JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| hopperName| string| El nombre de la hopper seleccionado.| 
| waitTime| entero de 32 bits con signo| Promedio de tiempo para el hopper (un número entero de segundos) de coincidencia. | 
| Rellenado| entero de 32 bits con signo| El número de personas esperando coincidencias en el hopper.| 
  
<a id="ID4EW"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo 
 

```json
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }
  
    
```

  
<a id="ID4EGB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIB"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUB"></a>

 
##### <a name="reference"></a>Referencia 

[GET (/serviceconfigs/{scid}/hoppers/{name}/stats)](../uri/matchtickets/uri-serviceconfigsscidhoppershoppernamestatsget.md)

   