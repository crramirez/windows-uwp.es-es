---
title: URI de DVR de juego
assetID: 472f705e-bf28-7894-b1ba-80933d8746a6
permalink: en-us/docs/xboxlive/rest/atoc-reference-dvr.html
description: " URI de DVR de juego"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b4bfd6e51efce4c6ec85db99a10a44a776dcb840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617850"
---
# <a name="game-dvr-uris"></a>URI de DVR de juego
 
Esta sección proporcionan detalles sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados de servicios de Xbox Live para *game DVR*.
 
Consolas sólo pueden grabar un mensaje de juegos, pero cualquier dispositivo que puede tener acceso a puede mostrar un clip.
 
Dependiendo de la función del identificador URI en cuestión, los dominios para estos URI son:
 
   *  gameclipsmetadata.xboxlive.com 
   *  gameclipstransfer.xboxlive.com 
  
<a id="ID4EZB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/public/scids/{scid}/clips](uri-publicscidclips.md)

&nbsp;&nbsp;Clips de acceso públicos. Este URI puede especificarse realmente de dos formas: `/public/scids/{scid}/clips` y `/public/titles/{titleId}/clips`. Consulta más adelante para más información.

[/{uri}](uri-uri.md)

&nbsp;&nbsp;Acceder a los datos de clip de juego.

[/users/me/scids/{scid}/clips](uri-usersmescidclips.md)

&nbsp;&nbsp;Solicitud de carga de acceso inicial.

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

&nbsp;&nbsp;Acceder a los datos de clip de juego y los metadatos.

[/users/{ownerId}/clips](uri-usersowneridclips.md)

&nbsp;&nbsp;Lista de acceso de los clips del usuario.

[/users/{ownerId}/scids/{scid}/clips/{gameClipId}](uri-usersowneridscidclipsgameclipid.md)

&nbsp;&nbsp;Tener acceso a un único juego clip desde el sistema si se conocen todos los identificadores para buscarlo.
 
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   