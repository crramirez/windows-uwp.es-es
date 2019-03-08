---
title: /users/{ownerId}/people/xuids
assetID: db2faec7-9f6c-f240-586a-45d6ed596e88
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuids.html
description: " /users/{ownerId}/people/xuids"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 27b7695ba163bf0ca832a96df030868e646e0abc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57626340"
---
# <a name="usersowneridpeoplexuids"></a>/users/{ownerId}/people/xuids
Tiene acceso a las personas por XUID de colección de personas de la persona que llama. Es el dominio para estos URI `social.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-usersowneridpeoplexuidspost.md)

&nbsp;&nbsp;Obtiene las personas por XUID de personas de la persona que llama colección.
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   