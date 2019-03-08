---
title: DELETE (/handles/{handleId})
assetID: 71cceff4-3a74-dc05-12db-cfe3f20a8aea
permalink: en-us/docs/xboxlive/rest/uri-handleshandleiddelete.html
description: " DELETE (/handles/{handleId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 354f3563c48139edc5d5cc041e8304998af55620
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613410"
---
# <a name="delete-handleshandleid"></a>DELETE (/handles/{handleId})
Elimina identificadores especificados por el identificador de controlador.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EAB)
  * [Códigos de estado HTTP](#ID4ELB)
  * [Cuerpo de la solicitud](#ID4ESB)
  * [Cuerpo de respuesta](#ID4E4B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones
Este método HTTP/REST elimina los identificadores para el identificador especificado y borra la actividad del usuario actual para la sesión. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.ClearActivityAsync**.  
<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| handleId| GUID| El identificador único del controlador de la sesión.|

<a id="ID4ELB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4ESB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E4B"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ningún objeto que se envía en el cuerpo de la respuesta.

<a id="ID4EIC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EKC"></a>


##### <a name="parent"></a>Primario

[/handles/{handleId}](uri-handleshandleid.md)
