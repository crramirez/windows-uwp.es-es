---
title: GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
assetID: 613ba53f-03cb-5ed3-a5ba-be59e5a146d1
permalink: en-us/docs/xboxlive/rest/uri-titlestitleidsessionssessionidallocationstatus-get.html
description: " GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 793d634bc1e3dc431b3797759751afb6dfd9c00a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650860"
---
# <a name="get-titlestitleidsessionssessionidallocationstatus"></a>GET (/titles/{titleId}/sessions/{sessionId}/allocationStatus)
Devuelve el estado de asignación de la sessionhost identificado por su Id. de sesión. Los dominios para estos URI son `gameserverds.xboxlive.com` y `gameserverms.xboxlive.com`.
 
  * [Encabezados de solicitud necesarios](#ID4E4)
  * [Encabezados de respuesta necesaria](#ID4EEB)
  * [Cuerpo de respuesta](#ID4ELB)
 
<a id="ID4E4"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
Ninguno.
  
<a id="ID4EEB"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
Ninguno.
  
<a id="ID4ELB"></a>

 
## <a name="response-body"></a>Cuerpo de respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | 
| description| Devuelve una cadena que se (izquierda en versiones anteriores compatibilidad) vacía.| 
| clusterId| Devuelve una cadena que se (izquierda en versiones anteriores compatibilidad) vacía.| 
| hostName| La dirección URL del host de sesión.| 
| status| Indica en la cola, ha realizado o anulado.| 
| sessionHostId| El identificador de host de sesión.| 
| sessionId| El cliente proporcionado (en el momento de la asignación) identificador de sesión.| 
| secureContext| La dirección del dispositivo seguro.| 
| portMappings| Las asignaciones de puerto para la instancia.| 
| Región| La ubicación de la instancia.| 
| ticketId| El identificador de la sesión actual (izquierdo en versiones anteriores compatibilidad).| 
| gameHostId| El sessionHostId actual (izquierda en versiones anteriores compatibilidad).| 
 
<a id="ID4EGD"></a>

 
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
        "status",:"Fulfilled",
        "region": "WestUS",
        "secureContext": "AQDc8Hen/QCDJwWRPcW/1QEEAiABAACdOJU8JNujcXyUPwUBCnue+g==",
        "sessionId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "description": "",
        "clusterId": "",
        "sessionHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0",
        "ticketId": "05328154-1bbe-4f5b-8caa-4e44106712f9",
        "gameHostId": "r111ybf4drgo12kq25tc-082yo7y9sz72f2odtq1ya5yhda-155169995-ncus.GSDKAgent_IN_0.0"

      
```

  
<a id="remarks"></a>

 
### <a name="remarks"></a>Observaciones
 
Un título sólo debe reintentar la llamada al servicio cuando se reciben los siguientes códigos de respuesta:
 
   * 200: correcto 
   * 400: solicitud contiene parámetros no válidos 
   * 401: no autorizado 
   * 404: no era válido o no se encuentra el Id. de título o el Id. de vale 
   * 500: error inesperado del servidor. 
    