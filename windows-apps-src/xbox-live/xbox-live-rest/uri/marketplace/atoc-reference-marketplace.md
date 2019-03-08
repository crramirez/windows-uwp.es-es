---
title: URI de Marketplace
assetID: 27b6035f-84b9-67a8-6a12-85c450d18a58
permalink: en-us/docs/xboxlive/rest/atoc-reference-marketplace.html
description: " URI de Marketplace"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9fd8112c6e16b3e9d9fb70c34381e88ba5aa6273
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593880"
---
# <a name="marketplace-uris"></a>URI de Marketplace

Esta sección proporciona información detallada sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados desde servicios de Xbox Live para *marketplace* servicios, también conocido como entretenimiento Servicios de detección (EDS).

Solo se ejecuta en una consola Xbox 360, en un dispositivo Windows Phone o en Xbox.com juegos pueden usar este servicio.

Los dominios para estos URI son eds.xboxlive.com y inventory.xboxlive.com.

<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/users/me/inventory](uri-inventory.md)

&nbsp;&nbsp;Obtiene acceso al conjunto de inventario actualmente asociado con el usuario proporcionado.

[/users/me/consumables/{itemID}](uri-inventoryconsumablesitemurl.md)

&nbsp;&nbsp;El conjunto completo de los detalles de un elemento de inventario consumibles específico de accesos.

[/inventory/{itemID}](uri-inventoryitemurl.md)

&nbsp;&nbsp;El conjunto completo de los detalles de un elemento de inventario concreto de accesos.

[/media/{marketplaceId}/crossMediaGroupSearch](uri-localecrossmediagroupsearch.md)

&nbsp;&nbsp;Tiene acceso a los elementos de varios grupos de medios diferentes.

[/media/{marketplaceId}/browse](uri-medialocalebrowse.md)

&nbsp;&nbsp;Permite buscar elementos dentro de un grupo de medios.

[/media/{marketplaceId}/contentRating](uri-medialocalecontentrating.md)

&nbsp;&nbsp;Tener acceso al token de clasificación de contenido.

[/media/{marketplaceId}/details](uri-medialocaledetails.md)

&nbsp;&nbsp;Devuelve ofrece detalles y los metadatos sobre uno o varios elementos.

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

&nbsp;&nbsp;El token de campos se tiene acceso.

[/media/{marketplaceId}/metadata/mediaGroups](uri-medialocalemetadatamediagroups.md)

&nbsp;&nbsp;Enumera todos los mediaGroups admitidos para la versión especificada de EDS.

[/media/{marketplaceId}/metadata/mediaGroups/{mediagroup}/mediaItemTypes](uri-medialocalemetadatamediagroupsmediaitemtypes.md)

&nbsp;&nbsp;Tiene acceso a la mediaItemTypes disponibles por grupo de medios para la versión especificada de EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaItemType}/fields](uri-medialocalemetadatamediaitemtypefields.md)

&nbsp;&nbsp;Campos de accesos que se pueden esperar que los datos, para un determinado mediaitemtype y una versión concreta de EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners](uri-medialocalemetadatamediaitemtypequeryrefiners.md)

&nbsp;&nbsp;Tiene acceso a los refinadores de consulta para los medios de determinado tipo de elemento.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryrefinername}](uri-medialocalemetadatamediaitemtypequeryrefinersqueryrefinername.md)

&nbsp;&nbsp;Tiene acceso a los valores aceptables para el nombre de la matriz de consulta determinada y tipo de elemento multimedia especificado.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/queryrefiners/{queryRefiner}/subQueryRefinerValues](uri-medialocalemediaitemtypequeryrefinersubqueryrefinervalues.md)

&nbsp;&nbsp;Obtener acceso a la lista de valores secundario para un valor de matriz de consulta determinada (por ejemplo, "subgenres de un género determinado").

[/media/{marketplaceId}/metadata/mediaItemTypes](uri-medialocalemetadatamediaitemtypes.md)

&nbsp;&nbsp;Tiene acceso a todos los mediaItemTypes admitidos para la versión especificada de EDS.

[/media/{marketplaceId}/metadata/mediaItemTypes/{mediaitemtype}/sortorders](uri-medialocalemetadatamediaitemtypesortorders.md)

&nbsp;&nbsp;Accesos disponibles ordenación los pedidos de un tipo determinado mediaitem y una versión concreta de EDS.

[/media/{marketplaceId}/singleMediaGroupSearch](uri-medialocalesinglemediagroupsearch.md)

&nbsp;&nbsp;Permite buscar elementos dentro de un grupo de medios.

<a id="ID4EFD"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EHD"></a>


##### <a name="parent"></a>Primario

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)


<a id="ID4ERD"></a>


##### <a name="further-information"></a>Para obtener más información

[Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)
