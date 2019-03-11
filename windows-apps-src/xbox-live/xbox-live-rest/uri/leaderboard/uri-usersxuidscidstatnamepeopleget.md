---
title: GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
assetID: 942cf0d7-f988-0495-cf28-cdac608b8109
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeopleget.html
description: " GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 674e52d15d115560d4813edcd9687c2fe9694c9b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57650770"
---
# <a name="get-usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>GET (/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite})
Devuelve un marcador social mediante la clasificación de que los Estados de valores (puntuaciones) para todos los contactos conocidos del usuario actual o sólo los contactos designados como personas favoritas por ese usuario.
Es el dominio para estos URI `leaderboards.xboxlive.com`.

  * [Comentarios](#ID4EV)
  * [Parámetros de URI](#ID4EAB)
  * [Parámetros de cadena de consulta](#ID4ELB)
  * [Autorización](#ID4EQD)
  * [Encabezados de solicitud necesarios](#ID4EGE)
  * [Encabezados de solicitud opcionales](#ID4EXF)
  * [Cuerpo de la solicitud](#ID4ETG)
  * [Códigos de estado HTTP](#ID4ECEAC)
  * [Encabezados de respuesta necesaria](#ID4E1HAC)
  * [Encabezados de respuesta opcional](#ID4EDJAC)
  * [Cuerpo de respuesta](#ID4EDKAC)

<a id="ID4EV"></a>


## <a name="remarks"></a>Observaciones

Las API de tabla de clasificación son todas de solo lectura y, por tanto, solo admiten el verbo GET (a través de HTTPS). Reflejan una clasificación y ordenadas "páginas" de las estadísticas de Reproductor indizada que se derivan de las estadísticas de usuario individual que se escribieron a través de la plataforma de datos. Los índices de marcador completa pueden ser bastante grandes y nunca le pedirá los autores de llamadas ver en su totalidad, por lo tanto, este URI es compatible con varios argumentos de cadena de consulta que permiten un autor de llamada especificar qué tipo de vista desea ver en ese marcador.

Las operaciones GET no modificación los recursos, por lo que esto producirá los mismos resultados si ejecuta una o varias veces.

<a id="ID4EAB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| xuid| string| Identificador del usuario.|
| scid| string| Identificador de la configuración del servicio que contiene el recurso que se tiene acceso.|
| statname| string| Identificador único del recurso stat usuario que se obtiene acceso.|
| all|favorito| enumeración| Si se debe clasificar la stat valores (puntuaciones) para todos los contactos conocidos del usuario actual o sólo los contactos designados como personas favoritas por ese usuario.|

<a id="ID4ELB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| maxItems| entero de 32 bits sin signo| Número máximo de registros de marcador que se devuelven en una página de resultados. Si no se especifica, se devolverá un número predeterminado (10). El valor máximo de maxItems es aún sin definir, pero queremos evitar grandes conjuntos de datos, por lo que este valor debe tener como destino probablemente el mayor conjunto que un sintonizador de que interfaz de usuario podría controlar por llamada. |
| skipToRank| entero de 32 bits sin signo| Devolver una página de resultados a partir de la clasificación de marcador especificado. El resto de los resultados estarán en el criterio de ordenación por rango. Esta cadena de consulta puede devolver un token de continuación que se puede insertar en una consulta posterior para obtener "la" página siguiente de resultados. |
| skipToUser| string| Devolver una página de resultados de la tabla de clasificación en torno a la xuid jugador especificado, independientemente del rango o la puntuación de ese usuario. La página se ordenarán por el rango percentil con el usuario especificado en la última posición de la página de vistas predefinidas, o en medio de las vistas de tabla de clasificación stat. No hay ningún <b>continuationToken</b> proporcionado para este tipo de consulta. |
| continuationToken| string| Si una llamada anterior devolvió un <b>continuationToken</b>, a continuación, el llamador puede devolver ese token "tal cual" en una cadena de consulta para obtener la página siguiente de resultados. |
| sort| string| Especifique si desea clasificar la lista de jugadores de orden bajo a alto valor ("ascendente") o de alta o baja valor orden ("descendente"). Este es un parámetro opcional; el valor predeterminado es descendente.|

<a id="ID4EQD"></a>


## <a name="authorization"></a>Autorización

Se necesita autorización Xuid.

Se implementa la lógica de autorización para los fines de aislamiento de contenido y los escenarios de control de acceso.

Estadísticas de usuario y de marcadores se pueden leer desde los clientes en cualquier plataforma, siempre que el llamador envía un token válido de XSTS con su solicitud. Escrituras son (Obviamente) se limita a los clientes compatibles con la plataforma de datos.

Los desarrolladores de título pueden marcar las estadísticas abiertas o restringidas con XDP o centro de partners. Tablas de clasificación son estadísticas abiertas. Estadísticas abiertas pueden obtenerse mediante Smartglass, así como iOS, Android, Windows, Windows Phone y aplicaciones web, siempre y cuando el usuario está autorizado para el espacio aislado. Autorización del usuario a un espacio aislado se administra a través de XDP o centro de partners.

<a id="ID4EGE"></a>


## <a name="required-request-headers"></a>Encabezados de solicitud necesarios

| Encabezado| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- |
| Autorización| Cadena. Credenciales de autenticación para la autenticación HTTP. Valor de ejemplo: "XBL3.0 x =&lt;userhash >;&lt; token > ".|
| Content-Type| Cadena. El tipo MIME del cuerpo de la solicitud. Valor de ejemplo: "application/json".|
| X-RequestedServiceVersion| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud solo se enrutarán a que el servicio después de comprobar la validez del encabezado, las notificaciones en el token de autenticación y así sucesivamente. Valor predeterminado: 1.|
| Aceptar| Cadena. Valores aceptables de Content-Type. Valor de ejemplo: "application/json".|

<a id="ID4EXF"></a>


## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales

| Encabezado| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| If-None-Match| Cadena. Etiqueta de entidad - usa si el cliente admite el almacenamiento en caché. Valor de ejemplo: "686897696a7c876b7e".|

<a id="ID4ETG"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

Para maximizar la capacidad de cualquier persona que llama a comprender los datos que se está volviendo para visualización adecuada, se devolverá cada valor stat para cada usuario como una cadena en el formato en el que debe mostrar y con formato para que coincida con la configuración regional especificada en el lenguaje de aceptación encabezado de la solicitud. Un localizado "nombre para mostrar" también se devolverá para statname para ese marcador.

<a id="ID4E2G"></a>


### <a name="required-members"></a>Miembros requeridos

| Miembro| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| <b>pagingInfo</b>| Sección| Opcional. Se devuelve cuando el rango de la última entrada en la página es inferior a totalItems. En esta sección también se devuelve no si skipToUser se especifica en la solicitud.|
| continuationToken| string| Obligatorio. Especifica qué valor a la fuente en el parámetro de consulta "continuationToken" para obtener la siguiente página de resultados para ese URI si lo desea. Si no se devuelve ningún pagingInfo no hay ninguna "página siguiente" de datos que se va a obtenerse.|
| totalItems| entero sin signo de 64 bits| Obligatorio. Número total de entradas en la tabla de líderes.|
| <b>leaderboardInfo</b>| Sección| Obligatorio. Siempre se devuelven. Contiene los metadatos sobre el marcador solicitado.|
| displayName| string| Obligatorio. Nombre para mostrar localizado de la tabla de clasificación predefinido. Valor de ejemplo: "Total de marcas capturadas".|
| totalCount| string| Obligatorio. Número total de entradas en la tabla de líderes.|
| Columnas| array| Obligatorio.|
| displayName| string| Obligatorio. Corresponde a una columna en la tabla de líderes.|
| statName| string| Obligatorio. Corresponde a una columna en la tabla de líderes.|
| type| string| Obligatorio. Corresponde a una columna en la tabla de líderes.|
| <b>userList</b>| Sección| Obligatorio. Siempre se devuelven. Contiene las puntuaciones de usuario real para la tabla de líderes solicitado.|
| gamertag| string| Obligatorio. Corresponde al usuario en la entrada de tabla de clasificación.|
| xuid| entero de 32 bits con signo| Obligatorio. Corresponde al usuario en la entrada de tabla de clasificación.|
| Percentil| string| Obligatorio. Corresponde al usuario en la entrada de tabla de clasificación.|
| rango| string| Obligatorio. Corresponde al usuario en la entrada de tabla de clasificación.|
| Valores| array| Obligatorio. Cada valor separados por comas que se corresponde con una columna en la tabla de líderes.|

<a id="ID4ECEAC"></a>


## <a name="http-status-codes"></a>Códigos de estado HTTP

El servicio devuelve uno de los códigos de estado de esta sección en respuesta a una solicitud realizada con este método en este recurso. Para obtener una lista completa de los códigos de estado HTTP estándar, puede usar con servicios de Xbox Live, consulte [códigos de estado HTTP estándar](../../additional/httpstatuscodes.md).

| Código| Frase de motivo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 200| Aceptar| La sesión se ha recuperado correctamente.|
| 304| No modificado|  |
| 400| solicitud incorrecta| | Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|
| 401| Sin autorización| | La solicitud requiere autenticación del usuario.|
| 403| prohibido| No se permite la solicitud para el usuario o servicio.|
| 404| No encontrado| No se encontró el recurso especificado.|
| 406| No es aceptable| No se admite la versión del recurso; se debe rechazar el nivel de MVC.|
| 408| Tiempo de espera de solicitud| Servicio no entendió la solicitud con formato incorrecto. Normalmente, un parámetro no válido.|

<a id="ID4E1HAC"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Content-Type| string| El tipo mime del cuerpo de la respuesta. Valores de ejemplo: "application/json".|
| Content-Length| string| El número de bytes que se va a enviar en la respuesta. Valor de ejemplo: "232".|

<a id="ID4EDJAC"></a>


## <a name="optional-response-headers"></a>Encabezados de respuesta opcional

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".|

<a id="ID4EDKAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

Solicitud para el marcador social, ninguna paginación:

https://leaderboards.xboxlive.com/users/xuid(2533274916402282)/scids/c1ba92a9-0000-0000-0000-000000000000/stats/EnemyDefeats/people/all?sort=descending

<a id="ID4ENKAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
    "pagingInfo": null,
    "leaderboardInfo": {
        "displayName": "Kills",
        "totalCount": 3,
        "columns": [
            {
                "displayName": "Kills",
                "statName": "enemydefeats",
                "type": "integer"
            }
        ]
    },
    "userList": [
        {
            "gamertag":"xxxSniper39",
            "xuid":"1234567890123555",
            "percentile":1.0,
            "rank":1,
            "values": [
                "47"
            ]
        },
        {
            "gamertag":"WarriorSaint",
            "xuid":"2533274916402282",
            "percentile":0.66,
            "rank":2,
            "values": [
                "42"
            ]
        },
        {
            "gamertag":"WhockaWhocka",
            "xuid":"1234567890123666",
            "percentile":0.33,
            "rank":3,
            "values": [
                "12"
            ]
        }
    ]
}

```


<a id="ID4EXKAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EZKAC"></a>


##### <a name="parent"></a>Primario

[/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all\|favorite}](uri-usersxuidscidstatnamepeople.md)
