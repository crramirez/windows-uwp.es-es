---
title: GET (/users/xuid({xuid})/history/titles)
assetID: c0a6cb3b-bab6-03b8-a79e-87defaaa6421
permalink: en-us/docs/xboxlive/rest/uri-titlehistoryusersxuidhistorytitlesgetv2.html
description: " GET (/users/xuid({xuid})/history/titles)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cadbf38385bbc321ef5bf23eb93c3fbc5c1a2417
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655590"
---
# <a name="get-usersxuidxuidhistorytitles"></a>GET (/users/xuid({xuid})/history/titles)
Obtiene una lista de títulos para los que el usuario ha desbloqueado o hizo avances en sus logros. Esta API no devuelve el historial completo de un usuario de títulos de reproducir o iniciado. Es el dominio para estos URI `achievements.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EY)
  * [Parámetros de cadena de consulta](#ID4EDB)
  * [Autorización](#ID4EFD)
  * [Encabezados de solicitud opcionales](#ID4EGE)
  * [Cuerpo de la solicitud](#ID4ERF)
 
<a id="ID4EY"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| entero sin signo de 64 bits| Id. de usuario de Xbox (XUID) del usuario cuyo historial de título se tiene acceso.| 
  
<a id="ID4EDB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Requerido| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| skipItems| No| entero de 32 bits con signo| Devolver elementos a partir de después del número especificado de elementos. Por ejemplo, <b>skipItems = "3"</b> recuperará los elementos recuperados a partir del cuarto elemento. | 
| continuationToken| No| string| Devolver los elementos comenzando en el token de continuación determinado. | 
| maxItems| No| entero de 32 bits con signo| Número máximo de elementos que se va a devolver de la colección, que se puede combinar con <b>skipItems</b> y <b>continuationToken</b> para devolver un intervalo de elementos. El servicio puede proporcionar un valor predeterminado si <b>maxItems</b> no está presente y es posible que devuelva menos de <b>maxItems</b>, incluso si aún no se ha devuelto la última página de resultados. | 
  
<a id="ID4EFD"></a>

 
## <a name="authorization"></a>Autorización
 
| Notificación| ¿Obligatorio?| Descripción| Comportamiento si falta| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Usuario| Autor de llamada es un usuario autorizado de Xbox LIVE.| El llamador debe ser un usuario válido en Xbox LIVE.| 403 Prohibido| 
  
<a id="ID4EGE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| <b>X-RequestedServiceVersion</b>| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera.| 
| <b>x-xbl-contract-version</b>| entero de 32 bits sin signo| Si está presente y establecido en 2, se usará la versión V2 de esta API. En caso contrario, V1.| 
  
<a id="ID4ERF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
Ningún objeto que se envía en el cuerpo de esta solicitud.
  
<a id="ID4EDG"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EFG"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/history/titles](uri-titlehistoryusersxuidhistorytitlesv2.md)

  
<a id="ID4EPG"></a>

 
##### <a name="reference"></a>Referencia 

[UserTitle (JSON)](../../json/json-usertitlev2.md)

 [PagingInfo (JSON)](../../json/json-paginginfo.md)

 [Parámetros de paginación](../../additional/pagingparameters.md)

   