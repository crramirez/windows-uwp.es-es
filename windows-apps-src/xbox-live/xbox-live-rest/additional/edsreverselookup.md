---
title: Búsqueda inversa de EDS para vídeo
assetID: 773f7a8e-7571-3aec-96d6-478437696ea6
permalink: en-us/docs/xboxlive/rest/edsreverselookup.html
description: " Búsqueda inversa de EDS para vídeo"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: d535dec8a95eba4d486bfc6946e187e2da66ae49
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598440"
---
# <a name="eds-reverse-lookup-for-video"></a>Búsqueda inversa de EDS para vídeo
 
  * [Pasos de búsqueda inversos](#ID4EQ)
 
<a id="ID4EQ"></a>

 
## <a name="reverse-lookup-steps"></a>Pasos de búsqueda inversos
 
Se admite la búsqueda inversa de los servicios de detección (EDS) de entretenimiento para todos los tipos de multimedia de vídeo (**MediaItemType.Movie**, **MediaItemType.TVSeries**, **MediaItemType.TVEpisode**, **MediaItemType.TVSeason**, y **MediaItemType.TVShow**), así como **MediaItemType.Unknown**.
 
Búsqueda inversa necesita 4 parámetros que se pasarán: 
   * `idType=ScopedMediaId`
   * `ids=` el identificador de la media de proveedor
   * `ScopeIdType=Title`
   * `ScopeId=` el identificador del proveedor título
 
 
Normalmente, la búsqueda inversa requiere de 2 pasos: 
   * Recuperar el identificador del proveedor de medios (por ejemplo, desde una llamada de detalles) si no está disponible. 

```cpp
GET /media/en-us/details?ids=4eeaf5b4-9af2-56e4-a738-68b48e954494&desiredMediaItemTypes=Movie&desired=Providers
```

 
   * Puede emitir la llamada para el uso de búsqueda inversa la **ProviderMediaId** arrastrándolo desde la respuesta anterior: 

```cpp
GET /media/en-us/details?ids=047d19ca-3a7d-462c-bdbb-163543125583&idType=ScopedMediaId&desiredMediaItemTypes=Movie&fields=all&ScopeIdType=Title&ScopeId=0x5848085B
```

 
  
 
Si el **ProviderMediaId** campo no se ha recuperado de EDS, a continuación, el campo debe ser codificados de dirección URL que se pasan correctamente al EDS.
  
<a id="ID4EOC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EQC"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4E3C"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   