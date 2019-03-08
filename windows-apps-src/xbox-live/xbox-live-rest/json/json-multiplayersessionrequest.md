---
title: MultiplayerSessionRequest (JSON)
assetID: 2e311c6d-3427-5a39-1989-06dc08483057
permalink: en-us/docs/xboxlive/rest/json-multiplayersessionrequest.html
description: " MultiplayerSessionRequest (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 18929c060adeae47f0305422dd312e7410f93981
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628350"
---
# <a name="multiplayersessionrequest-json"></a>MultiplayerSessionRequest (JSON)
Pasa el objeto de solicitud JSON para una operación en un **MultiplayerSession** objeto. 
<a id="ID4EQ"></a>

  
 
El objeto MultiplayerSessionRequest JSON tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| Constantes| object| Configuración de solo lectura que se combina con la plantilla de sesión para generar las constantes para la sesión. | 
| propiedades | object | Cambios que se combinarán en las propiedades de sesión.| 
| members.me | object| Constantes y las propiedades que trabajan en forma muy como a sus homólogos de nivel superior. Cualquier método PUT requiere que el usuario sea miembro de la sesión y agrega el usuario si es necesario. Si "" se especifica como null, se quita el miembro que realiza la solicitud de la sesión. | 
| miembros | object| Otros objetos que representan a los usuarios agregar a la sesión, ordenados por un índice de base cero. El número de miembros en una solicitud siempre comienza con 0, incluso si la sesión ya contiene a miembros. Los miembros se agregan a la sesión en el orden en que aparecen en la solicitud. Las propiedades de miembro solo pueden establecerse por el usuario a los que pertenecen. | 
| servidores | object| Los valores que indican las actualizaciones y adiciones a la sesión del conjunto de participantes de servidor asociado. Si un servidor se especifica como null, se quita esa entrada del servidor de la sesión. | 
  
<a id="ID4EZ"></a>

 
## <a name="request-structure"></a>Estructura de la solicitud
 

```json
{
  "constants": { /* Property Bag */ },
  "properties": { /* Property Bag */ },
  "members": {
    // Requires a service principal. Existing members can be deleted by index.
    // Not available on large sessions.
    "5": null,

    // Reservation requests must start with zero. New users will get added in order to the end of the session's member list.
    // Large sessions don't support reservations.
    "reserve_0": {
      "constants": { /* Property Bag */ }
    },
    "reserve_1": {
      "constants": { /* Property Bag */ }
    },

    // Requires a user principal with a xuid claim. Can be 'null' to remove myself from the session.
    "me": {
      "constants": { /* Property Bag */ },
      "properties": { /* Property Bag */ },
    }
  },

  // Requires a server principal.
  "servers": {
    // Can be any name. The value can be 'null' to remove the server from the session.
    "name": {
      "constants": {  /* Property Bag */ },
      "properties": {  /* Property Bag */ }
    }
  }
}
```

  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4EMB"></a>

 
##### <a name="reference"></a>Referencia 

[MultiplayerSession (JSON)](json-multiplayersession.md)

 [PUT (/serviceconfigs/{scid}/sessiontemplates/{sessionTemplateName}/sessions/{sessionName})](../uri/sessiondirectory/uri-serviceconfigsscidsessiontemplatessessiontemplatenamesessionssessionnameput.md)

   