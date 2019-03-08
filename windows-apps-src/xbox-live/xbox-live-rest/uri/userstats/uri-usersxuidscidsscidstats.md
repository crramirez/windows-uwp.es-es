---
title: /users/xuid({xuid})/scids/{scid}/stats
assetID: 3cf9ffd4-9a8b-2658-402b-2e933f7f6f1b
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidsscidstats.html
description: " /users/xuid({xuid})/scids/{scid}/stats"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53a6c7bb0e7390b024b01e221d8061316a80509e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650420"
---
# <a name="usersxuidxuidscidsscidstats"></a>/users/xuid({xuid})/scids/{scid}/stats
Tiene acceso a una configuración de servicio con ámbito de una lista delimitada por comas de nombres de estadísticas de usuario en nombre del usuario especificado. Es el dominio para estos URI `userstats.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| GUID| Id. de usuario de Xbox (XUID) del usuario en cuyo nombre para tener acceso a la configuración del servicio.| 
| scid| GUID| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.| 
  
<a id="ID4E4B"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET](uri-usersxuidscidsscidstatsget.md)

&nbsp;&nbsp;Obtiene una configuración de servicio con ámbito de una lista delimitada por comas de nombres de estadísticas de usuario en nombre del usuario especificado.

[OBTENER con metadatos de valor](uri-usersxuidscidsscidstatsgetvaluemetadata.md)

&nbsp;&nbsp;Obtiene una lista de estadísticas especificado, incluidos los metadatos asociados con los valores de estadística para un usuario en una configuración de servicio especificado.
 
<a id="ID4EKC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EMC"></a>

 
##### <a name="parent"></a>Primario 

[URI de las estadísticas de usuario](atoc-reference-userstats.md)

   