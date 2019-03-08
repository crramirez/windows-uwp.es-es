---
title: GET (/users/me)
assetID: 726c279b-73fb-02ea-cbff-700ff2dc31af
permalink: en-us/docs/xboxlive/rest/uri-usersmeget.html
description: " GET (/users/me)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: b06305fde989d0c30570beda5d4b0aabe7bf0518
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606730"
---
# <a name="get-usersme"></a>GET (/users/me)
Obtener el usuario actual [PresenceRecord](../../json/json-presencerecord.md) sin tener que conocer XUID del usuario.
Es el dominio para estos URI `userpresence.xboxlive.com`.

  * [Parámetros de cadena de consulta](#ID4EZ)
  * [Autorización](#ID4EIC)
  * [Encabezados de solicitud necesarios](#ID4ELD)
  * [Encabezados de solicitud opcionales](#ID4EPF)
  * [Cuerpo de la solicitud](#ID4EPG)
  * [Cuerpo de respuesta](#ID4E1G)

<a id="ID4EZ"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| level| string| Opcional. <ul><li><b>user</b>: Devuelve solo el nodo de usuario.</li><li><b>device</b>: Devuelve los nodos del nodo y el dispositivo de usuario.</li><li><b>title</b>: Default. Devuelve la totalidad del árbol, excepto la actividad.</li><li><b>all</b>: Devuelve la totalidad del árbol, incluida la presencia de nivel de actividad.</li></ul> | 

<a id="ID4EIC"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- |
| XUID| Sí| Id. de usuario de Xbox (XUID) del llamador| 403 Prohibido|

<a id="ID4ELD"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.|
| Aceptar| string| Tipos de contenido que son aceptables. El único admitido por la presencia es application/json, pero se debe especificar en el encabezado.|
| Aceptar idioma| string| Configuración regional aceptable para las cadenas en la respuesta. Valores de ejemplo: en-US.|
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: presencebeta.xboxlive.com.|

<a id="ID4EPF"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.|

<a id="ID4EPG"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4E1G"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EAH"></a>


### <a name="sample-response"></a>Respuesta de ejemplo

Este método devuelve un [PresenceRecord](../../json/json-presencerecord.md).


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


<a id="ID4EQH"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ESH"></a>


##### <a name="parent"></a>Primario

[/users/me](uri-usersme.md)
