---
title: POST (/users/me/scids/{scid}/clips/{gameClipId})
assetID: 410aecad-57f9-c3dc-f35f-19c4d8dfb704
permalink: en-us/docs/xboxlive/rest/uri-usersmescidclipsgameclipidpost.html
description: " POST (/users/me/scids/{scid}/clips/{gameClipId})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 89f3b53631f5570ab6d0d0619f6678fc3e3c2dd2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645830"
---
# <a name="post-usersmescidsscidclipsgameclipid"></a>POST (/users/me/scids/{scid}/clips/{gameClipId})
Actualizar metadatos de clip de juego para los datos del usuario. Los dominios para estos URI son `gameclipsmetadata.xboxlive.com` y `gameclipstransfer.xboxlive.com`, según la función del identificador URI en cuestión.
 
  * [Comentarios](#ID4EX)
  * [Parámetros de URI](#ID4EAB)
  * [Encabezados de solicitud necesarios](#ID4ELB)
  * [Encabezados de solicitud opcionales](#ID4EXD)
  * [Cuerpo de la solicitud](#ID4EAF)
  * [Encabezados de respuesta necesaria](#ID4EVF)
  * [Encabezados de respuesta opcional](#ID4EJAAC)
  * [Cuerpo de respuesta](#ID4EJBAC)
  * [URI relacionados](#ID4EWBAC)
 
<a id="ID4EX"></a>

 
## <a name="remarks"></a>Observaciones
 
La API para actualizar los metadatos de clip de juego se divide en dos categorías, actualizar los metadatos de sus juegos clips como accesibilidad y el título y la actualización de los atributos públicos (por ejemplo, aplicar una clasificación o de incrementar el recuento de vistas) de cualquier otro juego clip. Si el XUID en el URI no coincide con el XUID en la notificación, se pueden editar los datos públicos y se denegará cualquier solicitud para modificar cualquiera de los demás datos. En el caso de varios campos están intentando editar y uno de ellos no es válido para la solicitud, se producirá un error en la solicitud completa.
  
<a id="ID4EAB"></a>

 
## <a name="uri-parameters"></a>Parámetros de URI
 
| Parámetro| Tipo| Descripción| 
| --- | --- | --- | 
| scid| string| Id. de configuración de servicio del recurso al que se tiene acceso. Debe coincidir con el ¿SCID del usuario autenticado.| 
| gameClipId| string| Clip de juego el identificador del recurso que está accediendo.| 
  
<a id="ID4ELB"></a>

 
## <a name="required-request-headers"></a>Encabezados de solicitud necesarios
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | 
| Autorización| string| Credenciales de autenticación para la autenticación HTTP. Valores de ejemplo: <b>Xauth=&lt;authtoken></b>| 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
  
<a id="ID4EXD"></a>

 
## <a name="optional-request-headers"></a>Encabezados de solicitud opcionales
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Accept-Encoding| string| Codificaciones de compresión aceptable. Valores de ejemplo: gzip, deflate, la identidad.| 
| ETag| string| Se usa para la optimización de memoria caché. Valor de ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4EAF"></a>

 
## <a name="request-body"></a>Cuerpo de la solicitud
 
El cuerpo de la solicitud debe ser un [UpdateMetadataRequest](../../json/json-updatemetadatarequest.md) objeto en formato JSON. Ejemplos:
 
Cambiar el nombre de usuario Clip y visibilidad:
 

```cpp
{
  "userCaption": "I've changed this 100 Times!",
  "visibility": "Owner"
}

```

 
Cambiar solo las propiedades de título (Esto es solo un ejemplo, puesto que el esquema de este campo es hasta el llamador):
 

```cpp
{
  "titleData": "{ 'Id': '123456', 'Location': 'C:\\videos\\123456.mp4' }"
}

```

  
<a id="ID4EVF"></a>

 
## <a name="required-response-headers"></a>Encabezados de respuesta necesaria
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| string| Nombre o número del servicio de Xbox LIVE a la que se debe dirigir esta solicitud de compilación. La solicitud se enrutarán solo a ese servicio después de comprobar la validez de la cabecera, las notificaciones en el token de autenticación, etcetera. Ejemplos: 1 vnext.| 
| Content-Type| string| Tipo MIME del cuerpo de respuesta. Ejemplo: <b>application/json</b>.| 
| Cache-Control| string| Solicitud normal para especificar el comportamiento de almacenamiento en caché.| 
| Aceptar| string| Valores aceptables de Content-Type. Ejemplo: <b>application/json</b>.| 
| Retry-After| string| Indica el cliente que vuelva a intentarlo en el caso de un servidor no disponible.| 
| Variar| string| Indica cómo almacenar en caché las respuestas de proxies de nivel inferiores.| 
  
<a id="ID4EJAAC"></a>

 
## <a name="optional-response-headers"></a>Encabezados de respuesta opcional
 
| Encabezado| Tipo| Descripción| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| ETag| string| Se usa para la optimización de memoria caché. Por ejemplo: "686897696a7c876b7e".| 
  
<a id="ID4EJBAC"></a>

 
## <a name="response-body"></a>Cuerpo de la respuesta
 
Tras la actualización correcta de los metadatos de un código de estado HTTP 200, se devolverá.
 
En caso contrario, se devolverá un objeto ServiceErrorResponse en formato JSON con un código de estado HTTP adecuado.
  
<a id="ID4EWBAC"></a>

 
## <a name="related-uris"></a>URI relacionados
 
Los siguientes URI actualizar campos públicos en los metadatos. No hay ningún cuerpo necesario para estas solicitudes. Tras la actualización correcta de los metadatos de un código de estado HTTP 200, se devolverá. En caso contrario, se devolverá un objeto ServiceErrorResponse en formato JSON con un código de estado HTTP adecuado.
 
   * **POST/Users / {ownerId} /scids/ {¿scid} /clips/ {gameClipId} /ratings/ {valor de clasificación}** -se aplica la clasificación especificada en el clip especificado. Valor de clasificación debe ser un entero entre 1 y 5.
   * **REGISTRAR/Users / {ownerId} /scids/ {¿scid} /clips/ {gameClipId} / marca** -marca el clip para contener contenido potencialmente dudosa que se comprobará mediante la aplicación.
   * **POST/Users / {ownerId} /scids/ {¿scid} /clips/ {gameClipId} / vistas** -incrementa el recuento de vista para el clip de juego especificado. Se recomienda que esto se denomina no adecuado cuando se inicia la reproducción, pero cuando se haya completado el 75% - 80% de reproducción.
   
<a id="ID4EMCAC"></a>

 
## <a name="see-also"></a>Consulte también
 
<a id="ID4EOCAC"></a>

 
##### <a name="parent"></a>Primario 

[/users/me/scids/{scid}/clips/{gameClipId}](uri-usersmescidclipsgameclipid.md)

   