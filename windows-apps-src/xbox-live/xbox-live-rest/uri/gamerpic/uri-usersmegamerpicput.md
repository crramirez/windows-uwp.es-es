---
title: PUT (/users/me/gamerpic)
assetID: ddf71c62-197d-a81d-35a7-47c6dc9e1b0c
permalink: en-us/docs/xboxlive/rest/uri-usersmegamerpicput.html
description: " PUT (/users/me/gamerpic)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7aedc7cbd8366c9cb8d3a60e2cb1f5e843b24a8a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57661660"
---
# <a name="put-usersmegamerpic"></a>PUT (/users/me/gamerpic)
Carga un gamerpic 1080 x 1080. 
  * [Cuerpo de la solicitud](#ID4EQ)
  * [Códigos de estado HTTP](#ID4EZ)
  * [Cuerpo de respuesta](#ID4EXC)
 
<a id="ID4EQ"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
El cuerpo de solicitud es un gamerpic (archivo PNG de 1080 x 1080).
  
<a id="ID4EZ"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | 
| 200| Aceptar| GET correcta.| 
| 201| Creado.| Carga fue correcta.| 
| 403| prohibido| Se ha revocado el privilegio.| 
| 500| Error| Se ha producido un problema.| 
  
<a id="ID4EXC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Ningún objeto que se envía en el cuerpo de la respuesta.
  
<a id="ID4ECD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EED"></a>

 
##### <a name="parent"></a>Primario 

[/users/me/gamerpic](uri-usersmegamerpic.md)

   