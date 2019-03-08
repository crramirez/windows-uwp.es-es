---
title: DeviceEndpoint (JSON)
assetID: bd6c4af8-e491-8885-970e-e53d1d60642b
permalink: en-us/docs/xboxlive/rest/json-deviceendpoint.html
description: " DeviceEndpoint (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0eaa21072ebf14b6f6d959ff40af34724a45522f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618880"
---
# <a name="deviceendpoint-json"></a>DeviceEndpoint (JSON)
 
<a id="ID4EO"></a>

 
## <a name="deviceendpoint"></a>DeviceEndpoint
 
El objeto DeviceEndpoint tiene la especificación siguiente.
 
| Miembro| Tipo| Descripción| 
| --- | --- | --- | 
| deviceName| string| Opcional. Un nombre descriptivo para el dispositivo, si procede. Actualmente no se utiliza este valor.| 
| endpointUri| string| Obligatorio. La dirección URL que ha obtenido la plataforma de cliente (Windows o Windows Phone) de su servicio de notificaciones push (WNS o MPNS).| 
| configuración regional| string| Obligatorio. El idioma deseado de las notificaciones enviadas a este punto de conexión. Puede ser una lista de valores separados por comas en el orden de preferencia. Ejemplo: "de-DE, en-US, en".| 
| Plataforma| string| Opcional. Los valores admitidos actualmente son "Windows Phone" y "Windows". Si no se especifica, se deriva el token del dispositivo.| 
| platformVersion| string| Opcional. El formato de esta cadena es específico de cada plataforma. Actualmente no se utiliza este valor.| 
| systemId| GUID| Obligatorio. Identificador único para la instancia de"app" (combinación usuario/dispositivo). Implementación de prácticas recomendada para una aplicación generar un GUID aleatorio tras la instalación y primera ejecución y seguir usando ese valor en las ejecuciones posteriores de la aplicación.| 
| titleId| entero de 32 bits sin signo| Obligatorio. El identificador del título del juego emitir la llamada al servicio.| 
  
<a id="ID4EGD"></a>

 
## <a name="sample-json-syntax"></a>Sintaxis de JSON de ejemplo
 

```json
{
  "systemId": "e9af05b4-70de-4920-880f-026c6fb67d1b",
  "userId" : 1234567890
  "endpointUri": "https://cloud.notify.windows.com/.../",
  "platform": "Windows",
  "platformVersion": "1.0",
  "deviceName": "Test Endpoint",
  "locale": "en-US",
  "titleId": 1297290219
}
    
```

  
<a id="ID4EPD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ERD"></a>

 
##### <a name="parent"></a>Primario 

[Referencia de objeto de objeto de JavaScript Notation (JSON)](atoc-xboxlivews-reference-json.md)

  
<a id="ID4E4D"></a>

 
##### <a name="reference"></a>Referencia   