---
title: POST (/users/xuid({xuid})/resetreputation)
assetID: 3b76857f-b043-2c76-cf0c-c8f355fe3849
permalink: en-us/docs/xboxlive/rest/uri-usersxuidresetreputationpost.html
description: " POST (/users/xuid({xuid})/resetreputation)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2918249eaf359b383e89f24b8a37352bc3fe5132
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623530"
---
# <a name="post-usersxuidxuidresetreputation"></a>POST (/users/xuid({xuid})/resetreputation)
Permite al equipo de cumplimiento establecer puntuaciones de reputación del usuario especificado en algunos valores arbitrarios después un secuestro de cuenta (por ejemplo). Es el dominio para estos URI `reputation.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Encabezados de solicitud necesarios](#ID4E5B)
  * [Cuerpo de la solicitud](#ID4EYD)
  * [Códigos de estado HTTP](#ID4EOE)
  * [Cuerpo de respuesta](#ID4EQH)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
También puede llamar a este método cualquier otros asociados para todos los espacios aislados, excepto por menor y los usuarios en todos los espacios aislados, excepto la venta directa, con fines de prueba. Tenga en cuenta que esta solicitud se establece a un usuario "base" puntuaciones de reputación y sus ponderaciones de votos positivos se todos se pone a cero. Reputación real del usuario después de realizar esta llamada será estas puntuaciones bases más su ventaja de ambassador más su ventaja de seguidores.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| xuid| string| La consola Xbox usuario ID (XUID) del usuario especificado.| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Autorización
 
De asociado: El espacio aislado de venta directa, **PartnerClaim** desde el equipo de la aplicación; para todos los demás espacios aislados, **PartnerClaim**.
 
Usuario: Para todos los espacios aislados, excepto la venta directa, **XuidClaim** y **TitleClaim**.
  
<a id="ID4E5B"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
De todos: **Content-Type: application/json**.
 
De asociado: **X-Xbl-contrato-Version** (versión actual es 101), **Xbl-X-Sandbox**.
 
Usuario: **X-Xbl-contrato-Version** (versión actual es 101).
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
| X-RequestedServiceVersion|  | Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 101.| 
  
<a id="ID4EYD"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4E5D"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 
El cuerpo de solicitud es una sencilla [ResetReputation (JSON)](../../json/json-resetreputation.md) documento.
 

```cpp
{
    "fairplayReputation": 75,
    "commsReputation": 75,
    "userContentReputation": 75
}
      
```

   
<a id="ID4EOE"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| Aceptar.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 500| error interno del servidor| El servidor encontró una condición inesperada que le impidió satisfacer la solicitud.| 
| 503| servicio no disponible| Se ha limitado la solicitud, intente la solicitud de nuevo después del valor de reintento del cliente en segundos (por ejemplo, 5 segundos más tarde).| 
  
<a id="ID4EQH"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Si se ejecuta correctamente, el cuerpo de respuesta está vacío. En caso de error, un [depuracuión puede contener (JSON)](../../json/json-serviceerror.md) se devuelve el documento.
 
<a id="ID4E3H"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "code": 4000,
    "source": "ReputationFD",
    "description": "No targetXuid field was supplied in the request"
}
         
```

   
<a id="ID4EHAAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EJAAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/xuid({xuid})/resetreputation](uri-usersxuidresetreputation.md)

   