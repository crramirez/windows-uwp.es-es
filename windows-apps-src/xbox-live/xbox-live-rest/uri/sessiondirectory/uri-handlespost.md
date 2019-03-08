---
title: POST (/handles)
assetID: 21f3e289-0b0e-2731-befb-bd4c0d71973e
permalink: en-us/docs/xboxlive/rest/uri-handlespost.html
description: " POST (/handles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ed3482b8e629749d294ed25944db16372cc7fee6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594750"
---
# <a name="post-handles"></a>POST (/handles)
Establece la sesión de varios jugadores de la actividad del usuario actual e invita a los miembros de la sesión si es necesario.

> [!IMPORTANT]
> Este método usa la multijugador 2015 y se aplica solo a esa versión multijugador y versiones posteriores. Está pensado para su uso con el contrato de la plantilla 104/105 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 105/104 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4EHB)
  * [Códigos de estado HTTP](#ID4EPB)
  * [Cuerpo de la solicitud](#ID4EVB)
  * [Cuerpo de respuesta](#ID4EJC)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones

Este método HTTP/REST puede usarse para establecer la sesión para la actividad actual. En este caso, el método puede ir entre **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SetActivityAsync**. El cuerpo de solicitud debe definir la sesión de referencia, mediante el **sessionRef** objeto en el archivo JSON, con el campo de tipo a la "actividad". No se recupera ningún cuerpo de respuesta. Para obtener definiciones de los elementos especificados en una referencia de la sesión, vea **Microsoft.Xbox.Services.Multiplayer.MultiplayerSessionReference**.

También se puede usar este método POST para invitar a usuarios especificados por los controladores para una sesión. En este caso, el método puede ir entre **Microsoft.Xbox.Services.Multiplayer.MultiplayerService.SendInvitesAsync**. Este uso del método POST requiere el cuerpo de la solicitud para definir la referencia de la sesión, pero con el tipo de campo establecido en "Invitar". El cuerpo de respuesta es un identificador de invitación.

<a id="ID4EHB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

Ninguno

<a id="ID4EPB"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EVB"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

<a id="ID4E1B"></a>


### <a name="request-body-for-setting-activity"></a>Cuerpo para la configuración de la actividad de la solicitud


```cpp
{
  "version": 1,
  "type": "activity",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    // The session name is optional in a POST; if not specified, MPSD fills in a GUID.//
    "name": "session-49"
  },
}

```


<a id="ID4EBC"></a>


### <a name="request-body-for-sending-invites"></a>Cuerpo de la solicitud para enviar invitaciones


```cpp
{
  // Common handle fields
  "id: "47ca0049-a5ba-4bc1-aa22-fcf834ce4c13",
  "version": 1,
  "type": "invite",
  "sessionRef": {
    "scid": "bd6c41c3-01c3-468a-a3b5-3e0fe8133862",
    "templateName": "deathmatch",
    "name": "session-49"
   },
   "inviteAttributes": {
     "titleId" : {titleId}, // The title being invited to, in decimal uint32. This value is used to find the title name and/or image.
     "context" : {context}, // The title defined context token. Must be 256 characters or less when URI-encoded.
     "contextString" : {contextstring}, // The string name of a custom invite string to display in the invite notification.
     "senderString" : {sender} // The string name of the sender when the sender is a service.
   },
   "invitedXuid": "3210",
}

```


<a id="ID4EJC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EOC"></a>


### <a name="response-body-for-setting-activity"></a>Cuerpo de respuesta para la configuración de la actividad
Ninguno.  
<a id="ID4ESC"></a>


### <a name="response-body-for-sending-invites"></a>Cuerpo de respuesta para enviar invitaciones
Un identificador de invitación.   
<a id="ID4EXC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EZC"></a>


##### <a name="parent"></a>Primario

[/handles](uri-handles.md)
