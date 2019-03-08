---
title: GET (/users/{ownerId}/clips)
assetID: da972b4e-bc38-66f5-2222-5e79d7c8a183
permalink: en-us/docs/xboxlive/rest/uri-usersowneridclipsget.html
description: " GET (/users/{ownerId}/clips)"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7c52daf4a07914c34f1aadc84a7238771669d65f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623210"
---
# <a name="get-usersowneridclips"></a>GET (/users/{ownerId}/clips)
Recuperar la lista de clips del usuario.
Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.

  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EEB)
  * [Parámetros de cadena de consulta](#ID4EPB)
  * [Cuerpo de la solicitud](#ID4EPE)
  * [Encabezados de respuesta necesaria](#ID4E1E)
  * [Encabezados de respuesta opcional](#ID4ENH)
  * [Cuerpo de respuesta](#ID4EOAAC)
  * [URI relacionados](#ID4EABAC)

<a id="ID4EX"></a>


## <a name="remarks"></a>Observaciones

Esta API permite varias formas de lista que propia de un usuario recorta los clips, así como otros usuarios que están almacenados en el servicio. Varios puntos de entrada devuelvan datos de diferentes niveles y permiten el filtrado a través de los parámetros de consulta. Si el XUID en la notificación coincide con el propietario especificado en el URI, los clips del usuario se devuelven después de comprobaciones de aislamiento del contenido. Si el propietario en el URI no coincide con la notificación XUID, a continuación, los clips del usuario especificado se basa en las comprobaciones de privacidad y comprobaciones de aislamiento de contenido de la solicitud XUID.

Las consultas se optimizan por usuario y el Id. de configuración de servicio (¿scid). Especificar aún más filtros o criterios de ordenación distintos de los predeterminados como se indica a continuación puede en algunas circunstancias tardan más en volver. Esto es más evidente para conjuntos más grandes de vídeos por usuario.

No hay ninguna API de lote para obtener la lista de varios de los usuarios dentro de la misma llamada de API. El patrón recomendado (actualmente) de los arquitectos de SLS es consultar a su vez para cada usuario.

<a id="ID4EEB"></a>


## <a name="uri-parameters"></a>Parámetros de URI

| Parámetro| Tipo| Descripción|
| --- | --- | --- |
| ownerId| string| Identidad de usuario del usuario cuyo recurso se está obteniendo acceso. Formatos admitidos: "me" o "xuid(123456789)". Longitud máxima: 16.|

<a id="ID4EPB"></a>


## <a name="query-string-parameters"></a>Parámetros de cadena de consulta

| Parámetro| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- |
| skipItems| entero de 32 bits con signo| Opcional. Devolver los elementos que empieza por N + 1 en la colección (es decir, omitir N elementos).|
| continuationToken| string| Opcional. Devolver los elementos comenzando en el token de continuación determinado. El parámetro continuationToken tiene prioridad sobre skipItems si se especifican ambas. En otras palabras, el parámetro skipItems se omite si el parámetro continuationToken está presente. Tamaño máximo: 36.|
| maxItems| entero de 32 bits con signo| Opcional. Número máximo de elementos que se va a devolver de la colección (se puede combinar con skipItems y continuationToken para devolver un intervalo de elementos). El servicio puede proporcionar un valor predeterminado si no está presente de maxItems y puede devolver menos de maxItems (incluso si la última página de resultados todavía no se han devuelto).|
| Orden| Carácter Unicode| Opcional. Especifica si se devuelve la lista en escendente (D) (alto valor primero) o (A) scendente (menor valor primero) orden. Default: D.|
| type| GameClipTypes| Opcional. Conjunto delimitado por comas del tipo de clips a devolver. Default: Todos los.|
| eventId| string| Opcional. Conjunto delimitado por comas de objetos eventids comprendidos para filtrar los resultados por. Default: es null.|
| calificador| string| Opcional. Especifica el calificador de orden que se usará para obtener los clips. <ul><li>creado: especifica los clips se devuelven en orden de fecha en el sistema</li><li>Rating - [más valorados]: Especifica que los clips se devuelven por su valor de clasificación</li><li>vistas - [lo más visto -] especifica los clips se devuelven por número de vistas</li></ul><br/> Tamaño máximo: 12. Valor predeterminado: "creado".| 

<a id="ID4EPE"></a>


## <a name="request-body"></a>Cuerpo de la solicitud

No hay ningún miembro necesario para esta solicitud.

<a id="ID4E1E"></a>


## <a name="required-response-headers"></a>Encabezados de respuesta necesaria

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.|
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.|
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.|
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.|
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible.|
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores.|

<a id="ID4ENH"></a>


## <a name="optional-response-headers"></a>Encabezados de respuesta opcional

| Encabezado| Tipo| Descripción|
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| ETag| string| Se usa para la optimización de memoria caché. Por ejemplo: "686897696a7c876b7e".|

<a id="ID4EOAAC"></a>


## <a name="response-body"></a>Cuerpo de la respuesta

<a id="ID4EUAAC"></a>


### <a name="sample-response"></a>Respuesta de ejemplo


```cpp
{
       "gameClips":
       [
           {
               "xuid": "2716903703773872",
               "clipName": "[en-US] Localized Greatest Moment Name",
               "titleName": "[en-US] Localized Title Name",
               "gameClipLocale": "en-US",
               "gameClipId": "6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
               "state": "Published",
               "dateRecorded": "2013-06-14T01:02:55.4918465Z",
               "lastModified": "2013-06-14T01:05:41.3652693Z",
               "userCaption": "Set by user!",
               "type": "DeveloperInitiated",
               "source": "Console",
               "visibility": "Public",
               "durationInSeconds": 30,
               "scid": "00000000-0000-0012-0023-000000000070",
               "titleId": 354975,
               "rating": 3.75,
               "ratingCount": 245,
               "views": 7453,
               "titleData": "",
               "systemProperties": "",
               "savedByUser": false,
               "achievementId": "AchievementId",
               "greatestMomentId": "GreatestMomentId",
               "thumbnails": [
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/large",
                       "fileSize": 637293,
                       "thumbnailType": "Large"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000/thumbnails/small",
                       "fileSize": 163998,
                       "thumbnailType": "Small"
                   }
               ],
               "gameClipUris": [
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest",
                       "uriType": "SmoothStreaming",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/897f65a9-63f0-45a0-926f-05a3155c04fc/GameClip-Original_4000.ism/manifest(format=m3u8-aapl)",
                       "uriType": "Ahls",
                       "expiration": "2013-06-14T01:10:08.73652Z"
                   },
                   {
                       "uri": "http://localhost/users/xuid(2716903703773872)/scids/00000000-0000-0012-0023-000000000070/clips/6873f6cf-af12-48a5-8be6-f3dfc3f61538-000",
                       "fileSize": 88820,
                       "uriType": "Download",
                       "expiration": "2999-12-31T11:59:40Z"
                   }
               ]
           }
       ],
   "pagingInfo":
       {
           "continuationToken": null,
           "totalItems": 1
       }
   }

```


<a id="ID4EABAC"></a>


## <a name="related-uris"></a>URI relacionados

El siguiente URI es idéntico al principal en este documento, pero con un parámetro de ruta de acceso adicional para especificar un ¿SCID. Se devolverá únicamente los clips del usuario para ese ¿SCID. El usuario solicitante debe tener acceso a la ¿SCID solicitado, en caso contrario, el error 403 de HTTP se devolverá.

   * **/users/{ownerId}/scids/{scid}/clips**

<a id="ID4ENBAC"></a>


## <a name="see-also"></a>Consulte también

<a id="ID4EPBAC"></a>


##### <a name="parent"></a>Primario

[/users/{ownerId}/clips](uri-usersowneridclips.md)
