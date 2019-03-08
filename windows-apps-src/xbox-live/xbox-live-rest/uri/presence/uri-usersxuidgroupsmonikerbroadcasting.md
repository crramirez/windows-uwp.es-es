---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting
assetID: cf8319f6-46a2-b263-ea4c-f1ce403b571b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcasting.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 98eaa60204e3c98eb1b09a13372f7b0c084a6608
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651470"
---
# <a name="usersxuidxuidgroupsmonikerbroadcasting"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting
Accesos el registro de la presencia de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID) del usuario relacionada con el XUIDs en el grupo.| 
| moniker| string| Cadena que define el grupo de usuarios. El moniker solo aceptado en la actualidad es "People", con una letra mayúscula "P".| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting )](uri-usersxuidgroupsmonikerbroadcastingget.md)

&nbsp;&nbsp;Recupera el registro de la presencia de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Primario 

[URI de presencia](atoc-reference-presence.md)

   