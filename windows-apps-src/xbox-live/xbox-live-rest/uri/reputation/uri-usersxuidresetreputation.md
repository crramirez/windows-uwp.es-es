---
title: /users/xuid({xuid})/resetreputation
assetID: 85c74beb-a12f-4015-e244-36942e366afc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidresetreputation.html
description: " /users/xuid({xuid})/resetreputation"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c6cd2f28833cdc86fb3fd01bb85890dcb0654901
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607530"
---
# <a name="usersxuidxuidresetreputation"></a>/users/xuid({xuid})/resetreputation
Permite al equipo de cumplimiento tener acceso a las puntuaciones de reputación del usuario especificado. Es el número de puerto y el dominio para estos URI `reputation.xboxlive.com:10433`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| La consola Xbox usuario ID (XUID) del usuario especificado.| 
  
<a id="ID4EVB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST (/users/xuid({xuid})/resetreputation)](uri-usersxuidresetreputationpost.md)

&nbsp;&nbsp;Permite al equipo de cumplimiento establecer puntuaciones de reputación del usuario especificado en algunos valores arbitrarios después un secuestro de cuenta (por ejemplo).
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Primario 

[URI de reputación](atoc-reference-reputation.md)

   