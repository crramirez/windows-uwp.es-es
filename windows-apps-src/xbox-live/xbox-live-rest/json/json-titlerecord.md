---
title: TitleRecord (JSON)
assetID: 8e1bd699-e408-67c8-31da-2d968adfbc21
permalink: en-us/docs/xboxlive/rest/json-titlerecord.html
description: " TitleRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89baf7e9a737428d492246f1647a561a4a8170cf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603990"
---
# <a name="titlerecord-json"></a>TitleRecord (JSON)
Información sobre un título, incluido su nombre y una marca de tiempo de última modificación. 
<a id="ID4EN"></a>

 
## <a name="titlerecord"></a>TitleRecord
 
Un TitleRecord debe contener un DeviceRecord o un LastSeenRecord, pero no puede contener ambas.
 
El objeto TitleRecord tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| id| entero de 32 bits sin signo| TitleId del registro.| 
| name| string| Nombre localizado del título.| 
| actividad| [ActivityRecord](json-activityrecord.md)| La actividad del usuario en el título. Solo se devuelve si la profundidad es "all".| 
| lastModified| DateTime| Marca de tiempo UTC cuando se actualizó por última vez el registro.| 
| selección de ubicación| string| La ubicación de la aplicación dentro de la interfaz de usuario. Las posibilidades incluyen "fill", "full", "acopladas" o "background". El valor predeterminado es "full" para los dispositivos sin la capacidad para colocar las aplicaciones.| 
| Estado| string| El estado del título. Puede ser "activo" o "inactivo" (predeterminado). El título establece el estado en función de sus propios criterios de actividad e inactividad.| 
  
<a id="ID4E6C"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    }
    
```

  
<a id="ID4EID"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EKD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EUD"></a>

 
##### <a name="reference"></a>Referencia 

[POST (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentpost.md)

   