---
title: GET (/users/{userId}/profile/settings/people/{userList})
assetID: f6553499-89e2-f21b-a00f-7e5437c045ff
permalink: en-us/docs/xboxlive/rest/uri-usersuseridprofilesettingspeopleuserlistget.html
description: " GET (/users/{userId}/profile/settings/people/{userList})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f868fdf4f3d5cd36000784d9c5a3437fa5d67ffa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593860"
---
# <a name="get-usersuseridprofilesettingspeopleuserlist"></a>GET (/users/{userId}/profile/settings/people/{userList})
Obtener el perfil de un usuario o usuarios, con el Moniker de personas compatibilidad. Es el dominio para estos URI `profile.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EKB)
  * [Parámetros de cadena de consulta](#ID4EVB)
  * [Encabezados de solicitud necesarios](#ID4EQC)
  * [Cuerpo de la solicitud](#ID4E2D)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
**userList** y **userIds** son los parámetros se excluyen mutuamente. Si se especifican ambos, o bien, obtendrá un **BadRequest** atrás. **userList** es una matriz para el futuro en escenarios donde varias listas con nombre son útiles para la solicitud. **identificadores de usuario** se compone de las cadenas decimal de XUIDs - JSON es incorrecta en serializar enteros sin signo de 64 bits. Por último, la configuración en Xbox One se denominará configuración, con nombres legibles normales, en lugar de enteros de 64 bits sin signo o constantes oscuras, como **XONLINE_PROFILE_ASDF**.
  
<a id="ID4EKB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| userId| string| ¿Puedo ser 'xuid(12345)', 'gt(myGamertag)' o 'me'.| 
| userList| string| Lista de personas para obtener la configuración con nombre. Actualmente, las personas es la única lista compatible.| 
  
<a id="ID4EVB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| configuración| string| Una lista delimitada por comas de nombres de configuración.| 
  
<a id="ID4EQC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| x-xbl-contract-version| entero de 32 bits con signo| Valor = 2| 
| content-type| string| Valor = <code>application/json</code>| 
  
<a id="ID4E2D"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4EBE"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
GET /users/me/profile/settings/people/people?settings=GameDisplayName,GameDisplayPicRaw,Gamerscore,Gamertag
      
```

  
<a id="ID4EKE"></a>

  
 
<a id="ID4EME"></a>

 
##### <a name="response-body"></a>Cuerpo de la respuesta 
La respuesta es un **ReadMultiSettingsResponseV2** objeto. Suponiendo que el usuario que realiza la llamada tiene solo un amigo:
  

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
                  "value":"http://images-eds.xboxlive.com/image?url=z951ykn43p4FqWbbFvR2Ec.8vbDhj8G2Xe7JngaTToBrrCmIEEXHC9UNrdJ6P7KIN0gxC2r1YECCd3mf2w1FDdmFCpSokJWa2z7xtVrlzOyVSc6pPRdWEXmYtpS2xE4F&format=png&w=64&h=64"
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

   
<a id="ID4E3E"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4E5E"></a>

 
##### <a name="parent"></a>Primario 

[/users/{userId}/profile/settings/people/{userList}?settings={settings}](uri-usersuseridprofilesettingspeopleuserlist.md)

 [Profile (JSON)](../../json/json-profile.md)

   