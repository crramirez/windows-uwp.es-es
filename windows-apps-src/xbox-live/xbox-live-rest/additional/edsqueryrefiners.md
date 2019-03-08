---
title: Refinadores de consulta EDS
assetID: ab5c066a-a48b-3042-351d-d0a15f663276
permalink: en-us/docs/xboxlive/rest/edsqueryrefiners.html
description: " Refinadores de consulta EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: c00ff971e05003ec88c47d3803e565f6e9406c47
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611470"
---
# <a name="eds-query-refiners"></a>Refinadores de consulta EDS
 
<a id="ID4EO"></a>

  
 
Los parámetros siguientes pueden usarse para refinar una consulta de servicios de detección de entretenimiento (EDS) a un conjunto de elementos más orientado. Ninguno de estos parámetros son necesarios en cualquier API, pero haya sido aceptados en cualquier API que acepte refinadores de consulta.
 
Los nombres de parámetro se pueden pasar como valores en cualquier parámetro de "queryRefiners". Esta después, aplica el número de elementos que se devolverían si la solicitud se repitieron con la matriz de consulta, desglosada por cada valor de la matriz de consulta si la devolución.
 
Le mostramos cómo podría funcionar en la práctica:
 
   * Se realiza una llamada a la API de exploración, incluido el parámetro "queryRefiners = género".
   * La API devuelve ocho juegos. Además de los elementos, se devolverán una lista de cada género que tiene elementos, junto con el número de elementos pertenece a ese género. Para un juego, esto podría ser "Shooter: 3, rompecabezas: 5".
   * Se realiza una segunda consulta. Es idéntico al primero, salvo que "género = Shooter" se agrega.
   * Ahora, la respuesta contiene solo tres juegos, que pertenecen a la categoría "Shooter".
  
| Parámetro| Tipo de datos| Descripción| 
| --- | --- | --- | 
| <b>decade</b>| string| La década en la que se deben haber liberado todos los elementos.| 
| <b>genre</b>| matriz de cadena| La lista de géneros que deben tener todos los elementos.| 
| <b>labelOwner</b>| string| La etiqueta de música asociada con el intérprete, álbum o seguimiento.| 
| <b>network</b>| matriz de cadena| La red que creó los elementos.| 
| <b>studio</b>| matriz de cadena| El estudio que creó los elementos.| 
| <b>xboxAppCategories</b>| matriz de cadena| La lista de categorías que deben tener todas las aplicaciones de Xbox.| 
| <b>xboxAvatarClothes</b>| matriz de cadena| La lista de tipos de ropa deben tener todos los elementos de Xbox Avatar.| 
| <b>xboxAvatarStores</b>| matriz de cadena| La lista de almacenes de a qué Xbox todos los elementos del avatar deben pertenecer.| 
| <b>xboxGamePublisherBits</b>| matriz de cadena| La lista de bits del Editor del juego que se debe establecer en todos los elementos de GameType o elementos AppType.| 
| <b>xboxIsBrowsable</b>| Valor booleano| Si <b>true</b>, devolverá juegos completas que no son directamente procesables además de contenido útil. El valor predeterminado es <b>false</b>.| 
| <b>xboxHasChildMediaItemTypes</b>| matriz de cadena| Todos los elementos devueltos con un grupo de medios de juego deben tener elementos secundarios cuyo tipo de elemento multimedia es uno de los valores proporcionados.| 
  
<a id="ID4EEF"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EGF"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ESF"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[URI de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   