---
title: POST (/users/{ownerId}/people/xuids)
assetID: e20bfb58-9c3b-14ed-6462-85d42fa6fe1a
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeoplexuidspost.html
description: " POST (/users/{ownerId}/people/xuids)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1cb160f3276d215e3aba5dfd671c67fa17d883b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57589870"
---
# <a name="post-usersowneridpeoplexuids"></a>POST (/users/{ownerId}/people/xuids)
Obtiene las personas por XUID de personas de la persona que llama colección. Es el dominio para estos URI `social.xboxlive.com`.
 
  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4E5)
  * [Autorización](#ID4EJB)
  * [Encabezados de solicitud necesarios](#ID4ERC)
  * [Encabezados de solicitud opcionales](#ID4EBE)
  * [Cuerpo de la solicitud](#ID4EHF)
  * [Códigos de estado HTTP](#ID4EKH)
  * [Encabezados de respuesta necesaria](#ID4ENBAC)
  * [Cuerpo de respuesta](#ID4EZCAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Observaciones
 
REGISTRAR operaciones no modificación todos los recursos para que esto producirá los mismos resultados si ejecuta una o varias veces.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| ownerId| string| Identificador del usuario cuyo recurso se está obteniendo acceso. Debe coincidir con el usuario autenticado. Los valores posibles son "me", xuid({xuid}) o gt({gamertag}).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Autorización
 
| Tipo| Requerido| Descripción| Si falta de respuesta| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| sí| Autor de llamada tiene el Id. de usuario de Xbox (XUID del usuario).| 401 no autorizado| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Cadena. Datos de autorización de Xbox LIVE. Esto suele ser un token XSTS cifrado. Valor de ejemplo: <b>XBL3.0 x =&lt;userhash >;&lt; token ></b>.| 
| Content-Length| entero de 32 bits sin signo. Longitud, en bytes, del cuerpo de la solicitud. Valor de ejemplo: 22.| 
| Content-Type| Cadena. Tipo MIME del cuerpo de la solicitud. Debe ser <b>application/json</b>.| 
  
<a id="ID4EBE"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor predeterminado: 1.| 
| Aceptar| Cadena. Tipos de contenido que acepta el llamador en la respuesta. Todas las respuestas son <b>application/json</b>.| 
  
<a id="ID4EHF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
<a id="ID4ENF"></a>

 
### <a name="required-members"></a>Miembros requeridos
 
| Miembro| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| XuidList| Matriz de XUIDs que identifican las personas que puedan devolverse desde la colección de las personas del llamador. Consulte [XuidList (JSON)](../../json/json-xuidlist.md).| 
  
<a id="ID4EKG"></a>

 
### <a name="optional-members"></a>Miembros opcionales
 
No hay ningún miembro opcional para esta solicitud.
  
<a id="ID4EVG"></a>

 
### <a name="prohibited-members"></a>Miembros prohibidos
 
Todos los demás miembros están prohibidas en una solicitud.
  
<a id="ID4EAH"></a>

 
### <a name="sample-request"></a>Solicitud de ejemplo
 

```cpp
{
    "xuids": [
        "2533274790395904", 
        "2533274792986770", 
        "2533274794866999"
    ]
}
      
```

   
<a id="ID4EKH"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| Operación correcta al método es "get".| 
| 204| Sin contenido| Operación correcta al método es "add" o "quitar".| 
| 400| solicitud incorrecta| Parámetro de método era incorrecto o que faltan o los identificadores de usuario estaban tiene un formato incorrecto.| 
| 403| prohibido| No se pudo analizar la notificación XUID del encabezado de autorización.| 
  
<a id="ID4ENBAC"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| entero de 32 bits sin signo| Longitud, en bytes, del cuerpo de respuesta. Valor de ejemplo: 22.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Esto siempre será <b>application/json</b>.| 
  
<a id="ID4EZCAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Un cuerpo de respuesta solo se envía cuando el método de solicitud es "get". No hay ningún cuerpo de respuesta de "Agregar" o "quitar".
 
Si una llamada de método "get" se realiza correctamente, el servicio devuelve al número total de personas en las personas del llamador, colección y una matriz que contiene la colección de las personas del llamador. Se devuelve ninguna respuesta para los métodos de "Agregar" y "quitar". Consulte [PeopleList (JSON)](../../json/json-peoplelist.md).
 
<a id="ID4EHDAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 

```cpp
{
    "people": [
        {
            "xuid": "2603643534573573",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573572",
            "isFavorite": true,
            "isFollowingCaller": false,
            "socialNetworks": ["LegacyXboxLive"]
        },
        {
            "xuid": "2603643534573577",
            "isFavorite": false,
            "isFollowingCaller": false
        },
    ],
    "totalCount": 3
}
         
```

   
<a id="ID4ERDAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4ETDAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/{ownerId}/people/xuids](uri-usersowneridpeoplexuids.md)

   