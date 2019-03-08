---
title: /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
assetID: edcb19bd-87a5-732b-0c45-6f7355fc2dd1
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistnameindex.html
description: " /users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b56b563c72c206340aa2c1ce9f73aa8dfe50809d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641230"
---
# <a name="usersxuidxuidlistspinslistnameindexindexinsertindexinsertindex"></a>/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}
Mueve un elemento dentro de una lista. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI 
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| XUID| string| XUID del usuario.| 
| nombre de lista| string| Nombre de la lista para manipular.| 
| index| string| Especifica el índice actual del elemento que se va a mover. Si el valor de índice es cero o un entero positivo, esto se hace referencia al índice del elemento actual y el cuerpo de solicitud de la llamada debe estar vacío. Sin embargo, si el valor de índice es "-1", debe especificarse el elemento que se va a mover ItemId o proveedor/ProviderID en el cuerpo de solicitud de la llamada. | 
  
<a id="ID4EHC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST](uri-usersxuidlistspinslistnameindexpost.md)

&nbsp;&nbsp;Mueve un elemento en una lista a una posición diferente en la lista.
 
<a id="ID4ERC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ETC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   