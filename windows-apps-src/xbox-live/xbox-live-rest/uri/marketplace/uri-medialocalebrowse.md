---
title: /media/{marketplaceId}/browse
assetID: 4fedc780-b3c2-c83b-e7af-9e18666a4771
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowse.html
description: " /media/{marketplaceId}/browse"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f692fb66580e20ffeefb3595b8cf9d795f504311
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589690"
---
# <a name="mediamarketplaceidbrowse"></a>/media/{marketplaceId}/browse
Permite buscar elementos dentro de un grupo de medios. La API de exploración permite a los clientes buscar los elementos desde dentro de un grupo de medios. Pueden tener acceso a las páginas de datos no secuencialmente mediante el parámetro skipItems en lugar de usar el token de continuación.
 
Esta API también permite explorar dentro de los elementos secundarios de un elemento determinado. Por ejemplo, pasando un identificador y un parámetro MediaItemType para un juego de Xbox 360, esto permite examinar y diltering en los elementos secundarios de ese elemento, como elementos de Avatar o DLC para el juego.
 
Esta API acepta refinadores de consulta.
 
Se incluyen algunos escenarios de recuperación de elementos secundarios:
 
   * Álbum de pistas
   * Serie para la temporada de invierno
   * Estaciones a episodios
   * Realizar un seguimiento de vídeo de música
   * Intérprete para álbumes
   * Juegos para los complementos de juego (DLC, Avatar, temas, etcetera.)
  
Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EMB)
 
<a id="ID4EMB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ENC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (media/{marketplaceId}/browse)](uri-medialocalebrowseget.md)

&nbsp;&nbsp;Permite buscar elementos dentro de un grupo de medios. 
 
<a id="ID4EXC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EZC"></a>

 
##### <a name="parent"></a>Primario 

[URI de Marketplace](atoc-reference-marketplace.md)

  
<a id="ID4EDD"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   