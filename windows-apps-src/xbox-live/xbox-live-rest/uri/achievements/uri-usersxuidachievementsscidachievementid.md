---
title: /users/xuid({xuid})/achievements/{scid}/{achievementid}
assetID: 656a6d63-1a11-b0a5-63d2-2b010abd62e7
permalink: en-us/docs/xboxlive/rest/uri-usersxuidachievementsscidachievementid.html
description: " /users/xuid({xuid})/achievements/{scid}/{achievementid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 00c577f60b67f15f75c47b5e737ca12819695110
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599450"
---
# <a name="usersxuidxuidachievementsscidachievementid"></a>/users/xuid({xuid})/achievements/{scid}/{achievementid}
Devuelve información sobre la realización, incluidos sus metadatos configurado y datos específicos del usuario. 

> [!NOTE] 
> Solo se admite para la plataforma. 

 
Es el dominio para estos URI `achievements.xboxlive.com`.
 
  * [Parámetros de URI](#ID4E2)
 
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el XUID del usuario autenticado.| 
| scid| GUID| Identificador único de la configuración del servicio cuyo cumplimiento se tiene acceso.| 
| achievementid| entero de 32 bits sin signo| Identificador único (dentro de la ¿SCID especificado) del logro que está accediendo.| 
  
<a id="ID4EMC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/achievements/{scid}/{achievementid})](uri-usersxuidachievementsscidachievementidget.md)

&nbsp;&nbsp;Obtiene los detalles de la realización.
 
<a id="ID4EWC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EYC"></a>

 
##### <a name="parent"></a>Primario 

[URI de logros](atoc-reference-achievementsv2.md)

   