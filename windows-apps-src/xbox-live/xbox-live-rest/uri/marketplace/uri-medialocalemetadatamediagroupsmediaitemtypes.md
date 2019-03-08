---
title: /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
assetID: fc096def-ac64-76c6-09f8-8f33a6bb47a0
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediagroupsmediaitemtypes.html
description: " /media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d014cbcf4e42e2f07c3cc32ceeb557a3c8537871
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637070"
---
# <a name="mediamarketplaceidmetadatamediagroupsmediagroupmediaitemtypes"></a>/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes
Tiene acceso a la mediaItemTypes disponibles por grupo de medios para la versión especificada de EDS. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediagroup| string| Obligatorio. Uno de los valores de [GET (/media/ {marketplaceId} / metadata/mediaGroups)](uri-medialocalemetadatamediagroupsget.md).| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md)

&nbsp;&nbsp;Enumera la mediaItemTypes disponibles por grupo de medios para la versión especificada de EDS.
 
<a id="ID4ELC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ENC"></a>

 
##### <a name="parent"></a>Primario 

[URI de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EXC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   