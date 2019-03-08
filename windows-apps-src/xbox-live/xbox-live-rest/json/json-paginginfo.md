---
title: PagingInfo (JSON)
assetID: 645e575d-3e8e-d954-90e6-e51dd83da93b
permalink: en-us/docs/xboxlive/rest/json-paginginfo.html
description: " PagingInfo (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0e773d73499e79fe23f736a536027932ca1a07b4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651220"
---
# <a name="paginginfo-json"></a>PagingInfo (JSON)
Contiene información de paginación de resultados que se devuelven en páginas de datos. 
<a id="ID4EN"></a>

 
## <a name="paginginfo"></a>PagingInfo
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| continuationToken| string| Un token de continuación opaco que se usa para acceder a la página siguiente de resultados. Número máximo de 32 caracteres. Los llamadores pueden suministrar este valor en el <b>continuationToken</b> parámetro de consulta para recuperar el siguiente conjunto de elementos de la colección. Si esta propiedad es <b>null</b>, entonces no hay ningún elemento adicional en la colección. Esta propiedad es necesaria y se proporciona incluso si la colección se paginan con <b>skipItems</b>.| 
| totalItems| entero de 32 bits con signo| El número total de elementos de la colección. No se proporciona si el servicio no puede proporcionar una vista en tiempo real en el tamaño de la colección.| 
  
<a id="ID4E4B"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
   "continuationToken":"16354135464161213-0708c1ba-147f-48cc-9ad9-546gaadg648"
   "totalItems":5,
}
    
```

  
<a id="ID4EGC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EIC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

   