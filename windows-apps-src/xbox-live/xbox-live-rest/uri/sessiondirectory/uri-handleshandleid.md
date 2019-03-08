---
title: /handles/{handleId}
assetID: 5b722d3e-fe80-fec5-a26b-8b3db6422004
permalink: en-us/docs/xboxlive/rest/uri-handleshandleid.html
description: " /handles/{handleId}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a312d3744e96755a899d73307a47c01e3dc79fd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632640"
---
# <a name="handleshandleid"></a>/handles/{handleId}
Admite operaciones DELETE y GET para los identificadores de sesión especificados por identificador. 

> [!NOTE] 
> Este URI se usa por varios jugadores 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Se está diseñado para su uso con el contrato de la plantilla 104/105 o posterior.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | 
| handleId| GUID| El identificador único del controlador de la sesión.| 
  
<a id="ID4ERB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[DELETE (/handles/{handleId})](uri-handleshandleiddelete.md)

&nbsp;&nbsp;Elimina identificadores especificados por el identificador de controlador.

[GET (/handles/{handle-id})](uri-handleshandleidget.md)

&nbsp;&nbsp;Recupera los identificadores especificados por el identificador de controlador.
 
<a id="ID4E4B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E6B"></a>

 
##### <a name="parent"></a>Primario 

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)

   