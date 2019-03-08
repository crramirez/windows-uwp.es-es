---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632580"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
Tiene acceso a un marcador social (clasificación).
Es el dominio para estos URI `leaderboards.xboxlive.com`.

  * [Parámetros de URI](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| string| Identificador del usuario.|
| scid| string| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.|
| statname| string| Identificador único del recurso stat usuario que se obtiene acceso.|
| todos los\|favorito| enumeración| Si se debe clasificar la stat valores (puntuaciones) para todos los contactos conocidos del usuario actual o sólo los contactos designados como personas favoritas por ese usuario.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Métodos válidos

[GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;Devuelve un marcador social mediante la clasificación de que los Estados de valores (puntuaciones) para todos los contactos conocidos del usuario actual o sólo los contactos designados como personas favoritas por ese usuario.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E1C"></a>


##### <a name="parent"></a>Primario

[URI de marcadores](atoc-reference-leaderboard.md)
