---
title: inventoryItem (JSON)
assetID: 446cca28-b2d3-1b84-f973-94065519b391
permalink: en-us/docs/xboxlive/rest/json-inventoryitem.html
description: " inventoryItem (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 240e527258923cff146009810c190e401e0574d0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628500"
---
# <a name="inventoryitem-json"></a>inventoryItem (JSON)
El artículo de inventario de core representa el elemento estándar en el que se puede conceder un derecho.
<a id="ID4EN"></a>


## <a name="inventoryitem"></a>inventoryItem

El objeto inventoryItem tiene la especificación siguiente.

| Miembro| Tipo| Descripción|
| --- | --- | --- |
| url| string| Identificador único para este elemento de inventario concreto.|
| itemType| string| Tipo del elemento. Los valores actuales son <ul><li><b>Unknown</b></li><li><b>Juego</b></li><li><b>Película</b></li><li> <b>TVShow</b></li><li><b>MusicVideo</b></li><li><b>GameTrial</b></li><li><b>ViralVideo</b></li><li><b>TVEpisode</b></li><li><b>TVSeason</b></li><li><b>TVSeries</b></li><li><b>VideoPreview</b></li><li><b>Poster</b></li><li><b>Podcast</b></li><li><b>Image</b></li><li><b>BoxArt</b></li><li><b>ArtistPicture</b></li><li><b>GameContent</b></li><li><b>GameDemo</b></li><li><b>Tema</b></li><li><b>XboxOriginalGame</b></li><li><b>GamerTile</b></li><li><b>ArcadeGame</b></li><li><b>GameConsumable</b></li><li><b>Album</b></li><li><b>AlbumDisc</b></li><li><b>AlbumArt</b></li><li><b>GameVideo</b></li><li><b>BackgroundArt</b></li><li><b>TVTrailer</b></li><li><b>GameTrailer</b></li><li><b>VideoShort</b></li><li><b>agrupación</b></li><li><b>XnaCommunityGame</b></li><li><b>Promocional</b></li><li><b>MovieTrailer</b></li><li><b>SlideshowPreviewImage</b></li><li><b>ServerBackedGames</b></li><li><b>Marketplace</b></li><li><b>AvatarItem</b></li><li><b>LiveApp</b></li><li><b>WebGame</b></li><li><b>MobileGame</b></li><li><b>MobilePdlc</b></li><li><b>MobileConsumable</b></li><li><b>App</b></li><li><b>MetroGame</b></li><li><b>MetroGameContent</b></li><li><b>MetroGameConsumable</b></li><li><b>GameLayer</b></li><li><b>GameActivity</b></li><li><b>GameV2</b></li><li><b>SubscriptionV2</b></li><li><b>Suscripción</b><br/><br/> **Nota:** Juegos se designan mediante **GameV2**, son consumibles **GameConsumable**, y es duradero DLC **GameContent**. |
  | Contenedores | string | Este es el conjunto de contenedores"" que contienen este elemento. Inventario de un usuario se puede consultar para elementos que pertenecen a un contenedor específico. Estos contenedores se determinan cuando se agrega el elemento con el inventario mediante la compra. |
  | obtener | DateTime | Fecha y hora que se agregó el elemento al inventario del usuario. |
  | startDate | DateTime | Fecha y hora se convirtió en el elemento o se pasará a estar disponible para su uso. |
  | endDate | DateTime | Fecha y hora en el elemento se convirtió en o quedará inservible. |
  | Estado | string | El estado del elemento. Los valores permitidos son **habilitado**, **Suspended**, **expirado**, **Canceled**, **renovación**.  |
  | versión de prueba | Valor booleano | Obligatorio. Es True si este derecho es una versión de prueba; en caso contrario, false. Si comprar la versión de prueba de un derecho y, a continuación, compre la versión completa, se recibirán los dos. |
  | trialTimeRemaining | TimeSpan | Que acepta valores NULL. ¿Cuánto tiempo restante de la versión de prueba, en minutos. |
  | consumable | array | Si los elementos es consumibles, contiene una representación en línea del identificador único (vínculo) para el artículo de inventario consumibles, así como la cantidad actual. |

<a id="ID4EMAAC"></a>


## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo


```json
inventoryItem {
  "url": string,
  "itemType": "Music" | "Video" | "Game" | "AvatarItem" | "Subscription" | "DLC" | "Consumable" | ...,
  "obtained": DateTime,
  "beginDate": DateTime,
  "endDate": DateTime,
  "state": "Unavailable" | "Available" | "Suspended" | "Expired",
  "trial": true,
  "trialTimeRemaining":"23:12:14",
  ("consumable": {"url": string, "quantity": int})
}

```


<a id="ID4EVAAC"></a>


## <a name="consumable-inventory-item"></a>Elemento de inventario consumibles

La entidad consumible presenta el conjunto mínimo de propiedades para un elemento consumible.

| Miembro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| url| string| Identificador único para el elemento de inventario consumibles específico.|
| quantity| entero de 32 bits con signo| La cantidad actual de este artículo de inventario.|


```json
consumableInventoryItem {
  "url": string,
  "quantity": int
}

```


<a id="ID4E4BAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4E6BAC"></a>


##### <a name="parent"></a>Primario

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)


<a id="ID4EJCAC"></a>


##### <a name="reference"></a>Referencia

[/users/me/inventory](../uri/marketplace/uri-inventory.md)

 [/inventory/consumables/{itemID}](../uri/marketplace/uri-inventoryconsumablesitemurl.md)

 [/inventory/{itemID}](../uri/marketplace/uri-inventoryitemurl.md)
