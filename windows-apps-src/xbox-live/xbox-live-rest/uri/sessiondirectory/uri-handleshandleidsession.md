---
title: /handles/{handleId}/session
assetID: 4ed2dcf5-5d1f-91ce-4a3f-eb3ba68727bf
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsession.html
description: " /handles/{handleId}/session"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: e7b6990917437c22dd4d9282492e2a0eab37893b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628150"
---
# <a name="handleshandleidsession"></a>/handles/{handleId}/session
Admite operaciones GET y PUT para una sesión, utilizando la desreferenciación del identificador. 

> [!NOTE] 
> Este URI se usa por varios jugadores 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Se está diseñado para su uso con el contrato de la plantilla 104/105 o posterior.  

 

> [!NOTE] 
> Este URI actualmente solo es accesible desde el exterior, consolas Xbox One y servidores mediante un identificador de servicio.  

 
<a id="ID4ES"></a>

 
## <a name="domain"></a>Dominio
sessiondirectory.xboxlive.com  
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | 
| handleId| GUID| El identificador único del controlador de la sesión.| 
  
<a id="ID4ESB"></a>

 
## <a name="valid-methods"></a>Métodos válidos

[GET (/handles/{handleId}/session)](uri-handleshandleidsessionget.md)

&nbsp;&nbsp;Obtiene un objeto de sesión para el identificador del identificador especificado. 

[PUT (/handles/{handle-id}/session)](uri-handleshandleidsessionput.md)

&nbsp;&nbsp;Crea o actualiza una sesión de desreferenciar un identificador.
 
<a id="ID4E6B"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBC"></a>

 
##### <a name="parent"></a>Primario 

[URI del directorio de sesión](atoc-reference-sessiondirectory.md)

   