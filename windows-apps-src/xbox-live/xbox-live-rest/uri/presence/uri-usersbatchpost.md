---
title: POST (/users/batch)
assetID: bd0b18fe-8a6d-d591-5b13-bcd9643e945a
permalink: en-us/docs/xboxlive/rest/uri-usersbatchpost.html
description: " POST (/users/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47376338a1c515aa00a7e0247df4cc16ee0db8d2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655680"
---
# <a name="post-usersbatch"></a>POST (/users/batch)
Obtener la presencia de un lote de usuarios.
Es el dominio para estos URI `userpresence.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Autorización](#ID4EAB)
  * [Efecto de la configuración de privacidad en recurso](#ID4EDC)
  * [Encabezados de solicitud necesarios](#ID4EYF)
  * [Encabezados de solicitud opcionales](#ID4EGAAC)
  * [Cuerpo de la solicitud](#ID4EGBAC)
  * [Cuerpo de respuesta](#ID4ESEAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Este método se debe utilizar por cualquier cliente, el servicio o el título que quieran aprender información de presencia de un lote de usuarios.

Las respuestas para esta solicitud por lotes pueden ser filtros por la profundidad y la ruta de acceso. Los consumidores pueden usar esto para obtener y mostrar la presencia sobre un conjunto de usuarios. Los filtros en esta API funcionan como ORs en una propiedad, pero ANDs en Propiedades.

<a id="ID4EAB"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- |
| XUID| Sí| Id. de usuario de Xbox (XUID) del llamador| 403 Prohibido|

<a id="ID4EDC"></a>


## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso

Este método siempre devuelve 200 OK, pero no puede devolver el contenido en el cuerpo de respuesta.

| Solicitud de usuario| Configuración de privacidad del usuario de destino| Comportamiento|
| --- | --- | --- | --- | --- | --- | --- |
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

<a id="ID4EYF"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| x-xbl-contract-version| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valores de ejemplo: 3, vnext.|
| Aceptar| string| Tipos de contenido que son aceptables. El único admitido por la presencia es application/json, pero se debe especificar en el encabezado.|
| Aceptar idioma| string| Configuración regional aceptable para las cadenas en la respuesta. Valores de ejemplo: en-US.|
| Host| string| Nombre de dominio del servidor. Valor de ejemplo: presencebeta.xboxlive.com.|
| Content-Length| string| Longitud del cuerpo de la solicitud. Valor de ejemplo: 312.|

<a id="ID4EGAAC"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.|

<a id="ID4EGBAC"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

<a id="ID4EMBAC"></a>


### <a name="required-members"></a>Miembros requeridos

| Miembro| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| usuarios| XUIDs de lista de usuarios cuya presencia desea aprender, con un máximo de XUIDs 1100 a la vez.|

<a id="ID4EHCAC"></a>


### <a name="optional-members"></a>Miembros opcionales

| Miembro| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| deviceTypes| Lista de tipos de dispositivos utilizado por los usuarios que desea conocer. Si la matriz se deja vacía, el valor predeterminado es todos los posibles tipos de dispositivos (es decir, no se filtran).|
| titles| Lista de dispositivos cuyos usuarios desea saber acerca de los tipos. Si la matriz se deja vacía, el valor predeterminado es todos los títulos de posibles (es decir, no se filtran).|
| level| Posibles valores: <ul><li>usuario - get usuario nodos</li><li>dispositivo - usuario get y nodos de dispositivo</li><li>título: obtener información de nivel básico de título</li><li>todo: obtener información de presencia enriquecida, la información multimedia o ambos</li></ul><br> El valor predeterminado es "title".|
| onlineOnly| Si esta propiedad es true, la operación por lotes filtrará los registros para los usuarios sin conexión (incluidos los ocultos). Si no se proporciona, se devolverán los usuarios en línea y sin conexión.|

<a id="ID4E4DAC"></a>


### <a name="prohibited-members"></a>Miembros prohibidos

Todos los demás miembros están prohibidas en una solicitud.

<a id="ID4EIEAC"></a>


### <a name="sample-request"></a>Solicitud de ejemplo


```cpp
{
  users:
  [
    "1234567890",
    "4567890123",
    "7890123456"
  ]
}

```


<a id="ID4ESEAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4E1EAC"></a>


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


<a id="ID4EKFAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EMFAC"></a>


##### <a name="parent"></a>Primario

[/users/batch](uri-usersbatch.md)
