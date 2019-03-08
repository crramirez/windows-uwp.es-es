---
title: /users/xuid(xuid)/lists/PINS/{listname}
assetID: b6421b11-fcd1-cfdb-c1fa-6cab3dab89d9
permalink: en-us/docs/xboxlive/rest/uri-usersxuidlistspinslistname.html
description: " /users/xuid(xuid)/lists/PINS/{listname}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f9610b400e9530f86e264cea30bfdfdd1b09c8d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628000"
---
# <a name="usersxuidxuidlistspinslistname"></a>/users/xuid(xuid)/lists/PINS/{listname}
Tiene acceso a los elementos de una lista. Es el dominio para estos URI `eplists.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| Id. de usuario de Xbox (XUID).| 
| ListType| string| Tipo de la lista (cómo se usa y cómo actúa). Siempre "ANCLA" para estos métodos relacionados.| 
| nombre de lista| string| Nombre de la lista (la lista de un determinado listtype para actuar sobre). Siempre "XBLPins" para los elementos de PIN.| 
  
<a id="ID4EGC"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[ELIMINAR](uri-usersxuidlistspinslistnamedelete.md)

&nbsp;&nbsp;Quita elementos de una lista.

[GET](uri-usersxuidlistspinslistnameget.md)

&nbsp;&nbsp;Devuelve el contenido de una lista.

[POST](uri-usersxuidlistspinslistnamepost.md)

&nbsp;&nbsp;Inserta elementos en la lista en el índice basado en el parámetro de cadena de consulta **insertIndex**.

[PUT](uri-usersxuidlistspinslistnameput.md)

&nbsp;&nbsp;Actualiza los elementos en una lista en función de los índices especificados para cada elemento en el cuerpo de solicitud.
 
<a id="ID4EZC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E2C"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   