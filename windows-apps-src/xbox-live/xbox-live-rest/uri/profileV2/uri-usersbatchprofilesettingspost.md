---
title: POST (/users/batch/profile/settings)
assetID: 2a619148-a626-f413-bda1-a2790063075d
permalink: en-us/docs/xboxlive/rest/uri-usersbatchprofilesettingspost.html
description: " POST (/users/batch/profile/settings)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 0f859a58e32624223d59d918d46f6230a3abd6db
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662230"
---
# <a name="post-usersbatchprofilesettings"></a>POST (/users/batch/profile/settings)
Obtener el perfil de un usuario o usuarios. Es el dominio para estos URI `profile.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Autorización](#ID4EFB)
  * [Encabezados de solicitud necesarios](#ID4EOB)
  * [Cuerpo de la solicitud](#ID4EZC)
  * [Cuerpo de respuesta](#ID4EJD)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
Esto es permitido en la URL del perfil solo completo. Se bloquean todas las otras API de perfil de los clientes.
  
<a id="ID4EFB"></a>

 
## <a name="authorization"></a>Autorización
 
Para obtener acceso a un perfil, se necesitan solo un token de autenticación normal y notificaciones.
  
<a id="ID4EOB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | 
| x-xbl-contract-version| entero de 32 bits sin signo| La versión de contrato debe establecerse en 2, para distinguir esta llamada de la API de Xbox 360.| 
| content-type| string| Valor = <code>application/json</code>| 
  
<a id="ID4EZC"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4E6C"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
POST /users/batch/profile/settings
   {
      "userIds":[
         "2533274791381930"
       ],
      "settings":[
         "GameDisplayName",
         "GameDisplayPicRaw",
         "Gamerscore",
         "Gamertag"
      ]
   }
      
```

   
<a id="ID4EJD"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EPD"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
      "profileUsers":[
         {
            "id":"2533274791381930",
            "settings":[
               {
                  "id":"GameDisplayName",
                  "value":"John Smith"
               },
               {
                  "id":"GameDisplayPicRaw",
                  "value":"https://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F"
               },
               {
                  "id":"Gamerscore",
                  "value":"0"
               },
               {
                  "id":"Gamertag",
                  "value":"CracklierJewel9"
               }
            ]
         }
      ]
   }
         
```

   
<a id="ID4EZD"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E2D"></a>

 
##### <a name="parent"></a>Primario 

[/users/batch/profile/settings](uri-usersbatchprofilesettings.md)

  
<a id="ID4EFE"></a>

 
##### <a name="reference"></a>Referencia 

[Profile (JSON)](../../json/json-profile.md)

   