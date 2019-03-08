---
title: /users/xuid({xuid})/inbox
assetID: 352740c6-42e2-0000-495d-bf384dc3e941
permalink: en-us/docs/xboxlive/rest/uri-usersxuidinbox.html
description: " /users/xuid({xuid})/inbox"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8ded70b32dfd291d17a43a1741b26710f681a397
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57640900"
---
# <a name="usersxuidxuidinbox"></a>/users/xuid({xuid})/inbox
Proporciona acceso a un usuario de mensajería de bandeja de entrada para los servicios de Xbox LIVE. Es el dominio para estos URI `msg.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid | entero de 64 bits sin signo | La consola Xbox usuario ID (XUID) del Reproductor que realiza la solicitud. | 
| messageId | string[50] | Identificador del mensaje que se va a recuperar o eliminar. | 
  
<a id="ID4EDC"></a>

 
## <a name="valid-methods"></a>Métodos válidos 

[GET (/users/xuid({xuid})/inbox)](uri-usersxuidinboxget.md)

&nbsp;&nbsp;Recupera un número especificado de los resúmenes de mensaje de usuario desde el servicio. 

[DELETE (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageiddelete.md)

&nbsp;&nbsp;Elimina un mensaje de usuario en la Bandeja de entrada.

[GET (/users/xuid({xuid})/inbox/{messageId})](uri-usersxuidinboxmessageidget.md)

&nbsp;&nbsp;Recupera el texto del mensaje detallado para un mensaje de usuario en particular, marcándolo como de lectura en el servicio. 
 
<a id="ID4EVC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EXC"></a>

 
##### <a name="parent"></a>Primario  

[URI de los usuarios](atoc-reference-users.md)

   