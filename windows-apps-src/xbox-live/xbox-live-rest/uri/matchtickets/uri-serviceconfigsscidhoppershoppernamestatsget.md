---
title: GET (/serviceconfigs/{scid}/hoppers/{name}/stats)
assetID: 4de5b07d-93e1-8ff0-05dd-1d3bb1802088
permalink: en-us/docs/xboxlive/rest/uri-serviceconfigsscidhoppershoppernamestatsget.html
description: " GET (/serviceconfigs/{scid}/hoppers/{name}/stats)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 95de95b35de496331dd3fe0a4c69f18e047c1020
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621700"
---
# <a name="get-serviceconfigsscidhoppersnamestats"></a>GET (/serviceconfigs/{scid}/hoppers/{name}/stats)

Obtiene las estadísticas para un hopper.

> [!IMPORTANT]
> Este método está pensado para su uso con el contrato 103 o posterior y requiere un elemento de encabezado de X-Xbl-contrato-Version: 103 o posterior en cada solicitud.

  * [Comentarios](#ID4ET)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Códigos de estado HTTP](#ID4E3C)
  * [Cuerpo de la solicitud](#ID4EFD)
  * [Cuerpo de respuesta](#ID4EQD)

<a id="ID4ET"></a>


## <a name="remarks"></a>Observaciones
Este método HTTP/REST Obtiene las estadísticas de la hopper con nombre en la configuración del servicio de nivel de Id. (¿SCID). Este método se puede ajustar mediante el **Microsoft.Xbox.Services.Matchmaking.MatchmakingService.GetHopperStatisticsAsync** API.  
<a id="ID4E5"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- |
| scid| GUID| El identificador de configuración de servicio (¿SCID) para la sesión.|
| name| string| El nombre de la hopper.|

<a id="ID4EJB"></a>


## <a name="authorization"></a>Autorización

| Tipo| Requerido| Descripción| Si falta de respuesta|
| --- | --- | --- | --- | --- | --- | --- | --- |
| XUID (user ID)| sí| El usuario que realiza la solicitud debe ser un miembro de la sesión de vale que hace referencia el vale. | 403|
| Privilegios y el tipo de dispositivo| sí| Cuando se establece el tipo de dispositivo del usuario a la consola, se permiten solo los usuarios con el privilegio para varios jugadores en sus notificaciones para realizar llamadas para el servicio. | 403|
| Id. de título y prueba del tipo de compra/dispositivo| sí| El título que se buscan en debe permitir que los contactos de la notificación de título especificado, la combinación del tipo de dispositivo. | 403|

<a id="ID4E3C"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP
El servicio devuelve un código de estado HTTP tal como se aplica a MPSD.  
<a id="ID4EFD"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Ningún objeto que se envía en el cuerpo de esta solicitud.

<a id="ID4EQD"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

| Miembro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| hopperName| string| El nombre de la hopper seleccionado.|
| waitTime| entero de 32 bits con signo| Promedio de tiempo para el hopper (un número entero de segundos) de coincidencia. |
| Rellenado| entero de 32 bits con signo| El número de personas esperando coincidencias en el hopper.|

<a id="ID4E1D"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
      "hopperName":"contosoawesome2",
      "waitTime":30,
      "population":1
    }


```


<a id="ID4EJE"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4ELE"></a>


##### <a name="parent"></a>Primario  

[/serviceconfigs/{scid}/hoppers/{name}/stats](uri-serviceconfigsscidhoppershoppernamestats.md)
