---
title: PresenceRecord (JSON)
assetID: 414e6ef5-f7bd-70d0-7386-7aa1c3a56e21
permalink: en-us/docs/xboxlive/rest/json-presencerecord.html
description: " PresenceRecord (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6d531352c4336e00c93a91e7c945602ab69695f2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622590"
---
# <a name="presencerecord-json"></a>PresenceRecord (JSON)
Datos sobre la presencia en línea de un solo usuario.
<a id="ID4EN"></a>


## <a name="presencerecord"></a>PresenceRecord

El objeto PresenceRecord tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| xuid| string| La consola Xbox usuario ID (XUID) del usuario de destino. Es la presencia de datos proporcionado para este usuario.|
| dispositivos| matriz de [DeviceRecord](json-devicerecord.md)| Lista de registros de dispositivo del usuario.|
| Estado| string| Actividad del usuario en Xbox LIVE. Posibles valores: <ul><li>en línea: Usuario tiene al menos un dispositivo.</li><li>Distancia: Usuario está conectado en Xbox LIVE, pero no está activo en cualquier título.</li><li>sin conexión: Usuario no está presente en cualquier dispositivo.</li></ul> | 
| lastSeen| [LastSeenRecord](json-lastseenrecord.md)| La última información encontrada solo está disponible cuando el usuario no tiene ningún DeviceRecords válido. Si el objeto se quitó de la memoria caché, los datos podrían no devolverse, porque no hay ningún almacén persistente.|

<a id="ID4E2C"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:"W8",
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page"
      }
    }]
  }]
}

```


<a id="ID4EED"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EGD"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EQD"></a>


##### <a name="reference"></a>Referencia

[POST (/users/batch)](../uri/presence/uri-usersbatchpost.md)

 [GET (/users/me)](../uri/presence/uri-usersmeget.md)

 [DELETE (/users/xuid({xuid})/devices/current/titles/current)](../uri/presence/uri-usersxuiddevicescurrenttitlescurrentdelete.md)

 [GET (/users/xuid({xuid}))](../uri/presence/uri-usersxuidget.md)
