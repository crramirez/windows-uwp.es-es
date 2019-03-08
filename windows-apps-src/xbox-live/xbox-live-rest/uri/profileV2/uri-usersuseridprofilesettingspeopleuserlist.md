---
title: /users/{userId}/profile/settings/people/{userList}?settings={settings}
assetID: 0ba20eba-f0ab-28ab-61d3-b4f9e4c07bc5
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlist.html
description: " /users/{userId}/profile/settings/people/{userList}?settings={settings}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 24b58c817156a7c372a8e6acfab895e6b7c51207
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636970"
---
# <a name="usersuseridprofilesettingspeopleuserlistsettingssettings"></a>/users/{userId}/profile/settings/people/{userList}?settings={settings}
Acceso al perfil para un usuario o usuarios, con compatibilidad con el Moniker de personas. Es el dominio para estos URI `profile.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| userId| string| ¿Puedo ser 'xuid(12345)', 'gt(myGamertag)' o 'me'.| 
| userList| string| Lista de personas para obtener la configuración con nombre. Actualmente, las personas es la única lista compatible.| 
  
<a id="ID4E1B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{userId}/profile/settings/people/{userList})](uri-usersuseridprofilesettingspeopleuserlistget.md)

&nbsp;&nbsp;Obtener el perfil de un usuario o usuarios, con el Moniker de personas compatibilidad.
 
<a id="ID4EEC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EGC"></a>

 
##### <a name="parent"></a>Primario 

[URI de perfiles](atoc-reference-profiles.md)

 [Profile (JSON)](../../json/json-profile.md)

   