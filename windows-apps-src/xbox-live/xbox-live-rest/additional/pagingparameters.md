---
title: Parámetros de paginación
assetID: f8d059fd-30e7-be60-0a46-c9a833c400c6
permalink: en-us/docs/xboxlive/rest/pagingparameters.html
description: " Parámetros de paginación"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fe80e1666f9eab8caf7e0bbdb2b537bd7661c9a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627300"
---
# <a name="paging-parameters"></a>Parámetros de paginación
 
Algunos URI de servicios de Xbox Live devuelven colecciones de objetos de JavaScript Object Notation (JSON). Estas colecciones se pueden paginar a través de especificando los parámetros de paginación como parte de la cadena de consulta asociada al identificador URI. Sigue una lista completa de los parámetros de paginación. Todos los identificadores URI que permitan los parámetros de paginación se vinculan a la parte inferior de esta página.
 
<a id="ID4E2"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta 
 
| Parámetro| Requerido| Tipo| Descripción| 
| --- | --- | --- | --- | 
| continuationToken| No| string| Devolver los elementos comenzando en el token de continuación determinado. | 
| maxItems| No| entero de 32 bits con signo| Número máximo de elementos que se va a devolver de la colección, que se puede combinar con <b>skipItems</b> y <b>continuationToken</b> para devolver un intervalo de elementos. El servicio puede proporcionar un valor predeterminado si <b>maxItems</b> no está presente y es posible que devuelva menos de <b>maxItems</b>, incluso si aún no se ha devuelto la última página de resultados. | 
| skipItems| No| entero de 32 bits con signo| Devolver elementos a partir de después del número especificado de elementos. Por ejemplo, <b>skipItems = "3"</b> recuperará los elementos recuperados a partir del cuarto elemento. | 
  
<a id="ID4EDD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EFD"></a>

 
##### <a name="parent"></a>Primario  

[Referencia adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4ERD"></a>

 
##### <a name="reference--get-usersxuidxuidachievementsuriachievementsuri-achievementsusersxuidachievementsgetv2md"></a>Referencia [GET (/users/xuid({xuid})/achievements)](../uri/achievements/uri-achievementsusersxuidachievementsgetv2.md)

   