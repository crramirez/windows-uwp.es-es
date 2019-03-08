---
title: /media/{marketplaceId}/singleMediaGroupSearch
assetID: f5599db7-4050-640e-db96-2df01a007c07
permalink: en-us/docs/xboxlive/rest/uri-medialocalesinglemediagroupsearch.html
description: " /media/{marketplaceId}/singleMediaGroupSearch"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b26b4c2dc51ef5591480372aa9908a49d2f8cbe2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661760"
---
# <a name="mediamarketplaceidsinglemediagroupsearch"></a>/media/{marketplaceId}/singleMediaGroupSearch
Permite buscar elementos dentro de un grupo de medios. Tenga en cuenta que las páginas de datos devueltos por esta búsqueda se pueden acceder sin secuencialmente mediante el parámetro skipItems en lugar de usar el token de continuación. Esta API acepta refinadores de consulta.
 
Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EYB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (media/{marketplaceId}/singleMediaGroupSearch)](uri-medialocalesinglemediagroupsearchget.md)

&nbsp;&nbsp;Permite buscar elementos dentro de un grupo de medios. 
 
<a id="ID4ECC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EEC"></a>

 
##### <a name="parent"></a>Primario 

[URI de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EOC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   