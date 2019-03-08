---
title: /users/{ownerId}/people/{targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
description: " /users/{ownerId}/people/{targetid}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 6238996abdeaca9b7a9a7a20d3f1ae9702e95a73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661750"
---
# <a name="usersowneridpeopletargetid"></a>/users/{ownerId}/people/{targetid}
Tiene acceso a una persona por Id. de destino de colección de personas de la persona que llama. Es el dominio para estos URI `social.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).| 
| targetID| string| Identificador del usuario cuyos datos se recuperan de la lista de personas del propietario, un Id. de usuario de Xbox (XUID) o un nombre de jugador. Valores de ejemplo: xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;Obtiene a una persona por Id. de destino de colección de personas de la persona que llama.
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   