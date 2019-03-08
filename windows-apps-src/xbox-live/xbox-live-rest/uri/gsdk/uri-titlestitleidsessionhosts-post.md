---
title: POST (/titles/{Title Id}/sessionhosts)
assetID: 8558b336-1af9-8143-9752-477ceb3a8e4e
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionhosts-post.html
description: " POST (/titles/{Title Id}/sessionhosts)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 47e3ecbf0a519b92ae467199e5d454523864310a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655150"
---
# <a name="post-titlestitle-idsessionhosts"></a>POST (/titles/{Title Id}/sessionhosts)
Crear nueva solicitud de clúster. Es el dominio para estos URI `gameserverms.xboxlive.com`.
 
  * [Parámetros de URI](#ID4EX)
  * [Encabezados de solicitud necesarios](#ID4EGB)
  * [Cuerpo de la solicitud](#ID4E5B)
  * [Encabezados de respuesta necesaria](#ID4ELD)
  * [Cuerpo de respuesta](#ID4ESD)
 
<a id="ID4EX"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Descripción| 
| --- | --- | 
| titleId| Id. del título que debe operar la solicitud.| 
  
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nombre de host

gameserverms.xboxlive.com
 
<a id="ID4EGB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
Al realizar una solicitud, los encabezados que se muestra en la tabla siguiente son necesarios.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | 
| Content-Type| application/json| Tipo de datos que se está enviadas.| 
  
<a id="ID4E5B"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
La solicitud debe contener un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | 
| sessionId| Esto es que el llamador especificado identificador. Se asigna al host de sesión que se asignan y se devuelve. Más adelante puede hacer referencia a la sessionhost específico por este identificador. Debe ser único globalmente (es decir, GUID).| 
| SandboxId| El recinto de seguridad que desea que se asignen en el host de sesión.| 
| cloudGameId| El identificador de juego en la nube.| 
| Ubicaciones| Desea que la sesión de la lista ordenada de las ubicaciones preferidas.| 
| sessionCookie| Se trata de un llamador especificado cadena opaca. Está asociado con el sessionhost y se puede hacer referencia en el código de juego. Utilice a este miembro para pasar una pequeña cantidad de información desde el cliente al servidor (el tamaño máximo es 4KB).| 
| gameModelId| El identificador del modo de juego.| 
 
<a id="ID4EDD"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
        "sessionId": "3536d3e6-e85d-4f47-b898-9617d19dabcd",
        "sandboxId": "ISST.0",
        "cloudGameId": "1b7f9925-369c-4301-b1f7-1125dce25776",
        "locations": [
        "West US",
        "East US",
        "West Europe"
        ],
        "sessionCookie": "Caller provided opaque string",
        "gameModeId": "2162d32c-7ac8-40e9-9b1f-56676b8b2513"
        }
      
```

   
<a id="ID4ELD"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
Ninguno.
  
<a id="ID4ESD"></a>

 
## <a name="response-body"></a>Cuerpo de respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| hostName| El nombre de la instancia de host.| 
| portMappings| Las asignaciones de puerto.| 
| Región| La instancia de la región se hospeda en.| 
| secureContext| La dirección del dispositivo seguro.| 
 
<a id="ID4ESE"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
        "hostName": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.cloudapp.net",
        "portMappings": [
        {
        "Key": "GSDKTCPTest",
        "Value": {
        "internal": 10100,
        "external": 10103
        }
        },
        {
        "Key": "GSDKUDPTest",
        "Value": {
        "internal": 5000,
        "external": 5000
        }
        }
        ],
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g=="
        }
      
```

   
<a id="remarks"></a>

 
## <a name="remarks"></a>Observaciones
 
Un título sólo debe reintentar la llamada al servicio cuando se reciben los siguientes códigos de respuesta:
 
   * 200: correcto: respuesta devuelta.
   * 400: parámetros no válidos o el cuerpo de solicitud con formato incorrecto.
   * 401: no autorizado
   * 404, Id. de título no tiene ninguna suscripción asignada a él.
   * 409: cuando se realiza la solicitud idéntico (mismo sessionId) en aproximadamente al mismo tiempo, es posible esta respuesta. Si se realiza una solicitud de asignación y un host de sesión ya tiene el Id. de sesión especificado ya está activo, se devuelven información detallada acerca de ese sessionhost. Si el host de sesión, no está activa todavía, recibirá un conflicto.
   * 500: error inesperado del servidor.
   * 503: ningún sessionhosts StandingBy. Vuelva a intentar la solicitud cuando algunos de estos recursos son gratuitas.
   
<a id="ID4EFG"></a>

 
## <a name="see-also"></a>Consulte también
 [/titles/{titleId}/sessionhosts](uri-titlestitleidsessionhosts.md)

  