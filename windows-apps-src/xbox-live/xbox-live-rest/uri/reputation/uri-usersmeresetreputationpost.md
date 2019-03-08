---
title: POST (/users/me/resetreputation)
assetID: 1a4fed45-f608-dac2-3384-2ee493112f7b
permalink: en-us/docs/xboxlive/rest/uri-usersmeresetreputationpost.html
description: " POST (/users/me/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fc770673ac7e4034004f58032c7fa66cb28413e7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632690"
---
# <a name="post-usersmeresetreputation"></a>POST (/users/me/resetreputation)
Permite al equipo de cumplimiento establecer puntuaciones de reputación del usuario actual con algunos valores arbitrarios después un secuestro de cuenta (por ejemplo). Es el dominio para estos URI `reputation.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Autorización](#ID4E5)
  * [Encabezados de solicitud necesarios](#ID4ETB)
  * [Cuerpo de la solicitud](#ID4END)
  * [Códigos de estado HTTP](#ID4EDE)
  * [Cuerpo de respuesta](#ID4EFH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
También puede llamar a este método cualquier otros asociados para todos los espacios aislados, excepto por menor y los usuarios en todos los espacios aislados, excepto la venta directa, con fines de prueba. Tenga en cuenta que esta solicitud se establece a un usuario "base" puntuaciones de reputación y sus ponderaciones de votos positivos se todos se pone a cero. Reputación real del usuario después de realizar esta llamada será estas puntuaciones bases más su ventaja de ambassador más su ventaja de seguidores.
  
<a id="ID4E5"></a>

 
## <a name="authorization"></a>Autorización
 
De asociado: El espacio aislado de venta directa, **PartnerClaim** desde el equipo de la aplicación; para todos los demás espacios aislados, **PartnerClaim**.
 
Usuario: Para todos los espacios aislados, excepto la venta directa, **XuidClaim** y **TitleClaim**.
  
<a id="ID4ETB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
De todos: **Content-Type: application/json**.
 
De asociado: **X-Xbl-contrato-Version** (versión actual es 101), **Xbl-X-Sandbox**.
 
Usuario: **X-Xbl-contrato-Version** (versión actual es 101).
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 101.| 
  
<a id="ID4END"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4ETD"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 
El cuerpo de solicitud es una sencilla [ResetReputation (JSON)](../../json/json-resetreputation.md) documento.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EDE"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| Aceptar.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EFH"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si se ejecuta correctamente, el cuerpo de respuesta está vacío. En caso de error, un [depuracuión puede contener (JSON)](../../json/json-serviceerror.md) se devuelve el documento.
 
<a id="ID4ERH"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4E2H"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E4H"></a>

 
##### <a name="parent"></a>Primario 

[/users/me/resetreputation](uri-usersmeresetreputation.md)

   