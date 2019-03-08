---
title: URI de presencia
assetID: 4ba44d9c-8615-cacc-2eee-7ff5e7c74383
permalink: en-us/docs/xboxlive/rest/atoc-reference-presence.html
description: " URI de presencia"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1a46ecd48c2b0bf523ab234a5f20cf9ed6669e75
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632610"
---
# <a name="presence-uris"></a>URI de presencia
 
Esta sección proporcionan detalles sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para *presencia*.
 
Sólo los juegos que se ejecutan en una consola Xbox 360, en un dispositivo Windows Phone o en Windows pueden usar este servicio.
 
El dominio para estos URI es userpresence.xboxlive.com.
 
Puede suscribirse a los cambios de presencia de un usuario con el servicio de la actividad en tiempo Real (ATR).
 
<a id="ID4ERB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/users/batch](uri-usersbatch.md)

&nbsp;&nbsp;Presencia de acceso para un lote de usuarios.

[/users/me](uri-usersme.md)

&nbsp;&nbsp;Obtener acceso a la presencia del usuario actual.

[/users/me/groups/{moniker}](uri-usersmegroupsmoniker.md)

&nbsp;&nbsp;Tiene acceso a la PresenceRecord para mi grupo.

[/users/xuid({xuid})](uri-usersxuid.md)

&nbsp;&nbsp;Obtener acceso a la presencia de otro usuario o cliente.

[/users/xuid({xuid})/devices/current/titles/current](uri-usersxuiddevicescurrenttitlescurrent.md)

&nbsp;&nbsp;Obtener acceso a la presencia de un título o usuario de un título.

[/users/xuid({xuid})/groups/{moniker}](uri-usersxuidgroupsmoniker.md)

&nbsp;&nbsp;Tiene acceso a la PresenceRecord para un grupo.

[/users/xuid({xuid})/groups/{moniker}/broadcasting](uri-usersxuidgroupsmonikerbroadcasting.md)

&nbsp;&nbsp;Accesos el registro de la presencia de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI.

[/users/xuid({xuid})/groups/{moniker}/broadcasting/count](uri-usersxuidgroupsmonikerbroadcastingcount.md)

&nbsp;&nbsp;Accesos el recuento de los usuarios de difusión especificados por el moniker de grupos relacionados con el XUID que aparece en el URI.
 
<a id="ID4EMC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EOC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   