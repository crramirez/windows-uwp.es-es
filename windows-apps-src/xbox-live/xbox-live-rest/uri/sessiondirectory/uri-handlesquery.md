---
title: /handles/query
assetID: e00d31ad-b9ba-8e52-1333-83192eab0446
permalink: en-us/docs/xboxlive/rest/uri-handlesquery.html
description: " /handles/query"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: eaa148972ce1e65056470a6c4082cb4e50de3f09
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598570"
---
# <a name="handlesquery"></a>/handles/query
Admite operaciones POST para crear consultas para los identificadores de sesión. 

> [!NOTE] 
> Este URI se usa por varios jugadores 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Se está diseñado para su uso con el contrato de la plantilla 104/105 o posterior.  

 
<a id="ID4EQ"></a>

 
## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
Este URI es compatible con las consultas para los identificadores. A diferencia de las consultas de sesión, que son consultas de cadena y por lotes, consultas de identificador de utilizan un estilo de procesador de consultas. Se admiten hasta 100 identificadores.  
<a id="ID4E2"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
Ninguno   
<a id="ID4EEB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[POST (/handles/query)](uri-handlesquerypost.md)

&nbsp;&nbsp;Crea consultas para los identificadores de sesión.

[POST (/handles/query?include=relatedInfo)](uri-handlesqueryincludepost.md)

&nbsp;&nbsp;Crea consultas para los identificadores de sesión que incluyen información de la sesión relacionadas.
 
<a id="ID4EQB"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ESB"></a>

 
##### <a name="parent"></a>Primario 

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)

   