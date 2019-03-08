---
title: GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
assetID: ee69a9e3-ea91-3cf5-a10a-811762cba892
permalink: en-us/docs/xboxlive/rest/uri-scidsscidleaderboardsleaderboardnamegetvaluemetadata.html
description: " GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: fad57c5e989d933777c913030faaa594c6bbd059
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623440"
---
# <a name="get-scidsscidleaderboardsleaderboardnameincludevaluemetadata"></a>GET (/scids/{scid}/leaderboards/{leaderboardname}?include=valuemetadata)
 
Obtiene un marcador global predefinido junto con los metadatos asociados con los valores de marcador.
 
Es el dominio para estos URI `leaderboards.xboxlive.com`.
 
  * [Comentarios](#ID4EY)
  * [Parámetros de URI](#ID4EHB)
  * [Parámetros de cadena de consulta](#ID4ESB)
  * [Autorización](#ID4EVD)
  * [Efecto de la configuración de privacidad en recurso](#ID4EQE)
  * [Encabezados de solicitud necesarios](#ID4EZE)
  * [Encabezados de solicitud opcionales](#ID4EOG)
  * [Códigos de estado HTTP](#ID4EOH)
  * [Encabezados de respuesta](#ID4EFDAC)
  * [Cuerpo de respuesta](#ID4ECFAC)
 
<a id="ID4EY"></a>

 
## <a name="remarks"></a>Observaciones
 
El? incluyen = valuemetadata parámetro de consulta permite la respuesta incluir los metadatos asociados con los valores de marcador. Los metadatos de valor contienen el cliente especificada datos asociados con el valor, como el modelo y el color de un automóvil que se utiliza para lograr un mejor momento en una pista de carreras.
 
Metadatos de valor están definidos en los Estados de usuario que se basa la tabla de líderes en, no en la tabla de líderes.
 
Leaderboard APIs son todas de solo lectura y, por tanto, solo se admite el verbo GET. Reflejan una clasificación y ordenadas "páginas" de las estadísticas de Reproductor indizada que se derivan de las estadísticas de usuario individual que se escribieron a través de la plataforma de datos. Los índices de marcador completa pueden ser bastante grandes y nunca le pedirá los autores de llamadas ver en su totalidad, por lo tanto, este URI es compatible con varios argumentos de cadena de consulta que permiten un autor de llamada especificar qué tipo de vista desea ver en ese marcador.
 
Las operaciones GET no modificación los recursos, por lo que esto producirá los mismos resultados si ejecuta una o varias veces.
  
<a id="ID4EHB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| GUID| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.| 
| leaderboardname| string| Identificador único del recurso marcador predefinidas que se obtiene acceso.| 
  
<a id="ID4ESB"></a>

 
## <a name="query-string-parameters"></a>Parámetros de cadena de consulta
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| include=valuemetadata| string| Indica que la respuesta incluye los metadatos de valor asociado con los valores de marcador.| 
| maxItems| entero de 32 bits sin signo| Número máximo de registros de marcador que se devuelven en una página de resultados. Si no se especifica, se devolverá un número predeterminado (10). El valor máximo de maxItems es aún sin definir, pero queremos evitar grandes conjuntos de datos, por lo que este valor debe tener como destino probablemente el mayor conjunto que un sintonizador de que interfaz de usuario podría controlar por llamada.| 
| skipToRank| entero de 32 bits sin signo| Devolver una página de resultados a partir de la clasificación de marcador especificado. El resto de los resultados estarán en el criterio de ordenación por rango. Esta cadena de consulta puede devolver un token de continuación que se puede insertar en una consulta posterior para obtener "la" página siguiente de resultados.| 
| skipToUser| string| Devolver una página de resultados de la tabla de clasificación en torno a la xuid jugador especificado, independientemente del rango o la puntuación de ese usuario. La página se ordenarán por el rango percentil con el usuario especificado en la última posición de la página de vistas predefinidas, o en medio de las vistas de tabla de clasificación stat. No hay ningún continuationToken proporcionado para este tipo de consulta.| 
| continuationToken| string| Si una llamada anterior devolvió un continuationToken, a continuación, el llamador puede devolver ese token "tal cual" en una cadena de consulta para obtener la página siguiente de resultados.| 
  
<a id="ID4EVD"></a>

 
## <a name="authorization"></a>Autorización
 
No hay lógica de autorización que se implementa para escenarios de aislamiento de contenido y control de acceso.
 
   * Estadísticas de usuario y de marcadores se pueden leer desde los clientes en cualquier plataforma, siempre que el llamador envía un token XSTS válido con la solicitud. Escrituras son obviamente se limita a los clientes compatibles con la plataforma de datos.
   * Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.
  
Pseudocódigo para la comprobación tiene este aspecto:
 

```cpp
If (!checkAccess(serviceConfigId, resource, CLAIM[userid, deviceid, titleid]))
{
        Reject request as Unauthorized
}

// else accept request.
         
```

  
<a id="ID4EQE"></a>

 
## <a name="effect-of-privacy-settings-on-resource"></a>Efecto de la configuración de privacidad en recurso
 
Se realiza ninguna comprobación de la privacidad al leer los datos de marcador.
  
<a id="ID4EZE"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Autorización| Cadena. Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: <b>XBL3.0 x =&lt;userhash >;&lt; símbolo (token) ></b>| 
| X-Xbl-Contract-Version| Cadena. Indica qué versión de la API para usar. Este valor debe establecerse en "3" con el fin de incluir los metadatos de valor en la respuesta.| 
| X-RequestedServiceVersion| Cadena. Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Valor predeterminado: 1.| 
| Aceptar| Cadena. Tipos de contenido que son aceptables. Valor de ejemplo: <b>application/json</b>| 
  
<a id="ID4EOG"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| If-None-Match| Cadena. Etiqueta de entidad, se usa si el cliente admite el almacenamiento en caché. Valor de ejemplo: <b>"686897696a7c876b7e"</b>|  | 
  
<a id="ID4EOH"></a>

 
## <a name="http-status-codes"></a>Códigos de estado HTTP
 
El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).
 
| Código| Frase de motivo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| Aceptar| La sesión se ha recuperado correctamente.| 
| 304| No modificado| Recursos no ha sido modificado desde la última solicitado.| 
| 400| solicitud incorrecta| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.| 
| 401| Sin autorización| La solicitud requiere autenticación del usuario.| 
| 403| prohibido| No se permite la solicitud para el usuario o servicio.| 
| 404| No encontrado| No se encontró el recurso especificado.| 
| 406| No es aceptable| No se admite la versión del recurso.| 
| 408| Tiempo de espera de solicitud| No se admite la versión del recurso; se debe rechazar el nivel de MVC.| 
  
<a id="ID4EFDAC"></a>

 
## <a name="response-headers"></a>Encabezados de respuesta
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Type| string| Obligatorio. El tipo MIME del cuerpo de la respuesta. Ejemplo: <b>application/json</b>.| 
| Content-Length| string| Obligatorio. El número de bytes que se va a enviar en la respuesta. Por ejemplo: <b>232</b>.| 
| ETag| string| Opcional. Se usa para la optimización de memoria caché. Por ejemplo: <b>686897696a7c876b7e</b>.| 
  
<a id="ID4ECFAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
<a id="ID4EIFAC"></a>

 
### <a name="response-members"></a>Miembros de la respuesta
 
pagingInfo | pagingInfo| Sección| Opcional. Solo está presente cuando maxItems se especifica en la solicitud.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| continuationToken| entero sin signo de 64 bits| Obligatorio. Especifica qué valor a la fuente en el <b>skipItems</b> consultar parámetro para obtener la página siguiente de resultados para ese URI si lo desea. Si no hay ningún <b>pagingInfo</b> se devuelve, no hay ninguna página siguiente de datos que se va a obtenerse.| 
| totalItems| entero sin signo de 64 bits| Obligatorio. Número total de entradas en la tabla de líderes. Valor de ejemplo: 1234567890| 
 
leaderboardInfo | leaderboardInfo| Sección| Obligatorio. Siempre se devuelven. Contiene los metadatos sobre el marcador solicitado.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| displayName| string| No se utiliza.| 
| totalCount| cadena (entero sin signo de 64 bits)| Obligatorio. Número total de entradas en la tabla de líderes. Valor de ejemplo: 1234567890| 
| columnDefinition| Objeto JSON| Obligatorio. Describe la columna en la tabla de líderes.| 
| columnDefinition.displayName| string| Obligatorio. Un nombre descriptivo de la columna en la tabla de líderes.| 
| columnDefinition.statName| string| Obligatorio. El nombre de la estadística de usuario que se basa el marcador.| 
| columnDefinition.type| string| Obligatorio. El tipo de datos de los Estados de usuario en la columna.| 
 
userList | userList| Sección| Obligatorio. Siempre se devuelven. Contiene las puntuaciones de usuario real para la tabla de líderes solicitado.| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| gamertag| string| Obligatorio. El nombre de jugador del usuario en la entrada de tabla de clasificación.| 
| xuid| entero sin signo de 64 bits| Obligatorio. El identificador de usuario de Xbox del usuario en la entrada de tabla de clasificación.| 
| Percentil| string| Obligatorio. Rango percentil del usuario en la tabla de líderes.| 
| rango| string| Obligatorio. Rango numérico del usuario en la tabla de líderes.| 
| value| string| Obligatorio. El valor del usuario de la estadística en la que se basa el marcador.| 
| valueMetadata| string| Obligatorio. Una cadena de coma separada por pares de cadenas en el formato "\"nombre\" : \"valor\",..."

Si no hay ningún metadato, este valor es una cadena vacía. | 
  
<a id="ID4EGLAC"></a>

 
### <a name="sample-response"></a>Respuesta de ejemplo
 
El URI de solicitud siguiente muestra omitiendo para clasificar en un tablero de puntuaciones global.
 

```cpp
https://leaderboards.xboxlive.com/scids/0FA0D955-56CF-49DE-8668-05D82E6D45C4/leaderboards/TotalFlagsCaptured?include=valuemetadata&maxItems=3&skipToRank=15000
         
```

 
Con el fin de devolver los metadatos de valor, también se debe especificar el encabezado siguiente.
 

```cpp
X-Xbl-Contract-Version : 3
          
```

 
El URI anterior devuelve la siguiente respuesta JSON.
 

```cpp
{
    "pagingInfo": {
        "continuationToken": "15003",
        "totalItems": 23437
    },
    "leaderboardInfo": {
        "displayName": "Total flags captured",
        "totalCount": 23437,
        "columnDefinition" : 
            {
                "displayName": "Flags Captured",
                "statName": "flagscaptured",
                "type": "Integer"
            }
    },
    "userList": [
        {
            "gamertag": "WarriorSaint",
            "xuid": "1234567890123444",
            "percentile": 0.64,
            "rank": 15000,
            "value": "1002",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2000, \"israining\" : false}"
        },
        {
            "gamertag": "xxxSniper39",
            "xuid": "1234567890123555",
            "percentile": 0.64,
            "rank": 15001,
            "value": "993",
            "valuemetadata" : "{\"color\" : \"silver\",\"weight\" : 2020, \"israining\" : true}"
 
        },
        {
            "gamertag": "WhockaWhocka",
            "xuid": "1234567890123666",
            "percentile": 0.64,
            "rank": 15002,
            "value": "700",
            "valuemetadata" : "{\"color\" : \"red\",\"weight\" : 300, \"israining\" : false}"
        }
    ]
}
         
```

   
<a id="ID4E6LAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EBMAC"></a>

 
##### <a name="parent"></a>Primario 

[/scids/{scid}/leaderboards/{leaderboardname}](uri-scidsscidleaderboardsleaderboardname.md)

   