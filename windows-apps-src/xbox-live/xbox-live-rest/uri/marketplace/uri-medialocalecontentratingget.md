---
title: GET (/media/{marketplaceId}/contentRating)
assetID: e677acdb-d905-3bea-b0ca-6d8becd663c0
permalink: en-us/docs/xboxlive/rest/uri-medialocalecontentratingget.html
description: " GET (/media/{marketplaceId}/contentRating)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 8d1cb9d09de8671d4cd3d61e96a8335412237e5c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641590"
---
# <a name="get-mediamarketplaceidcontentrating"></a>GET (/media/{marketplaceId}/contentRating)
Obtener el token de clasificación de contenido. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4ELB)
  * [Parámetros de cadena de consulta](#ID4EWB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Aplicar controles parentales sobre el contenido que los niños pueden ver es una tarea complicada. No sólo cada tipo de elemento multimedia tiene su propio sistema de clasificación, pero los sistemas de clasificación pueden variar de un país a otro. Esto significa que hay distintas partes de datos que deben especificarse para filtrar correctamente todos los elementos.
 
En lugar de especificar todos los parámetros en todas las llamadas de API, esta API genera un valor que se pasará a **combinedContentRating** parámetros en otras API y comunicarse todavía la misma información. Esto está diseñado para facilitar las API y mantenimiento, como los varios parámetros pasados a esta API se contraen en un único valor reutilizable para las otras API.
 
Aunque finalmente pueden cambiar los valores exactos devueltos por esta API, debe cambiar con muy poca frecuencia (por ejemplo, entre las versiones de servicios de detección de entretenimiento (EDS)) y, por tanto, podría almacenarse en caché durante largos períodos de tiempo. Cualquier API que acepte un **combinedContentRating** parámetro proporcionará un mensaje de error descriptivo si el valor pasado no es válido, que es un valor que indica el llamador necesita simplemente llamar a esta API para obtener un valor actualizado. Si una API acepta un **combinedContentRating** parámetro, pero uno no proporciona ningún filtrado de contenido se llevará a cabo según el control parental. 

> [!NOTE] 
> Esto no significa que se devuelve solo "seguro" contenido--significa que se devuelve todo el contenido, incluido contenido potencialmente explícito. 


  
<a id="ID4ELB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| filterExplicit| Valor booleano| Opcional. Filtra las letras de canciones.| 
| filterFamilyOnlyApps| Valor booleano| Opcional. Filtre las aplicaciones no marcadas como familia descriptivas.| 
| filterUnrated| Valor booleano| Opcional. Determina si se debe incluir contenido sin clasificación en la respuesta o no.| 
| maxGameRating| entero de 32 bits con signo| Opcional. Filtra los juegos.| 
| maxMovieRating| entero de 32 bits con signo| Opcional. Filtra las películas por encima de un nivel específico.| 
| maxTVRating| entero de 32 bits con signo| Opcional. Filters TV.| 
  
<a id="ID4E5D"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAE"></a>

 
##### <a name="parent"></a>Primario 

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

  
<a id="ID4EKE"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   