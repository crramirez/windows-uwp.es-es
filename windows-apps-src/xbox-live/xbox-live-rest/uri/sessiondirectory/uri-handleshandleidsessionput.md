---
title: PUT (/handles/{handle-id}/session)
assetID: d8a3d473-1192-ec0c-3935-c301f4f61e03
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionput.html
description: " PUT (/handles/{handle-id}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3a1872857d8b8e692f67e3c7b2a067ae86663c00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641610"
---
# <a name="put-handleshandle-idsession"></a>PUT (/handles/{handle-id}/session)
Crea o actualiza una sesión de desreferenciar un identificador.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4ECB)
  * [Códigos de estado HTTP](#ID4ENB)
  * [Cuerpo de la solicitud](#ID4EUB)
  * [Cuerpo de respuesta](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST escribe una sesión nueva o actualizada en el servicio para varios jugadores, con el identificador de identificador de sesión proporcionado. El resultado es un objeto que representa la sesión nueva o actualizada que se devuelve desde el servidor. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.WriteSessionByHandleAsync**.

El llamador de este método obtiene el Id. del identificador de un reproductor **MultiplayerActivityDetails** objeto. Como alternativa, el autor de la llamada Obtiene el identificador de activación de un protocolo después de que un usuario ha aceptado una invitación para jugar.

<a id="ID4ECB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| handleId| GUID| El identificador único del controlador de la sesión.|

<a id="ID4ENB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EUB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Ningún objeto que se envía en el cuerpo de la respuesta.

<a id="ID4EKC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EMC"></a>


##### <a name="parent"></a>Primario

[/handles/{handleId}/session](uri-handleshandleidsession.md)
