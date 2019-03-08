---
title: URI de listas
assetID: 84dcbd11-86a0-8a1e-7db9-bcecf9b7f853
permalink: en-us/docs/xboxlive/rest/atoc-reference-lists.html
description: " URI de listas"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a6f1e743542e70ee96ad93ee1cf2a7f2c3ed7158
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599330"
---
# <a name="lists-uris"></a>URI de listas
 
Esta sección proporciona información detallada sobre las direcciones de identificador de recursos Universal (URI) y los métodos de protocolo de transporte de hipertexto (HTTP) asociados desde servicios de Xbox Live para *PIN*.
 
Solo los juegos y aplicaciones que se ejecutan en una consola Xbox 360, un dispositivo Windows Phone, SmartGlass o Xbox.com pueden usar este servicio.
 
El dominio para estos URI es eplists.xboxlive.com.
 
<a id="ID4EPB"></a>

 
## <a name="in-this-section"></a>En esta sección

[/users/xuid(xuid)/lists/PINS/{listname}](uri-usersxuidlistspinslistname.md)

&nbsp;&nbsp;Tiene acceso a los elementos de una lista.

[/users/xuid(xuid)/lists/PINS/{listname}/ContainsItems](uri-usersxuidlistspinslistnamecontainsitems.md)

&nbsp;&nbsp;Determina si un conjunto de elementos (especificado por ID) se encuentran en una lista sin tener que recuperar toda la lista.

[/users/xuid(xuid)/lists/PINS/{listname}/index({index})?insertIndex={insertIndex}](uri-usersxuidlistspinslistnameindex.md)

&nbsp;&nbsp;Mueve un elemento dentro de una lista.

[/users/xuid(xuid)/lists/PINS/{listname}/RemoveItems](uri-usersxuidlistspinslistnameremoveitems.md)

&nbsp;&nbsp;Quita elementos de una lista.
 
<a id="ID4E5B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EAC"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de identificador (URI) de recursos universal](../atoc-xboxlivews-reference-uris.md)

   