---
title: UserTitle (JSON)
assetID: 3e5767af-5704-8463-696b-42a7d2a1e231
permalink: en-us/docs/xboxlive/rest/json-usertitlev2.html
description: " UserTitle (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 33901a5bde25fd17072c2b45d587a33209424378
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623030"
---
# <a name="usertitle-json"></a>UserTitle (JSON)
Contiene los datos de título del usuario. 
<a id="ID4EN"></a>

 
## <a name="usertitle"></a>UserTitle
 
El objeto UserTitle tiene la especificación siguiente. Todas las propiedades son necesarias.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| lastUnlock| DateTime| El tiempo por última vez se ha obtenido un logro.| 
| titleId| entero de 32 bits sin signo| El identificador único para el título.| 
| titleVersion| string| La versión del título.| 
| serviceConfigId| string| Id. del conjunto de configuración de servicio principal asociado con el título.| 
| titleType| string| El tipo de título.| 
| Plataforma| string| La plataforma admitida.| 
| name| string| El nombre de este título de texto. Longitud máxima: 22.| 
| earnedAchievements| entero de 32 bits sin signo| El número de logros había acumulado para el título, incluidos logros desbloqueados y desafíos completó correctamente.| 
| currentGamerscore| entero de 32 bits sin signo| La puntuación total de que este usuario ha ganado en este título.| 
| maxGamerscore| entero de 32 bits sin signo| La puntuación total posible para este título.| 
  
<a id="ID4EFE"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EHE"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   