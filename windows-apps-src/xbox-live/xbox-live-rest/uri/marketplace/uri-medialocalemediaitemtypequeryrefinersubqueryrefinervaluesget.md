---
title: GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
assetID: 0fcbef77-4607-765e-72e1-d2e7620e2c61
permalink: en-us/docs/xboxlive/rest/uri-medialocalemediaitemtypequeryrefinersubqueryrefinervaluesget.html
description: " GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 183d15a306d0f15fcb5f9f7fad8411772f763c13
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661820"
---
# <a name="get-mediamarketplaceidmetadatamediaitemtypesmediaitemtypequeryrefinersqueryrefinersubqueryrefinervalues"></a>GET (/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues)
Obtiene la lista de valores secundario para un valor de matriz de consulta determinada (por ejemplo, "subgenres de un género determinado"). Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EDB)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
El valor de matriz de la consulta se pasa como un parámetro de cadena de consulta denominado **queryRefinerValue**, lo que se hace para permitir que consulta los valores de matriz con los caracteres se prohíbe en recursos de identificador URI que se pasará.
 
Esta API solo se admite para música.
  
<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EOB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQB"></a>

 
##### <a name="parent"></a>Primario 

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

  
<a id="ID4E1B"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   