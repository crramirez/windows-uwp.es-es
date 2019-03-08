---
title: /users/xuid({xuid})/groups/{moniker}
assetID: 7c73236b-95ee-723b-e5e0-68252c953e14
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmoniker.html
description: " /users/xuid({xuid})/groups/{moniker}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 25ddc8120f05f04d5285fbbe4efc5a41f98265ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617560"
---
# <a name="usersxuidxuidgroupsmoniker"></a>/users/xuid({xuid})/groups/{moniker}
Tiene acceso a la PresenceRecord para un grupo. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID) del usuario relacionada con el XUIDs en el grupo.| 
| moniker| string| Cadena que define el grupo de usuarios. El moniker solo aceptado en la actualidad es "People", con una letra mayúscula "P".| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker} )](uri-usersxuidgroupsmonikerget.md)

&nbsp;&nbsp;Obtiene el PresenceRecord para un grupo.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Primario 

[URI de presencia](atoc-reference-presence.md)

   