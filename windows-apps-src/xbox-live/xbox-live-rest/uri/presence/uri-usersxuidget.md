---
title: GET (/users/xuid({xuid}))
assetID: c97ef943-8bea-8a41-90d7-faea874284c8
permalink: en-us/docs/xboxlive/rest/uri-usersxuidget.html
description: " GET (/users/xuid({xuid}))"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 5fcc5d3b6a172eccab0656da39e6896b4df50840
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650930"
---
# <a name="get-usersxuidxuid"></a>GET (/users/xuid({xuid}))
Detectar la presencia de otro usuario o cliente.
Es el dominio para estos URI `userpresence.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EDB)
  * [Parámetros de cadena de consulta](#ID4EOB)
  * [Autorización](#ID4E4C)
  * [Efecto de la configuración de privacidad en recurso](#ID4EAE)
  * [Encabezados de solicitud necesarios](#ID4EVH)
  * [Encabezados de solicitud opcionales](#ID4E1BAC)
  * [Cuerpo de la solicitud](#ID4E1CAC)
  * [Cuerpo de respuesta](#ID4EFDAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Se puede filtrar la respuesta para proporcionar parte de la [PresenceRecord](../../json/json-presencerecord.md) si el consumidor no está interesado en todo el objeto.

> [!NOTE] 
> Los datos devueltos está restringidos por las reglas de aislamiento de privacidad y el contenido.



<a id="ID4EDB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario de destino.|

<a id="ID4EOB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- |
| level| string| Opcional. <ul><li><b>user</b>: Devuelve solo el nodo de usuario.</li><li><b>device</b>: Devuelve los nodos del nodo y el dispositivo de usuario.</li><li><b>title</b>: Default. Devuelve la totalidad del árbol, excepto la actividad.</li><li><b>all</b>: Devuelve la totalidad del árbol, incluida la presencia de nivel de actividad.</li></ul> |

<a id="ID4E4C"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| XUID| Sí| Id. de usuario de Xbox (XUID) del llamador| 403 Prohibido|

<a id="ID4EAE"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Este método siempre devuelve 200 OK, pero no puede devolver el contenido en el cuerpo de respuesta.

| Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Me| -| 200 correctos|
| Friend| Todo el mundo| 200 correctos|
| Friend| solo los amigos| 200 correctos|
| Friend| bloqueado| 200 correctos|
| usuario de no confianza| Todo el mundo| 200 correctos|
| usuario de no confianza| solo los amigos| 200 correctos|
| usuario de no confianza| bloqueado| 200 correctos|
| sitio de terceros| Todo el mundo| 200 correctos|
| sitio de terceros| solo los amigos| 200 correctos|
| sitio de terceros| bloqueado| 200 correctos|

<a id="ID4EVH"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.|
| Aceptar| string| Tipos de contenido que son aceptables. El único admitido por la presencia es application/json, pero se debe especificar en el encabezado.|
| Aceptar idioma| string| Configuración regional aceptable para las cadenas en la respuesta. Valores de ejemplo: en-US.|
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: presencebeta.xboxlive.com.|

<a id="ID4E1BAC"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.|

<a id="ID4E1CAC"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4EFDAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4ELDAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo

Si no hay ningún registro existente para el usuario, se devuelve un registro con ningún dispositivo.


```cpp
{
  xuid:"0123456789",
  state:"online",
  devices:
  [{
    type:"D",
    titles:
    [{
      id:"12341234",
      name:"Contoso 5",
      state:"active",
      placement:"fill",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Team Deathmatch on Nirvana"
      }
    },
    {
      id:"12341235",
      name:"Contoso Waypoint",
      timestamp:"2012-09-17T07:15:23.4930000",
      placement:"snapped",
      state:"active",
      activity:
      {
        richPresence:"Using radar"
      }
    }]
  },
  {
    type:W8,
    titles:
    [{
      id:"23452345",
      name:"Contoso Gamehelp",
      state:"active",
      placement:"full",
      timestamp:"2012-09-17T07:15:23.4930000",
      activity:
      {
        richPresence:"Nirvana page",
      }
    }]
  }]
}

```


<a id="ID4EXDAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EZDAC"></a>


##### <a name="parent"></a>Primario

[/users/xuid({xuid})](uri-usersxuid.md)
