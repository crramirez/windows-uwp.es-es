---
title: GET (/qosservers)
assetID: 8b940c1b-947c-eab3-78ed-4384f57ea0bd
permalink: en-us/docs/xboxlive/rest/uri-qosservers-get.html
description: " GET (/qosservers)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 02d24dbf1d189b759784dbbfa7052e2c218ec27e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632070"
---
# <a name="get-qosservers"></a>GET (/qosservers)
URI que se llama a un cliente para obtener la lista de servidores de calidad de servicio disponibles para su uso con Xbox Live Compute. Los dominios para estos URI son `gameserverds.xboxlive.com` y `gameserverms.xboxlive.com`.
 
  * [Encabezados de solicitud necesarios](#ID4EBB)
  * [Encabezados de respuesta necesaria](#ID4EUC)
  * [Cuerpo de respuesta](#ID4EVD)
 
<a id="ID5EG"></a>

 
## <a name="host-name"></a>Nombre de host

gameserverds.xboxlive.com
 
<a id="ID4EBB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
Al realizar una solicitud, los encabezados que se muestra en la tabla siguiente son necesarios.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | 
| Content-Type| application/json| Tipo de datos que se está enviadas.| 
| Host| gameserverds.xboxlive.com|  | 
| Content-Length|  | Longitud del objeto de solicitud.| 
| x-xbl-contract-version| 1| Versión del contrato de API.| 
  
<a id="ID4EUC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
Una respuesta siempre incluirá los encabezados que se muestra en la tabla siguiente.
 
| Encabezado| Valor| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Content-Type| application/json| Tipo de datos en el cuerpo de respuesta.| 
| Content-Length|  | Longitud del cuerpo de respuesta.| 
  
<a id="ID4EVD"></a>

 
## <a name="response-body"></a>Cuerpo de respuesta
 
Si la llamada se realiza correctamente, el servicio devolverá un objeto JSON con los siguientes miembros.
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| qosservers| Una matriz de información del servidor.| 
| serverFqdn| El nombre de dominio completo del servidor.| 
| serverSecureDeviceAddress| La dirección del dispositivo segura del servidor.| 
| targetLocation| La ubicación geográfica del servidor.| 
 
<a id="ID4EUE"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{ 
  "qosServers" : 
  [ 
    { "serverFqdn" : "xblqosncus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "North Central US" },
    { "serverFqdn" : "xblqoswus.cloudapp.net", "serverSecureDeviceAddress" : "&lt;base-64 encoded blob>", "targetLocation" : "West US" },
  ]
}

      
```

   
<a id="ID4EBF"></a>

 
## <a name="see-also"></a>Consulte también
 [/qosservers](uri-qosservers.md)

  