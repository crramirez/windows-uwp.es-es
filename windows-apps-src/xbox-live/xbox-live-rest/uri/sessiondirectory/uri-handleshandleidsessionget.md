---
title: GET (/handles/{handleId}/session)
assetID: 1f22954c-e77b-69c2-63f4-741fbd965f8f
permalink: en-us/docs/xboxlive/rest/uri-handleshandleidsessionget.html
description: " GET (/handles/{handleId}/session)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 41911dc53b316f4f323b9859d9101581ec88e497
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593900"
---
# <a name="get-handleshandleidsession"></a>GET (/handles/{handleId}/session)
Obtiene un objeto de sesión para el identificador del identificador especificado.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EDB)
  * [Códigos de estado HTTP](#ID4EOB)
  * [Cuerpo de la solicitud](#ID4EVB)
  * [Cuerpo de respuesta](#ID4E6B)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST recupera un objeto de sesión desde el servidor, mediante el puntero del servicio proporcionado a la sesión (identificador). El valor devuelto es el objeto de sesión, con todos sus atributos. Este método se puede ajustar mediante **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.GetCurrentSessionByHandleAsync**.

El llamador de este método obtiene el Id. del identificador de un reproductor **MultiplayerActivityDetails** objeto. Como alternativa, el autor de la llamada Obtiene el identificador de activación de un protocolo después de que un usuario ha aceptado una invitación para jugar.

<a id="ID4EDB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| handleId| GUID| El identificador único del controlador de la sesión.|

<a id="ID4EOB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E6B"></a>


## <a name="response-body"></a>Cuerpo de la respuesta
Ver la estructura de respuesta en [MultiplayerSession (JSON)](../../json/json-multiplayersession.md).  
<a id="ID4EIC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EKC"></a>


##### <a name="parent"></a>Primario

[/handles/{handleId}/session](uri-handleshandleidsession.md)
