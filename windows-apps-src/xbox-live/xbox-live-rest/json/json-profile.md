---
title: Perfil (JSON)
assetID: b92b1750-c2df-39b6-6c5c-f9e8068c8097
permalink: en-us/docs/xboxlive/rest/json-profile.html
description: " Perfil (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7299fcb4d375a3fc35ad67306b70f5fa4afde963
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607520"
---
# <a name="profile-json"></a>Perfil (JSON)
La configuración del perfil personal para un usuario. 
<a id="ID4EN"></a>

 
## <a name="profile"></a>Perfil
 
El objeto de perfil tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| AppDisplayName| string| Nombre para mostrar en las aplicaciones. Esto podría ser el nombre del usuario"real" o su nombre de jugador, dependiendo de la privacidad. Esta configuración representa la cadena de identidad del usuario que se debe usar para su presentación en las aplicaciones.| 
| GameDisplayName| string| Nombre para mostrar en los juegos. Esto podría ser el nombre del usuario"real" o su nombre de jugador, dependiendo de la privacidad. Esta configuración representa la cadena de identidad del usuario que se debe usar para su presentación en juegos.| 
| Gamertag| string| Nombre de jugador del usuario.| 
| AppDisplayPicRaw| string| URL de pic de presentación de la aplicación sin procesar (ver abajo).| 
| GameDisplayPicRaw| string| Dirección URL de presentación de juegos sin procesar pic (ver abajo).| 
| AccountTier| string| ¿Qué tipo de cuenta tiene el usuario? ¿Oro, plata o FamilyGold?| 
| TenureLevel| entero de 32 bits sin signo| ¿Cuántos años ha sido el usuario con Xbox Live?| 
| Puntuación| entero de 32 bits sin signo| Puntuación del usuario.| 
  


> [!NOTE] 
> Imágenes pueden ser la imagen del usuario' verdadero' o su gamerpic XboxOne, dependiendo de la privacidad. Esta configuración representa la dirección url de imagen del usuario que debe usarse para mostrar en el cliente. Esta imagen puede estar vacía (lo que indica que el usuario no ha establecido ninguna imagen). 


 
La dirección URL sin procesar es un tamaño variable. Se puede usar para especificar uno de los siguientes tamaños y formatos mediante anexando `&format={format}&w={width}&h={height}` al URI:
 
Formato: png
 
Tamaños: 64x64, 208x208, 424x424
 
<a id="ID4E2D"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E4D"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   