---
title: GET (/media/{marketplaceId}/fields)
assetID: 1d535344-8522-0fd1-4daa-cd0f0a0f1121
permalink: en-us/docs/xboxlive/rest/uri-medialocalefieldsget.html
description: " GET (/media/{marketplaceId}/fields)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cc3ae8abfe04b7a0b9728d07b9488f9ed7c27816
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606680"
---
# <a name="get-mediamarketplaceidfields"></a>GET (/media/{marketplaceId}/fields)
Obtiene el token de campos. Es el dominio para estos URI `eds.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EGC)
  * [Parámetros de cadena de consulta](#ID4ERC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
API, de forma predeterminada, servicios de detección de entretenimiento (EDS) devuelve un conjunto muy pequeño mínimo de campos de cada elemento:
 
   * Tipo de elemento multimedia
   * Grupo de medios
   * Id
   * Nombre
  
Para obtener más información, acepten las API de un **campos** parámetro que especifica qué partes de datos adicionales se deben devolver. Porque hay muchos campos posibles, especificando su nombre en su totalidad para cada llamada de API sería en gran medida a la saturación de la solicitud. En su lugar, los nombres se pueden pasar a esta API, lo que generará un valor mucho más pequeño que se puede pasar a las otras API.
 
Para cualquier API que acepte este parámetro, el valor proporcionado debe ser el superconjunto de todos los campos en todos los tipos de elemento de medio especificado. No es posible especificar distintos conjuntos de campos para tipos de elemento de medios diferentes. Sin embargo, si un campo que se aplica a un elemento tipo de medio, pero no otra, es sólo aparecerán en el medio de tipos de elementos que existen datos (por ejemplo, si se incluye "AvatarBodyType" en la llamada a [obtener (/media/ {marketplaceId} / campos)](uri-medialocalefields.md), solo AvatarItems contendrá el campo).
 
Los valores devueltos por esta API son altamente almacenable en caché, en realidad, no debería cambiar, excepto entre las implementaciones de EDS. Se recomienda que, si se desea el almacenamiento en caché, la memoria caché no durar más de la sesión del usuario.
 
Además de aceptar los nombres de campo en Sí, esta API acepta "all" como un valor válido. Esto generará un valor que contiene cada campo que es posible especificar. Se recomienda el uso del valor "all" solo para el desarrollo, depuración y pruebas.
 
Como alternativa, puede enviar `desired={list of fields separated by a '.'}` a cualquier API que acepta el **campos** token.
 
No se puede pasar tanto en **deseado** y **campos** juntos.
  
<a id="ID4EGC"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| marketplaceId| string| Obligatorio. Valor obtenido de la cadena la <b>Windows.Xbox.ApplicationModel.Store.Configuration.MarketplaceId</b>.| 
  
<a id="ID4ERC"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| deseado| string| Obligatorio. El '.' -separados por la lista de campos que se deben devolver además del conjunto mínimo. No todos los campos especificados se garantiza que se devuelve para cada objeto (datos simplemente no exista para determinados elementos), pero no se devolverá ningún campo fuera del conjunto mínimo que no se especifica aquí. | 
  
<a id="ID4EMD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EOD"></a>

 
##### <a name="parent"></a>Primario 

[/media/{marketplaceId}/fields](uri-medialocalefields.md)

  
<a id="ID4EYD"></a>

 
##### <a name="further-information"></a>Para obtener más información 

[Encabezados comunes de EDS](../../additional/edscommonheaders.md)

 [Parámetros de EDS](../../additional/edsparameters.md)

 [Consulta de EDS refinadores](../../additional/edsqueryrefiners.md)

 [URI de Marketplace](atoc-reference-marketplace.md)

 [Referencia adicional](../../additional/atoc-xboxlivews-reference-additional.md)

   