---
title: /users/{ownerId}/people
assetID: 9745a93c-720e-606d-bff2-ad0ec610ed98
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeople.html
description: " /users/{ownerId}/people"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c3980ca2d755d9fceb4b9059f8c5f529a7c16218
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598400"
---
# <a name="usersowneridpeople"></a>/users/{ownerId}/people
Tiene acceso a la colección de personas de la persona que llama. Es el dominio para estos URI `social.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-usersowneridpeopleget.md)

&nbsp;&nbsp;Obtiene colección de personas de la persona que llama.
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   