---
title: /media/{marketplaceId}/details
assetID: bc8758ed-2f90-b501-5c3f-6f253f02d754
permalink: en-us/docs/xboxlive/rest/uri-medialocaledetails.html
description: " /media/{marketplaceId}/details"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f58e5247c3fd52e84a3a9bab28c6926f74e864e3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655710"
---
# <a name="mediamarketplaceiddetails"></a>/media/{marketplaceId}/details
Devuelve ofrece detalles y los metadatos sobre uno o varios elementos. Es el dominio para estos URI `eds.xboxlive.com`.
 
Los detalles de la API difiere de la API relacionada y la API de examinar (cuando passin en un Id.) como esas API devuelven información acerca de otros elementos que están asociados con el identificador fiven, explícita o implícitamente, mientras que la API de detalles devuelve información adicional acerca del mismo elemento.
 
Se pueden pasar varios identificadores de tipos de elemento de medios diferentes en una sola llamar (siempre y cuando no son de tipo ProviderContentID - vea a continuación), pero todos ellos deben pertenecer al mismo grupo de medios. Sin embargo, hay un par de escenarios de cliente donde el autor de llamada no conoce el grupo de medios. La API es compatible con esto, ya que permite un valor especiales de "Desconocido" para el grupo de medios en las situaciones siguientes:
 
   * Tipo_id = XboxHexTitle, lo que generará elementos AppType o GameType
   * Tipo_id = ProviderContentId, lo que producirá MovieType o TVType elementos
  
El gráfico siguiente resume la asignación completa de qué Id. de tipos pueden proporcionarse con los grupos de medios:
 
| Tipo de Id.| AppType| GameType| MovieType| MusicArtistType| MusicType| TVType| WebVideoType| Unknown| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| canónico| esté| esté| esté| esté| esté| esté| esté| N| 
| ZuneCatalog| N| N| esté| esté| esté| esté| N| N| 
| ZuneMediaInstance| N| N| esté| N| esté| esté| N| N| 
| Oferta| esté| esté| esté| N| esté| esté| N| N| 
| AMG| N| N| N| N| esté| N| N| N| 
| MediaNet| N| N| N| N| esté| N| N| N| 
| XboxHexTitle| esté| esté| N| N| N| N| N| esté| 
| ProviderContentId| N| N| esté| N| N| esté| N| esté| 
 
  * [Notas de parámetro](#ID4EEH)
  * [Parámetros de URI](#ID4EUH)
 
<a id="ID4EEH"></a>

 
## <a name="parameter-notes"></a>Notas de parámetro
 
<a id="ID4EIH"></a>

 
### <a name="providercontentid"></a>ProviderContentId
 
Esto se usa para el proveedor de búsquedas identificador específico. P. ej. Id. de Netflix o Id. de Hulu.
 
Una vez Tipo_id ProviderContentId, se acepta un solo valor. Esto es porque ProviderContentIds son el único tipo de identificador que puede contener el '.' caracteres. Puesto que la '.' carácter es también el separador que se usa entre los identificadores, hay ambigüedad entre lo que es un delimieter entre los identificadores y cuál es la parte de la misma identificación. El resto de la API funciona del mismo modo para ProviderContentIds, excepto para la funcionalidad de búsqueda masiva.
   
<a id="ID4EUH"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EWAAC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/media/{marketplaceId}/details)](uri-medialocaledetailsget.md)

&nbsp;&nbsp;Devuelve ofrece detalles y los metadatos sobre uno o varios elementos. 
 
<a id="ID4EABAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ECBAC"></a>

 
##### <a name="parent"></a>Primario 

[URI de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EMBAC"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   