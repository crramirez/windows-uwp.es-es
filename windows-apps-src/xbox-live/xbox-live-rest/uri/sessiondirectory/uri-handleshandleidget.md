---
title: GET (/handles/{handle-id})
assetID: c95b5ab5-d56a-f70d-20d8-afb48d122ccd
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidget.html
description: " GET (/handles/{handle-id})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 501d36f4d1ac079af15d6bb7f35a90d5328fc8db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598470"
---
# <a name="get-handleshandle-id"></a>GET (/handles/{handle-id})
Recupera los identificadores especificados por el identificador de controlador.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EDB)
  * [Códigos de estado HTTP](#ID4EOB)
  * [Cuerpo de la solicitud](#ID4EUB)
  * [Cuerpo de respuesta](#ID4E5B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST Obtiene la actividad actual de los usuarios de la sesión, para los identificadores especificados. El valor devuelto es el objeto de sesión, con todos sus atributos. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

El llamador de este método obtiene el Id. del identificador de un reproductor **MultiplayerActivityDetails** objeto. Como alternativa, el autor de la llamada Obtiene el identificador de activación de un protocolo después de que un usuario ha aceptado una invitación para jugar.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| handleId| GUID| El identificador único del controlador de la sesión.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E5B"></a>


## <a name="response-body"></a>Cuerpo de la respuesta
Ver la estructura de respuesta en [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EKC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EMC"></a>


##### <a name="parent"></a>Primario

[/handles/{handleId}](uri-handleshandleid.md)
