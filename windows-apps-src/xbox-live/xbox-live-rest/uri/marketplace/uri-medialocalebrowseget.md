---
title: GET (media/{marketplaceId}/browse)
assetID: 024447a0-c615-e08b-f867-3b6c4c0db5dc
permalink: en-us/docs/xboxlive/rest/uri-medialocalebrowseget.html
description: " GET (media/{marketplaceId}/browse)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b072c482fba1f36ac425b98c0126b56e735af078
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622580"
---
# <a name="get-mediamarketplaceidbrowse"></a>GET (media/{marketplaceId}/browse)
Permite buscar elementos dentro de un grupo de medios. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EFB)
  * [Parámetros de cadena de consulta](#ID4EQB)
  * [Cuerpo de respuesta](#ID4E6B)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Pueden tener acceso a las páginas de datos devueltos por esta búsqueda no secuencialmente mediante el parámetro skipItems en lugar de usar el token de continuación. Esta API acepta refinadores de consulta. 
 
 **SandboxId** se recuperan de la notificación en el XToken ahora y se aplican. Si el **SandboxId** no está presente, a continuación, servicios de detección de entretenimiento (EDS) producirá un error 400 Solicitud incorrecta. 
  
<a id="ID4EFB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4EQB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
Esta API acepta los siguientes parámetros de consulta: combinedContentRating, desiredMediaItemTypes, campos, maxItems, preferredProvider, q, queryRefiners, skipItems, firstPartyOnly, freeOnly, hasTrailer, latestOnly, subscriptionLevel y topRatedOnly .
 
Consulte [EDS parámetros](../../additional/edsparameters.md) para obtener más información acerca de estos parámetros.
  
<a id="ID4E6B"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EFC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 
El siguiente código JSON es en respuesta a la llamada `/media/en-us/browse?orderBy=releaseDate&desiredMediaItemTypes=DGame&fields=all`.
 

```cpp
{
    "Items": [{
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "6c5402e4-3cd5-4b29-a9c4-bec7d2c7514a",
        "Name": "Tanks-A",
        "ReducedName": "Tanks-A short title",
        "ReleaseDate": "2013-05-14T00: 00: 00Z",
        "TitleId": "1676B413",
        "VuiDisplayName": "Tanks-A vui",
        "DeveloperName": "Microsoft Studios",
        "PublisherName": "Microsoft Studios",
        "SortName": "Tanks-A sort title. ",
        "KValue": "4",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "1676B413"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "www.microsoft.com"
            }],
            "ContentId": "c9fcb807-626d-5347-b918-65496f084219",
            "GameRegionMask": 1,
            "PackageType": "CAB",
            "LicenseBit": 1,
            "LicenseType": "SyncCast DTO"
        }],
        "SandboxId": "PART.Dev1"
    },
    {
        "MediaGroup": "GameType",
        "MediaItemType": "DGame",
        "ID": "fd16e2fb-eca4-4182-8f69-a98fdd6e57a1",
        "Name": "Vector based massively multiplayer Tanks game.",
        "ReducedName": "Tanks",
        "ReleaseDate": "2013-03-15T00: 00: 00Z",
        "TitleId": "69A60C76",
        "VuiDisplayName": "Tanks",
        "DeveloperName": "Microsoft Studios",
        "Images": [{
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 300,
            "Width": 219,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Box_Art"],
            "Purpose": "Box_Art",
            "Height": 120,
            "Width": 85,
            "Order": 1
        },
        {
            "ID": "32ebd9fe-6a22-4111-954e-564ec69d802a",
            "ResizeUrl": "http: //images-eds.dnet.xboxlive.com/image?url=RIJNAEIo6.u.tudW9rXJ2lWDOsMikqfNiHE2Sp4qbgNbH6_drY8Ek2cyHXEnnUKPUXAH_m8a3Oe4_wpV7CkKA0Snc9puIYOGxsIfyyncTBv.MIluDZX6UqAPsJYHE5go_J_BBfxNWW6yrK4.K75aMQ--",
            "Purposes": ["Image"],
            "Purpose": "Image",
            "Height": 720,
            "Width": 1280,
            "Order": 1
        }],
        "PublisherName": "Microsoft Studios",
        "RatingId": "10",
        "ParentalRating": "E",
        "ParentalRatingSystem": "ESRB",
        "SortName": "\n            Vector based massively multiplayer Tanks game.\n          ",
        "Capabilities": [{
            "NonLocalizedName": "onlineMultiplayerMin",
            "Value": "3"
        },
        {
            "NonLocalizedName": "onelineMultiplayerMax",
            "Value": "5"
        }],
        "KValue": "5",
        "KValueNamespace": "bingbox",
        "AllTimePlayCount": 30.0,
        "SevenDaysPlayCount": 30.0,
        "ThirtyDaysPlayCount": 30.0,
        "AllTimeRatingCount": 12.0,
        "ThirtyDaysRatingCount": 12.0,
        "SevenDaysRatingCount": 12.0,
        "AllTimeAverageRating": 0.8,
        "ThirtyDaysAverageRating": 0.8,
        "SevenDaysAverageRating": 0.8,
        "LegacyIds": [{
            "IdType": "TitleId",
            "Value": "69A60C76"
        }],
        "Availabilities": [{
            "AvailabilityID": "",
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "Devices": [{
                "Name": "Durango"
            }]
        }],
        "Packages": [{
            "CdnFileLocation": [{
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            },
            {
                "SortOrder": null,
                "Uri": "https: //update.dnet.xboxlive.com/test_cdn/tanks-randomkey.xvc"
            }],
            "ContentId": "7AE9DAB2-1162-488D-9F80-B804EC5AF879",
            "PackageType": "XVC",
            "LicenseType": "Xbox Live Games Machine and User"
        }],
        "SandboxId": "PART.Dev1"
    }],
    "ImpressionGuid": "5d3085cf-7d17-43b4-a9c1-a1ccfe764eb1"
}
         
```

   
<a id="ID4EUC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EWC"></a>

 
##### <a name="parent"></a>Primario 

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

  
<a id="ID4EAD"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   