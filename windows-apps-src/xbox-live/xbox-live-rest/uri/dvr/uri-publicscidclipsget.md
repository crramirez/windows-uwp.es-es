---
title: GET (/public/scids/{scid}/clips)
assetID: 15b3e873-1f96-b1da-2f79-6dac1369a4c0
permalink: en-us/docs/xboxlive/rest/uri-publicscidclipsget.html
description: " GET (/public/scids/{scid}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5bce1dd261e0ad1172068a0287519cd0480da85f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589810"
---
# <a name="get-publicscidsscidclips"></a>GET (/public/scids/{scid}/clips)
Lista de clips públicas. El dominio de este URI es `gameclipsmetadata.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4ECB)
  * [Parámetros de cadena de consulta](#ID4ENB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Esta API permite varias formas de clips de la lista que son públicos. Se devuelve en las comprobaciones de privacidad y comprobaciones de aislamiento de contenido de la solicitud XUID de base de la lista de clips.
 
Las consultas se optimizan por identificador de configuración de servicio (¿SCID). Especificar aún más filtros o criterios de ordenación distintos de los predeterminados que se enumeran a continuación puede en algunas circunstancias tardan más en volver. Esto es más evidente para conjuntos más grandes de vídeos. Las consultas no pueden especificar un criterio de ordenación ascendente.
 
Se requiere el calificador para llegar a los clips de seapública colección específica. El usuario solicitante debe tener acceso a la ¿SCID solicitado, en caso contrario, 403 de HTTP se devolverá.
  
<a id="ID4ECB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| string| El identificador de configuración de servicio principal de los clips públicos.| 
| titleid| string| TitleId de los clips públicos. No se puede especificar en el mismo URI como el ¿SCID. Si se especifica, se usará para buscar el ¿SCID principal.| 
  
<a id="ID4ENB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| <b>?achievementId={achievementId}</b>| Clips más reciente de coincidencia especificado <b>achievementId</b>.| No se admite la ordenación o filtrado adicional.| 
| <b>?greatestMomentId={greatestMomentId}</b>| Clips más reciente de coincidencia especificado <b>greatestMomentId</b>.| No se admite la ordenación o filtrado adicional.| 
| <b>? calificador = creado </b>| Más reciente| Obligatorio.| 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Primario 

[/public/scids/{scid}/clips](uri-publicscidclips.md)

   