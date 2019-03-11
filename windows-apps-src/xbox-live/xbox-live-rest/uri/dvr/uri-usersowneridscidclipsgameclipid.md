---
title: /users/{ownerId}/scids/{scid}/clips/{gameClipId}
assetID: 49b68418-71f1-c5a2-3a9b-869fd1fa663c
permalink: en-us/docs/xboxlive/rest/uri-usersowneridscidclipsgameclipid.html
description: " /users/{ownerId}/scids/{scid}/clips/{gameClipId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7ea92e89d54df17e8d82084d840a7ee9ef7d032
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658340"
---
# <a name="usersowneridscidsscidclipsgameclipid"></a>/users/{ownerId}/scids/{scid}/clips/{gameClipId}
Tener acceso a un único juego clip desde el sistema si se conocen todos los identificadores para buscarlo. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Parámetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identidad de usuario del usuario cuyo recurso se está obteniendo acceso. Formatos admitidos: "me" o "xuid(123456789)". Longitud máxima: 16.| 
| scid| string| Id. de configuración de servicio del recurso al que se tiene acceso. Debe coincidir con el ¿SCID del usuario autenticado.| 
| gameClipId| string| Clip de juego el identificador del recurso que está accediendo.| 
  
<a id="ID4EFC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/users/{ownerId}/scids/{scid}/clips/{gameClipId})](uri-usersowneridscidclipsgameclipidget.md)

&nbsp;&nbsp;Obtener un único juego clip del sistema si se conocen todos los identificadores para buscarlo.
 
<a id="ID4EPC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERC"></a>

 
##### <a name="parent"></a>Primario 

[URI Game DVR](atoc-reference-dvr.md)

   