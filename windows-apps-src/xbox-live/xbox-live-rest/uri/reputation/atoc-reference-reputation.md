---
title: URI de reputación
assetID: d1cb76c0-86a4-8c51-19f6-5f223b517d46
permalink: en-us/docs/xboxlive/rest/atoc-reference-reputation.html
description: " URI de reputación"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 93d6d6e6acfd8fa39bd9d26c87ed99362d2c88d6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614010"
---
# <a name="reputation-uris"></a>URI de reputación
 
Esta sección proporciona información detallada sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados desde servicios de Xbox Live para la **Microsoft.Xbox.Services.Social.ReputationService** . El dominio de la reputación del URI es reputation.xboxlive.com. Una representación de URI típica podría ser https://reputation.xboxlive.com/users/xuid(2533274790412952)/feedback. 
 
El servicio de reputación usa comentarios, como se describe en [comentarios (JSON)](../../json/json-feedback.md), para calcular una puntuación de reputación. Esta puntuación se guarda en el área de estadísticas para el usuario bajo la clave ReputationOverall. Para obtener más información acerca de cómo recuperar las estadísticas de usuario, consulte [obtener (/users/xuid({xuid})/scids/{scid}/stats)](../userstats/uri-usersxuidscidsscidstatsget.md). 
 
Juegos en todas las plataformas pueden usar el servicio de reputación.
 
<a id="ID4EMB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/users/xuid({xuid})/feedback](uri-reputationusersxuidfeedback.md)

&nbsp;&nbsp;Usar desde el título si así lo desea agregar una opción de comentarios en el juego, en lugar de usar el shell.

[/users/batchfeedback](uri-reputationusersbatchfeedback.md)

&nbsp;&nbsp;Servicio de su título usa para enviar comentarios con formato por lotes fuera de la interfaz de su título.

[/users/me/resetreputation](uri-usersmeresetreputation.md)

&nbsp;&nbsp;Permite al equipo de cumplimiento tener acceso a las puntuaciones de reputación del usuario actual.

[/users/xuid({xuid})/deleteuserdata](uri-usersxuiddeleteuserdata.md)

&nbsp;&nbsp;Restablece completamente los datos de reputación de un usuario de prueba. Solo para pruebas.

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

&nbsp;&nbsp;Permite al equipo de cumplimiento tener acceso a las puntuaciones de reputación del usuario especificado.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   