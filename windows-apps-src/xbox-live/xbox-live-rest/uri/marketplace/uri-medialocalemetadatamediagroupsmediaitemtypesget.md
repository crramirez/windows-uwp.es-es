---
title: GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
assetID: 1bbfdfd7-84e0-68e0-49e8-ba1c60fabaa3
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c48079e57d4d490ab75e8120eff81c77af855923
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611450"
---
# <a name="get-mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)
Enumera la mediaItemTypes disponibles por grupo de medios para la versión especificada de EDS. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediagroup| string| Obligatorio. Uno de los valores de [GET (/media/ {marketplaceId} / metadata/mediaGroups)](uri-medialocalemetadatamediagroupsget.md).| 
  
<a id="ID4EAB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ECB"></a>

 
##### <a name="parent"></a>Primario 

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

  
<a id="ID4EMB"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   