---
title: /media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields
assetID: fc9b556a-7fc7-64ec-cb5c-b5cabd2ab4ce
permalink: en-us/docs/xboxlive/rest/uri-medialocalemetadatamediaitemtypefields.html
description: " /media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 53d0113368754abc3be36b4e7dda213adf38b640
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613060"
---
# <a name="mediamarketplaceidmetadatamediaitemtypesmediaitemtypefields"></a>/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields
Campos de accesos que se pueden esperar que los datos, para un determinado mediaitemtype y una versión concreta de EDS. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
| mediaitemtype| string| Obligatorio. Uno de los valores de [obtener (/media/ {marketplaceId} / metadata/mediaGroups / {mediagroup} / mediaItemTypes)](uri-medialocalemetadatamediagroupsmediaitemtypesget.md).| 
  
<a id="ID4EBC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields)](uri-medialocalemetadatamediaitemtypefieldsget.md)

&nbsp;&nbsp;Enumera los campos que se pueden esperar que los datos, para un determinado mediaitemtype y una versión concreta de EDS.
 
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

   