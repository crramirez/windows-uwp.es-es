---
title: /users/xuid({xuid})/groups/{moniker}/broadcasting/count
assetID: 535c8d46-7001-c31e-3e9d-82ad275095ae
permalink: en-us/docs/xboxlive/rest/uri-usersxuidgroupsmonikerbroadcastingcount.html
description: " /users/xuid({xuid})/groups/{moniker}/broadcasting/count"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7a39bc9c3302ba26949700774997355a216fe70d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651360"
---
# <a name="usersxuidxuidgroupsmonikerbroadcastingcount"></a>/users/xuid({xuid})/groups/{moniker}/broadcasting/count
Accesos el recuento de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI. Es el dominio para estos URI `userpresence.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID) del usuario relacionada con el XUIDs en el grupo.| 
| moniker| string| Cadena que define el grupo de usuarios. El moniker solo aceptado en la actualidad es "People", con una letra mayúscula "P".| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/groups/{moniker}/broadcasting/count )](uri-usersxuidgroupsmonikerbroadcastingcountget.md)

&nbsp;&nbsp;Recupera el recuento de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI.
 
<a id="ID4EHC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EJC"></a>

 
##### <a name="parent"></a>Primario 

[URI de presencia](atoc-reference-presence.md)

   