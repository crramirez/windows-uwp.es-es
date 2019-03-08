---
title: Información general sobre el tipo de datos
assetID: c154a6fa-e7b2-4652-f6fc-f946f74480e9
permalink: en-us/docs/xboxlive/rest/datatypeoverview.html
description: " Información general sobre el tipo de datos"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 62932a921d51a988a5533d7ee08f4968bb67a29d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57659660"
---
# <a name="data-type-overview"></a>Información general sobre el tipo de datos
 
Servicios de Xbox Live, se usa una variedad de tipos de datos relacionados con la autenticación e identidad. En este tema se proporciona información general de esos tipos.
 
| Tipo| Descripción| 
| --- | --- | 
| gamertag| Un nombre de pantalla de lenguaje natural, único para el usuario.| 
| Reproductor| Un objeto JSON que contiene el usuario XUID y nombre de jugador, además de índice del jugador en la sesión (o "puestos"), si el jugador todavía está participando en la sesión y un blob pequeño de datos personalizados.| 
| perfil| Información sobre el usuario tiene acceso a través de direcciones URI de perfil y los métodos HTTP, normalmente UserSettings del usuario, pero también posiblemente incluidos gamercard, nombre de jugador, XUID y así sucesivamente.| 
| Configuración de| Uno de la configuración específica del título de un objeto UserSettings.| 
| UserClaims| Un objeto JSON simple que contiene el usuario XUID y nombre de jugador.| 
| UserSettings| Un objeto JSON que contiene una colección de configuración específica del título o las preferencias del usuario autenticado actual. UserSettings puede contener datos arbitrarios, posiblemente relacionados con la actividad del juego.| 
| XUID| Identificador de usuario de Xbox del usuario, un único entero largo sin signo. No pretende ser legible.| 
 
<a id="ID4E6D"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBE"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ENE"></a>

 
##### <a name="reference--player-jsonjsonjson-playermd"></a>Referencia [Reproductor (JSON)](../json/json-player.md)

 [UserClaims (JSON)](../json/json-userclaims.md)

 [UserSettings (JSON)](../json/json-usersettings.md)

   