---
title: POST (/batch)
assetID: f5997c8e-82c7-0fba-9991-ce1116db4830
permalink: en-us/docs/xboxlive/rest/uri-batchpost.html
description: " POST (/batch)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: a854fc830c87afbf675a379599916bf3db919539
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645840"
---
# <a name="post-batch"></a>POST (/batch)
Método que funciona como un método GET para las solicitudes por lotes complejos para varias estadísticas del Reproductor a través de varios títulos de POST. Es el dominio para estos URI `userstats.xboxlive.com`.
 
<a id="ID4ET"></a>

 
## <a name="remarks"></a>Observaciones
 
Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.
  
  * [Comentarios](#ID4ET)
  * [Comentarios](#ID4EFB)
  * [Autorización](#ID4EUB)
  * [Encabezados de solicitud necesarios](#ID4ETC)
  * [Encabezados de solicitud opcionales](#ID4E3D)
  * [Cuerpo de la solicitud](#ID4EAF)
  * [Códigos de estado HTTP](#ID4EWF)
  * [Cuerpo de respuesta](#ID4ENBAC)
 
<a id="ID4EFB"></a>

 
## <a name="remarks"></a>Observaciones
 
El autor de la llamada proporciona un cuerpo de mensaje con una matriz de los usuarios, identificadores (SCIDs) configuración del servicio y una lista de nombres de estadística por SCIDs para que se va a recuperar esas estadísticas.
 
Le resultará más útil para revisar el simple, la única estadística [obtener](uri-usersxuidscidsscidstatsget.md) método antes de leer esta página más compleja, de modo por lotes.
  
<a id="ID4EUB"></a>

 
## <a name="authorization"></a>Autorización
 
No hay lógica de autorización que se implementa para escenarios de aislamiento de contenido y control de acceso.
 
   * Las estadísticas de tablas de clasificación y usuario se pueden leer desde los clientes en cualquier plataforma, siempre que el llamador envía un token XSTS válido con la solicitud. Escrituras son obviamente se limita a los clientes compatibles con el.
   * Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.
  
El ejemplo siguiente es el seudocódigo para la comprobación:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4ETC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".| 
  
<a id="ID4E3D"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion|  | Nombre o número del servicio al que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones del token de autenticación y así sucesivamente. Valor predeterminado: 1.| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4EIF"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 
el siguiente cuerpo POST se informa al servicio que se ha solicitado el cuatro de las estadísticas de dos SCIDs diferentes para dos usuarios distintos.
 

```cpp
{    
"requestedusers": [
                1234567890123460,
                1234567890123234
            ],
            "requestedscids": [
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c1212",
                    "requestedstats": [
                        "Test4FirefightKills",
                        "Test4FirefightHeadshots"
                    ]
                },
                {
                    "scid": "c402ff50-3e76-11e2-a25f-0800200c0343",
                    "requestedstats": [
                        "OverallTestKills",
                        "TestHeadshots"
                    ]
                }
            ] 
}
      
```

   
<a id="ID4EWF"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 304| No modificado| Recursos no ha sido modificado desde la última solicitado.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| No se admite la versión del recurso; se debe rechazar el nivel de MVC.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EXBAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{    
   "users":[          
       {    
      "xuid": "123456789"
        "gamertag": "WarriorSaint",
        "scids":[
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c1212",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":7
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":4
             }]
                        },
          {
             "scid":"c402ff50-3e76-11e2-a25f-0800200c0343",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":3434
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":41
             }]
          }],
                   },
    {    
                   "gamertag":"TigerShark",
                   "xuid":1234567890123234
        "scids":[
          {
             "scid":"123456789",
             "stats":  [
          {
                 "statname":"Test4FirefightKills",
                 "type":"Integer",
                 "value":63
             },
          {
                 "statname":"Test4FirefightHeadshots",
                 "type":"Integer",
                 "value":12
             }]
                        },
          {
"scid":"987654321",
             "stats":  [
          {
                 "statname":"OverallTestKills",
                 "type":"Integer",
                 "value":375
             },
          {
                 "statname":"TestHeadshots",
                 "type":"Integer",
                 "value":34
             }]
          }],
                   }]
}
         
```

   
<a id="ID4EDCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EFCAC"></a>

 
##### <a name="parent"></a>Primario 

[/batch](uri-batch.md)

   