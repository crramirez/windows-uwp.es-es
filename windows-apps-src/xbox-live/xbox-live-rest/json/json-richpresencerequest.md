---
title: RichPresenceRequest (JSON)
assetID: 599266be-f747-0be1-fadf-f8e0262dc27f
permalink: en-us/docs/xboxlive/rest/json-richpresencerequest.html
description: " RichPresenceRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 4c49da63ecd091a886a68f508af09e33fb9c58ac
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57654130"
---
# <a name="richpresencerequest-json"></a>RichPresenceRequest (JSON)
Solicitud para obtener información acerca de qué se debe usar la información de presencia enriquecida. 
<a id="ID4EN"></a>

 
## <a name="richpresencerequest"></a>RichPresenceRequest
 
El objeto RichPresenceRequest tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| id| string| El <b>friendlyName</b> de la cadena de presencia enriquecida que se usará.| 
| scid| string| ¿Scid que nos indica donde se definen las cadenas de presencia enriquecida.| 
| params| matriz de cadena| Matriz de <b>friendlyName</b> cadenas con el que se va a finalizar la cadena de presencia enriquecida. Deben especificarse solo los nombres de enumeración descriptivos, no las estadísticas. Si deja este vacío, se quitará cualquier valor anterior.| 
  
<a id="ID4EDC"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
      id:"playingMapWeapon",
      scid:"abba0123-08ba-48ca-9f1a-21627b189b0f",
    }
    
```

  
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   